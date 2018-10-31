---
author: TylerMSFT
title: Déclencher une tâche en arrière-plan à partir de votre application
description: Décrit comment déclencher une tâche en arrière-plan à partir d’une application
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
keywords: déclencheur de tâche en arrière-plan, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: 6846cfe77272a78eff7ddc05c9a7e48dddd21fc2
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5814935"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Déclencher une tâche en arrière-plan à partir de votre application

Découvrez comment utiliser [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) pour activer une tâche en arrière-plan à partir de votre application.

Pour obtenir un exemple montrant comment créer un déclencheur d’Application, consultez cet [exemple](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

Cette rubrique suppose que vous disposez d’une tâche en arrière-plan que vous souhaitez activer à partir de votre application. Si vous n’avez pas encore une tâche en arrière-plan, il est un exemple de tâche en arrière-plan à [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). Sinon, suivez les étapes [créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md) en créer un.

## <a name="why-use-an-application-trigger"></a>Pourquoi utiliser un déclencheur d’application

Utilisez un **ApplicationTrigger** pour exécuter du code dans un processus distinct à partir de l’application au premier plan. Un **ApplicationTrigger** est approprié si votre application dispose de travail qui doit être effectué en arrière-plan, même si l’utilisateur ferme l’application au premier plan. Si la tâche en arrière-plan doit s’arrêter lorsque l’application est fermée, ou doit être liée à l’état du processus de premier plan, puis [L’exécution étendue](run-minimized-with-extended-execution.md) doit être utilisé, à la place.

## <a name="create-an-application-trigger"></a>Créer un déclencheur d’application

Créez un nouveau [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Vous pouvez le stocker dans le champ comme c’est le cas dans l’extrait de code ci-dessous. Il s’agit pour des raisons pratiques afin que nous n’êtes pas obligé de créer une nouvelle instance ultérieurement lorsque nous voulons signaler le déclencheur. Toutefois, vous pouvez utiliser n’importe quelle instance **ApplicationTrigger** pour signaler le déclencheur.

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

Vous pouvez créer une condition de tâche en arrière-plan au contrôle lorsque la tâche s’exécute. Une condition empêche la tâche en arrière-plan de s’exécuter tant que la condition est remplie. Pour plus d’informations, voir [définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

Dans cet exemple, que la condition est définie sur **InternetAvailable** afin que, une fois déclenchée, la tâche s’exécute une fois que l’accès à internet n’est disponible. Pour obtenir la liste des conditions possibles, voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

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

Pour plus d’informations sur les conditions et les types de déclencheurs d’arrière-plan, consultez [la prise en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Appeler RequestAccessAsync()

Avant d’inscrire la tâche d’arrière-plan **ApplicationTrigger** , appelez [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) pour déterminer le niveau d’activité en arrière-plan que permet à l’utilisateur dans la mesure où l’utilisateur a peut-être désactivé activité en arrière-plan pour votre application. Reportez-vous à [optimiser l’activité en arrière-plan](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) pour plus d’informations sur les utilisateurs de méthodes peuvent contrôler les paramètres de l’activité en arrière-plan.

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan

Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire. Pour plus d’informations sur l’inscription des tâches en arrière-plan et pour la définition de la méthode **RegisterBackgroundTask()** dans l’exemple de code ci-dessous, consultez [inscrire une tâche en arrière-plan](register-a-background-task.md).

Si vous envisagez d’utiliser un déclencheur d’Application afin de prolonger la durée de vie de votre processus de premier plan, envisagez plutôt d’utiliser [L’exécution étendue](run-minimized-with-extended-execution.md) . Le déclencheur d’Application est conçu pour la création d’un processus hébergé séparément pour effectuer des tâches dans. L’extrait de code suivant inscrit un déclencheur en arrière-plan out-of-process.

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

Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Une erreur est retournée si l’un des paramètres d’inscription n’est pas valide. Vérifiez que votre application gère de manière fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

## <a name="trigger-the-background-task"></a>Déclencher la tâche en arrière-plan

Avant de déclencher la tâche en arrière-plan, utilisez [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) pour vérifier que la tâche en arrière-plan est enregistrée. Du moment idéal pour vérifier que toutes les tâches en arrière-plan sont enregistrés est pendant le lancement de l’application.

Déclencher la tâche en arrière-plan en appelant la méthode [ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger). N’importe quelle instance **ApplicationTrigger** fera.

Notez que **ApplicationTrigger.RequestAsync** ne peut pas être appelée à partir de la tâche en arrière-plan elle-même, ou lorsque l’application est en arrière-plan en cours d’exécution état (voir le [cycle de vie d’application](app-lifecycle.md) pour plus d’informations sur les États de l’application).
Elle peut retourner [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) si l’utilisateur a défini des stratégies d’énergie ou de confidentialité qui empêchent l’exécution d’activité en arrière-plan de l’application.
En outre, AppTrigger qu’un seul peut s’exécuter à la fois. Si vous essayez d’exécuter un AppTrigger tandis que l’autre est déjà en cours d’exécution, la fonction retourne [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult).

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Gérer des ressources pour votre tâche en arrière-plan

Utilisez la méthode [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) pour déterminer si l’utilisateur a opté pour une activité limitée de votre application en arrière-plan. Tenez compte du taux d’utilisation de la batterie; exécutez l’application en arrière-plan uniquement lorsqu’elle est nécessaire dans le cadre d’une action souhaitée par l’utilisateur. Reportez-vous à [optimiser l’activité en arrière-plan](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) pour plus d’informations sur les utilisateurs de méthodes peuvent contrôler les paramètres de l’activité en arrière-plan.  

- Memory: Réglage utilisation d’énergie et de la mémoire de votre application est essentiel pour vérifier que le système d’exploitation va autoriser votre tâche en arrière-plan à s’exécuter. Utilisez les [API de gestion de mémoire](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) pour voir la quantité de mémoire à l’aide de votre tâche en arrière-plan. Plus votre tâche en arrière-plan utilise de mémoire, il est difficile pour le système d’exploitation poursuivre son exécution lorsqu’une autre application est au premier plan. L’utilisateur dispose d’un contrôle étroit sur l’ensemble des activités en arrière-plan que votre application peut exécuter, et bénéficie d’une visibilité étendue sur l’impact de cette dernière sur le taux d’utilisation de la batterie.  
- Temps processeur: les tâches en arrière-plan sont limitées par la quantité de temps horloge utilisation en fonction de leur type de déclencheur. Tâches en arrière-plan déclenchées par le déclencheur d’Application sont limités à environ 10 minutes.

Pour plus d’informations sur les contraintes de ressource appliquées aux tâches en arrière-plan, consultez [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

## <a name="remarks"></a>Remarques

À compter de Windows 10, il n’est plus nécessaire à l’utilisateur d’ajouter votre application à l’écran de verrouillage pour utiliser des tâches en arrière-plan.

Une tâche en arrière-plan s’exécutent uniquement à l’aide d’un **ApplicationTrigger** si vous avez appelé [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) tout d’abord.

## <a name="related-topics"></a>Rubriquesconnexes

* [Recommandations pour les tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Exemple de code de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Créez et inscrivez une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Libérer de la mémoire lorsque l’application bascule en arrière-plan](reduce-memory-usage.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications UWP (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Reporter la suspension d’une application avec l’exécution étendue](run-minimized-with-extended-execution.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
