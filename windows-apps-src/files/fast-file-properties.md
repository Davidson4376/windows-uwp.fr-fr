---
title: Accès rapide aux propriétés de fichier dans UWP
description: Rassemblez efficacement une liste des fichiers et de leurs propriétés à partir d’une bibliothèque à utiliser dans une application UWP.
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, uwp, fichier, propriétés
ms.localizationpriority: medium
ms.openlocfilehash: 5ae884ca5424f50a7a835bc55602b5aa7c54096d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630234"
---
# <a name="fast-access-to-file-properties-in-uwp"></a>Accès rapide aux propriétés de fichier dans UWP 

Découvrez comment récupérer rapidement une liste de fichiers et de leurs propriétés à partir d’une bibliothèque et utiliser ces propriétés dans une application.  

Conditions préalables 
- **Programmation pour les applications de plateforme universelle Windows (UWP) asynchrone**   vous pouvez apprendre à écrire des applications asynchrones C# ou Visual Basic, consultez [appeler les API asynchrones dans C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps). 
- **Autorisations d’accès aux bibliothèques**   requiert le code dans ces exemples le **picturesLibrary** fonctionnalité, mais que votre emplacement de fichier peut nécessiter une fonctionnalité différente, ou aucune fonctionnalité du tout. Pour en savoir plus, voir [Autorisations d’accès aux fichiers](https://docs.microsoft.com/windows/uwp/files/file-access-permissions). 
- **Énumération de fichiers simple**    cet exemple utilise [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) à définir quelques propriétés avancées d’énumération. Pour en savoir plus sur l’obtention d’une simple liste de fichiers pour un petit répertoire, voir [Énumérer et interroger des fichiers et dossiers](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders). 

## <a name="usage"></a>Utilisation  
De nombreuses applications doivent répertorier les propriétés d’un groupe de fichiers, mais n'ont pas toujours besoin d’interagir directement avec les fichiers. Par exemple, une application de musique lit (ouvre) un fichier à la fois, mais a besoin des propriétés de tous les fichiers d'un dossier pour que l’application puisse afficher la file d’attente des morceaux ou pour que l’utilisateur puisse choisir un fichier valide à lire. 

Les exemples de cette page ne doivent pas être utilisés dans des applications qui modifient les métadonnées de chaque fichier ou application qui interagit avec tous les StorageFiles résultants en plus de lire leurs propriétés. Voir [Énumérer et interroger des fichiers et dossiers](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders) pour plus d’informations. 

## <a name="enumerate-all-the-pictures-in-a-location"></a>Énumérer toutes les images d'un emplacement 
Dans cet exemple, nous allons :
-  Créer un objet [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) pour spécifier que l’application souhaite énumérer les fichiers aussi rapidement que possible.
-  Extraire les propriétés de fichier en paginant les objets StorageFile dans l’application. La pagination des fichiers réduit la mémoire utilisée par l’application et améliore sa réactivité perçue.

### <a name="creating-the-query"></a>Création de la requête 
Pour créer la requête, nous utilisons un objet QueryOptions pour spécifier que l’application ne s'intéresse à l'énumération que de certains types de fichiers d'images et pour filtrer les fichiers protégés par la Protection des informations Windows (System.Security.EncryptionOwners). 

Il est important de définir les propriétés auxquelles l’application va accéder à l’aide de [QueryOptions.SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch). Si l’application accède à une propriété qui n’est pas prérécupérée, cela peut entraîner une altération des performances.

Le paramètre [IndexerOption.OnlyUseIndexerAndOptimzeForIndexedProperties](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.IndexerOption) indique au système de renvoyer les résultats aussi rapidement que possible, mais d'inclure uniquement les propriétés spécifiées dans [SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch). 

### <a name="paging-in-the-results"></a>Pagination dans les résultats 
Les utilisateurs peuvent avoir des milliers ou des millions de fichiers dans leur bibliothèque d’images, donc l’appel de [GetFilesAsync](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) surchargerait leur ordinateur car il crée un StorageFile pour chaque image. Cela peut être résolu en créant un nombre fixe de StorageFiles en même temps, en les traitant dans l’interface utilisateur, ce qui libère la mémoire. 

Dans notre exemple, nous le faisons à l’aide de [StorageFileQueryResult.GetFilesAsync(UInt32 StartIndex, UInt32 maxNumberOfItems)](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) pour extraire uniquement 100 fichiers à la fois. L’application traite alors les fichiers et autorise le système d’exploitation à libérer ensuite cette mémoire. Cette technique plafonne la mémoire maximale de l’application et garantit que le système reste réactif. Bien entendu, vous devez ajuster le nombre de fichiers retournés pour votre scénario, mais pour garantir une expérience réactive pour tous les utilisateurs, il est recommandé de ne pas extraire plus de 500 fichiers en même temps.


**Exemple**  
```csharp
StorageFolder folderToEnumerate = KnownFolders.PicturesLibrary; 
// Check if the folder is indexed before doing anything. 
IndexedState folderIndexedState = await folderToEnumerate.GetIndexedStateAsync(); 
if (folderIndexedState == IndexedState.NotIndexed || folderIndexedState == IndexedState.Unknown) 
{ 
    // Only possible in indexed directories.  
    return; 
} 
 
QueryOptions picturesQuery = new QueryOptions() 
{ 
    FolderDepth = FolderDepth.Deep, 
    // Filter out all files that have WIP enabled
    ApplicationSearchFilter = "System.Security.EncryptionOwners:[]", 
    IndexerOption = IndexerOption.OnlyUseIndexerAndOptimizeForIndexedProperties 
}; 

picturesQuery.FileTypeFilter.Add(".jpg"); 
string[] otherProperties = new string[] 
{ 
    SystemProperties.GPS.LatitudeDecimal, 
    SystemProperties.GPS.LongitudeDecimal 
}; 
 
picturesQuery.SetPropertyPrefetch(PropertyPrefetchOptions.BasicProperties | PropertyPrefetchOptions.ImageProperties, 
                                    otherProperties); 
SortEntry sortOrder = new SortEntry() 
{ 
    AscendingOrder = true, 
    PropertyName = "System.FileName" // FileName property is used as an example. Any property can be used here.  
}; 
picturesQuery.SortOrder.Add(sortOrder); 
 
// Create the query and get the results 
uint index = 0; 
const uint stepSize = 100; 
if (!folderToEnumerate.AreQueryOptionsSupported(picturesQuery)) 
{ 
    log("Querying for a sort order is not supported in this location"); 
    picturesQuery.SortOrder.Clear(); 
} 
StorageFileQueryResult queryResult = folderToEnumerate.CreateFileQueryWithOptions(picturesQuery); 
IReadOnlyList<StorageFile> images = await queryResult.GetFilesAsync(index, stepSize); 
while (images.Count != 0 || index < 10000) 
{ 
    foreach (StorageFile file in images) 
    { 
        // With the OnlyUseIndexerAndOptimizeForIndexedProperties set, this won't  
        // be async. It will run synchronously. 
        var imageProps = await file.Properties.GetImagePropertiesAsync(); 
 
        // Build the UI 
        log(String.Format("{0} at {1}, {2}", 
                    file.Path, 
                    imageProps.Latitude, 
                    imageProps.Longitude)); 
    } 
    index += stepSize; 
    images = await queryResult.GetFilesAsync(index, stepSize); 
} 
```

### <a name="results"></a>Résultats 
Les fichiers StorageFile contiennent uniquement les propriétés demandées, mais sont renvoyés 10 fois plus rapidement par rapport aux autres IndexerOptions. L’application peut toujours demander l’accès aux propriétés non déjà incluses dans la requête, mais cela implique une baisse des performances pour ouvrir le fichier et extraire ces propriétés.  

## <a name="adding-folders-to-libraries"></a>Ajout de dossiers à des bibliothèques 
Les applications peuvent demander à l’utilisateur d'ajouter l’emplacement à l’index à l’aide de [StorageLibrary.RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibrary.RequestAddFolderAsync). Une fois que l’emplacement est inclus, il sera automatiquement indexé et les applications peuvent utiliser cette technique pour énumérer les fichiers.
 
## <a name="see-also"></a>Voir également
[Référence de l’API QueryOptions](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions)  
[Énumérer et interroger des fichiers et dossiers](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)  
[Autorisations d’accès aux fichiers](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)  
 
 
