---
title: Prendre en charge votre application avec des tâches en arrière-plan
description: Les rubriques de cette section expliquent comment exécuter du code léger en arrière-plan, en réponse à des déclencheurs.
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
ms.date: 08/21/2017
ms.topic: article
keywords: Windows 10, uwp, tâche d’arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: 2413a27c12a9b36f0fd57482492414e7b5a379b6
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7705603"
---
# <a name="support-your-app-with-background-tasks"></a>Prendre en charge votre application avec des tâches en arrière-plan


Les rubriques de cette section expliquent comment exécuter du code léger en arrière-plan, en réponse à des déclencheurs. Vous pouvez utiliser les tâches en arrière-plan pour fournir des fonctionnalités lorsque votre application est suspendue ou n’est pas en cours d’exécution. Les tâches en arrière-plan sont également utiles pour les applications de communication en temps réel (VoIP, messagerie électronique et messagerie instantanée, par exemple).

## <a name="playing-media-in-the-background"></a>Lecture de contenu multimédia en arrière-plan

À partir de Windows10, version1607, il est beaucoup plus facile de lire du contenu audio en arrière-plan. Voir [Lire du contenu multimédia en arrière-plan](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio) pour obtenir plus d’informations.

## <a name="in-process-and-out-of-process-background-tasks"></a>Tâches en arrière-plan in-process et hors processus

Il existe deux approches pour implémenter des tâches en arrière-plan:

* Dans le processus: l’application et son processus en arrière-plan s’exécutent dans le même processus
* Out-of-process: l’application et le processus en arrière-plan s’exécutent dans des processus distincts.

L’approche in-process a été introduite dans Windows10 version1607 pour simplifier l’écriture des tâches en arrière-plan. Toutefois, vous pouvez toujours écrire des tâches en arrière-plan hors processus. Consultez la rubrique [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md) pour savoir quand utiliser une approche hors processus ou intra-processus pour écrire une tâche en arrière-plan.

Les tâches en arrière-plan out-of-process sont plus résistants, car le processus en arrière-plan ne peut pas bloquer le processus de votre application en cas de problème. Mais la résilience implique le prix d’une plus grande complexité pour gérer la communication interprocessus entre l’application et la tâche en arrière-plan.

Les tâches en arrière-plan hors processus sont implémentées en tant que classes légères qui implémentent l’interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) que le système d’exploitation s’exécute dans un processus distinct (backgroundtaskhost.exe). Inscrire une tâche en arrière-plan à l’aide de la classe [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) . Le nom de la classe est utilisé pour spécifier le point d’entrée lors de l’inscription de la tâche en arrière-plan.

Dans Windows10 version1607, vous pouvez activer l’activité en arrière-plan sans avoir à créer de tâche en arrière-plan. Vous pouvez exécuter à la place de votre code en arrière-plan directement à l’intérieur de processus de l’application au premier plan.

Pour savoir comment créer des tâches en arrière-plan in-process, consultez la rubrique [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).

Pour savoir comment créer des tâches en arrière-plan hors processus, consultez la rubrique [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md).

> [!TIP]
> À compter de Windows 10, vous n’avez plus besoin de placer une application sur l’écran de verrouillage en tant qu’inscrive une tâche en arrière-plan pour celui-ci.

## <a name="background-tasks-for-system-events"></a>Tâches en arrière-plan pour événements système

Votre application peut répondre à des événements générés par le système en inscrivant une tâche en arrière-plan à l’aide de la classe [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838). Une application peut utiliser au choix l’un des déclencheurs d’événements système suivants (définis dans [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839))

| Nom du déclencheur                     | Description                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | Internet devient disponible.                                                                                |
| **NetworkStateChange**           | Une modification, liée au coût ou à la connectivité, par exemple, se produit sur le réseau.                                              |
| **OnlineIdConnectedStateChange** | ID en ligne associé aux modifications du compte.                                                                 |
| **SmsReceived**                  | Un nouveau message SMS est reçu par un appareil mobile à haut débit installé.                                         |
| **TimeZoneChange**               | Le fuseau horaire est modifié sur l’appareil (par exemple, lorsque le système règle l’horloge pour le passage à l’heure d’été). |

Pour plus d’informations, voir [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md).

## <a name="conditions-for-background-tasks"></a>Conditions pour tâches en arrière-plan

Vous pouvez contrôler à quel moment la tâche en arrière-plan est exécutée, même après l’avoir déclenchée, en ajoutant une condition. Une fois déclenchée, la tâche en arrière-plan ne sera pas exécutée tant que toutes ses conditions ne seront pas remplies. Les conditions suivantes (représentées par l’énumération [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)) peuvent être appliquées.

| Nom de la condition           | Description                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | Internet doit être accessible.   |
| **InternetNotAvailable** | Internet ne doit pas être accessible. |
| **SessionConnected**     | La session doit être connectée.    |
| **SessionDisconnected**  | La session doit être déconnectée. |
| **UserNotPresent**       | L’utilisateur doit être absent.            |
| **UserPresent**          | L’utilisateur doit être présent.         |

Ajoutez la condition **InternetAvailable** à votre tâche en arrière-plan [BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) pour retarder le déclenchement de la tâche en arrière-plan jusqu'à ce que la pile réseau s’exécute. Cette condition économise l’énergie car la tâche en arrière-plan ne s’exécute jusqu'à ce que le réseau est disponible. Cette condition ne fournit pas d’activation en temps réel.

Si votre tâche en arrière-plan nécessite une connectivité réseau, définissez [IsNetworkRequested](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) pour vous assurer que le réseau reste opérationnel pendant l’exécution de la tâche en arrière-plan. Cela indique à l’infrastructure de tâches en arrière-plan qu’elle doit maintenir le réseau actif pendant l’exécution de la tâche, même si le périphérique est passé en mode de veille connectée. Si votre tâche en arrière-plan ne définit pas **IsNetworkRequested**, alors votre tâche en arrière-plan sera pas en mesure d’accéder au réseau en mode de veille connectée (par exemple, lorsque l’écran du téléphone est désactivée.)
 
Pour plus d’informations sur les conditions de tâche en arrière-plan, consultez [définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

## <a name="application-manifest-requirements"></a>Conditions requises pour le manifeste de l’application

Pour que votre application puisse inscrire une tâche en arrière-plan qui s’exécute hors processus, vous devez au préalable la déclarer dans le manifeste de l’application. Il n’est pas nécessaire de déclarer dans le manifeste de l’application les tâches en arrière-plan qui s’exécutent dans le même processus que leur application hôte. Pour plus d’informations, voir [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md).

## <a name="background-tasks"></a>Tâches en arrière-plan

Les déclencheurs en temps réel suivants peuvent être utilisés pour exécuter du code léger personnalisé en arrière-plan:

| Déclencheur en temps réel  | Description |
|--------------------|-------------|
| **Canal de contrôle** | Les tâches en arrière-plan peuvent conserver une connexion active et recevoir des messages sur le canal de contrôle en utilisant l’objet [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Si votre application est à l’écoute d’un socket, vous pouvez utiliser le Broker de socket à la place du **ControlChannelTrigger**. Pour plus d’informations sur l’utilisation du Broker de socket, voir [SocketActivityTrigger](https://msdn.microsoft.com/library/windows/apps/dn806009). La classe **ControlChannelTrigger** n’est pas prise en charge sur Windows Phone. |
| **Minuteur** | Vous pouvez exécuter des tâches en arrière-plan toutes les 15minutes et programmer leur exécution à l’aide du [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). Pour plus d’informations, voir [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md). |
| **Notification Push** | Les tâches en arrière-plan répondent à l’objet [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) pour recevoir des notifications Push brutes. |

**Remarque**  

Les applications Windows universelles doivent appeler l’élément [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire n’importe quel type de déclencheur en arrière-plan.

Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, appelez [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471), puis [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) lorsque votre application est lancée après avoir été mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

**Le nombre d’instances de déclencheur:** des limites régissent le nombre d’instances de certains déclencheurs qu’une application peut inscrire. Une application peut inscrire [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) et [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396) une seule fois par instance de l’application. Si une application dépasse cette limite, l’inscription lève une exception.

## <a name="system-event-triggers"></a>Déclencheurs d’événements système

L’énumération [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) répertorie les déclencheurs d’événements système suivants:

| Nom du déclencheur            | Description                                                       |
|-------------------------|-------------------------------------------------------------------|
| **UserPresent**         | La tâche en arrière-plan est déclenchée dès que l’utilisateur est présent.   |
| **UserAway**            | La tâche en arrière-plan est déclenchée dès que l’utilisateur est absent.    |
| **ControlChannelReset** | La tâche en arrière-plan est déclenchée lorsqu’un canal de contrôle est réinitialisé. |
| **SessionConnected**    | La tâche en arrière-plan est déclenchée dès que la session est connectée.   |

   
Les déclencheurs d’événements système suivants permettent de savoir lorsque l’utilisateur active ou désactive une application sur l’écran de verrouillage.

| Nom du déclencheur                     | Description                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded**   | Une vignette d’application est ajoutée à l’écran de verrouillage.     |
| **LockScreenApplicationRemoved** | Une vignette d’application est supprimée de l’écran de verrouillage. |

 
## <a name="background-task-resource-constraints"></a>Contraintes de ressource des tâches en arrière-plan

Les tâches en arrière-plan sont légères. Le fait de limiter autant que possible l’exécution en arrière-plan garantit une expérience utilisateur optimale pour les applications de premier plan et une meilleure autonomie de la batterie. Pour cela, il convient d’appliquer des contraintes de ressource aux tâches en arrière-plan.

Les tâches en arrière-plan sont limitées à 30secondes de l’utilisation de l’horloge.

### <a name="memory-constraints"></a>Contraintes de mémoire

En raison des contraintes de ressources des appareils disposant de peu de mémoire, les tâches en arrière-plan peuvent avoir une limite de mémoire qui détermine la quantité maximale de mémoire que la tâche en arrière-plan peut utiliser. Si votre tâche en arrière-plan tente d’exécuter une opération qui dépasse cette limite, l’opération échoue et peut générer une exception de mémoire insuffisante que la tâche pourra gérer. Si la tâche ne parvient pas à gérer cette exception, ou si la nature de l’opération est telle qu’elle n’a pas généré d’exception de mémoire insuffisante, la tâche sera immédiatement arrêtée.  

Vous pouvez utiliser les API [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) pour connaître le niveau d’utilisation actuel et les limites de la mémoire afin d’en déterminer le plafond (le cas échéant) et de surveiller l'utilisation de la mémoire de votre tâche en arrière-plan.

### <a name="per-device-limit-for-apps-with-background-tasks-for-low-memory-devices"></a>Limite par appareil pour les applications avec des tâches en arrière-plan pour les appareils dont la mémoire est insuffisante

Sur les appareils à faible capacité de mémoire, le nombre d’applications pouvant être installées et l’utilisation de tâches en arrière-plan à tout moment sont limités. En cas de dépassement, l’appel à [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485), qui est requis pour inscrire toutes les tâches en arrière-plan, échoue.

### <a name="battery-saver"></a>Économiseur de batterie

Sauf si vous appliquez une exception à votre application pour qu’elle puisse continuer à exécuter des tâches en arrière-plan et à recevoir des notifications push lorsque l’Économiseur de batterie est activé, la fonctionnalité Économiseur de batterie, lorsqu’elle est activée, empêche l’exécution de tâches en arrière-plan quand l’appareil n’est pas connecté à une source d’énergie externe et que la batterie passe sous une quantité d’énergie restante spécifiée. Cela ne vous empêche pas d’inscrire des tâches en arrière-plan.

Toutefois, pour les applications d’entreprise et les applications qui ne seront pas publiées dans le Microsoft Store, voir [s’exécuter en arrière-plan pendant une période indéfinie](run-in-the-background-indefinetly.md) pour savoir comment utiliser des fonctionnalités pour exécuter indéfiniment une tâche en arrière-plan ou une session d’exécution étendue en arrière-plan.

## <a name="background-task-resource-guarantees-for-real-time-communication"></a>Garanties de ressources des tâches en arrière-plan pour la communication en temps réel

Afin que les quotas de ressources n’interfèrent pas avec la fonctionnalité de communication en temps réel, les tâches en arrière-plan qui utilisent le [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) et le [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) bénéficient de quotas de ressources de processeur garantis pour chaque tâche en cours d’exécution. Les quotas de ressources sont comme mentionné plus haut et demeurent inchangés pour ces tâches en arrière-plan.

Votre application n’a pas besoin d’effectuer une procédure différente pour obtenir les quotas de ressources garantis pour les tâches en arrière-plan [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) et [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543). Le système les traite toujours en tant que tâches en arrière-plan critiques.

## <a name="maintenance-trigger"></a>Déclencheur de maintenance

Les tâches de maintenance s’exécutent uniquement lorsque l’appareil est branché sur le secteur. Pour plus d’informations, voir [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md).

## <a name="background-tasks-for-sensors-and-devices"></a>Tâches en arrière-plan pour les capteurs et les périphériques

Votre application peut accéder à des capteurs et des périphériques à partir d’une tâche en arrière-plan à l’aide de la classe [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Vous pouvez utiliser ce déclencheur pour des opérations longues telles que la synchronisation ou la surveillance de données. Contrairement aux tâches associées aux événements système, une tâche **DeviceUseTrigger** peut être déclenchée uniquement lorsque votre application s’exécute au premier plan et ne prend en charge aucune condition.

> [!IMPORTANT]
> Les déclencheurs **DeviceUseTrigger** et **DeviceServicingTrigger** ne peuvent pas être utilisés avec des tâches en arrière-plan in-process.

Certaines opérations d’appareil critiques, comme les longues mises à jour de microprogrammes, ne peuvent pas être exécutées avec le déclencheur [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). De telles opérations ne peuvent être effectuées que sur le PC, et uniquement par une application privilégiée utilisant le [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315). Une *application privilégiée* est une application autorisée par le fabricant de l’appareil à effectuer ces opérations. Les métadonnées de périphérique permettent de spécifier l’application définie, le cas échéant, comme application privilégiée d’un appareil. Pour plus d’informations, voir la [synchronisation et mise à jour pour les applications de périphérique du Microsoft Store](http://go.microsoft.com/fwlink/p/?LinkId=306619)

## <a name="managing-background-tasks"></a>Gestion des tâches en arrière-plan

Les tâches en arrière-plan peuvent signaler leur progression, leur annulation et leur achèvement à votre application par le biais d’événements et du stockage local. Votre application peut également intercepter les exceptions levées par une tâche en arrière-plan et gérer l’inscription des tâches en arrière-plan lors des mises à jour des applications. Pour plus d’informations, voir :

[Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)  
[Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)

Vérifiez votre inscription de tâche en arrière-plan pendant le lancement de l’application. Assurez-vous que les tâches en arrière-plan dissociées de votre application sont présents dans BackgroundTaskBuilder.AllTasks. Réinscrivez ceux qui n’est pas présents. Annulez l’enregistrement de toutes les tâches qui ne sont plus nécessaires. Cela garantit que toutes les inscriptions de tâches en arrière-plan sont à jour chaque fois que l’application est lancée.

## <a name="related-topics"></a>Rubriques associées

**Recommandations conceptuelles pour le multitâche sous Windows 10**

* [Lancement, reprise et multitâche](index.md)

**Recommandations connexes en matière de tâches en arrière-plan**

* [Recommandations pour les tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Accéder à des capteurs et des appareils à partir d’une tâche en arrière-plan](access-sensors-and-devices-from-a-background-task.md)
* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md)
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Convertir une tâche en arrière-plan hors processus en tâche en arrière-plan in-process](convert-out-of-process-background-task.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Regrouper une inscription de la tâche en arrière-plan.](group-background-tasks.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications UWP (lors du débogage)](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Lire du contenu multimédia en arrière-plan](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Exécuter une tâche en arrière-plan lorsque votre application UWP est mise à jour](run-a-background-task-during-updatetask.md)
* [Exécuter indéfiniment en arrière-plan](run-in-the-background-indefinetly.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Déclencher une tâche en arrière-plan à partir de votre application](trigger-background-task-from-app.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
