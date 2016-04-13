---
Répondre aux événements système avec des tâches en arrière-plan
Découvrez comment créer une tâche en arrière-plan qui répond aux événements SystemTrigger.
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
---

# Répondre aux événements système avec des tâches en arrière-plan


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)

Découvrez comment créer une tâche en arrière-plan qui répond aux événements [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

Cette rubrique suppose qu’une classe de tâche en arrière-plan est écrite pour votre application et que cette tâche s’exécute en réponse à un événement déclenché par le système (par exemple, Internet qui devient accessible ou l’utilisateur qui se connecte). Cette rubrique porte sur la classe [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839). Pour plus d’informations sur l’écriture d’une classe de tâche en arrière-plan, voir [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md).

## Créer un objet SystemTrigger


-   Dans le code de votre application, créez un objet [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838). Le premier paramètre, *triggerType*, indique le type d’événement de déclencheur système qui activera cette tâche en arrière-plan. Pour obtenir la liste des types d’événements, voir [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839).

    Le deuxième paramètre, *OneShot*, indique si la tâche en arrière-plan sera exécutée une seule fois la prochaine fois que l’événement système survient ou déclenche des tâches en arrière-plan, ou bien chaque fois que l’événement système survient jusqu’à ce que la tâche soit désinscrite.

    Le code suivant spécifie que la tâche en arrière-plan s’exécute chaque fois qu’Internet devient accessible :

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
    > ```
    > ```cpp
    > SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
    > ```

## Inscrire la tâche en arrière-plan


-   Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire. Pour plus d’informations sur l’inscription des tâches en arrière-plan, voir [Inscrire une tâche en arrière-plan](register-a-background-task.md).

    Le code suivant inscrit la tâche en arrière-plan :

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Internet-based background task";
    > 
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Internet-based background task";
    > 
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```

    > **Remarque** Les applications Windows universelles doivent appeler [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire tout type de déclencheur en arrière-plan.

    Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, vous devez appeler [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471), puis [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) lorsque votre application est lancée après avoir été mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

    > **Remarque** Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Une erreur est retournée si l’un des paramètres d’inscription n’est pas valide. Vérifiez que votre application gère de façon fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

     

## Remarques


Pour voir une inscription de tâche en arrière-plan en action, téléchargez l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666).

Les tâches en arrière-plan peuvent s’exécuter en réponse à des événements [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) et [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517), mais vous devez toujours [déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md). Vous devez également appeler [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire tout type de tâche en arrière-plan.

Les applications peuvent inscrire des tâches en arrière-plan qui répondent aux événements [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) et [**NetworkOperatorNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/br224831), ce qui leur permet d’assurer une communication en temps réel avec l’utilisateur même lorsque l’application n’est pas au premier plan. Pour plus d’informations, voir [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

> **Remarque** Cet article s’adresse aux développeurs de Windows 10 qui créent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 
## Rubriques connexes


****

* [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md)

****

* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications du Windows Store (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=Mar16_HO1-->


