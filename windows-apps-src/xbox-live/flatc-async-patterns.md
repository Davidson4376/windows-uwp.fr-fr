---
title: Modèles d’appel asynchrones API C
description: Découvrez l’API C asynchrone appelant des modèles pour l’API C XSAPI
ms.date: 06/10/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, programme pour développeurs
ms.localizationpriority: medium
ms.openlocfilehash: edc6248a363b844d94c8fa03ab7ce071cc941908
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608004"
---
# <a name="calling-pattern-for-xsapi-flat-c-layer-async-calls"></a>Modèle d’appel pour XSAPI plat les appels asynchrones couche C

Un **API asynchrone** est une API qui retourne rapidement, mais démarre un **tâche asynchrone** et le résultat est retourné une fois que la tâche est terminée.

Jeux présentent généralement peu de contrôle sur le thread qui exécute le **tâche asynchrone** et le thread qui retourne les résultats lorsque vous utilisez un **rappel d’achèvement**.  Certains jeux est conçus afin qu’une section du tas est uniquement couvertes par un seul thread afin d’éviter toute nécessité de synchronisation de thread. Si le **rappel d’achèvement** n’est pas appelée à partir d’un thread, le jeu des contrôles, la mise à jour d’état partagé avec le résultat d’un **tâche asynchrone** nécessitera la synchronisation de threads.

L’API C XSAPI expose une API C asynchrone qui offre aux développeurs contrôle direct de thread lors de l’établissement une **API asynchrone** appeler, telles que **XblSocialGetSocialRelationshipsAsync()**,  **XblProfileGetUserProfileAsync()** et **XblAchievementsGetAchievementsForTitleIdAsync()**.

Voici un exemple de base appelant le **XblProfileGetUserProfileAsync** API.

```cpp
AsyncBlock* asyncBlock = new AsyncBlock {};
asyncBlock->queue = asyncQueue;
asyncBlock->context = customDataForCallback;
asyncBlock->callback = [](AsyncBlock* asyncBlock)
{
    XblUserProfile profile;
    if( SUCCEEDED( XblProfileGetUserProfileResult(asyncBlock, &profile) ) )
    {
        printf("Profile retrieved successfully\r\n");
    }
    delete asyncBlock;
};
XblProfileGetUserProfileAsync(asyncBlock, xboxLiveContext, xuid);
```

Pour comprendre ce modèle d’appel, vous devez comprendre comment utiliser le **AsyncBlock** et **AsyncQueue**.

* Le **AsyncBlock** comporte toutes les informations se rapportant à la **tâche asynchrone** et **rappel d’achèvement**.

* Le **AsyncQueue** vous permet de déterminer le thread qui exécute le **tâche asynchrone** et le thread qui appelle le AsyncBlock **rappel d’achèvement**.

## <a name="the-asyncblock"></a>Le **AsyncBlock**

Jetons un œil à la **AsyncBlock** en détail. Il s’agit d’un struct défini comme suit :

```cpp
typedef struct AsyncBlock
{
    AsyncCompletionRoutine* callback;
    void* context;
    async_queue_handle_t queue;
} AsyncBlock;
```

Le **AsyncBlock** contient :

* *rappel* -fonction de rappel facultative qui sera appelée une fois le travail asynchrone a été effectué.  Si vous ne spécifiez pas un rappel, vous pouvez attendre la **AsyncBlock** avec **GetAsyncStatus** , puis d’obtenir les résultats.
* *contexte* -vous permet de transmettre des données à la fonction de rappel.
* *file d’attente* - async_queue_handle_t qui est un pointeur désignant un **AsyncQueue**. Si ce n’est pas défini, une file d’attente par défaut sera utilisé.

Vous devez créer un nouveau AsyncBlock sur le tas pour chaque API que vous appelez une opération asynchrone.  Le AsyncBlock doit résider jusqu'à ce que le rappel d’achèvement de la AsyncBlock est appelée, et il peut alors être supprimé.

> [!IMPORTANT]
> Un **AsyncBlock** doivent rester en mémoire jusqu'à ce que le **tâche asynchrone** se termine. Si elle est allouée dynamiquement, il peut être supprimé à l’intérieur de la AsyncBlock **rappel d’achèvement**.

### <a name="waiting-for-asynchronous-task"></a>En attente de **tâche asynchrone**

Vous pouvez indiquer un **tâche asynchrone** est d’effectuer un certain nombre de façons différentes :

* le AsyncBlock **rappel d’achèvement** est appelée
* Appelez **GetAsyncStatus** par true pour attendre jusqu'à la fin.
* Définir un waitEvent dans le **AsyncBlock** et attendez que l’événement soit signalé

Avec **GetAsyncStatus** et waitEvent, le **tâche asynchrone** est considérée comme terminée après la AsyncBlock **rappel d’achèvement** exécute toutefois le AsyncBlock **rappel d’achèvement** est facultatif.

Une fois le **tâche asynchrone** est terminée, vous pouvez obtenir les résultats.

### <a name="getting-the-result-of-the-asynchronous-task"></a>Obtenir le résultat de la **tâche asynchrone**

Pour obtenir le résultat, la plupart **API asynchrone** fonctions ont un correspondant \[nom de fonction\]entraîner de fonction pour recevoir le résultat de l’appel asynchrone. Dans notre exemple de code, **XblProfileGetUserProfileAsync** correspond un **XblProfileGetUserProfileResult** (fonction). Vous pouvez utiliser cette fonction pour retourner le résultat de la fonction et agir en conséquence.  Consultez la documentation de chaque **API asynchrone** (fonction) pour plus d’informations sur la récupération des résultats.

## <a name="the-asyncqueue"></a>Le **AsyncQueue**

Le **AsyncQueue** vous permet de déterminer le thread qui exécute le **tâche asynchrone** et le thread qui appelle le AsyncBlock **rappel d’achèvement**.

Vous pouvez contrôler le thread qui effectue ces opérations en définissant un *mode de répartition*. Il existe trois modes de distribution disponibles :

* *Manuel* -la file d’attente manuelle ne sont pas distribuées automatiquement.  Il incombe au développeur de les répartir sur n’importe quel thread qu’ils souhaitent. Cela peut servir à attribuer le travail ou le rappel côté d’un appel asynchrone à un thread spécifique.  Ce sujet est abordé plus en détail ci-dessous.

* *Pool de threads* - distribue à l’aide d’un pool de threads.  Le pool de threads appelle les appels en parallèle, en prenant un appel à exécuter à partir de la file d’attente à son tour comme des threads de pool de threads deviennent disponibles.  Le démontrer à utiliser ce service vous donne la quantité minimum de contrôle sur lequel le thread est utilisé.

* *Correction de Thread* - distribue à l’aide de QueueUserAPC sur le thread qui a créé la file d’attente asynchrone. Lorsque la file d’attente est un APC en mode utilisateur, le thread n’est pas dirigé pour appeler la fonction APC, sauf si elle est dans un état critique. Un thread entre dans un état critique à l’aide de **SleepEx**, **SignalObjectAndWait**, **WaitForSingleObjectEx**, **WaitForMultipleObjectsEx**, ou **MsgWaitForMultipleObjectsEx** pour effectuer une opération d’une attente

Pour créer un nouveau **AsyncQueue** vous devez appeler **CreateAsyncQueue**.

```cpp
STDAPI CreateAsyncQueue(
    _In_ AsyncQueueDispatchMode workDispatchMode,
    _In_ AsyncQueueDispatchMode completionDispatchMode,
    _Out_ async_queue_handle_t* queue);
```

où AsyncQueueDispatchMode contient les modes de trois répartition introduits précédemment :

```cpp
typedef enum AsyncQueueDispatchMode
{
    /// <summary>
    /// Async callbacks are invoked manually by DispatchAsyncQueue
    /// </summary>
    AsyncQueueDispatchMode_Manual,

    /// <summary>
    /// Async callbacks are queued to the thread that created the queue
    /// and will be processed when the thread is alertable.
    /// </summary>
    AsyncQueueDispatchMode_FixedThread,

    /// <summary>
    /// Async callbacks are queued to the system thread pool and will
    /// be processed in order by the threadpool.
    /// </summary>
    AsyncQueueDispatchMode_ThreadPool

} AsyncQueueDispatchMode;
```

**workDispatchMode** détermine le mode de distribution pour le thread qui gère le travail asynchrone, tandis que **completionDispatchMode** détermine le mode de distribution pour le thread qui gère l’achèvement de l’opération asynchrone opération.

Vous pouvez également appeler **CreateSharedAsyncQueue** pour créer un **AsyncQueue** avec le même type de file d’attente en spécifiant un ID pour la file d’attente.

```cpp
STDAPI CreateSharedAsyncQueue(
    _In_ uint32_t id,
    _In_ AsyncQueueDispatchMode workerMode,
    _In_ AsyncQueueDispatchMode completionMode,
    _Out_ async_queue_handle_t* aQueue);
```

> [!NOTE]
> S’il existe déjà une file d’attente avec cet ID et la distribution des modes, il sera référencé.  Sinon, une file d’attente sera créée.

Une fois que vous avez créé votre **AsyncQueue** simplement l’ajouter à la **AsyncBlock** pour contrôler le threading sur vos fonctions de travail et l’achèvement.
Lorsque vous avez terminé à l’aide de la **AsyncQueue**, généralement lorsque le jeu se termine, vous pouvez la fermer avec **CloseAsyncQueue**:

```cpp
STDAPI_(void) CloseAsyncQueue(
    _In_ async_queue_handle_t aQueue);
```

### <a name="manually-dispatching-an-asyncqueue"></a>La distribution des manuellement un **AsyncQueue**

Si vous avez utilisé le mode de répartition manuelle de file d’attente pour un **AsyncQueue** file d’attente de travail ou à l’achèvement, vous devez distribuer manuellement.
Supposons qu’un **AsyncQueue** a été créé dans lequel la file d’attente de travail et de la file d’attente de fin sont définies pour distribuer manuellement comme suit :

```cpp
CreateAsyncQueue(
    AsyncQueueDispatchMode_Manual,
    AsyncQueueDispatchMode_Manual,
    &queue);
```

Afin de répartir le travail qui a été affectée **AsyncQueueDispatchMode_Manual** vous devrez distribuer avec le **DispatchAsyncQueue** (fonction).

```cpp
STDAPI_(bool) DispatchAsyncQueue(
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type,
    _In_ uint32_t timeoutInMs);
```

* *file d’attente* -la file d’attente pour répartir le travail sur
* *type* : une instance de la **AsyncQueueCallbackType** enum
* *timeoutInMs* -un uint32_t pour le délai d’expiration en millisecondes.

Il existe deux types de rappel définis par le **AsyncQueueCallbackType** enum :

```cpp
typedef enum AsyncQueueCallbackType
{
    /// <summary>
    /// Async work callbacks
    /// </summary>
    AsyncQueueCallbackType_Work,

    /// <summary>
    /// Completion callbacks after work is done
    /// </summary>
    AsyncQueueCallbackType_Completion
} AsyncQueueCallbackType;
```

### <a name="when-to-call-dispatchasyncqueue"></a>Quand appeler **DispatchAsyncQueue**

Pour vérifier quand la file d’attente a reçu un nouvel élément vous pouvez appeler **AddAsyncQueueCallbackSubmitted** pour définir un gestionnaire d’événements pour informer votre code que le travail ou les saisies semi-automatiques sont prêts à être distribué.

```cpp
STDAPI AddAsyncQueueCallbackSubmitted(
    _In_ async_queue_handle_t queue,
    _In_opt_ void* context,
    _In_ AsyncQueueCallbackSubmitted* callback,
    _Out_ uint32_t* token);
```

**AddAsyncQueueCallbackSubmitted** accepte les paramètres suivants :

* *file d’attente* -la file d’attente asynchrone vous envoyez le rappel pour.
* *contexte* -un pointeur vers les données qui doivent être passés au rappel de soumission.
* *rappel* -la fonction qui sera appelée quand un nouveau rappel est envoyé à la file d’attente.
* *jeton* -un jeton qui sera utilisé dans un appel ultérieur à **RemoveAsynceCallbackSubmitted** pour supprimer le rappel.

Par exemple, Voici un appel à **AddAsyncQueueCallbackSubmitted**:

`AddAsyncQueueCallbackSubmitted(queue, nullptr, HandleAsyncQueueCallback, &m_callbackToken);`

Correspondants **AsyncQueueCallbackSubmitted** rappel peut être implémenté comme suit :

```cpp
void CALLBACK HandleAsyncQueueCallback(
    _In_ void* context,
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type)
{
    switch (type)
    {
    case AsyncQueueCallbackType::AsyncQueueCallbackType_Work:
        ReleaseSemaphore(g_workReadyHandle, 1, nullptr);
        break;
    }
}
```

Puis dans un thread d’arrière-plan, vous pouvez écouter pour ce sémaphore sortir de veille et appeler **DispatchAsyncQueue**.

```cpp
DWORD WINAPI BackgroundWorkThreadProc(LPVOID lpParam)
{
    HANDLE hEvents[2] =
    {
        g_workReadyHandle.get(),
        g_stopRequestedHandle.get()
    };

    async_queue_handle_t queue = static_cast<async_queue_handle_t>(lpParam);

    bool stop = false;
    while (!stop)
    {
        DWORD dwResult = WaitForMultipleObjectsEx(2, hEvents, false, INFINITE, false);
        switch (dwResult)
        {
        case WAIT_OBJECT_0: 
            // Background work is ready to be dispatched
            DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);
            break;

        case WAIT_OBJECT_0 + 1:
        default:
            stop = true;
            break;
        }
    }

    CloseAsyncQueue(queue);
    return 0;
}
```

Il est préférable de mettre en œuvre de pratique consiste à utiliser avec l’objet sémaphore de Win32.  Si vous implémentez plutôt à l’aide d’un objet d’événement de Win32, puis vous devrez vous assurer que ne manquez pas les événements avec du code tel que :

```cpp
    case WAIT_OBJECT_0: 
        // Background work is ready to be dispatched
        DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);        
        
        if (!IsAsyncQueueEmpty(queue, AsyncQueueCallbackType_Work))
        {
            // If there's more pending work, then set the event to process them
            SetEvent(g_workReadyHandle.get());
        }
        break;
```


Vous pouvez afficher un exemple des meilleures pratiques pour l’intégration d’async à [Social AsyncIntegration.cpp d’exemple C](https://github.com/Microsoft/xbox-live-api/blob/master/InProgressSamples/Social/Xbox/C/AsyncIntegration.cpp)

