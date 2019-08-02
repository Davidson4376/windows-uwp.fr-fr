---
description: Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec C++/WinRT.
title: Opérations concurrentes et asynchrones avec C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, concurrence, asynchrone, async
ms.localizationpriority: medium
ms.openlocfilehash: 1dd6ac2760189578932fc22db89c7091f2e527ab
ms.sourcegitcommit: 8179902299df0f124dd770a09a5a332397970043
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68428637"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Opérations concurrentes et asynchrones avec C++/WinRT

Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

Après avoir lu cette rubrique, consultez également [Concurrence et opérations asynchrones plus avancées](concurrency-2.md) pour davantage de scénarios.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Opérations asynchrones et fonctions « Async » Windows Runtime

Les API Windows Runtime dont l’exécution est susceptible de prendre plus de 50 millisecondes sont implémentées en tant que fonctions asynchrones (avec un nom se terminant par « Async »). L’implémentation d’une fonction asynchrone lance le travail sur un autre thread et retourne immédiatement un objet qui représente l’opération asynchrone. À la fin de l’opération asynchrone, l’objet retourné contient n’importe quelle valeur qui résulte du travail. L’espace de noms Windows Runtime **Windows::Foundation** contient quatre types d’objets d’opérations asynchrones.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) et
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Chacun de ces types d’opérations asynchrones est projeté en un type correspondant dans l’espace de noms C++/WinRT **winrt::Windows::Foundation**. C++/WinRT contient également un struct d’adaptateur await interne. Vous ne l’utilisez pas directement, mais grâce à cette structure, vous pouvez écrire une instruction `co_await` pour attendre de manière coopérative le résultat de n’importe quelle fonction qui retourne l’un de ces types d’opérations asynchrones. Et vous pouvez créer vos propres coroutines qui retournent ces types.

Un exemple de fonction Windows asynchrone est [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), qui retourne un objet d’opération asynchrone de type [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Examinons des façons, tout d’abord bloquantes, puis non bloquantes, d’utiliser C++/WinRT pour appeler une API similaire.

## <a name="block-the-calling-thread"></a>Bloquer le thread appelant

L’exemple de code ci-dessous reçoit un objet d’opération asynchrone à partir de **RetrieveFeedAsync** et appelle **get** sur cet objet pour bloquer le thread appelant jusqu’à ce que les résultats de l’opération asynchrone soient disponibles.

Si vous souhaitez copier-coller cet exemple directement dans le fichier de code source principal d’un projet **Application console Windows (C++/WinRT)** , définissez d’abord **Sans utiliser les en-têtes précompilés** dans les propriétés du projet.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
    // use syndicationFeed.
}

int main()
{
    winrt::init_apartment();
    ProcessFeed();
}
```

Appeler **get** rend le codage pratique et est idéal pour les applications console ou les threads d’arrière-plan dans lesquels vous ne souhaitez pas utiliser de coroutine pour une raison quelconque. Mais ce n’est ni simultané ni asynchrone, donc ce n’est pas approprié pour un thread d’interface utilisateur (et une assertion se déclenchera dans les versions non optimisées, si vous tentez de l’utiliser sur l’une d’elles). Pour éviter d’empêcher les threads du système d’exploitation d’effectuer d’autres tâches utiles, nous avons besoin d’une autre technique.

## <a name="write-a-coroutine"></a>Écrire une coroutine

C++/WinRT intègre des coroutines C++ dans le modèle de programmation pour fournir un moyen naturel d’attendre de manière coopérative un résultat. Vous pouvez générer votre propre opération asynchrone Windows Runtime en écrivant une coroutine. Dans l’exemple de code ci-dessous, **ProcessFeedAsync** est la coroutine.

> [!NOTE]
> La fonction **get** existe sur le type de projection C++/WinRT **winrt::Windows::Foundation::IAsyncAction** ; vous pouvez donc l’appeler à partir de n’importe quel projet C++/WinRT. Vous ne trouverez pas la fonction **get** listée en tant que membre de l’interface [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction), car elle ne fait pas partie de la surface d’interface binaire-programme (ABI, Application Binary Interface) du type Windows Runtime **IAsyncAction**.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

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

    auto processOp{ ProcessFeedAsync() };
    // do other work while the feed is being printed.
    processOp.get(); // no more work to do; call get() so that we see the printout before the application exits.
}
```

Une coroutine est une fonction qui peut être suspendue et reprise. Dans la coroutine **ProcessFeedAsync** ci-dessus, quand l’instruction `co_await` est atteinte, la coroutine lance de façon asynchrone l’appel **RetrieveFeedAsync**, puis se suspend immédiatement et retourne le contrôle à l’appelant (à savoir **main** dans l’exemple ci-dessus). **main** peut alors continuer sa tâche pendant que le flux est récupéré et imprimé. Quand l’opération est effectuée (lorsque l’appel **RetrieveFeedAsync** se termine), la coroutine **ProcessFeedAsync** reprend à l’instruction suivante.

Vous pouvez agréger une coroutine dans d’autres coroutines. Ou vous pouvez appeler **get** pour la bloquer et attendre qu’elle se termine (et obtenir le résultat, le cas échéant). Ou vous pouvez la transmettre à un autre langage de programmation qui prend en charge Windows Runtime.

Il est également possible de gérer les événements terminés et/ou en cours des actions et des opérations asynchrones à l’aide de délégués. Pour plus d’informations et pour obtenir des exemples de code, consultez [Types délégués pour les actions et opérations asynchrones](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="asychronously-return-a-windows-runtime-type"></a>Retourner de façon asynchrone un type Windows Runtime

Dans l’exemple suivant, nous allons encapsuler un appel à **RetrieveFeedAsync**, pour un URI spécifique, afin d’obtenir une fonction **RetrieveBlogFeedAsync** qui retourne de façon asynchrone un [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed).

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

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

    auto feedOp{ RetrieveBlogFeedAsync() };
    // do other work.
    PrintFeed(feedOp.get());
}
```

Dans l’exemple ci-dessus, **RetrieveBlogFeedAsync** retourne un **IAsyncOperationWithProgress**, qui a une progression et une valeur de retour. Nous pouvons effectuer d’autres tâches pendant que **RetrieveBlogFeedAsync** effectue le traitement et récupère le flux. Ensuite, nous allons appeler **get** sur cet objet d’opération asynchrone à bloquer, attendre qu’il se termine et obtenir les résultats de l’opération.

Si vous retournez de façon asynchrone un type Windows Runtime, vous devez retourner un [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) ou un [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Toute classe runtime interne ou tierce est appropriée, ou tout type qui peut être transmis vers ou à partir d’une fonction Windows Runtime (par exemple `int` ou **winrt::hstring**). Le compilateur vous aidera en affichant une erreur « *must be WinRT type* » (doit être de type WinRT) si vous essayez d’utiliser l’un de ces types d’opérations asynchrones avec un type non Windows Runtime.

Si une coroutine ne possède pas au moins une instruction `co_await`, pour être appropriée en tant que coroutine elle doit avoir au moins une instruction `co_return` ou `co_yield`. Il y aura des cas où votre coroutine peut retourner une valeur sans présenter de comportement asynchrone et donc sans blocage ni changement de contexte. Voici un exemple qui le fait (au deuxième appel et aux suivants) en mettant une valeur en cache.

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

Si vous retournez de façon asynchrone un type qui n’est *pas* un type Windows Runtime, vous devez retourner une bibliothèque de modèles parallèles (PPL, Parallel Patterns Library) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class). Nous vous recommandons **concurrency::task**, car elle vous donne de meilleures performances (et une meilleure compatibilité à l’avenir) que **std::future**.

> [!TIP]
> Si vous incluez `<pplawait.h>`, vous pouvez ensuite utiliser **concurrency::task** comme type de coroutine.

```cppwinrt
// main.cpp
#include <iostream>
#include <ppltasks.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
        {
            Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
            SyndicationClient syndicationClient;
            SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
            return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
        });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp{ RetrieveFirstTitleAsync() };
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

    co_await DoOtherWorkAsync(); // (this is the first suspension point)...

    // ...accessing value here carries no guarantees of safety.
}
```

Dans une coroutine, l’exécution est synchrone jusqu’au premier point d’interruption, où le contrôle est retourné à l’appelant et où la trame d’appel devient hors de portée. Le temps que la coroutine reprenne, tout peut arriver à la valeur source référencée par un paramètre de référence. Du point de vue de la coroutine, un paramètre de référence a une durée de vie non contrôlée. Ainsi, dans l’exemple ci-dessus, nous sommes sûrs d’accéder à *value* jusqu’au `co_await`, mais pas après celui-ci. Si *value* est détruit par l’appelant, toute tentative pour y accéder plus tard dans la coroutine entraîne une altération de la mémoire. Nous ne pouvons pas non plus transmettre en toute sécurité *value* à **DoOtherWorkAsync** s’il y a un risque que cette fonction s’interrompe à son tour et essaye d’utiliser *value* après sa reprise.

Pour sécuriser l’utilisation des paramètres après une interruption et une reprise, vos coroutines doivent employer le passage par valeur par défaut. Ainsi, vous avez la garantie qu’elles effectuent des captures par valeur. De plus, cela vous permet d’éviter les problèmes de durée de vie. Les cas où vous pouvez vous écarter de ces recommandations car vous êtes certain que la méthode est sécurisée seront rares.

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value); // not const&
```

Avec le passage par valeur, l’argument doit être peu coûteux à déplacer ou à copier. Cela est généralement le cas d’un pointeur intelligent.

On pourrait également considérer comme recommandé (sauf si vous voulez déplacer la valeur) le passage par la valeur const. Cela n’aura aucun effet sur la valeur source à partir de laquelle vous effectuez une copie, mais cela rend l’intention claire, et peut aider si vous modifiez la copie par inadvertance.

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

Voir également [Vecteurs et tableaux standard](std-cpp-data-types.md#standard-arrays-and-vectors), qui aborde la manière de passer un vecteur standard dans un appelé asynchrone.

Si vous ne pouvez pas changer la signature de votre coroutine, mais que vous pouvez changer l’implémentation, créez une copie locale avant le premier `co_await`.

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_value = value;
    // It's ok to access both safe_value and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_value here (not value).
}
```

Si `Param` est coûteux à copier, extrayez simplement les parties dont vous avez besoin avant le premier `co_await`.

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_data = value.data;
    // It's ok to access safe_data, value.data, and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_data here (not value.data, nor value).
}
```

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Accès sécurisé au pointeur *this* dans une coroutine de membre de classe

Consultez [Références fortes et faibles en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine).

## <a name="important-apis"></a>API importantes
* [concurrency::task, classe](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction, interface](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;, interface](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;, interface](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;, interface](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync, méthode](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed, classe](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>Rubriques connexes
* [Concurrence et opérations asynchrones plus avancées](concurrency-2.md)
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)
* [Types de données C++ standard et C++/WinRT](std-cpp-data-types.md)