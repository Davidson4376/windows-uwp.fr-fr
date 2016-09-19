---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Learn how to add application ID and ad unit ID values from the Windows Dev Center dashboard to your app before you submit your app to the Store.
title: Set up ad units in your app
translationtype: Human Translation
ms.sourcegitcommit: c6e0cf98c6eb2cdc656d5b4555d794ff6a94d2bc
ms.openlocfilehash: 705955faf7ddd67f80098f8c3ac7b2844553de95


---

# Set up ad units in your app




When you use an [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) or [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) to display ads in your app, you must specify an application ID and ad unit ID. While you are developing your app, use the appropriate [test application ID and ad unit ID values](test-mode-values.md) to see how your app renders ads during testing.

After you finish testing your app and you are ready to submit it to Windows Dev Center, you must update your app code to use application ID and ad unit ID values from the [Windows Dev Center dashboard](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). If you try to use test values in your live app, your app will not receive live ads.

To set up the application ID and ad units for your live app:

1.  On the Windows Dev Center dashboard, select your app and then click **Monetization > Monetize with ads**.
2.  In the **Microsoft Advertising ad units** section on this page, create an ad unit. For the ad unit type, select **Banner** if you are using an **AdControl**, or select **Video interstitial** if you are using an **InterstitialAd**. For more information about this page, see [Monetize with ads](../publish/monetize-with-ads.md).

3.  For each generated ad unit, you will see an **Application ID** and an **Ad unit ID** on this page. To show ads in your app, you'll need to use these values in your apps code:

    * If your app shows banner ads, assign these values to the **ApplicationId** and **AdUnitId** properties of your **AdControl** object.

    * If your app shows video interstitial ads, pass these values to the **RequestAd** method of your **InterstitialAd** object.

 

## Related topics

[Test mode values](test-mode-values.md)


 

 



<!--HONumber=Aug16_HO3-->


