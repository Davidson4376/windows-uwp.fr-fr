---
description: Cet article explique comment recevoir dans votre application UWP du contenu partagé à partir d’une autre application à l’aide du contrat de partage. Ce contrat de partage permet à votre application d’être présentée en tant qu’option quand l’utilisateur appelle l’option Partager.
title: Recevoir des données
ms.assetid: 0AFF9E0D-DFF4-4018-B393-A26B11AFDB41
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7d64e4a2d4251aca6bbce39b5f24e3e5f35295b8
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6048676"
---
# <a name="receive-data"></a>Recevoir des données



Cet article explique comment recevoir dans votre application UWP du contenu partagé à partir d’une autre application à l’aide du contrat de partage. Ce contrat de partage permet à votre application d’être présentée en tant qu’option quand l’utilisateur appelle l’option Partager.

## <a name="declare-your-app-as-a-share-target"></a>Déclaration de votre application comme cible de partage

Le système affiche une liste des applications cibles possibles quand un utilisateur appelle l’option Partager. Pour figurer dans la liste, votre application doit déclarer qu’elle prend en charge le contrat de partage. Cela indique au système que votre application est apte à recevoir du contenu.

1.  Ouvrez le fichier manifeste. Il doit normalement porter un nom similaire à **package.appxmanifest**.
2.  Ouvrez l’onglet **Déclarations**.
3.  Choisissez **Cible du partage** dans la liste **Déclarations disponibles**, puis sélectionnez **Ajouter**.

## <a name="choose-file-types-and-formats"></a>Choix des types et formats de fichier

Ensuite, déterminez les types de fichiers et les formats de données qui sont pris en charge. Les API de partage prennent en charge plusieurs formats standard, parmi lesquels les formats texte, HTML et bitmap. Vous pouvez également spécifier des types de fichiers et formats de données personnalisés. Dans ce cas, vérifiez que les applications sources connaissent ces types et formats afin qu’elles puissent les utiliser pour partager des données.

Enregistrez uniquement les formats gérés par votre application. Seules les applications cibles qui prennent en charge les données partagées apparaissent quand l’utilisateur appelle l’option Partager.

Pour définir les types de fichier:

1.  Ouvrez le fichier manifeste. Il doit normalement avoir un nom similaire à **package.appxmanifest**.
2.  Dans la section **Types de fichiers pris en charge** de la page **Déclarations**, sélectionnez **Ajouter nouveau**.
3.  Tapez l’extension de nom de fichier que vous voulez prendre en charge, par exemple, «.docx». Vous devez inclure le point. Pour prendre en charge tous les types de fichiers, cochez la case **SupportsAnyFileType**.

Pour définir les formats de données:

1.  Ouvrez le fichier manifeste.
2.  Ouvrez la section **Formats de données** de la page **Déclarations** et sélectionnez **Ajouter nouveau**.
3.  Tapez le nom du format de données à prendre en charge, par exemple, «Texte».

## <a name="handle-share-activation"></a>Gestion de l’activation du partage

Quand un utilisateur sélectionne votre application (généralement en la choisissant dans une liste d’applications cibles disponibles dans l’interface utilisateur de partage), un événement [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Application.OnShareTargetActivated(Windows.ApplicationModel.Activation.ShareTargetActivatedEventArgs)) est déclenché. Votre application doit gérer cet événement afin de traiter les données que l’utilisateur veut partager.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    // Code to handle activation goes here. 
} 
```

Les données que l’utilisateur souhaite partager sont stockées dans un objet [**ShareOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation). Vous pouvez utiliser cet objet pour vérifier le format des données qu’il contient.

```cs
ShareOperation shareOperation = args.ShareOperation;
if (shareOperation.Data.Contains(StandardDataFormats.Text))
{
    string text = await shareOperation.Data.GetTextAsync();

    // To output the text from this example, you need a TextBlock control
    // with a name of "sharedContent".
    sharedContent.Text = "Text: " + text;
} 
```

## <a name="report-sharing-status"></a>État de partage de rapports

Dans certains cas, votre application peut avoir besoin de plus de temps pour traiter les données à partager. Il s’agit, par exemple, des partages de collections de fichiers ou d’images. Ces éléments sont plus volumineux qu’une simple chaîne de texte, ce qui explique leur temps de traitement plus long.

```cs
shareOperation.ReportStarted(); 
```

Après avoir appelé la méthode [**ReportStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportStarted), ne prévoyez aucune interaction avec votre application de la part de l’utilisateur. Par conséquent, vous ne devez appeler cette méthode que si votre application est parvenue à un stade de l’opération où elle peut être masquée par l’utilisateur.

Avec un partage étendu, l’utilisateur peut être amené à masquer l’application source avant que votre application n’ait reçu toutes les données de l’objet DataPackage. Pour cette raison, nous recommandons que votre application avertisse le système quand elle a obtenu toutes les données nécessaires. Cela permet au système de suspendre ou d’arrêter l’application source selon les besoins.

```cs
shareOperation.ReportSubmittedBackgroundTask(); 
```

En cas de problème, appelez [**ReportError**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportError(System.String)) pour envoyer un message d’erreur au système. L’utilisateur voit le message quand il vérifie l’état du partage. À ce stade, votre application est arrêtée et le partage est terminé. L’utilisateur devra recommencer pour partager le contenu vers votre application. Selon la situation, vous pouvez décider qu’un certain type d’erreur ne suffit pas pour mettre fin à l’opération de partage. Dans ce cas, vous pouvez omettre d’appeler **ReportError** et continuer le partage.

```cs
shareOperation.ReportError("Could not reach the server! Try again later."); 
```

Enfin, lorsque votre application a terminé de traiter le contenu partagé, appelez la méthode [**ReportCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportCompleted) qui notifie au système la fin de l’opération.

```cs
shareOperation.ReportCompleted();
```

Généralement, vous appelez ces méthodes selon l’ordre indiqué, une seule fois chacune. Toutefois, une application cible peut parfois appeler [**ReportDataRetrieved**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportDataRetrieved) avant [**ReportStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportStarted). Par exemple, l’application peut récupérer les données dans le cadre d’une tâche dans le gestionnaire d’activation, mais ne pas appeler **ReportStarted** tant que l’utilisateur ne sélectionne pas un bouton **Partager**.

## <a name="return-a-quicklink-if-sharing-was-successful"></a>Renvoi d’un QuickLink si le partage a réussi

Lorsqu’un utilisateur sélectionne votre application pour recevoir du contenu, nous vous recommandons de créer un [**QuickLink**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.QuickLink). Un objet **QuickLink** est une sorte de raccourci qui permet à l’utilisateur de partager plus facilement des informations avec votre application. Par exemple, vous pouvez créer un **QuickLink** qui ouvre un nouveau message électronique dans lequel l’adresse de messagerie du destinataire est déjà indiquée.

Un **QuickLink** doit avoir un titre, une icône et un Id. Le titre (comme «message pour maman») et l’icône s’affichent lorsque l’utilisateur appuie sur l’icône partager. L’ID est utilisé par l’application pour accéder à des informations personnalisées, telles qu’une adresse de messagerie ou des informations d’identification. Quand votre application crée un **QuickLink**, elle renvoie ce **QuickLink** au système en appelant [**ReportCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.ReportCompleted).

Un **QuickLink** ne stocke pas de données. Cet objet contient à la place un identificateur qui, lorsqu’il est sélectionné, est envoyé à votre application. Votre application stocke l’ID de **QuickLink** ainsi que les données utilisateur correspondantes. Quand l’utilisateur appuie sur l’objet **QuickLink**, vous pouvez obtenir l’ID de ce dernier à l’aide de la propriété [**QuickLinkId**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation.QuickLinkId).

```cs
async void ReportCompleted(ShareOperation shareOperation, string quickLinkId, string quickLinkTitle)
{
    QuickLink quickLinkInfo = new QuickLink
    {
        Id = quickLinkId,
        Title = quickLinkTitle,

        // For quicklinks, the supported FileTypes and DataFormats are set 
        // independently from the manifest
        SupportedFileTypes = { "*" },
        SupportedDataFormats = { StandardDataFormats.Text, StandardDataFormats.Uri, 
                StandardDataFormats.Bitmap, StandardDataFormats.StorageItems }
    };

    StorageFile iconFile = await Windows.ApplicationModel.Package.Current.InstalledLocation.CreateFileAsync(
            "assets\\user.png", CreationCollisionOption.OpenIfExists);
    quickLinkInfo.Thumbnail = RandomAccessStreamReference.CreateFromFile(iconFile);
    shareOperation.ReportCompleted(quickLinkInfo);
}
```

## <a name="see-also"></a>Voir également 

* [Communication entre les applications](index.md)
* [Partager des données](share-data.md)
* [OnShareTargetActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onsharetargetactivated.aspx)
* [ReportStarted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted.aspx)
* [ReportError](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reporterror.aspx)
* [ReportCompleted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted.aspx)
* [ReportDataRetrieved](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved.aspx)
* [ReportStarted](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted.aspx)
* [QuickLink](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.quicklink.aspx)
* [QuickLInkId](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharetarget.quicklink.id.aspx)
