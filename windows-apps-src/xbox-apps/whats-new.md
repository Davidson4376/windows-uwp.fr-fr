---
author: v-angraf
title: What's new for UWP on Xbox One
description: Highlights new features for UWP apps on Xbox One.
translationtype: Human Translation
ms.sourcegitcommit: 044aac722180015586487dcc8738facccf209f5c
ms.openlocfilehash: 4cc1e0b495a80e019296b9c3be9e75a37c60224a

---

# What's new for developers in the latest update of UWP on Xbox One

The July 2016 release of Universal Windows Platform (UWP) on Xbox One contains the following new features, updates to existing features, and bug fixes.

## Networking using TCP/UDP sockets is now available  
Inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is now available.

## Fiddler support
You can now enable Fiddler as a proxy for a console that has enabled UWP on Xbox One. Fiddler allows you to log and inspect all HTTP/HTTPS traffic to and from Xbox services and relying-party web services. For more information, see [How to use Fiddler with Xbox One when developing for UWP](uwp-fiddler.md).

## Mouse mode is now enabled by default
Mouse mode is now enabled by default for XAML and Hosted Web Apps.
We strongly recommend that you turn this off and optimize for directional controller navigation.
To learn how to turn mouse mode off, see [How to disable mouse mode](how-to-disable-mouse-mode.md).
For more information about how to build great apps for Xbox, see [Designing for Xbox and TV](../input-and-devices/designing-for-tv.md#mouse-mode).

## Extended UWP API surface area is now functional on the console
Additional UWP APIs are now functional on the Xbox console. For more information about UWP API support, see [UWP features that aren't yet supported on Xbox](http://go.microsoft.com/fwlink/p/?LinkID=760755). 

## Background music and audio capabilities
You can now play music and audio from an app that is running in the background.

## XAML improvements
The following improvements have been made to the XAML platform:
-   The focus rectangle is now styled for a television 10-foot experience.
-   Xbox sounds are now embedded in the XAML platform.
-   XY focus navigation between UI elements has been improved. 

## You can now change the size of allocated developer storage on the console
A new setting in the Dev Home app allows you to increase or decrease the size of the allocated developer storage on your console. For more information about changing the size of your allocated developer storage, see [Introduction to Xbox One tools](introduction-to-xbox-tools.md).

## WDP tool enhancements
The following improvements have been made to the Windows Device Portal (WDP) Tool for Xbox:
 - The tool includes additional console settings. For more information about console settings, see the [/ext/settings](wdp-xboxsettings-api.md) reference topic. 
 - Users can be signed in and out on the console. For more information about users, see the [/ext/user](wdp-user-management.md) reference topic.
 - You can now capture a screenshot of the console. For more information about taking a screenshot, see the [/ext/screenshot](wdp-media-capture-api.md) reference topic.
 - The tool can deploy a loose file build of your app. For more information about loose file builds, see the [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) reference topic.
 - Developer files on your console can be accessed from File Explorer on your development PC. For more information about accessing files through File Explorer, see the [/ext/smb/developerfolder](wdp-smb-api.md) reference topic.

## See also
- [Known issues](known-issues.md)
- [UWP on Xbox One](index.md)



<!--HONumber=Aug16_HO3-->


