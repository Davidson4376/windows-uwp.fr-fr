---
author: TylerMSFT
title: Porter une tâche en arrière-plan hors processus vers une tâche en arrière-plan in-process
description: Porter une tâche en arrière-plan out-of-process en une tâche en arrière-plan in-process qui s’exécute dans le processus de votre application au premier plan.
ms.author: twhitney
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tâche en arrière-plan, le service d’application
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: b9010f82b0460bd46757bc1e0d58c01dec459104
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4386420"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Porter une tâche en arrière-plan hors processus vers une tâche en arrière-plan in-process

Le moyen le plus simple de porter votre activité d’en arrière-plan out-of-process (OOP) in-process activité consiste à importer votre code de méthode [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) à l’intérieur de votre application et lancer à partir de [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). La technique décrite ici n’est pas sur la création d’un shim à partir d’une tâche en arrière-plan OOP en une tâche en arrière-plan in-process; sa réécriture sur (ou portage) une version OOP vers une version in-process.

Si votre application comporte plusieurs tâches en arrière-plan, [l’exemple d’activation en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) montre comment vous pouvez utiliser `BackgroundActivatedEventArgs.TaskInstance.Task.Name` pour identifier la tâche qui est lancée.

Si vous êtes en train de communiquer entre les processus en arrière-plan et au premier plan, vous pouvez supprimer ce code de communication et de gestion de l’état.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Types de déclencheur et tâches en arrière-plan qui ne peuvent pas être convertis

* Les tâches en arrière-plan intégrées au processus ne prennent pas en charge l’activation d’une tâche VoIP en arrière-plan.
* Les tâches en arrière-plan in-process ne prennent pas en charge les déclencheurs suivants: [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) et **IoTStartupTask**.
