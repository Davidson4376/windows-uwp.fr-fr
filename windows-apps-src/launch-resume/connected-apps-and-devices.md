---
author: TylerMSFT
title: Connected apps and devices (Project "Rome")
description: This section describes how to use the Remote Systems platform to discover remote devices, launch an app on a remote device, and communicate with an app service on a remote device.
translationtype: Human Translation
ms.sourcegitcommit: 871599217e6da0eb0febd140323e99db7d1258cb
ms.openlocfilehash: 2dbce34aeaf4933eee22e1e8ef40c48e633f6f7e

---

# <a name="connected-apps-and-devices-project-rome"></a>Connected apps and devices (Project "Rome")

This section explains how to connect apps across devices and platforms using Project "Rome." Learn how to discover remote devices, launch an app on a remote device, and communicate with an app service on a remote device.

Most people have multiple devices and often begin an activity on one device and finish it on another. To accommodate this, apps need to span devices and platforms.

The [Remote Systems APIs](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems) introduced in Windows 10, version 1607, enable you to write apps that allow users to start a task on one device and finish it on another. The task remains the central focus, and users can do their work on the device that is most convenient. For example, you might be listening to the radio on your phone in the car, but when you get home you may want to transfer playback to your Xbox One that is hooked up to your home stereo system.

You can also use Project "Rome" for companion devices, or remote control scenarios. Use the app messaging APIs to create an app channel between two devices to send and receive custom messages. For example, you can write an app for your phone that controls playback on your TV, or a companion app that provides information about the characters on a TV show you are watching on another app.  

Devices can be connected proximally through Bluetooth and wireless, or remotely through the cloud, and are connected by the Microsoft account of the person using them.

See the [Remote Systems sample](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) for examples of how to discover remote system, launch an app on a remote system, and use app services to send messages between apps running on two systems.

| Topic | Description |
|-------|-------------|
| [Discover remote devices](discover-remote-devices.md)  | Learn how to discover devices that you can connect to. |
| [Launch an app on a remote device](launch-a-remote-app.md) | Learn how to launch an app on a remote device.  |
| [Communicate with a remote app service](communicate-with-a-remote-app-service.md) | Learn how to interact with an app on a remote device. |



<!--HONumber=Dec16_HO1-->


