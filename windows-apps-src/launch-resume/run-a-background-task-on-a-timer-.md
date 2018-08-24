---
author: TylerMSFT
title: Exécuter une tâche en arrière-plan en fonction d’un minuteur
description: Découvrez comment planifier une tâche en arrière-plan unique ou exécuter une tâche en arrière-plan périodique.
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: tâche d’arrière-plan Windows 10, uwp,
ms.localizationpriority: medium
ms.openlocfilehash: 25e3c76ae09ed6835f89f0d98c308f11c7a99624
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2836341"
---
# <a name="run-a-background-task-on-a-timer"></a>Exécuter une tâche en arrière-plan en fonction d’un minuteur

Découvrez comment utiliser le [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) pour planifier une tâche d’arrière-plan unique ou exécuter une tâche de fond périodiques.

Voir **Scenario4** dans l' [exemple de l’activation d’arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation) pour voir un exemple d’implémentation de l’heure tâche déclenchée en arrière-plan décrite dans cette rubrique.

Cette rubrique suppose que vous disposez d’une tâche en arrière-plan qui doit s’exécuter périodiquement ou à un moment donné. Si vous ne disposez pas d’une tâche en arrière-plan, il est un exemple de tâche de fond à [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). Ou, suivez les étapes de [créer et enregistrer une tâche en arrière-plan en cours](create-and-register-an-inproc-background-task.md) ou [créer et enregistrer une tâche d’arrière-plan du processus de mise](create-and-register-a-background-task.md) en créer une.

## <a name="create-a-time-trigger"></a>Créer un déclencheur de temps

Créez un [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). Le second paramètre, *OneShot*, indique si la tâche en arrière-plan s’exécute une seule fois ou régulièrement. Si *OneShot* est défini sur true, le premier paramètre (*FreshnessTime*) indique le nombre de minutes à attendre avant de planifier la tâche en arrière-plan. Si *OneShot* est défini sur false, le paramètre *FreshnessTime* indique la fréquence d’exécution de la tâche en arrière-plan.

Le minuteur intégré pour les applications de plateforme Windows universelle (UWP) qui ciblent les appareils mobiles ou de bureau exécute des tâches en arrière-plan par intervalles de 15minutes. (Le minuteur s’exécute dans des intervalles de 15 minutes afin que le système n’a pas besoin de sortir toutes les 15 minutes pour activer les applications qui ont demandé TimerTriggers--qui enregistre power.)

- Si *FreshnessTime* indique une fréquence de 15minutes et que *OneShot* est défini sur true, la tâche sera programmée pour être exécutée une seule fois, 15 à 30minutes après son inscription. Si le paramètre FreshnessTime indique une fréquence de 25minutes et que *OneShot* est défini sur true, la tâche sera programmée pour être exécutée une seule fois, 25 à 40minutes après son inscription.

- Si *FreshnessTime* indique une fréquence de 15minutes et que *OneShot* est défini sur false, la tâche sera programmée pour être exécutée toutes les 15minutes, 15 à 30minutes après son inscription. Si FreshnessTime indique une fréquence de nminutes et que *OneShot* est défini sur false, la tâche sera programmée pour être exécutée toutes les nminutes, n à n+15minutes après son inscription.

> [!NOTE]
> Si la valeur *FreshnessTime* est inférieure à 15 minutes, une exception est levée lors de la tentative d’enregistrement de la tâche en arrière-plan.
 
Par exemple, ce déclencheur entraînera une tâche en arrière-plan exécuter une fois par heure.

```cs
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
```

```cppwinrt
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };
```

```cpp
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
```

## <a name="optional-add-a-condition"></a>(Facultatif) Ajouter une condition

Vous pouvez créer une condition de tâche de fond au contrôle lors de l’exécution de la tâche. Une condition empêche la tâche d’arrière-plan d’en cours d’exécution jusqu'à ce que la condition est remplie. Pour plus d’informations, voir [définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

Dans cet exemple, la condition est définie sur **UserPresent**. Ainsi, une fois déclenchée, la tâche s’exécute une fois que l’utilisateur est actif. Pour obtenir la liste des conditions possibles, voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

```cs
SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
```

```cpp
SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent);
```

Pour plus d’informations sur les conditions et les types de déclencheurs d’arrière-plan, voir [prise en charge de votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Appeler RequestAccessAsync()

Avant d’enregistrer la tâche d’arrière-plan **ApplicationTrigger** , appelez [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) pour déterminer le niveau d’activité en arrière-plan que permet à l’utilisateur, car l’utilisateur a peut-être désactivé activité en arrière-plan pour votre application. Voir l' [activité d’arrière-plan optimiser](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) pour plus d’informations sur les utilisateurs moyens peuvent contrôler les paramètres pour l’activité en arrière-plan.

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan

Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire. Pour plus d’informations sur l’enregistrement des tâches en arrière-plan et pour voir la définition de la méthode **RegisterBackgroundTask()** dans l’exemple de code ci-dessous, voir [enregistrement d’une tâche en arrière-plan](register-a-background-task.md).

> [!IMPORTANT]
> Pour les tâches en arrière-plan qui s’exécutent dans le même processus que votre application, ne définissez pas `entryPoint`. Pour les tâches en arrière-plan qui s’exécutent dans un processus distinct à partir de votre application, définissez `entryPoint` à l’espace de noms «.» et le nom de la classe qui contient votre implémentation de la tâche en arrière-plan.

```cs
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example hourly background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example hourly background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example hourly background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Une erreur est retournée si l’un des paramètres d’inscription n’est pas valide. Vérifiez que votre application gère de manière fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

## <a name="manage-resources-for-your-background-task"></a>Gestion des ressources pour votre tâche de fond

Utilisez la méthode [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) pour déterminer si l’utilisateur a opté pour une activité limitée de votre application en arrière-plan. Tenez compte du taux d’utilisation de la batterie; exécutez l’application en arrière-plan uniquement lorsqu’elle est nécessaire dans le cadre d’une action souhaitée par l’utilisateur. Voir l' [activité d’arrière-plan optimiser](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) pour plus d’informations sur les utilisateurs moyens peuvent contrôler les paramètres pour l’activité en arrière-plan.

- Mémoire: Réglage utilisation de mémoire et de l’énergie de votre application est essentielle pour garantir que le système d’exploitation permettra exécuter votre tâche en arrière-plan. Utilisez les [API de gestion de mémoire](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) pour afficher la quantité de mémoire à l’aide de votre tâche en arrière-plan. Plus de mémoire votre tâche en arrière-plan utilise, il est difficile pour le système d’exploitation afin qu’il fonctionne lorsqu’une autre application est au premier plan. L’utilisateur dispose d’un contrôle étroit sur l’ensemble des activités en arrière-plan que votre application peut exécuter, et bénéficie d’une visibilité étendue sur l’impact de cette dernière sur le taux d’utilisation de la batterie.  
- Temps processeur: tâches en arrière-plan sont limités par la quantité d’utilisation horloge murale ils obtient basées sur le type de déclencheur.

Pour plus d’informations sur les contraintes de ressource appliquées aux tâches en arrière-plan, consultez [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

## <a name="remarks"></a>Remarques

En commençant par 10 Windows, il n’est plus nécessaire pour l’utilisateur ajouter votre application à l’écran de verrouillage afin d’utiliser des tâches en arrière-plan.

Une tâche en arrière-plan ne s’exécute à l’aide d’un **TimeTrigger** si vous avez appelé [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) tout d’abord.

## <a name="related-topics"></a>Rubriques associées

* [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Exemple de code de tâche de fond](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md)
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Libérer de la mémoire quand l’application bascule en arrière-plan](reduce-memory-usage.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications UWP (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Reporter la suspension d’une application avec l’exécution étendue](run-minimized-with-extended-execution.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
