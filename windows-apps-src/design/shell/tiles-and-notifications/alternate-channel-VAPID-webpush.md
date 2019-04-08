---
title: Canaux de push autre à l’aide de Webpush et VAPID dans UWP
description: Instructions pour l’utilisation des canaux de push autre avec le protocole VAPID à partir d’une application UWP
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, uwp, API WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: ba8630a2e877345adeac7eb443dd3e418d3ed277
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639524"
---
# <a name="alternate-push-channels-using-webpush-and-vapid-in-uwp"></a>Canaux de push autre à l’aide de Webpush et VAPID dans UWP 
À partir de Fall Creators Update, les applications UWP peuvent utiliser des push de web avec l’authentification VAPID pour envoyer des notifications push.  

## <a name="introduction"></a>Introduction
L’introduction de la norme de push web permet de sites Web peuvent agir plus comme les applications, même lorsque les utilisateurs ne sont pas sur le site Web, l’envoi de notifications.

Le protocole d’authentification VAPID a été créé pour autoriser les sites Web pour s’authentifier auprès des serveurs de push dans un fournisseur de façon indépendante. Avec tous les fournisseurs à l’aide du protocole VAPID, sites Web peuvent envoyer des notifications push sans connaître le navigateur sur lequel il est en cours d’exécution. Il s’agit une amélioration significative sur l’implémentation d’un protocole de push différents pour chaque plateforme. 

Les applications UWP peuvent utiliser webpush et VAPID pour envoyer des notifications push avec ces avantages. Ces protocoles peuvent gagner du temps de développement pour les nouvelles applications et simplifier la prise en charge multiplateforme pour les applications existantes. En outre, les applications d’entreprise ou des versions test d’applications peut maintenant envoyer des notifications sans l’enregistrer dans le Microsoft Store. J’espère que vous ouvrirez ainsi les nouvelles façons d’interagir avec les utilisateurs sur toutes les plateformes.  

## <a name="alternate-channels"></a>Autres canaux 
Dans la plateforme Windows universelle, ces canaux VAPID est appelées des autres canaux et fournissent des fonctionnalités similaires à un canal de push web. Ils peuvent déclencher une tâche d’arrière-plan d’application pour exécuter, activer le chiffrement de message et pour plusieurs canaux à partir d’une seule application. Pour plus d’informations sur la différence entre les différents types de canaux, consultez [en choisissant le bon canal](channel-types.md).

À l’aide de canaux de l’autre est un excellent moyen d’accéder à des notifications push si votre application ne peut pas utiliser un canal primaire ou si vous souhaitez partager du code entre votre site Web et votre application. Configuration d’un canal est simple et familier pour quiconque a utilisé la norme de push web ou travaillé avec Windows notifications push avant.

## <a name="code-example"></a>Exemple de code

Le processus de base de configuration d’un canal de remplacement pour une application UWP est similaire à la configuration d’un canal primaire ou secondaire. Tout d’abord, inscrivez-vous à un canal avec le [server WNS](windows-push-notification-services--wns--overview.md). Ensuite, inscrivez-vous pour s’exécuter comme une tâche en arrière-plan. Une fois que la notification est envoyée et la tâche en arrière-plan est déclenchée, gérez l’événement.  

### <a name="get-a-channel"></a>Obtenir un canal 
Pour créer un autre canal, l’application doit fournir deux informations : la clé publique pour son serveur et le nom du canal en cours de création. Les détails sur les clés de serveur sont disponibles dans la spécification de push web, mais nous recommandons d’utiliser une bibliothèque de push web standard sur le serveur pour générer les clés.  

ID du canal est particulièrement important, car une application peut créer plusieurs canaux de remplacement. Chaque canal doit être identifiée par un ID unique qui sera inclus dans les charges utiles de notification envoyés le long de ce canal.  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
L’application envoie le canal sauvegarder sur son serveur et l’enregistre localement. L’enregistrement de l’ID du canal local permet à l’application faire la distinction entre canaux et renouveler des canaux afin d’éviter leur expiration.

Comme tous les autres types de canal de notification push, canaux web peuvent expirer. Pour empêcher les canaux ne doit pas expirer sans connaître à votre application, créez un nouveau canal chaque fois que votre application est lancée.    

### <a name="register-for-a-background-task"></a>S’inscrire à une tâche en arrière-plan 

Une fois que votre application a créé un autre canal, il doit s’inscrire pour recevoir les notifications dans le premier plan ou d’arrière-plan. L’exemple ci-dessous inscrit pour utiliser le modèle de processus d’un pour recevoir les notifications en arrière-plan.  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>Recevoir les notifications 

Une fois que l’application s’est inscrit pour recevoir les notifications, il doit être en mesure de traiter les notifications entrantes. Dans la mesure où une seule application peut inscrire plusieurs canaux, veillez à vérifier l’ID du canal avant de traiter la notification.  

```csharp
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args) 
{ 
    base.OnBackgroundActivated(args); 
    var raw = args.TaskInstance.TriggerDetails as RawNotification; 
    switch (raw.ChannelId) 
    { 
        case "Football": 
            AppPostFootballScore(raw.Content); 
            break; 
        case "News": 
            AppProcessNewsItem(raw.Content); 
            break; 
        case "Baseball": 
            AppProcessBaseball(raw.Content); 
            break; 
 
        default: 
            //We don't have the channelID registered, should only happen in the case of a 
            //caching issue on the application server 
            break; 
    }                           
} 
```

Notez que si la notification provient d’un canal principal, puis l’ID du canal ne sera pas définie.  

## <a name="note-on-encryption"></a>Note sur le chiffrement 

Vous pouvez utiliser tout schéma de chiffrement vous jugez plus utiles pour votre application. Dans certains cas, il est suffisant pour s’appuient sur la norme TLS entre le serveur et n’importe quel appareil Windows. Dans d’autres cas, il peut être plus judicieux d’utiliser le schéma de chiffrement de push web ou un autre schéma de votre conception.  

Si vous souhaitez utiliser une autre forme de chiffrement, la clé est l’utilisation du texte brute. Propriété Headers. Il contient tous les en-têtes de chiffrement qui ont été inclus dans la requête POST vers le serveur de push. À partir de là, votre application peut utiliser les clés pour déchiffrer le message.  

## <a name="related-topics"></a>Rubriques connexes
- [Types de canaux de notification](channel-types.md)
- [Services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Classe de PushNotificationChannel](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [Classe de PushNotificationChannelManager](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)


