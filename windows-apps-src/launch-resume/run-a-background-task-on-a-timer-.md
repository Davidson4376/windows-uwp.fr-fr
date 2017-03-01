---
author: TylerMSFT
title: "Exécuter une tâche en arrière-plan en fonction d’un minuteur"
description: "Découvrez comment planifier une tâche en arrière-plan unique ou exécuter une tâche en arrière-plan périodique."
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3708a9b7768d4fb7fbb6af0e55836471a2ba29ed
ms.lasthandoff: 02/07/2017

---

# <a name="run-a-background-task-on-a-timer"></a>Exécuter une tâche en arrière-plan en fonction d’un minuteur

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**API importantes**

-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)
-   [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)

Découvrez comment planifier une tâche en arrière-plan unique ou exécuter une tâche en arrière-plan périodique.

-   Cet exemple suppose que vous disposez d’une tâche en arrière-plan qui doit être exécutée régulièrement, ou à une heure spécifique, pour prendre en charge votre application. Une tâche en arrière-plan ne s’exécutera qu’à l’aide d’un [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) si vous avez appelé [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485).
-   Cette rubrique part du principe que vous avez déjà créé une classe de tâche en arrière-plan. Pour commencer rapidement à créer une tâche en arrière-plan, consultez [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md) ou [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md). Pour des informations plus détaillées sur les conditions et les déclencheurs, voir [Définition de tâches en arrière-plan pour les besoins de votre application](support-your-app-with-background-tasks.md).

## <a name="create-a-time-trigger"></a>Créer un déclencheur de temps

-   Créez un [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). Le second paramètre, *OneShot*, indique si la tâche en arrière-plan s’exécute une seule fois ou régulièrement. Si *OneShot* est défini sur true, le premier paramètre (*FreshnessTime*) indique le nombre de minutes à attendre avant de planifier la tâche en arrière-plan. Si *OneShot* est défini sur false, le paramètre *FreshnessTime* indique la fréquence d’exécution de la tâche en arrière-plan.

    Le minuteur intégré pour les applications de plateforme Windows universelle (UWP) qui ciblent les appareils mobiles ou de bureau exécute des tâches en arrière-plan par intervalles de 15 minutes.

    -   Si *FreshnessTime* indique une fréquence de 15 minutes et que *OneShot* est défini sur true, la tâche sera programmée pour être exécutée une seule fois, 15 à 30 minutes après son inscription. Si le paramètre FreshnessTime indique une fréquence de 25 minutes et que *OneShot* est défini sur true, la tâche sera programmée pour être exécutée une seule fois, 25 à 40 minutes après son inscription.

    -   Si *FreshnessTime* indique une fréquence de 15 minutes et que *OneShot* est défini sur false, la tâche sera programmée pour être exécutée toutes les 15 minutes, 15 à 30 minutes après son inscription. Si FreshnessTime indique une fréquence de n minutes et que *OneShot* est défini sur false, la tâche sera programmée pour être exécutée toutes les n minutes, n à n+15 minutes après son inscription.

    **Remarque** Si *FreshnessTime* indique une fréquence inférieure à 15 minutes, une exception est levée lors de la tentative d’inscription de la tâche en arrière-plan.
 

    Par exemple, ce déclencheur entraîne l’exécution d’une tâche en arrière-plan une fois par heure :

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
    > ```
    > ```cpp
    > TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
    > ```

## <a name="optional-add-a-condition"></a>(Facultatif) Ajouter une condition

-   Si besoin est, créez une condition de tâche en arrière-plan afin de contrôler le moment où la tâche est exécutée. Une condition empêche la tâche en arrière-plan de s’exécuter tant que la condition n’est pas satisfaite. Pour plus d’informations, voir [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

    Dans cet exemple, la condition est définie sur **UserPresent**. Ainsi, une fois déclenchée, la tâche s’exécute une fois que l’utilisateur est actif. Pour obtenir la liste des conditions possibles, voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
    > ```
    > ```cpp
    > SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent)
    > ```

##  <a name="call-requestaccessasync"></a>Appeler RequestAccessAsync()

-   Avant d’essayer d’inscrire la tâche en arrière-plan [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), appelez [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > BackgroundExecutionManager.RequestAccessAsync();
    > ```
    > ```cpp
    > BackgroundExecutionManager::RequestAccessAsync();
    > ```

## <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan

-   Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire. Pour plus d’informations sur l’inscription des tâches en arrière-plan, voir [Inscrire une tâche en arrière-plan](register-a-background-task.md).

> [!Important]
> Pour les tâches en arrière-plan qui s’exécutent dans le même processus que votre application, ne définissez pas `entryPoint`. Pour les tâches en arrière-plan qui s’exécutent dans un processus distinct de celui de votre application, définissez `entryPoint` au format suivant : espace de noms, «. » et nom de la classe qui contient l’implémentation de votre tâche en arrière-plan.

    The following code registers a background task that runs out-of-process:

    > > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```

    > **Note**  Background task registration parameters are validated at the time of registration. An error is returned if any of the registration parameters are invalid. Ensure that your app gracefully handles scenarios where background task registration fails - if instead your app depends on having a valid registration object after attempting to register a task, it may crash.


## <a name="remarks"></a>Remarques

> **Remarque** Depuis Windows 10, l’utilisateur n’est plus obligé d’ajouter votre application à l’écran de verrouillage pour utiliser des tâches en arrière-plan. Pour obtenir des indications sur les types de déclencheur de tâche en arrière-plan, voir [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

> **Remarque** Cet article s’adresse aux développeurs Windows 10 qui développent des applications de plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

## <a name="related-topics"></a>Rubriques connexes

* [Créez et inscrivez une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
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

