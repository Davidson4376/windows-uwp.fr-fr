---
author: mcleblanc
title: Support your app with background tasks
description: The topics in this section show how to run your own lightweight code in the background by responding to triggers with background tasks.
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
---

# Support your app with background tasks


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


The topics in this section show how to run your own lightweight code in the background by responding to triggers with background tasks. Background tasks are lightweight classes that the OS runs in the background. You can use background tasks to provide functionality when your app is suspended or not running. You can also use background tasks for real-time communication apps like VOIP, mail, and IM.

Background tasks are separate classes that implement the [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) interface. You register a background task by using the [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) class. The class name is used to specify the entry point when you registering the background task.

To get started quickly with a background task, see [Create and register a background task](create-and-register-a-background-task.md).

**Tip**  Starting with Windows 10, you no longer need to place an app on the lock screen in order to register background tasks.

 

## Background tasks for system events


Your app can respond to system-generated events by registering a background task with the [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) class. An app can use any of the following system event triggers (defined in [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839))

| Trigger name                     | Description                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | The Internet becomes available.                                                                                |
| **NetworkStateChange**           | A network change such as a change in cost or connectivity occurs.                                              |
| **OnlineIdConnectedStateChange** | Online ID associated with the account changes.                                                                 |
| **SmsReceived**                  | A new SMS message is received by an installed mobile broadband device.                                         |
| **TimeZoneChange**               | The time zone changes on the device (for example, when the system adjusts the clock for daylight saving time). |

 

For more info see [Respond to system events with background tasks](respond-to-system-events-with-background-tasks.md).

## Conditions for background tasks


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

## Application manifest requirements


Before your app can successfully register a background task, it must be declared in the application manifest. For more info see [Declare background tasks in the application manifest](declare-background-tasks-in-the-application-manifest.md).

## Background tasks


The following real-time triggers can be used to run lightweight custom code in the background:

**Control Channel:  **Background tasks can keep a connection alive, and receive messages on the control channel, by using the [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). If your app is listening to a socket, you can use the Socket Broker instead of the **ControlChannelTrigger**. For more details on using the Socket Broker, see [SocketActivityTrigger](https://msdn.microsoft.com/library/windows/apps/dn806009). The **ControlChannelTrigger** is not supported on Windows Phone.

**Timer:  **Background tasks can run as frequently as every 15 minutes, and they can be set to run at a certain time by using the [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). For more info see [Run a background task on a timer](run-a-background-task-on-a-timer-.md).

**Push Notification:  **Background tasks respond to the [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) to receive raw push notifications.

**Note**  

Universal Windows apps must call [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) before registering any of the background trigger types.

To ensure that your Universal Windows app continues to run properly after you release an update, you must call [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) and then call [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) when your app launches after being updated. For more information, see [Guidelines for background tasks](guidelines-for-background-tasks.md).

## System event triggers


> **Note**  The [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) enumeration includes the following system event triggers.

| Trigger name            | Description                                                       |
|-------------------------|-------------------------------------------------------------------|
| **UserPresent**         | The background task is triggered when the user becomes present.   |
| **UserAway**            | The background task is triggered when the user becomes absent.    |
| **ControlChannelReset** | The background task is triggered when a control channel is reset. |
| **SessionConnected**    | The background task is triggered when the session is connected.   |

 

The following system event triggers make it possible to recognize when the user has moved an app on or off the lock screen.

| Trigger name                     | Description                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded**   | An app tile is added to the lock screen.     |
| **LockScreenApplicationRemoved** | An app tile is removed from the lock screen. |

 
## Background task resource constraints


Background tasks are lightweight. Keeping background execution to a minimum ensures the best user experience with foreground apps and battery life. This is enforced by applying resource constraints to background tasks:

-   Background tasks are limited to 30 seconds of wall-clock usage.

## Additional background task resource constraints


### Memory constraints

Due to the resource constraints for low-memory devices, background tasks may have a memory limit that determines the maximum amount of memory the background task can use. If your background task attempts an operation that would exceed this limit, the operation will fail and may generate an out-of-memory exception that the task can handle. If the task does not handle the out-of-memory exception, or the nature of the attempted operation is such that an out-of-memory exception was not generated, then the task will be terminated immediately. You can use the [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) APIs to query your current memory usage and limit in order to discover your cap (if any), and to monitor your background task's ongoing memory usage.

### Per-device limit for apps with background tasks for low-memory devices

On memory-constrained devices, there is a limit to the number of apps that can be installed on a device and use background tasks at any given time. If this number is exceeded, the call to [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485), which is required to register all background tasks, will fail.

### Battery Saver

Unless you exempt your app so that it can still run background tasks and receive push notifications when Battery Saver is on, the Battery Saver feature, when enabled, will prevent background tasks from running when the device is not connected to external power and the battery goes below a specified amount of power remaining. This will not prevent you from registering background tasks.

## Background task resource guarantees for real-time communication


To prevent resource quotas from interfering with real-time communication functionality, background tasks using the [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) and [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) receive guaranteed CPU resource quotas for every running task. The resource quotas are as mentioned above, and remain constant for these background tasks.

Your app doesn't have to do anything differently to get the guaranteed resource quotas for [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) and [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) background tasks. The system always treats these as critical background tasks.

## Maintenance trigger


Maintenance tasks only run when the device is plugged in to AC power. For more info see [Use a maintenance trigger](use-a-maintenance-trigger.md).

## Background task for sensors and devices


Your app can access sensors and peripheral devices from a background task with the [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) class. You can use this trigger for long-running operations such as data synchronization or monitoring. Unlike tasks for system events, a **DeviceUseTrigger** task can only be triggered while your app is running in the foreground and no conditions can be set on it.

Some critical device operations, such as long running firmware updates, cannot be performed with the [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Such operations can be performed only on the PC, and only by a privileged app that uses the [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315). A *privileged app* is an app that the device's manufacturer has authorized to perform those operations. Device metadata is used to specify which app, if any, has been designated as the privileged app for a device. For more info, see [Device sync and update for Windows Store device apps](http://go.microsoft.com/fwlink/p/?LinkId=306619)

## Managing background tasks


Background tasks can report progress, completion, and cancellation to your app using events and local storage. Your app can also catch exceptions thrown by a background task, and manage background task registration during app updates. For more info see:

[Handle a cancelled background task](handle-a-cancelled-background-task.md)

[Monitor background task progress and completion](monitor-background-task-progress-and-completion.md)

**Note**  
This article is for Windows 10 developers writing Universal Windows Platform (UWP) apps. If you’re developing for Windows 8.x or Windows Phone 8.x, see the [archived documentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Related topics


**Conceptual guidance for multitasking in Windows 10**

* [Launching, resuming, and multitasking](index.md)

**Related background task guidance**

* [Access sensors and devices from a background task](access-sensors-and-devices-from-a-background-task.md)
* [Guidelines for background tasks](guidelines-for-background-tasks.md)
* [Create and register a background task](create-and-register-a-background-task.md)
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

 

 



