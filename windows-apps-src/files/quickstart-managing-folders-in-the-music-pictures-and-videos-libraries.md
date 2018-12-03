---
ms.assetid: 1AE29512-7A7D-4179-ADAC-F02819AC2C39
title: Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos
description: Ajoutez les dossiers existants de musique, images ou vidéos dans les bibliothèques correspondantes. Vous pouvez également supprimer des dossiers de bibliothèques, obtenir la liste des dossiers d’une bibliothèque et découvrir des photos, de la musique et des vidéos.
ms.date: 06/18/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e04170fb8952ecd5802b6190816d44012f56d8a
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458952"
---
# <a name="files-and-folders-in-the-music-pictures-and-videos-libraries"></a>Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos

Ajoutez les dossiers existants de musique, images ou vidéos dans les bibliothèques correspondantes. Vous pouvez également supprimer des dossiers de bibliothèques, obtenir la liste des dossiers d’une bibliothèque, et découvrir des photos, de la musique et des vidéos.

Une bibliothèque est une collection virtuelle de dossiers, qui comprend un dossier connu par défaut, ainsi que d’autres dossiers que l’utilisateur a ajouté à la bibliothèque à l’aide de votre application ou d’une des applications intégrées. Par exemple, la bibliothèque d’images inclut le dossier connu d’images par défaut. L’utilisateur peut ajouter ou supprimer des dossiers dans la bibliothèque d’images à l’aide de votre application ou de l’application Photos intégrée.

## <a name="prerequisites"></a>Conditions préalables


-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Autorisations d’accès à l’emplacement**

    Dans Visual Studio, ouvrez le fichier manifeste de l’application dans le concepteur du manifeste. Dans la page **Fonctionnalités**, sélectionnez les bibliothèques gérées par votre application.

    -   **Médiathèque**
    -   **Bibliothèque d’images**
    -   **Vidéothèque**

    Pour en savoir plus, voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## <a name="get-a-reference-to-a-library"></a>Obtenir une référence à une bibliothèque

> [!NOTE]
> N’oubliez pas de déclarer la fonctionnalité appropriée. Pour plus d’informations, voir [Déclarations des fonctionnalités d’application](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).
 

Pour obtenir une référence à la bibliothèque Musique, Images ou Vidéo de l’utilisateur, appelez la méthode [**StorageLibrary.GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/dn251725). Indiquez la valeur correspondante de l’énumération [**KnownLibraryId**](https://msdn.microsoft.com/library/windows/apps/dn298399).

-   [**KnownLibraryId.Music**](https://msdn.microsoft.com/library/windows/apps/br227155)
-   [**KnownLibraryId.Pictures**](https://msdn.microsoft.com/library/windows/apps/br227156)
-   [**KnownLibraryId.Videos**](https://msdn.microsoft.com/library/windows/apps/br227159)

```cs
var myPictures = await Windows.Storage.StorageLibrary.GetLibraryAsync(Windows.Storage.KnownLibraryId.Pictures);
```

## <a name="get-the-list-of-folders-in-a-library"></a>Obtenir la liste des dossiers d’une bibliothèque


Pour obtenir la liste des dossiers d’une bibliothèque, obtenez la valeur de la propriété [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724).

```cs
using Windows.Foundation.Collections;
IObservableVector<Windows.Storage.StorageFolder> myPictureFolders = myPictures.Folders;
```

## <a name="get-the-folder-in-a-library-where-new-files-are-saved-by-default"></a>Obtenir le dossier contenu dans une bibliothèque où les nouveaux fichiers sont enregistrés par défaut


Pour obtenir le dossier d’une bibliothèque où les nouveaux fichiers sont enregistrés par défaut, obtenez la valeur de la propriété [**StorageLibrary.SaveFolder**](https://msdn.microsoft.com/library/windows/apps/dn251728).

```cs
Windows.Storage.StorageFolder savePicturesFolder = myPictures.SaveFolder;
```

## <a name="add-an-existing-folder-to-a-library"></a>Ajouter un dossier existant à une bibliothèque

Pour ajouter un dossier à une bibliothèque, vous appelez la méthode [**StorageLibrary.RequestAddFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251726). En prenant l’exemple de la bibliothèque Images, l’appel de cette méthode entraîne l’affichage d’un sélecteur de dossiers avec un bouton **Ajouter ce dossier à Images**. Si l’utilisateur sélectionne un dossier, celui-ci reste à son emplacement d’origine sur le disque et devient un élément dans la propriété [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) (et dans l’application Photos intégrée), mais le dossier n’apparaît pas en tant qu’enfant du dossier Images dans l’Explorateur de fichiers.


```cs
Windows.Storage.StorageFolder newFolder = await myPictures.RequestAddFolderAsync();
```

## <a name="remove-a-folder-from-a-library"></a>Supprimer un dossier d’une bibliothèque

Pour supprimer un dossier d’une bibliothèque, appelez la méthode [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727) et spécifiez le dossier à supprimer. Vous pouvez utiliser [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) et un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) (ou similaire) pour permettre à l’utilisateur de sélectionner un dossier à supprimer.

Lorsque vous appelez la méthode [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727), l’utilisateur voit une boîte de dialogue de confirmation indiquant que le dossier n’apparaîtra plus dans le dossier Images, mais ne sera pas supprimé. Cela signifie que le dossier reste dans son emplacement d’origine sur le disque, est supprimé de la propriété [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) et n’est plus inclus dans l’application Photos intégrée.

L’exemple suivant suppose que l’utilisateur a sélectionné le dossier à supprimer d’un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) nommé **lvPictureFolders**.


```cs
bool result = await myPictures.RequestRemoveFolderAsync(folder);
```

## <a name="get-notified-of-changes-to-the-list-of-folders-in-a-library"></a>Recevoir des notifications de modifications apportées à la liste des dossiers d’une bibliothèque


Pour être averti des modifications apportées à la liste des dossiers d’une bibliothèque, inscrivez un gestionnaire pour l’événement [**StorageLibrary.DefinitionChanged**](https://msdn.microsoft.com/library/windows/apps/dn251723) de la bibliothèque.


```cs
myPictures.DefinitionChanged += MyPictures_DefinitionChanged;

void HandleDefinitionChanged(Windows.Storage.StorageLibrary sender, object args)
{
    // ...
}
```

## <a name="media-library-folders"></a>Dossiers de bibliothèque multimédia


Un appareil propose cinq emplacements prédéfinis aux utilisateurs et aux applications pour stocker des fichiers multimédias. Les applications intégrées stockent à la fois les médias créés par l’utilisateur et les médias téléchargés à ces emplacements.

Ces emplacements sont :

-   dossier **Images**. Contient des images.

    -   Dossier **Pellicule**. Contient les photos et vidéos de l’appareil photo intégré.

    -   Dossier **Images enregistrées**. Contient les images que l’utilisateur a enregistrées à partir d’autres applications.

-   Dossier **Musique**. Contient des chansons, des podcasts et des livres audio.

-   Dossier **Vidéo**. Contient des vidéos.

Les utilisateurs ou applications peuvent également stocker des fichiers multimédias en dehors des dossiers de bibliothèque multimédia sur la carte SD. Pour trouver un fichier multimédia de manière fiable sur la carte SD, analysez le contenu de la carte SD ou demandez à l’utilisateur de localiser le fichier à l’aide d’un sélecteur de fichiers. Pour plus d’informations, voir [Accéder à la carte SD](access-the-sd-card.md).

## <a name="querying-the-media-libraries"></a>Interrogation des bibliothèques multimédias

Pour obtenir une collection de fichiers, spécifiez la bibliothèque et le type des fichiers souhaités.

```cs
using Windows.Storage;
using Windows.Storage.Search;

private async void getSongs()
{
    QueryOptions queryOption = new QueryOptions
        (CommonFileQuery.OrderByTitle, new string[] { ".mp3", ".mp4", ".wma" });

    queryOption.FolderDepth = FolderDepth.Deep;

    Queue<IStorageFolder> folders = new Queue<IStorageFolder>();

    var files = await KnownFolders.MusicLibrary.CreateFileQueryWithOptions
      (queryOption).GetFilesAsync();

    foreach (var file in files)
    {
        // do something with the music files
    }
}
```

### <a name="query-results-include-both-internal-and-removable-storage"></a>Les résultats de requête incluent à la fois le stockage interne et amovible

Par défaut, les utilisateurs peuvent choisir de stocker les fichiers sur la carte SD en option. Les applications, en revanche, peuvent choisir de ne pas autoriser le stockage des fichiers sur la carte SD. Par conséquent, les bibliothèques multimédias peuvent se partager entre le stockage interne de l’appareil et la carte SD.

Il n’est pas nécessaire d’écrire d’autre code pour gérer cette possibilité. Les méthodes de l’espace de noms [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) qui interrogent les dossiers connus combinent en toute transparence les résultats de requête issus de ces deux emplacements. De même, vous n’avez pas à spécifier la fonctionnalité **removableStorage** dans le fichier manifeste de l’application pour obtenir ces résultats combinés.

Examinons l’état du stockage de l’appareil illustré dans l’image suivante:

![Images sur le téléphone et la carte SD](images/phone-media-locations.png)

Si vous interrogez le contenu de la bibliothèque d’images en appelant `await KnownFolders.PicturesLibrary.GetFilesAsync()`, les résultats incluent à la fois internalPic.jpg et SDPic.jpg.


## <a name="working-with-photos"></a>Utilisation de photos

Sur les périphériques où l’appareil photo enregistre à la fois une image basse résolution et une image haute résolution de chaque photo, les requêtes profondes retournent uniquement l’image basse résolution.

Les dossiers Pellicule et Images enregistrées ne prennent pas en charge les requêtes profondes.

**Ouverture d’une photo dans l’application qui l’a capturée**

Si vous voulez laisser l’utilisateur rouvrir une photo dans l’application qui l’a capturée, vous pouvez enregistrer le **CreatorAppId** avec les métadonnées de la photo en utilisant du code similaire à celui de l’exemple suivant. Dans cet exemple, **testPhoto** est un [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

```cs
IDictionary<string, object> propertiesToSave = new Dictionary<string, object>();

propertiesToSave.Add("System.CreatorOpenWithUIOptions", 1);
propertiesToSave.Add("System.CreatorAppId", appId);

testPhoto.Properties.SavePropertiesAsync(propertiesToSave).AsyncWait();   
```

## <a name="using-stream-methods-to-add-a-file-to-a-media-library"></a>Utilisation de méthodes de flux pour ajouter un fichier à une bibliothèque multimédia

Quand vous accédez à une bibliothèque multimédia à l’aide d’un dossier connu comme **KnownFolders.PictureLibrary** et que vous utilisez des méthodes de flux pour y ajouter un fichier, veillez à fermer tous les flux que votre code ouvre. Sinon, ces méthodes ne parviennent pas à ajouter le fichier à la bibliothèque multimédia comme prévu car au moins un flux possède toujours un handle vers le fichier.

Par exemple, quand vous exécutez le code suivant, le fichier n’est pas ajouté à la bibliothèque multimédia. Dans la ligne de code, `using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))`, les méthodes **OpenAsync** et **GetOutputStreamAt** ouvrent toutes les deux un flux. En revanche, seul le flux ouvert par la méthode **GetOutputStreamAt** est supprimé à l’issue de l’instruction **using**. L’autre flux reste ouvert et empêche l’enregistrement du fichier.

```cs
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");
using (var sourceStream = (await sourceFile.OpenReadAsync()).GetInputStreamAt(0))
{
    using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))
    {
        await RandomAccessStream.CopyAndCloseAsync(sourceStream, destinationStream);
    }
}
```

Pour bien utiliser des méthodes de flux pour ajouter un fichier à la bibliothèque multimédia, veillez à fermer tous les flux que votre code ouvre, comme illustré dans l’exemple suivant.

```cs
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");

using (var sourceStream = await sourceFile.OpenReadAsync())
{
    using (var sourceInputStream = sourceStream.GetInputStreamAt(0))
    {
        using (var destinationStream = await destinationFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            using (var destinationOutputStream = destinationStream.GetOutputStreamAt(0))
            {
                await RandomAccessStream.CopyAndCloseAsync(sourceInputStream, destinationStream);
            }
        }
    }
}
```
