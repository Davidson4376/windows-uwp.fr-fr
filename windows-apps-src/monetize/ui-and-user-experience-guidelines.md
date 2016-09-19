---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: Learn about UI and user experience guidelines for ads in apps.
title: UI and user experience guidelines for ads in apps
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: d464a2de442e6f1833f429c8460c27bf85e577d1


---

# UI and user experience guidelines for ads in apps




## General UI resources for Windows apps

You can find information about how to design the look and feel for apps at [Design & UI](https://developer.microsoft.com/windows/design).

## AdControl best practices

* [AdControl best practices: DO](#adcontrolbestpracticesdo10)
* [AdControl best practices: DON'T](#adcontrolbestpracticesdont10)

<span id="adcontrolbestpracticesdo10"/>
### AdControl best practices: DO

* Design advertising into your experience. Give your designers a sample ad to plan how the advertising is going to look. Two examples of well-planned ads in apps are the ads-as-content layout and the split layout.

  To see how different ad sizes will look and function within your app, you can utilize our test mode ad units for Windows Phone, Windows 8.1, and Windows 10. When you’re done with the test mode ad units, remember to [update your app with real ad unit IDs](set-up-ad-units-in-your-app.md) before submitting the app for certification.

* Plan for times when no ads are available. There may be times when ads aren't being sent to your app. Lay out your pages in such a way that they look great whether they showcase an ad or not. For more information, see [Error handling](error-handling-with-advertising-libraries.md).

<span id="adcontrolbestpracticesdont10"/>
### AdControl Best Practices: DON'T

* Bolt advertising into open real estate. Ad space shouldn't be placed into the first open piece of real estate you can find. Instead, it should be incorporated into your app's overall design.

* Over-advertise and saturate your app. Too many ads in your app detract from its appearance and usability. You want to make money with advertising, but not at the expense of the app itself.

* Distract user from their core tasks. The primary focus should always be on the app. The ad space should be incorporated so it remains a secondary focus.

<span id="interstitialbestpractices10"/>
## Interstitial best practices

* [Interstitial best practices: DO](#interstitialbestpracticesdo10)
* [Interstitial best practices: AVOID](#interstitialbestpracticesavoid10)
* [Interstitial best practices: NEVER (Policy Enforced)](#interstitialbestpracticesnever10)

When used elegantly, video interstitial ads can vastly increase your app revenue, without negatively impacting user satisfaction. When used improperly, such ads can have the exact opposite effect.

Here we aim to help you achieve elegance. Since you know your app better than anyone, except where policy is concerned, we leave it up to you to make the best final decision. What’s most important to keep in mind is that your app ratings and revenue are tightly coupled.

<span id="interstitialbestpracticesdo10"/>
### Interstitial best practices: DO

* Fit interstitial ads within the natural flow of the app, such as between game levels.

* Associate ads with tangible upsides, such as:

    * Hints towards level completion.

    * Extra time to retry a level.

    * Custom avatar features, like a tattoo or hat.

* If your app requires that a video ad be watched to completion, mention that rule upfront so they aren’t surprised with an error message upon hitting the close button.

* Pre-fetch the ad (by calling [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx)) ideally 30-60 seconds before you need to show it.

* Subscribe to all four events exposed in the [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) class (**Canceled**, **Completed**, **AdReady**, and **ErrorOccurred**) and use them to make the right decisions for your app.

* Have some built-in experience to use in lieu of a server-matched ad. You’ll find this useful in a few scenarios:

    * Offline mode, when ad servers can’t be reached.

    * When the **ErrorOccurred** event fires.

    * If you opt to save user bandwidth based on [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx), there are APIs in the **ConnectionProfile** class which can help.

* Use the default (30s) timeout unless you have a valid reason to do otherwise, in which case don’t go below 10s.

    * Video ads take substantially longer to download than banners, especially in markets that don’t have high speed connections.


* Be mindful of the user’s data plan. For example, either don’t show, or warn user, before serving a video ad on a mobile device that is near/over its data limit. There are APIs in the [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) class which can help.

* Continuously improve your app after the initial submission. Look at the ad reports and make design changes to improve fill and video completion rates.

<span id="interstitialbestpracticesavoid10"/>
### Interstitial best practices: AVOID

* Overdoing it. Don’t force ads more than every 5 minutes or so, unless the user explicitly engages with an optional tangible benefit, beyond just playing the game.

* Video interstitials at app launch, since users may believe they clicked the wrong tile.

* Video interstitials at exit. This is bad inventory, since completion rates will be near zero.

    * Two or more video ads back to back.

    * Users will be frustrated to see the ad progress bar reset to the starting point.

    * Many will think it’s just a coding or ad serving bug.

* Fetching a video ad more than 5 minutes before calling [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

    * Good inventory will maximize the conversion of pre-fetched ads to billable impressions.


* Penalizing a user for failures in ad serving, such as no ad available. For example, if you show a UI option to “Watch an ad to get *xxx*”, you should provide *xxx* if the user did her part. Two options to consider:

    * Don’t include the option unless the [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) event has fired.

    * Have the app include a built-in experience that yields the same benefit as a real ad.

* Letting a user gain a competitive advantage in a multi-player game.

    * A better gun in a first-person shooter game would clearly fall into this bucket.

    * A custom shirt on the player’s avatar is fine, so long as it doesn’t provide camouflage!

<span id="interstitialbestpracticesnever10"/>
### Interstitial best practices: NEVER (Policy Enforced)

* Put any UI elements over the ad container.

    * Advertisers have paid for the full screen.


* Call **Show** while user is engaged with the app.

    * Because the **InterstitialAd** will create a full screen overlay, the user will find this jarring.

    * It may also lead to exaggerated click-through rates.

* Use ads to obtain anything that may be consumed as a currency or traded with other users.

 

 



<!--HONumber=Aug16_HO3-->


