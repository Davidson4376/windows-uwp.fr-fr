---
description: L’environnement Windows Runtime est un système avec décompte des références ; dans un tel système, il est important de connaître la signification des références fortes et faibles, et de faire la distinction entre elles.
title: Références faibles en C++/WinRT
ms.date: 05/16/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, forte, faible, référence
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 46a0e21295ba430671be4e36ab213e182c2b1737
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66721632"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Références fortes et faibles en C++/WinRT

L’environnement Windows Runtime est un système avec décompte des références. Dans un tel système, il est important de connaître la signification des références fortes et faibles (ainsi que des références qui ne sont ni fortes ni faibles, comme le pointeur implicite *this*), et de faire la distinction entre elles. Comme vous le verrez dans cette rubrique, le fait de savoir gérer ces références correctement est indispensable pour bénéficier d’un système fiable qui s’exécute correctement et éviter les plantages imprévisibles. En fournissant des fonctions d’assistance qui prennent entièrement en charge la projection de langage, [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) fait la moitié du travail nécessaire à la création de systèmes plus complexes, simplement et correctement.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Accès sécurisé au pointeur *this* dans une coroutine de membre de classe

Le listing de code ci-dessous montre un exemple typique d’une coroutine, qui est une fonction membre d’une classe. Vous pouvez copier-coller cet exemple dans les fichiers spécifiés, dans un nouveau projet **Windows Console Application (C++/WinRT)**

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

**MyClass::RetrieveValueAsync** s’exécute pendant un certain temps, puis finit par retourner une copie du membre de données `MyClass::m_value`. L’appel de **RetrieveValueAsync** entraîne la création d’un objet asynchrone, et cet objet a un pointeur implicite *this* (par le biais duquel `m_value` devient accessible).

Voici la séquence complète des événements.

1. Dans **main**, une instance de **MyClass** est créée (`myclass_instance`).
2. L’objet `async` est créé, et pointe vers `myclass_instance` (via son pointeur *this*).
3. La fonction **winrt::Windows::Foundation::IAsyncAction::get** se bloque pendant quelques secondes, puis retourne le résultat de **RetrieveValueAsync**.
4. **RetrieveValueAsync** retourne la valeur de `this->m_value`.

L’étape 4 est sécurisée tant que le pointeur *this* est valide.

Mais, que se passe-t-il si l’instance de classe est détruite avant la fin de l’opération asynchrone ? L’instance de classe peut devenir hors de portée avant la fin de la méthode asynchrone pour de nombreuses raisons. Toutefois, nous pouvons le simuler en définissant l’instance de classe sur `nullptr`.

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

Après la destruction de l’instance de classe, il semble que nous ne le référençons plus directement. Bien entendu, l’objet asynchrone est pointé par un pointeur *this*, et tente de l’utiliser pour copier la valeur stockée dans l’instance de classe. La coroutine est une fonction membre, et s’attend naturellement à pouvoir utiliser son pointeur *this*.

Après cette modification du code, nous rencontrons un problème à l’étape 4, car l’instance de classe a été détruite, et que le pointeur *this* n’est plus valide. Dès que l’objet asynchrone tente d’accéder à la variable à l’intérieur de l’instance de classe, il plante (ou fait quelque chose de totalement inattendu).

La solution consiste à donner à l’opération asynchrone (la coroutine) sa propre référence forte à l’instance de classe. Telle qu’elle est écrite, la coroutine contient effectivement un pointeur *this* brut qui pointe vers l’instance de classe. Toutefois, cela ne suffit pas à préserver l’instance de classe.

Pour préserver l’instance de classe, remplacez l’implémentation de **RetrieveValueAsync** par celle illustrée ci-dessous.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Une classe C++/WinRT dérive directement ou indirectement d’un modèle [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements). Pour cette raison, l’objet C++/WinRT peut appeler sa fonction membre protégée [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) pour récupérer une référence forte à son pointeur *this*. Notez qu’il est inutile d’utiliser la variable `strong_this` dans l’exemple de code ci-dessus. Il vous suffit d’appeler **get_strong** pour incrémenter le nombre de références de l’objet C++/WinRT et pour que son pointeur implicite *this* reste valide.

> [!IMPORTANT]
> Étant donné que **get_strong** est une fonction membre du modèle struct **winrt::implements**, vous pouvez l’appeler uniquement à partir d’une classe qui dérive directement ou indirectement de **winrt::implements**, comme la classe C++/WinRT. Pour plus d’informations sur la dérivation à partir de **winrt::implements**, et pour obtenir des exemples, consultez [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

Cela résout le problème que nous avons eu à l’étape 4. Même si toutes les autres références à l’instance de classe disparaissent, la coroutine a pris la précaution de garantir la stabilité de ses dépendances.

Si une référence forte ne convient pas, vous pouvez appeler à la place [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) pour récupérer une référence faible au pointeur *this*. Vérifiez simplement que vous pouvez récupérer une référence forte avant d’accéder au pointeur *this*. Là encore, **get_weak** est une fonction membre du modèle de struct **winrt::implements**.

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

Dans l’exemple ci-dessus, la référence faible n’empêche pas la destruction de l’instance de classe lorsqu’il ne reste plus aucune référence forte. Toutefois, elle vous permet de vérifier si une référence forte peut être acquise avant d’accéder à la variable membre.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Accès sécurisé au pointeur *this* avec un délégué de gestion des événements.

### <a name="the-scenario"></a>Scénario

Pour obtenir des informations générales sur la gestion des événements, consultez [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md).

La section précédente a abordé des problèmes potentiels de durée de vie au niveau des coroutines et de l’accès concurrentiel. Toutefois, si vous gérez un événement avec la fonction membre d’un objet, ou à partir d’une fonction lambda au sein de la fonction membre d’un objet, vous devez penser aux durées de vie relatives du destinataire d’événement (l’objet qui gère l’événement) et de la source d’événement (l’objet qui déclenche l’événement). Voyons quelques exemples de code.

Le listing de code ci-dessous définit d’abord une simple classe **EventSource**, ce qui déclenche un événement générique qui est géré par des délégués qui lui ont été ajoutés. Cet exemple d’événement se produit pour utiliser le type délégué [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler). Cependant, les problèmes et les solutions mentionnés ici s’appliquent à tous les types délégués.

Ensuite, la classe **EventRecipient** fournit un gestionnaire pour l’événement **EventSource::Event** sous la forme d’une fonction lambda.

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

Dans ce modèle, le destinataire de l’événement a un gestionnaire d’événements lambda avec des dépendances à son pointeur *this*. Chaque fois que la durée de vie du destinataire de l’événement dépasse celle de la source d’événements, elle dépasse également celle de ces dépendances. Et, dans ce cas (qui est courant), le modèle fonctionne bien. Certains de ces cas sont évidents, par exemple, lorsqu’une page d’interface utilisateur gère un événement déclenché par un contrôle qui se trouve dans la page. La durée de vie de la page dépasse celle du bouton, par conséquent, la durée de vie du gestionnaire dépasse également celle du bouton. Ceci est vrai chaque fois que le destinataire est le propriétaire de la source (comme membre de données, par exemple), ou chaque fois que le destinataire et la source sont frère/sœur et appartiennent directement à un autre objet. Si vous êtes certain que vous avez un cas où le gestionnaire ne survivra pas au *this* dont il dépend, vous pouvez capturer *this* normalement, sans considération pour la durée de vie forte ou faible.

Il existe cependant des cas où *this* ne survit pas à son utilisation dans un gestionnaire (y compris les gestionnaires d’événements de progression et d’achèvement déclenchés par des actions et opérations asynchrones), et il est important de savoir comment les gérer.

- Si vous créez une coroutine pour implémenter une méthode asynchrone, alors cela est possible.
- Dans de rares cas, avec certains objets du framework de l’interface utilisateur XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), par exemple), cela est possible, si le destinataire est finalisé sans désinscription de la source d’événement.

### <a name="the-issue"></a>Le problème

La version suivante de la fonction **main** simule ce qui se passe lorsque le destinataire d’événement est détruit (par exemple, s’il devient hors de portée) alors que la source d’événements continue de déclencher des événements.

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

Le destinataire de l’événement est détruit, mais le gestionnaire d’événements lambda qu’il contient est encore abonné à l’événement **Event**. Lorsque cet événement est déclenché, l’expression lambda tente de déréférencer le pointeur *this*, qui à ce stade, n’est plus valide. Par conséquent, une violation d’accès se produit après que le code du gestionnaire (ou de la continuation d’une coroutine) a tenté de l’utiliser.

> [!IMPORTANT]
> Si vous rencontrez une situation similaire, vous devrez réfléchir à la durée de vie de l’objet *this* et décider si l’objet *this* capturé doit survivre à la capture. Si ce n’est pas le cas, capturez-le avec une référence forte ou faible, comme nous allons l’expliquer ci-dessous.
>
> Ou, si cela a du sens pour votre scénario et si les considérations liées au thread le rendent même possible, alors une autre option consiste à révoquer le gestionnaire une fois que le destinataire en a terminé avec l’événement, ou dans le destructeur du destinataire. Consultez [Révoquer un délégué inscrit](handle-events.md#revoke-a-registered-delegate).

Voici comment nous allons inscrire le gestionnaire.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

L’expression lambda capture automatiquement toutes les variables locales par référence. Par conséquent, pour cet exemple, nous pourrions avoir écrit cela.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Dans les deux cas, nous allons simplement capturer le pointeur *this* brut. Cela n’a aucun effet sur le comptage de références, donc rien n’empêche l’objet actuel d’être détruit.

### <a name="the-solution"></a>La solution

La solution consiste à capturer une référence forte. Une référence forte *incrémente* le nombre de références et *préserve* l’objet actuel. Il vous suffit de déclarer une variable de capture (appelée `strong_this` dans cet exemple) et de l’initialiser avec un appel à [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function), qui récupère une référence forte à notre pointeur *this*.

> [!IMPORTANT]
> Étant donné que **get_strong** est une fonction membre du modèle struct **winrt::implements**, vous pouvez l’appeler uniquement à partir d’une classe qui dérive directement ou indirectement de **winrt::implements**, comme la classe C++/WinRT. Pour plus d’informations sur la dérivation à partir de **winrt::implements**, et pour obtenir des exemples, consultez [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Vous pouvez même omettre la capture automatique de l’objet actuel et accéder au membre de données via la variable de capture, au lieu de passer par le pointeur implicite *this*.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Si une référence forte ne convient pas, vous pouvez appeler à la place [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) pour récupérer une référence faible au pointeur *this*. Vérifiez simplement que vous pouvez toujours récupérer une référence forte avant d’accéder aux membres.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>Si vous utilisez une fonction membre comme délégué

Tout comme les fonctions lambda, ces principes s’appliquent à l’utilisation d’une fonction membre comme délégué. La syntaxe est toutefois différente, nous allons donc regarder à quoi ressemble le code. Tout d’abord, voici le gestionnaire d’événements des fonctions membres potentiellement dangereuses, qui utilise un pointeur *this* brut.

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

Il s’agit de la méthode conventionnelle pour référencer un objet et sa fonction membre. Pour rendre ce scénario sécurisé, vous pouvez (à compter de la version 10.0.17763.0 du SDK Windows, Windows 10, version 1809) créer une référence forte ou faible au point où le gestionnaire est inscrit. À ce stade, l’objet de destinataire d’événement est encore en vie.

Pour une référence forte, appelez [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) au lieu du pointeur *this* brut. C++/WinRT garantit que le délégué résultant conserve une référence forte à l’objet actuel.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

Pour une référence faible, appelez [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function). C++/WinRT garantit que le délégué résultant conserve une référence faible. À la dernière minute, et en arrière-plan, le délégué tente de résoudre la référence faible en une référence forte, et n’appelle la fonction membre qu’en cas de réussite.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Exemple de référence faible utilisant **SwapChainPanel::CompositionScaleChanged**

Dans cet exemple de code, nous utilisons l’événement [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) par le biais d’une autre illustration de références faibles. Le code inscrit un gestionnaire d’événements utilisant une expression lambda qui capture une référence faible au destinataire.

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

Dans la clause de capture lamba, une variable temporaire est créée et représente une référence faible à *this*. Dans le corps de l’expression lambda, si une référence forte à *this* peut être obtenue, alors la fonction **OnCompositionScaleChanged** est appelée. De cette façon, à l’intérieur de **OnCompositionScaleChanged**, *this* peut être utilisé de façon sécurisée.

## <a name="weak-references-in-cwinrt"></a>Références faibles en C++/WinRT

Nous avons vu plus haut comment utiliser des références faibles. En général, elles conviennent bien à la suppression des références cycliques. Par exemple, pour l’implémentation native du framework d’interface utilisateur basé sur XAML (en raison de la conception historique du framework), le mécanisme de références faibles en C++/WinRT est nécessaire pour gérer les références cycliques. En dehors du XAML, cependant, vous n’aurez probablement pas à utiliser des références faibles (même si celles-ci ne sont pas liées intrinsèquement au XAML). Vous devriez pouvoir, le plus souvent, concevoir vos propres API C++/WinRT de manière à éviter le besoin de références cycliques et de références faibles. 

Pour n’importe quel type donné que vous déclarez, il n’est pas immédiatement évident pour C++/WinRT de déterminer si des références faibles sont nécessaires. Par conséquent, C++/WinRT fournit la prise en charge des références faibles automatiquement sur le modèle de struct [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), à partir duquel vos propres types C++/WinRT dérivent directement ou indirectement. Il s’agit d’une prise en charge de type « pay-for-play », dans la mesure où elle ne vous coûte rien, sauf si votre objet est réellement interrogé pour [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource). Et vous pouvez choisir explicitement de [refuser cette prise en charge](#opting-out-of-weak-reference-support).

### <a name="code-examples"></a>Exemples de code
Le modèle de struct [**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) permet d’obtenir une référence faible à une instance de classe.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

Vous pouvez également utiliser la fonction d’assistance [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak).

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

La création d’une référence faible n’affecte pas le nombre de références de l’objet. Elle entraîne simplement l’allocation d’un bloc de contrôle. Ce bloc de contrôle s’occupe de l’implémentation de la sémantique des références faibles. Vous pouvez essayer de promouvoir la référence faible en une référence forte et, en cas de réussite, l’utiliser.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

À condition qu’une autre référence forte existe toujours, l’appel [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) incrémente le nombre de références et retourne la référence forte à l’appelant.

### <a name="opting-out-of-weak-reference-support"></a>Refus de la prise en charge des références faibles
La prise en charge des références faibles est automatique. Mais vous pouvez choisir explicitement de refuser cette prise en charge en passant le struct de marqueur [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) en tant qu’argument de modèle à votre classe de base.

Si vous dérivez directement de **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

Si vous créez une classe Runtime.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

Peu importe où le struct de marqueur apparaît dans le pack de paramètre variadique. Si vous demandez une référence faible pour un type refusé, le compilateur vous aidera en affichant une erreur « *This is only for weak ref support* » (Ceci est réservé à la prise en charge des références faibles).

## <a name="important-apis"></a>API importantes
* [implements::get_weak, fonction](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::make_weak, modèle de fonction](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref, struct de marqueur](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref, modèle de struct](/uwp/cpp-ref-for-winrt/weak-ref)
