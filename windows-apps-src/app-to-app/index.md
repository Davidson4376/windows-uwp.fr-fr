---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: This section explains how to share data between Universal Windows Platform (UWP) apps, including how to use the Share contract, copy and paste, and drag and drop.
title: App-to-app communication
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: 94e1586a73743e8918ef160897b1b22c8c545ea0
ms.openlocfilehash: 05ac668e0e3c33f6dd9da9f578335bab96c6429c

---

# App-to-app communication

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

This section explains how to share data between Universal Windows Platform (UWP) apps, including how to use the Share contract, copy and paste, and drag and drop.

The Share contract is one way users can quickly exchange data between apps. For example, a user might want to share a webpage with their friends using a social networking app, or save a link in a notes app to refer to later. Consider using a Share contract if your app receives content in scenarios that a user can quickly complete while in the context of another app.

An app can support the Share feature in two ways. First, it can be a source app that provides content that the user wants to share. Second, the app can be a target app that the user selects as the destination for shared content. An app can also be both a source app and a target app. If you want your app to share content as a source app, you need to decide what data formats your app can provide.

In addition to the Share contract, apps can also integrate classic techniques for transferring data, such as dragging and dropping or copy and pasting. In addition to communication between UWP apps, these methods also support sharing to and from desktop applications.

## In this section

| Topic | Description |
|-------|-------------|
| [Share data](share-data.md) | This article explains how to support the Share contract in a UWP app. The Share contract is an easy way to quickly share data, such as text, links, photos, and videos, between apps. For example, a user might want to share a webpage with their friends using a social networking app, or save a link in a notes app to refer to later. |
| [Receive data](receive-data.md) | This article explains how to receive content in your UWP app shared from another app using Share contract. This Share contract allows your app to be presented as an option when the user invokes Share. |
| [Copy and paste](copy-and-paste.md) | This article explains how to support copy and paste in UWP apps using the clipboard. Copy and paste is the classic way to exchange data either between apps, or within an app, and almost every app can support clipboard operations to some degree. |
| [Drag and drop](drag-and-drop.md) | This article explains how to add dragging and dropping in your UWP app. Drag and drop is a classic, natural way of interacting with content such as images and files. Once implemented, drag and drop works seamlessly in all directions, including app-to-app, app-to-desktop, and desktop-to app. |

## See also
- [Develop UWP apps](https://developer.microsoft.com/en-us/windows/develop)



<!--HONumber=Aug16_HO5-->


