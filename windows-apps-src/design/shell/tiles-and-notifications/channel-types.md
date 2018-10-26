---
author: adwilso
Description: Windows Push Notification Services (WNS) enables third-party developers to send toast, tile, badge, and raw updates from their own cloud service. There are many ways to send the notifications depending on the needs of your application
title: Choisir le type de canal de notification Push adapté
ms.author: mijacobs
ms.date: 07/07/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: db2cf1c732669a2ae45b8a5eb427e8864446c800
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5617576"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>Choisir le type de canal de notification Push adapté

Cet article couvre les trois les types de canaux de notification Push UWP (principal, secondaire et autre) qui vous permettent de remettre du contenu à votre application. 

(Pour plus d’informations sur la création des notifications push, consultez la [vue d’ensemble des Services de notifications Push Windows (WNS)](../tiles-and-notifications/windows-push-notification-services--wns--overview.md).) 

## <a name="types-of-push-channels"></a>Types de canaux Push 

Il existe trois types de canaux Push qui peuvent être utilisés pour envoyer des notifications à une application UWP. Ils sont les suivants: 

[Canal principal](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_): le canal Push «classique». Peut être utilisé par n’importe quelle application du Windows store pour envoyer des notifications toast, par vignette, brutes ou par badge (lien vers les descriptions des notifications par vignette/toast/par badge)

[Canal de vignette secondaire](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_): utilisé pour transférer des mises à jour de vignette pour une vignette secondaire. Peut être utilisé uniquement pour envoyer des notifications par vignette ou par badge à une vignette secondaire épinglée sur l’écran de démarrage de l’utilisateur

[Autre canal](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_): nouveau type de canal ajouté dans Creators Update. Il permet l’envoi de notifications brutes à n’importe quelle application UWP, y compris celles qui ne sont pas inscrites dans le Windows Store. 

> [!NOTE]
> Quel que soit le canal Push utilisé, dès que votre application est en cours d’exécution sur l’appareil, elle est toujours en mesure d’envoyer des notifications par badge, par vignette ou toast locales. Elle peut envoyer des notifications locales à partir des processus d’application au premier plan ou d’une tâche en arrière-plan. 


## <a name="primary-channels"></a>Canaux principaux

Ces canaux sont les plus couramment utilisés sur Windows actuellement et sont adaptés quasiment à tous les scénarios, dans lesquels votre application doit être distribuée par le biais du Microsoft Store. Ils permettent d’envoyer tous les types de notifications à l’application. 

### <a name="what-do-primary-channels-enable"></a>Fonctionnalités des canaux principaux?

-   **Envoi de mises à jour de vignette ou de badge à la vignette principale.** Si l’utilisateur a choisi d’épingler votre vignette à l’écran de démarrage, c’est l’occasion de la présenter. Envoi de mises à jour avec des informations utiles ou des rappels d’expériences au sein de votre application. 
-   **Envoi de notifications toast.** Les notifications toast sont l’occasion d’obtenir des informations de la part de l’utilisateur immédiatement. Elles sont affichées par l’interpréteur de commandes au-dessus de la plupart des applications et en direct dans le centre de notifications afin que l’utilisateur puisse y revenir et interagir avec celles-ci ultérieurement. 
-   **Envoi de notifications brutes pour déclencher une tâche en arrière-plan.** Parfois, vous souhaitez effectuer un travail pour le compte de l’utilisateur en vous basant sur une notification. Les notifications brutes permettent l’exécution des tâches en arrière-plan de votre application 
-   **Chiffrement des messages en transit proposé par Windows à l’aide de TLS.** Les messages sont chiffrés sur le réseau à la fois arrivant dans WNS et en accédant à l’appareil de l’utilisateur.  

### <a name="limitations-of-primary-channels"></a>Limitations des canaux principaux

-   Nécessite l’utilisation de l’API REST WNS pour transférer des notifications Push, qui n’est pas standard chez les fournisseurs d’appareils. 
-   Un seul canal peut être créé par application 
-   Exige que votre application soit enregistrée dans le Microsoft Store

### <a name="creating-a-primary-channel"></a>Création d’un canal principal 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>Canaux de vignette secondaire

Il s’agit de canaux qui peuvent être utilisés pour transférer des mises à jour de vignette et de badge vers une vignette secondaire. Ils sont utilisés par les applications pour faire part aux utilisateurs d’actions ou d’informations intéressantes avec lesquelles ils peuvent interagir au sein de l’application, comme les nouveaux messages d’une conversation de groupe ou la mise à jour d’un score de sport. 

### <a name="what-do-secondary-tile-channels-enable"></a>Fonctionnalités des canaux de vignette secondaires?

-   Envoi de notifications par vignette ou par badge à des vignettes secondaires. Les vignettes secondaires sont un excellent moyen de ramener les utilisateurs vers votre application. Il s’agit d’un lien profond vers les informations qui les intéressent. De plus, le fait de placer des informations pertinentes sur les vignettes permet de ramener sans cesse les utilisateurs vers l’application.
-   Séparation des canaux (et des expirations) en plusieurs vignettes. Cela vous permet de séparer la logique du système principal entre les différents types de vignettes secondaires qu’un utilisateur est susceptible d’épingler à son écran de démarrage. 
-   Chiffrement des messages en transit proposé par Windows à l’aide de TLS. Les messages sont chiffrés sur le réseau à la fois arrivant dans WNS et en accédant à l’appareil de l’utilisateur.  

### <a name="limitations-of-secondary-tile-channels"></a>Limitations des canaux de vignettes secondaires

-   Aucune notification toast ou brute n’est autorisée. Les notifications toast ou brutes envoyées à une vignette secondaire sont ignorées par le système.
-   Exige que votre application soit enregistrée dans le Microsoft Store


### <a name="creating-a-secondary-tile-channel"></a>Création d’un canal de vignette secondaire 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>Autres canaux

Les autres canaux permettent aux applications d’envoyer des notifications Push sans enregistrement dans le Microsoft Store ou création de canaux Push en dehors du canal principal utilisé pour l’application. 
 
### <a name="what-do-alternate-channels-enable"></a>Fonctionnalités des autres canaux?
-   Envoi de notifications Push brutes à un UWP s’exécutant sur n’importe quel appareil Windows. Les autres canaux autorisent uniquement les notifications brutes.
-   Permet aux applications de créer plusieurs canaux Push bruts pour différentes fonctionnalités au sein de l’application. Une application peut créer jusqu’à 1000 autres canaux, et chacun d’eux est valide pendant 30jours. Chacun de ces canaux peut être géré ou révoqué séparément par l’application.
-   Les autres canaux Push peuvent être créés sans enregistrer d’application dans le Microsoft Store. Si votre application doit être installée sur des appareils sans l’enregistrer dans le Microsoft Store, elle sera toujours en mesure de recevoir des notifications Push.
-   Les serveurs peuvent transférer des notifications à l’aide des API REST standard W3C et du protocole VAPID. Les autres canaux utilisent le protocole standard W3C, ce qui vous permet de simplifier la logique du serveur qui doit être gérée.
-   Chiffrement complet de bout en bout des messages. Le canal principal assure le chiffrement en cours de transit. Toutefois, si vous souhaitez un niveau de sécurité supplémentaire, les autres canaux permettent à votre application de passer à travers des en-têtes de chiffrement pour protéger un message. 

### <a name="limitations-of-alternate-channels"></a>Limitations des autres canaux
-   Les applications ne peuvent pas envoyer de notifications toast, par vignette ou par badge. L’autre canal limite la possibilité d’envoyer d’autres types de notifications. Votre application est toujours en mesure d’envoyer des notifications locales à partir de votre tâche en arrière-plan. 
-   Nécessite une API REST différente des canaux de vignette principaux ou secondaires. L’utilisation de l’API REST W3C standard signifie que votre application devra avoir une logique différente pour l’envoi de mises à jour de notifications Push toast ou par vignette

### <a name="creating-an-alternate-channel"></a>Création d’un autre canal 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>Comparaison des types de canaux
Voici une comparaison rapide des différents types de canaux:

<table>

<tr class="header">
<th align="left"><b>Type</b></th>
<th align="left"><b>Notifications Push toast?</b></th>
<th align="left"><b>Notifications Push par vignette/badge?</b></th>
<th align="left"><b>Notifications Push brutes?</b></th>
<th align="left"><b>Authentification</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>Enregistrement dans le Windows Store requis?</b></th>
<th align="left"><b>Canaux</b></th>
<th align="left"><b>Chiffrement</b></th>
</tr>


<tr class="odd">
<td align="left">Principal</td>
<td align="left">Oui</td>
<td align="left">Oui - vignette principale uniquement</td>
<td align="left">Oui</td>
<td align="left">OAuth</td>
<td align="left">API REST WNS</td>
<td align="left">Oui</td>
<td align="left">Une par application</td>
<td align="left">En cours de transit</td>
</tr>
<tr class="even">
<td align="left">Vignette secondaire</td>
<td align="left">Non</td>
<td align="left">Oui - vignette secondaire uniquement</td>
<td align="left">Non</td>
<td align="left">OAuth</td>
<td align="left">API REST WNS</td>
<td align="left">Oui</td>
<td align="left">Une par vignette secondaire</td>
<td align="left">En cours transit</td>
</tr>
<tr class="odd">
<td align="left">Autre</td>
<td align="left">Non</td>
<td align="left">Non</td>
<td align="left">Oui</td>
<td align="left">VAPID</td>
<td align="left">WebPush W3C standard</td>
<td align="left">Non</td>
<td align="left">1000 par application</td>
<td align="left">Chiffrement en cours de transit et de bout en bout possible avec passage à travers des en-têtes (nécessite le code d’application)</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>Choix du canal adapté

En règle générale, nous vous recommandons d’utiliser le canal principal dans votre application, à quelques exceptions près: 

1. Si vous transférez une mise à jour par vignette vers une vignette secondaire, utilisez le canal Push de vignette secondaire.
2. Si vous distribuez des canaux à d’autres services (par exemple, dans le cas d’un navigateur), utilisez l’autre canal.
3. Si vous créez une application qui n’est pas répertoriée dans le Windows store (par exemple, une application métier), utilisez un autre canal.
4. Si vous souhaitez réutiliser du code Push web existant sur votre serveur ou si vous avez besoin de plusieurs canaux dans le service de votre système principal, utilisez les autres canaux.

## <a name="related-articles"></a>Articles connexes

* [Envoyer une notification par vignette locale](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Notifications toast adaptatives et interactives](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [Démarrage rapide: envoi d’une notification Push](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [Comment mettre à jour un badge via des notifications Push](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [Comment demander, créer et enregistrer un canal de notification](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [Comment intercepter les notifications pour les applications en cours d’exécution](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [Comment s’authentifier auprès des services de notifications Push Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [En-têtes des demandes et des réponses du service de notifications Push](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [Recommandations et liste de vérification sur les notifications Push](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Notifications brutes](raw-notification-overview.md)
