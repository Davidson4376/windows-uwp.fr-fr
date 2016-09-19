---
author: mcleanbyron
ms.assetid: fcebd659-438b-4d03-bc73-6b662ed6f1f3
description: Learn about the end-to-end process for developing and publishing an app with ads.
title: Workflows for creating apps with ads
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 0f832674f0e635f609f09fa15acead109b1d67ff


---

# Workflows for creating apps with ads




To display ads in your apps, your app needs to be able receive ads from an ad network. Microsoft provides a web service that allows Windows app developers to receive ads. When users click the ad in your app, you (being the *publisher* of the ad) earn money from the creator of the ads (the *advertiser*). The money earned from advertisers is paid to you using your account.

The following high-level steps describe the general process of developing and publishing an app with ads.

1.  Development stage:

    * Set up your Windows Dev Center account.
    * Develop your app using test mode values.

2.  Ready to release:

    * Configure your app to receive live ads.
    * Submit your app to Windows Dev Center and review performance data.

For more information about each step, read its corresponding section below.

## Set up your Windows Dev Center account

You need to have an account with Windows Dev Center to publish your app and receive ads. This is true regardless of whether or not you are using ad mediation. Advertising-related app management is also done in Windows Dev Center. If you have used Microsoft pubCenter to manage advertising in your apps, this has been replaced with features in Windows Dev Center

To set up your account with Windows Dev Center, visit the [home page](https://dev.windows.com/windows-apps). Additional information is available at the Windows Dev Center [help pages](https://dev.windows.com/develop).

## Develop your app using test mode values

Use the instructions in the following walkthroughs to add an [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) or [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) to display ads in your app:

-   [Interstitial Ads](interstitial-ads.md)
-   [AdControl in XAML and .NET](adcontrol-in-xaml-and--net.md)
-   [AdControl in HTML 5 and Javascript](adcontrol-in-html-5-and-javascript.md)
-   [AdControl in Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md)

When you use an **AdControl** or **InterstitialAd** to display ads in your app, you must specify an application ID and ad unit ID in your code to link your app to your Windows Dev Center account and to serve ads. While you are developing your app, use test application ID and ad unit ID values to see how your app renders ads during testing. This enables you to see how the app is receiving and rendering advertisements during testing. For more information, see [Test mode values](test-mode-values.md).

For complete sample projects that demonstrate how to add banner and video interstitial ads to JavaScript/HTML apps and XAML apps using C# and C++, see the [advertising samples on GitHub](http://aka.ms/githubads).

## Configure your app to receive live ads

After you finish testing your app and you are ready to submit it to Windows Dev Center, you must update your app code to use application ID and ad unit ID values from the [Windows Dev Center dashboard](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). If you try to use test values in your live app, your app will not receive live ads. For more information, see [Set up ad units in your app](set-up-ad-units-in-your-app.md).

## Submit your app

After you complete development of your app, you can publish your app in the Windows Store by using the Windows Dev Center dashboard. In addition to meeting requirements for all apps in the Windows Store, apps that display ads must meet several additional requirements. For more information, see [Submit an app with ads to the Windows Store](submit-an-app-with-ads-to-the-windows-store.md).

After your app is published and available in the Windows Store, you can review your [advertising performance reports](../publish/advertising-performance-report.md) in the Dev Center dashboard.

 

 



<!--HONumber=Aug16_HO3-->


