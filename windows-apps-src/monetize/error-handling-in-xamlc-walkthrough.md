---
author: mcleanbyron
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: Learn how to catch AdControl errors in your app.
title: Error handling in XAML/C# walkthrough
translationtype: Human Translation
ms.sourcegitcommit: 90c866fcdb4df0f32a4ace0cb4f6b761d6e9170e
ms.openlocfilehash: bca54776fb4793fbc9e0b9af070a0cc676168d86


---

# Error handling in XAML/C# walkthrough




This topic demonstrates how to catch [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) errors in your app.

These examples assume that you have a XAML/C# app that contains an **AdControl**. For step-by-step instructions that demonstrate how to add an **AdControl** to your app, see [AdControl in XAML and .NET](adcontrol-in-xaml-and--net.md). For a complete sample project that demonstrates how to add banner ads to a XAML app using C# and C++, see the [advertising samples on GitHub](http://aka.ms/githubads).

1.  In your MainPage.xaml file, locate the definition for the **AdControl**. That code looks like this.

    ``` syntax
    <UI:AdControl
       ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
       AdUnitId="10865270"
       HorizontalAlignment="Left"
       Height="250"
       Margin="10,10,0,0"
       VerticalAlignment="Top"
       Width="300" />
    ```

2.   After the **Width** property, but before the closing tag, assign a name of an error event handler to the [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx) event. In this walkthrough, the name of the error event handler is **OnAdError**.

    ``` syntax
    ErrorOccurred="OnAdError"
    ```

    The complete XAML code that defines the **AdControl** looks like this.

    ``` syntax
    <UI:AdControl
       ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
       AdUnitId="10865270"
       HorizontalAlignment="Left"
       Height="250"
       Margin="10,10,0,0"
       VerticalAlignment="Top"
       Width="300"
       ErrorOccurred="OnAdError"/>
    ```

2.  To generate an error at runtime, create a second **AdControl** with a different application ID. Because all **AdControl** objects in an app must use the same application ID, creating an additional **AdControl** with a different application id will throw an error.

    Define a second **AdControl** in MainPage.xaml just after the first **AdControl**, and set the [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) property to zero (“0”).

    ``` syntax
    <UI:AdControl
      ApplicationId="0"
      AdUnitId="10865270"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,265,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError" />
    ```

3.  In MainPage.xaml.cs, add the following **OnAdError** event handler to the **MainPage** class. This event handler writes information to the Visual Studio **Output** window.

    ``` syntax
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  Build and run the project.

After the app is running, you will see a message similar to the one below in the **Output** window of Visual Studio.

``` syntax
AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
```

## Related topics

* [Advertising samples on GitHub](http://aka.ms/githubads)

 



<!--HONumber=Sep16_HO3-->


