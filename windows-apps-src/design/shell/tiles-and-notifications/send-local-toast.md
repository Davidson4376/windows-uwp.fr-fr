---
Description: Learn how to send a local toast notification and handle the user clicking the toast.
title: Envoyer une notification toast locale
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp, envoyer des notifications toast, notifications, envoyer des notifications, notifications toast, procédure, démarrage rapide, prise en main, exemple de code, procédure pas à pas
ms.localizationpriority: medium
ms.openlocfilehash: dd7dfb621d84a3ce1d934c358ab60683caee9238
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8215683"
---
# <a name="send-a-local-toast-notification"></a>Envoyer une notification toast locale


Une notification toast est un message qu’une application peut créer et remettre à l’utilisateur alors que son application n’est pas active. Ce démarrage rapide vous guide dans les procédures de création, remise et affichage d’une notification toast Windows10 avec les nouveaux modèles adaptatifs et actions interactives. Ces actions sont illustrées via une notification locale, qui est la notification la plus simple à implémenter.

> [!IMPORTANT]
> Les applications de bureau (Pont du bureau et Win32classique) ont des étapes différentes pour l’envoi de notifications et la gestion de l’activation. Pour plus d'informations sur l'implémentation de toasts, consultez la documentation [Applications de bureau](toast-desktop-apps.md).

Nous allons examiner les étapes suivantes:

### <a name="sending-a-toast"></a>Envoi d’un toast

* Construction de la partie visuelle (texte et image) de la notification
* Ajout d’actions à la notification
* Définition d’une date/heure d’expiration du toast
* Définition d’une propriété Tag/Group afin de pouvoir remplacer/supprimer le toast ultérieurement
* Envoi du toast à l’aide des API locales

### <a name="handling-activation"></a>Gestion de l’activation

* Gestion de l’activation lorsque l’utilisateur clique sur le corps ou les boutons
* Gestion de l’activation au premier plan
* Gestion de l’activation en arrière-plan

> **API importantes**: [classe ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification), [classe ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)


## <a name="prerequisites"></a>Éléments prérequis

Pour bien comprendre cette rubrique, il est utile de disposer de ce qui suit...

* Une connaissance pratique des concepts et des termes relatifs aux notifications toast. Pour plus d’informations, voir[Toast et notifications du centre de vue d’ensemble](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/).
* Une bonne connaissance du contenu d’une notification toast Windows10. Pour plus d’informations, voir la [documentation sur le contenu des toasts](adaptive-interactive-toasts.md).
* Un projet d’application UWP Windows10

> [!NOTE]
> Contrairement à Windows8/8.1, vous n’avez plus besoin de déclarer dans le manifeste de votre application que celle-ci peut afficher les notifications toast. Toutes les applications sont en mesure d’envoyer et d’afficher des notifications toast.

> [!NOTE]
> **Applications Windows8/8.1**: reportez-vous à la [documentation archivée](https://msdn.microsoft.com/library/windows/apps/xaml/hh868254.aspx).


## <a name="install-nuget-packages"></a>Installer des packages NuGet

Nous vous recommandons d’installer les deux packages NuGet suivants à votre projet. Notre exemple de code les utilise. À la fin de l’article, nous vous fournissons des extraits de code standard qui n’utilisent pas de packages NuGet.

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): générez des charges utiles de toast via des objets à la place de code XML brut.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/): générez et analysez les chaînes de requête en C#.


## <a name="add-namespace-declarations"></a>Ajouter des déclarations d’espace de noms

`Windows.UI.Notifications` inclut les API de toast.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>Envoyer un toast

Dans Windows10, le contenu de votre notification toast est décrit à l’aide d’un langage adaptatif qui permet une grande flexibilité en termes d’apparence de notification. Pour plus d’informations, voir la [documentation sur le contenu des toasts](adaptive-interactive-toasts.md).

### <a name="constructing-the-visual-part-of-the-content"></a>Construction de la partie visuelle du contenu

Commençons par construire la partie visuelle du contenu, qui inclut le texte et les images que vous souhaitez montrer à l’utilisateur.

Grâce à la bibliothèque Notifications, la génération du contenu XML est simple. Si vous n’installez pas la bibliothèque Notifications à partir de NuGet, vous devez construire le code XML manuellement, ce qui peut prêter aux erreurs.

> [!NOTE]
> Des images peuvent être utilisées à partir du package de l’application, du stockage local de l’application ou du web. À partir de Fall Creators Update, les images Web peuvent atteindre 3Mo sur les connexions normales et 1Mo sur les connexions limitées. Sur les appareils qui n’exécutent pas encore Fall Creators Update, les images web ne doivent pas dépasser 200Ko.

```csharp
// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "https://picsum.photos/360/202?image=883";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// Construct the visuals of the toast
ToastVisual visual = new ToastVisual()
{
    BindingGeneric = new ToastBindingGeneric()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = title
            },
 
            new AdaptiveText()
            {
                Text = content
            },
 
            new AdaptiveImage()
            {
                Source = image
            }
        },
 
        AppLogoOverride = new ToastGenericAppLogo()
        {
            Source = logo,
            HintCrop = ToastGenericAppLogoCrop.Circle
        }
    }
};
```


### <a name="constructing-actions-part-of-the-content"></a>Construction de la partie du contenu relative aux actions

À présent, ajoutons des actions au contenu.

Dans l’exemple, ci-dessous, nous avons inclus un élément d’entrée qui permet à l’utilisateur de saisir du texte, qui est retourné à l’application lorsque l’utilisateur clique sur l’un des boutons ou sur le toast proprement dit.

Nous avons ensuite ajouté deux boutons, chacun avec son propre type d’activation, son contenu et ses arguments.
* **ActivationType** permet de spécifier le mode d’activation de votre application lorsque cette action est effectuée par l’utilisateur. Vous pouvez choisir de lancer votre application au premier plan, de lancer une tâche en arrière-plan ou de lancer une autre application par protocole. Que votre application choisisse le premier plan ou l’arrière-plan, vous recevrez toujours l’entrée utilisateur et les arguments que vous avez spécifiés, afin que votre application puisse effectuer l’action appropriée, comme l’envoi du message ou l’ouverture d’une conversation.

```csharp
// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Construct the actions for the toast (inputs and buttons)
ToastActionsCustom actions = new ToastActionsCustom()
{
    Inputs =
    {
        new ToastTextBox("tbReply")
        {
            PlaceholderContent = "Type a response"
        }
    },
 
    Buttons =
    {
        new ToastButton("Reply", new QueryString()
        {
            { "action", "reply" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background,
            ImageUri = "Assets/Reply.png",
 
            // Reference the text box's ID in order to
            // place this button next to the text box
            TextBoxId = "tbReply"
        },
 
        new ToastButton("Like", new QueryString()
        {
            { "action", "like" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background
        },
 
        new ToastButton("View", new QueryString()
        {
            { "action", "viewImage" },
            { "imageUrl", image }
 
        }.ToString())
    }
};
```


### <a name="combining-the-above-to-construct-the-full-content"></a>Combinaison des éléments ci-dessus pour construire l’intégralité du contenu

La construction du contenu est désormais terminée, et nous pouvons l’utiliser pour instancier votre objet [**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification).

**Remarque**: vous pouvez également fournir un type d’activation dans l’élément racine pour spécifier le type d’activation qui doit se produire lorsque l’utilisateur appuie sur le corps de la notification toast. Normalement, appuyer sur le corps du toast doit lancer votre application au premier plan pour créer une expérience utilisateur cohérente, mais vous pouvez utiliser d’autres types d’activation pour concorder avec votre scénario spécifique si cela est plus judicieux pour l’utilisateur.

Vous devez toujours définir la propriété **Launch** de manière à ce que lorsque l’utilisateur appuie sur le corps du toast et que votre application est lancée, cette dernière sait quel contenu elle doit afficher.

```csharp
// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = visual,
    Actions = actions,
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
 
    }.ToString()
};
 
// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());
```


## <a name="set-an-expiration-time"></a>Définir une date/heure d’expiration

Dans Windows10, toutes les notifications sont enregistrées dans le centre de notifications après que l’utilisateur les a ignorées. Les utilisateurs peuvent donc consulter votre notification une fois que la fenêtre contextuelle a disparu.

Toutefois, si le message de votre notification est uniquement pertinent pour une période donnée, vous devez définir une date/heure d’expiration dans la notification toast pour que les utilisateurs ne voient pas d’informations obsolètes provenant de votre application. Par exemple, si une promotion n’est valide que pendant 12heures, définissez le délai d’expiration sur 12heures. Dans le code ci-dessous, nous avons défini la date d’expiration sur 2jours.

> [!NOTE]
> La date/heure d’expiration par défaut et maximale pour les notifications toast locales est de 3jours.

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Fournir une clé primaire pour votre toast

Si vous voulez supprimer ou remplacer la notification que vous envoyez par programme, vous devez utiliser la propriété Tag (et éventuellement la propriété Group) pour fournir une clé primaire pour votre notification. Vous pouvez alors utiliser cette clé primaire à l’avenir pour supprimer ou remplacer la notification.

Pour plus d’informations sur le remplacement/la suppression des notifications toast déjà remises, voir [Démarrage rapide: gestion des notifications toast dans le centre de notifications (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/dn631260.aspx).

Les propriétés Tag et Group combinées font office de clé primaire composite. La propriété Group est l’identificateur le plus générique, où vous pouvez affecter des groupes tels que «wallPosts», «messages», «friendRequests», etc. tandis que la propriété Tag doit identifier de manière unique la notification proprement dite au sein du groupe. En utilisant un groupe générique, vous pouvez ensuite supprimer toutes les notifications de ce groupe à l’aide de l’[API RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>Envoyer la notification

Une fois que vous avez initialisé votre toast, créez simplement un élément [ToastNotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier) et appelez Show(), en transmettant votre notification toast.

```csharp
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


## <a name="clear-your-notifications"></a>Effacer vos notifications

Les applications UWP sont chargées de la suppression et de l’effacement de leurs propres notifications. Lorsque votre application est lancée, nous n’effaçons PAS automatiquement vos notifications.

Windows ne supprime automatiquement une notification que si l’utilisateur clique explicitement sur celle-ci.

Voici un exemple de ce que doit faire une application de messagerie…

1. L’utilisateur reçoit plusieurs toasts sur de nouveaux messages d’une conversation.
2. L’utilisateur appuie sur l’un de ces toasts pour ouvrir la conversation.
3. L’application ouvre la conversation, puis efface tous les toasts correspondant à cette conversation (à l’aide de [RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) dans le groupe fourni par l’application pour cette conversation).
4. Le centre de notifications de l’utilisateur reflète à présent correctement l’état des notifications, puisqu’il ne reste pas de notifications obsolètes pour cette conversation dans le centre de notifications.

Pour en savoir plus sur l’effacement de toutes les notifications ou la suppression de certaines d’entre elles, voir [Démarrage rapide: gestion des notifications toast dans le centre de notifications (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/dn631260.aspx).


## <a name="handling-activation"></a>Gestion de l’activation

Dans Windows10, lorsque l’utilisateur clique sur votre toast, vous pouvez faire activer votre application de deuxmanières différentes...

* Activation au premier plan
* Activation en arrière-plan

> [!NOTE]
> Si vous utilisez les modèles de toast hérités de Windows8.1, c’est la méthode **OnLaunched** qui est appelée à la place de **OnActivated**. La documentation suivante s’applique uniquement aux notifications Windows10modernes utilisant la bibliothèque Notifications (ou le modèle ToastGeneric si vous utilisez le code XML brut).


### <a name="handling-foreground-activation"></a>Gestion de l’activation au premier plan

Dans Windows10, lorsqu’un utilisateur clique sur un toast moderne (ou un bouton du toast), la méthode **OnActivated** est appelée à la place de **OnLaunched**, avec un nouveau type d’activation: **ToastNotification**. Le développeur peut donc facilement distinguer une activation de toast et effectuer les tâches en conséquence.

Dans l’exemple affiché ci-dessous, vous pouvez récupérer la chaîne d’arguments que vous avez initialement indiquée dans le contenu de toast. Vous pouvez également récupérer l’entrée de l’utilisateur dans vos zones de texte et zones de sélection.

> [!IMPORTANT]
> Vous devez initialiser votre image et activer votre fenêtre tout comme votre code **OnLaunched**. **La méthode OnLaunched n’est PAS appelée si l’utilisateur clique sur votre toast**, même si votre application a été fermée et est démarrée pour la première fois. Nous recommandons souvent de combiner les méthodes **OnLaunched** et **OnActivated** en une méthode `OnLaunchedOrActivated` qui vous est propre, car une même initialisation doit se produire dans les deux.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        var toastActivationArgs = e as ToastNotificationActivatedEventArgs;
                 
        // Parse the query string (using QueryString.NET)
        QueryString args = QueryString.Parse(toastActivationArgs.Argument);
 
        // See what action is being requested 
        switch (args["action"])
        {
            // Open the image
            case "viewImage":
 
                // The URL retrieved from the toast args
                string imageUrl = args["imageUrl"];
 
                // If we're already viewing that image, do nothing
                if (rootFrame.Content is ImagePage && (rootFrame.Content as ImagePage).ImageUrl.Equals(imageUrl))
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ImagePage), imageUrl);
                break;
                             
 
            // Open the conversation
            case "viewConversation":
 
                // The conversation ID retrieved from the toast args
                int conversationId = int.Parse(args["conversationId"]);
 
                // If we're already viewing that conversation, do nothing
                if (rootFrame.Content is ConversationPage && (rootFrame.Content as ConversationPage).ConversationId == conversationId)
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ConversationPage), conversationId);
                break;
        }
 
        // If we're loading the app for the first time, place the main page on
        // the back stack so that user can go back after they've been
        // navigated to the specific page
        if (rootFrame.BackStack.Count == 0)
            rootFrame.BackStack.Add(new PageStackEntry(typeof(MainPage), null, null));
    }
 
    // TODO: Handle other types of activation
 
    // Ensure the current window is active
    Window.Current.Activate();
}
```


## <a name="handling-background-activation"></a>Gestion de l’activation en arrière-plan

Si vous spécifiez l’activation en arrière-plan dans votre toast (ou sur un bouton du toast), votre tâche en arrière-plan sera exécutée à la place de l’activation de votre application au premier plan.

Pour plus d’informations sur les tâches en arrière-plan, consultez [Prendre en charge votre application avec des tâches en arrière-plan](/launch-resume/support-your-app-with-background-tasks.md).

Si vous ciblez les builds14393 ou supérieures, vous pouvez utiliser des tâches en arrière-plan in-process, ce qui simplifie considérablement les choses. Notez que l’exécution des tâches en arrière-plan in-process échoue sur les versions plus anciennes de Windows. Nous allons utiliser une tâche en arrière-plan in-process dans cet exemple de code.

```csharp
const string taskName = "ToastBackgroundTask";

// If background task is already registered, do nothing
if (BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals(taskName)))
    return;

// Otherwise request access
BackgroundAccessStatus status = await BackgroundExecutionManager.RequestAccessAsync();

// Create the background task
BackgroundTaskBuilder builder = new BackgroundTaskBuilder()
{
    Name = taskName
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


Dans votre fichier App.xaml.cs, remplacez la méthode OnBackgroundActivated pour pouvoir récupérer les arguments prédéfinis et les entrées utilisateur, à l’instar de l’activation au premier plan.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "ToastBackgroundTask":
            var details = args.TaskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
            if (details != null)
            {
                string arguments = details.Argument;
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```



## <a name="plain-vanilla-code-snippets"></a>Extraits de code standard

Si vous n’utilisez pas la bibliothèque Notifications de NuGet, vous pouvez construire manuellement votre fichier XML, comme illustré ci-dessous pour créer un objet [ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification).

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;

// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "http://blogs.msdn.com/cfs-filesystemfile.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-71-81-permanent/2727.happycanyon1_5B00_1_5D00_.jpg";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// TODO: all values need to be XML escaped
 
// Construct the visuals of the toast
string toastVisual =
$@"<visual>
  <binding template='ToastGeneric'>
    <text>{title}</text>
    <text>{content}</text>
    <image src='{image}'/>
    <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
  </binding>
</visual>";

// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Generate the arguments we'll be passing in the toast
string argsReply = $"action=reply&conversationId={conversationId}";
string argsLike = $"action=like&conversationId={conversationId}";
string argsView = $"action=viewImage&imageUrl={Uri.EscapeDataString(image)}";
 
// TODO: all args need to be XML escaped
 
string toastActions =
$@"<actions>
 
  <input
      type='text'
      id='tbReply'
      placeHolderContent='Type a response'/>
 
  <action
      content='Reply'
      arguments='{argsReply}'
      activationType='background'
      imageUri='Assets/Reply.png'
      hint-inputId='tbReply'/>
 
  <action
      content='Like'
      arguments='{argsLike}'
      activationType='background'/>
 
  <action
      content='View'
      arguments='{argsView}'/>
 
</actions>";

// Now we can construct the final toast content
string argsLaunch = $"action=viewConversation&conversationId={conversationId}";
 
// TODO: all args need to be XML escaped
 
string toastXmlString =
$@"<toast launch='{argsLaunch}'>
    {toastVisual}
    {toastActions}
</toast>";
 
// Parse to XML
XmlDocument toastXml = new XmlDocument();
toastXml.LoadXml(toastXmlString);
 
// Generate toast
var toast = new ToastNotification(toastXml);
```


## <a name="resources"></a>Ressources

* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-toast)
* [Documentation sur le contenu des toasts](adaptive-interactive-toasts.md)
* [Classe ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)
* [Classe ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
