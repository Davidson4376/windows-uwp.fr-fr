---
author: msatranjr
title: Bluetooth Advertisements
description: This section contains articles on how to integrate Bluetooth Low Energy (LE) Advertisements into Universal Windows Platform (UWP) apps through the user of AdvertisementWatcher and AdvertisementPublisher APIs.
translationtype: Human Translation
ms.sourcegitcommit: b1493d3d0d61a5fc45ab563b56bffa43650bbed9
ms.openlocfilehash: feda9b20b4cbc265832bdb51f90546d9e1f668e8

---

# <a name="bluetooth-le-advertisements"></a>Bluetooth LE Advertisements

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Important APIs**

-   [**Windows.Devices.Bluetooth.Advertisement**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.aspx)

This article provides an overview of Bluetooth Low Energy (LE) Advertisement beacons for Universal Windows Platform (UWP) apps.  

## <a name="overview"></a>Overview

There are two main functions that a developer can perform using the LE Advertisement APIs:

-   [Advertisement Watcher](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.aspx): listen for nearby beacons and filter them out based on payload or proximity.  
-   [Advertisement Publisher](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher.aspx): define a payload for Windows to advertise on a developers behalf.  

Full sample code is found in the [Bluetooth Advertisement Sample](http://go.microsoft.com/fwlink/p/?LinkId=619990) on Github

## <a name="basic-setup"></a>Basic Setup

To use basic Bluetooth LE functionality in a Universal Windows Platform app, you must check the Bluetooth capability in the Package.appxmanifest.

1. Open Package.appxmanifest
2. Go to the Capabilities tab
3. Find Bluetooth in the list on the left and check the box next to it.

## <a name="publishing-advertisements"></a>Publishing Advertisements

Bluetooth LE Advertisements allow your device to constantly beacon out a specific payload, called an advertisement. This advertisement can be seen by any nearby Bluetooth LE capable device, if they are set up to listen for this specific advertisment.

**Note** For user privacy, the lifespan of your advertisement is tied to that of your app. You can create a BluetoothLEAdvertisementPublisher and call Start in a background task for advertisement in the background. For more information about background tasks, see [Launching, resuming, and background tasks](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/index).

### <a name="basic-publishing"></a>Basic Publishing

There are many different ways to add data to an Advertisement. This example shows a common way to create a company-specific advertisement. 

First, create the advertisement publisher that controls whether or not the device is beaconing out a specific advertisement.

```csharp
BluetoothLEAdvertisementPublisher publisher = new BluetoothLEAdvertisementPublisher();
```

Second, create a custom data section. This example uses an unassigned **CompanyId** value *0xFFFE* and adds the text *Hello World* to the advertisement. 

```csharp
// Add custom data to the advertisement
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

var writer = new DataWriter();
writer.WriteString("Hello World");

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
manufacturerData.Data = writer.DetachBuffer();

// Add the manufacturer data to the advertisement publisher:
publisher.Advertisement.ManufacturerData.Add(manufacturerData);
```

Now that the publisher has been created and setup, you can call **Start** to begin advertising.

```csharp
publisher.Start();
```

## <a name="watching-for-advertisements"></a>Watching for Advertisements

### <a name="basic-watching"></a>Basic Watching

The following code demonstrates how to create a Bluetooth LE Advertisement watcher, set a callback, and start watching for all LE advertisements.

```csharp
BluetoothLEAdvertisementWatcher watcher = new BluetoothLEAdvertisementWatcher();
watcher.Received += OnAdvertisementReceived;
watcher.Start();
``` 

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // Do whatever you want with the advertisement
}
```

#### <a name="active-scanning"></a>Active Scanning
To receive scan response advertisements as well, set the following after creating the watcher. Note that this will cause greater power drain and is not available while in background modes.

```csharp
watcher.ScanningMode = BluetoothLEScanningMode.Active;
```

### <a name="watching-for-a-specific-advertisement-pattern"></a>Watching for a Specific Advertisement Pattern

Sometimes you want to listen for a specific advertisement. In this case, listen for an advertisement containing a payload with a made up company (identified as 0xFFFE) and containing the string *Hello World* in the advertisement. This can be paired with the Basic Publishing example to have one Windows machine advertising and another watching. Be sure to set this advertisement filter before you start the watcher!

```csharp
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
var writer = new DataWriter();
writer.WriteString("Hello World");
manufacturerData.Data = writer.DetachBuffer();

watcher.AdvertisementFilter.Advertisement.ManufacturerData.Add(manufacturerData);
```

### <a name="watching-for-a-nearby-advertisement"></a>Watching for a Nearby Advertisement

Sometimes you only want to trigger your watcher when the device advertising has come in range. You can define your own range, just note that values will be clipped to between 0 and -128. 

```csharp
// Set the in-range threshold to -70dBm. This means advertisements with RSSI >= -70dBm 
// will start to be considered "in-range" (callbacks will start in this range).
watcher.SignalStrengthFilter.InRangeThresholdInDBm = -70;

// Set the out-of-range threshold to -75dBm (give some buffer). Used in conjunction 
// with OutOfRangeTimeout to determine when an advertisement is no longer 
// considered "in-range".
watcher.SignalStrengthFilter.OutOfRangeThresholdInDBm = -75;

// Set the out-of-range timeout to be 2 seconds. Used in conjunction with 
// OutOfRangeThresholdInDBm to determine when an advertisement is no longer 
// considered "in-range"
watcher.SignalStrengthFilter.OutOfRangeTimeout = TimeSpan.FromMilliseconds(2000);
```

### <a name="gauging-distance"></a>Gauging Distance

When your Bluetooth LE Watcher's callback is triggered, the eventArgs include an RSSI value telling you the received signal strength (how strong the Bluetooth signal is).

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // The received signal strength indicator (RSSI)
    Int16 rssi = eventArgs.RawSignalStrengthInDBm;
}
```

This can be roughly translated into distance, but should not be used to measure true distances as each individual radio is different. Different environmental factors can make distance difficult to gauge (such as walls, cases around the radio, or even air humidity).

An alternative to judging pure distance is to define "buckets". Radios tend to report 0 to -50 DBm when they are very close, -50 to -90 when they are a medium distance away, and below -90 when they are far away. Trial and error is best to determine what you want these buckets to be for your app.


<!--HONumber=Dec16_HO1-->


