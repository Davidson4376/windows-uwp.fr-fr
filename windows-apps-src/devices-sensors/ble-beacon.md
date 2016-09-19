---
author: msatranjr
title: Bluetooth Advertisements
description: This section contains articles on how to integrate Bluetooth Low Energy (LE) Advertisements into Universal Windows Platform (UWP) apps through the user of AdvertisementWatcher and AdvertisementPublisher APIs.
translationtype: Human Translation
ms.sourcegitcommit: 62e97bdb8feb78981244c54c76a00910a8442532
ms.openlocfilehash: c8210fd07cdea4095d1f989fe5f55cda529d8de1

---

# Bluetooth Advertisements

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** Important APIs ** 

-   [**Windows.Devices.Bluetooth.Advertisement**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.aspx)

This article provides an overview of Bluetooth Advertisements (Beacons) for Universal Windows Platform (UWP) apps.  

## Overview

There are two main functions that a developer can perform using the Advertisement APIs:

-   [Advertisement Watcher](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.aspx): listen for nearby beacons and filter them out based on payload or proximity.  
-   [Advertisement Publisher](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher.aspx): define a payload for Windows to advertise on a developers behalf.  

Full sample code is found in the [Bluetooth Advertisement Sample](http://go.microsoft.com/fwlink/p/?LinkId=619990) on Github



<!--HONumber=Aug16_HO3-->


