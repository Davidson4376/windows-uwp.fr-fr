---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: Learn how to programmatically create an **AdControl** using JavaScript.
title: Create an AdControl in Javascript
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 68bc124aea079bc60fa22e1e6a038caf95fe765c


---

# Create an AdControl in Javascript




This example shows how to programmatically create an [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) using JavaScript.

## HTML div for an AdControl

An **AdControl** needs to have a **div** on the html page that will show the ad. The code below provides an example of such a **div**.

``` syntax
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## JavaScript for creating an AdControl

The following example assumes that you are using an existing **div** in your HTML with the ID **myAd**.

Instantiate the **AdControl** in the **app.onactivated** function.

``` syntax
// TODO: This application has been newly launched. Initialize
// your application here.
var adDiv = document.getElementById("myAd");
var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "3f83fe91-d6be-434d-a0ae-7351c5a997f1",
        adUnitId: "10865270"
    });
myAdControl.isAutoRefreshEnabled = false;
myAdControl.onErrorOccurred = myAdError;
myAdControl.onAdRefreshed = myAdRefreshed;
myAdControl.onEngagedChanged = myAdEngagedChanged;
```

The values are examples. In your code you will set the values of these functions and properties appropriate for your app.

If you use this code and do not see ads, you can try inserting an attribute of **position:relative** in the **div** that contains the **AdControl**. This will override the default setting of the **IFrame**. Ads will be displayed correctly, unless they are not being shown due to the value of this attribute. Note that new ad units may not be available for up to 30 minutes.

## Related topics

* [Advertising samples on GitHub](http://aka.ms/githubads)

 

 



<!--HONumber=Aug16_HO3-->


