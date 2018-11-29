---
Description: Learn how to group notifications in Action Center using collections.
title: Collections de toasts
label: Toast Collections
template: detail.hbs
ms.date: 05/16/2018
ms.topic: article
keywords: windows10, uwp, notification, collections, collection, notifications de groupe, notifications de regroupement, regrouper, organiser, centre de notifications, toast
ms.localizationpriority: medium
ms.openlocfilehash: 9b6818f876c094298a0a6636faa00efa9a192545
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7983794"
---
# <a name="grouping-toast-notifications-with-collections"></a>Regroupement de notifications toast avec des collections
Utilisez des regroupements pour organiser les toasts de votre application dans le centre de notifications. Les collections permettent aux utilisateurs de localiser plus facilement les informations dans le centre de notifications et aux développeurs de mieux gérer leurs notifications.  Les API ci-dessous permettent de supprimer, de créer et de mettre à jour des collections de notifications.

> [!IMPORTANT]
> **Nécessite Creators Update**: vous devez cibler le Kit de développement logiciel (SDK)15063 et exécuter la Build15063 ou une version plus récente pour utiliser les collections de toasts. Les API associées sont [Windows.UI.Notifications.ToastCollection](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection) et [Windows.UI.Notifications.ToastCollectionManager](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollectionmanager)

Vous pouvez voir l’exemple ci-dessous, avec une application de messagerie qui sépare les notifications en fonction du groupe de discussion; chaque titre (Comp Sci 160A Project Chat, Direct Messages, Lacrosse Team Chat) est une collection distincte.  Notez la façon dont les notifications sont nettement regroupées comme si elles venaient d’une application distincte, même si ce sont toutes des notifications provenant de la même application.  Si vous recherchez un moyen plus subtil d'organiser vos notifications, consultez [en-têtes de toasts](toast-headers.md).  
![Exemple de collection avec deux groupes de notifications différents](images/toast-collection-example.png)

## <a name="creating-collections"></a>Création de collections
Lors de la création de chaque collection, vous devez fournir un nom d’affichage et une icône, qui sont affichés dans le centre de notifications dans le cadre du titre de la collection, comme illustré dans l’image ci-dessus. Les collections requièrent également un argument de lancement pour aider l’application à accéder à l’emplacement approprié au sein de l’application lorsque le titre de la collection est activé par l’utilisateur.  

### <a name="create-a-collection"></a>Créer une collection

``` csharp 
public const string toastCollectionId = "ToastCollection";

// Create a toast collection
public async void CreateToastCollection()
{
    string displayName = "Work Email"; 
    string launchArg = "NavigateToWorkEmailInbox"; 
    Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/workEmail.png");

    // Constructor
    ToastCollection workEmailToastCollection = new ToastCollection(MainPage.toastCollectionId, 
        displayName,
        launchArg, 
        icon);

    // Calls the platform to create the collection
    await ToastNotificationManager.GetDefault().GetToastCollectionManager().SaveToastCollectionAsync(workEmailToastCollection);                                 
}
```

## <a name="sending-notifications-to-a-collection"></a>Envoyer des notifications à une collection
Nous allons traiter de l'envoi de notifications à partir de trois pipelines de toasts différents: local, planifié et push.  Pour chacun de ces exemples, nous créerons un exemple de toast à envoyer avec le code immédiatement dessous, puis nous nous concentrerons sur l’ajout de la notification toast à une collection via chaque pipeline.

Construire la charge utile de la notification:

``` csharp
public const string toastCollectionId = "MyToastCollection";

public async void SendToastToToastCollection()
{
    // Construct the notification Content
    string toastXmlString = 
        $@"<toast launch=’’>
            <visual>
                <binding template=’ToastGeneric’>
                    <text>Hello,</text>
                    <text>it’s me</text>
                </binding>
            </visual>
        </toast>";
    // Convert to XML
    XmlDocument toastXml = new XmlDocment();
    toastXml.LoadXml(toastXmlString);
    ToastNotification toast = new ToastNotification(toastXml);
```

### <a name="send-a-toast-to-a-collection"></a>Envoyer un toast à une collection

```csharp
// Get the collection notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);

// And show the toast
notifier.Show(toast);
```

### <a name="add-a-scheduled-toast-to-a-collection"></a>Ajouter un toast planifié à une collection

``` csharp
// Create scheduled toast from XML above
ScheduledToastNotification scheduledToast = new ScheduledToastNotification(toastXml, DateTimeOffset.Now.AddSeconds(10));

// Get notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);
    
// Add to schedule
notifier.AddToSchedule(scheduledToast);
```

### <a name="send-a-push-toast-to-a-collection"></a>Envoyer un toast push à une collection
Pour les toasts push, vous devez ajouter l’en-tête X-WNS-CollectionId au message POST.
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>Gestion des collections
#### <a name="create-the-toast-collection-manager"></a>Créer le gestionnaire de collection de toast
Pour le reste des extraits de code de cette section «Gestion des collections», nous allons utiliser le collectionManager ci-dessous.
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>Obtenir toutes les collections

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>Obtenir le nombre de collections créées

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>Supprimer une collection

``` csharp
await collectionManager.RemoveToastCollectionAsync(MainPage.toastCollectionId);
```

#### <a name="update-a-collection"></a>Mettre à jour une collection
Vous pouvez mettre à jour les collections en créant une nouvelle collection ayant le même ID et en enregistrant la nouvelle instance de la collection.
``` csharp
string displayName = "Updated Display Name"; 
string launchArg = "UpdatedLaunchArgs"; 
Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/updatedPicture.png");

// Construct a new toast collection with the same collection id
ToastCollection updatedToastCollection = new ToastCollection(MainPage.toastCollectionId, 
            displayName,
            launchArg, 
            icon);

// Calls the platform to update the collection by saving the new instance
await collectionManager.SaveToastCollectionAsync(updatedToastCollection);                               
```
## <a name="managing-toasts-within-a-collection"></a>Gestion des toasts au sein d’une collection
#### <a name="group-and-tag-properties"></a>Propriétés de groupe et de balise
Les propriétés de groupe et de balise identifient ensemble une notification de manière unique dans une collection.  Le groupe (et la balise) sert de clé primaire composite (plusieurs identificateurs) pour identifier votre notification. Par exemple, si vous souhaitez supprimer ou remplacer une notification, vous devez être en mesure de spécifier *quelle notification* vous souhaitez supprimer/remplacer. Vous le faites en spécifiant la balise et le groupe. Une application de messagerie en est exemple.  Le développeur peut utiliser l’ID de conversation en tant que le groupe et l’ID de message en tant que la balise.

#### <a name="remove-a-toast-from-a-collection"></a>Supprimer un toast d'une collection
Vous pouvez supprimer des toasts individuels à l’aide de l’ID de balise et de groupe ou effacer toutes les notifications toast d'une collection.
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>Effacer tous les toasts d’une collection
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>Collections dans le Visualiseur de notifications
Vous pouvez utiliser l'outil [Notifications Visualizer](notifications-visualizer.md) pour vous aider à concevoir vos collections. Suivez les étapes ci-dessous:

* Cliquez sur l’icône de des paramètres (roue crantée) dans le coin inférieur droit. 
* Sélectionnez «Toast collections».
* Au-dessus de l’aperçu de la notification toast, il existe un menu déroulant de «Toast Collection». Sélectionnez «Manage collections».
* Cliquez sur «Add collection», renseignez les détails de la collection et enregistrez.
* Vous pouvez ajouter d'autres collections ou décocher la zone de gestion des collections pour revenir à l’écran principal.
* Sélectionnez la collection à laquelle vous souhaitez ajouter le toast dans le menu déroulant «Toast collection».
* Lorsque vous démarrerez la notification toast, elle sera ajoutée à la collection appropriée dans le centre de notifications.


## <a name="other-details"></a>Autres détails
Les collections de toast que vous créez seront également répercutées dans les paramètres de notification de l’utilisateur.  Vous pouvez activer les paramètres de chaque collection de façon à activer ou désactiver ces sous-groupes.  Si les notifications sont désactivées au niveau supérieur de l’application, toutes les notifications de collections seront également désactivées.  En outre, chaque collection affiche par défaut 3notifications dans le centre de notifications et l’utilisateur peut la développer pour afficher jusqu'à20 notifications.

## <a name="related-topics"></a>Rubriquesconnexes

* [Contenu des toasts](adaptive-interactive-toasts.md)
* [En-têtes de toast](toast-headers.md)
* [Bibliothèque Notifications sur GitHub (faisant partie du Kit de ressources de la Communauté Windows)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)