---
description: Cet article explique comment prendre en charge le copier-coller dans les applications UWP en utilisant le Presse-papiers.
title: Copier et coller
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ba498a9bc6541f45f01e74b1ea90f4f9a1bfde90
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8736967"
---
# <a name="copy-and-paste"></a>Copier et coller

Cet article explique comment prendre en charge le copier-coller dans les applications UWP en utilisant le Presse-papiers. Le copier-coller est la méthode classique d’échanger des données entre les applications, ou dans une application, et presque chaque application peut prendre en charge les opérations du Presse-papiers dans une certaine mesure.

## <a name="check-for-built-in-clipboard-support"></a>Rechercher la prise en charge intégrée du Presse-papiers

Le plus souvent, vous n’avez pas besoin d’écrire de code supplémentaire pour fournir une prise en charge des opérations du Presse-papiers. De nombreux contrôles XAML par défaut disponibles pour créer les applications offrent déjà une prise en charge des opérations du Presse-papiers. 

## <a name="get-set-up"></a>Préparation

Tout d’abord, incluez l’espace de noms [**Windows.ApplicationModel.DataTransfer**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer) dans votre application. Ensuite, ajoutez une instance à l’objet [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage). Cet objet contient les données que l’utilisateur souhaite copier ainsi que les propriétés (telles qu’une description) que vous voulez ajouter.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>Copier et Couper

Copier et Couper (également appelé *déplacement*) fonctionne presque exactement de la même manière. Choisissez l’opération que vous souhaitez effectuer à l’aide de la propriété [**RequestedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage.RequestedOperation).

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```
## <a name="drag-and-drop"></a>Glisser-déplacer

Vous pouvez ensuite ajouter les données sélectionnées par l’utilisateur dans l’objet [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage). Si les données sont prises en charge par la classe **DataPackage**, utilisez l’une des méthodes correspondantes dans l’objet **DataPackage**. Voici comment ajouter du texte:

```cs
dataPackage.SetText("Hello World!");
```

La dernière étape consiste à ajouter l’objet [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) dans le Presse-papiers en appelant la méthode statique [**SetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.SetContent(Windows.ApplicationModel.DataTransfer.DataPackage)).

```cs
Clipboard.SetContent(dataPackage);
```
## <a name="paste"></a>Coller

Pour obtenir le contenu du Presse-papiers, appelez la méthode statique [**GetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.GetContent). Cette méthode renvoie un objet [**DataPackageView**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView) avec son contenu. Cet objet est identique à l’objet [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage), sauf qu’il est en lecture seule. Avec cet objet, vous pouvez utiliser la propriété [**AvailableFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.AvailableFormats) ou la méthode [**Contains**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.Contains(System.String)) pour identifier les formats disponibles. Ensuite, appelez la méthode [**DataPackageView**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView) correspondante pour obtenir les données.

```cs
async void OutputClipboardText()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="track-changes-to-the-clipboard"></a>Suivi des modifications dans le Presse-papiers

En plus des commandes copier et coller, vous pouvez également effectuer le suivi des modifications dans le Presse-papiers. Vous pouvez le faire en gérant l’événement [**ContentChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.ContentChanged) du Presse-papiers.

```cs
Clipboard.ContentChanged += async (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="see-also"></a>Voir également

* [Communication entre les applications](index.md)
* [DataTransfer](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.aspx)
* [DataPackage](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx)
* [DataPackageView](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx)
* [DataPackagePropertySet]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx) 
* [DataRequested]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx)
* [ShowShareUi](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx)
* [RequestedOperation](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx) 
* [ControlsList](https://msdn.microsoft.com/library/windows/apps/xaml/mt185406.aspx)
* [SetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx)
* [GetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx)
* [AvailableFormats](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx)
* [Contains](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx)
* [ContentChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx)

