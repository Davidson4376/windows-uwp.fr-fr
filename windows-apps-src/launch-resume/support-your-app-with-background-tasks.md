---
author: TylerMSFT
title: "Prendre en charge votre application avec des tâches en arrière-plan"
description: "Les rubriques de cette section expliquent comment exécuter du code léger en arrière-plan, en réponse à des déclencheurs."
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
translationtype: Human Translation
ms.sourcegitcommit: 7208ca16fe7eee2ec4039f3c4bc6305dceac96f3
ms.openlocfilehash: b33fd118289ca575207be97bd8a1a33ddcc49a87

---

# <a name="support-your-app-with-background-tasks"></a>Prendre en charge votre application avec des tâches en arrière-plan

\[ Mise à jour pour les applications UWP sur Windows&nbsp;10. Pour les articles sur Windows&nbsp;8.x, consultez l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132).\]

Les rubriques de cette section expliquent comment exécuter du code léger en arrière-plan, en réponse à des déclencheurs. Vous pouvez utiliser les tâches en arrière-plan pour fournir des fonctionnalités lorsque votre application est suspendue ou n’est pas en cours d’exécution. Les tâches en arrière-plan sont également utiles pour les applications de communication en temps réel (VoIP, messagerie électronique et messagerie instantanée, par exemple).

## <a name="playing-media-in-the-background"></a>Lecture de contenu multimédia en arrière-plan

À partir de Windows&nbsp;10, version&nbsp;1607, il est beaucoup plus facile de lire du contenu audio en arrière-plan. Voir [Lire du contenu multimédia en arrière-plan](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio) pour obtenir plus d’informations.

## <a name="in-process-and-out-of-process-background-tasks"></a>Tâches en arrière-plan in-process et hors processus

Il existe deux approches pour implémenter des tâches en arrière-plan&nbsp;: in-process, dans lequel l’application et son processus en arrière-plan s’exécutent dans le même processus&nbsp;; et hors processus, où l’application et le processus en arrière-plan s’exécutent dans des processus distincts. L’approche in-process a été introduite dans Windows&nbsp;10 version&nbsp;1607 pour simplifier l’écriture des tâches en arrière-plan. Toutefois, vous pouvez toujours écrire des tâches en arrière-plan hors processus. Consultez la rubrique [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md) pour savoir quand utiliser une approche hors processus ou intra-processus pour écrire une tâche en arrière-plan.

Les tâches en arrière-plan hors processus offrent une meilleure résilience, car votre processus en arrière-plan ne peut pas bloquer le processus de votre application en cas de problème. Mais la résilience implique une plus grande complexité lorsqu’il s’agit de gérer la communication entre les processus.

Les tâches en arrière-plan hors processus sont implémentées en tant que classes légères que le système d’exploitation exécute dans un processus distinct (backgroundtaskhost.exe). Les tâches en arrière-plan hors processus sont des classes que vous écrivez pour implémenter l’interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Vous inscrivez une tâche en arrière-plan à l’aide de la classe [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Le nom de la classe est utilisé pour spécifier le point d’entrée lors de l’inscription de la tâche en arrière-plan.

Dans Windows&nbsp;10 version&nbsp;1607, vous pouvez activer l’activité en arrière-plan sans avoir à créer de tâche en arrière-plan. Vous pouvez exécuter votre code en arrière-plan directement à l’intérieur de l’application au premier plan.

Pour savoir comment créer des tâches en arrière-plan in-process, consultez la rubrique [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).

Pour savoir comment créer des tâches en arrière-plan hors processus, consultez la rubrique [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-an-outofproc-background-task.md).

> [!TIP]
> À partir de Windows&nbsp;10, il n’est plus nécessaire de placer une application sur l’écran de verrouillage pour qu’elle inscrive une tâche en arrière-plan.

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

 
Pour plus d’informations, voir [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

## <a name="application-manifest-requirements"></a>Conditions requises pour le manifeste de l’application

Pour que votre application puisse inscrire une tâche en arrière-plan qui s’exécute hors processus, vous devez au préalable la déclarer dans le manifeste de l’application. Il n’est pas nécessaire de déclarer dans le manifeste de l’application les tâches en arrière-plan qui s’exécutent dans le même processus que leur application hôte. Pour plus d’informations, voir [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md).

## <a name="background-tasks"></a>Tâches en arrière-plan

Les déclencheurs en temps réel suivants peuvent être utilisés pour exécuter du code léger personnalisé en arrière-plan&nbsp;:

| Déclencheur en temps réel  | Description |
|--------------------|-------------|
| **Canal de contrôle** | Les tâches en arrière-plan peuvent conserver une connexion active et recevoir des messages sur le canal de contrôle en utilisant l’objet [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Si votre application est à l’écoute d’un socket, vous pouvez utiliser le Broker de socket à la place du **ControlChannelTrigger**. Pour plus d’informations sur l’utilisation du Broker de socket, voir [SocketActivityTrigger](https://msdn.microsoft.com/library/windows/apps/dn806009). La classe **ControlChannelTrigger** n’est pas prise en charge sur Windows Phone. |
| **Minuteur** | Vous pouvez exécuter des tâches en arrière-plan toutes les 15&nbsp;minutes et programmer leur exécution à l’aide du [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). Pour plus d’informations, voir [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md). |
| **Notification Push** | Les tâches en arrière-plan répondent à l’objet [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) pour recevoir des notifications Push brutes. |

**Remarque**  

Les applications Windows universelles doivent appeler l’élément [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire n’importe quel type de déclencheur en arrière-plan.

Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, appelez [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471), puis [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) lorsque votre application est lancée après avoir été mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

## <a name="system-event-triggers"></a>Déclencheurs d’événements système

L’énumération [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) répertorie les déclencheurs d’événements système suivants&nbsp;:

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

Les tâches en arrière-plan sont limitées à 30&nbsp;secondes de l’utilisation de l’horloge.

### <a name="memory-constraints"></a>Contraintes de mémoire

En raison des contraintes de ressources des appareils disposant de peu de mémoire, les tâches en arrière-plan peuvent avoir une limite de mémoire qui détermine la quantité maximale de mémoire que la tâche en arrière-plan peut utiliser. Si votre tâche en arrière-plan tente d’exécuter une opération qui dépasse cette limite, l’opération échoue et peut générer une exception de mémoire insuffisante que la tâche pourra gérer. Si la tâche ne parvient pas à gérer cette exception, ou si la nature de l’opération est telle qu’elle n’a pas généré d’exception de mémoire insuffisante, la tâche sera immédiatement arrêtée.  
 Vous pouvez utiliser les API [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) pour connaître le niveau d’utilisation actuel et les limites de la mémoire afin d’en déterminer le plafond (le cas échéant) et de surveiller l'utilisation de la mémoire de votre tâche en arrière-plan.

### <a name="per-device-limit-for-apps-with-background-tasks-for-low-memory-devices"></a>Limite par appareil pour les applications avec des tâches en arrière-plan pour les appareils dont la mémoire est insuffisante

Sur les appareils à faible capacité de mémoire, le nombre d’applications pouvant être installées et l’utilisation de tâches en arrière-plan à tout moment sont limités. En cas de dépassement, l’appel à [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485), qui est requis pour inscrire toutes les tâches en arrière-plan, échoue.

### <a name="battery-saver"></a>Économiseur de batterie

Sauf si vous appliquez une exception à votre application pour qu’elle puisse continuer à exécuter des tâches en arrière-plan et à recevoir des notifications push lorsque l’Économiseur de batterie est activé, la fonctionnalité Économiseur de batterie, lorsqu’elle est activée, empêche l’exécution de tâches en arrière-plan quand l’appareil n’est pas connecté à une source d’énergie externe et que la batterie passe sous une quantité d’énergie restante spécifiée. Cela ne vous empêche pas d’inscrire des tâches en arrière-plan.

## <a name="background-task-resource-guarantees-for-real-time-communication"></a>Garanties de ressources des tâches en arrière-plan pour la communication en temps réel

Afin que les quotas de ressources n’interfèrent pas avec la fonctionnalité de communication en temps réel, les tâches en arrière-plan qui utilisent le [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) et le [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) bénéficient de quotas de ressources de processeur garantis pour chaque tâche en cours d’exécution. Les quotas de ressources sont comme mentionné plus haut et demeurent inchangés pour ces tâches en arrière-plan.

Votre application n’a pas besoin d’effectuer une procédure différente pour obtenir les quotas de ressources garantis pour les tâches en arrière-plan [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) et [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543). Le système les traite toujours en tant que tâches en arrière-plan critiques.

## <a name="maintenance-trigger"></a>Déclencheur de maintenance

Les tâches de maintenance s’exécutent uniquement lorsque l’appareil est branché sur le secteur. Pour plus d’informations, voir [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md).

## <a name="background-tasks-for-sensors-and-devices"></a>Tâches en arrière-plan pour les capteurs et les périphériques

Votre application peut accéder à des capteurs et des périphériques à partir d’une tâche en arrière-plan à l’aide de la classe [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Vous pouvez utiliser ce déclencheur pour des opérations longues telles que la synchronisation ou la surveillance de données. Contrairement aux tâches associées aux événements système, une tâche **DeviceUseTrigger** peut être déclenchée uniquement lorsque votre application s’exécute au premier plan et ne prend en charge aucune condition.

> [!IMPORTANT]
> Les déclencheurs **DeviceUseTrigger** et **DeviceServicingTrigger** ne peuvent pas être utilisés avec des tâches en arrière-plan in-process.

Certaines opérations d’appareil critiques, comme les longues mises à jour de microprogrammes, ne peuvent pas être exécutées avec le déclencheur [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). De telles opérations ne peuvent être effectuées que sur le PC, et uniquement par une application privilégiée utilisant le [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315). Une *application privilégiée* est une application autorisée par le fabricant de l’appareil à effectuer ces opérations. Les métadonnées de l’appareil permettent de spécifier l’application définie, le cas échéant, comme application privilégiée d’un appareil. Pour plus d’informations, voir [Synchronisation et mise à jour des périphériques pour les applications de périphérique du Windows Store](http://go.microsoft.com/fwlink/p/?LinkId=306619).

## <a name="managing-background-tasks"></a>Gestion des tâches en arrière-plan

Les tâches en arrière-plan peuvent signaler leur progression, leur annulation et leur achèvement à votre application par le biais d’événements et du stockage local. Votre application peut également intercepter les exceptions levées par une tâche en arrière-plan et gérer l’inscription des tâches en arrière-plan lors des mises à jour des applications. Pour plus d’informations, voir :

[Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)  
[Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)

**Remarque**  
Cet article s’adresse aux développeurs de Windows&nbsp;10 qui développent des applications de la plateforme Windows universelle (UWP). Si vous développez une application pour Windows&nbsp;8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 ## <a name="related-topics"></a>Rubriques connexes

**Recommandations conceptuelles pour le multitâche sous Windows&nbsp;10**

* [Lancement, reprise et multitâche](index.md)

**Recommandations connexes en matière de tâches en arrière-plan**

* [Lire du contenu multimédia en arrière-plan](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio)
* [Accéder aux capteurs et aux périphériques à partir d’une tâche en arrière-plan](access-sensors-and-devices-from-a-background-task.md)
* [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-an-outofproc-background-task.md)
* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications du Windows Store (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [Synchronisation et mise à jour des périphériques pour les applications pour périphériques du Windows Store](http://go.microsoft.com/fwlink/p/?LinkId=306619)



<!--HONumber=Dec16_HO1-->


