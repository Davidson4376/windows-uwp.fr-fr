---
author: awkoren
Description: Prepare your Windows desktop application (like Win32, WPF, and Windows Forms) for conversion to a Universal Windows Platform (UWP) app by using the Desktop Conversion extensions.
Search.Product: eADQiWindows 10XVcnh
title: Convert your desktop application to a Universal Windows Platform (UWP) app
translationtype: Human Translation
ms.sourcegitcommit: ff8cd3ab5e38cfc3a2b5fabaad15f78a5f2620f2
ms.openlocfilehash: 6cf6367ed0f6acea0f87ac36a050e874425423b1

---

# Convert your desktop application to a Universal Windows Platform (UWP) app

\[Some information relates to pre-released product which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.\]

Prepare your Windows desktop application (like Win32, WPF, and Windows Forms) for conversion to a Universal Windows Platform (UWP) app by using the Desktop Conversion extensions.

## Benefits of converting your application to UWP

UWP using Desktop Conversion extensions is a bridge that enables you to convert your Windows desktop application (like Win32, Windows Forms, and WPF) or game to a Universal Windows Platform (UWP) app or game. For more info, see [Guide to UWP apps](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx). After conversion, your Windows desktop application is packaged, serviced, and deployed in the form of a UWP app package (an .appx or an .appxbundle) targeting Windows 10 Desktop.

There are two parts to the technology that enables desktop apps to be converted to UWP packages. The first is the Desktop App Converter, which takes your existing binaries and repackages them as a UWP package. Your code is still the same, it's just packaged differently. The second piece comprises runtime technologies in the Windows Anniversary update that enable a UWP package to have executables that run as full trust instead of in an app container. This technology also gives a converted app a package identity, which is required to use some UWP APIs.

Here are some of the benefits of converting your Windows desktop application.

* Your app's installation experience is much smoother for your customers. You can deploy it to computers using sideloading (see [Sideload LOB apps in Windows 10](https://technet.microsoft.com/library/mt269549.aspx)), and it leaves no trace behind after being uninstalled. Longer term, you'll also be able to publish your app to the Windows Store.

* Because your converted app has package identity, you can call more UWP APIs, even from the full-trust partition, than you could before. See a full list of [Supported UWP APIs for converted desktop apps](desktop-to-uwp-supported-api.md). 

* At your own pace, you can add UWP features to your app's package, like a XAML user-interface, live tile updates, UWP background tasks, app services, and many more. All of the functionality available to any other UWP app is available to your app.

* If you choose to move all of your app's functionality out of the full-trust partition of the app and into the app container partition, then your app will be able to run on any Windows 10 device.

* As a UWP app, your app is able to do the things it could do as a Windows desktop application. It interacts with a virtualized view of the registry and file system that's indistinguishable from the actual registry and file system.

* Your app can participate in the Windows Store's built-in licensing and automatic update facilities. Automatic update is a highly reliable and efficient mechanism, because only the changed parts of files are downloaded.

## Prepare your desktop app for conversion to UWP

You may not need to do much to get your app ready for the conversion process. Remember that the Windows Store handles licensing and automatic updating for you, so you can remove those features from your codebase. If any of these situations applies to your application, you need to address this issue before conversion.

+ __Your app uses a version of .NET earlier than 4.6.1__. Only .NET 4.6.1 is supported. You must retarget your app to .NET 4.6.1 before converting. 

+ __Your app always runs with elevated security privileges__. Your app needs to work while running as the interactive user. Users who install your app from the Windows Store may not be system administrators, so requiring your app to run elevated means that it won't run correctly for standard users.

+ __Your app requires a kernel-mode driver or a Windows service__. The bridge is suitable for an app, but it does not support a kernel-mode driver or a Windows service that needs to run under a system account. Instead of a Windows service, use a [background task](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Your app's modules are loaded in-process to processes that are not in your AppX package__. This isn't permitted, which means that in-process extensions, like [shell extensions](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), aren't supported. But if you have two apps in the same package, you can do inter-process communication between them.

+ __Your app calls [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) or [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)__. These functions are not currently supported for converted apps. We are working on adding support in a future release. As a workaround, you can copy any .dlls you were locating using these functions to your package root. 

+ __Your app uses a custom Application User Model ID (AUMID)__. If your process calls [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) to set its own AUMID, then it may only use the AUMID generated for it by the app model environment/AppX package. You can't define custom AUMIDs.

+ __Your app modifies the HKEY_LOCAL_MACHINE (HKLM) registry hive__. Any attempt by your app to create an HKLM key, or to open one for modification, will result in an access-denied failure. Remember that your app has its own private virtualized view of the registry, so the notion of a user- and machine-wide registry hive (which is what HKLM is) does not apply. You will need to find another way of achieving what you were using HKLM for, like writing to HKEY_CURRENT_USER (HKCU) instead.

+ __Your app uses a ddeexec registry subkey as a means of launching another app__. Instead, use one of the DelegateExecute verb handlers as configured by the various Activatable* extensions in your [app package manifest](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Your app writes to the AppData folder with the intention of sharing data with another app__. After conversion, AppData is redirected to the local app data store, which is a private store for each UWP app. Use a different means of inter-process data sharing. For more info, see [Store and retrieve settings and other app data](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Your app writes to the install directory for your app__. For example, your app writes to a log file that you put in the same directory as your exe. This isn't supported, so you'll need to find another location, like the local app data store.

+ __Your app installation requires user interaction__. Your app installer must be able to run silently, and it must install all of its prerequisites that aren't on by default on a clean OS image.

+ __Your app uses the Current Working Directory__. At runtime, your converted app won't get the same Working Directory that you previously specified in your desktop .LNK shortcut. You need to change your CWD at runtime if having the correct directory is important for your app to function correctly.

+ __Your app requires UIAccess__. If your application specifies `UIAccess=true` in the `requestedExecutionLevel` element of the UAC manifest, conversion to UWP isn't supported currently. For more info, see [UI Automation Security Overview](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Your app is already a full UWP app and wants to invoke a full trust process from within the app package__. Using the bridge in "reverse" is not supported and UWP apps attempting to invoke full trust processes will fail Store certification. To enable this scenario in a sideloaded app, first include the ```<desktop:Extension>``` declaration in your app's manifest (which includes the *EntryPoint* attribute with the value *Windows.FullTrustApplication*). Next, set your app's  ```<TargetDeviceFamily>``` to *Windows.Desktop*. This indicates your app is only targeting the desktop device family and not all UWP devices. If your app returns the error *APPX0501: Validation Error* with these settings in place, verify your target device family; your app is likely targeting the *Windows.Universal* family and not *Windows.Desktop*. 

+ __Your app exposes COM objects or GAC assemblies for use by other processes__. In the current release, your app cannot expose COM objects or GAC assemblies for use by processes originating from executables external to your AppX package. Processes from within the package can register and use COM objects and GAC assemblies as normal, but they will not be visible externally. This means interop scenarios like OLE will not function if invoked by external processes. 

+ __Your app is linking C runtime libraries in an unsupported manner__. The Microsoft C/C++ runtime library provides routines for programming for the Microsoft Windows operating system. These routines automate many common programming tasks that are not provided by the C and C++ languages. If your app utilizes C/C++ runtime library, you need to ensure it is linked in a supported manner. 
    
    Visual Studio 2015 supports both dynamic linking, to let your code use common DLL files, or static linking, to link the library directly into your code, to the current version of the CRT. If possible, we recommend your app use dynamic linking with VS 2015. 

    Support in previous versions of Visual Studio varies. See the following table for details: 

    <table>
    <th>Visual Studio version</td><th>Dynamic linking</th><th>Static linking</th></th>
    <tr><td>2005 (VC 8)</td><td>Not supported</td><td>Supported</td>
    <tr><td>2008 (VC 9)</td><td>Not supported</td><td>Supported</td>
    <tr><td>2010 (VC 10)</td><td>Supported</td><td>Supported</td>
    <tr><td>2012 (VC 11)</td><td>Supported</td><td>Not supported</td>
    <tr><td>2013 (VC 12)</td><td>Supported</td><td>Not supported</td>
    <tr><td>2015 (VC 14)</td><td>Supported</td><td>Supported</td>
    </table>
    
    Note: In all cases, you must link to the latest publically available CRT.

+ __Your app installs and loads assemblies from the Windows side-by-side folder__. For example, your app uses C runtimes libraries VC8 or VC9 and is dynamically linking them from Windows side-by-side folder, meaning your code is using the common DLL files from a shared folder. This is not supported. You will need to statically link them by linking to the redistributable library files directly into your code.

## Start the conversion process

You have a few different options for converting your app.

* **Desktop App Converter (DAC)**. The DAC is a tool that automatically converts and signs your app for you. Using the DAC is convenient and automatic, and it's useful if your app makes lots of system modifications or if there's any uncertainty about what your installer does. To get started with the DAC, see [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md). 

* **Manual Conversion**. If your app is installed using xcopy, or you're familiar with the changes that your installer makes to the system, manual conversion might be a more straightforward choice. This involves creating a manifest file, running the MakeAppx.exe tool, and then signing your app package. For details on how to manually convert, see [Manually convert your Windows desktop application to a Universal Windows Platform (UWP) app](desktop-to-uwp-manual-conversion.md). 

* **Third-Party Installer**. Three popular third-party installers — [InstallShield by Flexera](http://www.flexerasoftware.com/producer/products/software-installation/installshield-software-installer), [WiX by FireGiant](https://www.firegiant.com/r/appx), and [Advanced Installer by Caphyon](http://www.advancedinstaller.com/uwp-app-package) — now support the Desktop Bridge and can generate both a MSI installer and converted app package with only a few clicks. For more information, visit the respective website for each installer. 

## Get support and give feedback

If you run in to issues converting your app, you can visit the [forums](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop) for help. 

To give feedback or make feature suggestions, check out [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial). 

## In this section

| Topic | Description |
|-------|-------------|
| [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | Shows how to run Desktop App Converter. You probably won't need to do much, if anything, to get your app ready for the conversion process. |
| [Manually convert your desktop app](desktop-to-uwp-manual-conversion.md) | Learn how to create an app package and manifest to by hand. |
| [Converted desktop app extensions](desktop-to-uwp-extensions.md) | Enhance your converted app with extensions to enable features like startup tasks, File Explorer integration, and more. |
| [Supported UWP APIs for converted desktop apps](desktop-to-uwp-supported-api.md) | See what UWP APIs are available for your converted desktop app to use. |
| [Sign your converted desktop app](desktop-to-uwp-signing.md) | Learn how to sign your converted .appx package with a certificate. |
| [Deploy and debug your converted UWP app](desktop-to-uwp-deploy-and-debug.md) | Get help with deploying and debugging your app after converting it. Also, if you're curious about some of the internals of the Desktop Conversion extensions, this topic is for you. |
| [Desktop app bridge to UWP code samples](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | Code samples on GitHub demonstrating features of converted apps. |


<!--HONumber=Sep16_HO3-->


