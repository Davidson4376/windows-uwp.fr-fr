---
description: L’environnement Windows Runtime est un système avec décompte des références ; dans un tel système, il est important de connaître la signification des références fortes et faibles, et de faire la distinction entre elles.
title: Références faibles en C++/WinRT
ms.date: 05/16/2019
ms.topic: article
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, fort, faible, référence
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 46a0e21295ba430671be4e36ab213e182c2b1737
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721632"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Les références fortes et faibles en C / c++ / WinRT

Le Runtime de Windows est un système contenant des références ; et dans un tel système, il est important de savoir sur la signification d’et la distinction entre, références fortes et faibles (et les références qui ne sont ni, telles que le caractère implicite *cela* pointeur). Comme vous le verrez dans cette rubrique, savoir comment gérer ces références correctement peut signifier une différence entre un système fiable qui s’exécute correctement et celui qui se bloque de façon imprévisible. En fournissant des fonctions d’assistance qui prend en charge approfondie dans la projection de langage, [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) répondent à vos à mi-chemin dans votre travail de création de systèmes plus complexes simplement et correctement.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>En toute sécurité l’accès à la *cela* pointeur dans une coroutine de membre de classe

Le code ci-dessous montre un exemple typique d’une coroutine qui est une fonction membre d’une classe. Vous pouvez copier-coller cet exemple dans les fichiers spécifiés dans une nouvelle **Application de Console Windows (C++/WinRT)** projet.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

struct MyClass : winrt::implements<MyClass, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    IAsyncOperation<winrt::hstring> RetrieveValueAsync()
    {
        co_await 5s;
        co_return m_value;
    }
};

int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };

    winrt::hstring result{ async.get() };
    std::wcout << result.c_str() << std::endl;
}
```

**MyClass::RetrieveValueAsync** passe à certains travaillent, et finalement, il retourne une copie de la `MyClass::m_value` membre de données. Appel **RetrieveValueAsync** provoque un objet asynchrone doit être créé, et que cet objet a implicite *cela* pointeur (par le biais duquel, finalement, `m_value` est accessible).

Voici la séquence complète des événements.

1. Dans **principal**, une instance de **MyClass** est créé (`myclass_instance`).
2. Le `async` objet est créé, pointant vers (via son *cela*) à `myclass_instance`.
3. Le **winrt::Windows::Foundation::IAsyncAction::get** fonction bloque pendant quelques secondes, puis retourne le résultat de **RetrieveValueAsync**.
4. **RetrieveValueAsync** retourne la valeur de `this->m_value`.

Étape 4 est sécurisé tant que *cela* est valide.

Mais, que se passe-t-il si l’instance de classe est détruite avant la fin de l’opération asynchrone ? Il existe toutes sortes de méthodes de que l’instance de classe pourrait sont hors de portée avant la fin de la méthode asynchrone. Mais nous pouvons la simuler en affectant à l’instance de classe `nullptr`.

```cppwinrt
int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };
    myclass_instance = nullptr; // Simulate the class instance going out of scope.

    winrt::hstring result{ async.get() }; // Behavior is now undefined; crashing is likely.
    std::wcout << result.c_str() << std::endl;
}
```

Après le point où nous détruire l’instance de classe, apparemment, nous faisons ne directement référence à celui-ci. Mais bien entendu l’objet asynchrone a un *cela* pointeur, et tente d’utiliser pour copier la valeur stockée à l’intérieur de l’instance de classe. La coroutine est une fonction membre, et il est censé être en mesure d’utiliser ses *cela* pointeur impunité.

Avec cette modification du code, nous rencontrons un problème à l’étape 4, étant donné que l’instance de classe a été détruite, et *cela* n’est plus valide. Dès que l’objet asynchrone tente d’accéder à la variable à l’intérieur de l’instance de classe, il sera se bloquer (ou créez quelque chose entièrement non défini).

La solution consiste à donner à l’opération asynchrone&mdash;la coroutine&mdash;sa propre référence forte à l’instance de classe. Tel qu’il est écrit, la coroutine conserve en fait un brutes *cela* pointeur vers l’instance de classe ; mais qui n’est pas suffisant pour maintenir l’instance de classe.

Pour maintenir l’instance de classe, modifier l’implémentation de **RetrieveValueAsync** à celle illustrée ci-dessous.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Un C++/WinRT classe dérive directement ou indirectement le [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) modèle. Pour cette raison, le C++/objet de WinRT peut appeler son [ **implements.get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) protégé par la fonction membre pour récupérer une référence forte à son *cela* pointeur. Notez qu’il est inutile d’utiliser réellement le `strong_this` variable dans l’exemple de code ci-dessus ; il vous suffit d’appeler **get_strong** incrémente le C++/WinRT objet du décompte de références et conserve son implicite *cette* pointeur valide.

> [!IMPORTANT]
> Étant donné que **get_strong** est une fonction membre de la **winrt::implements** modèle struct, vous pouvez l’appeler uniquement à partir d’une classe qui dérive directement ou indirectement **winrt::implements**, comme un C++/WinRT classe. Pour plus d’informations sur la dérivation à partir de **winrt::implements**, et obtenir des exemples, consultez [API auteur avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

Cela résout le problème que nous avions précédemment quand nous sommes à l’étape 4. Même si toutes les autres références à l’instance de classe disparaissent, la coroutine a pris la précaution de garantir que ses dépendances sont stables.

Si une référence forte n’est pas appropriée, vous pouvez appeler à la place [ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) pour récupérer une référence faible à *cela*. Il suffit de confirmer que vous pouvez récupérer une référence forte avant d’accéder à *cela*. Là encore, **get_weak** est une fonction membre de la **winrt::implements** modèle de struct.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto weak_this{ get_weak() }; // Maybe keep *this* alive.

    co_await 5s;

    if (auto strong_this{ weak_this.get() })
    {
        co_return m_value;
    }
    else
    {
        co_return L"";
    }
}
```

Dans l’exemple ci-dessus, la référence faible ne l’empêche pas l’instance de la classe d’être détruits lorsqu’il ne reste aucune référence forte. Mais il vous donne un moyen de vérifier si une référence forte peut être acquis avant d’accéder à la variable membre.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>En toute sécurité l’accès à la *cela* pointeur avec un délégué de gestion des événements

### <a name="the-scenario"></a>Le scénario

Pour obtenir des informations générales sur la gestion des événements, consultez [gérer les événements à l’aide de délégués en C / c++ / WinRT](handle-events.md).

La section précédente mis en évidence les problèmes potentiels de la durée de vie dans les domaines de coroutines et d’accès concurrentiel. Mais, si vous gérez un événement avec la fonction de membre d’un objet, ou depuis une fonction lambda à l’intérieur de la fonction de membre d’un objet, vous devez penser à la durée de vie relative au destinataire d’événement (l’objet gérant l’événement) et la source d’événement (l’objet déclenche l’événement). Examinons quelques exemples de code.

La liste de codes ci-dessous premier définit une simple **EventSource** (classe), ce qui déclenche un événement générique qui est géré par des délégués qui ont été ajoutés à ce dernier. Cet exemple d’événement se produit à utiliser le [ **Windows::Foundation::EventHandler** ](/uwp/api/windows.foundation.eventhandler) type délégué, mais que les problèmes et solutions ici s’appliquent aux types délégués tout ou partie.

Ensuite, le **EventRecipient** classe fournit un gestionnaire pour le **EventSource::Event** événement sous la forme d’une fonction lambda.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;

struct EventSource
{
    winrt::event<EventHandler<int>> m_event;

    void Event(EventHandler<int> const& handler)
    {
        m_event.add(handler);
    }

    void RaiseEvent()
    {
        m_event(nullptr, 0);
    }
};

struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event([&](auto&& ...)
        {
            std::wcout << m_value.c_str() << std::endl;
        });
    }
};

int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_source.RaiseEvent();
}
```

Le modèle est que le destinataire de l’événement a un gestionnaire d’événements lambda avec des dépendances son *cela* pointeur. Chaque fois que le destinataire de l’événement est supérieure à celle de la source d’événements, elle est supérieure à celle ces dépendances. Et, dans ce cas, qui sont commun, le modèle fonctionne bien. Certains de ces cas sont évidents, par exemple, lorsqu’une page d’interface utilisateur gère un événement déclenché par un contrôle qui se trouve sur la page. La page est supérieure à celle du bouton&mdash;par conséquent, le gestionnaire également est supérieure à celle du bouton. Cela est vrai chaque fois que le destinataire est le propriétaire de la source (comme membre de données, par exemple), ou chaque fois que le destinataire et la source sont frère/sœur et appartiennent directement à un autre objet. Si vous êtes certain que vous avez un cas où le gestionnaire ne survivra pas au *this* dont il dépend, vous pouvez capturer *this* normalement, sans considération pour la durée de vie forte ou faible.

Mais il existe toujours des cas où *cela* ne survivent pas son utilisation dans un gestionnaire (y compris les gestionnaires pour les événements de saisie semi-automatique et la progression déclenchés par des actions et opérations asynchrones), et il est important de savoir comment les traiter.

- Si vous créez une coroutine pour implémenter une méthode asynchrone, alors cela est possible.
- Dans de rares cas avec certains objets d’infrastructure de l’interface utilisateur XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), par exemple), cela est possible, si le destinataire est finalisé sans désinscription de la source d’événement.

### <a name="the-issue"></a>Le problème

Cette prochaine version de la **principal** fonction simule ce qui se passe lorsque le destinataire d’événement est détruit (peut-être qu’il est hors de portée) tandis que la source d’événements est toujours déclencher des événements.

```cppwinrt
int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_recipient = nullptr; // Simulate the event recipient going out of scope.
    event_source.RaiseEvent(); // Behavior is now undefined within the lambda event handler; crashing is likely.
}
```

Le destinataire de l’événement est détruit, mais le Gestionnaire d’événements lambda qu’il contient est encore abonné à la **événement** événement. Lorsque cet événement est déclenché, l’expression lambda tente de déréférencer les *cela* pointeur, qui est à ce stade non valide. Par conséquent, une violation d’accès provient du code dans le gestionnaire (ou dans les continuation d’une coroutine) de l’utiliser.

> [!IMPORTANT]
> Si vous rencontrez une telle situation, vous devez réfléchir à la durée de vie de la *cela* objet ; et ou non capturées *cela* objet est supérieure à celle de la capture. Si ce n’est pas le cas, puis capturer avec un nom fort ou une référence faible, comme nous allons montrer ci-dessous.
>
> Ou&mdash;si cela est pertinent pour votre scénario et si le thread considérations permettent même&mdash;une autre option consiste à révoquer le gestionnaire une fois que le destinataire est effectué avec l’événement, ou dans le destructeur du destinataire. Consultez [révoquer un délégué enregistré](handle-events.md#revoke-a-registered-delegate).

Voici comment nous allons l’inscription du gestionnaire.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

L’expression lambda capture automatiquement toutes les variables locales par référence. Par conséquent, pour cet exemple, nous pourrions aurait écrire cela.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Dans les deux cas, nous allons simplement capturer brut *cela* pointeur. Et qui n’a aucun effet sur le comptage de référence, donc rien n’empêche l’objet actuel en cours de destruction.

### <a name="the-solution"></a>La solution

La solution consiste à capturer une référence forte. Une référence forte *est* incrémenter le décompte de références et il *est* maintenir l’objet actuel. Vous déclarez une variable de capture (appelé `strong_this` dans cet exemple) et l’initialiser avec un appel à [ **implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function), qui Récupère une référence forte à notre  *Cela* pointeur.

> [!IMPORTANT]
> Étant donné que **get_strong** est une fonction membre de la **winrt::implements** modèle struct, vous pouvez l’appeler uniquement à partir d’une classe qui dérive directement ou indirectement **winrt::implements**, comme un C++/WinRT classe. Pour plus d’informations sur la dérivation à partir de **winrt::implements**, et obtenir des exemples, consultez [API auteur avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Vous pouvez même omettre la capture automatique de l’objet actuel et accéder au membre de données via la variable de capture au lieu de via l’implicite *cela*.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Si une référence forte n’est pas appropriée, vous pouvez appeler à la place [ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) pour récupérer une référence faible à *cela*. Il suffit de confirmer que vous pouvez toujours récupérer une référence forte à partir de celui-ci avant d’accéder à des membres.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>Si vous utilisez une fonction membre en tant que délégué

Ainsi que les fonctions lambda, ces principes s’appliquent également à l’utilisation d’une fonction membre comme votre délégué. La syntaxe est différente, nous allons donc étudier à du code. Tout d’abord, voici le Gestionnaire d’événements fonction membre potentiellement dangereuses, à l’aide d’un texte brut *cela* pointeur.

```cppwinrt
struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event({ this, &EventRecipient::OnEvent });
    }

    void OnEvent(IInspectable const& /* sender */, int /* args */)
    {
        std::wcout << m_value.c_str() << std::endl;
    }
};
```

Il s’agit de la façon standard, conventionnelle pour faire référence à un objet et sa fonction membre. Pour en faire sans échec, vous pouvez&mdash;depuis la version 10.0.17763.0 (Windows 10, version 1809) du SDK Windows&mdash;établir un nom fort ou une référence faible au point où le gestionnaire est enregistré. À ce stade, l’objet de destinataire d’événement est connu pour être encore active.

Pour une référence forte, appelez simplement [ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) à la place de brut *cela* pointeur. C++ / c++ / WinRT garantit que le délégué résultant conserve une référence forte à l’objet actuel.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

Pour une référence faible, appelez [ **get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function). C++ / c++ / WinRT garantit que le délégué résultant conserve une référence faible. À la dernière minute, dans les coulisses, le délégué tente de résoudre la référence faible à une forte et appelle uniquement la fonction membre, si elle réussit.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Un exemple de référence faible à l’aide de **SwapChainPanel::CompositionScaleChanged**

Dans cet exemple de code, nous utilisons le [ **SwapChainPanel::CompositionScaleChanged** ](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) événement par le biais d’une autre illustration de références faibles. Le code inscrit un gestionnaire d’événements à l’aide d’une expression lambda qui capture une référence faible au destinataire.

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weak_this{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strong_this{ weak_this.get() })
        {
            strong_this->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

Dans la clause de capture lamba, une variable temporaire est créée, qui représente une référence faible pour *this*. Dans le corps de l’expression lambda, si une référence forte à *this* peut être obtenue, alors la fonction **OnCompositionScaleChanged** est appelée. De cette façon, à l’intérieur de **OnCompositionScaleChanged**, *this* peut être utilisé en toute sécurité.

## <a name="weak-references-in-cwinrt"></a>Références faibles en C++/WinRT

Ci-dessus, nous avons vu des références faibles utilisés. En général, elles sont parfaites pour rompre des références cycliques. Par exemple, pour l’implémentation de l’infrastructure d’interface utilisateur XAML native&mdash;en raison de la conception d’historique de l’infrastructure&mdash;le faible référence mécanisme dans C++ / c++ / WinRT est nécessaire de gérer les références cycliques. En dehors de XAML, cependant, vous ne devrez probablement utiliser des références faibles (qu’il y a rien n’est pas intrinsèquement XAML spécifiques à leur sujet). Au lieu de cela, plus que rarement, soyez en mesure de concevoir votre propre C + c++ / WinRT APIs de manière à éviter la nécessité de références cycliques et références faibles. 

Pour n’importe quel type donné que vous déclarez, il n’est pas immédiatement évident pour C++/WinRT de déterminer si des références faibles sont nécessaires. Par conséquent, C++/WinRT fournit la prise en charge des références faibles automatiquement sur le modèle de structure [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), à partir duquel vos propres types C++/WinRT dérivent directement ou indirectement. Il s’agit d’une prise en charge « payer pour lire », dans la mesure où elle ne vous coûte rien, sauf si votre objet est réellement interrogé pour [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource). Et vous pouvez choisir explicitement de [refuser cette prise en charge](#opting-out-of-weak-reference-support).

### <a name="code-examples"></a>Exemples de code
Le modèle de structure [**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) est une option permettant d’obtenir une référence faible à une instance de classe.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

Ou, vous pouvez utiliser la fonction d’assistance [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak).

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

La création d’une référence faible n’affecte pas le nombre de références sur l’objet lui-même ; elle entraîne simplement l’allocation d’un bloc de contrôle. Ce bloc de contrôle s’occupe de l’implémentation de la sémantique des références faibles. Vous pouvez essayer de promouvoir la référence faible pour une référence forte et, en cas de réussite, l’utiliser.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

À condition qu’une autre référence forte existe toujours, l’appel [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) incrémente le nombre de références et retourne la référence forte à l’appelant.

### <a name="opting-out-of-weak-reference-support"></a>Refus de la prise en charge des références faibles
La prise en charge des références faibles est automatique. Mais vous pouvez choisir explicitement de refuser cette prise en charge en transmettant la structure de marqueur [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) en tant qu’argument de modèle à votre classe de base.

Si vous dérivez directement de **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

Si vous créez une classe runtime.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

Peu importe où la structure de marqueur apparaît dans le pack de paramètres variadiques. Si vous demandez une référence faible pour un type refusé, le compilateur vous aidera en affichant une erreur « *This is only for weak ref support* » (ceci est réservé à la prise en charge des références faibles).

## <a name="important-apis"></a>API importantes
* [Implements::get_weak (fonction)](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [modèle de fonction WinRT::make_weak](/uwp/cpp-ref-for-winrt/make-weak)
* [WinRT::no_weak_ref marqueur struct](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [modèle de struct WinRT::weak_ref](/uwp/cpp-ref-for-winrt/weak-ref)
