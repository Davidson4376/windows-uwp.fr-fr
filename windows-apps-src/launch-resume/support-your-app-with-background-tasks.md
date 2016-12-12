---
author: TylerMSFT
title: Support your app with background tasks
description: The topics in this section show you how to make lightweight code run in the background in response to triggers.
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
translationtype: Human Translation
ms.sourcegitcommit: 7208ca16fe7eee2ec4039f3c4bc6305dceac96f3
ms.openlocfilehash: b33fd118289ca575207be97bd8a1a33ddcc49a87

---

# <a name="support-your-app-with-background-tasks"></a>Support your app with background tasks

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

The topics in this section show you how to make lightweight code run in the background in response to triggers. You can use background tasks to provide functionality when your app is suspended or not running. You can also use background tasks for real-time communication apps like VOIP, mail, and IM.

## <a name="playing-media-in-the-background"></a>Playing media in the background

Starting in Windows 10, version 1607, playing audio in the background is much easier. See [Play media in the background](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio) for more information.

## <a name="in-process-and-out-of-process-background-tasks"></a>In-process and out-of-process background tasks

There are two approaches to implementing background tasks: in-process, in which the app and its background process run in the same process; and out-of-process, where the app and the background process run in separate processes. In-process background support was introduced in Windows 10, version 1607, to simplify writing background tasks. But you can still write out-of-process background tasks. See [Guidelines for background tasks](guidelines-for-background-tasks.md) for recommendations on when to write an in-process versus out-of-process background task.

Out-of-process background tasks are more resilient because your background process can't bring down your app process if something goes wrong. But the resiliency comes at the price of greater complexity to manage the cross-process communication.

Out-of-process background tasks are implemented as lightweight classes that the OS runs in a separate process (backgroundtaskhost.exe). Out-of-process background tasks are classes you write that implement the [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) interface. You register a background task by using the [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) class. The class name is used to specify the entry point when you registering the background task.

In Windows 10, version 1607, you can enable background activity without creating a background task. You can instead run your background code directly inside the foreground application.

To get started quickly with in-process background tasks, see [Create and register an in-process background task](create-and-register-an-inproc-background-task.md).

To get started quickly with out-of-process background tasks, see [Create and register an out-of-process background task](create-and-register-an-outofproc-background-task.md).

> [!TIP]
> Starting with Windows 10, you no longer need to place an app on the lock screen as a prerequisite for registering a background task for it.

## <a name="background-tasks-for-system-events"></a>Background tasks for system events

Your app can respond to system-generated events by registering a background task with the [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) class. An app can use any of the following system event triggers (defined in [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839))

| Trigger name                     | Description                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | The Internet becomes available.                                                                                |
| **NetworkStateChange**           | A network change such as a change in cost or connectivity occurs.                                              |
| **OnlineIdConnectedStateChange** | Online ID associated with the account changes.                                                                 |
| **SmsReceived**                  | A new SMS message is received by an installed mobile broadband device.                                         |
| **TimeZoneChange**               | The time zone changes on the device (for example, when the system adjusts the clock for daylight saving time). |

For more info see [Respond to system events with background tasks](respond-to-system-events-with-background-tasks.md).

## <a name="conditions-for-background-tasks"></a>Conditions for background tasks

You can control when the background task runs, even after it is triggered, by adding a condition. Once triggered, a background task will not run until all of its conditions are met. The following conditions (represented by the [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) enumeration) can be used.

| Condition name           | Description                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | The Internet must be available.   |
| **InternetNotAvailable** | The Internet must be unavailable. |
| **SessionConnected**     | The session must be connected.    |
| **SessionDisconnected**  | The session must be disconnected. |
| **UserNotPresent**       | The user must be away.            |
| **UserPresent**          | The user must be present.         |

 
For more info see [Set conditions for running a background task](set-conditions-for-running-a-background-task.md).

## <a name="application-manifest-requirements"></a>Application manifest requirements

Before your app can successfully register a background task that runs out-of-process, it must be declared in the application manifest. Background tasks that run in the same process as their host app do not need to be declared in the application manifest. For more info see [Declare background tasks in the application manifest](declare-background-tasks-in-the-application-manifest.md).

## <a name="background-tasks"></a>Background tasks

The following real-time triggers can be used to run lightweight custom code in the background:

| Real-time trigger  | Description |
|--------------------|-------------|
| **Control Channel** | Background tasks can keep a connection alive, and receive messages on the control channel, by using the [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). If your app is listening to a socket, you can use the Socket Broker instead of the **ControlChannelTrigger**. For more details on using the Socket Broker, see [SocketActivityTrigger](https://msdn.microsoft.com/library/windows/apps/dn806009). The **ControlChannelTrigger** is not supported on Windows Phone. |
| **Timer** | Background tasks can run as frequently as every 15 minutes, and they can be set to run at a certain time by using the [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). For more info see [Run a background task on a timer](run-a-background-task-on-a-timer-.md). |
| **Push Notification** | Background tasks respond to the [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) to receive raw push notifications. |

**Note**  

Universal Windows apps must call [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) before registering any of the background trigger types.

To ensure that your Universal Windows app continues to run properly after you release an update, call [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) and then call [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) when your app launches after being updated. For more information, see [Guidelines for background tasks](guidelines-for-background-tasks.md).

## <a name="system-event-triggers"></a>System event triggers

The [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) enumeration represents the following system event triggers:

| Trigger name            | Description                                                       |
|-------------------------|-------------------------------------------------------------------|
| **UserPresent**         | The background task is triggered when the user becomes present.   |
| **UserAway**            | The background task is triggered when the user becomes absent.    |
| **ControlChannelReset** | The background task is triggered when a control channel is reset. |
| **SessionConnected**    | The background task is triggered when the session is connected.   |

   
The following system event triggers signal when the user has moved an app on or off the lock screen.

| Trigger name                     | Description                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded**   | An app tile is added to the lock screen.     |
| **LockScreenApplicationRemoved** | An app tile is removed from the lock screen. |

 
## <a name="background-task-resource-constraints"></a>Background task resource constraints

Background tasks are lightweight. Keeping background execution to a minimum ensures the best user experience with foreground apps and battery life. This is enforced by applying resource constraints to background tasks.

Background tasks are limited to 30 seconds of wall-clock usage.

### <a name="memory-constraints"></a>Memory constraints

Due to the resource constraints for low-memory devices, background tasks may have a memory limit that determines the maximum amount of memory the background task can use. If your background task attempts an operation that would exceed this limit, the operation will fail and may generate an out-of-memory exception--which the task can handle. If the task does not handle the out-of-memory exception, or the nature of the attempted operation is such that an out-of-memory exception was not generated, then the task will be terminated immediately.  
 You can use the [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) APIs to query your current memory usage and limit in order to discover your cap (if any), and to monitor your background task's ongoing memory usage.

### <a name="per-device-limit-for-apps-with-background-tasks-for-low-memory-devices"></a>Per-device limit for apps with background tasks for low-memory devices

On memory-constrained devices, there is a limit to the number of apps that can be installed on a device and use background tasks at any given time. If this number is exceeded, the call to [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485), which is required to register all background tasks, will fail.

### <a name="battery-saver"></a>Battery Saver

Unless you exempt your app so that it can still run background tasks and receive push notifications when Battery Saver is on, the Battery Saver feature, when enabled, will prevent background tasks from running when the device is not connected to external power and the battery goes below a specified amount of power remaining. This will not prevent you from registering background tasks.

## <a name="background-task-resource-guarantees-for-real-time-communication"></a>Background task resource guarantees for real-time communication

To prevent resource quotas from interfering with real-time communication functionality, background tasks using the [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) and [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) receive guaranteed CPU resource quotas for every running task. The resource quotas are as mentioned above, and remain constant for these background tasks.

Your app doesn't have to do anything differently to get the guaranteed resource quotas for [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) and [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) background tasks. The system always treats these as critical background tasks.

## <a name="maintenance-trigger"></a>Maintenance trigger

Maintenance tasks only run when the device is plugged in to AC power. For more info see [Use a maintenance trigger](use-a-maintenance-trigger.md).

## <a name="background-tasks-for-sensors-and-devices"></a>Background tasks for sensors and devices

Your app can access sensors and peripheral devices from a background task with the [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) class. You can use this trigger for long-running operations such as data synchronization or monitoring. Unlike tasks for system events, a **DeviceUseTrigger** task can only be triggered while your app is running in the foreground and no conditions can be set on it.

> [!IMPORTANT]
> The **DeviceUseTrigger** and **DeviceServicingTrigger** cannot be used with in-process background tasks.

Some critical device operations, such as long running firmware updates, cannot be performed with the [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Such operations can be performed only on the PC, and only by a privileged app that uses the [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315). A *privileged app* is an app that the device's manufacturer has authorized to perform those operations. Device metadata is used to specify which app, if any, has been designated as the privileged app for a device. For more info, see [Device sync and update for Windows Store device apps](http://go.microsoft.com/fwlink/p/?LinkId=306619)

## <a name="managing-background-tasks"></a>Managing background tasks

Background tasks can report progress, completion, and cancellation to your app using events and local storage. Your app can also catch exceptions thrown by a background task, and manage background task registration during app updates. For more info see:

[Handle a cancelled background task](handle-a-cancelled-background-task.md)  
[Monitor background task progress and completion](monitor-background-task-progress-and-completion.md)

**Note**  
This article is for Windows 10 developers writing Universal Windows Platform (UWP) apps. If you’re developing for Windows 8.x or Windows Phone 8.x, see the [archived documentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 ## <a name="related-topics"></a>Related topics

**Conceptual guidance for multitasking in Windows 10**

* [Launching, resuming, and multitasking](index.md)

**Related background task guidance**

* [Play media in the background](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio)
* [Access sensors and devices from a background task](access-sensors-and-devices-from-a-background-task.md)
* [Guidelines for background tasks](guidelines-for-background-tasks.md)
* [Create and register an out-of-process background task](create-and-register-an-outofproc-background-task.md)
* [Create and register an in-process background task](create-and-register-an-inproc-background-task.md)
* [Debug a background task](debug-a-background-task.md)
* [Declare background tasks in the application manifest](declare-background-tasks-in-the-application-manifest.md)
* [Handle a cancelled background task](handle-a-cancelled-background-task.md)
* [Monitor background task progress and completion](monitor-background-task-progress-and-completion.md)
* [Register a background task](register-a-background-task.md)
* [Respond to system events with background tasks](respond-to-system-events-with-background-tasks.md)
* [Run a background task on a timer](run-a-background-task-on-a-timer-.md)
* [Set conditions for running a background task](set-conditions-for-running-a-background-task.md)
* [Update a live tile from a background task](update-a-live-tile-from-a-background-task.md)
* [Use a maintenance trigger](use-a-maintenance-trigger.md)
* [How to trigger suspend, resume, and background events in Windows Store apps (when debugging)](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [Device sync and update for Windows Store device apps](http://go.microsoft.com/fwlink/p/?LinkId=306619)



<!--HONumber=Dec16_HO1-->


