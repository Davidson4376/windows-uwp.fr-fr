---
author: drewbatgit
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: You can use the Windows.Media.Transcoding APIs to transcode video files from one format to another.
title: Transcode media files
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 7e96f12881e4f210a1bba57d2a9c298dbb1c32e3

---

# Transcode media files

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


You can use the [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) APIs to transcode video files from one format to another.

*Transcoding* is the conversion of a digital media file, such as a video or audio file, from one format to another. This is usually done by decoding and then re-encoding the file. For example, you might convert a Windows Media file to MP4 so that it can be played on a portable device that supports MP4 format. Or, you might convert a high-definition video file to a lower resolution. In that case, the re-encoded file might use the same codec as the original file, but it would have a different encoding profile.

## Set up your project for transcoding

In addition to the namespaces referenced by the default project template, you will need to reference these namespaces in order to transcode media files using the code in this article.

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## Select source and destination files

The way that your app determines the source and destination files for transcoding depends on your implementation. This example uses a [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) and a [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) to allow the user to pick a source and a destination file.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## Create a media encoding profile

The encoding profile contains the settings that determine how the destination file will be encoded. This is where you have the greatest number of options when you transcode a file.

The [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) class provides static methods for creating predefined encoding profiles:

-   Wav
-   AAC audio (M4A)
-   MP3 audio
-   Windows Media Audio (WMA)
-   Avi
-   MP4 video (H.264 video plus AAC audio)
-   Windows Media Video (WMV)

The first four profiles in this list contain audio only. The other three contain video and audio.

The following code creates a profile for MP4 video.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

The static [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078) method creates an MP4 encoding profile. The parameter for this method gives the target resolution for the video. In this case, [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) means 1280 x 720 pixels at 30 frames per second. ("720p" stands for 720 progressive scan lines per frame.) The other methods for creating predefined profiles all follow this pattern.

Alternatively, you can create a profile that matches an existing media file by using the [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047) method. Or, if you know the exact encoding settings that you want, you can create a new [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) object and fill in the profile details.

## Transcode the file

To transcode the file, create a new [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) object and call the [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936) method. Pass in the source file, the destination file, and the encoding profile. Then call the [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) method on the [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) object that was returned from the async transcode operation.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## Respond to transcoding progress

You can register events to respond when the progress of the asynchronous [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) changes. These events are part of the async programming framework for Universal Windows Platform (UWP) apps and are not specific to the transcoding API.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]

 

 







<!--HONumber=Aug16_HO3-->


