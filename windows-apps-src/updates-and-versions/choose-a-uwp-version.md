---
author: QuinnRadich
title: Choose a UWP version
description: When writing a UWP app in Microsoft Visual Studio, you can choose which version to target. Learn about the difference between different UWP versions, and how to configure your choices in new and existing projects.
translationtype: Human Translation
ms.sourcegitcommit: 006b5d01c2474591a81e4d7a83c5735dc0b3d9d8
ms.openlocfilehash: 5d05c427ecc1ec57856b7c3909be50c3d87daa28

---

# <a name="choose-a-uwp-version"></a>Choose a UWP version

When writing a UWP app in Microsoft Visual Studio, you can choose which version to target. Currently, there are only three possible versions.

| Version | Description |
| --- | --- |
| Build 14393 (Anniversary Edition) | This is the most recent version of Windows 10, released in July 2016. Some highlighted features from this release include: </br> \* **Windows Ink:** New InkCanvas and InkToolbar controls. </br> \* **Cortana APIs:** Use new Cortana Actions to integrate Cortana support with specific functions of your app. </br> \* **Windows Hello:** Microsoft Edge now supports Windows Hello, giving web developers access to biometric authentication. </br> For information on these and many other features added in this release of windows, visit [the Dev Center](https://developer.microsoft.com/en-us/windows/windows-10-for-developers) or the more in-depth page on [What's new in Windows 10 for developers](../whats-new/windows-10-version-1607.md)  |
| Build 10586 | This version of Windows 10 was released in November 2015. Highlighted features include the introduction of ORTC (object real-time communications) APIs for video communication in Microsoft Edge and Providers APIs to enable apps to use Windows Hello face authentication. [More information on features introduced in this build.](../whats-new/windows-10-version-1511.md) |
| Build 10240 | This is the initial release version of Windows 10, from July 2015. [More information on features introduced in this build.](../whats-new/windows-10-version-1507.md) |

We highly recommend that new developers and developers writing code for a general audience always use the latest build of Windows (14393). Developers writing Enterprise apps should strongly consider supporting an older **Minimum Version**.

## <a name="whats-different-in-each-uwp-version"></a>What's different in each UWP version?

New and changed APIs for UWP are available in every successive version of Windows 10. For specific information about what features were added in which version, see [What's new for developers in Windows 10](../whats-new/windows-10-version-1607.md).

For reference topics that enumerate all device families and their versions, and all API contracts and their versions, see [Device families](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) and [API contracts](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## <a name="choose-which-version-to-use-for-your-app"></a>Choose which version to use for your app

In the **New Universal Windows Project** dialog in Visual Studio, you can choose a version for **Target Version** and for **Minimum Version**.

* **Target Version**. This sets the *TargetPlatformVersion* setting in your project file. It also determines the value of the *TargetDeviceFamily@MaxVersionTested* attribute in your app package manifest. The value you choose specifies the version of the UWP platform that your project is targeting—and therefore the set of APIs available to your app—so we recommend that you choose the most recent version possible. For more info about your app package manifest, and some guidelines around configuring TargetDeviceFamily manually, see [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Minimum Version**. This sets the *TargetPlatformMinVersion* setting in your project file. It also determines the value of the *TargetDeviceFamily@MinVersion* attribute in your app package manifest. The value you choose specifies the minimum version of the UWP platform that your project can work with.

Be aware that you're declaring that your app works on any version of Windows in the range from **Minimum Version** to **Target Version**. If those two are the same version then you don't need to do anything special. If they're different then here are some things to be aware of.

* In your code, you can freely (that is, without conditional checks) call any API that exists in the version specified by **Minimum Version**.
* Ensure that you test your code on a device running the **Minimum Version**, to be sure that it works without requiring APIs only present in the **Target Version**.
* The value of **Target Version** is used to identify all the references (contract winmds) used to compile your project. But those references will enable you to compile your code with calls to APIs that won't necessarily exist on devices that you've declared that you support (via **Minimum Version**). Therefore, any API that was introduced after **Minimum Version** will need to be called via adaptive code. For more information about adaptive code, see [Guide to Universal Windows Platform (UWP) apps](../get-started/universal-application-platform-guide.md).



<!--HONumber=Dec16_HO1-->


