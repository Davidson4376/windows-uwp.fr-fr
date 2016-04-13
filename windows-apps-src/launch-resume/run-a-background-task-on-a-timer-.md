---
title: Exécuter une tâche en arrière-plan en fonction d’un minuteur
description: Découvrez comment planifier une tâche en arrière-plan unique ou exécuter une tâche en arrière-plan périodique.
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
---

# Exécuter une tâche en arrière-plan en fonction d’un minuteur


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)
-   [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)

Découvrez comment planifier une tâche en arrière-plan unique ou exécuter une tâche en arrière-plan périodique.

-   Cet exemple suppose que vous disposez d’une tâche en arrière-plan qui doit être exécutée régulièrement, ou à une heure spécifique, pour prendre en charge votre application. Une tâche en arrière-plan ne s’exécutera qu’à l’aide d’un [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) si vous avez appelé [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485).
-   Cette rubrique suppose que vous avez déjà créé une classe de tâche en arrière-plan, incluant la méthode Run utilisée comme point d’entrée de la tâche en arrière-plan. Pour vous lancer rapidement dans la génération d’une tâche en arrière-plan, voir [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md). Pour des informations plus détaillées sur les conditions et les déclencheurs, voir [Définition de tâches en arrière-plan pour les besoins de votre application](support-your-app-with-background-tasks.md).

## Créer un déclencheur de temps


-   Créez un [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). Le second paramètre, *OneShot*, indique si la tâche en arrière-plan s’exécute une seule fois ou régulièrement. Si *OneShot* a la valeur True, le premier paramètre (*FreshnessTime*) indique le nombre de minutes à attendre avant de planifier la tâche en arrière-plan. Si *OneShot* a la valeur False, *FreshnessTime* indique la fréquence à laquelle la tâche en arrière-plan s’exécute.

    Le minuteur intégré pour les applications de plateforme Windows universelle (UWP) exécute des tâches en arrière-plan par intervalles de 15 minutes.

    -   Si *FreshnessTime* indique une fréquence de 15 minutes et si *OneShot* a la valeur True, la tâche sera exécutée une seule fois entre 0 et 15 minutes à partir du moment où elle sera inscrite.

    -   Si *FreshnessTime* indique une fréquence de 15 minutes et si *OneShot* a la valeur False, la tâche sera exécutée toutes les 15 minutes, entre 0 et 15 minutes à partir du moment où elle sera inscrite.

    **Remarque** Si *FreshnessTime* présente une fréquence inférieure à 15 minutes, une exception est levée lors de la tentative d’inscription de la tâche en arrière-plan.

     

    For example, this trigger will cause a background task to run once an hour:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
    > ```
    > ```cpp
    > TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
    > ```

## (Facultatif) Ajouter une condition


-   Si besoin est, créez une condition de tâche en arrière-plan afin de contrôler le moment où la tâche est exécutée. Une condition empêche la tâche en arrière-plan de s’exécuter tant que la condition n’est pas satisfaite. Pour plus d’informations, voir [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

    Dans cet exemple, la condition est définie à **UserPresent**. Ainsi, une fois déclenchée, la tâche s’exécute une fois que l’utilisateur est actif. Pour obtenir la liste des conditions possibles, voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
    > ```
    > ```cpp
    > SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent)
    > ```

##  Appeler RequestAccessAsync()


-   Avant d’essayer d’inscrire la tâche en arrière-plan [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), appelez [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > BackgroundExecutionManager.RequestAccessAsync();
    > ```
    > ```cpp
    > BackgroundExecutionManager::RequestAccessAsync();
    > ```

## Inscrire la tâche en arrière-plan


-   Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire. Pour plus d’informations sur l’inscription des tâches en arrière-plan, voir [Inscrire une tâche en arrière-plan](register-a-background-task.md).

    Le code suivant inscrit la tâche en arrière-plan :

    > > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = “Tasks.ExampleBackgroundTaskClass”;
    > string taskName   = “Example hourly background task”;
    > 
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = “Tasks.ExampleBackgroundTaskClass”;
    > String ^ taskName   = “Example hourly background task”;
    > 
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```
    
    > **Remarque** Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Une erreur est retournée si l’un des paramètres d’inscription n’est pas valide. Vérifiez que votre application gère de façon fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

   
## Remarques

> **Remarque** À partir de Windows 10, il n’est plus nécessaire pour l’utilisateur d’ajouter votre application à l’écran de verrouillage pour utiliser des tâches en arrière-plan. Pour obtenir des indications sur les types de déclencheurs de tâche en arrière-plan, voir [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

> **Remarque** Cet article s’adresse aux développeurs de Windows 10 qui créent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).


## Rubriques connexes


* [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Recommandations pour les tâches en arrière-plan](guidelines-for-background-tasks.md)

* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications du Windows Store (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=Mar16_HO1-->


