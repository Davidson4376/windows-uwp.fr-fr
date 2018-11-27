---
Description: Learn how to use Notification Listener to access all of the user's notifications.
title: Écouteur de notification
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tiles
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: windows10, uwp, écouteur de notification, usernotificationlistener, documentation, notifications d’accès
ms.localizationpriority: medium
ms.openlocfilehash: c0717fb3d1db42483214e8396d436c47c23744ee
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7703689"
---
# <a name="notification-listener-access-all-notifications"></a>Écouteur de notification: accéder à toutes les notifications

L'Écouteur de notification donne accès à toutes les notifications d'un utilisateur. Les montres intelligentes et d'autres appareils portables peuvent utiliser l’écouteur de notification pour envoyer les notifications du téléphone à l’appareil portable. Les applications de domotique peuvent utiliser l'écouteur de notification pour effectuer des actions spécifiques lors de la réception de notifications, par exemple, faire clignoter les lumières lorsque vous recevez un appel. 

> [!IMPORTANT]
> **Nécessite la mise à jour anniversaire**: vous devez cibler le Kit de développement logiciel (SDK)14393 et exécuter la Build14993 ou une version supérieure pour utiliser l'Écouteur de notification.


> **API importantes**: [classe UserNotificationListener](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.Management.UserNotificationListener), [classe UserNotificationChangedTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.UserNotificationChangedTrigger)


## <a name="enable-the-listener-by-adding-the-user-notification-capability"></a>Activer l'écouteur en ajoutant la fonctionnalité Notification à l'utilisateur 

Pour utiliser l’écouteur de notification, vous devez ajouter la fonctionnalité Écouteur de notification à l'utilisateur à votre manifeste d’application.

1. Dans Visual Studio, dans l'Explorateur de solutions, double-cliquez sur votre fichier `Package.appxmanifest` pour ouvrir le concepteur du manifeste.
2. Ouvrez l’onglet Fonctionnalités.
3. Cochez la fonctionnalité **Écouteur de notification à l'utilisateur**.


## <a name="check-whether-the-listener-is-supported"></a>Vérifier si l’écouteur est pris en charge

Si votre application prend en charge des versions antérieures de Windows10, vous devez utiliser la [classe ApiInformation](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation) pour vérifier si l’écouteur est pris en charge.  Si l’écouteur n'est pas pris en charge, évitez d’exécuter des appels vers les API de l'écouteur.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Notifications.Management.UserNotificationListener"))
{
    // Listener supported!
}
 
else
{
    // Older version of Windows, no Listener
}
```


## <a name="requesting-access-to-the-listener"></a>Demande d’accès à l’écouteur

Dans la mesure où l’écouteur permet d’accéder aux notifications de l’utilisateur, les utilisateurs doivent accorder à votre application l’autorisation d’accéder à leurs notifications. Lors de la première exécution de votre application, vous devez demander l’accès pour utiliser l’écouteur de notification. Si vous le souhaitez, vous pouvez afficher une interface utilisateur préliminaire qui explique pourquoi votre application a besoin d’accéder aux notifications de l’utilisateur avant d’appeler [RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.RequestAccessAsync), de sorte que l’utilisateur comprenne pourquoi il doit autoriser l’accès.

```csharp
// Get the listener
UserNotificationListener listener = UserNotificationListener.Current;
 
// And request access to the user's notifications (must be called from UI thread)
UserNotificationListenerAccessStatus accessStatus = await listener.RequestAccessAsync();
 
switch (accessStatus)
{
    // This means the user has granted access.
    case UserNotificationListenerAccessStatus.Allowed:
 
        // Yay! Proceed as normal
        break;
 
    // This means the user has denied access.
    // Any further calls to RequestAccessAsync will instantly
    // return Denied. The user must go to the Windows settings
    // and manually allow access.
    case UserNotificationListenerAccessStatus.Denied:
 
        // Show UI explaining that listener features will not
        // work until user allows access.
        break;
 
    // This means the user closed the prompt without
    // selecting either allow or deny. Further calls to
    // RequestAccessAsync will show the dialog again.
    case UserNotificationListenerAccessStatus.Unspecified:
 
        // Show UI that allows the user to bring up the prompt again
        break;
}
```

L’utilisateur peut révoquer l’accès à tout moment via les paramètres de Windows. Par conséquent, votre application doit toujours vérifier l’état de l’accès via la méthode [GetAccessStatus](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetAccessStatus) avant d'exécuter le code utilisant l’écouteur de notification. Si l’utilisateur révoque l’accès, l’API échouera sans avertissement plutôt que de lever une exception (par exemple, l’API permettant d'obtenir toutes les notifications renverra simplement une liste vide).


## <a name="access-the-users-notifications"></a>Accès aux notifications de l’utilisateur

Avec l’écouteur de notification, vous pouvez obtenir une liste des notifications actuelles de l’utilisateur. Il vous suffit d’appeler la méthode [GetNotificationsAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetNotificationsAsync) et de spécifier le type de notifications que vous souhaitez obtenir (actuellement, seules les notifications toast sont prises en charge).

```csharp
// Get the toast notifications
IReadOnlyList<UserNotification> notifs = await listener.GetNotificationsAsync(NotificationKinds.Toast);
```


## <a name="displaying-the-notifications"></a>Affichage des notifications

Chaque notification est représentée sous forme de [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification), qui fournit des informations sur l’application d'où provient la notification, l'heure à laquelle la notification a été créée, l’ID de la notification et la notification elle-même.

```csharp
public sealed class UserNotification
{
    public AppInfo AppInfo { get; }
    public DateTimeOffset CreationTime { get; }
    public uint Id { get; }
    public Notification Notification { get; }
}
```

La propriété [AppInfo](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.AppInfo) fournit les informations dont vous avez besoin pour afficher la notification.

> [!NOTE]
> Nous vous recommandons d'insérer tout votre code de traitement d’une notification unique dans un bloc try/catch, au cas où une exception inattendue se produirait lorsque vous capturez une seule notification. L'affichage d'autres notifications ne doit pas échouer complètement juste à cause d’un problème lié à une notification spécifique.

```csharp
// Select the first notification
UserNotification notif = notifs[0];
 
// Get the app's display name
string appDisplayName = notif.AppInfo.DisplayInfo.DisplayName;
 
// Get the app's logo
BitmapImage appLogo = new BitmapImage();
RandomAccessStreamReference appLogoStream = notif.AppInfo.DisplayInfo.GetLogo(new Size(16, 16));
await appLogo.SetSourceAsync(await appLogoStream.OpenReadAsync());
```

Le contenu de la notification elle-même, par exemple, le texte de la notification, est contenu dans la propriété [Notification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.Notification). Cette propriété contient la partie visuelle de la notification. (Si vous êtes familiarisé avec l’envoi de notifications sur Windows, vous remarquerez que les propriétés [Visual](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification.Visual) et [Visual.Bindings](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationvisual.Bindings) de l'objet [Notification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification) correspondent à ce que les développeurs envoient lors de l'affichage d'une notification.)

Nous voulons rechercher la liaison de toast (pour un code infaillible, vous devez vérifier que la liaison n’a pas la valeur null). À partir de la liaison, vous pouvez obtenir les éléments de texte. Vous pouvez choisir d’afficher autant d'éléments de texte que vous le souhaitez. (Dans l’idéal, vous devez tous les afficher). Vous pouvez choisir de traiter les éléments de texte de façon différente; par exemple, traiter le premier comme le texte de titre et les éléments suivants comme le corps du texte.

```csharp
// Get the toast binding, if present
NotificationBinding toastBinding = notif.Notification.Visual.GetBinding(KnownNotificationBindings.ToastGeneric);
 
if (toastBinding != null)
{
    // And then get the text elements from the toast binding
    IReadOnlyList<AdaptiveNotificationText> textElements = toastBinding.GetTextElements();
 
    // Treat the first text element as the title text
    string titleText = textElements.FirstOrDefault()?.Text;
 
    // We'll treat all subsequent text elements as body text,
    // joining them together via newlines.
    string bodyText = string.Join("\n", textElements.Skip(1).Select(t => t.Text));
}
```


## <a name="remove-a-specific-notification"></a>Supprimer une notification spécifique

Si votre appareil portable ou votre service permet à l’utilisateur d'ignorer les notifications, vous pouvez supprimer la notification proprement dite pour que l’utilisateur ne la voit pas ultérieurement sur son téléphone ou son PC. Fournissez simplement l’ID de notification (obtenu à partir de l'objet [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification)) de la notification que vous souhaitez supprimer: 

```csharp
// Remove the notification
listener.RemoveNotification(notifId);
```


## <a name="clear-all-notifications"></a>Effacer toutes les notifications

La méthode [UserNotificationListener.ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) efface toutes les notifications de l’utilisateur. Utilisez cette méthode avec précaution. Vous ne devez effacer toutes les notifications que si votre appareil portable ou votre service affiche TOUTES les notifications. Si votre appareil portable ou votre service n’affiche que certaines notifications, lorsque l’utilisateur clique sur le bouton «Effacer les notifications», il s’attend à ce qu'uniquement ces notifications spécifiques soient supprimées. Toutefois, l'appel à la méthode [ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) entraînera la suppression effective de toutes les notifications, y compris celles que votre appareil portable ou votre service n’affichait pas.

```csharp
// Clear all notifications. Use with caution.
listener.ClearNotifications();
```


## <a name="background-task-for-notification-addeddismissed"></a>Tâche en arrière-plan pour une notification ajoutée/ignorée

Une méthode courante pour permettre à une application de détecter les notifications consiste à définir une tâche en arrière-plan, de sorte que vous pouvez savoir quand une notification a été ajoutée ou ignorée, que votre application soit ou non en cours d’exécution.

Grâce au [modèle de processus unique](../../../launch-resume/create-and-register-an-inproc-background-task.md) ajouté dans la mise à jour anniversaire, l'ajout de tâches en arrière-plan est assez simple. Dans le code principal de votre application, une fois que vous avez obtenu l’accès de l'utilisateur à l’Écouteur de notification et l’accès pour exécuter des tâches en arrière-plan, enregistrez simplement une tâche en arrière-plan et définissez le [UserNotificationChangedTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.usernotificationchangedtrigger) à l’aide du [type de notification toast](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationkinds).

```csharp
// TODO: Request/check Listener access via UserNotificationListener.Current.RequestAccessAsync
 
// TODO: Request/check background task access via BackgroundExecutionManager.RequestAccessAsync
 
// If background task isn't registered yet
if (!BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals("UserNotificationChanged")))
{
    // Specify the background task
    var builder = new BackgroundTaskBuilder()
    {
        Name = "UserNotificationChanged"
    };
 
    // Set the trigger for Listener, listening to Toast Notifications
    builder.SetTrigger(new UserNotificationChangedTrigger(NotificationKinds.Toast));
 
    // Register the task
    builder.Register();
}
```

Puis, dans votre App.xaml.cs, remplacez la méthode [OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.OnBackgroundActivated) si ce n'est pas déjà fait et utilisez une instruction switch sur le nom de la tâche pour déterminer lequel de vos nombreux déclencheurs de tâche en arrière-plan a été appelé.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "UserNotificationChanged":
            // Call your own method to process the new/removed notifications
            // The next section of documentation discusses this code
            await MyWearableHelpers.SyncNotifications();
            break;
    }
 
    deferral.Complete();
}
```

La tâche en arrière-plan est une simple indication: elle ne fournit aucune information sur la notification spécifique qui a été ajoutée ou supprimée. Lorsque votre tâche en arrière-plan est déclenchée, vous devez synchroniser les notifications sur votre appareil portable afin qu’elles reflètent les notifications dans la plateforme. Cela garantit qu'en cas d'échec de votre tâche en arrière-plan, des notifications sur votre appareil portable peuvent encore être récupérées lors de la prochaine exécution de votre tâche en arrière-plan.

`SyncNotifications` est une méthode que vous implémentez; la prochaine section montre comment. 


## <a name="determining-which-notifications-were-added-and-removed"></a>Déterminer les notifications qui ont été ajoutées et supprimées

Dans votre méthode `SyncNotifications`, pour déterminer les notifications qui ont été ajoutées ou supprimées (en synchronisant les notifications avec votre appareil portable), vous devez calculer le delta entre votre collection de notifications actuelle et les notifications présentes dans la plateforme.

```csharp
// Get all the current notifications from the platform
IReadOnlyList<UserNotification> userNotifications = await listener.GetNotificationsAsync(NotificationKinds.Toast);
 
// Obtain the notifications that our wearable currently has displayed
IList<uint> wearableNotificationIds = GetNotificationsOnWearable();
 
// Copy the currently displayed into a list of notification ID's to be removed
var toBeRemoved = new List<uint>(wearableNotificationIds);
 
// For each notification in the platform
foreach (UserNotification userNotification in userNotifications)
{
    // If we've already displayed this notification
    if (wearableNotificationIds.Contains(userNotification.Id))
    {
        // We want to KEEP it displayed, so take it out of the list
        // of notifications to remove.
        toBeRemoved.Remove(userNotification.Id);
    }
 
    // Otherwise it's a new notification
    else
    {
        // Display it on the Wearable
        SendNotificationToWearable(userNotification);
    }
}
 
// Now our toBeRemoved list only contains notification ID's that no longer exist in the platform.
// So we will remove all those notifications from the wearable.
foreach (uint id in toBeRemoved)
{
    RemoveNotificationFromWearable(id);
}
```


## <a name="foreground-event-for-notification-addeddismissed"></a>Événement en arrière-plan pour une notification ajoutée/ignorée

> [!IMPORTANT] 
> Problème connu: l’événement au premier plan entraîne une boucle de processeur sur les versions récentes de Windows et précédemment n’a pas fonctionné auparavant. N’utilisez pas l’événement au premier plan. Dans une prochaine mise à jour vers Windows, nous sera résoudre ce problème.

Au lieu d’utiliser l’événement au premier plan, utilisez le code indiqué précédemment pour une tâche en arrière-plan de [modèle à processus unique](../../../launch-resume/create-and-register-an-inproc-background-task.md) . La tâche en arrière-plan vous permettent également de recevoir des notifications d’événement de modification à la fois pendant que votre application est fermée ou en cours d’exécution.

```csharp
// Subscribe to foreground event (DON'T USE THIS)
listener.NotificationChanged += Listener_NotificationChanged;
 
private void Listener_NotificationChanged(UserNotificationListener sender, UserNotificationChangedEventArgs args)
{
    // NOTE: This event WILL CAUSE CPU LOOPS, DO NOT USE. Use the background task instead.
}
```


## <a name="howto-fixdelays-in-the-background-task"></a>Fixdelays de faire dans la tâche en arrière-plan

Lorsque vous testez votre application, vous remarquerez peut-être que la tâche en arrière-plan est parfois retardée et ne déclenche pas pendant plusieurs minutes. Pour corriger le délai, invite l’utilisateur togo aux paramètres système -> système -> batterie -> utilisation de la batterie par application, recherchez votre application dans la liste, sélectionnez-le et définissez-la sur «toujours autorisé en arrière-plan».Après cela, la tâche en arrière-plan doit toujours être déclenchée au sein d’une seconde de la notification reçue environ.
