---
ms.assetid: 1AE29512-7A7D-4179-ADAC-F02819AC2C39
Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos
Ajoutez les dossiers existants de musique, images ou vidéos dans les bibliothèques correspondantes. Vous pouvez également supprimer des dossiers de bibliothèques, obtenir la liste des dossiers d’une bibliothèque et découvrir des photos, de la musique et des vidéos.
---

# Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Ajoutez les dossiers existants de musique, images ou vidéos dans les bibliothèques correspondantes. Vous pouvez également supprimer des dossiers de bibliothèques, obtenir la liste des dossiers d’une bibliothèque, et découvrir des photos, de la musique et des vidéos.

Une bibliothèque est une collection virtuelle de dossiers, qui comprend un dossier connu par défaut, ainsi que d’autres dossiers que l’utilisateur a ajouté à la bibliothèque à l’aide de votre application ou d’une des applications intégrées. Par exemple, la bibliothèque d’images inclut le dossier connu d’images par défaut. L’utilisateur peut ajouter ou supprimer des dossiers dans la bibliothèque d’images à l’aide de votre application ou de l’application Photos intégrée.

## Conditions préalables


-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Autorisations d’accès à l’emplacement**

    Dans Visual Studio, ouvrez le fichier manifeste de l’application dans le concepteur du manifeste. Dans la page **Fonctionnalités**, sélectionnez les bibliothèques gérées par votre application.

    -   **Médiathèque**
    -   **Bibliothèque d’images**
    -   **Vidéothèque**

    Pour en savoir plus, voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## Obtenir une référence à une bibliothèque


**Remarque** N’oubliez pas de déclarer la fonctionnalité appropriée.
 

Pour obtenir une référence à la bibliothèque Musique, Images ou Vidéo de l’utilisateur, appelez la méthode [**StorageLibrary.GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/dn251725). Indiquez la valeur correspondante de l’énumération [**KnownLibraryId**](https://msdn.microsoft.com/library/windows/apps/dn298399).

-   [**KnownLibraryId.Music**](https://msdn.microsoft.com/library/windows/apps/br227155)
-   [**KnownLibraryId.Pictures**](https://msdn.microsoft.com/library/windows/apps/br227156)
-   [**KnownLibraryId.Videos**](https://msdn.microsoft.com/library/windows/apps/br227159)

```CSharp
    var myPictures = await Windows.Storage.StorageLibrary.GetLibraryAsync
        (Windows.Storage.KnownLibraryId.Pictures);
```

## Obtenir la liste des dossiers d’une bibliothèque


Pour obtenir la liste des dossiers d’une bibliothèque, obtenez la valeur de la propriété [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724).

```CSharp
    using Windows.Foundation.Collections;

    // ...
            
    IObservableVector<Windows.Storage.StorageFolder> myPictureFolders = myPictures.Folders;
```

## Obtenir le dossier contenu dans une bibliothèque où les nouveaux fichiers sont enregistrés par défaut


Pour obtenir le dossier d’une bibliothèque où les nouveaux fichiers sont enregistrés par défaut, obtenez la valeur de la propriété [**StorageLibrary.SaveFolder**](https://msdn.microsoft.com/library/windows/apps/dn251728).

```CSharp
    Windows.Storage.StorageFolder savePicturesFolder = myPictures.SaveFolder;
```

## Ajouter un dossier existant à une bibliothèque


Pour ajouter un dossier à une bibliothèque, vous appelez la méthode [**StorageLibrary.RequestAddFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251726). En prenant l’exemple de la bibliothèque Images, l’appel de cette méthode entraîne l’affichage d’un sélecteur de dossiers avec un bouton **Ajouter ce dossier à Images**. Si l’utilisateur sélectionne un dossier, celui-ci reste à son emplacement d’origine sur le disque et devient un élément dans la propriété [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) (et dans l’application Photos intégrée), mais le dossier n’apparaît pas en tant qu’enfant du dossier Images dans l’Explorateur de fichiers.


```CSharp
    Windows.Storage.StorageFolder newFolder = await myPictures.RequestAddFolderAsync();
```

## Supprimer un dossier d’une bibliothèque


Pour supprimer un dossier d’une bibliothèque, appelez la méthode [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727) et spécifiez le dossier à supprimer. Vous pouvez utiliser [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) et un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) (ou similaire) pour permettre à l’utilisateur de sélectionner un dossier à supprimer.

Lorsque vous appelez la méthode [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727), l’utilisateur voit une boîte de dialogue de confirmation indiquant que le dossier n’apparaîtra plus dans le dossier Images, mais ne sera pas supprimé. Cela signifie que le dossier reste dans son emplacement d’origine sur le disque, est supprimé de la propriété [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) et n’est plus inclus dans l’application Photos intégrée.

L’exemple suivant suppose que l’utilisateur a sélectionné le dossier à supprimer d’un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) nommé **lvPictureFolders**.


```CSharp
    bool result = await myPictures.RequestRemoveFolderAsync(folder);
```

## Recevoir des notifications de modifications apportées à la liste des dossiers d’une bibliothèque


Pour être averti des modifications apportées à la liste des dossiers d’une bibliothèque, inscrivez un gestionnaire pour l’événement [**StorageLibrary.DefinitionChanged**](https://msdn.microsoft.com/library/windows/apps/dn251723) de la bibliothèque.


```CSharp
    myPictures.DefinitionChanged += MyPictures_DefinitionChanged;
    // ...

void HandleDefinitionChanged(Windows.Storage.StorageLibrary sender, object args)
{
    // ...
}
```

## Dossiers de bibliothèque multimédia


Un appareil propose cinq emplacements prédéfinis aux utilisateurs et aux applications pour stocker des fichiers multimédias. Les applications intégrées stockent à la fois les médias créés par l’utilisateur et les médias téléchargés à ces emplacements.

Ces emplacements sont :

-   dossier **Images**. Contient des images.

    -   Dossier **Pellicule**. Contient les photos et vidéos de l’appareil photo intégré.

    -   Dossier **Images enregistrées**. Contient les images que l’utilisateur a enregistrées à partir d’autres applications.

-   Dossier **Musique**. Contient des chansons, des podcasts et des livres audio.

-   Dossier **Vidéo**. Contient des vidéos.

Les utilisateurs ou applications peuvent également stocker des fichiers multimédias en dehors des dossiers de bibliothèque multimédia sur la carte SD. Pour trouver un fichier multimédia de manière fiable sur la carte SD, analysez le contenu de la carte SD ou demandez à l’utilisateur de localiser le fichier à l’aide d’un sélecteur de fichiers. Pour plus d’informations, voir [Accéder à la carte SD](access-the-sd-card.md).

## Interrogation des bibliothèques multimédias


### Les résultats de requête incluent à la fois le stockage interne et amovible

Par défaut, les utilisateurs peuvent choisir de stocker les fichiers sur la carte SD en option. Les applications, en revanche, peuvent choisir de ne pas autoriser le stockage des fichiers sur la carte SD. Par conséquent, les bibliothèques multimédias peuvent se partager entre le stockage interne de l’appareil et la carte SD.

Il n’est pas nécessaire d’écrire d’autre code pour gérer cette possibilité. Les méthodes de l’espace de noms [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) qui interrogent les dossiers connus combinent en toute transparence les résultats de requête issus de ces deux emplacements. De même, vous n’avez pas à spécifier la fonctionnalité **removableStorage** dans le fichier manifeste de l’application pour obtenir ces résultats combinés.

Examinons l’état du stockage de l’appareil illustré dans l’image suivante :

![Images sur le téléphone et la carte SD](images/phone-media-locations.png)

Si vous interrogez le contenu de la bibliothèque d’images en appelant `await KnownFolders.PicturesLibrary.GetFilesAsync()`, les résultats incluent à la fois internalPic.jpg et SDPic.jpg.

### Requêtes profondes

Utilisez les requêtes profondes pour énumérer rapidement tout le contenu d’une bibliothèque multimédia.

Les requêtes profondes retournent uniquement les fichiers du type de média spécifié. Par exemple, si vous interrogez la médiathèque avec une requête profonde, les résultats obtenus n’incluent pas les fichiers image trouvés dans le dossier Musique.

Sur les périphériques où l’appareil photo enregistre à la fois une image basse résolution et une image haute résolution de chaque photo, les requêtes profondes retournent uniquement l’image basse résolution.

Les dossiers Pellicule et Images enregistrées ne prennent pas en charge les requêtes profondes.

Les requêtes profondes suivantes sont disponibles :

**Bibliothèque d’images**

-   `GetFilesAsync(CommonFileQuery.OrderByDate)`

**Médiathèque**

-   `GetFilesAsync(CommonFileQuery.OrderByName)`
-   `GetFoldersAsync(CommonFolderQuery.GroupByArtist)`
-   `GetFoldersAysnc(CommonFolderQuery.GroupByAlbum)`
-   `GetFoldersAysnc(CommonFolderQuery.GroupByAlbumArtist)`
-   `GetFoldersAsync(CommonFolderQuery.GroupByGenre)`

**Vidéothèque**

-   `GetFilesAsync(CommonFileQuery.OrderByDate)`

### Requêtes plates

Pour obtenir la liste complète de tous les fichiers et dossiers inclus dans une bibliothèque, appelez `GetFilesAsync(CommonFileQuery.DefaultQuery)`. Cette méthode retourne tous les fichiers inclus dans la bibliothèque, quel que soit leur type. Il s’agit d’une requête superficielle, donc vous devez énumérer le contenu des sous-dossiers de manière récursive si l’utilisateur a créé des sous-dossiers dans la bibliothèque.

Utilisez des requêtes plates pour retourner les fichiers multimédias dont le type n’est pas reconnu par les requêtes intégrées, ou pour retourner tous les fichiers inclus dans une bibliothèque, notamment les fichiers qui ne sont pas du type spécifié. Par exemple, si vous interrogez la médiathèque avec une requête plate, les résultats obtenus incluent tous les fichiers image trouvés par la requête dans le dossier Musique.

### Exemple de requêtes

Supposons que l’appareil et sa carte SD optionnelle contiennent les dossiers et fichiers représentés dans l’image suivante :

![Fichiers activés ](images/phone-media-queries.png)

Voici quelques exemples de requêtes et les résultats qu’elles renvoient.

| Requête | Résultats |
|--------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| KnownFolders.PicturesLibrary.GetItemsAsync();  | - Dossier Pellicule du stockage interne <br>- Dossier Pellicule de la carte SD <br>- Dossier Images enregistrées du stockage interne <br>- Dossier Images enregistrées de la carte SD <br><br>Il s’agit d’une requête plate, donc seuls les enfants immédiats du dossier Images sont renvoyés. |
| KnownFolders.PicturesLibrary.GetFilesAsync();  | Aucun résultat. <br><br>Il s’agit d’une requête plate et le dossier Images ne contient pas de fichiers enfants immédiats. |
| KnownFolders.PicturesLibrary.GetFilesAsync(CommonFileQuery.OrderByDate); | - Fichier 4-3-2012.jpg de la carte SD <br>- Fichier 1-1-2014.jpg du stockage interne <br>- Fichier 1-2-2014.jpg du stockage interne <br>- Fichier 1-6-2014.jpg de la carte SD <br><br>Il s’agit d’une requête profonde, donc le contenu du dossier Images et de ses dossiers enfants est renvoyé. |
| KnownFolders.CameraRoll.GetFilesAsync(); | - Fichier 1-1-2014.jpg du stockage interne <br>- Fichier 4-3-2012.jpg de la carte SD <br><br>Il s’agit d’une requête plate. L’ordre des résultats n’est pas garanti. |

 
## Fonctionnalités et types de fichiers des bibliothèques multimédias


Voici les fonctionnalités que vous pouvez spécifier dans le fichier manifeste de l’application pour accéder aux fichiers multimédias de votre application.

-   **Musique**. Spécifiez la fonctionnalité **Music Library** dans le fichier manifeste de l’application pour permettre à votre application de voir les types de fichiers suivants et d’y accéder :

    -   .qcp
    -   .wav
    -   .mp3
    -   .m4r
    -   .m4a
    -   .aac
    -   .amr
    -   .wma
    -   .3g2
    -   .3gp
    -   .mp4
    -   .wm
    -   .asf
    -   .3gpp
    -   .3gp2
    -   .mpa
    -   .adt
    -   .adts
    -   .pya
-   **Photos**. Spécifiez la fonctionnalité **Pictures Library** dans le fichier manifeste de l’application pour permettre à votre application de voir les types de fichiers suivants et d’y accéder :

    -   .jpeg
    -   .jpe
    -   .jpg
    -   .gif
    -   .tiff
    -   .tif
    -   .png
    -   .bmp
    -   .wdp
    -   .jxr
    -   .hdp
-   **Vidéos**. Spécifiez la fonctionnalité **Video Library** dans le fichier manifeste de l’application pour permettre à votre application de voir les types de fichiers suivants et d’y accéder :

    -   .wm
    -   .m4v
    -   .wmv
    -   .asf
    -   .mov
    -   .mp4
    -   .3g2
    -   .3gp
    -   .mp4v
    -   .avi
    -   .pyv
    -   .3gpp
    -   .3gp2

## Utilisation de photos


Sur les périphériques où l’appareil photo enregistre à la fois une image basse résolution et une image haute résolution de chaque photo, les requêtes profondes retournent uniquement l’image basse résolution.

Les dossiers Pellicule et Images enregistrées ne prennent pas en charge les requêtes profondes.

**Ouverture d’une photo dans l’application qui l’a capturée**

Si vous voulez laisser l’utilisateur rouvrir une photo dans l’application qui l’a capturée, vous pouvez enregistrer le **CreatorAppId** avec les métadonnées de la photo en utilisant du code similaire à celui de l’exemple suivant. Dans cet exemple, **testPhoto** est un [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

```CSharp
  IDictionary<string, object> propertiesToSave = new Dictionary<string, object>();

  propertiesToSave.Add("System.CreatorOpenWithUIOptions", 1);
  propertiesToSave.Add("System.CreatorAppId", appId);
 
  testPhoto.Properties.SavePropertiesAsync(propertiesToSave).AsyncWait();   
```

## Utilisation de méthodes de flux pour ajouter un fichier à une bibliothèque multimédia


Quand vous accédez à une bibliothèque multimédia à l’aide d’un dossier connu comme **KnownFolders.PictureLibrary** et que vous utilisez des méthodes de flux pour y ajouter un fichier, veillez à fermer tous les flux que votre code ouvre. Sinon, ces méthodes ne parviennent pas à ajouter le fichier à la bibliothèque multimédia comme prévu car au moins un flux possède toujours un handle vers le fichier.

Par exemple, quand vous exécutez le code suivant, le fichier n’est pas ajouté à la bibliothèque multimédia. Dans la ligne de code, `using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))`, les méthodes **OpenAsync** et **GetOutputStreamAt** ouvrent toutes les deux un flux. En revanche, seul le flux ouvert par la méthode **GetOutputStreamAt** est supprimé à l’issue de l’instruction **using**. L’autre flux reste ouvert et empêche l’enregistrement du fichier.

```CSharp
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

```CSharp
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

 

 






<!--HONumber=Mar16_HO1-->


