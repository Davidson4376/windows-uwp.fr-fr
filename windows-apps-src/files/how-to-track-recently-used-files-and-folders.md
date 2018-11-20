---
author: laurenhughes
ms.assetid: BF929A68-9C82-4866-BC13-A32B3A550005
title: Suivre les fichiers et dossiers récemment utilisés
description: Effectuez le suivi des fichiers auxquels l’utilisateur accède fréquemment en les ajoutant à la liste des fichiers utilisés récemment de votre application.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 12b8a6462f6cc39ba85cddfaa7a92212955a79f5
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7282943"
---
# <a name="track-recently-used-files-and-folders"></a>Suivre les fichiers et dossiers récemment utilisés

**API importantes**

- [**MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458)
- [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369)

Effectuez le suivi des fichiers auxquels l’utilisateur accède fréquemment en les ajoutant à la liste des éléments utilisés récemment de votre application. La plateforme gère les éléments récents pour vous, en les triant selon le critère du dernier accès, et en supprimant l’élément le plus ancien quand la limite de 25 éléments est atteinte. Toutes les applications ont leurs propres éléments récents.

Les éléments récents de votre application sont représentés par la classe [**StorageItemMostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207475), que vous pouvez obtenir à partir de la propriété [**StorageApplicationPermissions.MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458) statique. Les éléments récents sont stockés en tant qu’objets [**IStorageItem**](https://msdn.microsoft.com/library/windows/apps/br227129), ce qui signifie que des objets [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) (qui représentent des fichiers) et [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) (qui représentent des dossiers) peuvent être ajoutés aux éléments récents.

> [!NOTE]
> Consultez également l’[exemple de sélecteur de fichiers](http://go.microsoft.com/fwlink/p/?linkid=619994) et l’[exemple d’accès aux fichiers](http://go.microsoft.com/fwlink/p/?linkid=619995).

 

## <a name="prerequisites"></a>Prérequis

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Autorisations d’accès à l’emplacement**

    Voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

-   [Ouvrir des fichiers et dossiers à l’aide d’un sélecteur](quickstart-using-file-and-folder-pickers.md)

    Les fichiers sélectionnés sont souvent ceux auxquels les utilisateurs reviennent inlassablement.

 ## <a name="add-a-picked-file-to-the-mru"></a>Ajouter un fichier sélectionné au éléments récents

-   Les fichiers que l'utilisateur sélectionne sont souvent des fichiers auxquels il revient à plusieurs reprises. Par conséquent, songez à ajouter des fichiers sélectionnés à la liste d’éléments récents de votre application dès leur sélection. Voici comment procéder.

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    var mru = Windows.Storage.AccessCache.StorageApplicationPermissions.MostRecentlyUsedList;
    string mruToken = mru.Add(file, "profile pic");
    ```

    [**StorageItemMostRecentlyUsedList.Add**](https://msdn.microsoft.com/library/windows/apps/br207476) est surchargé. Dans l’exemple, nous utilisons [**Add(IStorageItem, String)**](https://msdn.microsoft.com/library/windows/apps/br207481) afin de pouvoir associer des métadonnées au fichier. La définition des métadonnées vous permet d’enregistrer la finalité de l’élément, par exemple, «image du profil». Vous pouvez également ajouter le fichier à la liste Utilisés récemment sans métadonnées en appelant [**Add(IStorageItem)**](https://msdn.microsoft.com/library/windows/apps/br207480). Chaque fois que vous ajoutez un élément aux éléments récents, la méthode retourne une chaîne d’identification unique, ou jeton, qui est utilisée pour récupérer l’élément.

> [!TIP]
> Le jeton étant nécessaire pour récupérer un élément de la liste des fichiers utilisés récemment, il convient de le conserver quelque part. Pour plus d’informations sur les données d’application, consultez [Gestion des données d’application](https://msdn.microsoft.com/library/windows/apps/hh465109).

## <a name="use-a-token-to-retrieve-an-item-from-the-mru"></a>Utiliser un jeton pour récupérer un élément de la liste des éléments récents

Utilisez la méthode de récupération la plus appropriée pour l’élément à récupérer.

-   Récupérer un fichier en tant que [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) en utilisant [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br207486).
-   Récupérer un dossier en tant que [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) en utilisant [**GetFolderAsync**](https://msdn.microsoft.com/library/windows/apps/br207489).
-   Récupérer un [**IStorageItem**](https://msdn.microsoft.com/library/windows/apps/br227129) générique représentant un fichier ou un dossier en utilisant [**GetItemAsync**](https://msdn.microsoft.com/library/windows/apps/br207492).

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

La [**AccessListEntryView**](https://msdn.microsoft.com/library/windows/apps/br227349) vous permet d’itérer des entrées dans la liste Utilisés récemment. Ces entrées sont des structures [**AccessListEntry**](https://msdn.microsoft.com/library/windows/apps/br227348) contenant le jeton et les métadonnées d’un élément.

## <a name="removing-items-from-the-mru-when-its-full"></a>Suppression d’éléments de la liste des éléments récents quand celle-ci est pleine

Quand cette limite de 25 éléments est atteinte et que vous essayez d’ajouter un nouvel élément, l’élément auquel l’utilisateur a accédé dans le passé le plus lointain est automatiquement supprimé. Par conséquent, vous devez jamais supprimer un élément avant d'en ajouter un nouveau.

## <a name="future-access-list"></a>Liste d'accès futurs

Comme pour les éléments récents, votre application dispose d’une liste d'accès futurs. En sélectionnant des fichiers et dossiers, votre utilisateur autorise votre application à accéder à des éléments qui pourraient ne pas être accessibles autrement. En ajoutant ces éléments à votre liste d'accès futurs, vous conservez cette autorisation au cas où votre application devrait accéder à ces éléments ultérieurement. La liste d’accès futurs de votre application est représentée par la classe [**StorageItemAccessList**](https://msdn.microsoft.com/library/windows/apps/br207459) que vous obtenez à partir de la propriété [**StorageApplicationPermissions.FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) statique.

Quand un utilisateur sélectionne un élément, songez à ajouter celui-ci à votre liste d'accès futurs ainsi qu’aux éléments récents.

-   La [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) peut contenir jusqu’à 1000éléments. N'oubliez pas que ce nombre comprend les dossiers et les fichiers.
-   La plateforme ne supprime jamais d’éléments de la [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) à votre place. Lorsque vous avez atteint la limite des 1000éléments, vous ne pouvez plus en ajouter sans libérer préalablement de l’espace avec la méthode [**Remove**](https://msdn.microsoft.com/library/windows/apps/br207497).
