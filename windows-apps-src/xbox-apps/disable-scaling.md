---
author: payzer
title: How to turn off scaling
description: Instructions for turning off the default scale factor.
translationtype: Human Translation
ms.sourcegitcommit: 582f5677c15f7cd62c398103b48743ba4bea6c5b
ms.openlocfilehash: 8079be9685558277565766fa8d0ebbfd4a555904

---

# How to turn off scaling   
By default, applications are scaled to 200% for XAML and 150% for HTML apps. It is possible to turn off the default scale factor. This will cause your application to use the actual pixel dimensions of the device (1910 x 1080 pixels).   
   
## HTML   
You can opt out of scale factor by using the following code snippet: 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

Or, you can use a web-friendly method:   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## XAML
You can opt out of scale factor by using the following code snippet:   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## DirectX/C++   
DirectX/C++ applications are not scaled. Automatic scaling only applies to HTML and XAML applications.  

## See also
- [Best practices for Xbox](tailoring-for-xbox.md)
- [UWP on Xbox One](index.md)



<!--HONumber=Aug16_HO3-->


