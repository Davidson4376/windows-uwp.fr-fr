---
author: stevewhims
description: Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec C++/WinRT.
title: Opérations concurrentes et asynchrones avec C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, concurrence, asynchrone, async
ms.localizationpriority: medium
ms.openlocfilehash: 9f29828a800795aba70c17bcab19b56b85d56382
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4499653"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Opérations concurrentes et asynchrones avec C++/WinRT

Cette rubrique montre les méthodes dans lesquelles vous pouvez créer et utilisent des objets asynchrones Windows Runtime avec [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Opérations asynchrones et fonctions «Async» Windows Runtime
Les API Windows Runtime dont l’exécution est susceptible de prendre plus de 50millisecondes sont implémentées en tant que fonctions asynchrones (avec un nom se terminant par «Async»). L’implémentation d’une fonction asynchrone lance le travail sur un autre thread et renvoie immédiatement un objet qui représente l’opération asynchrone. À la fin de l’opération asynchrone, l’objet renvoyé contient n’importe quelle valeur qui résulte du travail. L’espace de noms Windows Runtime **Windows::Foundation** contient quatre types d’objet d’opération asynchrone.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) et
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Chacun de ces types d’opération asynchrone est projeté dans un type correspondant dans l’espace de noms C++/WinRT **winrt::Windows::Foundation**. C++/WinRT contient également une structure adaptateur d’attente interne. Vous ne l’utilisez directement, mais grâce à cette structure, vous pouvez écrire un `co_await` instruction à coopérative le résultat de n’importe quelle fonction qui retourne l’un de ces types d’opération. Et vous pouvez créer vos propres coroutines qui renvoient ces types.

Un exemple de fonction Windows asynchrone est [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), qui retourne un objet d’opération asynchrone de type [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Examinons des façons &mdash;bloquantes et non bloquantes&mdash; d’utiliser C++/WinRT pour appeler une API similaire.

## <a name="block-the-calling-thread"></a>Bloquer le thread appelant
L’exemple de code ci-dessous reçoit un objet d’opération asynchrone à partir de **RetrieveFeedAsync** et appelle **get** sur cet objet pour bloquer le thread appelant jusqu'à ce que les résultats de l’opération asynchrone soient disponibles.

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    // use syndicationFeed.
}

int main()
{
    winrt::init_apartment();
    ProcessFeed();
}
```

Appeler **get** rend le codage pratique et est idéal pour les applications de console ou les threads d’arrière-plan dans lesquels vous ne souhaitez pas utiliser de coroutine pour une raison quelconque. Mais ce n’est ni simultané ni asynchrone, donc ce n’est pas approprié pour un thread d’interface utilisateur (et une assertion se déclenchera dans les versions non optimisées, si vous tentez de l'utiliser sur une). Pour éviter d’empêcher les threads du système d’exploitation d’effectuer d’autres tâches utiles, nous avons besoin d’une autre technique.

## <a name="write-a-coroutine"></a>Écrire une coroutine
C++/WinRT intègre des coroutines C++ dans le modèle de programmation pour fournir un moyen naturel d’attendre de manière coopérative un résultat. Vous pouvez générer votre propre opération asynchrone Windows Runtime en écrivant une coroutine. Dans l’exemple de code ci-dessous, **ProcessFeedAsync** est la coroutine.

> [!NOTE]
> La fonction **get** existe sur C++ / WinRT projection tapez **winrt::Windows::Foundation::IAsyncAction**, afin que vous pouvez appeler la fonction dans n’importe quel C++ / WinRT projet. Vous ne trouverez pas la fonction répertoriée en tant que membre de l’interface [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) , dans la mesure où **obtenir** ne fait pas partie de la surface de (ABI) de l’interface binaire d’application du type Windows Runtime réel **IAsyncAction**.

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    PrintFeed(syndicationFeed);
}

int main()
{
    winrt::init_apartment();

    auto processOp = ProcessFeedAsync();
    // do other work while the feed is being printed.
    processOp.get(); // no more work to do; call get() so that we see the printout before the application exits.
}
```

Une coroutine est une fonction qui peut être suspendue et reprise. Dans la coroutine **ProcessFeedAsync** ci-dessus, lorsque l’instruction `co_await` est atteinte, la coroutine lance de façon asynchrone l’appel **RetrieveFeedAsync**, puis se suspend immédiatement et retourne le contrôle à l’appelant (à savoir **main** dans l’exemple ci-dessus). **main** peut alors continuer sa tâche pendant que le flux est récupéré et imprimé. Lorsque l’opération est effectuée (lorsque l’appel **RetrieveFeedAsync** se termine), la coroutine **ProcessFeedAsync** reprend à l’instruction suivante.

Vous pouvez agréger une coroutine dans d’autres coroutines. Ou vous pouvez appeler **get** pour la bloquer et attendre qu’elle se termine (et obtenir le résultat, le cas échéant). Ou vous pouvez la transmettre à un autre langage de programmation qui prend en charge Windows Runtime.

Il est également possible de gérer les événements terminés et/ou en cours des actions et des opérations asynchrones à l’aide de délégués. Pour plus d’informations et des exemples de code, voir [Types délégués pour les actions et opérations asynchrones](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="asychronously-return-a-windows-runtime-type"></a>Retourner de façon asynchrone un type Windows Runtime
Dans l’exemple suivant, nous allons encapsuler un appel à **RetrieveFeedAsync**, pour un URI spécifique, afin d’obtenir une fonction **RetrieveBlogFeedAsync** qui renvoie de façon asynchrone un [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed).

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> RetrieveBlogFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    return syndicationClient.RetrieveFeedAsync(rssFeedUri);
}

int main()
{
    winrt::init_apartment();

    auto feedOp = RetrieveBlogFeedAsync();
    // do other work.
    PrintFeed(feedOp.get());
}
```

Dans l’exemple ci-dessus, **RetrieveBlogFeedAsync** renvoie un **IAsyncOperationWithProgress**, qui possède une progression et une valeur de retour. Nous pouvons effectuer d’autres tâches pendant que **RetrieveBlogFeedAsync** effectue le traitement et récupère le flux. Ensuite, nous allons appeler **get** sur cet objet d’opération asynchrone à bloquer, attendre qu’il se termine et obtenir les résultats de l’opération.

Si vous renvoyez de façon asynchrone un type Windows Runtime, vous devez renvoyer un [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) ou un [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). N'importe quelle classe runtime interne ou tierce est appropriée ou n’importe quel type qui peut être transmis vers ou à partir d’une fonction Windows Runtime (par exemple, `int` ou **winrt::hstring**). Le compilateur vous aidera en affichant une erreur «*must be WinRT type*» (doit être de type WinRT) si vous essayez d’utiliser un de ces types d’opération asynchrone avec un type non Windows Runtime.

Si une coroutine ne possède pas au moins une instruction `co_await`, pour être appropriée en tant que coroutine, elle doit avoir au moins une instruction `co_return` ou `co_yield`. Il y aura des cas où votre coroutine peut retourner une valeur sans présenter de comportement asynchrone et donc sans blocage ni changement de contexte. Voici un exemple qui le fait (au deuxième appel et aux suivants) en mettant une valeur en cache.

```cppwinrt
winrt::hstring m_cache;
 
IAsyncOperation<winrt::hstring> ReadAsync()
{
    if (m_cache.empty())
    {
        // Asynchronously download and cache the string.
    }
    co_return m_cache;
}
``` 

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Retourner de façon asynchrone un type non Windows Runtime
Si vous renvoyez de façon asynchrone un type qui n’est *pas* un type Windows Runtime, vous devez renvoyer une bibliothèque de modèles parallèles (PPL) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class). Nous vous recommandons **concurrency::task**, car elle vous donne de meilleures performances (et une meilleure compatibilité à l’avenir) que **std::future**.

> [!TIP]
> Si vous incluez `<pplawait.h>`, vous pouvez ensuite utiliser **concurrency::task** comme type de coroutine.

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
#include <ppltasks.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
    {
        Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
        SyndicationClient syndicationClient;
        SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
        return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
    });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp = RetrieveFirstTitleAsync();
    // Do other work here.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="parameter-passing"></a>Passage de paramètres
Pour les fonctions synchrones, vous devez utiliser les paramètres `const&` par défaut. Cela évitera la surcharge liées aux copies (qui impliquent un décompte de références, et des incrémentations et décrémentations imbriquées).

```cppwinrt
// Synchronous function.
void DoWork(Param const& value);
```

Mais vous pouvez rencontrer des problèmes si vous passez un paramètre de référence à une coroutine.

```cppwinrt
// NOT the recommended way to pass a value to a coroutine!
IASyncAction DoWorkAsync(Param const& value)
{
    // While it's ok to access value here...
    
    co_await DoOtherWorkAsync();

    // ...accessing value here carries no guarantees of safety.
}
```

Dans une coroutine, l’exécution est synchrone jusqu'au premier point d'interruption, sur lequel le contrôle est retourné à l’appelant. Le temps que la coroutine reprenne, tout peut arriver à la valeur de la source référencée par un paramètre de référence. Du point de vue de la coroutine, un paramètre de référence a une durée de vie non contrôlée. Dans l’exemple ci-dessus, nous sommes sûrs d'accéder à la *valeur* jusqu'au `co_await`, mais pas après celui-ci. Nous ne pouvons pas non plus transmettre en toute sécurité la *valeur* à **DoOtherWorkAsync** s’il existe un risque que cette fonction s'interrompe à son tour et essaye d’utiliser la *valeur* après sa reprise. Pour sécuriser l'utilisation des paramètres après une interruption et une reprise, vos coroutines doivent utiliser passer par valeur par défaut pour être sûr qu’elles capturent par valeur et éviter les problèmes de durée de vie. Les cas où vous pouvez vous écarter de ces recommandations parce que vous êtes certain que la méthode est sécurisée seront rares.

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value);
```

Il est également discutable de considérer comme recommandé (sauf si vous voulez déplacer la valeur) le passage par la valeur const. Cela n’aura aucun effet sur la valeur de la source à partir de laquelle vous effectuez une copie, mais cela rend l'intention claire, et peut aider si vous modifiez la copie par inadvertance.
    
```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

Voir également [Vecteurs et tableaux standard](std-cpp-data-types.md#standard-arrays-and-vectors), qui porte sur la manière de passer un vecteur standard dans un appelé asynchrone.

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Déchargement de tâches sur le pool de threads Windows
Avant d'effectuer une tâche liée au calcul dans une coroutine, vous devez renvoyer l’exécution à l’appelant afin que l’appelant ne soit pas bloqué (en d’autres termes, présenter un point d'interruption). Si vous ne le faites pas déjà avec une instruction `co-await` pour attendre une autre opération, vous pouvez `co-await` la fonction **winrt::resume_background**. Cela retourne le contrôle à l’appelant, puis reprend immédiatement l'exécution sur un thread de pool de threads.

Le pool de threads utilisé dans l’implémentation est le [pool de threads Windows](https://msdn.microsoft.com/library/windows/desktop/ms686766) de bas niveau. Son efficacité est donc optimale.

```cppwinrt
IAsyncOperation<uint32_t> DoWorkOnThreadPoolAsync()
{
    co_await winrt::resume_background(); // Return control; resume on thread pool.

    uint32_t result;
    for (uint32_t y = 0; y < height; ++y)
    for (uint32_t x = 0; x < width; ++x)
    {
        // Do compute-bound work here.
    }
    co_return result;
}
```

## <a name="programming-with-thread-affinity-in-mind"></a>Programmation en tenant compte de l’affinité des threads
Ce scénario s'appuie sur le précédent. Vous déchargez certaines tâches sur le pool de threads, mais vous souhaitez ensuite afficher la progression dans l’interface utilisateur.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

Le code ci-dessus lève une exception [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/hresult-wrong-thread), car un **TextBlock** doit être mis à jour à partir du thread qui l’a créé, à savoir le thread d’interface utilisateur. Une solution consiste à capturer le contexte du thread dans lequel notre coroutine a été appelée à l’origine. Instanciez un objet **winrt::apartment_context**, puis attendez-le en utilisant `co_await`.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

Tant que la coroutine ci-dessus est appelée à partir du thread d’interface utilisateur qui a créé le **TextBlock**, cette technique fonctionne. Vous en serez certain dans de nombreux cas dans votre application.

Pour une solution plus générale pour la mise à jour de l’interface utilisateur, qui traite les cas où vous avez des incertitudes quant au thread appelant, vous pouvez `co-await` la fonction **winrt::resume_foreground** pour basculer vers un thread spécifique au premier plan. Dans l’exemple de code ci-dessous, nous spécifions le thread de premier plan en passant l’objet répartiteur associé au **TextBlock** (en accédant à sa propriété [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). L’implémentation de **winrt::resume_foreground** appelle [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) sur cet objet répartiteur afin d'exécuter la tâche qui apparaît dans la coroutine.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>L’annulation d’une opération asynchrone et rappels d’annulation

Fonctionnalités de Windows Runtime pour la programmation asynchrone permettent d’annuler une opération ou une action asynchrone en cours d’exécution. Commençons par un exemple simple.

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

IAsyncAction ImplicitCancellationAsync()
{
    while (true)
    {
        std::cout << "ImplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto implicit_cancellation{ ImplicitCancellationAsync() };
    co_await 3s;
    implicit_cancellation.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

Si vous exécutez l’exemple ci-dessus, puis vous verrez un message d’impression **ImplicitCancellationAsync** par seconde pour les trois secondes, après lequel fois qu’elle automatiquement s’arrête à la suite en cours d’annulation. Cela fonctionne dans la mesure où, en rencontrent un `co_await` expression, une coroutine vérifie si elle a été annulée. Si tel est le cas, puis elle court-circuite et si elle n’a pas encore, puis il suspend comme normal.

L’annulation peut, bien entendu, se produire lorsque la coroutine est suspendue. Uniquement lorsque la coroutine reprend, ou appuie sur un autre `co_await`, il vérifie l’annulation. Le problème est l’un des potentiellement trop approximative-accrue de latence dans la réponse à l’annulation.

Par conséquent, une autre option consiste à interroger explicitement l’annulation à partir d’au sein de votre coroutine. Mettre à jour de l’exemple ci-dessus par le code dans la liste ci-dessous. Dans cet exemple de nouveau, **ExplicitCancellationAsync** récupère l’objet renvoyé par la fonction [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token) et l’utilise pour vérifier régulièrement si la coroutine a été annulée. Tant qu’il n’est pas annulé, la coroutine effectue une boucle indéfiniment; une fois qu’il est annulé, la boucle et la fonction de sortie normalement. Le résultat est le même, comme l’exemple précédent, mais sortant ici se produit de manière explicite et sous le contrôle.

```cppwinrt
...
IAsyncAction ExplicitCancellationAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };

    while (!cancellation_token())
    {
        std::cout << "ExplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto explicit_cancellation{ ExplicitCancellationAsync() };
    co_await 3s;
    explicit_cancellation.Cancel();
}
...
```

En attente sur **winrt::get_cancellation_token** récupère un jeton d’annulation en sachant de **IAsyncAction** que la coroutine est produisant en votre nom. Vous pouvez utiliser l’opérateur d’appel de fonction sur ce jeton d’interroger l’état d’annulation&mdash;essentiellement d’interroger l’annulation. Si vous effectuez une opération liée au calcul, ou une itération dans une grande collection, puis il s’agit d’une technique raisonnable.

### <a name="register-a-cancellation-callback"></a>Inscrire un rappel d’annulation
Annulation de l’exécution Windows n’a pas automatiquement du flux à d’autres objets asynchrones. Mais&mdash;introduites dans la version 10.0.17763.0 (Windows 10, version 1809) du SDK Windows&mdash;vous pouvez inscrire un rappel d’annulation. Il s’agit d’un hook préventif par lequel annulation peut être propagée et rend possible pour l’intégration avec les bibliothèques d’accès concurrentiel existantes.

Dans cet exemple de code suivant, **NestedCoroutineAsync** effectue le travail, mais elle n’a aucune raison d’annulation spécial qu’il. **CancellationPropagatorAsync** est essentiellement un wrapper sur la coroutine imbriqué; le wrapper transfère annulation préalablement.

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

IAsyncAction NestedCoroutineAsync()
{
    while (true)
    {
        std::cout << "NestedCoroutineAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction CancellationPropagatorAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };
    auto nested_coroutine{ NestedCoroutineAsync() };

    cancellation_token.callback([&]
    {
        nested_coroutine.Cancel();
    });

    co_await nested_coroutine;
}

IAsyncAction MainCoroutineAsync()
{
    auto cancellation_propagator{ CancellationPropagatorAsync() };
    co_await 3s;
    cancellation_propagator.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

**CancellationPropagatorAsync** enregistre une fonction lambda pour son propre rappel d’annulation, puis il attend (elle suspend) jusqu'à ce que le travail imbriqué se termine. Lorsqu’ou si **CancellationPropagatorAsync** est annulé, il propage l’annulation à la coroutine imbriquée. Il n’est pas nécessaire pour l’interrogation de l’annulation; ni l’annulation est bloquée pendant une période indéfinie. Ce mécanisme est suffisamment flexible pour pouvoir l’utiliser pour l’interopérabilité avec une bibliothèque de coroutine ou d’accès concurrentiel qui ne sait rien de C++ / WinRT.

## <a name="reporting-progress"></a>Création de rapports de progression

Si votre coroutine retourne [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)ou [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_), puis vous pouvez récupérer l’objet renvoyé par la fonction [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) et utilisez-le pour signaler la progression à une progression Gestionnaire d’événements. Voici un exemple de code.

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

IAsyncOperationWithProgress<double, double> CalcPiTo5DPs()
{
    auto progress{ co_await winrt::get_progress_token() };

    co_await 1s;
    double pi_so_far{ 3.1 };
    progress(0.2);

    co_await 1s;
    pi_so_far += 4.e-2;
    progress(0.4);

    co_await 1s;
    pi_so_far += 1.e-3;
    progress(0.6);

    co_await 1s;
    pi_so_far += 5.e-4;
    progress(0.8);

    co_await 1s;
    pi_so_far += 9.e-5;
    progress(1.0);
    co_return pi_so_far;
}

IAsyncAction DoMath()
{
    auto async_op_with_progress{ CalcPiTo5DPs() };
    async_op_with_progress.Progress([](auto const& /* sender */, double progress)
    {
        std::wcout << L"CalcPiTo5DPs() reports progress: " << progress << std::endl;
    });
    double pi{ co_await async_op_with_progress };
    std::wcout << L"CalcPiTo5DPs() is complete !" << std::endl;
    std::wcout << L"Pi is approx.: " << pi << std::endl;
}

int main()
{
    winrt::init_apartment();
    DoMath().get();
}
```

> [!NOTE]
> Il n’est pas correct d’implémenter plus d’un *Gestionnaire d’achèvement* pour une action asynchrone ou une opération. Vous pouvez avoir soit un délégué unique pour son événement terminé, ou vous pouvez `co_await` il. Si vous avez à la fois, le second échouera. Soit un des deux types suivants de gestionnaires d’achèvement est approprié; pas à la fois pour le même objet asynchrone.

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
async_op_with_progress.Completed([](auto const& sender, AsyncStatus /* status */)
{
    double pi{ sender.GetResults() };
});
```

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
double pi{ co_await async_op_with_progress };
```

Pour plus d’informations sur les gestionnaires d’achèvement, voir [les types de délégués pour les actions et opérations asynchrones](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="fire-and-forget"></a>Déclenchement et oubli

Parfois, vous disposez d’une tâche qui peut être effectuée simultanément avec d’autres tâches, et vous n’avez pas besoin d’attendre que la tâche Terminer (aucun autre travail en dépend), ni vous en avez besoin pour renvoyer une valeur. Dans ce cas, vous pouvez déclencher la tâche et l’oublier. Vous pouvez faire qui en écrivant une coroutine dont le type de retour est [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) (au lieu d’un des types d’opération asynchrone Windows Runtime, ou **concurrency::task**).

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
using namespace winrt;
using namespace std::chrono_literals;

winrt::fire_and_forget CompleteInFiveSeconds()
{
    co_await 5s;
}

int main()
{
    winrt::init_apartment();
    CompleteInFiveSeconds();
    // Do other work here.
}
```

## <a name="important-apis"></a>API importantes
* [classe de Concurrency::Task](/cpp/parallel/concrt/reference/task-class)
* [Interface de IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; interface](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; interface](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; interface](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Méthode de SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Classe de SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
* [WinRT::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [WinRT::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [WinRT::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>Rubriquesconnexes
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)
* [Types de données C++ standard et C++/WinRT](std-cpp-data-types.md)