---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Obtenir les propriétés du fichier
description: Obtenir les propriétés de &\#8212 ; au niveau du haut, de base et étendus &\#8212 ; pour un fichier représenté par un objet StorageFile objet.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 01eda8eefea7e1b3b1102ef154a019e1630e80c2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369316"
---
# <a name="get-file-properties"></a>Obtenir les propriétés du fichier

**API importantes**

-   [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)
-   [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.retrievepropertiesasync)

Obtenez les propriétés (de niveau supérieur, de base et étendues) d’un fichier représenté par un objet [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile).

> [!NOTE]
> Pour obtenir un exemple complet, consultez la [exemple d’accès de fichier](https://go.microsoft.com/fwlink/p/?linkid=619995).

## <a name="prerequisites"></a>Prérequis

-   **Comprendre la programmation asynchrone pour les applications de plateforme universelle Windows (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Autorisations d’accès à l’emplacement**

    Par exemple, le code de ces exemples nécessite la fonctionnalité **picturesLibrary**, mais votre emplacement peut avoir besoin d’une autre fonctionnalité, voire d’aucune. Pour en savoir plus, voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## <a name="getting-a-files-top-level-properties"></a>Obtention des propriétés de niveau supérieur d’un fichier

De nombreuses propriétés de fichier de haut niveau sont accessibles en tant que membres de la classe [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile). Ces propriétés incluent les attributs des fichiers, le type de contenu, la date de création, le nom d’affichage, le type de fichier, etc.

> [!NOTE]
> N’oubliez pas de déclarer la fonctionnalité **picturesLibrary**.

Cet exemple énumère tous les fichiers de la bibliothèque d’images, en accédant à quelques-unes des propriétés de niveau supérieur de chaque fichier.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get top-level file properties.
    fileProperties.AppendLine("File name: " + file.Name);
    fileProperties.AppendLine("File type: " + file.FileType);
}
```

## <a name="getting-a-files-basic-properties"></a>Obtention des propriétés de base d’un fichier

De nombreuses propriétés de fichier de base sont obtenues en appelant d’abord la méthode [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync). Cette méthode retourne un objet [**BasicProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties) qui définit les propriétés de taille de l’élément (fichier ou dossier), ainsi que la date de sa dernière modification.

Cet exemple énumère tous les fichiers de la bibliothèque d’images, en accédant à quelques-unes des propriétés de base de chacun d’eux.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get file's basic properties.
    Windows.Storage.FileProperties.BasicProperties basicProperties =
        await file.GetBasicPropertiesAsync();
    string fileSize = string.Format("{0:n0}", basicProperties.Size);
    fileProperties.AppendLine("File size: " + fileSize + " bytes");
    fileProperties.AppendLine("Date modified: " + basicProperties.DateModified);
}
 ```

## <a name="getting-a-files-extended-properties"></a>Obtention des propriétés étendues d’un fichier

Outre les propriétés de fichier de haut niveau et de base, il existe de nombreuses propriétés associées au contenu du fichier. Ces propriétés étendues sont accessibles en appelant la méthode [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync). (Un [ **BasicProperties** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties) objet est obtenu en appelant le [ **StorageFile.Properties** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties) propriété.) Tandis que les propriétés du fichier de niveau supérieur et de base sont accessibles en tant que propriétés d’une classe,[**StorageFile** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) et **BasicProperties**, respectivement, sont des propriétés étendues obtenu en transmettant un [IEnumerable](https://go.microsoft.com/fwlink/p/?LinkID=313091) collection de [chaîne](https://go.microsoft.com/fwlink/p/?LinkID=325032) objets représentant les noms des propriétés qui doivent être récupérés à le  **BasicProperties.RetrievePropertiesAsync** (méthode). Cette méthode retourne ensuite une collection [IDictionary](https://go.microsoft.com/fwlink/p/?LinkId=325238). Chaque propriété étendue est ensuite récupérée à partir de la collection par nom ou par index.

Cet exemple énumère tous les fichiers de la bibliothèque d’images, spécifie les noms des propriétés souhaitées (**DataAccessed** et **FileOwner**) dans un objet [List](https://go.microsoft.com/fwlink/p/?LinkID=325246), passe cet objet [List](https://go.microsoft.com/fwlink/p/?LinkID=325246) à [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) pour récupérer ces propriétés, puis extrait celles-ci par nom de l’objet [IDictionary](https://go.microsoft.com/fwlink/p/?LinkId=325238) retourné.

Consultez [Propriétés principales de Windows](https://docs.microsoft.com/windows/desktop/properties/core-bumper) pour obtenir la liste complète des propriétés étendues d’un fichier.

```csharp
const string dateAccessedProperty = "System.DateAccessed";
const string fileOwnerProperty = "System.FileOwner";

// Enumerate all files in the Pictures library.
var folder = KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Define property names to be retrieved.
    var propertyNames = new List<string>();
    propertyNames.Add(dateAccessedProperty);
    propertyNames.Add(fileOwnerProperty);

    // Get extended properties.
    IDictionary<string, object> extraProperties =
        await file.Properties.RetrievePropertiesAsync(propertyNames);

    // Get date-accessed property.
    var propValue = extraProperties[dateAccessedProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("Date accessed: " + propValue);
    }

    // Get file-owner property.
    propValue = extraProperties[fileOwnerProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("File owner: " + propValue);
    }
}
```

 

 
