---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: This article shows you how video device controls to enable enhanced video capture scenarios including HDR video and exposure priority.
title: Capture device controls for video capture
---

# Capture device controls for video capture

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


This article shows you how video device controls to enable enhanced video capture scenarios including: HDR video and exposure priority.

The video device controls discussed in this article are all added to your app using the same pattern. First, you check to see if the control is supported on the current device on which your app is running. If the control is supported, then you set the desired mode for the control. Typically, if a particular control is unsupported on the current device, you should disable or hide the UI element that allows the user to enable the feature.

All of the device control APIs discussed in this article are members of the [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902) namespace.

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

**Note**  
This article builds on concepts and code discussed in [Capture Photos and Video with MediaCapture](capture-photos-and-video-with-mediacapture.md), which describes the steps for implementing basic photo and video capture. It is recommended that you familiarize yourself with the basic media capture pattern in that article before moving on to more advanced capture scenarios. The code in this article assumes that your app already has an instance of MediaCapture that has been properly initialized.

## HDR video

The High Dynamic Range (HDR) video feature applies HDR processing to the video stream of the capture device. Determine if HDR video is supported by checking the [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682) property.

The HDR video control supports three modes: on, off, and automatic, which means that the device dynamically determines if HDR video processing would improve the media capture and, if so, enables HDR video. To determine if a particular mode is supported on the current device, check to see if the [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) collection contains the desired mode.

Enable or disable HDR video processing by setting the [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) to the desired mode.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## Exposure priority

The [**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644), when enabled, evaluates the video frames from the capture device to determine if the video is capturing a low-light scene. If so, the control lowers the frame rate of the captured video in order to increase the exposure time for each frame and improve the visual quality of the captured video.

Determine if the exposure priority control is supported on the current device by checking the [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647) property.

Enable or disable the exposure priority control by setting the [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) to the desired mode.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## Related topics

* [Capture photos and video with MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 




<!--HONumber=Mar16_HO1-->
