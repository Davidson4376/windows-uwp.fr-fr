---
author: laurenhughes
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: Détermination de la disponibilité des fichiers MicrosoftOneDrive
description: Déterminez si un fichier MicrosoftOneDrive est disponible à l’aide de la propriété StorageFile.IsAvailable.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3c5de157d320b401fdc0e542eb0f1bdc241e2f21
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "459762"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>Détermination de la disponibilité des fichiers Microsoft OneDrive


**API importantes**

-   [**Classe FileIO**](https://msdn.microsoft.com/library/windows/apps/Hh701440)
-   [**Classe StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)
-   [**Propriété StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx)

Déterminez si un fichier MicrosoftOneDrive est disponible à l’aide de la propriété [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx).

## <a name="prerequisites"></a>Prérequis

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/Mt187337). Pour apprendre à écrire des applications asynchrones enC++, voir [Programmation asynchrone enC++](https://msdn.microsoft.com/library/windows/apps/Mt187334).

-   **Déclarations des fonctionnalités d’application**

    Voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## <a name="using-the-storagefileisavailable-property"></a>Utilisation de la propriété StorageFile.IsAvailable

Les utilisateurs peuvent marquer les fichiers OneDrive comme étant disponibles hors connexion (par défaut) ou en ligne uniquement. Cette fonctionnalité permet aux utilisateurs de déplacer des fichiers volumineux (tels que des images et des vidéos) vers leur emplacement OneDrive, de les marquer comme étant en ligne uniquement et d’économiser de l’espace disque (le seul élément conservé localement est un fichier de métadonnées).

[**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) permet de déterminer si un fichier est actuellement disponible. Le tableau suivant indique la valeur de la propriété **StorageFile.IsAvailable** dans différents scénarios.

| Type de fichier                              | En ligne | Connexion réseau limitée        | Hors connexion |
|-------------------------------------------|--------|------------------------|---------|
| Fichier local                                | True   | True                   | True    |
| Fichier OneDrive marqué comme étant disponible hors connexion | True   | True                   | True    |
| Fichier OneDrive marqué comme étant en ligne uniquement       | True   | Dépend des paramètres utilisateur | False   |
| Fichier réseau                              | True   | Dépend des paramètres utilisateur | False   |

 

Les étapes suivantes illustrent comment déterminer si un fichier est actuellement disponible.

1.  Déclarez une fonctionnalité appropriée pour la bibliothèque à laquelle vous voulez accéder.
2.  Incluez l’espace de noms [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346). Cet espace de noms comprend les types qui permettent de gérer les fichiers, les dossiers et les paramètres d’application. Il comprend également le type [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) nécessaire.
3.  Obtenez un objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) pour le ou les fichiers souhaités. Si vous énumérez une bibliothèque, vous devez généralement effectuer cette étape en appelant la méthode [**StorageFolder.CreateFileQuery**](https://msdn.microsoft.com/library/windows/apps/BR227252), puis la méthode [**GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276.aspx) de l’objet [**StorageFileQueryResult**](https://msdn.microsoft.com/library/windows/apps/BR208046) résultant. La méthode **GetFilesAsync** retourne une collection [IReadOnlyList](http://go.microsoft.com/fwlink/p/?LinkId=324970) d’objets **StorageFile**.
4.  Une fois que vous avez accès à un objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) qui représente le ou les fichiers souhaités, la valeur de la propriété [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) indique si le fichier est disponible ou non.

La méthode générique suivante montre comment énumérer un dossier et retourner la collection d’objets [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) pour ce dossier. La méthode d’appel itère la collection retournée en référençant la propriété [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) pour chaque fichier.

```cs
/// <summary>
/// Generic function that retrieves all files from the specified folder.
/// </summary>
/// <param name="folder">The folder to be searched.</param>
/// <returns>An IReadOnlyList collection containing the file objects.</returns>
async Task<System.Collections.Generic.IReadOnlyList<StorageFile>> GetLibraryFilesAsync(StorageFolder folder)
{
    var query = folder.CreateFileQuery();
    return await query.GetFilesAsync();
}

private async void CheckAvailabilityOfFilesInPicturesLibrary()
{
    // Determine availability of all files within Pictures library.
    var files = await GetLibraryFilesAsync(KnownFolders.PicturesLibrary);
    for (int i = 0; i < files.Count; i++)
    {
        StorageFile file = files[i];

        StringBuilder fileInfo = new StringBuilder();
        fileInfo.AppendFormat("{0} (on {1}) is {2}",
                    file.Name,
                    file.Provider.DisplayName,
                    file.IsAvailable ? "available" : "not available");
    }
}
```
