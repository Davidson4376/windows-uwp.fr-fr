---
author: laurenhughes
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: File access permissions
description: Apps can access certain file system locations by default. Apps can also access additional locations through the file picker, or by declaring capabilities.
translationtype: Human Translation
ms.sourcegitcommit: 6822bb63ac99efdcdd0e71c4445883f4df5f471d
ms.openlocfilehash: d6cb8bd32766157b1edf886621bdc605ca160db5

---
# <a name="file-access-permissions"></a>File access permissions

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Apps can access certain file system locations by default. Apps can also access additional locations through the file picker, or by declaring capabilities.

## <a name="the-locations-that-all-apps-can-access"></a>The locations that all apps can access

When you create a new app, you can access the following file system locations by default:

-   **Application install directory**. The folder where your app is installed on the user’s system.

    There are two primary ways to access files and folders in your app’s install directory:

    1.  You can retrieve a [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) that represents your app's install directory, like this:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
        ```
        ```javascript
        var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
        ```

       You can then access files and folders in the directory using [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) methods. In the example, this **StorageFolder** is stored in the `installDirectory` variable. You can learn more about working with your app package and install directory by downloading the [App package information sample](http://go.microsoft.com/fwlink/p/?linkid=231526) for Windows 8.1 and re-using its source code in your Windows 10 app.

    2.  You can retrieve a file directly from your app's install directory by using an app URI, like this:
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

        When [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) completes, it returns a [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) that represents the *file.txt* file in the app's install directory (`file` in the example).

        The "ms-appx:///" prefix in the URI refers to the app's install directory. You can learn more about using app URIs in [How to use URIs to reference content](https://msdn.microsoft.com/library/windows/apps/hh781215).

    In addition, and unlike other locations, you can also access files in your app install directory by using some [Win32 and COM for Universal Windows Platform (UWP) apps](https://msdn.microsoft.com/library/windows/apps/br205757) and some [C/C++ Standard Library functions from Microsoft Visual Studio](http://msdn.microsoft.com/library/hh875057.aspx).

    The app's install directory is a read-only location. You can’t gain access to the install directory through the file picker.

-   **Application data locations.** The folders where your app can store data. These folders (local, roaming and temporary) are created when your app is installed.

    There are two primary ways to access files and folders from your app’s data locations:

    1.  Use [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587) properties to retrieve an app data folder.

        For example, you can use [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) to retrieve a [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) that represents your app's local folder like this:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder localFolder = ApplicationData.Current.LocalFolder;
        ```
        ```javascript
        var localFolder = Windows.Storage.ApplicationData.current.localFolder;
        ```

        If you want to access your app's roaming or temporary folder, use the [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) or [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) property instead.

        After you retrieve a [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) that represents an app data location, you can access files and folders in that location by using **StorageFolder** methods. In the example, these **StorageFolder** objects are stored in the `localFolder` variable. You can learn more about using app data locations in [Managing application data](https://msdn.microsoft.com/library/windows/apps/hh465109), and by downloading the [Application data sample](http://go.microsoft.com/fwlink/p/?linkid=231478) for Windows 8.1 and re-using its source code in your Windows 10 app.

    2.  For example, you can retrieve a file directly from your app's local folder by using an app URI, like this:
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

        When [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) completes, it returns a [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) that represents the *file.txt* file in the app's local folder (`file` in the example).

        The "ms-appdata:///local/" prefix in the URI refers to the app's local folder. To access files in the app's roaming or temporary folders use "ms-appdata:///roaming/" or "ms-appdata:///temporary/" instead. You can learn more about using app URIs in [How to load file resources](https://msdn.microsoft.com/library/windows/apps/hh781229).

    In addition, and unlike other locations, you can also access files in your app data locations by using some [Win32 and COM for UWP apps](https://msdn.microsoft.com/library/windows/apps/br205757) and some C/C++ Standard Library functions from Visual Studio.

    You can’t access the local, roaming, or temporary folders through the file picker.

-   **Removable devices.** Additionally, your app can access some of the files on connected devices by default. This is an option if your app uses the [AutoPlay extension](https://msdn.microsoft.com/library/windows/apps/xaml/hh464906.aspx#autoplay) to launch automatically when users connect a device, like a camera or USB thumb drive, to their system. The files your app can access are limited to specific file types that are specified via File Type Association declarations in your app manifest.

    Of course, you can also gain access to files and folders on a removable device by calling the file picker (using [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) and [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) and letting the user pick files and folders for your app to access. Learn how to use the file picker in [Open files and folders with a picker](quickstart-using-file-and-folder-pickers.md).

    **Note**  For more info about accessing an SD card from a mobile app, see [Access the SD card](access-the-sd-card.md).

     

## <a name="locations-windows-store-apps-can-access"></a>Locations Windows Store apps can access

-   **User’s Downloads folder.** The folder where downloaded files are saved by default.

    By default, your app can only access files and folders in the user's Downloads folder that your app created. However, you can gain access to files and folders in the user's Downloads folder by calling a file picker ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) or [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) so that users can navigate and pick files or folders for your app to access.

    -   You can create a file in the user's Downloads folder like this:
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

        [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761) is overloaded so that you can specify what the system should do if there is already an existing file in the Downloads folder that has the same name. When these methods complete, they return a [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) that represents the file that was created. This file is called `newFile` in the example.

    -   You can create a subfolder in the user's Downloads folder like this:
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

        [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763) is overloaded so that you can specify what the system should do if there is already an existing subfolder in the Downloads folder that has the same name. When these methods complete, they return a [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) that represents the subfolder that was created. This file is called `newFolder` in the example.

    If you create a file or folder in the Downloads folder, we recommend that you add that item to your app's [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) so that your app can readily access that item in the future.

## <a name="accessing-additional-locations"></a>Accessing additional locations

In addition to the default locations, an app can access additional files and folders by declaring capabilities in the app manifest (see [App capability declarations](https://msdn.microsoft.com/library/windows/apps/mt270968)), or by calling a file picker to let the user pick files and folders for the app to access (see [Open files and folders with a picker](quickstart-using-file-and-folder-pickers.md)).

The following table lists additional locations that you can access by declaring a capability (or capabilities) and using the associated [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) API:

| Location | Capability | Windows.Storage API |
|----------|------------|---------------------|
| Documents | DocumentsLibrary <br><br>Note: You must add File Type Associations to your app manifest that declare specific file types that your app can access in this location. <br><br>Use this capability if your app:<br>- Facilitates cross-platform offline access to specific OneDrive content using valid OneDrive URLs or Resource IDs<br>- Saves open files to the user’s OneDrive automatically while offline | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| Music     | MusicLibrary <br>Also see [Files and folders in the Music, Pictures, and Videos libraries](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| Pictures  | PicturesLibrary<br> Also see [Files and folders in the Music, Pictures, and Videos libraries](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| Videos    | VideosLibrary<br>Also see [Files and folders in the Music, Pictures, and Videos libraries](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| Removable devices  | RemovableDevices <br><br>Note  You must add File Type Associations to your app manifest that declare specific file types that your app can access in this location. <br><br>Also see [Access the SD card](access-the-sd-card.md). | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| Homegroup libraries  | At least one of the following capabilities is needed. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| Media server devices (DLNA) | At least one of the following capabilities is needed. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) |
| Universal Naming Convention (UNC) folders | A combination of the following capabilities is needed. <br><br>The home and work networks capability: <br>- PrivateNetworkClientServer <br><br>And at least one internet and public networks capability: <br>- InternetClient <br>- InternetClientServer <br><br>And, if applicable, the domain credentials capability:<br>- EnterpriseAuthentication <br><br>Note: You must add File Type Associations to your app manifest that declare specific file types that your app can access in this location. | Retrieve a folder using: <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>Retrieve a file using: <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |



<!--HONumber=Dec16_HO1-->


