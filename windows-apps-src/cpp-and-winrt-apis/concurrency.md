---
description: Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec C++/WinRT.
title: Opérations concurrentes et asynchrones avec C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, concurrence, asynchrone, async
ms.localizationpriority: medium
ms.openlocfilehash: f7db1e5810de478f1c6198860100409d79d4f5d5
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270143"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Opérations concurrentes et asynchrones avec C++/WinRT

Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

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

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Déchargement de tâches sur le pool de threads Windows

Une coroutine est une fonction comme toute autre, dans le sens où l’appelant est bloqué jusqu’à ce qu’une fonction lui retourne l’exécution. Et la première occasion qu’a une coroutine de retourner est le premier `co_await`, `co_return` ou `co_yield`.

Avant d’effectuer une tâche liée au calcul dans une coroutine, vous devez retourner l’exécution à l’appelant (en d’autres termes, présenter un point d’interruption) afin que celui-ci ne soit pas bloqué. Si vous ne le faites pas déjà avec une instruction `co_await` pour attendre une autre opération, vous pouvez `co_await` la fonction [**winrt::resume_background**](/uwp/cpp-ref-for-winrt/resume-background). Cela retourne le contrôle à l’appelant, puis reprend immédiatement l’exécution sur un thread de pool de threads.

Le pool de threads utilisé dans l’implémentation est le [pool de threads Windows](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api) de bas niveau. Son efficacité est donc optimale.

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

Ce scénario s’appuie sur le précédent. Vous déchargez certaines tâches sur le pool de threads, mais vous souhaitez ensuite afficher la progression dans l’interface utilisateur.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

Le code ci-dessus lève une exception [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread), car un **TextBlock** doit être mis à jour à partir du thread qui l’a créé, à savoir le thread d’interface utilisateur. Une solution consiste à capturer le contexte du thread dans lequel notre coroutine a été appelée initialement. Pour ce faire, vous devez instancier un objet [**winrt::apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context), effectuer le travail en arrière-plan, puis `co_await` l’**apartment_context** afin de revenir au contexte appelant.

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

Pour obtenir une solution plus générale pour la mise à jour de l’interface utilisateur, qui couvre les cas où vous avez des doutes sur le thread appelant, vous pouvez `co_await` la fonction [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) afin de basculer vers un thread de premier plan spécifique. Dans l’exemple de code ci-dessous, nous spécifions le thread de premier plan en passant l’objet répartiteur associé au **TextBlock** (en accédant à sa propriété [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). L’implémentation de **winrt::resume_foreground** appelle [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) sur cet objet répartiteur afin d’exécuter la tâche qui apparaît dans la coroutine.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>Contextes d’exécution, reprise et basculement dans une coroutine

De manière générale, après un point de suspension dans une coroutine, le thread d’exécution d’origine peut disparaître et la reprise peut se produire sur n’importe quel thread (autrement dit, n’importe quel thread peut appeler la méthode **Completed** pour l’opération asynchrone).

Toutefois, si vous `co_await` l’un des quatre types d’opérations asynchrones Windows Runtime (**IAsyncXxx**), C++/WinRT capture le contexte d’appel au point où vous avez `co_await`. Et cela garantit que vous êtes toujours dans ce contexte lors de la reprise. C++/WinRT accomplit cela en vérifiant si vous êtes déjà sur le contexte d’appel et, si ce n’est pas le cas, en basculant vers lui. Si vous étiez sur un thread unique cloisonné (STA) avant `co_await`, vous serez sur le même par la suite. Si vous étiez sur un multithread cloisonné (MTA) avant `co_await`, vous y serez également par la suite.

```cppwinrt
IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    // The thread context at this point is captured...
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    // ...and is restored at this point.
}
```

Vous pouvez compter sur ce comportement car C++/WinRT fournit du code afin d’adapter ces types d’opérations asynchrones Windows Runtime à la prise en charge du langage de coroutine C++ (ces éléments de code sont appelés « adaptateurs await »). Les types pouvant être attendus restants en C++/WinRT sont simplement des wrappers de pool de threads et/ou des programmes d’assistance ; par conséquent, ils se terminent sur le pool de threads.

```cppwinrt
using namespace std::chrono;
IAsyncOperation<int> return_123_after_5s()
{
    // No matter what the thread context is at this point...
    co_await 5s;
    // ...we're on the thread pool at this point.
    co_return 123;
}
```

Si vous `co_await` un autre type (même dans une implémentation de coroutine C++/WinRT), une autre bibliothèque fournit les adaptateurs, et vous devrez comprendre ce que font ces adaptateurs en termes de reprise et de contextes.

Pour limiter au minimum les changements de contexte, vous pouvez appliquer certaines des techniques que nous avons déjà vues dans cette rubrique. Voyons-en quelques illustrations. Cet exemple de pseudo-code suivant présente un gestionnaire d’événements qui appelle une API Windows Runtime pour charger une image, dépose sur un thread d’arrière-plan pour traiter cette image, puis retourne sur le thread d’interface utilisateur afin d’afficher l’image dans l’interface utilisateur.

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    // Call StorageFile::OpenAsync to load an image file.

    // The call to OpenAsync occurred on a background thread, but C++/WinRT has restored us to the UI thread by this point.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

Pour ce scénario, il existe un peu d’inefficacité autour de l’appel à **StorageFile::OpenAsync**. Il y a un changement de contexte nécessaire vers un thread d’arrière-plan (afin que le gestionnaire puisse retourner l’exécution à l’appelant) au moment de la reprise, après quoi C++/WinRT restaure le contexte de thread d’interface utilisateur. Mais dans ce cas il n’est pas nécessaire de se trouver sur le thread d’interface utilisateur tant que nous ne sommes pas sur le point de mettre à jour l’interface utilisateur. Plus nous appelons d’API Windows Runtime *avant* notre appel à **winrt::resume_background**, plus nous observons de changements de contexte inutiles. La solution consiste à n’appeler *aucune* API Windows Runtime avant ce moment. Déplacez-les toutes après **winrt::resume_background**.

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Call StorageFile::OpenAsync to load an image file.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

Si vous voulez faire quelque chose de plus avancé, vous pouvez écrire vos propres adaptateurs await. Par exemple, si vous souhaitez qu’un `co_await` reprenne sur le même thread que celui sur lequel l’action asynchrone se termine (afin qu’il n’y ait aucun changement de contexte), vous pouvez commencer par écrire des adaptateurs await similaires à ceux illustrés ci-dessous.

> [!NOTE]
> L’exemple de code ci-dessous est fourni uniquement à des fins d’apprentissage ; son but est de vous aider à comprendre le fonctionnement des adaptateurs await. Si vous souhaitez appliquer cette technique dans votre propre base de code, nous vous recommandons de développer et tester vos propres structs d’adaptateurs await. Par exemple, vous pourriez écrire **complete_on_any**, **complete_on_current** et **complete_on(dispatcher)** . Vous pourriez également en faire des modèles qui prennent le type **IAsyncXxx** comme paramètre de modèle.

```cppwinrt
struct no_switch
{
    no_switch(Windows::Foundation::IAsyncAction const& async) : m_async(async)
    {
    }

    bool await_ready() const
    {
        return m_async.Status() == Windows::Foundation::AsyncStatus::Completed;
    }

    void await_suspend(std::experimental::coroutine_handle<> handle) const
    {
        m_async.Completed([handle](Windows::Foundation::IAsyncAction const& /* asyncInfo */, Windows::Foundation::AsyncStatus const& /* asyncStatus */)
        {
            handle();
        });
    }

    auto await_resume() const
    {
        return m_async.GetResults();
    }

private:
    Windows::Foundation::IAsyncAction const& m_async;
};
```

Pour comprendre comment utiliser les adaptateurs await **no_switch**, vous devez d’abord savoir que quand le compilateur C++ rencontre une expression `co_await`, il recherche des fonctions appelées **await_ready**, **await_suspend** et **await_resume**. La bibliothèque C++/WinRT fournit ces fonctions afin que vous obteniez un comportement raisonnable par défaut, comme ceci :

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

Pour utiliser les adaptateurs await **no_switch**, il vous suffit de changer le type de cette expression `co_await` de **IAsyncXxx** en **no_switch**, comme ceci :

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

Ainsi, au lieu de rechercher les trois fonctions **await_xxx** qui correspondent à **IAsyncXxx**, le compilateur C++ recherche des fonctions qui correspondent à **no_switch**.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>Annulation d’une opération asynchrone et rappels d’annulation

Les fonctionnalités du Windows Runtime pour la programmation asynchrone vous permettent d’annuler une opération ou une action asynchrone en cours. Voici un exemple qui appelle [**StorageFolder::GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync) pour récupérer une collection de fichiers potentiellement volumineuse. Il stocke l’objet d’opération asynchrone résultant dans un membre de données. L’utilisateur a la possibilité d’annuler l’opération.

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.Search.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::Storage;
using namespace Windows::Storage::Search;
using namespace Windows::UI::Xaml;
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();
    }

    IAsyncAction OnWork(IInspectable /* sender */, RoutedEventArgs /* args */)
    {
        workButton().Content(winrt::box_value(L"Working..."));

        // Enable the Pictures Library capability in the app manifest file.
        StorageFolder picturesLibrary{ KnownFolders::PicturesLibrary() };

        m_async = picturesLibrary.GetFilesAsync(CommonFileQuery::OrderByDate, 0, 1000);

        IVectorView<StorageFile> filesInFolder{ co_await m_async };

        workButton().Content(box_value(L"Done!"));

        // Process the files in some way.
    }

    void OnCancel(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        if (m_async.Status() != AsyncStatus::Completed)
        {
            m_async.Cancel();
            workButton().Content(winrt::box_value(L"Canceled"));
        }
    }

private:
    IAsyncOperation<::IVectorView<StorageFile>> m_async;
};
...
```

Pour le côté implémentation de l’annulation, commençons par un exemple simple.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

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

Si vous exécutez l’exemple ci-dessus, vous verrez qu’**ImplicitCancellationAsync** imprime un message par seconde pendant trois secondes, après quoi elle se termine automatiquement suite à son annulation. Cela fonctionne car quand elle rencontre une expression `co_await`, une coroutine vérifie si elle a été annulée. Si c’est le cas, elle est court-circuitée. Dans le cas contraire, elle est suspendue de manière normale.

L’annulation peut bien entendu se produire pendant que la coroutine est suspendue. Dans ce cas, elle ne vérifiera l’annulation qu’une fois qu’elle aura repris ou atteint un autre `co_await`. Le problème qui se pose est lié à une latence potentiellement trop grossière dans la réponse à l’annulation.

Une autre option consiste à interroger explicitement l’annulation dans votre coroutine. Mettez à jour l’exemple ci-dessus avec le code suivant. Dans ce nouvel exemple, **ExplicitCancellationAsync** récupère l’objet retourné par la fonction [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token) et l’utilise pour vérifier régulièrement si la coroutine a été annulée. Tant qu’elle n’est pas annulée, la coroutine effectue une boucle infinie ; une fois qu’elle est annulée, la boucle et la fonction quittent normalement. Le résultat est le même que dans l’exemple précédent, mais ici la sortie se produit explicitement et sous contrôle.

```cppwinrt
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

L’attente sur **winrt::get_cancellation_token** récupère un jeton d’annulation avec connaissance de l’**IAsyncAction** que la coroutine génère à votre place. Vous pouvez utiliser l’opérateur d’appel de fonction sur ce jeton pour interroger l’état d’annulation (ce qui revient à vérifier l’annulation). Si vous effectuez une opération liée aux calculs, ou une itération dans une grande collection, il s’agit d’une technique raisonnable.

### <a name="register-a-cancellation-callback"></a>Inscrire un rappel d’annulation

L’annulation du Windows Runtime ne circule pas automatiquement vers d’autres objets asynchrones. Cependant, à compter de la version 10.0.17763.0 (Windows 10 version 1809) du SDK Windows, vous pouvez inscrire un rappel d’annulation. Il s’agit d’un raccordement préemptif par lequel l’annulation peut être propagée, qui rend possible l’intégration avec les bibliothèques d’accès concurrentiel existantes.

Dans cet exemple de code suivant, **NestedCoroutineAsync** effectue le travail, mais ne comporte aucune logique d’annulation spéciale. **CancellationPropagatorAsync** est essentiellement un wrapper sur la coroutine imbriquée ; le wrapper transfère l’annulation de manière préemptive.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

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

    cancellation_token.callback([=]
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

**CancellationPropagatorAsync** inscrit une fonction lambda pour son propre rappel d’annulation, puis attend (suspend) jusqu’à ce que le travail imbriqué se termine. Quand ou si **CancellationPropagatorAsync** est annulée, elle propage l’annulation de la coroutine imbriquée. Il n’est pas nécessaire d’interroger l’annulation, et celle-ci n’est pas bloquée indéfiniment. Ce mécanisme est suffisamment flexible pour que vous puissiez l’utiliser à des fins d’interopérabilité avec une coroutine ou bibliothèque d’accès concurrentiel qui ne sait rien de C++/WinRT.

## <a name="reporting-progress"></a>Signalement de la progression

Si votre coroutine retourne [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_) ou [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_), vous pouvez récupérer l’objet retourné par la fonction [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) et l’utiliser pour signaler la progression à un gestionnaire de progression. Voici un exemple de code :

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

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
> Il n’est pas correct d’implémenter plusieurs *gestionnaires d’achèvement* pour une action ou opération asynchrone. Vous pouvez soit avoir un délégué unique pour son événement terminé, soit le `co_await`. Si vous avez les deux, le deuxième échoue. L’un ou l’autre des deux types de gestionnaires d’achèvement suivants convient, mais pas tous les deux pour le même objet asynchrone.

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

Pour plus d’informations sur les gestionnaires d’achèvement, consultez [Types délégués pour les actions et opérations asynchrones](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="fire-and-forget"></a>Déclencher et oublier

Parfois, vous avez une tâche qui peut être effectuée en même temps qu’un autre travail, et vous n’avez besoin ni d’attendre qu’elle se termine (aucun autre travail n’en dépend), ni qu’elle retourne une valeur. Dans ce cas, vous pouvez déclencher la tâche et l’oublier. Pour cela, vous pouvez écrire une coroutine dont le type de retour est [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) (plutôt que l’un des types d’opérations asynchrones Windows Runtime ou **concurrency::task**).

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

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

**winrt::fire_and_forget** est également utile en tant que type de retour de votre gestionnaire d’événements quand vous devez y effectuer des opérations asynchrones. Voici un exemple (consultez également [Références fortes et faibles en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)).

```cppwinrt
winrt::fire_and_forget MyClass::MyMediaBinder_OnBinding(MediaBinder const&, MediaBindingEventArgs args)
{
    auto lifetime{ get_strong() }; // Prevent *this* from prematurely being destructed.
    auto ensure_completion{ unique_deferral(args.GetDeferral()) }; // Take a deferral, and ensure that we complete it.

    auto file{ co_await StorageFile::GetFileFromApplicationUriAsync(Uri(L"ms-appx:///video_file.mp4")) };
    args.SetStorageFile(file);

    // The destructor of unique_deferral completes the deferral here.
}
```

Le premier argument (l’*expéditeur*) n’est pas nommé, car nous ne l’utilisons jamais. C’est la raison pour laquelle nous préférons le conserver en tant que référence. Mais notez que *args* est passé par valeur. Consultez la section [Passage de paramètres](#parameter-passing) ci-dessus.

## <a name="awaiting-a-kernel-handle"></a>En attente d’un handle de noyau

C++/WinRT fournit une classe **resume_on_signal** que vous pouvez utiliser pour interrompre l’opération jusqu’à ce qu’un événement de noyau soit signalé. Vous êtes tenu de vous assurer que le handle reste valide jusqu’au retour de votre `co_await resume_on_signal(h)`. **resume_on_signal** ne peut pas le faire à votre place, car vous avez peut-être perdu le handle avant le démarrage de **resume_on_signal**, comme dans le premier exemple.

```cppwinrt
IAsyncAction Async(HANDLE event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle is not valid here.
}
```

Le **HANDLE** entrant est valide uniquement jusqu’à ce que la fonction soit retournée. Cette fonction (qui est une coroutine) retourne au premier point d’interruption (le premier `co_await` dans le cas présent). Pendant que vous attendez **DoWorkAsync**, le contrôle est retourné à l’appelant, la trame d’appel est devenue hors de portée et vous ne savez plus si le handle sera valide lorsque votre coroutine reprendra.

Techniquement, notre coroutine reçoit ses paramètres par valeur, comme cela devrait être le cas (consultez [Passage de paramètres](#parameter-passing) ci-dessus). Mais dans le cas présent, nous devons aller plus loin afin de respecter *l’esprit* de ces recommandations (au lieu d’uniquement suivre ce qui est écrit). Nous devons transmettre une référence forte (en d’autres termes, la propriété) avec le handle. Voici comment procéder.

```cppwinrt
IAsyncAction Async(winrt::handle event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle *is* not valid here.
}
```

La transmission de [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) par valeur fournit la sémantique de la propriété, ce qui garantit que le handle de noyau reste valide tout au long de la durée de vie de la coroutine.

Voici comment vous pouvez appeler cette coroutine.

```cppwinrt
namespace
{
    winrt::handle duplicate(winrt::handle const& other, DWORD access)
    {
        winrt::handle result;
        if (other)
        {
            winrt::check_bool(::DuplicateHandle(::GetCurrentProcess(),
                other.get(), ::GetCurrentProcess(), result.put(), access, FALSE, 0));
        }
        return result;
    }

    winrt::handle make_manual_reset_event(bool initialState = false)
    {
        winrt::handle event{ ::CreateEvent(nullptr, true, initialState, nullptr) };
        winrt::check_bool(static_cast<bool>(event));
        return event;
    }
}

IAsyncAction SampleCaller()
{
    handle event{ make_manual_reset_event() };
    auto async{ Async(duplicate(event)) };

    ::SetEvent(event.get());
    event.close(); // Our handle is closed, but Async still has a valid handle.

    co_await async; // Will wake up when *event* is signaled.
}
```

## <a name="important-apis"></a>API importantes
* [concurrency::task, classe](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction, interface](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;, interface](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;, interface](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;, interface](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync, méthode](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed, classe](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>Rubriques connexes
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)
* [Types de données C++ standard et C++/WinRT](std-cpp-data-types.md)
