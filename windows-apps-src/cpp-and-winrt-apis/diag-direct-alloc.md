---
description: Cette rubrique décrit de façon détaillée la fonctionnalité C++/WinRT 2.0 qui vous permet de diagnostiquer l’erreur de créer un objet de type implémentation sur la pile, plutôt que d’utiliser la famille d’assistants [**winrt::make**](/uwp/cpp-ref-for-winrt/make), comme vous le devriez.
title: Diagnostic des allocations directes
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, direct, pile, allocations, projeté, implémentation
ms.localizationpriority: medium
ms.openlocfilehash: 7fe8ff6653b8655ee25cd9adc0c11acb22d42a11
ms.sourcegitcommit: 4e74c920f1fef507c5cdf874975003702d37bcbb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372790"
---
# <a name="diagnosing-direct-allocations"></a>Diagnostic des allocations directes

Comme expliqué dans [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis), lorsque vous créez un objet de type d’implémentation, il vous est conseillé d'utiliser la famille d'assistants [**winrt::make**](/uwp/cpp-ref-for-winrt/make). Cette rubrique décrit de façon détaillée la fonctionnalité C++/WinRT 2.0 qui vous permet de diagnostiquer l’erreur consistant à allouer directement un objet de type implémentation sur la pile.

De telles erreurs peuvent entraîner de mystérieux blocages, de même que des corruptions fastidieuses et chronophages en termes de débogage. Il s’agit donc d’une fonctionnalité importante dont il convient de comprendre le contexte.

## <a name="setting-the-scene-with-mystringable"></a>Poser les bases, avec **MyStringable**

Penchons-nous, dans un premier temps, sur une implémentation simple de [**IStringable**](/uwp/api/windows.foundation.istringable).

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const { return L"MyStringable"; }
};
```

Imaginez maintenant que vous devez appeler une fonction (à partir de votre implémentation) qui attend **IStringable** en tant qu'argument.

```cppwinrt
void Print(IStringable const& stringable)
{
    printf("%ls\n", stringable.ToString().c_str());
}
```

Mais notre type **MyStringable** n'est *pas* **IStringable**.

- Notre type **MyStringable** est une implémentation de l'interface **IStringable**.
- Le type **IStringable** est un type projeté.

> [!IMPORTANT]
> Il est important de comprendre la différence entre un *type d'implémentation* et un *type projeté*. Pour les concepts et termes essentiels, consultez [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

L'espace entre implémentation et projection peut être subtile. En fait, pour que l’implémentation ressemble un peu plus à la projection, l’implémentation fournit des conversions implicites à chaque type projeté qu’elle implémente. Cela ne signifie pas pour autant que nous pouvons nous en contenter.

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const;
 
    void Call()
    {
        Print(this);
    }
};
```

Il nous faut plutôt obtenir une référence pour pouvoir utiliser les opérateurs de conversion en tant que candidats à la résolution de l'appel.

```cppwinrt
void Call()
{
    Print(*this);
}
```

Et cela fonctionne. Une conversion implicite convertit (de manière très efficace) le type d'implémentation en type projeté, ce qui est très pratique pour de nombreux scénarios. Sans cette fonctionnalité, de nombreux types d’implémentation seraient très lourds à créer. À condition d'utiliser uniquement le modèle de fonction [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (ou [**winrt ::make_self**](/uwp/cpp-ref-for-winrt/make-self)) pour allouer l'implémentation, tout fonctionne parfaitement.

```cppwinrt
IStringable stringable{ winrt::make<MyStringable>() };
```

## <a name="potential-pitfalls-with-cwinrt-10"></a>Problèmes potentiels liés à C++/WinRT 1.0

Cela étant, les conversions implicites peuvent être source de problèmes. Prenez en compte fonction d’assistance inutile.

```cppwinrt
IStringable MakeStringable()
{
    return MyStringable(); // Incorrect.
}
```

Ou même cette déclaration apparemment anodine.

```cppwinrt
IStringable stringable{ MyStringable() }; // Also incorrect.
```

Malheureusement, un code comme celui-ci *a* été compilé avec C++/WinRT 1.0 en raison de cette conversion implicite. Le problème (très sérieux) est que nous renvoyons potentiellement un type projeté qui pointe vers un objet avec décompte des références dont la mémoire de sauvegarde se trouve sur la pile éphémère.

Voici un autre élément compilé avec C++/WinRT 1.0.

```cppwinrt
MyStringable* stringable{ new MyStringable() }; // Very inadvisable.
```

Les pointeurs bruts constituent une source de bogues aussi dangereux que chronophages. Ne les utilisez pas si vous n’en avez pas besoin. C++/WinRT ne permet pas de tout faire efficacement sans vous contraindre à utiliser des pointeurs bruts. Voici un autre élément compilé avec C++/WinRT 1.0.

```cppwinrt
auto stringable{ std::make_shared<MyStringable>(); } // Also very inadvisable.
```

Il s’agit d’une erreur à plusieurs niveaux. Le même objet présente deux décomptes de références différents. Windows Runtime (et COM classique avant lui) est basé sur un décompte de références intrinsèque qui n’est pas compatible avec **std::shared_ptr**. **std::shared_ptr** présente, bien sûr, de nombreuses applications valides, mais s'avère inutile lorsque vous partagez des objets Windows Runtime (et COM classiques). Enfin, il est également compilé avec C++/WinRT 1.0.

```cppwinrt
auto stringable{ std::make_unique<MyStringable>() }; // Highly dubious.
```

Un point qui est là encore contestable. La propriété unique est en opposition avec la durée de vie partagée du décompte de références intrinsèque de **MyStringable**.

## <a name="the-solution-with-cwinrt-20"></a>Solution avec C++/WinRT 2.0

Avec C++/WinRT 2.0, toutes les tentatives visant à allouer directement des types d’implémentation génèrent une erreur du compilateur. Il s'agit là du meilleur type d'erreur et d'une erreur nettement préférable à un mystérieux bogue de runtime.

Chaque fois qu'il vous faut effectuer une implémentation, vous pouvez tout simplement utiliser [**winrt::make**](/uwp/cpp-ref-for-winrt/make) ou [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self), comme indiqué ci-dessus. Et si vous oubliez de le faire, une erreur du compilateur faisant référence à une fonction abstraite nommée **use_make_function_to_create_this_object** vous le rappellera. Il ne s'agit pas exactement d'un `static_assert`, mais cela s'en approche. Et cela reste la méthode la plus fiable pour détecter toutes les erreurs décrites.

Ainsi, il nous faut placer quelques contraintes mineures sur l’implémentation. Étant donné que nous nous appuyons sur l’absence de remplacement pour détecter une allocation directe, le modèle de fonction **winrt::make** doit être satisfaire à la fonction virtuelle abstraite avec remplacement. Pour ce faire, il dérive de l'implémentation avec une classe `final` qui fournit le remplacement. Plusieurs éléments de ce processus sont prendre en compte.

Premièrement, la fonction virtuelle est présente uniquement dans les versions de débogage. Dès lors, la détection n’affecte pas la taille de la vtable dans vos builds optimisées.

Deuxièmement, la classe dérivée utilisée par **winrt::make** étant `final`, toute dévirtualisation que l’optimiseur peut déduire se produira même si vous avez précédemment choisi de ne pas marquer votre classe d’implémentation comme `final`. Il s'agit là d'une amélioration. Pour autant, votre implémentation ne peut *pas* être `final`. Là encore, cela est sans conséquence car le type instancié sera toujours `final`.

Troisièmement, rien ne vous empêche de marquer des fonctions virtuelles de votre implémentation comme `final`. Bien sûr, C++/WinRT est très différent de COM classique et des implémentations de type WRL, où tout ce qui a trait à votre implémentation tend à être virtuel. Dans C++/WinRT, la distribution virtuelle est limitée à l’interface binaire d’application (ABI) (qui est toujours `final`), et vos méthodes d’implémentation reposent sur le polymorphisme au moment de la compilation ou statique. Cela permet d'éviter un polymorphisme inutile du runtime et justifie la présence de fonctions virtuelles dans votre implémentation C++/WinRT, ce qui constitue un point très positif et permet une incorporation nettement plus prévisible.

Quatrièmement, **winrt::make** injectant une classe dérivée, votre implémentation ne peut avoir de destructeur privé. Les destructeurs privés étaient courants avec les implémentations COM classiques car là encore, tout était virtuel, et il était fréquent de recourir à des pointeurs bruts et donc d'appeler accidentellement `delete` au lieu de [**Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release). C++/WinRT ne facilite en rien l'utilisation de pointeurs bruts. Et il vous faut *vraiment* le vouloir pour obtenir un pointeur brut dans C++/WinRT et appeler `delete`. Les sémantiques de valeur impliquent que vous utilisiez des valeurs et des références, et rarement des pointeurs.

Dès lors, C++/WinRT met à mal nos idées préconçues sur ce qu'implique l'écriture de code COM classique. Et c'est parfaitement raisonnable, car WinRT n’est pas COM classique. COM classique est le langage assembleur de Windows Runtime. Il ne doit pas être le code que vous écrivez quotidiennement. C++/WinRT, quant à lui, vous permet d'écrire du code qui s'apparente plus à C++ qu'à COM classique.

## <a name="important-apis"></a>API importantes
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)
* [Modèle de fonction winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>Rubriques connexes
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)