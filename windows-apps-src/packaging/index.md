---
author: msatranjr
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Packaging apps
description: This section contains or links to articles about packaging for Universal Windows Platform (UWP) apps.
translationtype: Human Translation
ms.sourcegitcommit: f7eec4d8df2b62143c198fe507dffd936e2e325c
ms.openlocfilehash: 230c77cbe60631d643bebe905a92cdbeb76af4d2

---
# Packaging apps

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Purpose

This section contains or links to articles about packaging for Universal Windows Platform (UWP) apps.

| Topic | Description |
|-------|-------------|
| [Packaging UWP apps](packaging-uwp-apps.md) | To sell your UWP app or distribute it to other users, you need to create an appxupload package for it. When you create the appxupload, another appx package will be generated to use for testing and sideloading. You can distribute your app directly by sideloading the appx package to a device. This article describes the process of configuring, creating and testing a UWP app package. For more information about sideloading, see [Sideload Apps with DISM](http://go.microsoft.com/fwlink/?LinkID=231020). |
| [Install apps with the WinAppDeployCmd.exe tool](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows Application Deployment (WinAppDeployCmd.exe) is a command line tool that can use to deploy a UWP app from a Windows 10 machine to any Windows 10 Mobile device. You can use this tool to deploy an .appx package when the Windows 10 Mobile device is connected by USB or available on the same subnet without needing Microsoft Visual Studio or the solution for that app. This article describes how to install UWP apps using this tool. |
| [App capability declarations](app-capability-declarations.md) | Capabilities must be declared in your UWP app's [package manifest](https://msdn.microsoft.com/library/windows/apps/BR211474) to access certain API or resources like pictures, music, or devices like the camera or the microphone. |
| [Download and install package updates for your app](self-install-package-updates.md) | Your UWP app can programmatically check for package updates and install the updates. Your app can also query for packages that have been marked as mandatory on the Windows Dev Center dashboard and disable functionality until the mandatory update is installed. This article describes how to perform these tasks. |
 



<!--HONumber=Aug16_HO5-->


