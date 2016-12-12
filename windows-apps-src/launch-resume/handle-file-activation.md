---
author: TylerMSFT
title: Handle file activation
description: An app can register to become the default handler for a certain file type.
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
translationtype: Human Translation
ms.sourcegitcommit: ed7aee6add80d31b48006d9dec9e207c449a1912
ms.openlocfilehash: ffcfa8991e9eb73b8d6a47bb7dd1cd23220097e0

---

# <a name="handle-file-activation"></a>Handle file activation


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Important APIs**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

An app can register to become the default handler for a certain file type. Both Windows desktop applications and Universal Windows Platform (UWP) apps can register to be a default file handler. If the user chooses your app as the default handler for a certain file type, your app will be activated when that type of file is launched.

We recommend that you only register for a file type if you expect to handle all file launches for that type of file. If your app only needs to use the file type internally, then you don't need to register to be the default handler. If you do choose to register for a file type, you must provide the end user with the functionality that is expected when your app is activated for that file type. For example, a picture viewer app may register to display a .jpg file. For more info on file associations, see [Guidelines for file types and URIs](https://msdn.microsoft.com/library/windows/apps/hh700321).

These steps show how to register for a custom file type, .alsdk, and how to activate your app when the user launches an .alsdk file.

> **Note**  In UWP apps, certain URIs and file extensions are reserved for use by built-in apps and the operating system. Attempts to register your app with a reserved URI or file extension will be ignored. For more information, see [Reserved file and URI scheme names](reserved-uri-scheme-names.md).

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Step 1: Specify the extension point in the package manifest


The app receives activation events only for the file extensions listed in the package manifest. Here's how you indicate that your app handles the files with the `.alsdk` extension.

1.  In the **Solution Explorer**, double-click package.appxmanifest to open the manifest designer. Select the **Declarations** tab and in the **Available Declarations** drop-down, select **File Type Associations** and then click **Add**. See [Programmatic Identifiers](https://msdn.microsoft.com/library/windows/desktop/cc144152) for more details of identifiers used by file associations.

    Here is a brief description of each of the fields that you may fill in the manifest designer:

| Field | Description |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Display Name** | Specify the display name for a group of file types. The display name is used to identify the file type in the [Set Default Programs](https://msdn.microsoft.com/library/windows/desktop/cc144154) on the **Control Panel**. |
| **Logo** | Specify the logo that is used to identify the file type on the desktop and in the [Set Default Programs](https://msdn.microsoft.com/library/windows/desktop/cc144154) on the **Control Panel**. If no Logo is specified, the application’s small logo is used. |
| **Info Tip** | Specify the [info tip](https://msdn.microsoft.com/library/windows/desktop/cc144152) for a group of file types. This tool tip text appears when the user hovers on the icon for a file of this type. |
| **Name** | Choose a name for a group of file types that share the same display name, logo, info tip, and edit flags. Choose a group name that can stay the same across app updates. **Note**  The Name must be in all lower case letters. |
| **Content Type** | Specify the MIME content type, such as **image/jpeg**, for a particular file type. **Important Note about allowed content types:** Here is an alphabetic list of MIME content types that you cannot enter into the package manifest because they are either reserved or forbidden: **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **File type** | Specify the file type to register for, preceded by a period, for example, “.jpeg”. **Reserved and forbidden file types:** See [Reserved URI scheme names and file types](reserved-uri-scheme-names.md) for an alphabetic list of file types for built-in apps that you can't register for your UWP apps because they are either reserved or forbidden. |

2.  Enter `alsdk` as the **Name**.
3.  Enter `.alsdk` as the **File Type**.
4.  Enter “images\\Icon.png” as the Logo.
5.  Press Ctrl+S to save the change to package.appxmanifest.

The steps above add an [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) element like this one to the package manifest. The **windows.fileTypeAssociation** category indicates that the app handles files with the `.alsdk` extension.

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## <a name="step-2-add-the-proper-icons"></a>Step 2: Add the proper icons


Apps that become the default for a file type have their icons displayed in various places throughout the system. For example these icons are shown in:

-   Windows Explorer ItemsView, context menus, and the Ribbon
-   Default programs Control Panel
-   File picker
-   Search results on the Start screen

Match the look of the app tile logo and use your app's background color rather than making the icon transparent. Have the logo extend to the edge without padding it. Test your icons on white backgrounds. For example icons, see the [Association launching sample](http://go.microsoft.com/fwlink/p/?LinkID=620490).
![the solution explorer with a view of the files in the images folder. there are 16, 32, 48, and 256 pixel versions of both ‘icon.targetsize’ and ‘smalltile-sdk’](images/seviewofimages.png)

## <a name="step-3-handle-the-activated-event"></a>Step 3: Handle the activated event


The [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331) event handler receives all file activation events.

> [!div class="tabbedCodeSnippets"]
```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```
```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
       // TODO: Handle file activation
       // The number of files received is args->Files->Size
       // The first file is args->Files->GetAt(0)->Name
}
```
```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

    > **Note**  When launched via File Contract, make sure that Back button takes the user back to the screen that launched the app and not to the app's previous content.

It is recommended that apps create a new XAML Frame for each activation event that opens a new page. This way, the navigation backstack for the new XAML Frame will not contain any previous content that the app might have on the current window when suspended. Apps that decide to use a single XAML Frame for Launch and File Contracts should clear the pages on the Frame's navigation journal before navigating to a new page.

When launched via File activation, apps should consider including UI that allows the user to go back to the top page of the app.

## <a name="remarks"></a>Remarks


The files that you receive could come from an untrusted source. We recommend that you validate the content of a file before taking action on it. For more info on input validation, see [Writing Secure Code](http://go.microsoft.com/fwlink/p/?LinkID=142053)

> **Note**  This article is for Windows 10 developers writing Universal Windows Platform (UWP) apps. If you’re developing for Windows 8.x or Windows Phone 8.x, see the [archived documentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <a name="related-topics"></a>Related topics

**Complete example**

* [Association launching sample](http://go.microsoft.com/fwlink/p/?LinkID=231484)

**Concepts**

* [Default Programs](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [File Type and Protocol Associations Model](https://msdn.microsoft.com/library/windows/desktop/hh848047)

**Tasks**

* [Launch the default app for a file](launch-the-default-app-for-a-file.md)
* [Handle URI activation](handle-uri-activation.md)

**Guidelines**

* [Guidelines for file types and URIs](https://msdn.microsoft.com/library/windows/apps/hh700321)

**Reference**
* [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
* [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

 

 



<!--HONumber=Dec16_HO1-->


