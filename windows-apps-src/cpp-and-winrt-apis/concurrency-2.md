---
description: Scénarios de concurrence et d’opérations asynchrones plus avancés dans C++/WinRT.
title: Concurrence et opérations asynchrones plus avancées avec C++/WinRT
ms.date: 07/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, concurrence, asynchrone, async
ms.localizationpriority: medium
ms.openlocfilehash: 4a275d5c91e03f9eb5b6348cda673d93e7132d7a
ms.sourcegitcommit: 7ece8a9a9fa75e2e92aac4ac31602237e8b7fde5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68485144"
---
# <a name="more-advanced-concurrency-and-asynchrony-with-cwinrt"></a>Concurrence et opérations asynchrones plus avancées avec C++/WinRT

Cette rubrique décrit davantage de scénarios de concurrence et d’opérations asynchrones plus avancés dans C++/WinRT.

Pour une introduction sur ce sujet, commencez par lire [Concurrence et opérations asynchrones](concurrency.md).

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
using namespace std::chrono_literals;
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

## <a name="a-deeper-dive-into-winrtresumeforeground"></a>Présentation approfondie de **WinRT::resume_foreground**

À partir [de C++/WinRT 2.0](/windows/uwp/cpp-and-winrt-apis/newsnews#news-and-changes-in-cwinrt-20), la fonction [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) s’interrompt même si elle est appelée depuis le thread du répartiteur (dans les versions précédentes, elle pouvait occasionner des interblocages dans certains scénarios car elle n'était suspendue que si elle ne se trouvait pas déjà sur le thread du répartiteur).

Le comportement actuel permet le déroulement de la pile et la mise en file d'attente, ce qui est important pour la stabilité du système, notamment dans le code des systèmes de bas niveau. La dernière liste de code de la section précédente [Programmation en tenant compte de l’affinité des threads](#programming-with-thread-affinity-in-mind) illustre l’exécution d’un calcul complexe sur un thread d’arrière-plan, puis le basculement vers le thread d’interface utilisateur approprié pour mettre à jour l’utilisateur interface (IU).

Voici la manière dont **WinRT::resume_foreground** se présente en interne.

```cppwinrt
auto resume_foreground(...) noexcept
{
    struct awaitable
    {
        bool await_ready() const
        {
            return false; // Queue without waiting.
            // return m_dispatcher.HasThreadAccess(); // The C++/WinRT 1.0 implementation.
        }
        void await_resume() const {}
        void await_suspend(coroutine_handle<> handle) const { ... }
    };
    return awaitable{ ... };
};
```

Le comportement actuel, comparé au précédent, s'apparente à la différence entre [**PostMessage**](/windows/win32/api/winuser/nf-winuser-postmessagew) et [**SendMessage**](/windows/win32/api/winuser/nf-winuser-sendmessage) dans le développement d'applications Win32. **PostMessage** met le travail en file d’attente, puis déroule la pile sans attendre la fin de ce travail. Le déroulement de la pile peut être essentiel.

Au départ, la fonction **winrt::resume_foreground** prenait uniquement en charge [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) (lié à un [**CoreWindow**](/uwp/api/windows.ui.core.corewindow)), introduit avant Windows 10. Nous avons depuis introduit un répartiteur plus flexible et plus efficace : [**DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue). Vous pouvez créer un **DispatcherQueue** à utiliser à vos propres fins. Prenons l’exemple de cette application console simple.

```cppwinrt
using namespace Windows::System;

winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    auto controller{ DispatcherQueueController::CreateOnDedicatedThread() };
    RunAsync(controller.DispatcherQueue());
    getchar();
}
```

L’exemple ci-dessus crée une file d’attente (contenue dans un contrôleur) sur un thread privé, puis transmet le contrôleur à la coroutine. La coroutine peut utiliser la file d’attente pour attendre (suspendre et reprendre) sur le thread privé. Autre utilisation courante, **DispatcherQueue** permet aussi de créer une file d’attente sur le thread d’interface utilisateur actuel pour une application de bureau ou Win32 traditionnelle.

```cppwinrt
DispatcherQueueController CreateDispatcherQueueController()
{
    DispatcherQueueOptions options
    {
        sizeof(DispatcherQueueOptions),
        DQTYPE_THREAD_CURRENT,
        DQTAT_COM_STA
    };
 
    ABI::Windows::System::IDispatcherQueueController* ptr{};
    winrt::check_hresult(CreateDispatcherQueueController(options, &ptr));
    return { ptr, take_ownership_from_abi };
}
```

Cela illustre la façon dont vous pouvez appeler et incorporer des fonctions Win32 dans vos projets C++/WinRT, en appelant simplement la fonction de style Win32 [**CreateDispatcherQueueController**](/windows/win32/api/dispatcherqueue/nf-dispatcherqueue-createdispatcherqueuecontroller) pour créer le contrôleur, puis en transférant la propriété du contrôleur de file d’attente qui en résulte à l’appelant en tant qu’objet WinRT. C’est aussi précisément la manière dont vous pouvez prendre en charge une mise en file d’attente efficace et transparente sur votre application de bureau Win32 existante de style Petzold.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    Window window;
    auto controller{ CreateDispatcherQueueController() };
    RunAsync(controller.DispatcherQueue());
    MSG message;
 
    while (GetMessage(&message, nullptr, 0, 0))
    {
        DispatchMessage(&message);
    }
}
```

Ci-dessus, la simple **principale** commence par créer une fenêtre. Vous pouvez imaginer que cela enregistre une classe de fenêtre et appelle [**CreateWindow**](/windows/win32/api/winuser/nf-winuser-createwindoww) pour créer la fenêtre de bureau de niveau supérieur. La fonction **CreateDispatcherQueueController** est ensuite appelée pour créer le contrôleur de file d’attente avant d’appeler une coroutine avec la file d’attente du répartiteur appartenant à ce contrôleur. Une pompe de messages classique est ensuite entrée à l'endroit où la reprise de la coroutine intervient naturellement sur ce thread. Lorsque c'est chose faite, vous pouvez revenir aux coroutines pour votre workflow asynchrone ou basé sur des messages au sein de votre application.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ... // Begin on the calling thread...
 
    co_await winrt::resume_foreground(queue);
 
    ... // ...resume on the dispatcher thread.
}
```

L’appel à **WinRT::resume_foreground** est toujours *mis en file d'attente*, puis déroule la pile. Vous pouvez également définir la priorité de reprise,

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await winrt::resume_foreground(queue, DispatcherQueuePriority::High);
 
    ...
}
```

de même qu'utiliser l'ordre de mise en file d’attente par défaut.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await queue;
 
    ...
}
```

Ou, dans ce cas, détecter l'arrêt de la file d’attente et sa bonne gestion.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    if (co_await queue)
    {
        ... // Resume on dispatcher thread.
    }
    else
    {
        ... // Still on calling thread.
    }
}
```

L'expression `co_await` retourne `true`, ce qui indique que la reprise aura lieu sur le thread du répartiteur. En d’autres termes, la mise en file d’attente a réussi. À l’inverse, elle retourne `false` pour indiquer que l’exécution reste sur le thread appelant car le contrôleur de file d’attente s’arrête et ne traite plus les requêtes de file d’attente.

La puissance dont vous disposez augmente considérablement lorsque vous combinez C++/WinRT avec des coroutines, notamment lorsque vous développez des applications de bureau classiques de style Petzold.

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

Le premier argument (l’*expéditeur*) n’est pas nommé, car nous ne l’utilisons jamais. C’est la raison pour laquelle nous préférons le conserver en tant que référence. Mais notez que *args* est passé par valeur. Consultez la section [Passage de paramètres](concurrency.md#parameter-passing) ci-dessus.

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

Techniquement, notre coroutine reçoit ses paramètres par valeur, comme cela devrait être le cas (consultez [Passage de paramètres](concurrency.md#parameter-passing) ci-dessus). Mais dans le cas présent, nous devons aller plus loin afin de respecter *l’esprit* de ces recommandations (au lieu d’uniquement suivre ce qui est écrit). Nous devons transmettre une référence forte (en d’autres termes, la propriété) avec le handle. Voici comment procéder.

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

## <a name="asynchronous-timeouts-made-easy"></a>Délais d’attente asynchrones simplifiés

C++/WinRT participe activement aux coroutines C++. Elles ont un effet de transformation sur l'écriture de code concurrent. Cette section décrit les cas où les détails de l’asynchronie ne sont pas importants, et où seul le résultat compte. C'est la raison pour laquelle l'implémentation C++/WinRT de l'interface de l'opération asynchrone Windows Runtime [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) dispose d'une fonction **get**, similaire à celle fournie par **std::function**.

```cppwinrt
using namespace winrt::Windows::Foundation;
int main()
{
    IAsyncAction async = ...
    async.get();
    puts("Done!");
}
```

La fonction **get** crée un blocage indéfiniment pendant que l'objet asynchrone se termine. Les objets asynchrones ont tendance à être de courte durée, ce qui vous est souvent utile.

Mais cela ne suffit pas toujours, et il vous faut parfois renoncer à attendre après un certain laps de temps. L’écriture de ce code reste possible grâce aux blocs de construction fournis par Windows Runtime. Désormais, C++/WinRT facilite considérablement cette opération en mettant à disposition la fonction **wait_for**. Son implémentation porte aussi sur **IAsyncAction** et là encore, il est similaire à celui fourni par **std::function**.

```cppwinrt
using namespace std::chrono_literals;
int main()
{
    IAsyncAction async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        puts("done");
    }
}
```

Dans cet exemple, la fonction **wait_for** attend environ cinq secondes, puis vérifie si l'opération est terminée. Une comparaison positive vous indique que l'objet asynchrone s'est correctement terminé et que vous en avez fini. Si vous attendez un résultat, vous pouvez simplement le suivre à l'aide d'un appel de la fonction **get** pour récupérer ce résultat.

```cppwinrt
int main()
{
    IAsyncOperation<int> async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        printf("result %d\n", async.get());
    }
}
```

L'objet asynchrone étant alors terminé, la fonction **get** renvoie immédiatement le résultat, sans attendre davantage. Comme vous pouvez le constater, la fonction **wait_for** retourne l’état de l’objet asynchrone. Vous pouvez ainsi l'utiliser pour un contrôle plus précis, comme celui-ci.

```cppwinrt
switch (async.wait_for(5s))
{
case AsyncStatus::Completed:
    printf("result %d\n", async.get());
    break;
case AsyncStatus::Canceled:
    puts("canceled");
    break;
case AsyncStatus::Error:
    puts("failed");
    break;
case AsyncStatus::Started:
    puts("still running");
    break;
}
```

- N’oubliez pas que **AsyncStatus::Completed** signifie que l’objet asynchrone s’est correctement terminé et que vous pouvez appeler la fonction **get** pour récupérer les éventuels résultats.
- **AsyncStatus::Canceled** signifie que l’objet asynchrone a été annulé. Normalement, une annulation est demandée par l’appelant et il est donc rare de gérer cet état. En règle générale, un objet asynchrone annulé est tout simplement ignoré.
- **AsyncStatus::Error** signifie que l’objet asynchrone a échoué. Vous pouvez **demander** à lever à nouveau l’exception, si vous le souhaitez.
- **AsyncStatus::Started** signifie que l’objet asynchrone est toujours en cours d’exécution. Le modèle Windows Runtime asynchrone n’autorise ni les attentes multiples, ni les objets waiter. Dès lors, vous ne pouvez pas appeler **wait_for** dans une boucle. En cas de dépassement du délai d'attente, plusieurs options s'offrent à vous. Vous pouvez abandonner l’objet ou interroger son état avant d'appeler **get** pour récupérer les éventuels résultats. Mais à ce stade, il est préférable d'ignorer l’objet.

## <a name="important-apis"></a>API importantes
* [IAsyncAction, interface](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;, interface](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;, interface](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;, interface](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync, méthode](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::resume_foreground](/uwp/cpp-ref-for-winrt/resume-foreground)

## <a name="related-topics"></a>Rubriques connexes
* [Concurrence et opérations asynchrones](concurrency.md)
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)