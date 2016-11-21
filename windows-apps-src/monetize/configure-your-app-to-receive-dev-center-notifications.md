---
author: mcleanbyron
Description: "Découvrez comment inscrire votre applicationUWP pour la réception de notificationsPush envoyées depuis le Centre de développement Windows."
title: "Configurer votre application pour recevoir des notifications Push du Centre de développement"
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: 0e6ac52f1e76c0f59cc428b2ff26dc524e93cbde

---

# Configurer votre application pour recevoir des notifications Push du Centre de développement

Vous pouvez utiliser la page **Notifications Push** du tableau de bord du Centre de développement Windows afin d’entrer directement en contact avec les clients en envoyant des notificationsPush aux appareils sur lesquels votre applicationUWP est installée. Vous pouvez utiliser des notifications push ciblées afin d’inciter vos clients à effectuer une action, par exemple évaluer une application ou essayer une nouvelle fonctionnalité. Vous pouvez envoyer différents types de notificationsPush, dont les notificationstoast, les notifications par vignette et les notifications XML brutes. Vous pouvez également effectuer le suivi des lancements d’applications provoqués par vos notificationsPush. Pour plus d’informations sur cette fonctionnalité, consultez la page [Envoyer des notifications Push aux clients de vos applications](../publish/send-push-notifications-to-your-apps-customers.md).

Pour pouvoir envoyer des notificationsPush à vos clients à partir du Centre de développement, vous devez utiliser une méthode de la classe [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) du Microsoft Store Services SDK afin d’inscrire votre application pour la réception de notifications. Vous pouvez utiliser des méthodes supplémentaires de cette classe pour signaler au Centre de développement que votre application a été lancée en réaction à une notificationPush ciblée (si vous souhaitez effectuer le suivi de la fréquence des lancements de l’application provoqués par vos notifications) et pour arrêter la réception des notifications.

## Configurer votre projet

Avant d’écrire du code, suivez ces étapes afin d’ajouter une référence au Microsoft Store Services SDK dans votre projet:

1. Si vous ne l’avez pas encore fait, [Installez le Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) sur votre ordinateur de développement. Outre l’API dédiée à l’inscription d’une application pour la réception de notifications, ce SDK fournit des API pour d’autres fonctionnalités, comme l’exécution d’expériences dans vos applications avec des testsA/B et l’affichage d’annonces publicitaires. 
2. Ouvrez votre projet dans Visual Studio.
3. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud **Références**, puis sélectionnez **Ajouter une référence**.
4. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.
5. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.

## Inscrire pour les NotificationsPush

Pour inscrire votre application pour la réception de notificationsPush ciblées à partir du Centre de développement:

1. Dans votre projet, rechercher une section de code s’exécutant au démarrage, dans laquelle vous pouvez inscrire votre application pour la réception des notifications du Centre de développement.
2. Ajoutez l’instruction suivante dans la partie supérieure du fichier de code.

  ```csharp
  using Microsoft.Services.Store.Engagement;
  ```

3. Récupérez un objet [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) et appelez l’une des surcharges [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync.aspx) dans le code de démarrage identifié plus tôt. Cette méthode doit être appelée à chaque lancement de l’application.

  * Si vous souhaitez que le Centre de développement crée son propre URI de canal pour les notifications, appelez la surcharge [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx).

    ```csharp
    StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
    await engagementManager.RegisterNotificationChannelAsync();
    ```

    >**Important**&nbsp;&nbsp;Si votre application appelle également [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) pour créer un canal de notification pour WNS, assurez-vous que votre code n’appelle pas simultanément les surcharges [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) et [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx). Si vous devez appeler les deuxméthodes, assurez-vous de les appeler séquentiellement et d’attendre le retour d’une méthode avant d’appeler l’autre.

  * Si vous souhaitez spécifier l’URI de canal à utiliser pour les notificationsPush ciblées du Centre de développement, appelez la surcharge [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://msdn.microsoft.com/library/windows/apps/mt771191.aspx). Par exemple, vous pouvez procéder ainsi si votre application utilise déjà les services de notifications Push Windows (WNS) et que vous souhaitez utiliser le même URI de canal. Vous devez dans un premier temps créer l’objet [StoreServicesNotificationChannelParameters](https://msdns.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.aspx) et affecter la propriété [CustomNotificationChannelUri](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri.aspx) à votre URI de canal.

    ```csharp
    StoreServicesNotificationChannelParameters parameters =
        new StoreServicesNotificationChannelParameters();
    parameters.CustomNotificationChannelUri = "Assign your channel URI here";

    StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
    await engagementManager.RegisterNotificationChannelAsync(parameters);
    ```

  >#### Présentation de la réaction d’une application lancée par l’utilisateur

  >Une fois que votre application est inscrite pour la réception des notifications et que vous [envoyez une notificationPush aux clients de votre application à partir du Centre de développement](../publish/send-push-notifications-to-your-apps-customers.md), l’un des points d’entrée suivants de votre application est appelé lorsque votre application est lancée par votre utilisateur en réaction à votre notificationPush. Si vous possédez du code à exécuter lorsque l’utilisateur lance votre application, vous pouvez l’ajouter à l’un de ces points d’entrée dans votre application.

  >* Si la notificationPush présente un type d’activation au premier plan, supprimez la méthode [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) de la classe **App** dans votre projet et ajoutez votre code à cette méthode.

  >* Si la notificationPush présente un type d’activation en arrière-plan, ajoutez votre code à la méthode [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) de votre [tâche en arrière-plan](../launch-resume/support-your-app-with-background-tasks.md).

  >Par exemple, vous pouvez récompenser les utilisateurs de votre application qui ont fait l’acquisition d’extensions payantes en leur octroyant gratuitement une autre extension. Dans ce cas, vous pouvez envoyer une notificationPush à un [segment de clients](../publish/create-customer-segments.md) ciblant ces utilisateurs. Ensuite, vous pouvez ajouter du code afin de leur procurer un [achat in-app](in-app-purchases-and-trials.md) dans l’un des points d’entrée répertoriés ci-dessus.

## Signaler le lancement d’application au Centre de développement

Si vous sélectionnez l’option **Suivre la fréquence de lancement des applications** pour une notificationPush du Centre de développement, appelez la méthode [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) à partir du point d’entrée approprié de votre application afin de signaler au Centre de développement que votre application a été lancée en réaction à une notificationPush.

Cette méthode renvoie également les arguments de lancement d’origine associés à votre application. Une fois que vous avez choisi de suivre la fréquence des lancements de vos applications pour une notificationPush du Centre de développement, un ID de suivi opaque est ajouté aux arguments de lancement afin de faciliter le suivi du lancement des applications dans le Centre de développement. Vous devez transmettre les arguments de lancement de votre application à la méthode [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx), qui envoie l’ID de suivi au Centre de développement, le retire des arguments de lancement et renvoie les arguments de lancement d’origine à votre code.

La méthode d’appel de cette méthode dépend du type d’activation de la notificationPush ciblée:

* Si la notificationPush présente un type d’activation au premier plan, appelez cette méthode depuis la substitution de méthode  [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) de votre application et communiquez les arguments disponibles dans l’objet [ToastNotificationActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.toastnotificationactivatedeventargs.aspx) transmis à cette méthode. L’exemple de code suivant suppose que votre fichier de code présente des instructions **using** pour les espaces de noms **Microsoft.Services.Store.Engagement** et **Windows.ApplicationModel.Activation**.

  ```csharp
  protected override void OnActivated(IActivatedEventArgs args)
  {
       base.OnActivated(args);   

       if (args is ToastNotificationActivatedEventArgs)
       {
             var toastActivationArgs = args as ToastNotificationActivatedEventArgs;

             StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
             string originalArgs = engagementManager.ParseArgumentsAndTrackAppLaunch(
                 toastActivationArgs.Argument);

             // Use the originalArgs variable to access the original arguments
             // that were passed to the app.
       }
  }
  ```

* Si la notificationPush présente un type d’activation en arrière-plan, appelez cette méthode depuis la méthode [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) pour votre [tâche en arrière-plan](../launch-resume/support-your-app-with-background-tasks.md) et communiquez les arguments qui sont disponibles dans l’objet [ToastNotificationActionTriggerDetail](https://msdn.microsoft.com/library/windows/apps/windows.ui.notifications.toastnotificationactiontriggerdetail.aspx) transmis à cette méthode. L’exemple de code suivant suppose que votre fichier de code présente des instructions **using** pour les espaces de noms **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** et **Windows.UI.Notifications**.

  ```csharp
  public void Run(IBackgroundTaskInstance taskInstance)
  {
       var details = taskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;

       if (details != null)
       {
            StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
            string originalArgs = engagementManager.ParseArgumentsAndTrackAppLaunch(details.Argument);

            // Use the originalArgs variable to access the original arguments
            // that were passed to the app.
       }
  }
  ```

## Annuler l’inscription aux notifications Push

Si vous souhaitez que votre application cesse de recevoir les notificationsPush ciblées du Centre de développement Windows, appelez la méthode [UnregisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync).

```csharp
StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
await engagementManager.UnregisterNotificationChannelAsync();
```

Notez que cette méthode invalide le canal utilisé pour les notifications, de manière à ce que l’application ne reçoive de notificationsPush d’*aucun* service. Une fois clôturé, le canal n’est plus utilisable par aucun service, notamment par les notificationsPush ciblées du Centre de développement Windows et d’autres notifications utilisantWNS. Pour réactiver l’envoi des notificationsPush à cette application, l’application doit demander un nouveau canal.

## Rubriques connexes

* [Envoyer des notifications push ciblées aux clients de votre application](../publish/send-push-notifications-to-your-apps-customers.md)
* [Vue d’ensemble des services de notifications Push Windows (WNS)](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [Comment demander, créer et enregistrer un canal de notification](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh868221)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Nov16_HO1-->


