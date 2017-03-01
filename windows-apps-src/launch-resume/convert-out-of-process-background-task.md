---
author: TylerMSFT
title: "Convertir une tâche en arrière-plan hors processus en une tâche en arrière-plan intégrée au processus"
description: "Convertissez une tâche en arrière-plan hors processus en une tâche en arrière-plan intégrée au processus qui s’exécute à l’intérieur du processus de votre application au premier plan."
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: f67d3ea2293e50a04bdbb4277fa4ad9e46834473
ms.lasthandoff: 02/08/2017

---

# <a name="convert-an-out-of-process-background-task-to-an-in-process-background-task"></a>Convertir une tâche en arrière-plan hors processus en une tâche en arrière-plan intégrée au processus

Le moyen le plus simple de convertir votre activité en arrière-plan hors processus en activité intégrée au processus consiste à insérer votre code de méthode [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) dans votre application et de la lancer depuis [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Si votre application comporte plusieurs tâches en arrière-plan, [l’exemple d’activation en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) montre comment vous pouvez utiliser `BackgroundActivatedEventArgs.TaskInstance.Task.Name` pour identifier la tâche qui est lancée.

Si vous êtes en train de communiquer entre les processus en arrière-plan et au premier plan, vous pouvez supprimer ce code de communication et de gestion de l’état.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Types de déclencheur et tâches en arrière-plan qui ne peuvent pas être convertis

* Les tâches en arrière-plan intégrées au processus ne prennent pas en charge l’activation d’une tâche VoIP en arrière-plan.
* Les tâches en arrière-plan in-process ne prennent pas en charge les déclencheurs suivants : [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) et **IoTStartupTask**.

