---
ms.assetid: BF929A68-9C82-4866-BC13-A32B3A550005
title: Suivre les fichiers et dossiers récemment utilisés
description: Effectuez le suivi des fichiers auxquels l’utilisateur accède fréquemment en les ajoutant à la liste Utilisés récemment de votre application.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 97ad2485abab0bd4733699bc4ffcf29e17a22844
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369444"
---
# <a name="track-recently-used-files-and-folders"></a>Suivre les fichiers et dossiers récemment utilisés

**API importantes**

- [**MostRecentlyUsedList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist)
- [**FileOpenPicker**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker)

Effectuez le suivi des fichiers auxquels l’utilisateur accède fréquemment en les ajoutant à la liste Utilisés récemment de votre application. La plateforme gère les éléments récents pour vous, en les triant selon le critère du dernier accès, et en supprimant l’élément le plus ancien quand la limite de 25 éléments est atteinte. Toutes les applications ont leurs propres éléments utilisés récemment.

Les éléments récents de votre application sont représentés par la classe [**StorageItemMostRecentlyUsedList**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageItemMostRecentlyUsedList), que vous pouvez obtenir à partir de la propriété [**StorageApplicationPermissions.MostRecentlyUsedList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist) statique. Les éléments récents sont stockés en tant qu’objets [**IStorageItem**](https://docs.microsoft.com/uwp/api/Windows.Storage.IStorageItem), ce qui signifie que des objets [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) (qui représentent des fichiers) et [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) (qui représentent des dossiers) peuvent être ajoutés aux éléments récents.

> [!NOTE]
> Pour obtenir des exemples complets, reportez-vous à l’[exemple de sélecteur de fichiers](https://go.microsoft.com/fwlink/p/?linkid=619994) et à l’[exemple d’accès aux fichiers](https://go.microsoft.com/fwlink/p/?linkid=619995).

## <a name="prerequisites"></a>Conditions préalables

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Autorisations d’accès à l’emplacement**

    Voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

-   [Ouvrir des fichiers et des dossiers à l’aide d’un sélecteur](quickstart-using-file-and-folder-pickers.md)

    Les fichiers sélectionnés sont souvent ceux auxquels les utilisateurs reviennent inlassablement.

 ## <a name="add-a-picked-file-to-the-mru"></a>Ajouter un fichier sélectionné au éléments récents

-   Les fichiers que l'utilisateur sélectionne sont souvent des fichiers auxquels il revient à plusieurs reprises. Par conséquent, songez à ajouter des fichiers sélectionnés à la liste d’éléments récents de votre application dès leur sélection. Voici comment procéder.

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    var mru = Windows.Storage.AccessCache.StorageApplicationPermissions.MostRecentlyUsedList;
    string mruToken = mru.Add(file, "profile pic");
    ```

    [**StorageItemMostRecentlyUsedList.Add**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add) est surchargé. Dans l’exemple, nous utilisons [**Add(IStorageItem, String)** ](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add) afin de pouvoir associer des métadonnées au fichier. La définition des métadonnées vous permet d’enregistrer la finalité de l’élément, par exemple, « image du profil ». Vous pouvez également ajouter le fichier à la liste Utilisés récemment sans métadonnées en appelant [**Add(IStorageItem)** ](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add). Chaque fois que vous ajoutez un élément aux éléments récents, la méthode retourne une chaîne d’identification unique, ou jeton, qui est utilisée pour récupérer l’élément.

> [!TIP]
> Le jeton étant nécessaire pour récupérer un élément de la liste Utilisés récemment, il convient de le conserver quelque part. Pour plus d’informations sur les données d’application, consultez [Gestion des données d’application](https://docs.microsoft.com/previous-versions/windows/apps/hh465109(v=win.10)).

## <a name="use-a-token-to-retrieve-an-item-from-the-mru"></a>Utiliser un jeton pour récupérer un élément de la liste des éléments récents

Utilisez la méthode de récupération la plus appropriée pour l’élément à récupérer.

-   Récupérer un fichier en tant que [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) en utilisant [**GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getfileasync).
-   Récupérer un dossier en tant que [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) en utilisant [**GetFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getfolderasync).
-   Récupérer un [**IStorageItem**](https://docs.microsoft.com/uwp/api/Windows.Storage.IStorageItem) générique représentant un fichier ou un dossier en utilisant [**GetItemAsync**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getitemasync).

Voici comment récupérer le fichier que nous venons d'ajouter.

```cs
StorageFile retrievedFile = await mru.GetFileAsync(mruToken);
```

Voici comment itérer dans toutes les entrées pour obtenir des jetons, puis des éléments.

```cs
foreach (Windows.Storage.AccessCache.AccessListEntry entry in mru.Entries)
{
    string mruToken = entry.Token;
    string mruMetadata = entry.Metadata;
    Windows.Storage.IStorageItem item = await mru.GetItemAsync(mruToken);
    // The type of item will tell you whether it's a file or a folder.
}
```

La [**AccessListEntryView**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.AccessListEntryView) vous permet d’itérer des entrées dans la liste Utilisés récemment. Ces entrées sont des structures [**AccessListEntry**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.AccessListEntry) contenant le jeton et les métadonnées d’un élément.

## <a name="removing-items-from-the-mru-when-its-full"></a>Suppression d’éléments de la liste des éléments récents quand celle-ci est pleine

Quand cette limite de 25 éléments est atteinte et que vous essayez d’ajouter un nouvel élément, l’élément auquel l’utilisateur a accédé dans le passé le plus lointain est automatiquement supprimé. Par conséquent, vous devez jamais supprimer un élément avant d'en ajouter un nouveau.

## <a name="future-access-list"></a>Liste d'accès futurs

Comme pour les éléments récents, votre application dispose d’une liste d'accès futurs. En sélectionnant des fichiers et dossiers, votre utilisateur autorise votre application à accéder à des éléments qui pourraient ne pas être accessibles autrement. En ajoutant ces éléments à votre liste d'accès futurs, vous conservez cette autorisation au cas où votre application devrait accéder à ces éléments ultérieurement. La liste d’accès futurs de votre application est représentée par la classe [**StorageItemAccessList**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageItemAccessList) que vous obtenez à partir de la propriété [**StorageApplicationPermissions.FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) statique.

Quand un utilisateur sélectionne un élément, songez à ajouter celui-ci à votre liste d'accès futurs ainsi qu’aux éléments récents.

-   La [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) peut contenir jusqu’à 1 000 éléments. N'oubliez pas que ce nombre comprend les dossiers et les fichiers.
-   La plateforme ne supprime jamais d’éléments de la [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) à votre place. Lorsque vous avez atteint la limite des 1 000 éléments, vous ne pouvez plus en ajouter sans libérer préalablement de l’espace avec la méthode [**Remove**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.remove).
