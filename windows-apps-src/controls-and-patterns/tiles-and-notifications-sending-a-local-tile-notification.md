---
author: mijacobs
Description: This article describes how to send a local tile notification to a primary tile and a secondary tile using adaptive tile templates.
title: Send a local tile notification
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
label: TBD
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: d51aacb31f41cbd9c065b013ffb95b83a6edaaf4
ms.openlocfilehash: 8fc2fc007d14bd9c5d08ca4eb7e61a2dfdf04d3b

---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 
# <a name="send-a-local-tile-notification"></a>Send a local tile notification





Primary app tiles in Windows 10 are defined in your app manifest, while secondary tiles are programmatically created and defined by your app code. This article describes how to send a local tile notification to a primary tile and a secondary tile using adaptive tile templates. (A local notification is one that's sent from app code as opposed to one that's pushed or pulled from a web server.)

![default tile and tile with notification](images/sending-local-tile-01.png)

**Note**   Learn about [creating adaptive tiles](tiles-and-notifications-create-adaptive-tiles.md) and [adaptive tile template schema](tiles-and-notifications-adaptive-tiles-schema.md).

 

## <a name="install-the-nuget-package"></a>Install the NuGet package


We recommend installing the [Notifications library NuGet package](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), which simplifies things by generating tile payloads with objects instead of raw XML.

The inline code examples in this article are for C# using the Notifications library. (If you'd prefer to create your own XML, you can find code examples without the Notifications library toward the end of the article.)

## <a name="add-namespace-declarations"></a>Add namespace declarations


To access the tile APIs, include the [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661) namespace. We also recommend including the **NotificationsExtensions.Tiles** namespace so that you can take advantage of our tile helper APIs (you must install the [Notifications library](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) NuGet package to access these APIs).

```CSharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```

## <a name="create-the-notification-content"></a>Create the notification content


In Windows 10, tile payloads are defined using adaptive tile templates, which allow you to create custom visual layouts for your notifications. (To learn what's possible with adaptive tiles, see the [Create adaptive tiles](tiles-and-notifications-create-adaptive-tiles.md) and [Adaptive tile templates](tiles-and-notifications-adaptive-tiles-schema.md) articles.)

This code example creates adaptive tile content for medium and wide tiles.

```CSharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// Construct the tile content
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },

        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    }
};
```

The notification content looks like the following when displayed on a medium tile:

![notification content on a medium tile](images/sending-local-tile-02.png)

## <a name="create-the-notification"></a>Create the notification


Once you have your notification content, you'll need to create a new [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616). The **TileNotification** constructor takes a Windows Runtime [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br208620) object, which you can obtain from the **TileContent.GetXml** method if you're using the [Notifications library](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/).

This code example creates a notification for a new tile.

```CSharp
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <a name="set-an-expiration-time-for-the-notification-optional"></a>Set an expiration time for the notification (optional)


By default, local tile and badge notifications don't expire, while push, periodic, and scheduled notifications expire after three days. Because tile content shouldn't persist longer than necessary, it's a best practice to set an expiration time that makes sense for your app, especially on local tile and badge notifications.

This code example creates a notification that expires and will be removed from the tile after ten minutes.

```CSharp
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);
```

## <a name="send-the-notification"></a>Send the notification


Although locally sending a tile notification is simple, sending the notification to a primary or secondary tile is a bit different.

**Primary tile**

To send a notification to a primary tile, use the [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622) to create a tile updater for the primary tile, and send the notification by calling "Update". Regardless of whether it's visible, your app's primary tile always exists, so you can send notifications to it even when it's not pinned. If the user pins your primary tile later, the notifications that you sent will appear then.

This code example sends a notification to a primary tile.


```CSharp
// Send the notification to the primary tile
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**Secondary tile**

To send a notification to a secondary tile, first make sure that the secondary tile exists. If you try to create a tile updater for a secondary tile that doesn't exist (for example, if the user unpinned the secondary tile), an exception will be thrown. You can use [**SecondaryTile.Exists**](https://msdn.microsoft.com/library/windows/apps/br242205)(tileId) to discover if your secondary tile is pinned, and then create a tile updater for the secondary tile and send the notification.

This code example sends a notification to a secondary tile.

```CSharp
// If the secondary tile is pinned
if (SecondaryTile.Exists("MySecondaryTile"))
{
    // Get its updater
    var updater = TileUpdateManager.CreateTileUpdaterForSecondaryTile("MySecondaryTile");
 
    // And send the notification
    updater.Update(notification);
}
```

![default tile and tile with notification](images/sending-local-tile-01.png)

## <a name="clear-notifications-on-the-tile-optional"></a>Clear notifications on the tile (optional)


In most cases, you should clear a notification once the user has interacted with that content. For example, when the user launches your app, you might want to clear all the notifications from the tile. If your notifications are time-bound, we recommend that you set an expiration time on the notification instead of explicitly clearing the notification.

This code example clears the tile notification for the primary tile. You can do the same for secondary tiles by creating a tile updater for the secondary tile.

```CSharp
TileUpdateManager.CreateTileUpdaterForApplication().Clear();
```

For a tile with the notification queue enabled and notifications in the queue, calling the Clear method empties the queue. You can't, however, clear a notification via your app's server; only the local app code can clear notifications.

Periodic or push notifications can only add new notifications or replace existing notifications. A local call to the Clear method will clear the tile whether or not the notifications themselves came via push, periodic, or local. Scheduled notifications that haven't yet appeared are not cleared by this method.

![tile with notification and tile after being cleared](images/sending-local-tile-03.png)

## <a name="next-steps"></a>Next steps


**Using the notification queue**

Now that you have done your first tile update, you can expand the functionality of the tile by enabling a [notification queue](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234).

**Other notification delivery methods**

This article shows you how to send the tile update as a notification. To explore other methods of notification delivery, including scheduled, periodic, and push, see [Delivering notifications](tiles-and-notifications-choosing-a-notification-delivery-method.md).

**XmlEncode delivery method**

If you're not using the [Notifications library](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), this notification delivery method is another alternative.


```CSharp
public string XmlEncode(string text)
{
    StringBuilder builder = new StringBuilder();
    using (var writer = XmlWriter.Create(builder))
    {
        writer.WriteString(text);
    }

    return builder.ToString();
}
```

## <a name="code-examples-without-notifications-library"></a>Code examples without Notifications library


If you prefer to work with raw XML instead of the [Notifications library](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) NuGet package, use these alternate code examples to first three examples provided in this article. The rest of the code examples can be used either with the [Notifications library](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) or with raw XML.

Add namespace declarations

```CSharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

Create the notification content

```CSharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// TODO - all values need to be XML escaped
 
 
// Construct the tile content as a string
string content = $@"
<tile>
    <visual>
 
        <binding template='TileMedium'>
            <text>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
        <binding template='TileWide'>
            <text hint-style='subtitle'>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
    </visual>
</tile>";
```

Create the notification

```CSharp
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <a name="related-topics"></a>Related topics


* [Create adaptive tiles](tiles-and-notifications-create-adaptive-tiles.md)
* [Adaptive tile templates: schema and documentation](tiles-and-notifications-adaptive-tiles-schema.md)
* [Notifications library](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [Full code sample on GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Windows.UI.Notifications namespace**](https://msdn.microsoft.com/library/windows/apps/br208661)
* [How to use the notification queue (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [Delivering notifications](tiles-and-notifications-choosing-a-notification-delivery-method.md)
 

 







<!--HONumber=Dec16_HO1-->


