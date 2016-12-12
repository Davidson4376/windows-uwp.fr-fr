---
author: laurenhughes
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: Determining availability of Microsoft OneDrive files
description: Determine if a Microsoft OneDrive file is available using the StorageFile.IsAvailable property.
translationtype: Human Translation
ms.sourcegitcommit: 6822bb63ac99efdcdd0e71c4445883f4df5f471d
ms.openlocfilehash: 2289b85a8b26e1827446709e1db97c447b3b7964

---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>Determining availability of Microsoft OneDrive files

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** Important APIs **

-   [**FileIO class**](https://msdn.microsoft.com/library/windows/apps/Hh701440)
-   [**StorageFile class**](https://msdn.microsoft.com/library/windows/apps/BR227171)
-   [**StorageFile.IsAvailable property**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx)

Determine if a Microsoft OneDrive file is available using the [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) property.

## <a name="prerequisites"></a>Prerequisites

-   **Understand async programming for Universal Windows Platform (UWP) apps**

    You can learn how to write asynchronous apps in C# or Visual Basic, see [Call asynchronous APIs in C# or Visual Basic](https://msdn.microsoft.com/library/windows/apps/Mt187337). To learn how to write asynchronous apps in C++, see [Asynchronous programming in C++](https://msdn.microsoft.com/library/windows/apps/Mt187334).

-   **App capabilty declarations**

    See [File access permissions](file-access-permissions.md).

## <a name="using-the-storagefileisavailable-property"></a>Using the StorageFile.IsAvailable property

Users are able to mark OneDrive files as either available-offline (default) or online-only. This capability enables users to move large files (such as pictures and videos) to their OneDrive, mark them as online-only, and save disk space (the only thing kept locally is a metadata file).

[**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx), is used to determine if a file is currently available. The following table shows the value of the **StorageFile.IsAvailable** property in various scenarios.

| Type of file                              | Online | Metered network        | Offline |
|-------------------------------------------|--------|------------------------|---------|
| Local file                                | True   | True                   | True    |
| OneDrive file marked as available-offline | True   | True                   | True    |
| OneDrive file marked as online-only       | True   | Based on user settings | False   |
| Network file                              | True   | Based on user settings | False   |

 

The following steps illustrate how to determine if a file is currently available.

1.  Declare a capability appropriate for the library you want to access.
2.  Include the [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) namespace. This namespace includes the types for managing files, folders, and application settings. It also includes the needed [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) type.
3.  Acquire a [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) object for the desired file(s). If you are enumerating a library, this step is usually accomplished by calling the [**StorageFolder.CreateFileQuery**](https://msdn.microsoft.com/library/windows/apps/BR227252) method and then calling the resulting [**StorageFileQueryResult**](https://msdn.microsoft.com/library/windows/apps/BR208046) object's [**GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276.aspx) method. The **GetFilesAsync** method returns an [IReadOnlyList](http://go.microsoft.com/fwlink/p/?LinkId=324970) collection of **StorageFile** objects.
4.  Once you have the access to a [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) object representing the desired file(s), the value of the [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) property reflects whether or not the file is available.

The following generic method illustrates how to enumerate any folder and return the collection of [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) objects for that folder. The calling method then iterates over the returned collection referencing the [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) property for each file.

```CSharp
/// <summary>
/// Generic function that retrieves all files from the specified folder.
/// </summary>
/// <param name="folder">The folder to be searched.</param>
/// <returns>An IReadOnlyList collection containing the file objects.</returns>
async Task<System.Collections.Generic.IReadOnlyList<StorageFile>> GetLibraryFilesAsync(StorageFolder folder)
{
    var query = folder.CreateFileQuery();
    return await query.GetFilesAsync();
}

...

private async void CheckAvailabilityOfFilesInPicturesLibrary()
{
    // Determine availability of all files within Pictures library.
    var files = await GetLibraryFilesAsync(KnownFolders.PicturesLibrary);
    for (int i = 0; i < files.Count; i++)
    {
        StorageFile file = files[i];

        StringBuilder fileInfo = new StringBuilder();
        fileInfo.AppendFormat("{0} (on {1}) is {2}",
                    file.Name,
                    file.Provider.DisplayName,
                    file.IsAvailable ? "available" : "not available");
    }
}
```

 

 



<!--HONumber=Dec16_HO1-->


