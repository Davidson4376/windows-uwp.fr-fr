---
author: jnHs
Description: Follow these guidelines to prepare your app's packages for submission to the Windows Store.
title: App package requirements
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
translationtype: Human Translation
ms.sourcegitcommit: c15d4153f6ae83cc7bf1ae02d834bd07189e38ab
ms.openlocfilehash: 250e94c2766227cabad791db6d994bcfb1a2ac33

---

# App package requirements

Follow these guidelines to prepare your app's packages for submission to the Windows Store.

## Before you build your app's package for the Windows Store

Make sure to [test your app with the Windows App Certification Kit](https://msdn.microsoft.com/library/windows/apps/mt186449). We also recommend that you test your app on different types of hardware. Note that until we certify your app and make it available from the Windows Store, it can only be installed and run on computers that have developer licenses.

## Building the app package using Microsoft Visual Studio

If you're using Microsoft Visual Studio as your development environment, you already have built-in tools that make creating an app package a quick and easy process. For more info, see [Packaging apps](https://msdn.microsoft.com/library/windows/apps/mt270969).

> **Note**  Be sure that all your filenames use ANSI. 


When you create your package in Visual Studio, make sure you are signed in with the same account associated with your developer account. Some parts of the package manifest have specific details related to your account. This info is detected and added automatically.

When you build your app's packages, Visual Studio can create an .appx file or an .appxupload file (or a .xap file for Windows Phone 8.1 and earlier). For apps that target Windows 10, always upload the .appxupload file in the [Packages](upload-app-packages.md) page. For more info about packaging UWP apps for the Store, see [Packaging Universal Windows apps for Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

Your app's packages don't have to be signed with a certificate rooted in a trusted certificate authority.

### App bundles

For apps that target Windows 8.1, Windows Phone 8.1, and later, Visual Studio can generate an app bundle (.appxbundle) to reduce the size of the app that users download. This can be helpful if you've defined language-specific assets, a variety of image-scale assets, or resources that apply to specific versions of Microsoft DirectX.

> **Note**  One app bundle can contain your packages for all architectures. You should submit only one bundle for each targeted OS.


With an app bundle, a user will only download the relevant files, rather than all possible resources. For more info about app bundles, see [Packaging apps](https://msdn.microsoft.com/library/windows/apps/mt270969) and [Packaging Universal Windows apps for Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

## Building the app package manually

If you don't use Visual Studio to create your package, you must [create your package manifest manually](https://msdn.microsoft.com/library/windows/apps/br211476).

Be sure to review the [App package manifest](https://msdn.microsoft.com/library/windows/apps/br211474) documentation for complete manifest details and requirements. Your manifest must follow the package manifest schema in order to pass certification.

Your manifest must include some specific info about your account and your app. You can find this info by looking at [View app identity details](view-app-identity-details.md) in the **App management** section of your app's overview page in the dashboard.

> **Note**  Values in the manifest are case-sensitive. Spaces and other punctuation must also match. Enter the values carefully and review them to ensure that they are correct.


App bundles use a different manifest. Review the [Bundle manifest](https://msdn.microsoft.com/library/windows/apps/dn263089) documentation for the details and requirements for app bundle manifests.

> **Tip**  Be sure to run the [Windows App Certification Kit](https://msdn.microsoft.com/library/windows/apps/mt186449) before you submit your packages. This can you help determine if your manifest has any problems that might cause certification or submission failures.


If your app has more than one package, these app manifest elements must be the same in each package (per targeted OS):

-   [**Package/Capabilities**](https://msdn.microsoft.com/library/windows/apps/br211422)
-   [**Package/Dependencies**](https://msdn.microsoft.com/library/windows/apps/br211428)
-   [**Package/Resources**](https://msdn.microsoft.com/library/windows/apps/br211462)

## Package format requirements

Your app’s packages must comply with these requirements.

| App package property | Requirement                                                          |
|----------------------|----------------------------------------------------------------------|
| Package size         | .appxbundle: 25 GB maximum per bundle <br>.appx packages targeting Windows 8.1: 8 GB maximum per package <br> .appx packages targeting Windows 8: 2 GB maximum per package <br> .appx packages targeting Windows Phone 8.1: 4 GB maximum per package <br> .xap packages: 1 GB maximum per package                                                                           |
| Block map hashes     | SHA2-256 algorithm                                                   |
 

## StoreManifest XML file

StoreManifest.xml is an optional configuration file that may be included in app packages. Its purpose is to enable features, such as declaring your app as a Windows Store device app or declaring requirements that a package depends on to be applicable to a device, that the package manifest does not cover. StoreManifest.xml is submitted with the app package and must be in the root folder of your app's main project. For more info, see [StoreManifest schema](https://msdn.microsoft.com/library/windows/apps/mt617325).

 

 







<!--HONumber=Aug16_HO3-->


