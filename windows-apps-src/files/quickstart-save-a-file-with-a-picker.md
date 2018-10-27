---
author: laurenhughes
ms.assetid: 8BDDE64A-77D2-4F9D-A1A0-E4C634BCD890
title: Enregistrer un fichier avec un sélecteur
description: Utilisez FileSavePicker pour permettre aux utilisateurs de spécifier le nom et l’emplacement où ils souhaitent que votre application enregistre un fichier.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2a053047324fcb795a30951d70c5e0e78fbb5547
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5704832"
---
# <a name="save-a-file-with-a-picker"></a>Enregistrer un fichier avec un sélecteur

**API importantes**

-   [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)
-   [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)

Utilisez [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) pour permettre aux utilisateurs de spécifier le nom et l’emplacement où ils souhaitent que votre application enregistre un fichier.

> [!NOTE]
> Consultez également l’[exemple de sélecteur de fichiers](http://go.microsoft.com/fwlink/p/?linkid=619994).

 

## <a name="prerequisites"></a>Prérequis


-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Autorisations d’accès à l’emplacement**

    Voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## <a name="filesavepicker-step-by-step"></a>Sélecteur FileSavePicker : pas à pas

Utilisez un [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) pour permettre à l’utilisateur de spécifier le nom, le type et l’emplacement d’un fichier à enregistrer. Créez, personnalisez et affichez un objet sélecteur de fichiers, puis enregistrez des données via l’objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) retourné qui représente le fichier sélectionné.

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

Définissez des propriétés sur l’objet sélecteur de fichiers qui sont pertinentes pour vos utilisateurs et votre application.

Cet exemple définit troispropriétés: [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207880), [**FileTypeChoices**](https://msdn.microsoft.com/library/windows/apps/br207875) et [**SuggestedFileName**](https://msdn.microsoft.com/library/windows/apps/br207878).

> [!NOTE]
>Les objets [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) affichent le sélecteur de fichiers à l’aide de [**PickerViewMode.List**](https://msdn.microsoft.com/library/windows/apps/br207891).
     
- Comme l’utilisateur enregistre un document ou un fichier texte, l’exemple définit [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207880) sur le dossier local de l’application, avec la propriété [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621). Définissez [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) sur un emplacement approprié pour le type de fichier enregistré, par exemple, Musique, Images, Vidéos ou Documents. À partir de l’emplacement de départ, l’utilisateur peut accéder à d’autres emplacements.

- Pour nous assurer que l’application puisse ouvrir le fichier une fois celui-ci enregistré, nous utilisons [**FileTypeChoices**](https://msdn.microsoft.com/library/windows/apps/br207875) pour spécifier les types de fichiers que l’exemple prend en charge (documents Microsoft Word et fichiers texte). Assurez-vous que tous les types de fichiers que vous spécifiez sont pris en charge par votre application. L’utilisateur peut enregistrer son fichier sous tout type de fichier que vous spécifiez. Il peut également modifier le type de fichier en sélectionnant un autre type que vous avez spécifié. Le premier choix de type de fichier dans la liste est sélectionné par défaut: pour le contrôler, définissez la propriété [**DefaultFileExtension**](https://msdn.microsoft.com/library/windows/apps/br207873).

> [!NOTE]
> Le sélecteur de fichiers utilise également le type de fichier actuellement sélectionné pour filtrer les fichiers affichés afin que seuls ceux du type sélectionné soient présentés à l’utilisateur.

- Pour faciliter la saisie, l’exemple définit un [**SuggestedFileName**](https://msdn.microsoft.com/library/windows/apps/br207878). Assurez-vous que le nom de fichier suggéré est pertinent pour le fichier enregistré. Par exemple, comme Word, vous pouvez suggérer le nom du fichier existant éventuel, ou la première ligne d’un document si l’utilisateur souhaite enregistrer un fichier qui ne possède pas encore de nom.

2.  **Afficher le sélecteur FileSavePicker et enregistrer dans le fichier sélectionné**

    Affichez le sélecteur en appelant la méthode [**PickSaveFileAsync**](https://msdn.microsoft.com/library/windows/apps/br207876). Une fois que l’utilisateur a spécifié le nom, le type et l’emplacement du fichier et qu’il a confirmé l’enregistrement du fichier, **PickSaveFileAsync** retourne un objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) qui représente le fichier enregistré. À présent que vous disposez d’un accès en lecture et en écriture, vous pouvez capturer et traiter ce fichier.

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

**Conseil**vous devez toujours vérifier le fichier enregistré pour vous assurer qu’il est valide avant de poursuivre tout autre traitement. Ensuite, vous pouvez enregistrer du contenu dans le fichier si cela est opportun pour votre application et fournir le comportement approprié si le fichier sélectionné n’est pas valide.
