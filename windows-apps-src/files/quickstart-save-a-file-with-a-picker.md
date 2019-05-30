---
ms.assetid: 8BDDE64A-77D2-4F9D-A1A0-E4C634BCD890
title: Enregistrer un fichier avec un sélecteur
description: Utilisez FileSavePicker pour permettre aux utilisateurs de spécifier le nom et l’emplacement où ils souhaitent que votre application enregistre un fichier.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c1480c17d97cb8b5e227cc44b384f13095bfd469
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369252"
---
# <a name="save-a-file-with-a-picker"></a>Enregistrer un fichier avec un sélecteur

**API importantes**

-   [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker)
-   [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)

Utilisez [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) pour permettre aux utilisateurs de spécifier le nom et l’emplacement où ils souhaitent que votre application enregistre un fichier.

> [!NOTE]
> Pour obtenir un exemple complet, consultez la [exemple de sélecteur de fichier](https://go.microsoft.com/fwlink/p/?linkid=619994).

 

## <a name="prerequisites"></a>Prérequis


-   **Comprendre la programmation asynchrone pour les applications de plateforme universelle Windows (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Autorisations d’accès à l’emplacement**

    Voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## <a name="filesavepicker-step-by-step"></a>Sélecteur FileSavePicker : pas à pas

Utilisez un [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) pour permettre à l’utilisateur de spécifier le nom, le type et l’emplacement d’un fichier à enregistrer. Créez, personnalisez et affichez un objet sélecteur de fichiers, puis enregistrez des données via l’objet [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) retourné qui représente le fichier sélectionné.

1.  **Créer et personnaliser le FileSavePicker**

    ```cs
    var savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";
    ```

Définissez des propriétés sur l’objet sélecteur de fichiers qui sont pertinentes pour vos utilisateurs et votre application. Cet exemple définit trois propriétés : [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedstartlocation), [ **FileTypeChoices** ](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.filetypechoices) et [ **SuggestedFileName**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedfilename).
     
- Comme l’utilisateur enregistre un document ou un fichier texte, l’exemple définit [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedstartlocation) sur le dossier local de l’application, avec la propriété [**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder). Définissez [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) sur un emplacement approprié pour le type de fichier enregistré, par exemple, Musique, Images, Vidéos ou Documents. À partir de l’emplacement de départ, l’utilisateur peut accéder à d’autres emplacements.

- Pour nous assurer que l’application puisse ouvrir le fichier une fois celui-ci enregistré, nous utilisons [**FileTypeChoices**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.filetypechoices) pour spécifier les types de fichiers que l’exemple prend en charge (documents Microsoft Word et fichiers texte). Assurez-vous que tous les types de fichiers que vous spécifiez sont pris en charge par votre application. L’utilisateur peut enregistrer son fichier sous tout type de fichier que vous spécifiez. Il peut également modifier le type de fichier en sélectionnant un autre type que vous avez spécifié. Le premier choix de type de fichier dans la liste est sélectionné par défaut : pour contrôler cela, définissez la propriété [**DefaultFileExtension**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.defaultfileextension).

    > [!NOTE]
    > Le sélecteur de fichiers utilise également le type de fichier actuellement sélectionné pour filtrer les fichiers qui y sont affichées, et que seuls les types de fichiers qui correspondent aux types de fichiers sélectionnés sont affichés à l’utilisateur.

- Pour faciliter la saisie, l’exemple définit un [**SuggestedFileName**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedfilename). Assurez-vous que le nom de fichier suggéré est pertinent pour le fichier enregistré. Par exemple, comme Word, vous pouvez suggérer le nom du fichier existant éventuel, ou la première ligne d’un document si l’utilisateur souhaite enregistrer un fichier qui ne possède pas encore de nom.

> [!NOTE]
> [**FileSavePicker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) objets affichent le sélecteur de fichier à l’aide de la [ **PickerViewMode.List** ](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerViewMode) mode d’affichage.

2.  **Afficher le FileSavePicker et l’enregistrer dans le fichier sélectionnée**

    Affichez le sélecteur en appelant la méthode [**PickSaveFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.picksavefileasync). Une fois que l’utilisateur a spécifié le nom, le type et l’emplacement du fichier et qu’il a confirmé l’enregistrement du fichier, **PickSaveFileAsync** retourne un objet [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) qui représente le fichier enregistré. À présent que vous disposez d’un accès en lecture et en écriture, vous pouvez capturer et traiter ce fichier.

    ```cs
    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
        if (file != null)
        {
            // Prevent updates to the remote version of the file until
            // we finish making changes and call CompleteUpdatesAsync.
            Windows.Storage.CachedFileManager.DeferUpdates(file);
            // write to file
            await Windows.Storage.FileIO.WriteTextAsync(file, file.Name);
            // Let Windows know that we're finished changing the file so
            // the other app can update the remote version of the file.
            // Completing updates may require Windows to ask for user input.
            Windows.Storage.Provider.FileUpdateStatus status =
                await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
            if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
            {
                this.textBlock.Text = "File " + file.Name + " was saved.";
            }
            else
            {
                this.textBlock.Text = "File " + file.Name + " couldn't be saved.";
            }
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
    ```

L’exemple vérifie que le fichier est valide et y écrit son propre nom de fichier. Voir aussi [Création, écriture et lecture de fichier](quickstart-reading-and-writing-files.md).

> [!TIP]
> Vous devez toujours vérifier le fichier enregistré pour vous assurer qu’il est valide avant d’exécuter tout autre traitement. Ensuite, vous pouvez enregistrer du contenu dans le fichier si cela est opportun pour votre application et fournir le comportement approprié si le fichier sélectionné n’est pas valide.
