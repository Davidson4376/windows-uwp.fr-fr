---
If your app uses ad mediation or displays banner or video interstitial ads from Microsoft Advertising, use the Monetization &gt; Monetize with ads page to manage your use of ads.
Monetize with ads
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
---

# Monetize with ads


If your app uses ad mediation or displays banner or video interstitial ads from Microsoft Advertising, use the **Monetization** &gt; **Monetize with ads** page to manage your use of ads.

## Windows ad mediation


If your app uses ad mediation, use this section to configure your mediation settings and add required parameters for each of the ad networks used by your app. For more information about the options in this section, see [Submit your app and configure ad mediation](https://msdn.microsoft.com/library/windows/apps/mt219689).

Ad mediation lets you optimize your in-app advertising revenue by mediating banner ad requests from multiple ad networks. For more information about ad mediation, see [Use ad mediation to maximize revenue](https://msdn.microsoft.com/library/windows/apps/mt219691).

## COPPA compliance


For purposes of the Children's Online Privacy Protection Act (“COPPA”), you must notify Microsoft if your app is directed at children under the age of 13. If you use Dev Center to indicate to Microsoft that your app is directed at children under the age of 13, Microsoft will take steps to disable its behavioral advertising services when delivering advertising into your app. If your app is directed at children under the age of 13, you have certain obligations under COPPA.

For more information on your obligations under COPPA, please see [this page](http://go.microsoft.com/fwlink/p/?linkid=536558).

## Microsoft Advertising ad units


Use this section to create a Microsoft Advertising ad unit. You only need to create ad units in the following scenarios:

-   Your app shows banner ads from Microsoft Advertising by using an [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) object.
-   Your app shows video interstitial ads from Microsoft Advertising by using an [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) object.

To create an ad unit for these scenarios:

1.  Name the ad unit.
2.  Select the ad unit type (**Banner** or **Video interstitial**).
3.  Select the device type (**Mobile** or **PC/Tablet**).
4.  Click **Create ad unit**.

Your ad units appear in a table at the bottom of this section. For each ad unit you will see an **Application ID** and an **Ad unit ID**. To show ads in your app, you'll need to use these values in your code:

-   If your app shows banner ads, assign these values to the [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) and [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) properties of your [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) object.
-   If your app shows video interstitial ads, pass these values to the [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) method of your [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) object.

> **Note**  If your app uses ad mediation to show banner ads from Microsoft Advertising (that is, it uses an **AdMediatorControl** object), you do not need to request ad units. In this scenario, Microsoft Advertising ad units are automatically generated for you.

 

 

 




<!--HONumber=Mar16_HO1-->
