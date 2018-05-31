---
author: stevewhims
description: Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec C++/WinRT.
title: Opérations concurrentes et asynchrones avec C++/WinRT
ms.author: stwhi
ms.date: 04/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, concurrence, asynchrone, async
ms.localizationpriority: medium
ms.openlocfilehash: 3af9125abc3abf41327f5b49e6a05d81e214f89f
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1831833"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Opérations concurrentes et asynchrones avec [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec C++/WinRT.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Opérations asynchrones et fonctions «Async» Windows Runtime
Les API Windows Runtime dont l’exécution est susceptible de prendre plus de 50millisecondes sont implémentées en tant que fonctions asynchrones (avec un nom se terminant par «Async»). L’implémentation d’une fonction asynchrone lance le travail sur un autre thread et renvoie immédiatement un objet qui représente l’opération asynchrone. À la fin de l’opération asynchrone, l’objet renvoyé contient n’importe quelle valeur qui résulte du travail. L’espace de noms Windows Runtime **Windows::Foundation** contient quatre types d’objet d’opération asynchrone, à savoir

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) et
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Chacun de ces types d’opération asynchrone est projeté dans un type correspondant dans l’espace de noms C++/WinRT **winrt::Windows::Foundation**. C++/WinRT contient également une structure adaptateur d’attente interne. Vous ne l’utilisez pas directement, mais grâce à cette structure, vous pouvez écrire une instruction **co_await** pour attendre de manière coopérative le résultat de n’importe quelle fonction qui retourne l’un de ces types d’opération asynchrone. Et vous pouvez créer vos propres coroutines qui renvoient ces types.

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

Appeler **get** rend le codage pratique, mais cela n’est pas à proprement parler «coopératif». Ce n’est ni concurrent ni asynchrone. Pour éviter d’empêcher les threads du système d’exploitation d’effectuer d’autres tâches utiles, nous avons besoin d’une autre technique.

## <a name="write-a-coroutine"></a>Écrire une coroutine
C++/WinRT intègre des coroutines C++ dans le modèle de programmation pour fournir un moyen naturel d’attendre de manière coopérative un résultat. Vous pouvez générer votre propre opération asynchrone Windows Runtime en écrivant une coroutine. Dans l’exemple de code ci-dessous, **ProcessFeedAsync** est la coroutine.

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed syndicationFeed)
{
    for (SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = co_await syndicationClient.RetrieveFeedAsync(rssFeedUri);
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

Une coroutine est une fonction qui peut être suspendue et reprise. Dans la coroutine **ProcessFeedAsync** ci-dessus, lorsque l’instruction **co_await** est atteinte, la coroutine lance de façon asynchrone l’appel **RetrieveFeedAsync**, puis se suspend immédiatement et retourne le contrôle à l’appelant (à savoir **main** dans l’exemple ci-dessus). **main** peut alors continuer sa tâche pendant que le flux est récupéré et imprimé. Lorsque l’opération est effectuée (lorsque l’appel **RetrieveFeedAsync** se termine), la coroutine **ProcessFeedAsync** reprend à l’instruction suivante.

Vous pouvez agréger une couroutine dans d’autres coroutines. Ou vous pouvez appeler **get** pour la bloquer et attendre qu’elle se termine (et obtenir le résultat, le cas échéant). Ou vous pouvez la transmettre à un autre langage de programmation qui prend en charge Windows Runtime.

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

void PrintFeed(SyndicationFeed syndicationFeed)
{
    for (SyndicationItem syndicationItem : syndicationFeed.Items())
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

Si vous renvoyez de façon asynchrone un type Windows Runtime (que ce soit un type interne ou tiers), vous devez renvoyer un [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) ou un [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Le compilateur vous aidera en affichant une erreur «*must be WinRT type*» (doit être de type WinRT) si vous essayez d’utiliser un de ces types d’opération asynchrone avec un type non Windows Runtime.

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Retourner de façon asynchrone un type non Windows Runtime
Si vous renvoyez de façon asynchrone un type qui n’est *pas* un type Windows Runtime, vous devez renvoyer une bibliothèque de modèles parallèles (PPL) [**task**](https://msdn.microsoft.com/library/hh750113). Nous vous recommandons **task**, car elle vous donne de meilleures performances (et une meilleure compatibilité à l’avenir) que **std::future**.

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
    // do other work.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="important-apis"></a>API importantes
* [concurrency::task](https://msdn.microsoft.com/library/hh750113)
* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
