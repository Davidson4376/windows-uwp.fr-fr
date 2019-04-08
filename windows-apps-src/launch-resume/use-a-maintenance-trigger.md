---
title: Utiliser un déclencheur de maintenance
description: Découvrez comment utiliser la classe MaintenanceTrigger pour exécuter du code léger en arrière-plan tandis que l’appareil est branché.
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 53107ca6add4193737ab0d00497bbe6324bee44f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661854"
---
# <a name="use-a-maintenance-trigger"></a>Utiliser un déclencheur de maintenance

**API importantes**

- [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
- [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

Découvrez comment utiliser la classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) pour exécuter du code léger en arrière-plan tandis que l’appareil est branché.

## <a name="create-a-maintenance-trigger-object"></a>Créer un objet déclencheur de maintenance

Cet exemple suppose que vous disposez d’un code léger à exécuter en arrière-plan qui vous permettra d’améliorer votre application pendant que l’appareil est branché. Cette rubrique porte sur le déclencheur [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517), qui est semblable au déclencheur [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

Des informations supplémentaires sur l’écriture d’une classe de tâche en arrière-plan sont disponibles dans les rubriques [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md) ou [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md).

Créez un objet [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517). Le deuxième paramètre, *OneShot*, indique si la tâche de maintenance s’exécute une seule fois ou régulièrement. Si *OneShot* est défini sur true, le premier paramètre (*FreshnessTime*) indique le nombre de minutes à attendre avant de planifier la tâche en arrière-plan. Si *OneShot* est défini sur false, *FreshnessTime* indique la fréquence d’exécution de la tâche en arrière-plan.

> [!NOTE]
> Si *FreshnessTime* a la valeur inférieure à 15 minutes, une exception est levée lorsque vous tentez d’enregistrer la tâche en arrière-plan.

Cet exemple de code crée un déclencheur qui exécute une fois par heure.

```csharp
uint waitIntervalMinutes = 60;
MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
```

```cppwinrt
uint32_t waitIntervalMinutes{ 60 };
Windows::ApplicationModel::Background::MaintenanceTrigger taskTrigger{ waitIntervalMinutes, false };
```

```cpp
unsigned int waitIntervalMinutes = 60;
MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
```

## <a name="optional-add-a-condition"></a>(Facultatif) Ajouter une condition

- Si besoin est, créez une condition de tâche en arrière-plan afin de contrôler le moment où la tâche est exécutée. Une condition empêche votre tâche en arrière-plan de s’exécuter tant que la condition n’est pas satisfaite. Pour plus d’informations, voir [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

Dans cet exemple, la condition est définie sur **InternetAvailable**, de sorte que la maintenance s’exécute quand Internet est accessible (ou le devient). Pour obtenir la liste des conditions de tâche en arrière-plan possibles, voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

Le code suivant ajoute une condition au générateur de tâches de maintenance :

```csharp
SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition exampleCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan

- Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire. Pour plus d’informations sur l’inscription des tâches en arrière-plan, voir [Inscrire une tâche en arrière-plan](register-a-background-task.md).

Le code suivant inscrit la tâche de maintenance. Notez qu’il part du principe que votre tâche en arrière-plan s’exécute dans un processus distinct de celui de votre application, car il spécifie `entryPoint`. Si votre tâche en arrière-plan s’exécute dans le même processus que votre application, vous ne devez pas spécifier `entryPoint`.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Maintenance background task example";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Maintenance background task example" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Maintenance background task example";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

> [!NOTE]
> Pour toutes les familles d’appareils, à l’exception des ordinateurs de bureau, les tâches en arrière-plan peuvent être arrêtées en cas de mémoire insuffisante de l’appareil. Si aucune exception de mémoire insuffisante n’est exposée ou si l’application ne la gère pas, la tâche en arrière-plan est alors arrêtée sans avertissement ni déclenchement de l’événement OnCanceled. Cela permet de garantir l’expérience utilisateur de l’application au premier plan. Votre tâche en arrière-plan doit être conçue de manière à gérer ce scénario.

> [!NOTE]
> Les applications universelles Windows Platform doivent appeler [ **RequestAccessAsync** ](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire les types de déclencheur en arrière-plan.

Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après une mise à jour, vous devez appeler [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471), puis [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) lorsque votre application est lancée après avoir été mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

> [!NOTE]
> Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Si l’un des paramètres d’inscription n’est pas valide, une erreur est renvoyée. Vérifiez que votre application gère de manière fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

## <a name="related-topics"></a>Rubriques connexes

* [Créez et inscrivez une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Superviser la progression et l’exécution des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Recommandations relatives aux tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher suspendre, reprendre, événements et d’arrière-plan dans les applications UWP (lors du débogage)](https://go.microsoft.com/fwlink/p/?linkid=254345)
