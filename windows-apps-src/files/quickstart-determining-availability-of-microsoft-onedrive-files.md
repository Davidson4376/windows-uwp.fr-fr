---
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: Détermination de la disponibilité des fichiers Microsoft OneDrive
description: Déterminez si un fichier Microsoft OneDrive est disponible à l’aide de la propriété StorageFile.IsAvailable.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: effb28fa453ec884152dbc404245f00f4893ef5a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369426"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>Détermination de la disponibilité des fichiers Microsoft OneDrive


**API importantes**

-   [**FileIO classe**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
-   [**Classe de StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)
-   [**Propriété de StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable)

Déterminez si un fichier Microsoft OneDrive est disponible à l’aide de la propriété [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable).

## <a name="prerequisites"></a>Prérequis

-   **Comprendre la programmation asynchrone pour les applications de plateforme universelle Windows (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Déclarations Betacam d’application**

    Voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## <a name="using-the-storagefileisavailable-property"></a>Utilisation de la propriété StorageFile.IsAvailable

Les utilisateurs peuvent marquer les fichiers OneDrive comme étant disponibles hors connexion (par défaut) ou en ligne uniquement. Cette fonctionnalité permet aux utilisateurs de déplacer des fichiers volumineux (tels que des images et des vidéos) vers leur emplacement OneDrive, de les marquer comme étant en ligne uniquement et d’économiser de l’espace disque (le seul élément conservé localement est un fichier de métadonnées).

[**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable), est utilisée pour déterminer si un fichier est actuellement disponible. Le tableau suivant indique la valeur de la propriété **StorageFile.IsAvailable** dans différents scénarios.

| Type de fichier                              | La licence | Connexion réseau limitée        | Hors connexion |
|-------------------------------------------|--------|------------------------|---------|
| Fichier local                                | True   | True                   | True    |
| Fichier OneDrive marqué comme étant disponible hors connexion | True   | True                   | True    |
| Fichier OneDrive marqué comme étant en ligne uniquement       | True   | Dépend des paramètres utilisateur | False   |
| Fichier réseau                              | True   | Dépend des paramètres utilisateur | False   |

 

Les étapes suivantes illustrent comment déterminer si un fichier est actuellement disponible.

1.  Déclarez une fonctionnalité appropriée pour la bibliothèque à laquelle vous voulez accéder.
2.  Incluez l’espace de noms [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage). Cet espace de noms comprend les types qui permettent de gérer les fichiers, les dossiers et les paramètres d’application. Il comprend également le type [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) nécessaire.
3.  Obtenez un objet [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) pour le ou les fichiers souhaités. Si vous énumérez une bibliothèque, vous devez généralement effectuer cette étape en appelant la méthode [**StorageFolder.CreateFileQuery**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfilequery), puis la méthode [**GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) de l’objet [**StorageFileQueryResult**](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.StorageFileQueryResult) résultant. La méthode **GetFilesAsync** retourne une collection [IReadOnlyList](https://go.microsoft.com/fwlink/p/?LinkId=324970) d’objets **StorageFile**.
4.  Une fois que vous avez accès à un objet [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) qui représente le ou les fichiers souhaités, la valeur de la propriété [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) indique si le fichier est disponible ou non.

La méthode générique suivante montre comment énumérer un dossier et retourner la collection d’objets [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) pour ce dossier. La méthode d’appel itère la collection retournée en référençant la propriété [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) pour chaque fichier.

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
