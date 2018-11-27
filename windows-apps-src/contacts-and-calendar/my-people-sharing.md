---
title: Partage de mes contacts
description: Explique comment ajouter la prise en charge du partage de mes contacts
ms.date: 06/28/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 91d88dc78fd02ae3f16e1d980aa207d1dd458417
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7716064"
---
# <a name="my-people-sharing"></a>Partage de mes contacts

La fonctionnalité Mes Contact permet aux utilisateurs d’épingler des contacts à leur barre des tâches pour rester en contact facilement sous Windows à partir de n’importe quelle application. Maintenant, les utilisateurs peuvent partager leurs contacts épinglés en faisant glisser des fichiers à partir de l’Explorateur de fichiers vers Mes Contacts. Ils peuvent également partager tous les contacts du magasin de contacts Windows via l’icône de partage standard. Poursuivez votre lecture pour savoir comment activer votre application en tant que cible de partage Mes Contacts.

![Volet de partage Mes Contacts](images/my-people-sharing.png)

## <a name="requirements"></a>Configuration requise

+ Windows10 et Microsoft Visual Studio2017. Pour en savoir plus sur l’installation, voir [Prendre en main VisualStudio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Connaissances de base de C# ou d’un langage de programmation orienté objet similaire. Pour vous familiariser avec C#, voir [Créer une application «Hello, world»](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="overview"></a>Vue d’ensemble

Pour activer votre application en tant que cible de partage Mes Contacts, procédez comme suit:

1. [Déclarer la prise en charge du contrat d’activation shareTarget dans votre manifeste d’application.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [Annoter les contacts que les utilisateurs peuvent partager à l’aide de votre application.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3. Prendre en charge plusieurs instances de l’application en cours d’exécution en même temps.  Les utilisateurs doivent pouvoir interagir avec une version complète de votre application tout en l’utilisant également pour effectuer des partages avec d’autres personnes. Ils peuvent l’utiliser dans plusieurs fenêtres de partage à la fois. Pour ce faire, votre application doit être en mesure d’exécuter plusieurs vues simultanément. Pour savoir comment procéder, consultez l’article [«afficher plusieurs vues d’une application»](https://docs.microsoft.com/en-us/windows/uwp/layout/show-multiple-views).

Une fois cette opération effectuée, votre application s’affiche en tant que cible de partage dans la fenêtre de partage de mes contacts, qui peut être lancée de deux manières:
1. Un contact est choisi via l’icône de partage.
2. Les fichiers sont glissés et déposés sur un contact épinglé à la barre des tâches.

## <a name="declaring-support-for-the-share-contract"></a>Déclaration de prise en charge pour le contrat de partage

Pour déclarer la prise en charge de votre application en tant que cible de partage, ouvrez tout d’abord votre application dans Visual Studio. À partir de l’**Explorateur de solutions**, cliquez avec le bouton droit sur **Package.appxmanifest** et sélectionnez **Ouvrir avec**. Dans le menu, sélectionnez **Éditeur (de texte) XML** et cliquez sur **OK**. Apportez ensuite les modifications suivantes au manifeste:


**Avant**
```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
    </Application>
</Applications>
```

**Après**

```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
        <Extensions>
            <uap:Extension Category="windows.shareTarget">
                <uap:ShareTarget Description="Share with MyApp">
                    <uap:SupportedFileTypes>
                        <uap:SupportsAnyFileType/>
                    </uap:SupportedFileTypes>
                    <uap:DataFormat>Text</uap:DataFormat>
                    <uap:DataFormat>Bitmap</uap:DataFormat>
                    <uap:DataFormat>Html</uap:DataFormat>
                    <uap:DataFormat>StorageItems</uap:DataFormat>
                    <uap:DataFormat>URI</uap:DataFormat>
                </uap:ShareTarget>
            </uap:Extension>
         </Extensions>
    </Application>
</Applications>
```

Ce code ajoute la prise en charge de tous les formats de fichiers et de données. Toutefois, vous pouvez choisir de spécifier les types de fichiers et les formats de données pris en charge (voir la  [documentation relative à la classe ShareTarget](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget) pour plus d’informations).

## <a name="annotating-contacts"></a>Annoter des contacts

Pour autoriser la fenêtre de partage de mes contacts à afficher votre application en tant que cible de partage pour vos contacts, vous devez les écrire dans le magasin de contacts Windows. Pour savoir comment écrire des contacts, reportez-vous à l’[exemple d’intégration à une carte de visite](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration). 

Pour que votre application s’affiche en tant que cible de partage de mes contacts lorsque vous partagez un contact, celle-ci doit écrire une annotation dans ce contact. Les annotations sont des données issues de votre application qui sont associées à un contact. L’annotation doit contenir la classe activable correspondant à la vue de votre choix dans son membre **ProviderProperties** et déclarer prendre en charge l’opération **Share**.

Vous pouvez annoter les contacts à tout moment lorsque votre application est en cours d’exécution, mais généralement vous devez les annoter dès qu’ils sont ajoutés au magasin de contacts Windows.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and Share support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactShareAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::Share;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation);
}
```

«appId» est le nom de la famille de packages, suivi de «!» et l’ID de la classe activable. Pour rechercher le nom de famille de packages, ouvrez **Package.appxmanifest** à l’aide de l’éditeur par défaut, puis regardez dans l’onglet «Packages». Ici, «Application» est la classe activable correspondant à la vue de cible de partage.

## <a name="running-as-a-my-people-share-target"></a>En cours d’exécution en tant que cible de partage de mes contacts

Enfin, pour exécuter l’application, remplacez la méthode [OnShareTargetActivated](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) de la classe principale de votre application pour gérer l’activation de la cible de partage. La propriété [ShareTargetActivatedEventArgs.ShareOperation.Contacts](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) contiendra le(s) contact(s) partagé(s) ou sera vide s’il s’agit d’une opération de partage standard (pas un partage de mes contacts).

```Csharp
protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isPeopleShare = false;
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
    {
        // Make sure the current OS version includes the My People feature before
        // accessing the ShareOperation.Contacts property
        isPeopleShare = (args.ShareOperation.Contacts.Count > 0);
    }

    if (isPeopleShare)
    {
        // Show share UI for MyPeople contact(s)
    }
    else
    {
        // Show standard share UI for unpinned contacts
    }
}
```

## <a name="see-also"></a>Articles associés
+ [Ajout de la prise en charge de Mes Contacts](my-people-support.md)
+ [Classe ShareTarget](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [Exemple d’intégration à une carte de visite](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
