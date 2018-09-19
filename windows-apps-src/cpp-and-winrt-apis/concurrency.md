---
author: stevewhims
description: Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec C++/WinRT.
title: Opérations concurrentes et asynchrones avec C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, concurrence, asynchrone, async
ms.localizationpriority: medium
ms.openlocfilehash: 85071fb28cb87c991e2f5ba7f64b681c6850c819
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4061033"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Opérations concurrentes et asynchrones avec [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.**

Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec C++/WinRT.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Opérations asynchrones et fonctions «Async» Windows Runtime
Les API Windows Runtime dont l’exécution est susceptible de prendre plus de 50millisecondes sont implémentées en tant que fonctions asynchrones (avec un nom se terminant par «Async»). L’implémentation d’une fonction asynchrone lance le travail sur un autre thread et renvoie immédiatement un objet qui représente l’opération asynchrone. À la fin de l’opération asynchrone, l’objet renvoyé contient n’importe quelle valeur qui résulte du travail. L’espace de noms Windows Runtime **Windows::Foundation** contient quatre types d’objet d’opération asynchrone.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) et
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Chacun de ces types d’opération asynchrone est projeté dans un type correspondant dans l’espace de noms C++/WinRT **winrt::Windows::Foundation**. C++/WinRT contient également une structure adaptateur d’attente interne. Vous ne l’utilisez pas directement, mais grâce à cette structure, vous pouvez écrire une instruction «co_await» pour attendre de manière coopérative le résultat de n’importe quelle fonction qui retourne l’un de ces types d’opération asynchrone. Et vous pouvez créer vos propres coroutines qui renvoient ces types.

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
> La fonction **obtenir** existe sur C++ / WinRT projection tapez **winrt::Windows::Foundation::IAsyncAction**, afin que vous pouvez appeler la fonction dans n’importe quel C++ / WinRT projet. Vous ne trouverez pas la fonction répertoriée en tant que membre de l’interface [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) , dans la mesure où **obtenir** ne fait pas partie de la surface de (ABI) de l’interface binaire d’application du type Windows Runtime réel **IAsyncAction**.

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

> [!NOTE]
> **Le code suivant concerne la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.**

Pour une solution plus générale de mise à jour de l’interface utilisateur, qui traite les cas où vous avez des incertitudes quant au thread appelant, vous pouvez installer le [SDKWindows10 version d'évaluation17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK) ou une version ultérieure. Ensuite, vous pouvez attendre (`co-await`) la fonction **winrt::resume_foreground** pour basculer vers un thread spécifique au premier plan. Dans l’exemple de code ci-dessous, nous spécifions le thread de premier plan en passant l’objet répartiteur associé au **TextBlock** (en accédant à sa propriété [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). L’implémentation de **winrt::resume_foreground** appelle [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) sur cet objet répartiteur afin d'exécuter la tâche qui apparaît dans la coroutine.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
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

## <a name="related-topics"></a>Rubriquesconnexes
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)
* [Types de données C++ standard et C++/WinRT](std-cpp-data-types.md)