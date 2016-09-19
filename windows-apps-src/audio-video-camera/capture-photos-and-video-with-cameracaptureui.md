---
author: drewbatgit
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: This article describes how to use the CameraCaptureUI class to capture photos or videos using the camera UI built into Windows.
title: Capture photos and video with Windows built-in camera UI
translationtype: Human Translation
ms.sourcegitcommit: b4bf4d74ae291186100a553a90fd93f890b8ece4
ms.openlocfilehash: fea1c2f8f52ec9ac485d9a4846cc0661243a7ccc

---

# Capture photos and video with Windows built-in camera UI

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


This article describes how to use the CameraCaptureUI class to capture photos or videos using the camera UI built into Windows. This feature is easy to use and allows your app to get a user-captured photo or video with just a few lines of code.

If you want to provide your own camera UI or if your scenario requires more robust, low-level control of the capture operation, you should use the [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) object and implement your own capture experience. For more information, see [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

## Capture a photo with CameraCaptureUI

To use the camera capture UI, include the [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) namespace in your project. To do file operations with the returned image file, include [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346).

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

To capture a photo, create a new [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) object. Using the object's [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058) property you can specify properties for the returned photo such as the image format of the photo. By default, the camera capture UI allows the user to crop the photo before it is returned, although this can be disabled with the [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) property. This example sets the [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044) to request that the returned image be 200 x 200 in pixels.

> [!NOTE]
> Imaging cropping in the **CameraCaptureUI** is not supported for devices in the Mobile device family. The value of the [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) property is ignored when your app is running on these devices.

Call [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) and specify [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040) to specify that a photo should be captured. The method returns a [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) instance containing the image if the capture is successful. If the user cancels the capture, the returned object is null.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

The **StorageFile** containing the captured photo is given a dynamically generated name and saved in your app's local folder. To better organize your captured photos, you may want to move the file to a different folder.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

To use your photo in your app, you may want to create a [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) object that can be used with several different Universal Windows app features.

First you should include the [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400) namespace in your project.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Call [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) to get a stream from the image file. Call [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182) to get a bitmap decoder for the stream. Then call [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332) to get a **SoftwareBitmap** representation of the image.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

To display the image in your UI, declare an [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) control in your XAML page.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

To use the software bitmap in your XAML page, include the using [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) namespace in your project.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

The **Image** control requires that the image source be in BGRA8 format with premultiplied alpha or no alpha, so call the static method [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) to create a new software bitmap with the desired format. Next, create a new [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) object and call [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) to assign the software bitmap to the source. Finally, set the **Image** control's [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) property to display the captured photo in the UI.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## Capture a video with CameraCaptureUI

To capture a video, create a new [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) object. Using the object's [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059) property you can specify properties for the returned video such as the format of the video.

Call [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) and specify [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059) to specify that a video should be capture. The method returns a [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) instance containing the video if the capture is successful. If the user cancels the capture, the returned object is null.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

What you do with the captured video file depends on the scenario for your app. The rest of this article shows you how to quickly create a media composition from one or more captured videos and show it in your UI.

First, add a [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) control in which the video composition will be displayed to your XAML page.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]

Add the [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) and [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) namespaces to your project.


[!code-cs[UsingMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingMediaComposition)]

Declare member variables for a [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) object and a [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716) that you want to stay in scope for the lifetime of the page.

[!code-cs[DeclareMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

Once, before you capture any videos, you should create a new instance of the **MediaComposition** class.

[!code-cs[InitComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetInitComposition)]

With the video file returned from the camera capture UI, create a new [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) by calling [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607). Add the media clip to the composition's [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) collection.

Call [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) to create the **MediaStreamSource** object from the composition.

[!code-cs[AddToComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetAddToComposition)]

Finally, set the stream source to using the media element's [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) method to show the composition in the UI.

[!code-cs[SetMediaElementSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetMediaElementSource)]

You can continue to capture video clips and add them to the composition. For more information on media compositions, see [Media compositions and editing](media-compositions-and-editing.md).

> [!NOTE] 
> This article is for Windows 10 developers writing Universal Windows Platform (UWP) apps. If you’re developing for Windows 8.x or Windows Phone 8.x, see the [archived documentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Related topics

* [Camera](camera.md)
* [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 
 

 







<!--HONumber=Aug16_HO3-->


