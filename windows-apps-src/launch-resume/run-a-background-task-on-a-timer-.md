---
title: Exécuter une tâche en arrière-plan en fonction d’un minuteur
description: Découvrez comment planifier une tâche en arrière-plan unique ou exécuter une tâche en arrière-plan périodique.
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, uwp, les tâches en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: 46d2b5704fa8a9bf53534ded98647ce57da0f520
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661894"
---
# <a name="run-a-background-task-on-a-timer"></a>Exécuter une tâche en arrière-plan en fonction d’un minuteur

Découvrez comment utiliser le [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) pour planifier une tâche en arrière-plan unique ou exécuter une tâche en arrière-plan périodique.

Voir **Scenario4** dans [l’exemple d’activation en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation) pour voir comment implémenter la tâche en arrière-plan déclenchée sur des critères de date et d’heure décrite dans cette rubrique.

Cette rubrique suppose que vous disposez d’une tâche en arrière-plan qui doit être exécutée régulièrement, ou à une heure spécifique, pour prendre en charge votre application. Si vous ne disposez pas encore de tâche en arrière-plan, voici un exemple de tâche en arrière-plan : [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). Sinon, suivez les étapes de la rubrique [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md) ou [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md) pour en créer une.

## <a name="create-a-time-trigger"></a>Créer un déclencheur de temps

Créez un [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). Le second paramètre, *OneShot*, indique si la tâche en arrière-plan s’exécute une seule fois ou régulièrement. Si *OneShot* est défini sur true, le premier paramètre (*FreshnessTime*) indique le nombre de minutes à attendre avant de planifier la tâche en arrière-plan. Si *OneShot* est défini sur false, le paramètre *FreshnessTime* indique la fréquence d’exécution de la tâche en arrière-plan.

Le minuteur intégré pour les applications de plateforme Windows universelle (UWP) qui ciblent les appareils mobiles ou de bureau exécute des tâches en arrière-plan par intervalles de 15 minutes. (Le minuteur s’exécute par intervalles de 15 minutes, de sorte que le système doit uniquement sortir de veille toutes les 15 minutes pour sortir de veille les applications qui ont demandé TimerTriggers, ce qui économise l’énergie.)

- Si *FreshnessTime* indique une fréquence de 15 minutes et que *OneShot* est défini sur true, la tâche sera programmée pour être exécutée une seule fois, 15 à 30 minutes après son inscription. Si le paramètre FreshnessTime indique une fréquence de 25 minutes et que *OneShot* est défini sur true, la tâche sera programmée pour être exécutée une seule fois, 25 à 40 minutes après son inscription.

- Si *FreshnessTime* indique une fréquence de 15 minutes et que *OneShot* est défini sur false, la tâche sera programmée pour être exécutée toutes les 15 minutes, 15 à 30 minutes après son inscription. Si FreshnessTime indique une fréquence de n minutes et que *OneShot* est défini sur false, la tâche sera programmée pour être exécutée toutes les n minutes, n à n+15 minutes après son inscription.

> [!NOTE]
> Si *FreshnessTime* a la valeur inférieure à 15 minutes, une exception est levée lorsque vous tentez d’enregistrer la tâche en arrière-plan.
 
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

Vous pouvez créer une condition de tâche en arrière-plan pour contrôler le moment où la tâche est exécutée. La tâche en arrière-plan ne démarrera pas tant que la condition spécifiée n’aura pas été satisfaite. Pour plus d’informations, consultez [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

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

Pour des informations plus détaillées sur les conditions et les types de déclencheurs en arrière-plan, consultez [Ajouter des tâches en arrière-plan à votre application](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Appeler RequestAccessAsync()

Avant d’inscrire la tâche en arrière-plan **ApplicationTrigger**, appelez [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) pour déterminer le niveau d’activité en arrière-plan que permet l’utilisateur, car l’utilisateur a peut-être désactivé l’activité en arrière-plan pour votre application. Consultez [Optimiser l’activité en arrière-plan](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) pour plus d’informations sur les méthodes permettant aux utilisateurs de contrôler les paramètres de l’activité en arrière-plan.

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan

Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire. Pour plus d’informations sur l’inscription des tâches en arrière-plan et pour voir la définition de la méthode **RegisterBackgroundTask()** utilisée dans l’exemple de code ci-dessous, consultez [Inscrire une tâche en arrière-plan](register-a-background-task.md).

> [!IMPORTANT]
> Des tâches en arrière-plan, qui s’exécutent dans le même processus que votre application, ne définissez pas `entryPoint`. Des tâches en arrière-plan, qui s’exécutent dans un processus séparé à partir de votre application, définissez `entryPoint` à l’espace de noms, «. » et le nom de la classe qui contient votre implémentation de la tâche en arrière-plan.

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

Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Si l’un des paramètres d’inscription n’est pas valide, une erreur est renvoyée. Vérifiez que votre application gère de manière fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

## <a name="manage-resources-for-your-background-task"></a>Gérer les ressources pour votre tâche en arrière-plan

Utilisez la méthode [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) pour déterminer si l’utilisateur a opté pour une activité limitée de votre application en arrière-plan. Tenez compte du taux d’utilisation de la batterie ; exécutez l’application en arrière-plan uniquement lorsqu’elle est nécessaire dans le cadre d’une action souhaitée par l’utilisateur. Consultez [Optimiser l’activité en arrière-plan](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) pour plus d’informations sur les méthodes permettant aux utilisateurs de contrôler les paramètres de l’activité en arrière-plan.

- Mémoire : Le réglage d’utilisation de mémoire et d’énergie de votre application est indispensable pour garantir que le système d’exploitation permettra l’exécution de votre tâche en arrière-plan. Utilisez les [API de gestion de la mémoire](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) pour déterminer la quantité de mémoire utilisée par votre tâche en arrière-plan. Plus votre tâche en arrière-plan utilise de mémoire, plus le système d’exploitation a des difficultés à assurer son exécution lorsqu’une autre application se trouve au premier plan. L’utilisateur dispose d’un contrôle étroit sur l’ensemble des activités en arrière-plan que votre application peut exécuter, et bénéficie d’une visibilité étendue sur l’impact de cette dernière sur le taux d’utilisation de la batterie.  
- Temps processeur : Tâches en arrière-plan sont limitées par la quantité de temps de l’utilisation d’horloge qu'elles obtient basées sur le type de déclencheur.

Pour plus d’informations sur les contraintes de ressource appliquées aux tâches en arrière-plan, consultez [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

## <a name="remarks"></a>Notes

À compter de Windows 10, il n’est plus nécessaire pour l’utilisateur à ajouter votre application à l’écran de verrouillage pour pouvoir utiliser les tâches en arrière-plan.

Une tâche en arrière-plan ne s’exécutera qu’à l’aide d’un **TimeTrigger** si vous avez d’abord appelé [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485).

## <a name="related-topics"></a>Rubriques connexes

* [Recommandations relatives aux tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Exemple de code de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md)
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Libérer de la mémoire quand l’application passe en arrière-plan](reduce-memory-usage.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Comment déclencher suspendre, reprendre, événements et d’arrière-plan dans les applications UWP (lors du débogage)](https://go.microsoft.com/fwlink/p/?linkid=254345)
* [Superviser la progression et l’exécution des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Reporter la suspension d’une application avec l’exécution étendue](run-minimized-with-extended-execution.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
