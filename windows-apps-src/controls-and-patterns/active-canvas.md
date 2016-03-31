---
Description: A pattern with a content area and a command area for single-view apps or modal experiences, such as photo viewers/editors, document viewers, maps, painting, or other apps that make use of a free-scrolling view.
title: Guidelines for the active canvas pattern
ms.assetid: 4D768472-64D6-406C-9E87-F750F6B981A0
label: TBD
template: detail.hbs
---
An active canvas is a pattern with a content area and a command area. It's for single-view apps or modal experiences, such as photo viewers/editors, document viewers, maps, painting, or other apps that make use of a free-scrolling view. For taking actions, an active canvas can be paired with a command bar or just buttons, depending on the number and types of actions you need.

## Examples

This design of a photo editing app features an active canvas pattern, with a mobile example on the left, and a desktop example on the right. The image editing surface is a canvas, and the command bar at the bottom contains all of the contextual actions for the app.

![Example of a photo editor using active canvas pattern](images/uap-photo-pc-phone-700.png)

This design of a subway map app makes use of an active canvas with a simple UI strip at the top that has only two actions and a search box. Contextual actions are shown in the context menu, as seen on the right image.

![Example of a maps app using active canvas pattern](images/uap-subway-pc-phone-700.png)


## Implementing this pattern

The active canvas pattern consists of a content area and a command area.

**Content area.**  The content area is usually a free-scrolling canvas. Multiple content areas can exist within an app.

**Command area.**  If you're placing a lot of commands, then a command bar, which responds based on screen size, could be the way to go. If you're not placing that many commands and aren't as concerned with a responsive UI, space-saving buttons work well.

\[This article contains information that is specific to Universal Windows Platform (UWP) apps and Windows 10. For Windows 8.1 guidance, please download the [Windows 8.1 guidelines PDF](https://go.microsoft.com/fwlink/p/?linkid=258743).\]

## Related articles

-   [**App bar and command bar**](app-bars.md)
<!--HONumber=Mar16_HO1-->
