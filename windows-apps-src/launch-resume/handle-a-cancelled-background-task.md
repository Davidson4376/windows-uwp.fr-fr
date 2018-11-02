---
author: TylerMSFT
title: Gérer une tâche en arrière-plan annulée
description: Découvrez comment faire en sorte qu’une tâche en arrière-plan reconnaisse les demandes d’annulation et arrête le travail, tout en signalant l’annulation à l’application utilisant le stockage persistant.
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
keywords: tâche en arrière-plan Windows 10, uwp,
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 91de18af818113d79564ee8dfba7519a0f131246
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5969334"
---
# <a name="handle-a-cancelled-background-task"></a>Gérer une tâche en arrière-plan annulée

**API importantes**

-   [**BackgroundTaskCanceledEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224775)
-   [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)
-   [**ApplicationData.Current**](https://msdn.microsoft.com/library/windows/apps/br241619)

Découvrez comment créer une tâche en arrière-plan qui reconnaît une demande d’annulation, arrête le travail et signale l’annulation à l’application en utilisant le dispositif de stockage persistant.

Cette rubrique suppose que vous avez déjà créé une classe de tâche en arrière-plan, y compris la méthode **Run** utilisée comme le point d’entrée de tâche en arrière-plan. Pour commencer rapidement à créer une tâche en arrière-plan, consultez [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md) ou [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md). Pour des informations plus détaillées sur les conditions et les déclencheurs, voir [Définition de tâches en arrière-plan pour les besoins de votre application](support-your-app-with-background-tasks.md).

Cette rubrique s’applique également aux tâches en arrière-plan in-process. Mais au lieu de la méthode **Run** , remplacez **OnBackgroundActivated**. Pour les tâches en arrière-plan in-process, il n’est pas nécessaire d’utiliser un dispositif de stockage persistant pour signaler l’annulation. En effet, vous pouvez signaler l’annulation via le paramètre d’état de l’application, car la tâche en arrière-plan s’exécute dans le même processus que votre application au premier plan.

## <a name="use-the-oncanceled-method-to-recognize-cancellation-requests"></a>Utiliser la méthode OnCanceled pour reconnaître les demandes d’annulation

Écrivez une méthode permettant de gérer l’événement d’annulation.

> [!NOTE]
> Pour toutes les familles d’appareils, à l’exception des ordinateurs de bureau, les tâches en arrière-plan peuvent être arrêtées en cas de mémoire insuffisante de l’appareil. Si une exception de mémoire insuffisante n’est exposée ou si elle ne gère pas l’application, puis la tâche en arrière-plan est alors arrêtée sans avertissement ni déclenchement de l’événement OnCanceled. Cela permet de garantir l’expérience utilisateur de l’application au premier plan. Votre tâche en arrière-plan doit être conçue de manière à gérer ce scénario.

Créez une méthode nommée **OnCanceled** en procédant comme suit. Cette méthode constitue le point d’entrée appelé par Windows Runtime lorsqu’une demande d’annulation est formulée pour votre tâche en arrière-plan.

```csharp
private void OnCanceled(
    IBackgroundTaskInstance sender,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(
    IBackgroundTaskInstance^ taskInstance,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

Ajoutez une variable d’indicateur appelée **\_CancelRequested** à la classe de tâche en arrière-plan. Cette variable servira à indiquer qu’une demande d’annulation a été effectuée.

```csharp
volatile bool _CancelRequested = false;
```

```cppwinrt
private:
    volatile bool m_cancelRequested;
```

```cpp
private:
    volatile bool CancelRequested;
```

Dans la méthode **OnCanceled** que vous avez créé à l’étape 1, définissez la variable indicateur **\_CancelRequested** sur **true**.

L' complet [exemple de tâche en arrière-plan]( http://go.microsoft.com/fwlink/p/?linkid=227509) , méthode **OnCanceled** définit **\_CancelRequested** sur **true** et écrit une sortie de débogage peuvent se révéler utiles.

```csharp
private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    _cancelRequested = true;

    Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    m_cancelRequested = true;
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    CancelRequested = true;
}
```

Dans la méthode **Run** de la tâche en arrière-plan, inscrivez la méthode de gestionnaire d’événements **OnCanceled** avant de commencer à travailler. Dans le cas d’une tâche en arrière-plan in-process, vous pouvez effectuer cette inscription dans le cadre de l’initialisation de votre application. Par exemple, utilisez la ligne de code suivante.

```csharp
taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
```

```cppwinrt
taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });
```

```cpp
taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);
```

## <a name="handle-cancellation-by-exiting-your-background-task"></a>Gérer une annulation en fermant votre tâche en arrière-plan

Lors de la réception d’une demande d’annulation, la méthode qui effectue la tâche en arrière-plan doit arrêter le travail et se fermer en reconnaissant que **\_cancelRequested** est défini sur la valeur **true**. Pour les tâches en arrière-plan in-process, cela implique un retour à partir de la méthode **OnBackgroundActivated** . Pour les tâches en arrière-plan out-of-process, cela implique un retour à partir de la méthode **Run** .

Modifiez le code de votre classe de tâche en arrière-plan pour vérifier la variable d’indicateur pendant qu’elle est utilisée. Si **\_cancelRequested** devient défini sur true, arrêter le travail de continuer.

L' [exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666) comprend une vérification qui arrête le rappel de minuteur périodique en cas d’annulation de la tâche en arrière-plan.

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

> [!NOTE]
> L’exemple de code présenté ci-dessus utilise [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797). Propriété de [**progression**](https://msdn.microsoft.com/library/windows/apps/br224800) qui sert à enregistrer la progression de la tâche en arrière-plan. La progression est indiquée à l’application à l’aide de la classe [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782).

Modifiez la méthode **Run** de sorte qu’une fois le travail arrêté, elle enregistre si la tâche terminée ou a été annulée. Cette étape s’applique aux tâches en arrière-plan hors processus, car vous avez besoin d’un moyen pour communiquer entre les processus lorsque la tâche en arrière-plan a été annulée. Pour les tâches en arrière-plan in-process, vous pouvez simplement partager l’état avec l’application pour indiquer que la tâche a été annulée.

L' [exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666) enregistre l’état dans LocalSettings.

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();

    var settings = ApplicationData.Current.LocalSettings;
    var key = _taskInstance.Task.TaskId.ToString();

    // Write to LocalSettings to indicate that this background task ran.
    if (_cancelRequested)
    {
        settings.Values[key] = "Canceled";
    }
    else
    {
        settings.Values[key] = "Completed";
    }
        
    Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
        
    // Indicate that the background task has completed.
    _deferral.Complete();
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();

    // Write to LocalSettings to indicate that this background task ran.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    auto key{ m_taskInstance.Task().Name() };
    settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

    // Indicate that the background task has completed.
    m_deferral.Complete();
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
        
    // Write to LocalSettings to indicate that this background task ran.
    auto settings = ApplicationData::Current->LocalSettings;
    auto key = TaskInstance->Task->Name;
    settings->Values->Insert(key, (Progress < 100) ? "Canceled" : "Completed");
        
    // Indicate that the background task has completed.
    Deferral->Complete();
}
```

## <a name="remarks"></a>Remarques

Vous pouvez télécharger l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666) pour voir ces exemples de code dans le contexte des méthodes.

À des fins d’illustration, l’exemple de code présente uniquement des portions de la méthode **Run** (et le minuteur de rappel) de l' [exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666).

## <a name="run-method-example"></a>Exemple de méthode Run

La terminer méthode **Run** et le code de rappel de minuteur, à partir de l' [exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666) sont présentés ci-dessous pour le contexte.

```csharp
// The Run method is the entry point of a background task.
public void Run(IBackgroundTaskInstance taskInstance)
{
    Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");

    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
    var settings = ApplicationData.Current.LocalSettings;
    settings.Values["BackgroundWorkCost"] = cost.ToString();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance;
    _deferral = taskInstance.GetDeferral();
    _taskInstance = taskInstance;

    _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
}

// Simulate the background task activity.
private void PeriodicTimerCallback(ThreadPoolTimer timer)
{
    if ((_cancelRequested == false) && (_progress < 100))
    {
        _progress += 10;
        _taskInstance.Progress = _progress;
    }
    else
    {
        _periodicTimer.Cancel();

        var settings = ApplicationData.Current.LocalSettings;
        var key = _taskInstance.Task.Name;

        // Write to LocalSettings to indicate that this background task ran.
        settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
        Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);

        // Indicate that the background task has completed.
        _deferral.Complete();
    }
}
```

```cppwinrt
void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost{ Windows::ApplicationModel::Background::BackgroundWorkCost::CurrentBackgroundWorkCost() };
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    std::wstring costAsString{ L"Low" };
    if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::Medium) costAsString = L"Medium";
    else if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::High) costAsString = L"High";
    settings.Values().Insert(L"BackgroundWorkCost", winrt::box_value(costAsString));

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    m_deferral = taskInstance.GetDeferral();
    m_taskInstance = taskInstance;

    Windows::Foundation::TimeSpan period{ std::chrono::seconds{1} };
    m_periodicTimer = Windows::System::Threading::ThreadPoolTimer::CreatePeriodicTimer([this](Windows::System::Threading::ThreadPoolTimer timer)
    {
        if (!m_cancelRequested && m_progress < 100)
        {
            m_progress += 10;
            m_taskInstance.Progress(m_progress);
        }
        else
        {
            m_periodicTimer.Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
            auto key{ m_taskInstance.Task().Name() };
            settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

            // Indicate that the background task has completed.
            m_deferral.Complete();
        }
    }, period);
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
    auto settings = ApplicationData::Current->LocalSettings;
    settings->Values->Insert("BackgroundWorkCost", cost.ToString());

    // Associate a cancellation handler with the background task.
    taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    TaskDeferral = taskInstance->GetDeferral();
    TaskInstance = taskInstance;

    auto timerDelegate = [this](ThreadPoolTimer^ timer)
    {
        if ((CancelRequested == false) &&
            (Progress < 100))
        {
            Progress += 10;
            TaskInstance->Progress = Progress;
        }
        else
        {
            PeriodicTimer->Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings = ApplicationData::Current->LocalSettings;
            auto key = TaskInstance->Task->Name;
            settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");

            // Indicate that the background task has completed.
            TaskDeferral->Complete();
        }
    };

    TimeSpan period;
    period.Duration = 1000 * 10000; // 1 second
    PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
}
```

## <a name="related-topics"></a>Rubriquesconnexes

- [Créez et inscrivez une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
- [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
- [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
- [Recommandations pour les tâches en arrière-plan](guidelines-for-background-tasks.md)
- [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
- [Inscrire une tâche en arrière-plan](register-a-background-task.md)
- [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
- [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
- [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
- [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
- [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
- [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
- [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications UWP (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)
