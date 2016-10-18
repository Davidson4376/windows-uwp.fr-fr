---
author: normesta
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: "Autorisations d’accès aux fichiers"
description: "Les applications peuvent accéder à certains emplacements du système de fichiers par défaut. Les applications peuvent également accéder à des emplacements supplémentaires par le biais du sélecteur de fichiers, ou en déclarant des fonctionnalités."
translationtype: Human Translation
ms.sourcegitcommit: ef8d0e7ad9063fa57a9db7c3cbdcb6846d3b1133
ms.openlocfilehash: e58cdce7f803cd15b66371e3b03c4405cbdeb3ff

---
# Autorisations d’accès aux fichiers

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Les applications peuvent accéder à certains emplacements du système de fichiers par défaut. Les applications peuvent également accéder à des emplacements supplémentaires par le biais du sélecteur de fichiers ou en déclarant des fonctionnalités.

## Emplacements accessibles à toutes les applications

Lorsque vous créez une application, vous pouvez accéder par défaut aux emplacements suivants du système de fichiers :

-   **Répertoire d’installation de l’application**. Dossier dans lequel votre application est installée sur le système de l’utilisateur.

    Il existe deux manières principales d’accéder à des fichiers et des dossiers dans le répertoire d’installation de votre application :

    1.  Vous pouvez récupérer une classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) qui représente le répertoire d’installation de votre application, comme suit :
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
        ```
        ```javascript
        var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
        ```

       Vous pouvez accéder aux fichiers et dossiers du répertoire à l’aide des méthodes [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230). Dans l’exemple, cette classe **StorageFolder** est stockée dans la variable `installDirectory`. Pour en savoir plus sur l’utilisation de votre package d’application et du répertoire d’installation, téléchargez l’[exemple d’informations de package d’application](http://go.microsoft.com/fwlink/p/?linkid=231526) pour Windows8.1 et réutilisez son code source dans votre application Windows10.

    2.  Vous pouvez récupérer un fichier directement à partir du répertoire d’installation de votre application en utilisant un URI d’application, comme suit:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;            
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync("ms-appx:///file.txt");
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```

        Une fois la méthode [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) exécutée, elle renvoie une classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) qui représente le fichier *file.txt* dans le répertoire d’installation de l’application (`file`, dans cet exemple).

        Le préfixe «ms-appx:///» de l’URI fait référence au répertoire d’installation de l’application. Pour en savoir plus sur l’utilisation des URI d’application, voir [Comment utiliser des URI pour référencer du contenu](https://msdn.microsoft.com/library/windows/apps/hh781215).

    En outre, contrairement à d’autres emplacements, vous pouvez également accéder à des fichiers dans le répertoire d’installation de votre application à l’aide de certaines [API Win32 et COM pour les applications de plateforme Windows universelle (UWP)](https://msdn.microsoft.com/library/windows/apps/br205757) et de certaines [fonctions de la bibliothèque C/C++ standard à partir de Microsoft VisualStudio](http://msdn.microsoft.com/library/hh875057.aspx).

    Le répertoire d’installation de l’application est un emplacement en lecture seule. Vous ne pouvez pas accéder à ce répertoire d’installation par le biais du sélecteur de fichiers.

-   **Emplacements des données d’application** Les dossiers dans lesquels votre application peut stocker des données. Ces dossiers (local, roaming et temporary) sont créés lors de l’installation de votre application.

    Il existe deux manières principales d’accéder à des fichiers et des dossiers dans les emplacements des données de votre application :

    1.  Utilisez les propriétés [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587) pour récupérer un dossier de données de l’application.

        Par exemple, vous pouvez utiliser [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) pour récupérer une classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) qui représente le dossier local de votre application, comme suit :
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder localFolder = ApplicationData.Current.LocalFolder;
        ```
        ```javascript
        var localFolder = Windows.Storage.ApplicationData.current.localFolder;
        ```

        Si vous souhaitez accéder au dossier roaming ou temporary de votre application, utilisez la propriété [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) ou [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) à la place.

        Une fois que vous avez récupéré une classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) qui représente un emplacement des données de l’application, vous pouvez accéder aux fichiers et dossiers situés dans cet emplacement en utilisant les méthodes **StorageFolder**. Dans cet exemple, ces objets **StorageFolder** sont stockés dans la variable `localFolder`. Pour en savoir plus sur l’utilisation des emplacements des données de l’application, consultez [Gestion des données d’application](https://msdn.microsoft.com/library/windows/apps/hh465109), puis téléchargez l’[exemple de données d’application](http://go.microsoft.com/fwlink/p/?linkid=231478) pour Windows8.1 et réutilisez son code source dans votre application Windows10.

    2.  Par exemple, vous pouvez récupérer un fichier directement à partir du dossier local de votre application en utilisant un URI d’application, comme suit:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```

        Une fois la méthode [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) exécutée, elle renvoie une classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) qui représente le fichier *file.txt* dans le dossier local de l’application (`file`, dans cet exemple).

        Le préfixe «ms-appdata:///local/» de l’URI fait référence au dossier local de l’application. Pour accéder à des fichiers dans les dossiers roaming ou temporary, utilisez «ms-appdata:///roaming/» ou «ms-appdata:///temporary/» à la place. Pour en savoir plus sur l’utilisation des URI d’application, voir [Comment charger des ressources de fichiers](https://msdn.microsoft.com/library/windows/apps/hh781229).

    En outre, contrairement à d’autres emplacements, vous pouvez également accéder à des fichiers dans les emplacements de données de votre application à l’aide de certaines [API Win32 et COM pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/br205757) et de certaines fonctions de la bibliothèque C/C++ standard à partir de Microsoft VisualStudio.

    Vous ne pouvez pas accéder aux dossiers local, roaming ni temporary par le biais du sélecteur de fichiers.

-   **Périphériques amovibles** De plus, votre application peut accéder par défaut à certains fichiers sur les périphériques connectés. Cela est une option si votre application utilise l’[extension de lecture automatique](https://msdn.microsoft.com/library/windows/apps/xaml/hh464906.aspx#autoplay) pour se lancer automatiquement lorsque l’utilisateur connecte un appareil, tel qu’un appareil photo ou une cléUSB, à son système. Les fichiers auxquels votre application peut accéder sont limités aux types de fichiers spécifiques qui sont spécifiés par le biais de déclarations d’associations de types de fichiers dans le manifeste de votre application.

    Bien entendu, vous pouvez également accéder à des fichiers et dossiers sur un périphérique amovible en appelant le sélecteur de fichiers (en utilisant [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) et [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) et en laissant l’utilisateur sélectionner les fichiers et dossiers auxquels votre application pourra accéder. Découvrez comment utiliser le sélecteur de fichiers dans [Ouvrir des fichiers et dossiers à l’aide d’un sélecteur](quickstart-using-file-and-folder-pickers.md).

    **Remarque** Pour plus d’informations sur l’accès à une carteSD à partir d’une application mobile, voir [Accéder à la carteSD](access-the-sd-card.md).

     

## Emplacements auxquels les applications du Windows Store peuvent accéder

-   **Dossier Téléchargements de l’utilisateur** Dossier dans lequel les fichiers téléchargés sont enregistrés par défaut.

    Par défaut, votre application peut seulement accéder aux fichiers et dossiers situés dans le dossier Téléchargements de l’utilisateur que votre application a créé. Toutefois, vous pouvez accéder aux fichiers et dossiers situés dans le dossier Téléchargements de l’utilisateur en appelant un sélecteur de fichiers ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) ou [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) afin que les utilisateurs puissent parcourir et sélectionner les fichiers ou les dossiers auxquels votre application pourra accéder.

    -   Vous pouvez créer un fichier dans le dossier Téléchargements de l’utilisateur comme suit :
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
            function(newFile) {
                // Process file
            }
        );
        ```

        [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761) est surchargé afin que vous puissiez spécifier ce que le système doit faire si le dossier Téléchargements contient déjà un fichier du même nom. Une fois ces méthodes exécutées, elles renvoient une classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) qui représente le fichier qui a été créé. Ce fichier est appelé `newFile` dans cet exemple.

    -   Vous pouvez créer un sous-dossier dans le dossier Téléchargements de l’utilisateur comme suit:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
            function(newFolder) {
                // Process folder
            }
        );
        ```

        [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763) est surchargé afin que vous puissiez spécifier ce que le système doit faire si le dossier Téléchargements contient déjà un sous-dossier du même nom. Une fois ces méthodes exécutées, elles renvoient une classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) qui représente le sous-dossier qui a été créé. Ce fichier est appelé `newFolder` dans cet exemple.

    Si vous créez un fichier ou un dossier dans le dossier Téléchargements, nous vous recommandons d’ajouter cet élément à la propriété [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) de votre application afin qu’elle puisse accéder à cet élément dans le futur.

## Accès à des emplacements supplémentaires

Outre les emplacements par défaut, une application peut accéder à des fichiers et dossiers supplémentaires en déclarant des fonctionnalités dans le manifeste de l’application (voir [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/library/windows/apps/mt270968)) ou en appelant un sélecteur de fichiers pour permettre à l’utilisateur de sélectionner les fichiers et les dossiers auxquels l’application pourra accéder (voir [Ouvrir des fichiers et dossiers à l’aide d’un sélecteur](quickstart-using-file-and-folder-pickers.md)).

Le tableau qui suit dresse une liste d’emplacements supplémentaires auxquels vous pouvez accéder en déclarant une ou plusieurs fonctionnalités et en vous servant de l’API [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) associée:

| Emplacement | Fonctionnalité | API Windows.Storage |
|----------|------------|---------------------|
| Documents | DocumentsLibrary <br><br>Remarque: vous devez ajouter des associations de types de fichiers dans le manifeste de l’application pour déclarer les types de fichiers spécifiques auxquels votre application pourra accéder dans cet emplacement. <br><br>Utilisez cette fonctionnalité si votre application:<br>- facilite l’accès hors connexion interplateforme à du contenu OneDrive spécifique à l’aide d’URL ou d’ID de ressource OneDrive valides;<br>- enregistre automatiquement les fichiers ouverts sur le OneDrive de l’utilisateur en mode hors connexion. | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| Musique     | MusicLibrary <br>Voir également [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| Images  | PicturesLibrary<br> Voir également [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| Vidéos    | VideosLibrary<br>Voir également [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| Périphériques amovibles  | RemovableDevices <br><br>Remarque: vous devez ajouter des associations de types de fichiers dans le manifeste de l’application pour déclarer les types de fichiers spécifiques auxquels votre application pourra accéder dans cet emplacement. <br><br>Voir également [Accéder à la carteSD](access-the-sd-card.md). | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| Bibliothèques du groupe résidentiel  | Au moins une des fonctionnalités suivantes est requise. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| Appareils de serveur multimédia (DLNA) | Au moins une des fonctionnalités suivantes est requise. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) |
| Dossiers UNC (Universal Naming Convention) | Une combinaison des fonctionnalités suivantes est requise. <br><br>Fonctionnalité des réseaux domestiques et d’entreprise: <br>- PrivateNetworkClientServer <br><br>Et au moins une fonctionnalité de réseaux Internet et publics: <br>- InternetClient <br>- InternetClientServer <br><br>Et, le cas échéant, la fonctionnalité des informations d’identification de domaine:<br>- EnterpriseAuthentication <br><br>Remarque: vous devez ajouter des associations de types de fichiers dans le manifeste de l’application pour déclarer les types de fichiers spécifiques auxquels votre application pourra accéder dans cet emplacement. | Récupérez un dossier en utilisant: <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>Récupérez un fichier en utilisant: <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |



<!--HONumber=Aug16_HO4-->


