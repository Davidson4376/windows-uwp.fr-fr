---
author: TylerMSFT
title: "Utiliser un déclencheur de maintenance"
description: "Découvrez comment utiliser la classe MaintenanceTrigger pour exécuter du code léger en arrière-plan tandis que l’appareil est branché."
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 0da08ba5431f4d5c56d06657d3d6123a67ba5079

---

# Utiliser un déclencheur de maintenance


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

Découvrez comment utiliser la classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) pour exécuter du code léger en arrière-plan tandis que l’appareil est branché.

## Créer un objet déclencheur de maintenance


Cet exemple suppose que vous disposez d’un code léger à exécuter en arrière-plan qui vous permettra d’améliorer votre application pendant que l’appareil est branché. Cette rubrique porte sur la classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517), qui est semblable à [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839). Pour plus d’informations sur l’écriture d’une classe de tâche en arrière-plan, voir [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md).

Créez un objet [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). Le deuxième paramètre, *OneShot*, indique si la tâche de maintenance s’exécute une seule fois ou régulièrement. Si *OneShot* présente la valeur true, le premier paramètre (*FreshnessTime*) indique le nombre de minutes à attendre avant de planifier la tâche en arrière-plan. Si *OneShot* est défini sur false, *FreshnessTime* indique la fréquence d’exécution de la tâche en arrière-plan.

> **Remarque** Si *FreshnessTime* présente une fréquence inférieure à 15 minutes, une exception est levée lors de la tentative d’inscription de la tâche en arrière-plan.

 

Cet exemple de code crée un déclencheur qui s’exécute une fois par heure:

> [!div class="tabbedCodeSnippets"]
> ```cs
> uint waitIntervalMinutes = 60;
>
> MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
> ```
> ```cpp
> unsigned int waitIntervalMinutes = 60;
>
> MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
> ```

## [!div class="tabbedCodeSnippets"]

-   (Facultatif) Ajouter une condition Si besoin est, créez une condition de tâche en arrière-plan afin de contrôler le moment où la tâche est exécutée.

    Une condition empêche votre tâche en arrière-plan de s’exécuter tant que la condition n’est pas satisfaite. Pour plus d’informations, voir [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md). Dans cet exemple, la condition est définie sur **InternetAvailable**, de sorte que la maintenance s’exécute quand Internet est accessible (ou le devient).

    Pour obtenir la liste des conditions de tâche en arrière-plan possibles, voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
    > ```
    > ```cpp
    > SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
    > ```

## Le code suivant ajoute une condition au générateur de tâches de maintenance:


-   [!div class="tabbedCodeSnippets"] Inscrire la tâche en arrière-plan

    Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Maintenance background task example";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Maintenance background task example";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
    > ```

    > Pour plus d’informations sur l’inscription des tâches en arrière-plan, voir [Inscrire une tâche en arrière-plan](register-a-background-task.md). Le code suivant inscrit la tâche de maintenance: [!div class="tabbedCodeSnippets"] **Remarque** Pour toutes les familles d’appareils, à l’exception des ordinateurs de bureau, les tâches en arrière-plan peuvent être arrêtées en cas de mémoire insuffisante de l’appareil.

    > Si aucune exception de mémoire insuffisante n’est exposée ou que l’application ne la gère pas, la tâche en arrière-plan est arrêtée sans avertissement ni déclenchement de l’événement OnCanceled.

    Cela permet de garantir l’expérience utilisateur de l’application au premier plan. Votre tâche en arrière-plan doit être conçue de manière à gérer ce scénario.

    > **Remarque** Les applications Windows universelles doivent appeler [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire tout type de déclencheur en arrière-plan. Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, vous devez appeler [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471), puis [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) lorsque votre application est lancée après avoir été mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).


> **Remarque** Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Une erreur est retournée si l’un des paramètres d’inscription n’est pas valide.

## Vérifiez que votre application gère de façon fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.


****

* [**Remarque** Cet article s’adresse aux développeurs de Windows 10 qui créent des applications pour la plateforme Windows universelle (UWP).](create-and-register-a-background-task.md)
* [Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).](declare-background-tasks-in-the-application-manifest.md)
* [Rubriques connexes](handle-a-cancelled-background-task.md)
* [Créer et inscrire une tâche en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](register-a-background-task.md)
* [Gérer une tâche en arrière-plan annulée](respond-to-system-events-with-background-tasks.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Inscrire une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](run-a-background-task-on-a-timer-.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](guidelines-for-background-tasks.md)

****

* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](debug-a-background-task.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->


