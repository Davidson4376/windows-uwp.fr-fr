---
title: Déclencher une tâche en arrière-plan à partir de votre application
description: Décrit comment déclencher une tâche en arrière-plan à partir d’une application
ms.date: 07/06/2018
ms.topic: article
keywords: déclencheur de tâche en arrière-plan, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: 02e4bf3d7977c9bdd675f264a37e608a5082ef4c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608094"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Déclencher une tâche en arrière-plan à partir de votre application

Découvrez comment utiliser [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) pour activer une tâche en arrière-plan à partir de votre application.

Pour obtenir un exemple montrant comment créer un déclencheur d’Application, consultez ce [exemple](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

Cette rubrique suppose que vous disposez d’une tâche en arrière-plan que vous souhaitez activer à partir de votre application. Si vous ne disposez pas encore de tâche en arrière-plan, voici un exemple de tâche en arrière-plan : [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). Vous pouvez aussi consulter [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md) pour en créer une.

## <a name="why-use-an-application-trigger"></a>Pourquoi utiliser un déclencheur d’application

Utilisez un déclencheur **ApplicationTrigger** pour exécuter du code dans un processus distinct de l’application au premier plan. Un déclencheur **ApplicationTrigger** convient si votre application possède une tâche qui doit être effectuée en arrière-plan, même si l’utilisateur ferme l’application au premier plan. Si la tâche en arrière-plan doit s’arrêter lorsque l’application est fermée, ou doit être liée à l’état du processus au premier plan, utilisez plutôt [Exécution étendue](run-minimized-with-extended-execution.md).

## <a name="create-an-application-trigger"></a>Créer un déclencheur d’application

Créez un déclencheur [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Vous pouvez le stocker dans un champ comme dans l’extrait de code ci-dessous. Cela vous évitera d’avoir à créer une nouvelle instance plus tard lorsque vous souhaiterez signaler le déclencheur. Vous pouvez toutefois utiliser n’importe quelle instance **ApplicationTrigger** pour signaler le déclencheur.

```csharp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
_AppTrigger = new ApplicationTrigger();
```

```cppwinrt
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
Windows::ApplicationModel::Background::ApplicationTrigger _AppTrigger;
```

```cpp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
ApplicationTrigger ^ _AppTrigger = ref new ApplicationTrigger();
```

## <a name="optional-add-a-condition"></a>(Facultatif) Ajouter une condition

Vous pouvez créer une condition de tâche en arrière-plan pour contrôler le moment où la tâche est exécutée. La tâche en arrière-plan ne démarrera pas tant que la condition spécifiée n’aura pas été satisfaite. Pour plus d’informations, consultez [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

Dans cet exemple, la condition est définie sur **InternetAvailable** afin que, une fois déclenché, la tâche s’exécute uniquement une fois que l’accès à internet est disponible. Pour obtenir la liste des conditions possibles, voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable)
```

Pour des informations plus détaillées sur les conditions et les types de déclencheurs en arrière-plan, consultez [Ajouter des tâches en arrière-plan à votre application](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Appeler RequestAccessAsync()

Avant d’inscrire la tâche en arrière-plan **ApplicationTrigger**, appelez [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) pour déterminer le niveau d’activité en arrière-plan que permet l’utilisateur, car l’utilisateur a peut-être désactivé l’activité en arrière-plan pour votre application. Consultez [Optimiser l’activité en arrière-plan](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) pour plus d’informations sur les méthodes permettant aux utilisateurs de contrôler les paramètres de l’activité en arrière-plan.

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan

Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire. Pour plus d’informations sur l’inscription des tâches en arrière-plan et pour voir la définition de la méthode **RegisterBackgroundTask()** utilisée dans l’exemple de code ci-dessous, consultez [Inscrire une tâche en arrière-plan](register-a-background-task.md).

Si vous prévoyez d’utiliser un déclencheur d’application pour étendre la durée de vie de votre processus au premier plan, envisagez d’utiliser [Exécution étendue](run-minimized-with-extended-execution.md) à la place. Le déclencheur d’application est conçu pour créer un processus hébergé séparément pour effectuer des tâches. L’extrait de code suivant inscrit un déclencheur en arrière-plan hors processus.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example application trigger";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example application trigger" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example application trigger";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Si l’un des paramètres d’inscription n’est pas valide, une erreur est renvoyée. Vérifiez que votre application gère de manière fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

## <a name="trigger-the-background-task"></a>Déclencher la tâche en arrière-plan

Avant de déclencher la tâche en arrière-plan, utilisez [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) pour vérifier que la tâche est inscrite. Le lancement de l’application est un moment idéal pour vérifier que toutes les tâches en arrière-plan sont inscrites.

Déclenchez la tâche en arrière-plan en appelant [ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger). N’importe quelle instance **ApplicationTrigger** fera l’affaire.

Notez que **ApplicationTrigger.RequestAsync** ne peut pas être appelée à partir de la tâche en arrière-plan elle-même, ou lorsque l’application est en cours d’exécution en arrière-plan (consultez [Cycle de vie de l’application](app-lifecycle.md) pour plus d’informations sur les états d’application).
Elle peut retourner [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) si l’utilisateur a défini des stratégies de confidentialité et d’énergie qui empêchent l’application d’exécuter des tâches en arrière-plan.
En outre, un seul AppTrigger peut être exécuté à la fois. Si vous essayez d’exécuter un AppTrigger tandis qu’un autre est déjà en cours d’exécution, la fonction renverra [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult).

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Gérer les ressources pour votre tâche en arrière-plan

Utilisez la méthode [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) pour déterminer si l’utilisateur a opté pour une activité limitée de votre application en arrière-plan. Tenez compte du taux d’utilisation de la batterie ; exécutez l’application en arrière-plan uniquement lorsqu’elle est nécessaire dans le cadre d’une action souhaitée par l’utilisateur. Consultez [Optimiser l’activité en arrière-plan](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) pour plus d’informations sur les méthodes permettant aux utilisateurs de contrôler les paramètres de l’activité en arrière-plan.  

- Mémoire : Le réglage d’utilisation de mémoire et d’énergie de votre application est indispensable pour garantir que le système d’exploitation permettra l’exécution de votre tâche en arrière-plan. Utilisez les [API de gestion de la mémoire](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) pour déterminer la quantité de mémoire utilisée par votre tâche en arrière-plan. Plus votre tâche en arrière-plan utilise de mémoire, plus le système d’exploitation a des difficultés à assurer son exécution lorsqu’une autre application se trouve au premier plan. L’utilisateur dispose d’un contrôle étroit sur l’ensemble des activités en arrière-plan que votre application peut exécuter, et bénéficie d’une visibilité étendue sur l’impact de cette dernière sur le taux d’utilisation de la batterie.  
- Temps processeur : Tâches en arrière-plan sont limitées par la quantité de temps de l’utilisation d’horloge qu'elles obtient basées sur le type de déclencheur. Les tâches en arrière-plan déclenchées par le déclencheur d’application sont limitées à 10 minutes environ.

Pour plus d’informations sur les contraintes de ressource appliquées aux tâches en arrière-plan, consultez [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

## <a name="remarks"></a>Notes

À compter de Windows 10, il n’est plus nécessaire pour l’utilisateur à ajouter votre application à l’écran de verrouillage pour pouvoir utiliser les tâches en arrière-plan.

Une tâche en arrière-plan s’exécutera à l’aide d’un **ApplicationTrigger** uniquement si vous avez appelé [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485).

## <a name="related-topics"></a>Rubriques connexes

* [Instructions pour les tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Exemple de code de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Créez et inscrivez une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
* [Créer et inscrire une tâche en arrière-plan out-of-process](create-and-register-a-background-task.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste d’application](declare-background-tasks-in-the-application-manifest.md)
* [Mémoire disponible si votre application est déplacée vers l’arrière-plan](reduce-memory-usage.md)
* [Gérer une tâche en arrière-plan annulé](handle-a-cancelled-background-task.md)
* [Comment déclencher suspendre, reprendre, événements et d’arrière-plan dans les applications UWP (lors du débogage)](https://go.microsoft.com/fwlink/p/?linkid=254345)
* [Surveiller la progression des tâches en arrière-plan et saisie semi-automatique](monitor-background-task-progress-and-completion.md)
* [Reporter la suspension d’application avec l’exécution étendue](run-minimized-with-extended-execution.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour d’une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
