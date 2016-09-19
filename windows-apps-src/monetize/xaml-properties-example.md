---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: Learn how to assign **AdControl** properties to values.
title: XAML properties example
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: fb0533aa0ea760bca686276f886f0afcb21bf6f7


---

# XAML properties example




The following XAML example demonstrates how to assign [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) properties to values. If a property is not set, the **AdControl** will use default values to create an ad that is consistent with the user experience of the app.

The values are examples. In your code you will set the values of these functions and properties appropriate for your app.

``` syntax
Width="300",
Height="250",
AdUnitId="10865270",
ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
IsAutoRefreshEnabled="false",
AdRefreshed="OnAdRefresh",
ErrorOcurred="OnAdError",
IsEngagedChanged="OnAdEngagedChanged"
```

## Related topics

* [Advertising samples on GitHub](http://aka.ms/githubads)

 



<!--HONumber=Aug16_HO3-->


