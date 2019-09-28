---
title: Déboguer une tâche en arrière-plan
description: Découvrez comment déboguer une tâche en arrière-plan, notamment dans le cadre de son activation et du suivi de débogage dans le journal des événements Windows.
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: c337de2a1fc349cfa3965402848fbead51e61e83
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340474"
---
# <a name="debug-a-background-task"></a>Déboguer une tâche en arrière-plan


**API importantes**
-   [Windows.ApplicationModel.Background](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)

Découvrez comment déboguer une tâche en arrière-plan, notamment dans le cadre de son activation et du suivi de débogage dans le journal des événements Windows.

## <a name="debugging-out-of-process-vs-in-process-background-tasks"></a>Déboguer des tâches en arrière-plan hors processus ou in-process
Cette rubrique concerne principalement les tâches en arrière-plan qui s’exécutent dans un processus distinct que celui de l’application hôte. Si vous déboguez une tâche en arrière-plan in-process, vous n’avez pas de projet de tâche en arrière-plan distinct. Vous pouvez définir un point d’arrêt sur **OnBackgroundActivated()** (où votre code en arrière-plan in-process s’exécute) et consulter l’étape 2 dans la section [Déclencher des tâches en arrière-plan manuellement pour déboguer le code de la tâche en arrière-plan](#trigger-background-tasks-manually-to-debug-background-task-code), pour obtenir des instructions sur le déclenchement de votre code en arrière-plan à exécuter.

## <a name="make-sure-the-background-task-project-is-set-up-correctly"></a>Vérifier que le projet de tâche en arrière-plan est configuré comme il se doit

Cette rubrique part du principe que vous disposez d’une application avec une tâche en arrière-plan à déboguer. La suite concerne les tâches en arrière-plan qui s’exécutent hors processus et non les tâches en arrière-plan in-process.

-   En C# et C++, vérifiez que le projet principal fait référence au projet de tâche en arrière-plan. En l’absence de cette référence, la tâche en arrière-plan n’est pas incluse dans le package d’application.
-   En C# et C++, vérifiez que le **Output type** du projet de tâches en arrière-plan est « Composant Windows Runtime ».
-   La classe en arrière-plan doit être déclarée dans l’attribut de point d’entrée dans le manifeste du package.

## <a name="trigger-background-tasks-manually-to-debug-background-task-code"></a>Déclencher des tâches en arrière-plan manuellement pour déboguer le code de la tâche en arrière-plan

Les tâches en arrière-plan peuvent être déclenchées manuellement par le biais de Microsoft Visual Studio. Vous pouvez ensuite exécuter pas à pas le code et le déboguer.

1.  En C#, insérez un point d’arrêt dans la méthode Run de la classe en arrière-plan (pour les tâches en arrière-plan in-process, placez le point d’arrêt dans App.OnBackgroundActivated()) et/ou écrivez la sortie de débogage à l’aide de [**System.Diagnostics**](https://docs.microsoft.com/dotnet/api/system.diagnostics).

    En C++, insérez un point d’arrêt dans la fonction Run de la classe en arrière-plan (pour les tâches en arrière-plan in-process, placez le point d’arrêt dans App.OnBackgroundActivated()) et/ou écrivez la sortie de débogage à l’aide de [**OutputDebugString**](https://docs.microsoft.com/windows/desktop/api/debugapi/nf-debugapi-outputdebugstringw).

2.  Exécutez votre application dans le débogueur, puis déclenchez la tâche en arrière-plan à l’aide de la barre d’outils **Événements de cycle de vie**. Ce menu déroulant affiche le nom des tâches en arrière-plan qu’il est possible d’activer à l’aide de Visual Studio.

> [!NOTE]
> Les options de la barre d’outils événements de cycle de vie ne sont pas affichées par défaut dans Visual Studio. Pour afficher ces options, cliquez avec le bouton droit sur la barre d’outils actuelle dans Visual Studio et assurez-vous que l’option **emplacement de débogage** est activée.

    For this to work, the background task must already be registered and it must still be waiting for the trigger. For example, if a background task was registered with a one-shot TimeTrigger and that trigger has already fired, launching the task through Visual Studio will have no effect.

> [!Note]
> Les tâches en arrière-plan utilisant les déclencheurs suivants ne peuvent pas être activées de cette manière : [**Déclencheur d’application**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger), [**déclencheur MediaProcessing**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger), [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)et tâches en arrière-plan à l’aide d’un [**événement systemtrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) avec le type de déclencheur [**SmsReceived**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) .  
> Les déclencheurs **ApplicationTrigger** et **MediaProcessingTrigger** peuvent être déclarés manuellement dans le code avec `trigger.RequestAsync()`.

![Débogage de tâches en arrière-plan](images/debugging-activation.png)

3.  Lorsque la tâche en arrière-plan est activée, le débogueur s’y associe et affiche la sortie de débogage dans Visual Studio.

## <a name="debug-background-task-activation"></a>Déboguer l’activation de la tâche en arrière-plan

> [!NOTE]
> Cette section concerne les tâches en arrière-plan qui s’exécutent hors processus et non les tâches en arrière-plan in-process.

L’activation de la tâche en arrière-plan dépend de trois éléments :

-   nom et espace de noms de la classe de tâche en arrière-plan ;
-   attribut de point d’entrée spécifié dans le manifeste du package ;
-   point d’entrée spécifié par votre application lors de l’inscription de la tâche en arrière-plan.

1.  Utilisez Visual Studio pour noter le point d’entrée de la tâche en arrière-plan :

    -   En C# et C++, notez le nom et l’espace de noms de la classe de tâche en arrière-plan spécifiée dans le projet de tâche en arrière-plan.

2.  Utilisez le concepteur du manifeste pour vérifier que la tâche en arrière-plan est correctement déclarée dans le manifeste du package :

    -   En C# et C++, l’attribut de point d’entrée doit correspondre à l’espace de noms de la tâche en arrière-plan suivi du nom de la classe. Exemple : RuntimeComponent1.MyBackgroundTask.
    -   Tous les types de déclencheurs utilisés avec la tâche doivent également être indiqués.
    -   Le fichier exécutable NE doit PAS être spécifié sauf si vous utilisez l’objet [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) ou [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger).

3.  Windows uniquement. Pour afficher le point d’entrée utilisé par Windows afin d’activer la tâche en arrière-plan, activez le suivi de débogage et utilisez le journal des événements Windows.

    Si vous suivez cette procédure et que le journal des événements indique un déclencheur ou point d’entrée incorrect pour la tâche en arrière-plan, votre application n’inscrit pas correctement la tâche en arrière-plan. Pour obtenir de l’aide sur cette tâche, voir [Inscrire une tâche en arrière-plan](register-a-background-task.md).

    1.  Ouvrez l’Observateur d’événements en accédant à l’écran de démarrage et en recherchant eventvwr.exe.
    2.   - Accédez à **journaux des applications et des services** &gt; **Microsoft**  - **Windows** BackgroundTaskInfrastructure dans l’observateur d’événements. - &gt; &gt;
    3.  Dans le volet Actions, sélectionnez **Afficher**  - &gt; **afficher les journaux d’analyse et de débogage** pour activer la journalisation des Diagnostics.
    4.  Sélectionnez le **journal de diagnostic**, puis cliquez sur **Activer le journal**.
    5.  Essayez à présent d’utiliser votre application pour inscrire et activer la tâche en arrière-plan une nouvelle fois.
    6.  Consultez les journaux de diagnostic à la recherche d’informations détaillées sur l’erreur. Cela comprend le point d’entrée inscrit pour la tâche en arrière-plan.

![informations de débogage des tâches en arrière-plan dans l’observateur d’événements](images/event-viewer.png)

## <a name="background-tasks-and-visual-studio-package-deployment"></a>Tâches en arrière-plan et déploiement du package Visual Studio

Si vous déployez une application utilisant des tâches en arrière-plan à l’aide de Visual Studio et si vous mettez ensuite à jour la version (majeure et/ou mineure) précisée dans le concepteur du manifeste, le redéploiement de l’application avec Visual Studio qui s’ensuivra peut entraîner un blocage des tâches en arrière-plan de l’application. Vous pouvez remédier à ce problème de la manière suivante.

-   Utilisez Windows PowerShell pour déployer l’application mise à jour (plutôt que Visual Studio). Pour cela, exécutez le script généré avec le package.
-   Si vous avez déjà déployé l’application à l’aide de Visual Studio et si ses tâches en arrière-plan sont maintenant bloquées, redémarrez ou bien déconnectez-vous et reconnectez-vous afin de relancer l’exécution des tâches en arrière-plan de l’application.
-   Vous pouvez sélectionner l’option de débogage « Toujours réinstaller mon package » pour éviter cela dans des projets C#.
-   Patientez jusqu’à ce que l’application soit prête pour un déploiement final avant d’incrémenter la version du package (ne la changez pas pendant le débogage).

## <a name="remarks"></a>Notes

-   Assurez-vous que votre application vérifie la présence d’inscriptions de tâches en arrière-plan existantes avant d’inscrire de nouveau la tâche en arrière-plan. Plusieurs inscriptions de la même tâche en arrière-plan peuvent entraîner des résultats inattendus si vous exécutez plusieurs fois la tâche en arrière-plan à chacun de ses déclenchements.
-   Si la tâche en arrière-plan requiert un accès à l’écran de verrouillage, veillez à placer l’application sur l’écran de verrouillage avant d’essayer de déboguer la tâche en arrière-plan. Pour plus d’informations sur la spécification des options de manifeste pour les applications compatibles avec l’écran de verrouillage, consultez [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md).
-   Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Si l’un des paramètres d’inscription n’est pas valide, une erreur est renvoyée. Vérifiez que votre application gère de manière fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

Pour plus d’informations sur l’utilisation de VS pour déboguer une tâche en arrière-plan [, consultez Comment déclencher des événements de suspension, de reprise et d’arrière-plan dans les applications UWP](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015).

## <a name="related-topics"></a>Rubriques connexes

* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Recommandations relatives aux tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Comment déclencher des événements de suspension, de reprise et d’arrière-plan dans des applications UWP](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015)
* [Analyse de la qualité du code des applications UWP avec l’analyse du code Visual Studio](https://docs.microsoft.com/visualstudio/test/analyze-the-code-quality-of-store-apps-using-visual-studio-static-code-analysis?view=vs-2015)

 

 
