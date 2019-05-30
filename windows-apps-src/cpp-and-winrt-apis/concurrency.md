---
description: Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec C++/WinRT.
title: Opérations concurrentes et asynchrones avec C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, concurrence, asynchrone, async
ms.localizationpriority: medium
ms.openlocfilehash: 5dba97ede63b1bcb85c4ee1807d5558f4c93834a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361209"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Opérations concurrentes et asynchrones avec C++/WinRT

Cette rubrique montre comment dans lequel vous pouvez crée et consommer des objets asynchrones Windows Runtime avec [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Opérations asynchrones et fonctions « Async » Windows Runtime

Les API Windows Runtime dont l’exécution est susceptible de prendre plus de 50 millisecondes sont implémentées en tant que fonctions asynchrones (avec un nom se terminant par « Async »). L’implémentation d’une fonction asynchrone lance le travail sur un autre thread et renvoie immédiatement un objet qui représente l’opération asynchrone. À la fin de l’opération asynchrone, l’objet renvoyé contient n’importe quelle valeur qui résulte du travail. L’espace de noms Windows Runtime **Windows::Foundation** contient quatre types d’objet d’opération asynchrone.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_), et
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Chacun de ces types d’opération asynchrone est projeté dans un type correspondant dans l’espace de noms C++/WinRT **winrt::Windows::Foundation**. C++/WinRT contient également une structure adaptateur d’attente interne. Vous ne l’utilisez directement, mais grâce à cette structure, vous pouvez écrire un `co_await` instruction de manière coopérative attendre le résultat de n’importe quelle fonction qui retourne une de ces types d’opération asynchrone. Et vous pouvez créer vos propres coroutines qui renvoient ces types.

Un exemple de fonction Windows asynchrone est [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), qui retourne un objet d’opération asynchrone de type [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Nous allons examiner quelques méthodes&mdash;premier puis non-blocage et&mdash;de l’utilisation de C++ / c++ / WinRT pour appeler une API telle que celle.

## <a name="block-the-calling-thread"></a>Bloquer le thread appelant

L’exemple de code ci-dessous reçoit un objet d’opération asynchrone à partir de **RetrieveFeedAsync** et appelle **get** sur cet objet pour bloquer le thread appelant jusqu'à ce que les résultats de l’opération asynchrone soient disponibles.

Si vous souhaitez copier-coller cet exemple directement dans le fichier de code source principal d’un **Application de Console Windows (C++/WinRT)** projet, puis premier jeu **pas utiliser les en-têtes précompilés** dans le projet Propriétés.

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

Appeler **get** rend le codage pratique et est idéal pour les applications de console ou les threads d’arrière-plan dans lesquels vous ne souhaitez pas utiliser de coroutine pour une raison quelconque. Mais ce n’est ni simultané ni asynchrone, donc ce n’est pas approprié pour un thread d’interface utilisateur (et une assertion se déclenchera dans les versions non optimisées, si vous tentez de l'utiliser sur une). Pour éviter d’empêcher les threads du système d’exploitation d’effectuer d’autres tâches utiles, nous avons besoin d’une autre technique.

## <a name="write-a-coroutine"></a>Écrire une coroutine

> [!IMPORTANT]
> En tant que de [ C++WinRT 2.0](news.md#news-and-changes-in-cwinrt-20), veillez à `#include <winrt/coroutine.h>` chaque fois que vous créez ou consommer une coroutine.

C++/WinRT intègre des coroutines C++ dans le modèle de programmation pour fournir un moyen naturel d’attendre de manière coopérative un résultat. Vous pouvez générer votre propre opération asynchrone Windows Runtime en écrivant une coroutine. Dans l’exemple de code ci-dessous, **ProcessFeedAsync** est la coroutine.

> [!NOTE]
> Le **obtenir** fonction existe sur le C++ / c++ / type de projection WinRT **winrt::Windows::Foundation::IAsyncAction**, de sorte que vous pouvez appeler la fonction depuis n’importe quel C + c++ / WinRT projet. Vous ne trouverez pas de la fonction répertoriée en tant que membre de la [ **IAsyncAction** ](/uwp/api/windows.foundation.iasyncaction) interface, car **obtenir** ne fait pas partie de la surface de (ABI) de l’interface binaire d’application de la type réel de Windows Runtime **IAsyncAction**.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/coroutine.h>
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

Une coroutine est une fonction qui peut être suspendue et reprise. Dans la coroutine **ProcessFeedAsync** ci-dessus, lorsque l’instruction `co_await` est atteinte, la coroutine lance de façon asynchrone l’appel **RetrieveFeedAsync**, puis se suspend immédiatement et retourne le contrôle à l’appelant (à savoir **main** dans l’exemple ci-dessus). **main** peut alors continuer sa tâche pendant que le flux est récupéré et imprimé. Lorsque l’opération est effectuée (lorsque l’appel **RetrieveFeedAsync** se termine), la coroutine **ProcessFeedAsync** reprend à l’instruction suivante.

Vous pouvez agréger une coroutine dans d’autres coroutines. Ou vous pouvez appeler **get** pour la bloquer et attendre qu’elle se termine (et obtenir le résultat, le cas échéant). Ou vous pouvez la transmettre à un autre langage de programmation qui prend en charge Windows Runtime.

Il est également possible de gérer les événements terminés et/ou en cours des actions et des opérations asynchrones à l’aide de délégués. Pour plus d’informations et des exemples de code, voir [Types délégués pour les actions et opérations asynchrones](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="asychronously-return-a-windows-runtime-type"></a>Retourner de façon asynchrone un type Windows Runtime

Dans l’exemple suivant, nous allons encapsuler un appel à **RetrieveFeedAsync**, pour un URI spécifique, afin d’obtenir une fonction **RetrieveBlogFeedAsync** qui renvoie de façon asynchrone un [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed).

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/coroutine.h>
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

Dans l’exemple ci-dessus, **RetrieveBlogFeedAsync** renvoie un **IAsyncOperationWithProgress**, qui possède une progression et une valeur de retour. Nous pouvons effectuer d’autres tâches pendant que **RetrieveBlogFeedAsync** effectue le traitement et récupère le flux. Ensuite, nous allons appeler **get** sur cet objet d’opération asynchrone à bloquer, attendre qu’il se termine et obtenir les résultats de l’opération.

Si vous renvoyez de façon asynchrone un type Windows Runtime, vous devez renvoyer un [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) ou un [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). N'importe quelle classe runtime interne ou tierce est appropriée ou n’importe quel type qui peut être transmis vers ou à partir d’une fonction Windows Runtime (par exemple, `int` ou **winrt::hstring**). Le compilateur vous aidera en affichant une erreur « *must be WinRT type* » (doit être de type WinRT) si vous essayez d’utiliser un de ces types d’opération asynchrone avec un type non Windows Runtime.

Si une coroutine ne possède pas au moins une instruction `co_await`, pour être appropriée en tant que coroutine, elle doit avoir au moins une instruction `co_return` ou `co_yield`. Il y aura des cas où votre coroutine peut retourner une valeur sans présenter de comportement asynchrone et donc sans blocage ni changement de contexte. Voici un exemple qui le fait (au deuxième appel et aux suivants) en mettant une valeur en cache.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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
...
#include <winrt/coroutine.h>
...
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

Une coroutine est une fonction comme n’importe quel autre car l’appelant est bloqué jusqu'à ce qu’une fonction retourne l’exécution à ce dernier. Et, la première occasion pour une coroutine à retourner est le premier `co_await`, `co_return`, ou `co_yield`.

Par conséquent, avant de procéder travail liée aux calculs dans une coroutine, vous devez renvoyer l’exécution à l’appelant (en d’autres termes, introduire un point de suspension) afin que l’appelant n’est pas bloqué. Si vous n'êtes pas déjà cela `co_await`- ing une autre opération, vous pouvez `co_await` le [ **winrt::resume_background** ](/uwp/cpp-ref-for-winrt/resume-background) (fonction). Cela retourne le contrôle à l’appelant, puis reprend immédiatement l'exécution sur un thread de pool de threads.

Le pool de threads utilisé dans l’implémentation est le [pool de threads Windows](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api) de bas niveau. Son efficacité est donc optimale.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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
...
#include <winrt/coroutine.h>
...
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

Le code ci-dessus lève une exception [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread), car un **TextBlock** doit être mis à jour à partir du thread qui l’a créé, à savoir le thread d’interface utilisateur. Une solution consiste à capturer le contexte du thread dans lequel notre coroutine a été appelée à l’origine. Pour ce faire, vous devez instancier un [ **winrt::apartment_context** ](/uwp/cpp-ref-for-winrt/apartment-context) d’objet, le travail, en arrière-plan, puis `co_await` le **apartment_context** pour revenir à l’appel contexte.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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

Pour une solution plus générale pour la mise à jour d’interface utilisateur, qui couvre les cas où vous avez des doutes sur le thread appelant, vous pouvez `co_await` le [ **winrt::resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground) pour basculer vers un spécifique (fonction) thread de premier plan. Dans l’exemple de code ci-dessous, nous spécifions le thread de premier plan en passant l’objet répartiteur associé au **TextBlock** (en accédant à sa propriété [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). L’implémentation de **winrt::resume_foreground** appelle [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) sur cet objet répartiteur afin d'exécuter la tâche qui apparaît dans la coroutine.

```cppwinrt
...
#include <winrt/coroutine.h>
...
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>Contextes d’exécution, la reprise et le basculement dans une coroutine

Une manière générale, après un point de sélection dans une coroutine, le thread d’origine de l’exécution peut disparaître et reprise peut se produire sur n’importe quel thread (en d’autres termes, n’importe quel thread peut appeler le **terminé** méthode pour l’opération asynchrone).

Mais si vous `co_await` un des quatre types d’opération asynchrone Windows Runtime (**IAsyncXxx**), puis sur C et c++ / WinRT capture le contexte d’appel au point vous `co_await`. Et elle garantit que vous êtes toujours dans ce contexte lorsque la continuation reprend. C++ / c++ / WinRT pour ce faire de vérification si vous êtes déjà sur le contexte d’appel et, dans le cas contraire, basculant vers celle-ci. Si vous étiez sur un thread cloisonné monothread (STA) avant `co_await`, puis vous serez sur le même que celui par la suite ; si vous étiez sur un thread de cloisonnement multithread (MTA) avant `co_await`, vous pouvez ensuite sur l’un par la suite.

```cppwinrt
...
#include <winrt/coroutine.h>
...
IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    // The thread context at this point is captured...
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    // ...and is restored at this point.
}
```

Vous pouvez compter sur ce comportement est parce que C++ / c++ / WinRT fournit du code pour adapter ces types d’opération asynchrone Windows Runtime pour le C++ coroutine prise en charge linguistique (ces éléments de code sont appelées cartes d’attente). Les types pouvant être attendus restants en C / c++ / WinRT sont simplement des wrappers de pool de thread et/ou les programmes d’assistance ; Par conséquent, elles se terminent sur le pool de threads.

```cppwinrt
...
#include <winrt/coroutine.h>
...
using namespace std::chrono;
IAsyncOperation<int> return_123_after_5s()
{
    // No matter what the thread context is at this point...
    co_await 5s;
    // ...we're on the thread pool at this point.
    co_return 123;
}
```

Si vous `co_await` un autre type&mdash;même au sein de C++ / c++ / mise en œuvre de la coroutine WinRT&mdash;ensuite une autre bibliothèque fournit les cartes, et vous devez comprendre ce que font ces cartes en termes de reprise et de contextes.

Pour conserver les changements de contexte jusqu’au minimum, vous pouvez utiliser les techniques que nous avons déjà vu dans cette rubrique. Nous allons voir quelques illustrations de cette méthode. Dans cet exemple de pseudo-code suivant, nous montrons le contour d’un gestionnaire d’événements qui appelle une API de Runtime Windows pour charger une image, dépose sur un thread d’arrière-plan pour traiter cette image, puis retourne le thread d’interface utilisateur pour afficher l’image dans l’interface utilisateur.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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

Pour ce scénario, il existe un peu d’ineffiency autour de l’appel à **StorageFile::OpenAsync**. Il existe un changement de contexte nécessaire pour un arrière-plan de thread (afin que le gestionnaire peut retourner l’exécution à l’appelant), sur la reprise après les C + c++ / WinRT restaure le contexte de thread d’interface utilisateur. Mais, dans ce cas, il n’est pas nécessaire de se trouver sur le thread d’interface utilisateur jusqu'à ce que nous sommes sur le point de mise à jour de l’interface utilisateur. Les plus Windows Runtime APIs nous appelons *avant* notre appel à **winrt::resume_background**, les commutateurs de contexte serveur-et-les deux sens plus inutiles nous engager. La solution doit ne pas appeler *n’importe quel* Windows APIs Runtime avant cette date. Déplacez-les tout après le **winrt::resume_background**.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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

Si vous voulez faire quelque chose plus avancé, puis vous pouvez écrire votre propre await adaptateurs. Par exemple, si vous souhaitez un `co_await` à reprendre sur le même thread que celui qui l’action asynchrone se termine sur (par conséquent, il n’existe aucun changement de contexte), puis vous pouvez commencer en écrivant await adaptateurs similaires à ceux indiqués ci-dessous.

> [!NOTE]
> L’exemple de code ci-dessous est fourni à titre éducatif uniquement ; Il est de vous aider à démarrer compréhension await comment les adaptateurs. Si vous souhaitez utiliser cette technique dans votre propre code de base, nous vous recommandons de développerez et testez votre propre await struct(s) de l’adaptateur. Par exemple, vous pouvez écrire **complete_on_any**, **complete_on_current**, et **complete_on(dispatcher)** . Aussi envisager d’apporter les modèles qui prennent le **IAsyncXxx** type comme paramètre de modèle.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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

Pour comprendre comment utiliser le **valeur_log** await des adaptateurs, vous devez d’abord savoir que quand le C++ compilateur rencontre une `co_await` expression apparemment pour les fonctions appelées **await_ready**, **await_suspend**, et **await_resume**. C++ / c++ / WinRT bibliothèque fournit ces fonctions afin que vous obtenez raisonnable comportement par défaut, comme suit.

```cppwinrt
...
#include <winrt/coroutine.h>
...
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

Pour utiliser le **valeur_log** await adaptateurs, modifiez le type de ce `co_await` expression à partir de **IAsyncXxx** à **valeur_log**, comme suit.

```cppwinrt
...
#include <winrt/coroutine.h>
...
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

Ensuite, au lieu de rechercher les trois **await_xxx** fonctions qui correspondent à **IAsyncXxx**, le C++ compilateur recherche pour les fonctions qui correspondent aux **valeur_log**.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>Annulation d’une opération asynchrone et les rappels d’annulation

Fonctionnalités du Runtime Windows pour la programmation asynchrone permettent d’annuler une action asynchrone en cours ou l’opération. Voici un exemple qui appelle [ **StorageFolder::GetFilesAsync** ](/uwp/api/windows.storage.storagefolder.getfilesasync) pour récupérer une collection potentiellement volumineuse de fichiers et elle stocke l’objet d’une opération asynchrone résultant dans un membre de données. L’utilisateur a la possibilité d’annuler l’opération.

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/coroutine.h>
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

Pour le côté de l’implémentation de l’annulation, commençons par un exemple simple.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/coroutine.h>
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

Si vous exécutez l’exemple ci-dessus, vous verrez **ImplicitCancellationAsync** imprimer un message par seconde pendant trois secondes, après lequel temps qu’elle termine automatiquement ainsi d’être annulée. Cela fonctionne car, en rencontre une `co_await` expression, une coroutine vérifie si elle a été annulée. S’il possède, puis elle court-circuite et si elle n’est pas le cas, puis il suspend comme d’habitude.

L’annulation peut, bien sûr, se produire pendant la suspension de la coroutine. Uniquement lors de la coroutine reprend, ou un autre atteint `co_await`, il vérifie l’annulation. Le problème est un des potentiellement trop grossier-plus précis de latence dans la réponse à l’annulation.

Par conséquent, une autre option consiste à interroger explicitement l’annulation dans votre coroutine. Mettre à jour l’exemple ci-dessus par le code dans la liste ci-dessous. Dans cet exemple de nouveau, **ExplicitCancellationAsync** récupère l’objet retourné par la [ **winrt::get_cancellation_token** ](/uwp/cpp-ref-for-winrt/get-cancellation-token) de fonction et l’utilise pour régulièrement Vérifiez si la coroutine a été annulée. Tant qu’il n’est pas annulé, la coroutine effectue une itération indéfiniment ; une fois qu’elle est annulée, la boucle et la fonction de sortie normalement. Le résultat est le même que l’exemple précédent, mais la sortie ici se produit explicitement et sous contrôle.

```cppwinrt
...
#include <winrt/coroutine.h>
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

En attente sur **winrt::get_cancellation_token** récupère un jeton d’annulation qui connaissent bien les **IAsyncAction** qui génère la coroutine à votre place. Vous pouvez utiliser l’opérateur d’appel de fonction sur ce jeton pour interroger l’état d’annulation&mdash;essentiellement d’interrogation de l’annulation. Si vous effectuez une opération liée aux calculs, ou une itération dans une grande collection, puis il s’agit d’une technique raisonnable.

### <a name="register-a-cancellation-callback"></a>Inscrire un rappel d’annulation

L’annulation du Runtime Windows ne passe pas automatiquement à d’autres objets asynchrones. Mais&mdash;introduites dans la version 10.0.17763.0 (Windows 10, version 1809) du SDK Windows&mdash;vous pouvez inscrire un rappel d’annulation. Il s’agit d’un raccordement préemptif par lequel l’annulation peut être propagée et rend possible l’intégration avec les bibliothèques d’accès concurrentiel existantes.

Dans cet exemple de code suivant, **NestedCoroutineAsync** effectue le travail, mais n’a aucune logique d’annulation spéciaux qu’il contient. **CancellationPropagatorAsync** est essentiellement un wrapper sur la coroutine imbriquée ; le wrapper transfère l’annulation préalablement.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/coroutine.h>
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

**CancellationPropagatorAsync** enregistre une fonction lambda pour son propre rappel d’annulation, puis attend (il suspend) jusqu'à ce que le travail imbriqué se termine. Quand ou si **CancellationPropagatorAsync** est annulée, elle propage l’annulation de la coroutine imbriquée. Il n’est pas nécessaire pour l’interrogation de l’annulation ; ni l’annulation est bloquée indéfiniment. Ce mécanisme est suffisamment flexible pour pouvoir l’utiliser pour assurer l’interopérabilité avec une bibliothèque coroutine ou d’accès concurrentiel qui ne sait rien du C++ / c++ / WinRT.

## <a name="reporting-progress"></a>Signaler la progression

Si votre coroutine retourne soit [ **IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_), ou [ **IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_), vous pouvez récupérer le objet retourné par la [ **winrt::get_progress_token** ](/uwp/cpp-ref-for-winrt/get-progress-token) de fonction et l’utiliser pour signaler la progression à un gestionnaire de progression. Voici un exemple de code.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/coroutine.h>
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
> Il n’est pas correct implémenter plusieurs *Gestionnaire d’achèvement* pour une action ou opération asynchrone. Vous pouvez avoir soit un délégué unique pour son événement terminé, ou vous pouvez `co_await` il. Si vous avez les deux, la seconde échoue. Soit un des deux types suivants de gestionnaires d’achèvement est approprié ; pas à la fois pour le même objet async.

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

Pour plus d’informations sur les gestionnaires de saisie semi-automatique, consultez [déléguer des types pour les actions et opérations asynchrones](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="fire-and-forget"></a>Déclenchement et oubli

Parfois, vous avez une tâche qui peut être effectuée en même temps que d’autres tâches, et vous n’avez pas besoin d’attendre l’exécution de cette tâche (aucun autre travail ne dépend), ni vous en avez besoin pour retourner une valeur. Dans ce cas, vous pouvez déclencher la tâche et l’oublier. Vous pouvez le faire en écrivant une coroutine dont le type de retour est [ **winrt::fire_and_forget** ](/uwp/cpp-ref-for-winrt/fire-and-forget) (au lieu d’un des types d’opération asynchrone Windows Runtime, ou **concurrency::task**).

```cppwinrt
// main.cpp
#include <winrt/coroutine.h>
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

## <a name="important-apis"></a>API importantes
* [classe Concurrency::Task](/cpp/parallel/concrt/reference/task-class)
* [Interface de IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; interface](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; interface](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; interface](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync (méthode)](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Classe de SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>Rubriques connexes
* [Gérer les événements à l’aide de délégués en C / c++ / WinRT](handle-events.md)
* [Types de données C++ standard et C++/WinRT](std-cpp-data-types.md)
