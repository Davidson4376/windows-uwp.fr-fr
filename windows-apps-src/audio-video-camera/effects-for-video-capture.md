---
author: drewbatgit
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: This topic shows you how to use the video stabilization effect.
title: Effects for video capture
translationtype: Human Translation
ms.sourcegitcommit: 367ab34663d66d8c454ff305c829be66834e4ebe
ms.openlocfilehash: 3fe7abcc417db76b4375243d66b1c0ecb9092147

---

# Effects for video capture

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

This topic shows you how to use the video stabilization effect.

> [!NOTE] 
> This article builds on concepts and code discussed in [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), which describes the steps for implementing basic photo and video capture. We recommend that you familiarize yourself with the basic media capture pattern in that article before moving on to more advanced capture scenarios. The code in this article assumes that your app already has an instance of MediaCapture that has been properly initialized.

## Video stabilization effect

The video stabilization effect manipulates the frames of a video stream to minimize shaking caused by holding the capture device in your hand. Because this technique causes the pixels to be shifted right, left, up, and down, and because the effect can't know what the content just outside the video frame is, the stabilized video is cropped slightly from the original video. A utility function is provided to allow you to adjust your video encoding settings to optimally manage the cropping performed by the effect.

On devices that support it, Optical Image Stabilization (OIS) stabilizes video by mechanically manipulating the capture device and, therefore, does not need to crop the edges of the video frames. For more information, see [Capture device controls for video capture](capture-device-controls-for-video-capture.md).

### Set up your app to use video stabilization

In addition to the namespaces required for basic media capture, using the video stabilization effect requires the following namespace.

[!code-cs[VideoStabilizationEffectUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEffectUsing)]

Declare a member variable to store the [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760) object. As part of the effect implementation, you will modify the encoding properties that you use to encode the captured video. Declare two variables to store a backup copy of the initial input and output encoding properties so that you can restore them later when the effect is disabled. Finally, declare a member variable of type [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) because this object will be accessed from multiple locations within your code.

[!code-cs[DeclareVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

For this scenario, you should assign the media encoding profile object to a member variable so that you can access it later.

[!code-cs[EncodingProfileMember](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEncodingProfileMember)]

### Initialize the video stabilization effect

After your **MediaCapture** object has been initialized, create a new instance of the [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762) object. Call [**MediaCapture.AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035) to add the effect to the video pipeline and retrieve an instance of the [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760) class. Specify [**MediaStreamType.VideoRecord**](https://msdn.microsoft.com/library/windows/apps/br226640) to indicate that the effect should be applied to the video record stream.

Register an event handler for the [**EnabledChanged**](https://msdn.microsoft.com/library/windows/apps/dn948982) event and call the helper method **SetUpVideoStabilizationRecommendationAsync**, both of which are discussed later in this article. Finally, set the [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926775) property of the effect to true to enable the effect.

[!code-cs[CreateVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### Use recommended encoding properties

As discussed earlier in this article, the technique that the video stabilization effect uses necessarily causes the stabilized video to be cropped slightly from the source video. Define the following helper function in your code in order to adjust the video encoding properties to optimally handle this limitation of the effect. This step is not required in order to use the video stabilization effect, but if you don't perform this step, the resulting video will be upscaled slightly and therefore have slightly lower visual fidelity.

Call [**GetRecommendedStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948983) on your video stabilization effect instance, passing in the [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) object, which informs the effect about your current input stream encoding properties, and your **MediaEncodingProfile** which lets the effect know your current output encoding properties. This method returns a [**VideoStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn926727) object containing new recommended input and output stream encoding properties.

The recommended input encoding properties are, if it is supported by the device, a higher resolution that the initial settings you provided so that there is minimal loss in resolution after the effect's cropping is applied.

Call [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) to set the new encoding properties. Before setting the new properties, use the member variable to store the initial encoding properties so that you can change the settings back when you disable the effect.

If the video stabilization effect must crop the output video, the recommended output encoding properties will be the size of the cropped video. This means that the output resolution will match the cropped video size. If you do not use the recommended output properties, the video will be scaled up to match the initial output size, which will result in a loss of visual fidelity.

Set the [**Video**](https://msdn.microsoft.com/library/windows/apps/hh701124) property of the **MediaEncodingProfile** object. Before setting the new properties, use the member variable to store the initial encoding properties so that you can change the settings back when you disable the effect.

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### Handle the video stabilization effect being disabled

The system may automatically disable the video stabilization effect if the pixel throughput is too high for the effect to handle or if it detects that the effect is running slowly. If this occurs, the EnabledChanged event is raised. The **VideoStabilizationEffect** instance in the *sender* parameter indicates the new state of the effect, enabled or disabled. The [**VideoStabilizationEffectEnabledChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948979) has a [**VideoStabilizationEffectEnabledChangedReason**](https://msdn.microsoft.com/library/windows/apps/dn948981) value indicating why the effect was enabled or disabled. Note that this event is also raised if you programmatically enable or disable the effect, in which case the reason will be **Programmatic**.

Typically, you would use this event to adjust your app's UI to indicate the current status of video stabilization.

[!code-cs[VideoStabilizationEnabledChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### Clean up the video stabilization effect

To clean up the video stabilization effect, call [**ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) to clear all effects from the video pipeline. If the member variables containing the initial encoding properties are not null, use them to restore the encoding properties. Finally, remove the **EnabledChanged** event handler and set the effect to null.

[!code-cs[CleanUpVisualStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## Related topics

* [Camera](camera.md)
* [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 







<!--HONumber=Aug16_HO3-->


