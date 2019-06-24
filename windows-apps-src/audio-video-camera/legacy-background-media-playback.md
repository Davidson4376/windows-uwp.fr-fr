---
ms.assetid: 3848cd72-eccd-400e-93ff-13649cd81b6c
description: Cet article fournit une assistance relative aux applications recourant au modèle de lecture multimédia d’arrière-plan et fournit des recommandations relatives à la migration vers le nouveau modèle.
title: Lecture multimédia en arrière-plan héritée
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dee2bcb05b9d30177c68b1beac468ac19f4a1be9
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318343"
---
# <a name="legacy-background-media-playback"></a>Lecture multimédia en arrière-plan héritée


Cet article décrit le modèle hérité à deux processus d’ajout de la prise en charge de l’audio d’arrière-plan à votre application UWP. À partir de Windows 10, version 1607, un modèle à processus unique pour l’audio d’arrière-plan qui est bien plus facile à implémenter. Pour plus d’informations sur les recommandations actuelles en matière d’audio d’arrière-plan, consultez la section [Lire du contenu multimédia en arrière-plan](background-audio.md). Cet article vise à fournir un support pour les applications déjà développées à l’aide du modèle hérité à deux processus.

> [!NOTE]
> En commençant par Windows, version 1703, **BackgroundMediaPlayer** est déconseillée et n’est peut-être pas disponible dans les versions futures de Windows.

## <a name="background-audio-architecture"></a>Architecture de la lecture audio en arrière-plan

Une application exécutant la lecture en arrière-plan comprend deux processus. Le premier processus est l’application principale, qui contient l’interface utilisateur et la logique client de l’application, exécutée au premier plan. Le second processus est la tâche de lecture en arrière-plan, qui implémente [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask), comme toutes les tâches en arrière-plan d’application UWP. La tâche en arrière-plan contient la logique de lecture audio et les services en arrière-plan. La tâche en arrière-plan communique avec le système via les contrôles de transport de média système.

Le diagramme suivant est une vue d’ensemble de la conception du système.

![Architecture de la lecture audio en arrière-plan Windows 10](images/backround-audio-architecture-win10.png)
## <a name="mediaplayer"></a>MediaPlayer

L’espace de noms [**Windows.Media.Playback**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback) contient les API utilisées pour la lecture audio en arrière-plan. Il existe une seule instance de [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) par application par le biais de laquelle la lecture s’effectue. Votre application de lecture audio en arrière-plan appelle des méthodes et configure des propriétés sur la classe **MediaPlayer** pour définir la piste actuelle, démarrer la lecture, mettre en pause, avancer, reculer, etc. L’instance d’objet de lecteur multimédia est toujours accessible via la propriété [**BackgroundMediaPlayer.Current**](https://docs.microsoft.com/uwp/api/windows.media.playback.backgroundmediaplayer.current).

## <a name="mediaplayer-proxy-and-stub"></a>MediaPlayer Proxy et Stub

Lorsque **BackgroundMediaPlayer.Current** est accessible à partir du processus en arrière-plan de votre application, l’instance **MediaPlayer** est activée dans l’hôte de tâche en arrière-plan et peut être manipulée directement.

Lorsque **BackgroundMediaPlayer.Current** est accessible à partir de l’application au premier plan, l’instance **MediaPlayer** qui est renvoyée est en réalité un proxy qui communique avec un stub dans le processus en arrière-plan. Ce stub communique avec l’instance **MediaPlayer** réelle, qui est également hébergée dans le processus en arrière-plan.

Les processus de premier plan et d’arrière-plan peuvent accéder à la plupart des propriétés de l’instance **MediaPlayer**, à l’exception de [**MediaPlayer.Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source) et [**MediaPlayer.SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) qui ne sont accessibles qu’à partir du processus en arrière-plan. L’application de premier plan et le processus en arrière-plan peuvent recevoir les notifications d’événements propres au contenu multimédia comme [**MediaOpened**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.mediaopened), [**MediaEnded**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.mediaended), et [**MediaFailed**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.mediafailed).

## <a name="playback-lists"></a>Listes de lecture

Un scénario courant pour les applications audio en arrière-plan est de lire plusieurs éléments à la suite. Cette tâche s’effectue facilement dans votre processus en arrière-plan grâce à un objet [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList), qui peut être défini en tant que source sur le **MediaPlayer** en l’affectant à la propriété [**MediaPlayer.Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source).

Il n’est pas possible d’accéder à une **MediaPlaybackList** à partir du processus de premier plan défini dans le processus en arrière-plan.

## <a name="system-media-transport-controls"></a>Contrôles de transport de média système

Un utilisateur peut contrôler la lecture audio sans utiliser directement l’interface utilisateur de votre application par divers moyens comme les appareils Bluetooth, SmartGlass et les contrôles de transport de média système. Votre tâche en arrière-plan utilise la classe [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) pour s’abonner à ces événements système initiés par l’utilisateur.

Pour obtenir une instance **SystemMediaTransportControls** depuis le processus en arrière-plan, utilisez la propriété [**MediaPlayer.SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols). Les applications de premier plan obtiennent une instance de la classe en appelant [**SystemMediaTransportControls.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview), mais l’instance renvoyée est une instance de premier plan uniquement qui n’est pas associée à la tâche en arrière-plan.

## <a name="sending-messages-between-tasks"></a>Envoi de messages entre les tâches

Vous voudrez parfois que les deux processus d’une application de lecture audio en arrière-plan communiquent entre eux. Vous pouvez, par exemple, vouloir que la tâche en arrière-plan informe la tâche au premier plan que la lecture d’une nouvelle piste commence, puis qu’elle envoie le titre de la nouvelle chanson à la tâche de premier plan afin qu’elle l’affiche à l’écran.

Un mécanisme de communication simple déclenche des événements à la fois dans le processus au premier plan et dans le processus en arrière-plan. Les méthodes [**SendMessageToForeground**](https://docs.microsoft.com/uwp/api/windows.media.playback.backgroundmediaplayer.sendmessagetoforeground) et [**SendMessageToBackground**](https://docs.microsoft.com/uwp/api/windows.media.playback.backgroundmediaplayer.sendmessagetobackground) invoquent chacune des événements dans le processus correspondant. Les messages peuvent être reçus en vous abonnant aux événements [**MessageReceivedFromBackground**](https://docs.microsoft.com/uwp/api/windows.media.playback.backgroundmediaplayer.messagereceivedfrombackground) et [**MessageReceivedFromForeground**](https://docs.microsoft.com/uwp/api/windows.media.playback.backgroundmediaplayer.messagereceivedfromforeground).

Les données peuvent être transmises en tant qu’argument aux méthodes d’envoi des messages, qui sont ensuite transmises aux gestionnaires d’événements de message reçu. Passer des données à l’aide de la classe [**ValueSet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet). Cette classe est un dictionnaire qui contient une chaîne comme clé et d’autres types de valeur comme valeurs. Vous pouvez passer des types de valeur simples tels que des entiers, des chaînes et des valeurs booléennes.

## <a name="background-task-life-cycle"></a>Durée de vie d’une tâche en arrière-plan

La durée de vie d’une tâche en arrière-plan est étroitement liée à l’état de lecture actuelle de votre application. Par exemple, lorsque l’utilisateur met en pause la lecture audio, le système peut terminer ou annuler votre application en fonction des circonstances. Après une période de temps sans lecture audio, le système peut arrêter automatiquement la tâche en arrière-plan.

La méthode [**IBackgroundTask.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) est appelée lorsque votre application accède pour la première fois à [**BackgroundMediaPlayer.Current**](https://docs.microsoft.com/uwp/api/windows.media.playback.backgroundmediaplayer.current) dans le code de l’application au premier plan ou lorsque vous enregistrez un gestionnaire pour l’événement [**MessageReceivedFromBackground**](https://docs.microsoft.com/uwp/api/windows.media.playback.backgroundmediaplayer.messagereceivedfrombackground), selon la première éventualité. Il est recommandé de vous inscrire au gestionnaire de messages reçus avant d’appeler **BackgroundMediaPlayer.Current** pour la première fois afin que l’application au premier plan ne manque pas les messages envoyés à partir du processus en arrière-plan.

Pour garder active une tâche en arrière-plan, votre application devra demander une [**BackgroundTaskDeferral**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral) dans la méthode **Run** et appeler [**BackgroundTaskDeferral.Complete**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskdeferral.complete) lorsque l’instance de la tâche reçoit les événements [**Canceled**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled) ou [**Completed**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.completed). Ne créez pas de boucle ou n’attendez pas dans la méthode **Run**, car cela utilise des ressources et le système risque de mettre fin à la tâche en arrière-plan de votre application.

Votre tâche en arrière-plan obtient l’événement **Completed** lorsque la méthode **Run** est terminée et qu’aucun report n’est demandé. Dans certains cas, lorsque votre application obtient l’événement **Canceled**, il peut également être suivi de l’événement **Completed**. Votre tâche peut recevoir un événement **Canceled** pendant que **Run** est en cours d’exécution, veillez donc à gérer cette simultanéité potentielle.

Une tâche en arrière-plan peut être annulée dans les situations suivantes :

-   Une nouvelle application avec des fonctions de lecture audio démarre sur les systèmes qui appliquent la sous-stratégie d’exclusivité. Voir la section [Stratégies système pour la durée de vie de tâche audio en arrière-plan](#system-policies-for-background-audio-task-lifetime) ci-dessous.

-   Une tâche en arrière-plan a été lancée mais aucune musique n’est encore en cours de lecture ; l’application au premier plan est alors suspendue.

-   Interruptions d’autres médias, comme les appels téléphoniques entrants ou les appels VoIP.

Une tâche en arrière-plan peut être annulée sans préavis dans les situations suivantes :

-   Un appel VoIP entre et il n’existe pas suffisamment de mémoire disponible sur le système afin de maintenir la tâche en arrière-plan.

-   Une stratégie de ressource est enfreinte.

-   L’annulation ou la réalisation de la tâche ne s’est pas terminée comme prévu.

## <a name="system-policies-for-background-audio-task-lifetime"></a>Stratégies système pour la durée de vie de tâche audio en arrière-plan

Les stratégies suivantes permettent de déterminer comment le système gère la durée de vie des tâches audio en arrière-plan.

### <a name="exclusivity"></a>Exclusivité

Lorsqu’elle est activée, cette sous-stratégie limite le nombre de tâches audio en arrière-plan à 1 tout au plus, à tout moment. Elle est activée sur les appareils mobiles et sur les autres références non destinées à un ordinateur.

### <a name="inactivity-timeout"></a>Délai d’inactivité

En raison des contraintes de ressource, le système peut terminer votre tâche en arrière-plan après une période d’inactivité.

Une tâche en arrière-plan est considérée comme « inactive » si les deux conditions suivantes sont remplies :

-   L’application au premier plan n’est pas visible (elle est suspendue ou arrêtée).

-   Le lecteur multimédia en arrière-plan n’est pas en cours d’exécution.

Si ces deux conditions sont remplies, la stratégie du système multimédia en arrière-plan démarre un minuteur. Si aucune de ces conditions n’a été modifiée lorsque le minuteur expire, la stratégie du système multimédia en arrière-plan termine la tâche en arrière-plan.

### <a name="shared-lifetime"></a>Durée de vie partagée

Lorsqu’elle est activée, cette sous-stratégie force la tâche en arrière-plan à dépendre de la durée de vie de la tâche au premier plan. Si la tâche au premier plan est arrêtée, par l’utilisateur ou par le système, la tâche en arrière-plan s’arrête également.

Toutefois, cela ne signifie pas que le premier plan dépend de l’arrière-plan. Si la tâche en arrière-plan est arrêtée, cela ne force pas la tâche au premier plan à s’arrêter.

Le tableau suivant répertorie les stratégies sont appliqués selon les types d’appareils.

| Sous-stratégie             | Bureau  | Mobile   | Autres    |
|------------------------|----------|----------|----------|
| **Exclusivité**        | Désactivé | Enabled  | Enabled  |
| **Délai d’inactivité** | Désactivé | Enabled  | Désactivé |
| **Durée de vie partagée**    | Enabled  | Désactivé | Désactivé |


 

 




