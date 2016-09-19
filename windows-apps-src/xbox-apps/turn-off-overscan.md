---
author: payzer
title: How to draw UI to the edge of the screen
description: 
translationtype: Human Translation
ms.sourcegitcommit: b5961d3266a031ab09a9da63319e9883cf050789
ms.openlocfilehash: cddde27a17e897ab8a68bbed099e532a8cd48f07

---

# How to draw UI to the edge of the screen   
By default, applications will have borders placed at the edges of the viewport to account for the TV-safe area (for more information, see [Designing for Xbox and TV](../input-and-devices/designing-for-tv.md#tv-safe-area)). 

We recommend turning this off and drawing to the edge of the screen. You can draw to the edge of the screen by adding the following code when your application starts:
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> C++/DirectX applications do not have to worry about this. The system will always render your application to the edge of the screen.

## See also
- [Best practices for Xbox](tailoring-for-xbox.md)
- [UWP on Xbox One](index.md)



<!--HONumber=Aug16_HO3-->


