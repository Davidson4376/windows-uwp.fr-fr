---
author: normesta
description: "Cet article vous montre comment intégrer des flux de réseaux sociaux dans l’application Contacts"
MSHAttr: PreferredLib:/library/windows/apps
title: "Transmettre des flux de réseaux sociaux à l’application Contacts"
translationtype: Human Translation
ms.sourcegitcommit: 767acdc847e1897cc17918ce7f49f9807681f4a3
ms.openlocfilehash: c5b9666d8654a4065bc0e4e400d3e47de4773b8b

---

# Transmettre des flux de réseaux sociaux à l’application Contacts

Intégrer des données de flux sociaux à partir de votre base de données dans l’application Contacts.

Les données de votre flux apparaîtront dans les pages **Nouveautés** de l’application Contacts ou dans la page **Profil** d’un contact.

Pour ouvrir votre application, vos utilisateurs peuvent appuyer sur un élément de flux.

![Flux de réseaux sociaux dans l’application Contacts](images/social-feeds.png)

Pour commencer, créez une application de premier plan qui identifie les contacts des flux de réseaux sociaux et un agent d’arrière-plan qui transmet des données de flux à l’application Contacts.

Pour consulter un exemple plus complet, consultez la section [Exemple d’informations de flux de réseaux sociaux](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp).

## Créer une application au premier plan

Tout d’abord, créez un projet de plateforme Windows universelle, puis ajoutez-y le kit de développement logiciel **Windows Mobile Extensions for UWP**.

![Extensions mobiles](images/mobile-extensions.png)

### Rechercher ou créer des contacts

Vous pouvez rechercher des contacts à l’aide d’un nom, d’une adresse de messagerie ou d’un numéro de téléphone.

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```
Vous pouvez également créer des contacts à ajouter à une liste de contacts.

```cs
Contact contact = new Contact();
contact.FirstName = "TestContact";

ContactEmail email = new ContactEmail();
email.Address = "TestContact@contoso.com";
email.Kind = ContactEmailKind.Other;
contact.Emails.Add(email);

ContactPhone phone = new ContactPhone();
phone.Number = "4255550101";
phone.Kind = ContactPhoneKind.Mobile;
contact.Phones.Add(phone);

ContactStore store = await
    ContactManager.RequestStoreAsync(ContactStoreAccessType.AppContactsReadWrite);

ContactList contactList;

IReadOnlyList<ContactList> contactLists = await store.FindContactListsAsync();

if (0 == contactLists.Count)
    contactList = await store.CreateContactListAsync("TestContactList");
else
    contactList = contactLists[0];

await contactList.SaveContactAsync(contact);
```

### Identifier chaque contact avec une annotation

Avec cette *annotation*, l’application Contacts demande les données de flux pour le contact de votre agent d’arrière-plan.

Dans l’annotation, associez l’identifiant du contact à un identifiant que votre application utilise en interne pour identifier ce contact.

```cs
ContactAnnotationStore annotationStore = await
   ContactManager.RequestAnnotationStoreAsync(ContactAnnotationStoreAccessType.AppAnnotationsReadWrite);

ContactAnnotationList annotationList;

IReadOnlyList<ContactAnnotationList> annotationLists = await annotationStore.FindAnnotationListsAsync();
if (0 == annotationLists.Count)
    annotationList = await annotationStore.CreateAnnotationListAsync();
else
    annotationList = annotationLists[0];

ContactAnnotation annotation = new ContactAnnotation();
annotation.ContactId = contact.Id;
annotation.RemoteId = "user22";

annotation.SupportedOperations = ContactAnnotationOperations.SocialFeeds;

await annotationList.TrySaveAnnotationAsync(annotation);

```
### Approvisionner l’agent d’arrière-plan

Assurez-vous que le contrat de l’API [SocialInfoContract](https://msdn.microsoft.com/library/windows/apps/dn706146.aspx) est disponible sur l’appareil qui exécutera votre application.

S’il est disponible, approvisionnez l’agent d’arrière-plan.

```cs
if (Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
"Windows.ApplicationModel.SocialInfo.SocialInfoContract",
1))
{
    bool isProvisionSuccessful = await Windows.ApplicationModel.SocialInfo.Provider.SocialInfoProviderManager.ProvisionAsync();

    // Throw an exception if the app could not be provisioned
    if (!isProvisionSuccessful)
    {
        throw new Exception("Could not provision the app with the SocialInfoProviderManager");
    }
}
```
## Créer l’agent d’arrière-plan

L’agent d’arrière-plan est un composant Windows Runtime qui répond aux demandes de flux de l’application contacts.

Dans votre agent, vous répondez à ces demandes en donnant à l’application contacts les données de flux de votre base de données.

### Créer un composant Windows Runtime

Ajoutez un **composant Windows Runtime (Universel Windows)** à votre solution.

![Composant Windows Runtime](images/windows-runtime-component.png)

### Enregistrer l’agent d’arrière-plan en tant qu’instance AppService

Procédez à l’enregistrement en ajoutant des gestionnaires de protocole à l’élément ``Extensions`` du manifeste.

```xml
<Extensions>
  <uap:Extension Category="windows.appService" EntryPoint="SocialFeeds.BackgroundAgent">
    <uap:AppService Name="com.microsoft.windows.social-feeds" />
  </uap:Extension>
</Extensions>
```
Vous pouvez également les ajouter dans l’onglet **Declarations** du concepteur de manifeste de Visual Studio.

![App Service dans le manifeste d’application](images/manifest-designer-app-service.png)

### Demander des opérations de l’application Contacts

Demandez à l’application Contact de communiquer le prochain type de données souhaité. L’application Contacts répond à votre demande à l’aide d’un code spécifiant le flux dont les données sont requises.

Ce tableau décrit chaque flux:

| Flux | Description |
|-------|-------------|
| Accueil | Flux apparaissant dans la page Nouveautés de l’application Contacts. |
| Contact | Flux apparaissant dans la page Nouveautés d’un contact. |
| Tableau de bord | Flux apparaissant dans la carte de visite en regard de la photo de profil. |
<br>
Vous communiquerez votre demande à l’application Contacts en sollicitant une *opération*. Implémentez l’interface [IBackgroundTask](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx), puis remplacez la méthode [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx).

Dans la méthode [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx), envoyez deuxpaires clé-valeur à l’application Contacts. L’une d’entre elles comporte la version du protocole, l’autre contient le type de l’opération.

Détectez ensuite une réponse de l’application Contacts. Cette réponse contient un code.

```cs
public sealed class BackgroundAgent : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
        BackgroundTaskDeferral backgroundTaskDeferral = taskInstance.GetDeferral();

        AppServiceTriggerDetails triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;

        AppServiceConnection appServiceConnection = triggerDetails.AppServiceConnection;

        bool continueProcessing = true;

        while (continueProcessing)
        {
            // Get next operation.
            ValueSet fields =
                new ValueSet();

            fields.Add("Version",
                (1 << 16) | 0);

            fields.Add(
                "Type", 0x10);

            AppServiceResponse response =
                await appServiceConnection.SendMessageAsync(fields);

            if (response == null || response.Status != AppServiceResponseStatus.Success)
            { /* throw exception */ }

            UInt32 type = (UInt32)response.Message["Type"];

            switch (type)
            {
                //Get Next Operation.
                case 0x10:
                    break;
                //Download Home Feed Operation.
                case 0x11:
                    break;
                //Download Contact Feed Operation.
                case 0x13:
                    break;
                //Download Dashboard Feed Operation.
                case 0x15:
                    break;
                //Operation Result.
                case 0x80:
                    break;
                //Good Bye.
                case 0xF1:
                    continueProcessing = false;
                    break;
            }
        }
        // Complete the task deferral
        backgroundTaskDeferral.Complete();

        backgroundTaskDeferral = null;
        appServiceConnection = null;
    }

}
```

Reportez-vous à l’élément ``Type`` de la propriété [AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) pour récupérer ce code. Voici une liste complète des codes.

| Type| Description |
|-----|-------------|
| 0x10 | Une demande de l’application Contacts pour l’opération suivante. |
| 0x11 | Une demande de l’application Contacts relative à la fourniture du flux de la page d’accueil pour l’utilisateur principal. |
| 0x13 | Une demande de l’application Contacts relative à la récupération du flux du contact sélectionné. |
| 0x15 | Une demande de l’application Contacts relative à la récupération de l’élément de tableau de bord du contact sélectionné. |
| 0x80 | Indique que l’opération est terminée. Signale à l’application Contacts que les données sont désormais disponibles. |
| 0xF1 | Un message de l’application Contacts indiquant qu’elle ne sollicite aucune autre opération. L’agent d’arrière-plan peut désormais se fermer. |
<br>
La propriété [AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) renvoie également une collection d’autres paires clé-valeur décrivant la réponse. Voici une liste de ces éléments.

| Clé | Type | Description |
|-----|------|-------------|
| Version | UINT32 | (Obligatoire) Identifie la version du protocole de messagerie. Les 16bits de niveau inférieur correspondent à la version majeure, tandis que les 16bits de niveau inférieur sont associés à la version mineure. |
| Type | UINT32 | (Obligatoire) Le type d’opération à effectuer. L’exemple précédent utilise la clé Type pour déterminer l’opération demandée par l’application Contacts.
| OperationId | UINT32 | L’identifiant de l’opération. |
| OwnerRemoteId | Chaîne | Identifiant utilisé en interne par votre application pour identifier ce contact. |
| LastFeedItemTimeStamp | Chaîne | Identifiant du dernier élément de flux récupéré. |
| LastFeedItemTimeStamp | DateTime | Horodatage du dernier élément de flux récupéré. |
| ItemCount | UINT32 | Le nombre d’éléments demandés par l’application Contacts. |
| IsFetchMore | BOOLÉENNE | Détermine le moment de mise à jour du cache interne. |
| ErrorCode | UINT32 | Le code d’erreur associé à l’opération de l’agent d’arrière-plan. |
<br>
### Fournir un flux de données à l’application Contacts

Une valeur **Type** de ``0x11``, ``0x13`` ou ``0x15`` est une demande de l’application Contacts relative à des données de flux.  

Les extraits de code suivants illustrent une approche de transmission des données à l’application Contacts.

> [!NOTE]
> Ces extraits de code proviennent de l’[Exemple d’informations du flux de réseaux sociaux](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp). Ils contiennent des références à des interfaces, à des classes ou à des membres définis ailleurs dans l’exemple. Utilisez ces extraits avec d’autres exemples de cette rubrique pour comprendre le flux de tâches et reportez-vous à l’exemple pour en savoir plus sur la pile d’interfaces, de classes et de types.

**Obtenir les éléments du flux de contact**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the contact feeds
    IEnumerable<FeedItem> contactFeedItems =
        InMemorySocialCache.Instance.GetContactFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the SocialInfo APIs
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.ContactFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Translate the sample feed into Social info feed items.
    foreach (FeedItem fi in contactFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

**Obtenir les éléments du tableau de bord**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the dashboard feed item from your database.
    FeedItem dashboardFeedItem = InMemorySocialCache.Instance.GetDashboardFeed(OwnerRemoteId);

    if (dashboardFeedItem != null)
    {
        // Check if the platform supports the social info APIs.
        if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
             "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
        {

            return;
        }

        SocialDashboardItemUpdater dashboard =
            await SocialInfoProviderManager.CreateDashboardItemUpdaterAsync(OwnerRemoteId);

        dashboard.Content.Message = dashboardFeedItem.Message;
        dashboard.Content.Title = dashboardFeedItem.Title;
        dashboard.Timestamp = dashboardFeedItem.Timestamp;

        // The TargetUri of the dashboard always has to be set.
        dashboard.TargetUri = dashboardFeedItem.TargetUri;

        // For a thumbnail, there must always be a target Uri.
        if ((dashboardFeedItem.ImageUri != null) && (dashboardFeedItem.TargetUri != null))
        {
            dashboard.Thumbnail = new SocialItemThumbnail()
            {
                ImageUri = dashboardFeedItem.ImageUri,
                TargetUri = dashboardFeedItem.TargetUri
            };
        }

        await dashboard.CommitAsync();
    }
}
```

**Obtenir les éléments du flux de la page d’accueil**

```cs
public override async Task DownloadFeedAsync()
{
    // Query the "database" for the home feeds.
    IEnumerable<FeedItem> homeFeedItems =
        InMemorySocialCache.Instance.GetHomeFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the social info APIs.
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.HomeFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Generate each of the feed items.
    foreach (FeedItem fi in homeFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

### Renvoyer une notification de succès ou d’échec à l’application Contacts

Encapsulez vos appels dans un bloc catch try, puis retransmettez un message de réussite ou d’échec à l’application Contacts une fois les données du flux fournies.

```cs
try
{
    // Calls to methods that fetch data and populate feed.
}
catch (Exception exception)
{
    errorCode = (UInt32)exception.HResult;
}

// Send status back to the people app.
ValueSet fields =
    new ValueSet();

fields.Add("ErrorCode", errorCode);

UInt32 operationID = (UInt32)response.Message["OperationId"];

fields.Add("OperationId", operationID);

await this.mAppServiceConnection.SendMessageAsync(fields);

```



<!--HONumber=Aug16_HO4-->


