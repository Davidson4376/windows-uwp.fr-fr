---
author: TylerMSFT
title: "Recommandations pour les tâches en arrière-plan"
description: "Assurez-vous que votre application répond aux exigences relatives à l’exécution de tâches en arrière-plan."
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
translationtype: Human Translation
ms.sourcegitcommit: ea862ef33f58b33b70318ddfc1d09d9aca9b3517
ms.openlocfilehash: 2d03c7f47461422fef7a0905df7e68b3e65c33f0

---

# <a name="guidelines-for-background-tasks"></a>Recommandations pour les tâches en arrière-plan

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Assurez-vous que votre application répond aux exigences relatives à l’exécution de tâches en arrière-plan.

## <a name="background-task-guidance"></a>Recommandations en matière de tâches en arrière-plan

Tenez compte des recommandations suivantes au moment de développer une tâche en arrière-plan et avant de publier votre application.

Si vous utilisez une tâche en arrière-plan pour lire du contenu multimédia en arrière-plan, consultez [Lire du contenu multimédia en arrière-plan](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio). Les informations fournies sur les améliorations apportées dans Windows 10 version 1607 vous simplifieront la vie.

**Tâches en arrière-plan hors processus ou in-process :** Windows 10 version 1607 propose des [tâches en arrière-plan in-process](create-and-register-an-inproc-background-task.md) qui vous permettent d’exécuter du code en arrière-plan dans le même processus que votre application au premier plan. Pour déterminer s’il est préférable de disposer de tâches en arrière-plan in-process ou hors processus, prenez en compte les facteurs suivants :

|Facteur | Impact |
|--------------|--------|
|Résilience   | Si votre processus en arrière-plan s’exécute dans un autre processus, un blocage dans votre processus en arrière-plan ne bloque pas votre application au premier plan. De plus, l’activité en arrière-plan peut être arrêtée, même dans votre application, si elle s’exécute au-delà des limites de durée d’exécution. Séparer des tâches en arrière-plan dans une tâche distincte de l’application au premier plan peut être plus judicieux lorsque les processus au premier plan et en arrière-plan n’ont pas à communiquer entre eux (l’un des principaux avantages des tâches en arrière-plan in-process est qu’elles évitent toute communication entre les processus). |
|Simplicité    | Les tâches en arrière-plan in-process ne nécessitent aucune communication entre les processus et sont moins complexes à écrire.  |
|Déclencheurs disponibles | Les tâches en arrière-plan in-process ne prennent pas en charge les déclencheurs suivants : [DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) et **IoTStartupTask**. |
|VoIP | Les tâches en arrière-plan in-process ne prennent pas en charge l’activation d’une tâche en arrière-plan VoIP dans votre application. |  

**Quotas de processeur :** les tâches en arrière-plan sont limitées par la quantité de temps d’utilisation de l’horloge en fonction de leur type de déclencheur. La plupart des déclencheurs sont limités à 30 secondes d’utilisation de l’horloge, mais certains ont une capacité d’exécution pouvant atteindre 10 minutes pour exécuter des tâches intensives. Les tâches en arrière-plan doivent être légères pour préserver l’autonomie de la batterie et assurer une meilleure expérience utilisateur pour les applications de premier plan. Pour plus d’informations sur les contraintes de ressource appliquées aux tâches en arrière-plan, consultez [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

**Gérer les tâches en arrière-plan :** votre application doit obtenir la liste des tâches en arrière-plan inscrites, s’inscrire aux gestionnaires de progression et d’achèvement et gérer ces événements de manière appropriée. Vos classes de tâches en arrière-plan doivent signaler la progression, l’annulation et l’achèvement des tâches. Pour plus d’informations, consultez [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md) et [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md).

**Utilisez [BackgroundTaskDeferral](https://msdn.microsoft.com/library/windows/apps/hh700499) :** si votre classe de tâches en arrière-plan exécute du code asynchrone, veillez à utiliser des reports. Sinon, votre tâche en arrière-plan peut se terminer prématurément lorsque la méthode [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) (ou la méthode [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) dans le cas de tâches en arrière-plan in-process) est appelée. Pour plus d’informations, consultez [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md).

L’autre solution consiste à demander un report et à utiliser **async/await** pour exécuter des appels de méthode asynchrone. Fermez le report après les appels de la méthode **await**.

**Mettre à jour le manifeste de l’application :** pour les tâches en arrière-plan qui s’exécutent hors processus, déclarez chaque tâche en arrière-plan dans le manifeste de l’application, ainsi que le type de déclencheur avec lequel elle est utilisée. Sinon, votre application ne pourra pas inscrire la tâche en arrière-plan lors de l’exécution.

Les tâches en arrière-plan qui s’exécutent dans le même processus que l’application au premier plan n’ont pas à se déclarer dans le manifeste de l’application. Pour plus d’informations sur la déclaration hors processus dans le manifeste, consultez [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md).

**Préparez les mises à jour de l’application :** si votre application doit subir des mises à jour, créez et inscrivez une tâche en arrière-plan **ServicingComplete** (consultez [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)) pour annuler l’inscription des tâches en arrière-plan de la version précédente de l’application et inscrivez les tâches en arrière-plan à la nouvelle version. Le moment est également idéal pour effectuer des mises à jour d’application qui peuvent se révéler nécessaires hors du contexte d’exécution au premier plan.

**Demander l’exécution des tâches en arrière-plan :**

> **Important** Depuis Windows 10, les applications n’ont plus à figurer dans l’écran de verrouillage pour exécuter des tâches en arrière-plan.

Les applications de plateforme Windows universelle (UWP) peuvent exécuter tous les types de tâches prises en charge, sans être épinglées à l’écran de verrouillage. Toutefois, les applications doivent appeler [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire tout type de tâche en arrière-plan. Cette méthode retourne [**BackgroundAccessStatus.Denied**](https://msdn.microsoft.com/library/windows/apps/hh700439) si l’utilisateur a explicitement refusé des autorisations de tâche en arrière-plan pour votre application dans les paramètres de l’appareil.
## <a name="background-task-checklist"></a>Liste de vérifications relatives aux tâches en arrière-plan

*S’applique aux tâches en arrière-plan in-process et hors processus*

-   Associez votre tâche en arrière-plan au déclencheur approprié.
-   Ajoutez des conditions pour assurer une exécution aboutie de votre tâche en arrière-plan.
-   Gérez la progression, l’achèvement et l’annulation de la tâche en arrière-plan.
-   Réinscrivez vos tâches en arrière-plan pendant le lancement de l’application. De cette façon, elles seront inscrites au premier lancement de l’application. Cela permet également de détecter si l’utilisateur a désactivé les fonctionnalités d’exécution en arrière-plan de votre application (en cas d’échec de l’inscription).
-   Recherchez les erreurs d’inscription de la tâche en arrière-plan. Le cas échéant, tentez d’inscrire la tâche en arrière-plan une nouvelle fois avec d’autres valeurs de paramètres.
-   Pour toutes les familles d’appareils, à l’exception des ordinateurs de bureau, les tâches en arrière-plan peuvent être arrêtées en cas de mémoire insuffisante de l’appareil. Si aucune exception de mémoire insuffisante n’est exposée ou si l’application ne la gère pas, la tâche en arrière-plan est alors arrêtée sans avertissement ni déclenchement de l’événement OnCanceled. Cela permet de garantir l’expérience utilisateur de l’application au premier plan. Votre tâche en arrière-plan doit être conçue de manière à gérer ce scénario.

*S’applique uniquement aux tâches en arrière-plan hors processus*

-   Créez votre tâche en arrière-plan dans un composant Windows Runtime.
-   N’affichez pas l’interface utilisateur hormis les toasts, vignettes et mises à jour de badge de la tâche en arrière-plan.
-   Dans la méthode [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx), demandez des reports pour chaque appel de méthode asynchrone, puis fermez-les lorsque la méthode est terminée. Vous pouvez également utiliser un report avec **async/await**.
-   Utilisez un dispositif de stockage persistant pour partager des données entre la tâche en arrière-plan et l’application.
-   Déclarez chaque tâche en arrière-plan dans le manifeste de l’application, de même que le type de déclencheur avec lequel elle est utilisée. Vérifiez que le point d’entrée et les types de déclencheur sont corrects.
-   Ne spécifiez aucun élément Executable dans le manifeste sauf si vous utilisez un déclencheur à exécuter dans le même contexte que l’application (comme le déclencheur [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)).

*S’applique uniquement aux tâches en arrière-plan in-process*

- Lors de l’annulation d’une tâche, vérifiez que le gestionnaire d’événements `BackgroundActivated` se ferme avant l’annulation ou la fin du processus.
-   Écrivez des tâches en arrière-plan de courte durée. Les tâches en arrière-plan sont limitées à 30 secondes de l’utilisation de l’horloge.
-   Ne comptez pas sur l’interaction utilisateur dans les tâches en arrière-plan.

## <a name="windows-background-task-checklist-for-lock-screen-capable-apps"></a>Windows : liste de vérification des tâches en arrière-plan pour les applications compatibles avec l’écran de verrouillage

Suivez ces recommandations lorsque vous développez des tâches en arrière-plan pour des applications pouvant figurer dans l’écran de verrouillage. Suivez les recommandations de la rubrique [Recommandations et liste de vérification sur les vignettes d’écran de verrouillage](https://msdn.microsoft.com/library/windows/apps/hh465403).

-   Assurez-vous que votre application a besoin de figurer dans l’écran de verrouillage avant de la développer en tant qu’application compatible avec l’écran de verrouillage. Pour plus d’informations, voir [Vue d’ensemble des écrans de verrouillage](https://msdn.microsoft.com/library/windows/apps/hh779720).

-   Vérifiez que votre application fonctionne toujours lorsqu’elle ne figure pas dans l’écran de verrouillage.

-   Incluez une tâche en arrière-plan inscrite avec [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543), [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) ou [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) et déclarez-la dans le manifeste de l’application. Assurez-vous que le point d’entrée et les types de déclencheur sont corrects. Il s’agit d’une condition de certification, et cela permet à l’utilisateur de placer l’application dans l’écran de verrouillage.

**Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui créent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

## <a name="related-topics"></a>Rubriques connexes

* [Créez et inscrivez une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Contenu audio en arrière-plan](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications du Windows Store (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Dec16_HO2-->


