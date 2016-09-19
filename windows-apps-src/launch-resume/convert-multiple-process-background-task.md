---
author: TylerMSFT
title: "Convertir une tâche en arrière-plan à plusieurs processus en tâche en arrière-plan à processus unique"
description: "Convertissez une tâche en arrière-plan qui s’exécute dans un processus distinct en tâche en arrière-plan qui s’exécute dans votre processus d’application au premier plan."
translationtype: Human Translation
ms.sourcegitcommit: 2c34ca40d3c930254500477ab5a2e41e5206d823
ms.openlocfilehash: e342667347cf3b89a5aa193495cbf7195263b276

---

# Convertir une tâche en arrière-plan à plusieurs processus en tâche en arrière-plan à processus unique

Le moyen le plus simple de convertir votre activité en arrière-plan à plusieurs processus en processus unique consiste à insérer votre code de méthode [IBackgroundTask.Run](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) dans votre application et de la lancer depuis [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Si votre application comporte plusieurs tâches en arrière-plan, [l’exemple d’activation en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) montre comment vous pouvez utiliser `BackgroundActivatedEventArgs.TaskInstance.Task.Name` pour identifier la tâche qui est lancée.

Si vous êtes en train de communiquer entre les processus en arrière-plan et au premier plan, vous pouvez supprimer ce code de communication et de gestion de l’état.

## Les types de déclencheur et les tâches en arrière-plan qui ne peuvent pas être convertis

* Les tâches en arrière-plan à processus unique ne prennent pas en charge l’activation d’une tâche VoIP en arrière-plan.
* Les tâches en arrière-plan à processus unique processus ne prennent pas en charge les déclencheurs suivants: [DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) et **IoTStartupTask**.



<!--HONumber=Aug16_HO3-->


