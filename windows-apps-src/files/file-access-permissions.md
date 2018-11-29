---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: Autorisations d’accès aux fichiers
description: Les applications peuvent accéder à certains emplacements du système de fichiers par défaut. Les applications peuvent également accéder à des emplacements supplémentaires par le biais du sélecteur de fichiers ou par la déclaration de fonctionnalités.
ms.date: 06/28/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d960235e73ea9172fb966f227af9440923f3553e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7978259"
---
# <a name="file-access-permissions"></a>Autorisations d’accès aux fichiers

Les applications Windows universelles (applications) peuvent accéder à certains emplacements de systèmes de fichiers par défaut. Les applications peuvent également accéder à des emplacements supplémentaires par le biais du sélecteur de fichiers ou par la déclaration de fonctionnalités.

## <a name="the-locations-that-all-apps-can-access"></a>Emplacements accessibles à toutes les applications

Lorsque vous créez une application, vous pouvez accéder par défaut aux emplacements suivants du système de fichiers :

### <a name="application-install-directory"></a>Répertoire d’installation application
Le dossier dans lequel votre application est installée sur le système de l’utilisateur.

Il existe deux manières principales pour accéder aux fichiers et dossiers de votre application répertoire d’installation:

1. Vous pouvez récupérer une classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) qui représente le répertoire d’installation de votre application, comme suit :

```csharp
Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
```

```javascript
var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
```

```cppwinrt
#include <winrt/Windows.Storage.h>
...
Windows::Storage::StorageFolder installedLocation{ Windows::ApplicationModel::Package::Current().InstalledLocation() };
```

```cpp
Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
```

Vous pouvez accéder aux fichiers et dossiers du répertoire à l’aide des méthodes [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230). Dans l’exemple, cette classe **StorageFolder** est stockée dans la variable `installDirectory`. Pour plus d’informations sur l’utilisation de votre package d’application et du répertoire d’installation, consultez l’[exemple d’informations de package d’application](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package) sur GitHub.

2. Vous pouvez récupérer un fichier directement à partir du répertoire d’installation de votre application en utilisant un URI d’application, comme suit:

```cs
using Windows.Storage;            
StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
```

```javascript
Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
    function(file) {
        // Process file
    }
);
```

```cppwinrt
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFile file{
        co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{L"ms-appx:///file.txt"})
    };
    // Process file
}
```

```cpp
auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
getFileTask.then([](StorageFile^ file) 
{
    // Process file
});
```

Une fois la méthode [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) exécutée, elle renvoie une classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) qui représente le fichier `file.txt` dans le répertoire d’installation de l’application (`file` dans cet exemple).

Le préfixe «ms-appx:///» de l’URI fait référence au répertoire d’installation de l’application. Pour en savoir plus sur l’utilisation des URI d’application, voir [Comment utiliser des URI pour référencer du contenu](https://msdn.microsoft.com/library/windows/apps/hh781215).

En outre, contrairement à d’autres emplacements, vous pouvez également accéder à des fichiers dans le répertoire d’installation de votre application à l’aide de certaines [API Win32 et COM pour les applications de plateforme Windows universelle (UWP)](https://msdn.microsoft.com/library/windows/apps/br205757) et de certaines [fonctions de la bibliothèque C/C++ standard à partir de Microsoft VisualStudio](http://msdn.microsoft.com/library/hh875057.aspx).

Le répertoire d’installation de l’application est un emplacement en lecture seule. Vous ne pouvez pas accéder au répertoire d’installation par le biais du sélecteur de fichiers.

### <a name="application-data-locations"></a>Emplacements des données d’application
Les dossiers dans lesquels votre application peut stocker des données. Ces dossiers (local, roaming et temporary) sont créés lors de l’installation de votre application.

Il existe deux manières principales d’accéder aux fichiers et dossiers à partir d’emplacements de données de votre application:

1.  Utilisez les propriétés [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587) pour récupérer un dossier de données de l’application.

Par exemple, vous pouvez utiliser [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) pour récupérer une classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) qui représente le dossier local de votre application, comme suit :

```cs
using Windows.Storage;
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
```

```javascript
var localFolder = Windows.Storage.ApplicationData.current.localFolder;
```

```cppwinrt
Windows::Storage::StorageFolder storageFolder{
    Windows::Storage::ApplicationData::Current().LocalFolder()
};
```

```cpp
using namespace Windows::Storage;
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
```

Si vous souhaitez accéder au dossier roaming ou temporary de votre application, utilisez la propriété [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) ou [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) à la place.

Une fois que vous avez récupéré une classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) qui représente un emplacement des données de l’application, vous pouvez accéder aux fichiers et dossiers situés dans cet emplacement en utilisant les méthodes **StorageFolder**. Dans cet exemple, ces objets **StorageFolder** sont stockés dans la variable `localFolder`. Pour plus d’informations sur l’utilisation des emplacements de données de l’application, consultez la page [Classe ApplicationData](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata), puis téléchargez l’[exemple de données d’application](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) à partir de GitHub.

2. Par exemple, vous pouvez récupérer un fichier directement à partir du dossier local de votre application en utilisant un URI d’application, comme suit:

```cs
using Windows.Storage;
StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appdata:///local/file.txt"));
```

```javascript
Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
    function(file) {
        // Process file
    }
);
```

```cppwinrt
Windows::Storage::StorageFile file{
    co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{ L"ms-appdata:///local/file.txt" })
};
// Process file
```

```cpp
using Windows::Storage;
auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
getFileTask.then([](StorageFile^ file) 
{
    // Process file
});
```

Une fois la méthode [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) exécutée, elle renvoie une classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) qui représente le fichier `file.txt` dans le dossier local de l’application (`file` dans cet exemple).

Le préfixe «ms-appdata:///local/» de l’URI fait référence au dossier local de l’application. Pour accéder à des fichiers dans les dossiers roaming ou temporary, utilisez «ms-appdata:///roaming/» ou «ms-appdata:///temporary/» à la place. Pour en savoir plus sur l’utilisation des URI d’application, voir [Comment charger des ressources de fichiers](https://msdn.microsoft.com/library/windows/apps/hh781229).

En outre, contrairement à d’autres emplacements, vous pouvez également accéder à des fichiers dans les emplacements de données de votre application à l’aide de certaines [API Win32 et COM pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/br205757) et de certaines fonctions de la bibliothèque C/C++ standard à partir de Microsoft VisualStudio.

Vous ne pouvez pas accéder aux dossiers locales, roaming ni temporary par le biais du sélecteur de fichiers.

### <a name="removable-devices"></a>Périphériques amovibles
De plus, votre application peut accéder par défaut à certains fichiers sur les périphériques connectés. Cela est une option si votre application utilise l’[extension de lecture automatique](https://msdn.microsoft.com/library/windows/apps/xaml/hh464906.aspx#autoplay) pour se lancer automatiquement lorsque l’utilisateur connecte un appareil, tel qu’un appareil photo ou une cléUSB, à son système. Les fichiers auxquels votre application peut accéder sont limités aux types de fichiers spécifiques qui sont spécifiés par le biais de déclarations d’associations de types de fichiers dans le manifeste de votre application.

Bien entendu, vous pouvez également accéder à des fichiers et dossiers sur un périphérique amovible en appelant le sélecteur de fichiers (en utilisant [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) et [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) et en laissant l’utilisateur sélectionner les fichiers et dossiers auxquels votre application pourra accéder. Découvrez comment utiliser le sélecteur de fichiers dans [Ouvrir des fichiers et dossiers à l’aide d’un sélecteur](quickstart-using-file-and-folder-pickers.md).

> [!NOTE]
> Pour en savoir plus sur l’accès à une carte mémoireSecure Digital ou à d’autres appareils amovibles, consultez l’article [Accéder à la carte SD](access-the-sd-card.md).

## <a name="locations-that-uwp-apps-can-access"></a>Emplacements auxquels les applications UWP peuvent accéder
### <a name="users-downloads-folder"></a>Dossier Téléchargements de l’utilisateur

Dossier dans lequel les fichiers téléchargés sont enregistrés par défaut.

Par défaut, votre application peut seulement accéder aux fichiers et dossiers situés dans le dossier Téléchargements de l’utilisateur que votre application a créé. Toutefois, vous pouvez accéder aux fichiers et dossiers situés dans le dossier Téléchargements de l’utilisateur en appelant un sélecteur de fichiers ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) ou [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) afin que les utilisateurs puissent parcourir et sélectionner les fichiers ou les dossiers auxquels votre application pourra accéder.

- Vous pouvez créer un fichier dans le dossier Téléchargements de l’utilisateur comme suit :

```cs
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

```cppwinrt
Windows::Storage::StorageFile newFile{
    co_await Windows::Storage::DownloadsFolder::CreateFileAsync(L"file.txt")
};
// Process file
```

```cpp
using Windows::Storage;
auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
createFileTask.then([](StorageFile^ newFile)
{
    // Process file
});
```

[**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761) est surchargé afin que vous puissiez spécifier ce que le système doit faire si le dossier Téléchargements contient déjà un fichier du même nom. Une fois ces méthodes exécutées, elles renvoient une classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) qui représente le fichier qui a été créé. Ce fichier est appelé `newFile` dans cet exemple.

- Vous pouvez créer un sous-dossier dans le dossier Téléchargements de l’utilisateur comme suit:

```cs
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

```cppwinrt
Windows::Storage::StorageFolder newFolder{
    co_await Windows::Storage::DownloadsFolder::CreateFolderAsync(L"New Folder")
};
// Process folder
```

```cpp
using Windows::Storage;
auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
createFolderTask.then([](StorageFolder^ newFolder)
{
    // Process folder
});
```

[**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763) est surchargé afin que vous puissiez spécifier ce que le système doit faire si le dossier Téléchargements contient déjà un sous-dossier du même nom. Une fois ces méthodes exécutées, elles renvoient une classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) qui représente le sous-dossier qui a été créé. Ce fichier est appelé `newFolder` dans cet exemple.

Si vous créez un fichier ou un dossier dans le dossier Téléchargements, nous vous recommandons d’ajouter cet élément à la propriété [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) de votre application afin qu’elle puisse accéder à cet élément dans le futur.

## <a name="accessing-additional-locations"></a>Accès à des emplacements supplémentaires

Outre les emplacements par défaut, une application peut accéder à des fichiers et dossiers supplémentaires en déclarant des fonctionnalités dans le manifeste de l’application (voir [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/library/windows/apps/mt270968)) ou en appelant un sélecteur de fichiers pour permettre à l’utilisateur de sélectionner les fichiers et les dossiers auxquels l’application pourra accéder (voir [Ouvrir des fichiers et dossiers à l’aide d’un sélecteur](quickstart-using-file-and-folder-pickers.md)).

Les applications qui déclarent l'extension [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) disposent des autorisations de systèmes de fichiers à partir du répertoire depuis lequel elles sont lancées dans la fenêtre de console et aux niveaux inférieurs.

Le tableau qui suit dresse une liste d’emplacements supplémentaires auxquels vous pouvez accéder en déclarant une ou plusieurs fonctionnalités et en vous servant de l’API [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) associée:

| Emplacement | Fonctionnalité | API Windows.Storage |
|----------|------------|---------------------|
| Tous les fichiers auxquels l’utilisateur a accès. Par exemple: documents, images, photos, téléchargements, bureau, OneDrive, etc. | broadFileSystemAccess<br><br>Il s’agit d’une fonctionnalité restreinte. À la première utilisation, le système invite l’utilisateur à autoriser l’accès. L’accès est configurable dans Paramètres > Confidentialité > Système de fichiers. Si vous soumettez une application au Store qui déclare cette fonctionnalité, vous devrez fournir des descriptions supplémentaires de la raison pour laquelle votre application a besoin de cette fonctionnalité, et comment elle a l’intention de l’utiliser.<br>Cette fonctionnalité fonctionne pour les API dans l'espace de noms [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346). | Non applicable |
| Documents | DocumentsLibrary <br><br>Remarque: vous devez ajouter des associations de types de fichiers dans le manifeste de l’application pour déclarer les types de fichiers spécifiques auxquels votre application pourra accéder dans cet emplacement. <br><br>Utilisez cette fonctionnalité si votre application:<br>- facilite l’accès hors connexion interplateforme à du contenu OneDrive spécifique à l’aide d’URL ou d’ID de ressource OneDrive valides;<br>-Enregistre les fichiers ouverts sur le OneDrive de l’utilisateur automatiquement en mode hors connexion | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| Musique     | MusicLibrary <br>Voir également [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| Images  | PicturesLibrary<br> Voir également [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| Vidéos    | VideosLibrary<br>Voir également [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| Périphériques amovibles  | RemovableDevices <br><br>Remarque  Vous devez ajouter des associations de types de fichiers dans le manifeste de l’application pour déclarer les types de fichiers spécifiques auxquels votre application pourra accéder à cet emplacement. <br><br>Voir également [Accéder à la carteSD](access-the-sd-card.md). | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| Bibliothèques du groupe résidentiel  | Au moins une des fonctionnalités suivantes est requise. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| Appareils de serveur multimédia (DLNA) | Au moins une des fonctionnalités suivantes est requise. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) |
| Dossiers UNC (Universal Naming Convention) | Une combinaison des fonctionnalités suivantes est requise. <br><br>Fonctionnalité des réseaux domestiques et d’entreprise: <br>- PrivateNetworkClientServer <br><br>Et au moins une fonctionnalité de réseaux Internet et publics: <br>- InternetClient <br>- InternetClientServer <br><br>Et, le cas échéant, la fonctionnalité des informations d’identification de domaine:<br>- EnterpriseAuthentication <br><br>Remarque: vous devez ajouter des associations de types de fichiers dans le manifeste de l’application pour déclarer les types de fichiers spécifiques auxquels votre application pourra accéder dans cet emplacement. | Récupérez un dossier en utilisant: <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>Récupérez un fichier en utilisant: <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |

**Exemple**

Cet exemple ajoute la fonctionnalité restreinte `broadFileSystemAccess`. Outre la spécification de la fonctionnalité, l'espace de noms `rescap` doit être ajouté et est également ajouté à `IgnorableNamespaces`:

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp uap5 rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> Pour obtenir la liste complète des fonctionnalités de l’application, consultez l’article [Déclarations des fonctionnalités d’application](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).
