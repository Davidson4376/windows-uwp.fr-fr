---
author: Xansky
Description: Learn how to register your UWP app to receive push notifications that you send from Partner Center.
title: Configurer votre application pour les notifications Push ciblées
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store Services SDK, ciblées des notifications push, l’espace partenaires
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: 1d1281436ce0fe8c7b04429cea897eedc58b15d9
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7296509"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>Configurer votre application pour les notifications Push ciblées

Vous pouvez utiliser la page de **notifications Push** dans l’espace partenaires pour entrer directement en contact avec les clients en envoyant des notifications push ciblées aux appareils sur lesquels votre application de plateforme Windows universelle (UWP) est installée. Vous pouvez utiliser des notifications push ciblées afin d’inciter vos clients à effectuer une action, par exemple évaluer une application ou essayer une nouvelle fonctionnalité. Vous pouvez envoyer différents types de notificationsPush, dont les notificationstoast, les notifications par vignette et les notifications XML brutes. Vous pouvez également effectuer le suivi des lancements d’applications provoqués par vos notificationsPush. Pour plus d’informations sur cette fonctionnalité, consultez la page [Envoyer des notifications Push aux clients de vos applications](../publish/send-push-notifications-to-your-apps-customers.md).

Avant de pouvoir envoyer des notifications push ciblées à vos clients à partir de l’espace partenaires, vous devez utiliser une méthode de la classe [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) dans le Microsoft Store Services SDK pour inscrire votre application pour recevoir des notifications. Vous pouvez utiliser les méthodes supplémentaires de cette classe pour signaler au centre de l’espace que votre application a été lancée en réponse à une notification push ciblée (si vous souhaitez suivre la fréquence des lancements d’applications provoqués par vos notifications) et d’arrêter de recevoir des notifications.

## <a name="configure-your-project"></a>Configurer votre projet

Avant d’écrire du code, suivez ces étapes afin d’ajouter une référence au Microsoft Store Services SDK dans votre projet:

1. Si vous ne l’avez pas encore fait, [installez le Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) sur votre ordinateur de développement. 
2. Ouvrez votre projet dans Visual Studio.
3. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud **Références**, puis sélectionnez **Ajouter une référence**.
4. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.
5. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.

## <a name="register-for-push-notifications"></a>Inscrire pour les NotificationsPush

Pour inscrire votre application pour recevoir des notifications push ciblées à partir de l’espace partenaires:

1. Dans votre projet, recherchez une section de code s’exécutant au démarrage, dans laquelle vous pouvez inscrire votre application pour la réception des notifications.
2. Ajoutez l’instruction suivante en haut du fichier de code.

    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. Récupérez un objet [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) et appelez l’une des surcharges [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) dans le code de démarrage identifié plus tôt. Cette méthode doit être appelée à chaque lancement de l’application.

  * Si vous souhaitez que l’espace partenaires pour créer sa propre URI de canal pour les notifications, appelez la surcharge de [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) .

      [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]
      > [!IMPORTANT]
      > Si votre application appelle [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) pour créer un canal de notification pour WNS, vérifiez que votre code n’appelle pas simultanément la surcharge [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) et la surcharge [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync). Si vous devez appeler les deuxméthodes, assurez-vous de les appeler séquentiellement et d’attendre le retour d’une méthode avant d’appeler l’autre.

  * Si vous souhaitez spécifier l’URI de canal à utiliser pour les notifications push ciblées à partir de l’espace partenaires, appelez la surcharge de [registernotificationchannelasync (storeservicesnotificationchannelparameters)](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) . Par exemple, vous pouvez procéder ainsi si votre application utilise déjà les services de notifications Push Windows (WNS) et que vous souhaitez utiliser le même URI de canal. Vous devez dans un premier temps créer l’objet [StoreServicesNotificationChannelParameters](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) et affecter la propriété [CustomNotificationChannelUri](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) à votre URI de canal.

      [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

> [!NOTE]
> Lorsque vous appelez la méthode **RegisterNotificationChannelAsync**, un fichier nommé MicrosoftStoreEngagementSDKId.txt est créé dans le magasin de données d’application local pour votre application (le dossier renvoyé par la propriété [ApplicationData.LocalFolder](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalFolder)). Ce fichier contient un ID utilisé par l’infrastructure des notifications Push ciblées. Assurez-vous que votre application ne modifie ni ne supprime ce fichier. Dans le cas contraire, vos utilisateurs risquent de recevoir plusieurs instances de notifications, ou les notifications peuvent ne pas se comporter correctement.

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>Comment les notifications Push ciblées sont dirigées vers les clients

Lorsque votre application appelle **RegisterNotificationChannelAsync**, cette méthode recueille le compte Microsoft du client actuellement connecté à l’appareil. Plus tard, lorsque vous envoyez une notification push ciblée à un segment incluant ce client, l’espace partenaires envoie la notification aux périphériques qui sont associés au compte Microsoft de ce client.

Si le client qui a démarré votre application confie son appareil à un tiers qui l'utilise alors que celui-ci est toujours connecté au compte Microsoft du client, n’oubliez pas que cette personne peut voir la notification ciblant le client d’origine. Cela peut avoir des conséquences inattendues, en particulier concernant les applications qui offrent des services que les clients peuvent utiliser lorsqu'ils se connectent. Pour empêcher les autres utilisateurs de voir vos notifications ciblées dans un tel cas, utilisez la méthode [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) lorsque les clients se déconnectent de votre application. Pour plus d’informations, voir [Annuler l’inscription pour les notifications push](#unregister) plus loin dans cet article.

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>Réaction de l’application lancée par l’utilisateur

Une fois que votre application est configurée pour recevoir des notifications et que vous [envoyez une notification push aux clients de votre application à partir de l’espace partenaires](../publish/send-push-notifications-to-your-apps-customers.md), un des points d’entrée suivants dans votre application sera appelé lorsque l’utilisateur lance votre application en réponse à vos push notification. Si vous possédez du code à exécuter lorsque l’utilisateur lance votre application, vous pouvez l’ajouter à l’un de ces points d’entrée dans votre application.

  * Si la notificationPush présente un type d’activation au premier plan, supprimez la méthode [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) de la classe **App** dans votre projet et ajoutez votre code à cette méthode.

  * Si la notificationPush présente un type d’activation en arrière-plan, ajoutez votre code à la méthode [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) de votre [tâche en arrière-plan](../launch-resume/support-your-app-with-background-tasks.md).

Par exemple, vous pouvez récompenser les utilisateurs de votre application qui ont fait l’acquisition d’extensions payantes en leur octroyant gratuitement une autre extension. Dans ce cas, vous pouvez envoyer une notificationPush à un [segment de clients](../publish/create-customer-segments.md) ciblant ces utilisateurs. Ensuite, vous pouvez ajouter du code afin de leur procurer un [achat in-app](in-app-purchases-and-trials.md) dans l’un des points d’entrée répertoriés ci-dessus.

## <a name="notify-partner-center-of-your-app-launch"></a>Signaler au centre de l’espace de votre lancement de l’application

Si vous sélectionnez l’option de **suivre la fréquence de lancement de l’application** pour votre notification push ciblée dans l’espace partenaires, appelez la méthode [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) à partir du point d’entrée appropriée dans votre application afin de signaler l’espace partenaires que votre application a été lancée en réponse à une notification push.

Cette méthode renvoie également les arguments de lancement d’origine associés à votre application. Lorsque vous choisissez de suivre la fréquence de lancement de l’application pour votre notification push, un suivi opaque ID est ajouté aux arguments de lancement afin de faciliter le suivi de l’application de lancement dans l’espace partenaires. Vous devez transmettre les arguments de lancement de votre application à la méthode [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) , et cette méthode envoie l’ID de suivi à l’espace partenaires, le retire des arguments de lancement et renvoie les arguments de lancement d’origine à votre code.

La méthode d’appel de cette méthode dépend du type d’activation de la notificationPush:

* Si la notificationPush présente un type d’activation au premier plan, appelez cette méthode depuis la substitution de méthode [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) de votre application et communiquez les arguments disponibles dans l’objet [ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) transmis à cette méthode. L’exemple de code suivant suppose que votre fichier de code contient des instructions **using** pour les espaces de noms **Microsoft.Services.Store.Engagement** et **Windows.ApplicationModel.Activation**.

  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* Si la notificationPush présente un type d’activation en arrière-plan, appelez cette méthode depuis la méthode [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) de votre [tâche en arrière-plan](../launch-resume/support-your-app-with-background-tasks.md) et transmettez les arguments qui sont disponibles dans l’objet [ToastNotificationActionTriggerDetail](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) transmis à cette méthode. L’exemple de code suivant suppose que votre fichier de code contient des instructions **using** pour les espaces de noms **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** et **Windows.UI.Notifications**.

  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>Annuler l’inscription aux notificationsPush

Si vous souhaitez que votre application cesse de recevoir des notifications push ciblées à partir de l’espace partenaires, appelez la méthode [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) .

[!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

Notez que cette méthode invalide le canal utilisé pour les notifications et donc que l’application ne reçoit plus de notificationsPush d’*aucun* service. Une fois clôturé, le canal ne peut pas encore être utilisé pour tous les services, y compris les notifications push ciblées à partir de l’espace partenaires et autres notifications utilisant WNS. Pour réactiver l’envoi des notificationsPush à cette application, l’application doit demander un nouveau canal.

## <a name="related-topics"></a>Rubriques connexes

* [Envoyer des notifications push ciblées aux clients de votre application](../publish/send-push-notifications-to-your-apps-customers.md)
* [Vue d’ensemble des services de notifications Push Windows (WNS)](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)
* [Comment demander, créer et enregistrer un canal de notification](https://docs.microsoft.com/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](https://docs.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
