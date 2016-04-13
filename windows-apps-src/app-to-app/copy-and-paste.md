---
description: Cet article explique comment prendre en charge le copier-coller dans les applications UWP en utilisant le Presse-papiers.
title: Copier et coller
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
author: awkoren
---
#Copier et coller

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cet article explique comment prendre en charge le copier-coller dans les applications UWP en utilisant le Presse-papiers. Le copier-coller est la méthode classique d’échanger des données entre les applications, ou dans une application, et presque chaque application peut prendre en charge les opérations du Presse-papiers dans une certaine mesure.

## Rechercher la prise en charge intégrée du Presse-papiers


Le plus souvent, vous n’avez pas besoin d’écrire de code supplémentaire pour fournir une prise en charge des opérations du Presse-papiers. De nombreux contrôles XAML par défaut disponibles pour créer les applications offrent déjà une prise en charge des opérations du Presse-papiers. Pour plus d’informations sur les contrôles disponibles, voir [Liste des contrôles][ControlsList].

## Préparation

Tout d’abord, incluez l’espace de noms [**Windows.ApplicationModel.DataTransfer**][DataTransfer] dans votre application. Ensuite, ajoutez une instance à l’objet [**DataPackage**][DataPackage]. Cet objet contient les données que l’utilisateur souhaite copier ainsi que les propriétés (telles qu’une description) que vous voulez ajouter.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

## Copier et Couper

Copier et Couper (également appelé déplacement) fonctionne presque exactement de la même manière. Choisissez l’opération que vous souhaitez effectuer à l’aide de la propriété [**DataPackage.RequestedOperation**][RequestedOperation].

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

Vous pouvez ensuite ajouter les données sélectionnées par l’utilisateur dans l’objet [**DataPackage**][DataPackage]. Si les données sont prises en charge par la classe **DataPackage**, utilisez l’une des méthodes correspondantes dans l’objet **DataPackage**. Voici comment ajouter du texte :

```cs
dataPackage.SetText("Hello World!");
```

La dernière étape consiste à ajouter le [**DataPackage**][DataPackage] dans le Presse-papiers en appelant la méthode statique [**Clipboard.SetContent**][SetContent].

```cs
Clipboard.SetContent(dataPackage);
```
## Coller

Pour obtenir le contenu du Presse-papiers, appelez la méthode statique **Clipboard.getContent**[GetContent]. Cette méthode renvoie un objet [**DataPackageView**][DataPackageView] avec son contenu. Cet objet est identique à l’objet [**DataPackage**][DataPackage], sauf qu’il est en lecture seule. Avec cet objet, vous pouvez utiliser la propriété [**AvailableFormats**][AvailableFormats] ou la méthode [**Contains**][Contains] pour identifier les formats disponibles. Ensuite, appelez la méthode **DataPackageView** correspondante pour obtenir les données.

```cs
DataPackageView dataPackageView = Clipboard.GetContent();
if (dataPackageView.Contains(StandardDataFormats.Text))
{
    string text = await dataPackageView.GetTextAsync();
    // To output the text from this example, you need a TextBlock control
    TextOutput.Text = "Clipboard now contains: " + text;
}
```

## Suivi des modifications dans le Presse-papiers

En plus des commandes copier et coller, vous pouvez également effectuer le suivi des modifications dans le Presse-papiers. Vous pouvez le faire en gérant l’événement [**Clipboard.ContentChanged**][ContentChanged] du Presse-papiers.

```cs
Clipboard.ContentChanged += (s, e) => 
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

<!-- LINKS --> 
[DataTransfer]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.aspx 
[DataPackage]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx 
[DataPackageView]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx
[DataPackagePropertySet]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx 
[DataRequest]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx 
[DataRequested]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx 
[FailWithDisplayText]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx
[ShowShareUi]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx
[RequestedOperation]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx 
[ControlsList]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/mt185406.aspx 
[SetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx 
[GetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx
[AvailableFormats]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx 
[Contains]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx
[ContentChanged]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx 



<!--HONumber=Mar16_HO5-->


