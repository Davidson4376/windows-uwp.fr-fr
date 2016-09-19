---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: Use the test application ID and ad unit ID values from this article to see how your app renders ads during testing.
title: Test mode values
translationtype: Human Translation
ms.sourcegitcommit: c6e0cf98c6eb2cdc656d5b4555d794ff6a94d2bc
ms.openlocfilehash: e1462ae48e8aae8f5ed0e5a7e46a6660bf33e786

---

# Test mode values




When you use an [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) or [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)  to display ads in your app, you must specify an application ID and ad unit ID. While you are developing your app, use the test application ID and ad unit ID values from this article to see how your app renders ads during testing.


If you try to use test values in your app after you publish it, your live app not receive ads. To receive ads in your published app, you must update your code to use an application ID and ad unit ID provided by the Windows Dev Center dashboard. For more information, see [Set up ad units in your app](set-up-ad-units-in-your-app.md).
 

Here are the test values to use for video interstitial and banner ads.

* For video interstitial ads:

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* For banner ads:

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>


> **Important**   The size of a live ad is defined by the **Width** and **Height** properties of the **AdControl**. For best results, make sure that the **Width** and **Height** properties in your code are one of the [supported ad sizes for banner ads](supported-ad-sizes-for-banner-ads.md). The **Width** and **Height** properties will not change based on the size of a live ad.



 

 



<!--HONumber=Aug16_HO3-->


