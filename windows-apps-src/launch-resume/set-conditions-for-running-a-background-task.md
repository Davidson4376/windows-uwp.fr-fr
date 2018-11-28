---
title: Définir des conditions pour exécuter une tâche en arrière-plan
description: Découvrez comment définir des conditions qui contrôlent le moment auquel votre tâche en arrière-plan s’exécutera.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, uwp, tâche d’arrière-plan
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: ac6dd17f31dab1898aa394f901613d268c159b06
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7839847"
---
# <a name="set-conditions-for-running-a-background-task"></a>Définir des conditions pour exécuter une tâche en arrière-plan

**API importantes**

- [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
- [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

Découvrez comment définir des conditions spécifiant à quel moment votre tâche en arrière-plan s’exécutera.

Parfois, les tâches en arrière-plan doit remplir certaines conditions pour être remplies pour que la tâche en arrière-plan réussisse. Vous pouvez spécifier une ou plusieurs des conditions spécifiées par [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) au moment d’inscrire votre tâche en arrière-plan. La condition est vérifiée une fois que le déclencheur est activé. La tâche en arrière-plan sera alors être en file d’attente, mais ne s’exécute jusqu'à ce que toutes les conditions requises sont satisfaites.

Affectation de conditions aux tâches en arrière-plan permet d’économiser l’autonomie de la batterie et le processeur en empêchant toute exécution inutile des tâches. Par exemple, si votre tâche en arrière-plan est exécutée sur un minuteur et nécessite une connectivité Internet, ajoutez la condition **InternetAvailable** au [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) avant d’inscrire la tâche. Cela empêchera ainsi la tâche de faire inutilement appel aux ressources système et à l’autonomie de la batterie. Elle s’exécutera uniquement une fois que le minuteur sera arrivé à expiration *et* qu’Internet sera accessible.

Il est également possible de combiner plusieurs conditions en appelant **AddCondition** plusieurs fois sur le même [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Veillez à ne pas ajouter de conditions conflictuelles, telles que **UserPresent** et **UserNotPresent**.

## <a name="create-a-systemcondition-object"></a>Créer un objet SystemCondition

Cette rubrique suppose qu’une tâche en arrière-plan est déjà associée à votre application et que cette dernière comporte déjà du code qui crée un objet [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) nommé **taskBuilder**.  Consultez [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md) ou [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md) si vous devez commencer par créer une tâche en arrière-plan.

Cette rubrique concerne aussi bien les tâches en arrière-plan qui s’exécutent hors processus que celles qui s’exécutent dans le même processus que l’application au premier plan.

Avant d’ajouter la condition, créez un objet [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) pour représenter la condition qui doit être en vigueur pour exécuter une tâche en arrière-plan. Dans le constructeur, spécifiez la condition qui doit être remplie avec une valeur d’énumération [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) .

Le code suivant crée un objet [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) qui spécifie la condition **InternetAvailable** :

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="add-the-systemcondition-object-to-your-background-task"></a>Ajouter l’objet SystemCondition à votre tâche en arrière-plan

Pour ajouter la condition, appelez la méthode [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) sur l’objet [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) et transmettez-lui l’objet [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834).

Le code suivant utilise **taskBuilder** pour ajouter la condition **InternetAvailable** .

```csharp
taskBuilder.AddCondition(internetCondition);
```

```cppwinrt
taskBuilder.AddCondition(internetCondition);
```

```cpp
taskBuilder->AddCondition(internetCondition);
```

## <a name="register-your-background-task"></a>Inscrire votre tâche en arrière-plan

Maintenant, vous pouvez inscrire votre tâche en arrière-plan avec la méthode [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) , et la tâche en arrière-plan ne démarrera pas tant que la condition spécifiée est remplie.

Le code suivant inscrit la tâche et stocke l’objet BackgroundTaskRegistration obtenu:

```csharp
BackgroundTaskRegistration task = taskBuilder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
BackgroundTaskRegistration ^ task = taskBuilder->Register();
```

> [!NOTE]
> Les applications Windows universelles doivent appeler l’élément [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire n’importe quel type de déclencheur en arrière-plan.

Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, vous devez appeler [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471), puis [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) lorsque votre application est lancée après avoir été mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

> [!NOTE]
> Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Une erreur est retournée si l’un des paramètres d’inscription n’est pas valide. Vérifiez que votre application gère de façon fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

## <a name="place-multiple-conditions-on-your-background-task"></a>Placer plusieurs conditions dans la tâche en arrière-plan

Pour ajouter plusieurs conditions, votre application effectue plusieurs appels à la méthode [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769). Pour être effectifs, ces appels doivent intervenir avant l’inscription de la tâche.

> [!NOTE]
> Veillez à ne pas pour ajouter de conditions conflictuelles à une tâche en arrière-plan.

L’extrait de code suivant présente plusieurs conditions dans le contexte de création et d’inscription d’une tâche en arrière-plan.

```csharp
// Set up the background task.
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);

var recurringTaskBuilder = new BackgroundTaskBuilder();

recurringTaskBuilder.Name           = "Hourly background task";
recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.

BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```

```cppwinrt
// Set up the background task.
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };

Windows::ApplicationModel::Background::BackgroundTaskBuilder recurringTaskBuilder;

recurringTaskBuilder.Name(L"Hourly background task");
recurringTaskBuilder.TaskEntryPoint(L"Tasks.ExampleBackgroundTaskClass");
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
// Set up the background task.
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);

auto recurringTaskBuilder = ref new BackgroundTaskBuilder();

recurringTaskBuilder->Name           = "Hourly background task";
recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder->SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);

recurringTaskBuilder->AddCondition(userCondition);
recurringTaskBuilder->AddCondition(internetCondition);

// Done adding conditions, now register the background task.
BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## <a name="remarks"></a>Remarques

> [!NOTE]
> Choisissez les conditions appropriées pour votre tâche en arrière-plan afin qu’elle s’exécute uniquement lorsque cela est nécessaire et non quand il ne doit pas. Voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) pour obtenir une description des différentes conditions de tâche en arrière-plan.

## <a name="related-topics"></a>Rubriques associées

* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications UWP (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)
