---
author: drewbatgit
ms.assetid: 
description: This article shows you how to play media in your Universal Windows app with MediaPlayer.
title: Play audio and video with MediaPlayer
---

# Play audio and video with MediaPlayer

This article shows you how to play media in your Universal Windows app using the  [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) class. With Windows 10, version 1607, significant improvements were made to the media playback APIs, including a simplified single-process design for background audio, automatic integration with the System Media Transport Controls (SMTC), the ability to synchronize multiple media players, the ability to a Windows.UI.Composition surface, and an easy interface for creating and scheduling media breaks in your content. To take advantage of these improvements, the recommended best practice for playing media is to use the **MediaPlayer** class instead of **MediaElement** for media playback. The lightweight XAML control, [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement), has been introduced to allow you render media content in a XAML page. Many of the playback control and status APIs provided by **MediaElement** are now available through the new [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) object. **MediaElement** continues to function to support backwards compatibility, but no additional features will be added to this class.

This article will walk you through the **MediaPlayer** features that a typical media playback app will use. Note that **MediaPlayer** uses the [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) class as a container for all media items. This class allows you to load and play media from many different sources, including local files, memory streams, and network sources, all using the same interface. There are also higher-level classes that work with **MediaSource**, such as [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) and [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList), that provide more advanced features like playlists and the ability to manage media sources with multiple audio, video, and metadata tracks. For more information on **MediaSource** and related APIs, see [Media items, playlists, and tracks](media-playback-with-mediasource.md).


##Play a media file with MediaPlayer  
Basic media playback with **MediaPlayer** is very simple to implement. First, create a new instance of the **MediaPlayer** class. Your app can have multiple **MediaPlayer** instances active at once. Next, set the [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) property of the player to an object that implements the [**IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource), such as a [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), a [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem), or a [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList). In this example, a **MediaSource** is created from a file in the app's local storage, and then a **MediaPlaybackItem** is created from the source and then assigned to the player's **Source** property.

Unlike **MediaElement**, **MediaPlayer** does not automatically begin playback by default. You can start playback by calling [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play), by setting the [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) property to true, or waiting for the user to initiate playback with the built-in media controls.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

When your app is done using a **MediaPlayer**, you should call the [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) method (projected to **Dispose** in C#) to clean up the resources used by the player.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

##Use MediaPlayerElement to render video in XAML
You can play media in a **MediaPlayer** without displaying it in XAML, but many media playback apps will want to render the media in a XAML page. To do this, use the lightweight [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) control. Like **MediaElement**, **MediaPlayerElement** allows you to specify whether the built-in transport controls should be shown.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

You can set the **MediaPlayer** instance that the element is bound to by calling [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764).

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

You can also set the playback source on the **MediaPlayerElement** and the element will automatically create a new **MediaPlayer** instance that you can access using the [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer) property.

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

##Common MediaPlayer tasks
This section shows you how to use some of the features of the **MediaPlayer**.

###Set the audio category
Set the [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) property of a **MediaPlayer** to one of the values of the [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) enumeration to let the system know what kind of media you are playing. Games should categorize their music streams as **GameMedia** so that game music mutes automatically if another application plays music in the background. Music or video applications should categorize their streams as **Media** or **Movie** so they will take priority over **GameMedia** streams.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

###Output to a specific audio endpoint
By default, the audio output from a **MediaPlayer** is routed to the default audio endpoint for the system, but you can specify a specific audio endpoint that the **MediaPlayer** should use for output. In the example below, [**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) returns a string that uniquely idenfies the audio render category of devices. Next, the [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) method [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) is called to get a list of all available devices of the selected type. You may programmatically determine which device you want to use or add the returned devices to a [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) to allow the user to select a device.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

In the [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) event for the devices combo box, the [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) property of the **MediaPlayer** is set to the selected device, which was stored in the [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) property of the **ComboBoxItem**.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

###Playback session
As described previously in this article, many of the functions that are exposed by the **MediaElement** class have been moved to the [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) class. This includes information about the playback state of the player, such as the current playback position, whether the player is paused or playing, and the current playback speed. **MediaPlaybackSession** also provides several events to notify you when the state changes, including the current buffering and download status of content being played and the natural size and aspect ratio of the currently playing video content.

The following example shows you how to implement a button click handler that skips 10 seconds forward in the content. First, the **MediaPlaybackSession** object for the player is retrieved with the [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession) property. Next the [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) property is set to the current playback position plus 10 seconds.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

The next example illustrates using a toggle button to toggle between normal playback speed and 2X speed by setting the [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) property of the session.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

###Pinch and zoom video
**MediaPlayer** allows you to specify the source rectangle within video content that should be rendered, effectively allowing you to zoom into video. The rectangle you specify is relative to a normalized rectangle (0,0,1,1) where 0,0 is the upper left hand of the frame and 1,1 specifies the full width and height of the frame. So, for example, to set the zoom rectangle so that the top-right quadrant of the video is rendered, you would specify the rectangle (.5,0,.5,.5).  It is important that you check your values to make sure that your source rectangle is within the (0,0,1,1) normalized rectangle. Attempting to set a value outside of this range will cause an exception to be thrown.

To implement pinch and zoom using multi-touch gestures, you must first specify which gestures you want to support. In this example, scale and translate gestures are requested. The [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) event is raised when one of the subscribed gestures occurs. The [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) event will be used to reset the zoom to the full frame. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

Next, declare a **Rect** object that will store the current zoom source rectangle.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

The **ManipulationDelta** handler adjusts either the scale or the translation of the zoom rectangle. If the delta scale value is not 1, it means that the user performed a pinch gesture. If the value is greater than 1, the source rectangle should be made smaller to zoom into the content. If the value is less than 1, then the source rectangle should be made bigger to zoom out. Before setting the new scale values, the resulting rectangle is checked to make sure it lies entirely within the (0,0,1,1) limits.

If the scale value is 1, then the translation gesture is handled. The rectangle is simply translated by the number of pixels in gesture divided by the width and height of the control. Again, the resulting rectangle is checked to make sure it lies within the (0,0,1,1) bounds.

Finally, the [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect) of the **MediaPlaybackSession** is set to the newly adjusted rectangle, specifying the area within the video frame that should be rendered.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

In the [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) event handler, the source rectangle is set back to (0,0,1,1) to cause the entire video frame to be rendered.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]
		
##Use MediaPlayerSurface to render video to a Windows.UI.Composition surface
Starting with Windows 10, version 1607, you can use **MediaPlayer** to render video to an to render video to an [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface), which allows the player to interoperate with the APIs in the [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition) namespace. The composition framework allows you to work with graphics in the visual layer between XAML and the low-level DirectX graphics APIs. This enables scenarios like rendering video into any XAML control. For more information on using the composition APIs, see [Visual Layer](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer).

The following example illustrates how to render video player content onto a [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas) control. The media player-specific calls in this example are [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) and [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963). **SetSurfaceSize** tells the system the size of the buffer that should be allocated for rendering content. **GetSurface** takes a [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) as an arguemnt and retreives an instance of the [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) class. This class provides access to the **MediaPlayer** and **Compositor** used to create the surface and exposes the surface itself through the [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface) property.

The rest of the code in this example creates a [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual) to which the video is rendered and sets the size to the size of the canvas element that will display the visual. Next a [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush) is created from the [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) and assigned to the [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) property of the visual. Next a [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual) is created and the **SpriteVisual** is inserted at the top of its visual tree. Finally, [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981) is called to assign the container visual to the **Canvas**.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
		
##Use MediaTimelineController to synchronize content across multiple players.
As discussed previously in this article, your app can have several **MediaPlayer** objects active at a time. By default, each **MediaPlayer** you create operates independently. For some scenarios, such as synchronizing a commentary track to a video, you may want to synchronize the player state, playback position, and playback speed of multiple players. Starting with Windows 10, version 1607, you can implement this behavior by using the [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) class.

###Implement playback controls
The following example shows how to use a **MediaTimelineController** to control two instances of **MediaPlayer**. First, each instance of the **MediaPlayer** is instantiated and the **Source** is set to a media file. Next, a new **MediaTimelineController** is created. For each **MediaPlayer**, the [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) associated with each player is disabled by setting the [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) property to false. And then then the [**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController) property is set to the timeline controller object.

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**Caution** The [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) provides automatic integration between **MediaPlayer** and the System Media Transport Controls (SMTC), but this automatic integration can't be used with media players that are controlled with a **MediaTimelineController**. Therefore you must disable the command manager for the media player before setting the player's timeline controller. Failure to do so will result in an exception being thrown with the following message: "Attaching Media Timeline Controller is blocked because of the current state of the object." For more information on media player integration with the SMTC, see [Integrate with the System Media Transport Controls](integrate-with-systemmediatransportcontrols.md). If you are using a **MediaTimelineController** you can still control the SMTC manually. For more information, see [Manual control of the System Media Transport Controls](system-media-transport-controls.md).

Once you have attached a **MediaTimelineController** to one or more media players, you can control the playback state by using the methods exposed by the controller. The following example calls [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.Start) to begin playback of all associated media players at the beginning of the media.

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

This example illustrates pausing and resuming all of the attached media players.

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

To fast-forward all connected media players, set the playback speed to a value greater that 1.

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

The next example shows how to use a **Slider** control to show the current playback position of the timeline controller relative to the duration of the content of one of the connected media players. First, a new **MediaSource** is created and a handler for the [**OpenOperationCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource.OpenOperationCompleted) of the media source is registered. 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

The **OpenOperationCompleted** handler is used as an opportunity to discover the duration of the media source content. Once the duration is determined, the maximum value of the **Slider** control is set to the total number of seconds of the media item. The value is set inside a call to [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) to make sure it is run on the UI thread.

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

Next, a handler for the timeline controller's  [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.PositionChanged) event is registered. This is called periodically by the system, approximately 4 times per second.

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

In the handler for **PositionChanged**, the slider value is updated to reflect the current position of the timeline controller.

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

###Offset the playback position from the timeline position
In some cases you may want the playback position of one or more media players associated with a timeline controller to be offset from the other players. You can do this by setting the [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) property of the **MediaPlayer** object you want to be offset. The following example uses the durations of the content of two media players to set the minimum and maximum values of two slider control to plus and minus the length of the item.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

In the [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) event for each slider, the **TimelineControllerPositionOffset** for each player is set to the corresponding value.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

Note that if the offset value of a player maps to a negative playback position, the clip will remain paused until the offset reaches zero and then playback will begin. Likewise, if the offset value maps to a playback position greater than the duration of the media item, the final frame will be shown, just as it does when a single media player reached the end of its content.

## Related topics
* [Media playback](media-playback.md)
* [Media items, playlists, and tracks](media-playback-with-mediasource.md)
* [Integrate with the Sytem Media Transport Controls](integrate-with-systemmediatransportcontrols.md)
* [Create, schedule, and manage media breaks](create-schedule-and-manage-media-breaks.md)
* [Play media in the background](background-audio.md)





 

 




