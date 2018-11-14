---
author: laurenhughes
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Obtenir les propriétés du fichier
description: Obtenez les propriétés (de niveau supérieur, de base et étendues) d’un fichier représenté par un objet StorageFile.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8fc44300376efb5b56f390457e516f35a3ec4202
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6266194"
---
# <a name="get-file-properties"></a>Obtenir les propriétés du fichier



**API importantes**

-   [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737)
-   [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770652)

Obtenez les propriétés (de niveau supérieur, de base et étendues) d’un fichier représenté par un objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

> [!NOTE]
> Consultez également l’[exemple d’accès aux fichiers](http://go.microsoft.com/fwlink/p/?linkid=619995).

 


## <a name="prerequisites"></a>Prérequis

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Autorisations d’accès à l’emplacement**

    Par exemple, le code de ces exemples nécessite la fonctionnalité **picturesLibrary**, mais votre emplacement peut avoir besoin d’une autre fonctionnalité, voire d’aucune. Pour en savoir plus, voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## <a name="getting-a-files-top-level-properties"></a>Obtention des propriétés de niveau supérieur d’un fichier

De nombreuses propriétés de fichier de haut niveau sont accessibles en tant que membres de la classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). Ces propriétés incluent les attributs des fichiers, le type de contenu, la date de création, le nom d’affichage, le type de fichier, etc.

**Remarque**n’oubliez pas de déclarer la fonctionnalité **picturesLibrary** .

 

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

De nombreuses propriétés de fichier de base sont obtenues en appelant d’abord la méthode [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737). Cette méthode retourne un objet [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113) qui définit les propriétés de taille de l’élément (fichier ou dossier), ainsi que la date de sa dernière modification.

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

Outre les propriétés de fichier de haut niveau et de base, il existe de nombreuses propriétés associées au contenu du fichier. Ces propriétés étendues sont accessibles en appelant la méthode [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124). (Un objet [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113) est obtenu en appelant la propriété [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225).) Alors que les propriétés de fichier de niveau supérieur et de base sont accessibles en tant que propriétés d’une classe ([**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) et **BasicProperties**, respectivement), les propriétés étendues sont obtenues en passant une collection [IEnumerable](http://go.microsoft.com/fwlink/p/?LinkID=313091) d’objets [String](http://go.microsoft.com/fwlink/p/?LinkID=325032) qui représentent les noms des propriétés à récupérer à la méthode **BasicProperties.RetrievePropertiesAsync**. Cette méthode retourne ensuite une collection [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238). Chaque propriété étendue est ensuite récupérée à partir de la collection par nom ou par index.

Cet exemple énumère tous les fichiers de la bibliothèque d’images, spécifie les noms des propriétés souhaitées (**DataAccessed** et **FileOwner**) dans un objet [List](http://go.microsoft.com/fwlink/p/?LinkID=325246), passe cet objet [List](http://go.microsoft.com/fwlink/p/?LinkID=325246) à [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124) pour récupérer ces propriétés, puis extrait celles-ci par nom de l’objet [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238) retourné.

Consultez [Propriétés principales de Windows](https://msdn.microsoft.com/library/windows/desktop/mt805470) pour obtenir la liste complète des propriétés étendues d’un fichier.

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

 

 
