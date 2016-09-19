---
author: payzer
title: How to disable mouse mode
description: Instructions for disabling default mouse mode.
translationtype: Human Translation
ms.sourcegitcommit: b4df1f944d909640791e4ed7e3bcf8d8bdf7a0d1
ms.openlocfilehash: 91e530a3313d53c4e693b88a64b849f3188a72de

---

# How to disable mouse mode
Mouse mode is on by default for all applications, which means that all applications that have not opted out will receive a mouse pointer (similar to the one in the Edge browser on the console). We strongly recommend that you turn this off and optimize for directional controller navigation.   
   
## HTML   
To turn on directional controller navigation in a JavaScript Universal Windows Platform (UWP) app, use the [TVHelpers directional navigation](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) JavaScript library. Include the directional navigation JavaScript file in your app package, and add a reference to it in all of the HTML pages that require directional controller navigation:

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
For more details, see the [directional navigation wiki](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation).

If you instead want to turn off mouse mode and use the DOM or WinRT gamepad APIs directly, run the following for every page that requires it: 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   This property defaults to `mouse`, which enables mouse mode. Setting it to `keyboard` turns off mouse mode, and instead gamepad input generates DOM keyboard events. Setting it to `gamepad` turns off mouse mode and does not generate DOM keyboard events, and allows you to just use the DOM or WinRT gamepad APIs.

## XAML    
To turn off mouse mode, add the following to the constructor for your app:   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## C++/DirectX   
If you are writing a C++/DirectX app, there's nothing to do. Mouse mode only applies to HTML and XAML applications.

## See also
- [Best practices for Xbox](tailoring-for-xbox.md)
- [UWP on Xbox One](index.md)




<!--HONumber=Aug16_HO3-->


