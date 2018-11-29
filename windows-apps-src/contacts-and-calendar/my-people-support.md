---
title: Ajout de la prise en charge mes contacts à une application
description: Explique comment ajouter la prise en charge de mes contacts à une application et comment épingler et désépingler des contacts
ms.date: 06/28/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9a486f27d390a651cec0dcad82246a858bab2f33
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8193088"
---
# <a name="adding-my-people-support-to-an-application"></a>Ajout de la prise en charge mes contacts à une application

La fonctionnalité Mes contacts permet aux utilisateurs d’épingler les contacts d’une application directement à la barre des tâches, ce qui crée un nouvel objet contact avec lequel ils peuvent interagir de plusieurs façons. Cet article explique comment ajouter la prise en charge de cette fonctionnalité permettant aux utilisateurs d’épingler des contacts directement à partir de votre application. Lorsque les contacts sont épinglés, de nouveaux types d’interaction utilisateur deviennent disponibles, comme [le partage](my-people-sharing.md) et [les notifications de mes contacts](my-people-notifications.md).

![Conversation instantanée de mes contacts](images/my-people-chat.png)

## <a name="requirements"></a>Configuration requise

+ Windows10 et Microsoft Visual Studio2017. Pour en savoir plus sur l’installation, voir [Prendre en main VisualStudio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Connaissances de base de C# ou d’un langage de programmation orienté objet similaire. Pour vous familiariser avec C#, voir [Créer une application «Hello, world»](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="overview"></a>Vue d’ensemble

Il y a trois choses à faire pour activer votre application afin d’utiliser la fonctionnalité Mes contacts:

1. [Déclarer la prise en charge du contrat d’activation shareTarget dans votre manifeste d’application.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [Annoter les contacts que les utilisateurs peuvent partager à l’aide de votre application.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3.  Prendre en charge plusieurs instances de votre application en cours d’exécution en même temps. Les utilisateurs doivent pouvoir interagir avec une version complète de votre application lors de son utilisation dans un volet de contact.  Ils peuvent même l’utiliser dans plusieurs volets de contact à la fois.  Pour ce faire, votre application doit être en mesure d’exécuter plusieurs vues simultanément. Pour savoir comment procéder, consultez l’article [«afficher plusieurs vues d’une application»](https://docs.microsoft.com/en-us/windows/uwp/layout/show-multiple-views).

Lorsque vous avez terminé, votre application s’affiche dans le volet de contact pour les contacts annotés.

## <a name="declaring-support-for-the-contract"></a>Déclaration de prise en charge pour le contrat

Pour déclarer la prise en charge du contrat de mes contacts, ouvrez votre application dans Visual Studio. À partir de l’**Explorateur de solutions**, cliquez avec le bouton droit sur **Package.appxmanifest** et sélectionnez **Ouvrir avec**. Dans le menu, sélectionnez **Éditeur (de texte) XML)** et cliquez sur **OK**. Apportez les modifications suivantes au manifeste:

**Avant**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
        </Application>
    </Applications>

```

**Après**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
          <Extensions>
            <uap4:Extension Category="windows.contactPanel" />
          </Extensions>
        </Application>
    </Applications>

```

Grâce à cet ajout, votre application peut maintenant être lancée par le biais du contrat **windows. ContactPanel**, ce qui vous permet d’interagir avec les volets de contact.

## <a name="annotating-contacts"></a>Annoter des contacts

Pour permettre l’affichage des contacts de votre application dans la barre des tâches via le volet de mes contacts, vous devez les écrire dans le magasin de contacts Windows.  Pour savoir comment écrire des contacts, consultez [Exemple de carte de visite](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration).

Votre application doit également écrire une annotation sur chaque contact. Les annotations sont des données issues de votre application qui sont associées à un contact. L’annotation doit contenir la classe activable correspondant à la vue de votre choix dans son membre **ProviderProperties** et déclarer prendre en charge l’opération **ContactProfile**.

Vous pouvez annoter les contacts à tout moment lorsque votre application est en cours d’exécution, mais généralement vous devez les annoter dès qu’ils sont ajoutés au magasin de contact Windows.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and contact panel support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactPanelAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations.ContactProfile;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation));
}
```

«appId» est le nom de la famille de packages, suivi de «!» et l’ID de la classe activable. Pour rechercher votre Nom de la famille de packages, ouvrez **Package.appxmanifest** à l’aide de l’éditeur par défaut, puis regardez dans l’onglet «Packages». Ici, «Application» est la classe activable correspondant à l’affichage de démarrage de l'application.

## <a name="allow-contacts-to-invite-new-potential-users"></a>Autoriser des contacts à inviter de nouveaux utilisateurs potentiels

Par défaut, votre application s’affiche uniquement dans le volet de contact pour les contacts que vous avez spécifiquement annotés.  Il s’agit d’éviter toute confusion avec les contacts avec lesquels il est impossible d’interagir par le biais de votre application.  Si vous souhaitez que votre application s’affiche pour les contacts inconnus de votre application (pour inviter les utilisateurs à ajouter ce contact à leur compte, par exemple), vous pouvez ajouter les éléments suivants à votre manifeste:

**Avant**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel" />
      </Extensions>
    </Application>
</Applications>
```

**Après**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel">
            <uap4:ContactPanel SupportsUnknownContacts="true" />
        </uap4:Extension>
      </Extensions>
    </Application>
</Applications>
```

Par cette modification, votre application s’affiche comme une option disponible dans le volet de contact pour tous les contacts que l’utilisateur a épinglés.  Lorsque votre application est activée à l’aide du contrat du volet de contact, vous devez vérifier si le contact fait partie de ceux que connaît votre application.  Si ce n’est pas le cas, vous devez afficher la nouvelle expérience utilisateur de votre application.

![Volet de contact Mes contacts](images/my-people.png)

## <a name="support-for-email-apps"></a>Prise en charge des applications de messagerie

Si vous écrivez une application de messagerie, vous n’avez pas besoin d’annoter chaque contact manuellement.  Si vous déclarez la prise en charge du volet de contact et du protocole mailto:, votre application s’affiche automatiquement pour les utilisateurs avec une adresse de messagerie.

## <a name="running-in-the-contact-panel"></a>Exécution dans le volet de contact

Maintenant que votre application s’affiche dans le volet de contact pour certains ou tous les utilisateurs, vous devez gérer l’activation avec le contrat du volet de contact.

```Csharp
override protected void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.ContactPanel)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        var rootFrame = new Frame();

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        // Navigate to the page that shows the Contact UI.
        rootFrame.Navigate(typeof(ContactPage), e);

        // Ensure the current window is active
        Window.Current.Activate();
    }
}
```

Une fois votre application activée avec ce contrat, elle reçoit un [objet ContactPanelActivatedEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.activation.contactpanelactivatedeventargs).  Celui-ci contient l’ID du contact avec lequel votre application tente d’interagir lors du lancement, et un objet [ContactPanel](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactpanel). Vous devez conserver une référence à cet objet ContactPanel, ce qui vous permettra d’interagir avec le volet.

L’objet ContactPanel comporte deux événements que votre application doit écouter:
+ L’événement **LaunchFullAppRequested** est envoyé dès que l’utilisateur appelle l’élément d’interface utilisateur qui demande que votre application complète soit lancée dans sa propre fenêtre.  Votre application est chargée de se lancer elle-même, en transmettant tout le contexte nécessaire.  Vous êtes libre de le faire comme bon vous semble (par exemple, via le lancement du protocole).
+ L’**événement de fermeture** est envoyé lorsque votre application est sur le point d’être fermée, ce qui vous permet d’enregistrer le contexte.

L’objet ContactPanel vous permet également de définir la couleur d’arrière-plan de l’en-tête du volet de contact (s’il n’est pas défini, le thème du système sera appliqué par défaut) et de fermer par programme le volet de contact.

## <a name="supporting-notification-badging"></a>Prise en charge des badges de notification

Si vous souhaitez que les contacts épinglés à la barre des tâches reçoivent un badge lors de l’arrivée de nouvelles notifications liées à cette personne depuis votre application, vous devez inclure le paramètre **hint-personnes** dans vos [notifications toast](https://docs.microsoft.com/en-us/windows/uwp/shell/tiles-and-notifications/adaptive-interactive-toasts) et vos [Notifications de mes contacts](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-notifications) expressives.

![Badge de notification de contacts](images/my-people-badging.png)

Pour qu'un badge soit appliqué à une contact, le nœud toast de niveau supérieur doit inclure le paramètre hint-people pour indiquer le contact expéditeur ou associé. Ce paramètre peut prendre l'une des valeurs suivantes:
+ **Adresse électronique** 
    + Par exemple, mailto:johndoe@mydomain.com
+ **Numéro de téléphone** 
    + Par exemple, tél: 888-888-8888
+ **ID distant** 
    + Par exemple, remoteid:1234

Voici un exemple de procédure pour voir si une notification toast est liée à une personne spécifique:
```XML
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastText01">
            <text>John Doe posted a comment.</text>
        </binding>
    </visual>
</toast>
```

> [!NOTE]
> Si votre application utilise les [API ContactStore](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) et la propriété [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) pour lier des contacts stockés sur le PC avec des contacts stockés à distance, il est essentiel que la valeur de la propriété RemoteID soit stable et unique. Cela signifie que l’ID distant doit identifier de manière cohérente un compte d’utilisateur unique et doit contenir une balise unique afin de garantir qu’il n’est pas en conflit avec les ID distants des autres contacts sur le PC, y compris les contacts qui appartiennent à d’autres applications.
> S’il n’est pas garanti que les ID à distance utilisés par votre application soient stables et uniques, vous pouvez utiliser la classe RemoteIdHelper indiquée plus loin dans cette rubrique afin d’ajouter une balise unique à l’ensemble de vos ID distants avant de les ajouter au système. Vous pouvez également choisir de ne pas utiliser la propriété RemoteID et de créer à la place une propriété personnalisée étendue dans laquelle stocker les ID distants de vos contacts.

## <a name="the-pinnedcontactmanager-class"></a>Classe PinnedContactManager

[PinnedContactManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager) est utilisée pour gérer les contacts épinglés à la barre des tâches. Cette classe vous permet d’épingler et de désépingler des contacts, de déterminer si un contact est épinglé et si l’épinglage sur une surface spécifique est pris en charge par le système sur lequel votre application s’exécute.

Vous pouvez récupérer l’objet PinnedContactManager à l’aide de la méthode **GetDefault**:

```Csharp
PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
```

## <a name="pinning-and-unpinning-contacts"></a>Épingler et désépingler des contacts
Vous pouvez désormais épingler et désépingler des contacts à l’aide de la classe PinnedContactManager que vous venez de créer. Les méthodes **RequestPinContactAsync** et **RequestUnpinContactAsync** fournissent à l’utilisateur des boîtes de dialogue de confirmation, qui doivent être appelées à partir du thread unique cloisonné d’application (ASTA) ou d’interface utilisateur.

```Csharp
async void PinContact (Contact contact)
{
    await pinnedContactManager.RequestPinContactAsync(contact,
                                                      PinnedContactSurface.Taskbar);
}

async void UnpinContact (Contact contact)
{
    await pinnedContactManager.RequestUnpinContactAsync(contact,
                                                        PinnedContactSurface.Taskbar);
}
```

Vous pouvez également épingler plusieurs contacts en même temps:

```Csharp
async Task PinMultipleContacts(Contact[] contacts)
{
    await pinnedContactManager.RequestPinContactsAsync(
        contacts, PinnedContactSurface.Taskbar);
}
```

> [!Note]
> Il n’existe actuellement aucune opération permettant de désépingler des contacts.

**Remarque:** 

## <a name="see-also"></a>Articles associés
+ [Partage de mes contacts](my-people-sharing.md)
+ [Notifications de mes contacts](my-people-notifications.md)
+ [Vidéo Channel9 sur l’ajout de la prise en charge de mes contacts à une application](https://channel9.msdn.com/Events/Build/2017/P4056)
+ [Exemple d’intégration de mes contacts](http://aka.ms/mypeoplebuild2017)
+ [Exemples de carte de visite](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
+ [Documentation sur la classe PinnedContactManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager)
+ [Connecter votre application à des actions sur une carte de visite](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/integrating-with-contacts)
