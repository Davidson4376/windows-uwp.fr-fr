---
author: stevewhims
description: Windows Runtime est un système avec décompte des références; et dans un tel système, il est important pour vous de connaître la signification d’et la distinction entre les deux, références fortes et faibles.
title: Références faibles en C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, faible, référence forte
ms.localizationpriority: medium
ms.openlocfilehash: 414a73c8df31e4547b8bd154945a8e9960529320
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4384948"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Références fortes et faibles en C++ / WinRT

Windows Runtime est un système avec décompte des références; et, dans un tel système, il est important pour vous de connaître la signification d’et la distinction entre les deux, forts et faibles (et des références références qui ne sont pas, par exemple, le pointeur implicite *cette* ). Comme vous le verrez dans cette rubrique, savoir comment gérer ces références correctement peut signifier la différence entre un système fiable qui s’exécute de façon fluide et qui se bloque de manière imprévisible. En fournissant des fonctions d’assistance qui ont une prise en charge approfondie dans la projection de langage, [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) vous répond à mi-chemin dans votre travail de création de systèmes plus complexes simplement et correctement.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Accéder en toute sécurité au pointeur *this* dans une coroutine membre de classe

Le code ci-dessous montre un exemple typique d’une coroutine est une fonction membre d’une classe.

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

**MyClass::RetrieveValueAsync** ne fonctionne pas pendant un certain temps, et finalement il retourne une copie de la `MyClass::m_value` membre de données. L’appel de **RetrieveValueAsync** entraîne un objet asynchrone à créer et cet objet possède un implicite *ce* pointeur (par le biais duquel, en définitive, `m_value` est accessible).

Voici la séquence d’événements complète.

1. Dans la **principale**, une instance de **MyClass** est créée (`myclass_instance`).
2. Le `async` objet est créé, en pointant (via son *ce*) sur `myclass_instance`.
3. La fonction **winrt::Windows::Foundation::IAsyncAction::get** bloque pendant quelques secondes, puis renvoie le résultat de **RetrieveValueAsync**.
4. **RetrieveValueAsync** renvoie la valeur de `this->m_value`.

Étape 4 est sûre tant que *ce* n’est valide.

Toutefois, que se passe-t-il si l’instance de classe est détruite avant l’opération asynchrone se termine? Il existe toutes sortes de façons que l’instance de classe pourrait accéder hors de portée avant que la méthode asynchrone est terminé. Mais, nous pouvons simuler le son en affectant à l’instance de classe `nullptr`.

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

Après le point où nous détruire l’instance de classe, il se présente comme nous ne directement les désignons à nouveau. Toutefois, bien entendu l’objet asynchrone a un pointeur *this* qui lui et tente de qui permet de copier la valeur stockée à l’intérieur de l’instance de classe. La coroutine est une fonction membre, et il s’attend à être en mesure d’utiliser *ce* son pointeur mise.

Avec cette modification du code, nous avons rencontré un problème à l’étape 4, dans la mesure où l’instance de classe a été supprimée, et *ce* n’est plus valide. Dès que l’objet asynchrone tente d’accéder à la variable à l’intérieur de l’instance de classe, il se bloque (ou effectuer une action entièrement non défini).

La solution consiste à donner à l’opération asynchrone&mdash;la coroutine&mdash;sa propre référence forte à l’instance de classe. En tant qu’écrite actuellement, la coroutine conserve efficacement un brutes *ce* pointeur vers l’instance de classe; mais ce n’est pas suffisamment afin de maintenir l’instance de classe.

Pour garder l’instance de classe active, remplacez l’implémentation de **RetrieveValueAsync** à celui illustré ci-dessous.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Étant donné que C++ / objet WinRT dérive directement ou indirectement à partir du modèle [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) , C++ / WinRT objet peut appeler sa fonction de membre [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) protégé pour récupérer une référence forte à *ce* son pointeur. Notez qu’il n’est pas nécessaire pour utiliser réellement les `strong_this` variable; l’appel simplement **get_strong** incrémente le décompte de références et maintient votre implicite *ce* pointeur valide.

Cela résout le problème que nous avions précédemment lorsque nous avons obtenu à l’étape 4. Même si toutes les autres références à l’instance de classe disparaissent, la coroutine a effectué la précaution de garantir que ses dépendances sont stables.

Si une référence forte n’est pas appropriée, vous pouvez appeler [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) pour récupérer une référence faible à *cette*à la place. Confirmez que vous pouvez récupérer une référence forte avant d’accéder à *cette*.

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

Dans l’exemple ci-dessus, la référence faible n’a pas conserver l’instance de classe d’être détruits lorsqu’il ne restent pas de référence forte. Mais elle vous donne un moyen de vérifier si une référence forte peut être obtenue avant d’accéder à la variable de membre.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>En toute sécurité l’accès à *ce* pointeur avec un délégué de gestion des événements

### <a name="the-scenario"></a>Le scénario

Pour plus d’informations générales sur la gestion des événements, consultez [gérer des événements en utilisant des délégués en C++ / WinRT](handle-events.md).

La section précédente mis en surbrillance les problèmes potentiels de la durée de vie dans les zones de coroutines et d’accès concurrentiel. Mais, si vous gérez un événement avec la fonction de membre d’un objet, ou à partir d’une fonction lambda à l’intérieur de la fonction de membre d’un objet, puis vous devez penser à la durée de vie relatives du destinataire d’événement (l’objet qui gère l’événement) et la source d’événement (l’objet déclenchement de l’événement). Examinons quelques exemples de code.

Tout d’abord, la description dans le code ci-dessous définit une classe de **source d’événement** simple, qui déclenche un événement générique qui est géré par les délégués qui ont été ajoutés à celui-ci. Cet exemple d’événement se produit à utiliser le type de délégué [**Windows::Foundation:: EventHandler**](/uwp/api/windows.foundation.eventhandler) , mais les problèmes et des solutions ici s’appliquent aux types de toutes les délégué.

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

Le modèle est que le destinataire d’événement a un gestionnaire d’événements lambda avec les dépendances sur son *ce* pointeur. Chaque fois que le destinataire d’événement survit à la source d’événement, elle survit à ces dépendances. Et dans ces cas, qui sont communs, le modèle fonctionne bien. Certains de ces cas sont évidents, par exemple, lorsqu’une page d’interface utilisateur gère un événement déclenché par un contrôle qui se trouve sur la page. Le bouton de survit à la page&mdash;par conséquent, le gestionnaire est également survit au bouton. Cela est vrai chaque fois que le destinataire est le propriétaire de la source (comme membre de données, par exemple), ou chaque fois que le destinataire et la source sont frère/sœur et appartiennent directement à un autre objet. Si vous êtes certain que vous avez un cas où le gestionnaire ne survivra pas au *this* dont il dépend, vous pouvez capturer *this* normalement, sans considération pour la durée de vie forte ou faible.

Mais il existe cependant des cas où *cette* ne survit pas à son utilisation dans un gestionnaire (y compris les gestionnaires pour les événements de progression déclenchés par les opérations et les actions asynchrones et l’achèvement), il est important de savoir comment les traiter avec eux.

- Si vous créez une coroutine pour implémenter une méthode asynchrone, alors cela est possible.
- Dans de rares cas avec certains objets d’infrastructure de l’interface utilisateur XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), par exemple), cela est possible, si le destinataire est finalisé sans désinscription de la source d’événement.

### <a name="the-issue"></a>Le problème

Cette nouvelle version de la fonction **principale** simule ce qui se produit lorsque le destinataire d’événement est détruit (peut-être qu’il est hors de portée) alors que la source d’événement est toujours déclenchement d’événements.

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

Le destinataire d’événement est détruit, mais le Gestionnaire d’événements lambda au sein de celle-ci est toujours abonné à **l’événement** . Lorsque cet événement est déclenché, l’expression lambda tente de supprimer la référence à *ce* pointeur, qui est à ce stade non valide. Par conséquent, une violation d’accès résulte du code dans le gestionnaire (ou dans la continuation d’une coroutine) vous tentez de l’utiliser.

> [!IMPORTANT]
> Si vous rencontrez une telle situation, vous devez penser à la durée de vie de l’objet de *ce* ; et ou non le capturée *cet* objet survit à la capture. Si ce n’est pas le cas, capturez-le avec une référence forte ou faible, comme nous allons le montrer ci-dessous.
>
> Ou&mdash;si elle a du sens pour votre scénario et si les threads considérations le rendent même possible&mdash;, alors une autre option consiste à révoquer le gestionnaire une fois que le destinataire a terminé avec l’événement, ou dans le destructeur du destinataire. Voir [révoquer un délégué inscrit](handle-events.md#revoke-a-registered-delegate).

Voici comment nous allons inscrire le gestionnaire.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

L’expression lambda capture automatiquement des variables locales par référence. Par conséquent, pour cet exemple, nous pourrions de la même avoir écrit ceci.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Dans les deux cas, nous allons simplement la capture le pointeur brut *cette* . Et qui n’a aucun effet sur le décompte de références, afin que rien est empêche la destruction de l’objet en cours.

### <a name="the-solution"></a>La solution

La solution consiste à capturer une référence forte. Une référence forte *n’est* incrémente le décompte de références et il *est* conserver l’objet en cours actif. Vous déclarez une variable de capture (appelée `strong_this` dans cet exemple), puis initialisez-la avec un appel à [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function), qui Récupère une référence forte à notre *ce* pointeur.

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Vous pouvez même omettre la capture automatique de l’objet en cours et accéder au membre de données via la variable de capture à la place de via le implicite *cette*.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Si une référence forte n’est pas appropriée, vous pouvez appeler [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) pour récupérer une référence faible à *cette*à la place. Confirmez que vous pouvez toujours récupérer une référence forte à partir de celui-ci avant d’accéder aux membres.

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

Ainsi que les fonctions d’expression lambda, ces principes s’appliquent également à l’aide d’une fonction membre en tant que votre délégué. La syntaxe est différente, par conséquent, nous allons étudier un code. Tout d’abord, voici le Gestionnaire d’événements fonction membre potentiellement dangereux, à l’aide d’un brut *ce* pointeur.

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

Il s’agit de la manière standard, classique pour faire référence à un objet et sa fonction membre. Pour rendre cet sans échec, vous pouvez&mdash;à compter de la version 10.0.17763.0 (Windows 10, version 1809) du SDK Windows&mdash;établir une référence forte ou faible au niveau du point où le gestionnaire est enregistré. À ce stade, l’objet destinataire d’événement est connu pour être restés actifs.

Pour une référence forte, simplement appeler [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) à la place le brutes *ce* pointeur. C++ / WinRT permet de s’assurer que le délégué qui en résulte conserve une référence forte à l’objet en cours.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

Pour une référence faible, appelez [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function). C++ / WinRT permet de s’assurer que le délégué qui en résulte conserve une référence faible. À la dernière minute et en arrière-plan, le délégué tente de résoudre la référence faible vers une forte et uniquement appelle la fonction membre si l’opération a réussi.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Un exemple de référence faible à l’aide de **SwapChainPanel::CompositionScaleChanged**

Dans cet exemple de code, nous utilisons l’événement [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) par une autre illustration de références faibles. Le code inscrit un gestionnaire d’événements à l’aide d’une expression lambda qui capture une référence faible au destinataire.

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

Ci-dessus, nous avons vu des références faibles utilisés. En règle générale, elles sont parfaites pour endommager les références cycliques. Par exemple, pour l’implémentation de l’infrastructure d’interface utilisateur XAML native&mdash;en raison de la conception historique de l’infrastructure&mdash;le références faibles mécanisme en C++ / WinRT est nécessaire pour gérer les références cycliques. En dehors de XAML, cependant, vous ne devrez probablement utiliser des références faibles (qu’il y a rien n’est pas intrinsèquement XAML spécifiques à leur sujet). Au lieu de cela, plus souvent, soyez en mesure de concevoir votre propre C++ / WinRT APIs de manière à éviter le besoin de références cycliques et des références faibles. 

Pour n’importe quel type donné que vous déclarez, il n’est pas immédiatement évident pour C++/WinRT de déterminer si des références faibles sont nécessaires. Par conséquent, C++/WinRT fournit la prise en charge des références faibles automatiquement sur le modèle de structure [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), à partir duquel vos propres types C++/WinRT dérivent directement ou indirectement. Il s’agit d’une prise en charge «payer pour lire», dans la mesure où elle ne vous coûte rien, sauf si votre objet est réellement interrogé pour [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource). Et vous pouvez choisir explicitement de [refuser cette prise en charge](#opting-out-of-weak-reference-support).

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

La création d’une référence faible n’affecte pas le nombre de références sur l’objet lui-même; elle entraîne simplement l’allocation d’un bloc de contrôle. Ce bloc de contrôle s’occupe de l’implémentation de la sémantique des références faibles. Vous pouvez essayer de promouvoir la référence faible pour une référence forte et, en cas de réussite, l’utiliser.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

À condition qu’une autre référence forte existe toujours, l’appel [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) incrémente le nombre de références et retourne la référence forte à l’appelant.

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

Peu importe où la structure de marqueur apparaît dans le pack de paramètres variadiques. Si vous demandez une référence faible pour un type refusé, le compilateur vous aidera en affichant une erreur «*This is only for weak ref support*» (ceci est réservé à la prise en charge des références faibles).

## <a name="important-apis"></a>API importantes
* [Fonction implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [Modèle de fonction winrt::make_weak](/uwp/cpp-ref-for-winrt/make-weak)
* [Structure de marqueur winrt::no_weak_ref](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [Modèle de structure winrt::weak_ref](/uwp/cpp-ref-for-winrt/weak-ref)
