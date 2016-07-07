---
author: TylerMSFT
title: "Surveiller la progression et l’achèvement des tâches en arrière-plan"
description: "Découvrez comment votre application peut reconnaître la progression et l’achèvement signalés par une tâche en arrière-plan."
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 07d69b63b272153bc784ed19166e649f80a98297

---

# Surveiller la progression et l’achèvement des tâches en arrière-plan


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskProgressEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224785)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Découvrez comment votre application peut reconnaître la progression et l’achèvement signalés par une tâche en arrière-plan. Les tâches en arrière-plan sont découplées de l’application et sont exécutées séparément. Toutefois, la progression et l’achèvement des tâches en arrière-plan peuvent être surveillés par le code de l’application. Pour ce faire, l’application s’abonne aux événements des tâches en arrière-plan qu’elle a inscrites auprès du système.

-   Cette rubrique suppose que vous disposez d’une application qui inscrit les tâches en arrière-plan. Pour vous lancer rapidement dans la génération d’une tâche en arrière-plan, voir [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md). Pour des informations plus détaillées sur les conditions et les déclencheurs, voir [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

## Créer un gestionnaire d’événements pour gérer les tâches en arrière-plan achevées


1.  Créez une fonction de gestionnaire des événements pour gérer les tâches en arrière-plan achevées. Ce code doit suivre une empreinte spécifique, qui accepte un objet [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) et un objet [**BackgroundTaskCompletedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224778).

    Utilisez l’empreinte suivante pour la méthode de gestionnaire d’événements de tâche en arrière-plan OnCompleted:

>  [!div class="tabbedCodeSnippets"]
>  ```cs
>  private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
>  {
>      // TODO: Add code that deals with background task completion.
>  }
>  ```
>  ```cpp
>  auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
>  {
>      // TODO: Add code that deals with background task completion.
>  };
>  ```

2.  [!div class="tabbedCodeSnippets"]

    Ajoutez du code au gestionnaire d’événements qui traite l’achèvement des tâches en arrière-plan.

    > [!div class="tabbedCodeSnippets"]
    >     ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {    
    >         UpdateUI();
    >     };
    >     ```

## Par exemple, l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666) met à jour l’interface utilisateur.


1.  [!div class="tabbedCodeSnippets"]     ```cs
    private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    {
        UpdateUI();
    }
    ```
    ```cpp
    auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    {    
        UpdateUI();
    };
    ``` Créer une fonction de gestionnaire d’événements pour gérer la progression des tâches en arrière-plan

    Créez une fonction de gestionnaire des événements pour gérer les tâches en arrière-plan achevées.

    > [!div class="tabbedCodeSnippets"]
    >     ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     }
    >     ```
    >     ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     };
    >     ```

2.  Ce code doit suivre une empreinte spécifique, qui accepte un objet [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) et un objet [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782).

    Utilisez l’empreinte suivante pour la méthode de gestionnaire d’événements de tâche en arrière-plan OnProgress:

    > [!div class="tabbedCodeSnippets"]
    >     [!div class="tabbedCodeSnippets"]     ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     }
    ```
    ```cpp
    auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    {
        // TODO: Add code that deals with background task progress.
    };
    ```
    >
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         auto progress = "Progress: " + args->Progress + "%";
    >         BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     };
    >     ```

## Ajoutez du code au gestionnaire d’événements qui traite l’achèvement des tâches en arrière-plan.


1.  Ainsi, l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666) met à jour l’interface utilisateur conformément à l’état de progression transmis avec le paramètre *args*:

    [!div class="tabbedCodeSnippets"] ```cs private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args) { var progress = "Progress: " + args.Progress + "%"; BackgroundTaskSample.SampleBackgroundTaskProgress = progress;

    > [!div class="tabbedCodeSnippets"]
    >     ```cs
    >     private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
    >     {
    >         task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    >         task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    >     }
    >     ```
    >     Inscrivez les fonctions de gestionnaire d’événements auprès des tâches en arrière-plan nouvelles et existantes.
    >
    >         task->Progress += ref new BackgroundTaskProgressEventHandler(progress);
    >         
    >
    >         auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >         {
    >             UpdateUI();
    >         };
    >
    >         task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
    >     }
    >     ```

2.  Lorsque l’application inscrit une tâche en arrière-plan pour la première fois, elle doit s’inscrire pour recevoir les mises à jour de progression et d’achèvement de la tâche au cas où celle-ci s’exécuterait pendant que l’application est toujours active au premier plan. Ainsi, l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666) appelle la fonction suivante pour chaque tâche en arrière-plan qu’il inscrit:

    [!div class="tabbedCodeSnippets"]     ```cs
    private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
    {
        task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
        task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    }
    ```
 ```cpp void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task) { auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
           { auto progress = "Progress: " + args-&gt;Progress + "%"; BackgroundTaskSample::SampleBackgroundTaskProgress = progress; UpdateUI(); };

    > [!div class="tabbedCodeSnippets"]
    >     Lorsque l’application est lancée ou accède à une nouvelle page dans laquelle l’état des tâches en arrière-plan est important, elle doit obtenir la liste des tâches en arrière-plan actuellement inscrites et les associer aux fonctions de gestionnaire d’événements de progression et d’achèvement.
    >
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
    >     {
    >         // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    >         // as NotifyUser()
    >         rootPage = MainPage::Current;
    >
    >         //
    >         // Attach progress and completed handlers to any existing tasks.
    >         //
    >         auto iter = BackgroundTaskRegistration::AllTasks->First();
    >         auto hascur = iter->HasCurrent;
    >         while (hascur)
    >         {
    >             auto cur = iter->Current->Value;
    >
    >             if (cur->Name == SampleBackgroundTaskName)
    >             {
    >                 AttachProgressAndCompletedHandlers(cur);
    >                 break;
    >             }
    >
    >             hascur = iter->MoveNext();
    >         }
    >
    >         UpdateUI();
    >     }
    >     ```

## Cette liste est conservée dans la propriété [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).[**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787).


****

* [Ainsi, l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666) utilise le code suivant pour joindre les gestionnaires d’événements lorsque vous accédez à la page SampleBackgroundTask:](create-and-register-a-background-task.md)
* [[!div class="tabbedCodeSnippets"] ```cs protected override void OnNavigatedTo(NavigationEventArgs e) { foreach (var task in BackgroundTaskRegistration.AllTasks) { if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName) { AttachProgressAndCompletedHandlers(task.Value); BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true); } }](declare-background-tasks-in-the-application-manifest.md)
* [Rubriques connexes](handle-a-cancelled-background-task.md)
* [Créer et inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](respond-to-system-events-with-background-tasks.md)
* [Gérer une tâche en arrière-plan annulée](set-conditions-for-running-a-background-task.md)
* [Inscrire une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](use-a-maintenance-trigger.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](run-a-background-task-on-a-timer-.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](guidelines-for-background-tasks.md)

****

* [Utiliser un déclencheur de maintenance](debug-a-background-task.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->


