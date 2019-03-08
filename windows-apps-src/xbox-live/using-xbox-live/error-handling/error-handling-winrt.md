---
title: Gestion des erreurs d'API WinRT
description: Découvrez comment gérer les erreurs lors de l’établissement d’un appel de service Xbox Live et les APIs WinRT.
ms.assetid: b4f04798-df91-430e-90f0-83b5a48b9ab2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, la gestion des erreurs
ms.localizationpriority: medium
ms.openlocfilehash: e72dfa0b6f98284c240cf6af2dde02439d694b48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598634"
---
# <a name="winrt-api-error-handling"></a>Gestion des erreurs d'API WinRT

Dans l’API WinRT, qui peut être appelé à partir de C++ / c++ / CX, C#, ou Javascript, les erreurs sont signalées à l’aide des exceptions, afin de blocs try/catch doivent être utilisés pour gérer les erreurs.

Notez que le pour les appels asynchrones, deux blocs try/catch sont nécessaires comme suit :

```cpp
    try
    {
        auto asyncOp = xboxLiveContext->TitleStorageService->UploadBlobAsync(
            blobMetadata,
            blobBuffer,
            TitleStorageETagMatchCondition::NotUsed,
            DEFAULT_UPLOAD_BLOCK_SIZE
            );

        create_task(asyncOp)
        .then([this, ui]( task<TitleStorageBlobMetadata^> t )
        {
            try
            {
                TitleStorageBlobMetadata^ blobMetadata = t.get();

                ui->Log(L"UploadBlobAsync succeeded");
                PrintBlobMetadata(ui, blobMetadata);

                SaveNewETag(blobMetadata->ETag);
            }
            catch (Platform::Exception^ ex)
            {
                // This could happen if there's a network error or the service returns an error
                ui->Log("The async task of UploadBlobAsync failed with", ex->ToString());
            }
        });
    }
    catch (Platform::Exception^ ex)
    {
        // This could happen if there's invalid args sent to the API
        ui->Log("The API call to UploadBlobAsync failed with", ex->ToString());
    }
```

Les méthodes asynchrones XSAPI n’ont pas du code qui s’exécute de façon synchrone quand une méthode est appelée.  Le code synchrone ne fonctionne pas comme validation d’arguments d’entrée et de démarrer les opérations asynchrones ou les actions.  Par conséquent, même l’appel de méthodes async peut entraîner une exception, bien que cela ne devrait pas se produire pour les scénarios normales (erreur du programmeur, pas l’erreur réseau).  Veuillez être conscients de cette possibilité et le programme en prévention par la validation de l’entrée et de tester votre code de manière approfondie pendant le développement.  Si vous placez un try/catch autour de l’appel lui-même, ou que vous placez un à un niveau supérieur dans la pile des appels, l’important est d’avoir une.

## <a name="help-my-game-crashes-when-i-call-xyz-xbox-service-api"></a>Aide ! Mon jeu se bloque lorsque j’appelle l’API de Service XYZ Xbox

Par conséquent, votre jeu se bloque et la pile des appels dit qu’il est un appel d’API de Service Xbox, mais le fait ?

```cpp
msvcr110.dll!Concurrency::details::_ReportUnobservedException() Line 2455    C++
Social110Release.exe!Concurrency::details::_ExceptionHolder::~_ExceptionHolder() Line 915    C++
Social110Release.exe!Concurrency::details::_Task_impl_base::~_Task_impl_base() Line 1488    C++
Social110Release.exe!Concurrency::details::_Task_impl<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::`scalar deleting destructor'(unsigned int)    C++
Social110Release.exe!Concurrency::task<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::_ContinuationTaskHandle<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64,void,<lambda_8571b6148830c0805feee6ba9e76a692>,std::integral_constant<bool,1>,Concurrency::details::_TypeSelectorNoAsync>::`scalar deleting destructor'(unsigned int)    C++
msvcr110.dll!Concurrency::details::_TaskCollection::_NotifyCompletedChoreAndFree(Concurrency::details::_UnrealizedChore * pChore) Line 1171    C++
msvcr110.dll!Concurrency::details::_UnrealizedChore::_UnstructuredChoreWrapper(Concurrency::details::_UnrealizedChore * pChore) Line 373    C++
msvcr110.dll!Concurrency::details::InternalContextBase::Dispatch(Concurrency::DispatchState * pDispatchState) Line 1716    C++
msvcr110.dll!Concurrency::details::FreeThreadProxy::Dispatch() Line 203    C++
msvcr110.dll!Concurrency::details::ThreadProxy::ThreadProxyMain(void * lpParameter) Line 174    C++
ntdll.dll!RtlUserThreadStart(long (void *) * StartAddress, void * Argument) Line 822    C++
```

Ces piles d’appels sont très déroutants.  Accès concurrentiel, l’accès concurrentiel qui...  Oh Microsoft::Xbox::Services::Social::XboxUserProfile est dans la pile des appels, qui doit donc être la cause du problème.  En réalité, il s’agit d’une pile des appels généré par un appel à GetUserProfileAsync pour un ID d’utilisateur Xbox non valide.  L’exemple de code qui a généré cette pile des appels ressemble à ceci :

```cpp
    auto pAsyncOp = requester->ProfileService->GetUserProfileAsync("abc123"); //passing invalid Xbox User Id;

    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)
    {
        // Oops, I forgot my exception handling code here.
        // If I don't call resultTask.get() and catch any potential exception it may throw,
        // then PPL will report an unobserved exception.  That unobserved exception will cause your
        // app to crash.
    });
```

Votre pile des appels contient-elle Concurrency::_ReportUnobservedException() ?  Examinez à la pile des appels ci-dessus.  C'est important.  Si votre pile des appels contient d’accès concurrentiel :: _ReportUnobservedException() vous avez trouvé un bogue dans le code du jeu.

Bibliothèque de modèles parallèles crée des tâches, qui peuvent être suivies d’autres tâches.  Dans l’exemple ci-dessus, create_task() génère la tâche pour appeler GetUserProfileAsync(), et le .then() crée la tâche suivante.  Souvent appelés la tâche antécédente (tout d’abord un) et la continuation (deuxième).  Dans l’exemple, la continuation est manquante à la gestion des erreurs.  Le runtime met fin à l’application si une tâche lève une exception et que cette exception n’est pas interceptée par la tâche ou l’un de ses continuations.

Lorsqu’il s’agit des tâches de continuation, notez qu’il n’y a deux types différents.  Un seul type, la continuation basé sur des tâches, prend la tâche précédente comme l’argument d’entrée.  Cette tâche s’exécute toujours, même si l’antécédent de tâche lève une exception.  Pour obtenir le résultat de la tâche antécédente, vous devez appeler .get() sur l’argument.  La seconde, basé sur la valeur, reçoit la sortie de la tâche précédente directement – il s’agit vraiment syntaxique, à l’exception d’abord – les continuations en fonction de valeur ne sont pas totalement l’exécution si l’antécédent lève une exception.

Recommandation :  Pour éviter les blocages, utilisez une continuation basée sur une tâche à la fin de votre chaîne de continuation et de blocs pour gérer les erreurs qui peuvent être récupérées à partir de tous les appels d’accès concurrentiel :: task::get() ou d’accès concurrentiel :: task::wait() dans try/catch à effet surround.

Nous allons examiner quelques exemples :

Exemple de continuation basée sur des tâches

```cpp
    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)   // Task-based continuation
    {
        try
        {
            XboxUserProfile^ result = resultTask.get();

            // success, do something
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
        }
    });
```

Exemple de continuation basée sur une valeur

```cpp
    create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
        // if the task didn't complete successfully, you'd better have a task-based
        // continuation at the end of the continuation chain or the app will crash.
    })
    .then( [this] (task<void> previousTask) // Task-based continuation
    {
        try
        {
            // DO NOT IGNORE THIS THINKING IT'S NOT IMPORTANT.

            // call concurrency::task::get and handle any unobserved exception
            // so the application doesn't crash.
            previousTask.get();

            // success, continue
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
            // By handling this exception, you ensure your application will not
            // crash when calling Xbox Service APIs
        }
    });
```

Il existe une troisième solution : utilisez les continuations basées sur une valeur complètement, mais appeler .get() ou .wait() sur un autre thread et intercepter l’exception il.  Voici un exemple simple :

```cpp
    auto getProfileTask = create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
    });
    // Note the lack of a task-based continuation with error handling at the end

    // You may call .get() or .wait() on a value-based only chain, but
    // must ensure you surround the call in a try/catch block and handle errors
    try
    {
        getProfileTask.get();     // or getProfileTask.wait();
    }
    catch (Platform::Exception^ ex)
    {
        // concurrency::task::get threw an exception
        // safely handle the error here
        // By handling this exception, you ensure your application will not
        // crash when calling Xbox Service APIs
    }
```

**Si vous n’utilisez pas la bibliothèque de modèles parallèles** si vous utilisez AsyncOperationCompletionHandler ou AsyncActionCompletionHandler au lieu de la bibliothèque de modèles parallèles, vous avez également la gestion des erreurs qui si effectuée de manière incorrecte, entraîneront des blocages d’application.  Voici un exemple montrant comment gérer les erreurs

```cpp
    try
    {
        // Example is making a service call with an invalid XboxUserId which will result in an error.
        // The completion handler properly detects the error and does not crash the app.
        requester->ProfileService->GetUserProfileAsync("abc123")->Completed
            = ref new AsyncOperationCompletedHandler<XboxUserProfile^>([=] (IAsyncOperation<XboxUserProfile^>^ operation, Windows::Foundation::AsyncStatus status)
        {
            if( status == Windows::Foundation::AsyncStatus::Completed)
            {
                // Always check the AsyncStatus before calling GetResults().
                // If status is not AsyncStatus::Completed, calls to operation->GetResults()
                // may throw an exception.
                // You can also surround this call in a try/catch block for added safety.

                XboxUserProfile^ result = operation->GetResults();

                // success, do something with the result
            }
            else if( status == Windows::Foundation::AsyncStatus::Error )
            {
                // Failed
            }
        });
    }
    catch ( Platform::COMException^ ex )
    {
        // What is this try/catch block for?
        //
        // Xbox Service APIs do have some code that runs synchronously and errors need
        // to be safely handled.  In this example, if “” was passed instead of “abc123”,
        // then an invalid argument exception would be thrown when calling GetUserProfileAsync
    // See the next section for more a more detailed explanation.
        //
        // Note: this catch block will NOT catch exceptions thrown within the completion handler.
    }
```

**Exceptions et GetNextAsync()** à l’aide de l’API de pagination ?  Tous les objets AchievementResult, LeaderboardResult, InventoryItemResult et TitleStorageBlobMetadataResult contiennent une méthode GetNextAsync() pour demander la page suivante de résultats.  Il existe un cas particulier, lorsque aucune donnée n’est disponible, qui déclenche une exception lors de l’appel GetNextAsync().  Cette exception est levée pendant l’exécution synchrone de GetNextAsync().  Dans ce cas, la méthode GetNextAsync lève INET_E_DATA_NOT_AVAILABLE (0x800C0007).

Recommandation : Vérifiez que vous encapsulez GetNextAsync() méthode appelle dans un bloc try/catch et gérer correctement le cas INET_E_DATA_NOT_AVAILABLE.

```cpp
    try
    {
        // AchievementResult^ LastResult

        // Get next page of achievement results
        if(LastResult != nullptr)
        {
            auto getNextPage = LastResult->GetNextAsync(10);

            // create_task( getNextPage ) ...
        }
    }
    catch (Platform::Exception^ ex)
    {
        if (ex->HResult == INET_E_DATA_NOT_AVAILABLE)
        {
                // we hit the end of the achievements
        }
        else
        {
            // failed for unexpected reason
        }
    }
```
