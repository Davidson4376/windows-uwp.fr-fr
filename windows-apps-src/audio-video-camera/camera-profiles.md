---
author: drewbatgit
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: This article discusses how to use camera profiles to discover and manage the capabilities of different video capture devices. This includes tasks such as selecting profiles that support specific resolutions or frame rates, profiles that support simultaneous access to multiple cameras, and profiles that support HDR.
title: Discover and select camera capabilities with camera profiles
translationtype: Human Translation
ms.sourcegitcommit: 625cf715a88837cb920433fa34e47a1e1828a4c8
ms.openlocfilehash: 09cb41f834de52d541addee4e44715c52f5e99dc

---

# Discover and select camera capabilities with camera profiles

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


This article discusses how to use camera profiles to discover and manage the capabilities of different video capture devices. This includes tasks such as selecting profiles that support specific resolutions or frame rates, profiles that support simultaneous access to multiple cameras, and profiles that support HDR.

> [!NOTE] 
> This article builds on concepts and code discussed in [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), which describes the steps for implementing basic photo and video capture. It is recommended that you familiarize yourself with the basic media capture pattern in that article before moving on to more advanced capture scenarios. The code in this article assumes that your app already has an instance of MediaCapture that has been properly initialized.

 

## About camera profiles

Cameras on different devices support different capabilities including the set of supported capture resolutions, frame rate for video captures, and whether HDR or variable frame rate captures are supported. The Universal Windows Platform (UWP) media capture framework stores this set of capabilities in a [**MediaCaptureVideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695). A camera profile, represented by a [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694) object, has three collections of media descriptions; one for photo capture, one for video capture, and another for video preview.

Before initializing your [MediaCapture](capture-photos-and-video-with-mediacapture.md) object, you can query the capture devices on the current device to see what profiles are supported. When you select a supported profile, you know that the capture device supports all of the capabilities in the profile's media descriptions. This eliminates the need for a trial and error approach to determining which combinations of capabilities are supported on a particular device.

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

The code examples in this article replace this minimal initialization with the discovery of camera profiles supporting various capabilities, which are then used to initialize the media capture device.

## Find a video device that supports camera profiles

Before searching for supported camera profiles, you should find a capture device that supports the use of camera profiles. The **GetVideoProfileSupportedDeviceIdAsync** helper method defined in the example below uses the [**DeviceInformaion.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) method to retrieve a list of all available video capture devices. It loops through all of the devices in the list, calling the static method, [**IsVideoProfileSupported**](https://msdn.microsoft.com/library/windows/apps/dn926714), for each device to see if it supports video profiles. Also, the [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) property for each device, allowing you to specify wether you want a camera on the front or back of the device.

If a device that supports camera profiles is found on the specified panel, the [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) value, containing the device's ID string, is returned.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

If the device ID returned from the **GetVideoProfileSupportedDeviceIdAsync** helper method is null or an empty string, there is no device on the specified panel that supports camera profiles. In this case, you should initialize your media capture device without using profiles.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## Select a profile based on supported resolution and frame rate

To select a profile with particular capabilities, such as with the ability to achieve a particular resolution and frame rate, you should first call the helper method defined above to get the ID of a capture device that supports using camera profiles.

Create a new [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573) object, passing in the selected device ID. Next, call the static method [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) to get a list of all camera profiles supported by the device.

This example uses a Linq query method, included in the using **System.Linq** namespace, to select a profile that contains a [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) object where the [**Width**](https://msdn.microsoft.com/library/windows/apps/dn926700), [**Height**](https://msdn.microsoft.com/library/windows/apps/dn926697), and [**FrameRate**](https://msdn.microsoft.com/library/windows/apps/dn926696) properties match the requested values. If a match is found, [**VideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926679) and [**RecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926678) of the **MediaCaptureInitializationSettings** are set to the values from the anonymous type returned from the Linq query. If no match is found, the default profile is used.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

After you populate the **MediaCaptureInitializationSettings** with your desired camera profile, you simply call [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) on your media capture object to configure it to the desired profile.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## Select a profile that supports concurrence

You can use camera profiles to determine if a device supports video capture from multiple cameras concurrently. For this scenario, you will need to create two sets of capture objects, one for the front camera and one for the back. For each camera, create a **MediaCapture**, a **MediaCaptureInitializationSettings**, and a string to hold the capture device ID. Also, add a boolean variable that will track whether concurrence is supported.

[!code-cs[ConcurrencySetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConcurrencySetup)]

The static method [**MediaCapture.FindConcurrentProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926709) returns a list of the camera profiles that are supported by the specified capture device that can also supports concurrence. Use a Linq query to find a profile that supports concurrence and that is supported by both the front and back camera. If a profile that meets theses requirements is found, set the profile on each of the **MediaCaptureInitializationSettings** objects and set the boolean concurrence tracking variable to true.

[!code-cs[FindConcurrencyDevices](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindConcurrencyDevices)]

Call **MediaCapture.InitializeAsync** for the primary camera for your app scenario. If concurrence is supported, initialize the second camera as well.

[!code-cs[InitConcurrentMediaCaptures](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitConcurrentMediaCaptures)]

## Use known profiles to find a profile that supports HDR video

Selecting a profile that supports HDR begins like the other scenarios. Create a a **MediaCaptureInitializationSettings** and a string to hold the capture device ID. Add a boolean variable that will track whether HDR video is supported.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

Use the **GetVideoProfileSupportedDeviceIdAsync** helper method defined above to get the device ID for a capture device that supports camera profiles.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

The static method [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) returns the camera profiles supported by the specified device that is categorized by the specified [**KnownVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn948843) value. For this scenario, the **VideoRecording** value is specified to limit the returned camera profiles to ones that support video recording.

Loop through the returned list of camera profiles. For each camera profile, loop through each [**VideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695) in the profile checking to see if the [**IsHdrVideoSupported**](https://msdn.microsoft.com/library/windows/apps/dn926698) property is true. After a suitable media description is found, break out of the loop and assign the profile and description objects to the **MediaCaptureInitializationSettings** object.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## Determine if a device supports simultaneous photo and video capture

Many devices support capturing photos and video simultaneously. To determine if a capture device supports this, call [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) to get all of the camera profiles supported by the device. Use a link query to find a profile that has at least one entry for both [**SupportedPhotoMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926703) and [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) which means that the profile supports simultaneous capture.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

You can refine this query to look for profiles that support specific resolutions or other capabilities in addition to simultaneous video record. You can also use the [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) and specify the [**BalancedVideoAndPhoto**](https://msdn.microsoft.com/library/windows/apps/dn948843) value to retrieve profiles that support simultaneous capture, but querying all profiles will provide more complete results.

## Related topics

* [Camera](camera.md)
* [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 







<!--HONumber=Aug16_HO3-->


