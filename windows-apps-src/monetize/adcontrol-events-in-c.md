---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: Learn how to handle the events of the AdControl class.
title: AdControl events in C#
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 969d668c89b40e37245a8168879842159b4f5c14

---

# AdControl events in C\# #  




The following examples demonstrate how to handle the events of the [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) class. These examples assume that you have previously assigned the event handlers to the **AdControl** events in XAML. For more information about how to do this, see [XAML properties example](xaml-properties-example.md).

For more information about handling events in C#, see [Events and routed events overview (Universal Windows apps using C#/VB/C++ and XAML)](http://msdn.microsoft.com/library/windows/apps/hh758286).

## Examples


``` syntax
private void OnAdError(object sender, AdErrorEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}

private void OnAdRefresh(object sender, RoutedEventArgs e) {
  // place code here that you wish to execute when the ad refreshes.
 return;
}

private void OnAdEngagedChanged(object sender, RoutedEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}
```

## Related topics

* [Advertising samples on GitHub](http://aka.ms/githubads)
* [AdControl error handling](adcontrol-error-handling.md)
* [RoutedEventArgs Class](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Aug16_HO3-->


