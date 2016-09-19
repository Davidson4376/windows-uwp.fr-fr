---
author: jnHs
Description: In addition to creating an ad campaign for your app that will run in Windows apps, you can also promote your app using other channels.
title: Create a custom app promotion campaign
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
translationtype: Human Translation
ms.sourcegitcommit: 3afdf00864e023d913b635beef0c506735881b23
ms.openlocfilehash: a6e97968df4e9ab986d364b2573a31b4ba9d1958

---

# Create a custom app promotion campaign



In addition to creating an [ad campaign for your app](create-an-ad-campaign-for-your-app.md) that will run in Windows apps, you can also promote your app using other channels. For example, you can promote your app using a third-party app marketing provider, or you might post links to your app on social media sites. These activities are called *custom campaigns*.

If you run custom campaigns for your app, you can track the relative performance of each campaign by creating a different Windows Store app URL for each custom campaign, where each URL contains a different campaign ID. When a customer running Windows 10 clicks a URL that contains a campaign ID, Microsoft associates the click with the corresponding custom campaign and makes this data available to you.

There are two main types of data associated with custom campaigns: page views for your app and *conversions*. A conversion is an app install that results from a customer clicking on the Windows Store page for your app from a URL that is promoted via a custom campaign. For more details about conversions, see [Understanding how app installs qualify as conversions](#understanding-how-app-installs-qualify-as-conversions) in this topic.

You can retrieve custom campaign performance data for your app in the following ways:

-   If your app is a Universal Windows Platform (UWP) app, it can use the [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) method to programmatically retrieve the custom campaign ID that resulted in a conversion.
-   You can view data about page views and conversions for your app or add-on from the [Channels and conversions report](channels-and-conversions-report.md) in the Dev Center dashboard.

> **Important**   This data will only be tracked for customers running Windows 10. Customers using other operating systems can still follow the link to your app's listing, but data about those customers' activities will not be included.

 

## Example custom campaign scenario


Consider a game developer who has finished building a new game and would like to promote it to players of her existing games. She posts the announcement of the new game release on her Facebook page, including a link to the Windows Store page for the game. Many of her players also follow her on Twitter, so she also tweets the announcement with the link to the Windows Store page for the game.

To track the success of each of these promotion channels, the developer creates two variants of the URL for the Windows Store page for the game:

-   The URL she will post to her Facebook page includes the custom campaign ID `my-facebook-campaign`.
-   The URL she will post to Twitter includes the custom campaign ID `my-twitter-campaign`.

As her Facebook and Twitter followers click on the URLs, Microsoft tracks each click and associates it with the corresponding custom campaign. Subsequent qualifying acquisitions of the game and any add-on purchases are associated with the custom campaign and reported as conversions.

## Understanding how app installs qualify as conversions


A *conversion* is an app install that results from a customer clicking on the Windows Store page for your app from a URL that is promoted via a custom campaign. There are different conditions for qualifying as a conversion for the [Channels and conversions report](channels-and-conversions-report.md) on the Dev Center dashboard and for qualifying as a conversion for [programmatically retrieving the campaign ID](#programmatically).

To qualify as a conversion for the [Channels and conversions report](channels-and-conversions-report.md), the following conditions must be met:

-   A customer with a recognized Microsoft account clicks an app URL that contains a custom campaign ID, and is redirected to the Windows Store page for the app.
-   The same customer (as identified by the same Microsoft account) installs the app within 24 hours after they first clicked the Windows Store URL with the custom campaign ID. This qualifies as a conversion even if the customer installs the app on a different computer or device than the one on which they clicked the Windows Store URL with the custom campaign ID.
    > **Note**  For app installs that are counted as conversions for a custom campaign, any add-on purchases in that app are also counted as conversions for the same custom campaign.

     

To qualify as a conversion when programmatically retrieving the campaign ID associated with the app, the following conditions must be met:

-   A customer with a recognized Microsoft account clicks an app URL that contains a custom campaign ID, and is redirected to the Windows Store page for the app.
-   The customer installs the app during the same Windows Store page view after they click the URL. If the user leaves the page and then returns to the page (either on the same computer or device or a different computer or device) within 24 hours and installs the app, this will qualify as a conversion on the [Channels and conversions report](channels-and-conversions-report.md), but this will not qualify as a conversion if you programmatically retrieve the campaign ID.

## Embed a custom campaign ID to your app's Windows Store page URL


To create a Windows Store page URL for your app with a custom campaign ID:

1.  Create an ID string for your custom campaign. This string can contain up to 100 characters, although we recommend that you define short campaign IDs that are easily identifiable.
2.  Get the Windows Store page URL for your app in HTML or protocol format. The HTML format URL is available on the [**App Identity** page in the Dev Center dashboard](link-to-your-app.md).
    -   Use the HTTP format if you want customers to navigate to your app's Windows Store page in a browser (this URL will also launch the Windows Store app to your app's listing, if the Windows Store app is installed). This URL has the format **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**. For example, the HTTP URL for Skype is `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`.
        > **Note**  HTTP format URLs can be used to navigate to the Windows Store in a browser on computers and tablets running Windows 7 and later, and phones running Windows Phone 8 and later.
-   Use the protocol format if you are promoting your app from other Windows apps that are running on a device or computer with the Windows Store app installed, and you want customers to open to your app's page in the Windows Store app. This URL has the format **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. For example, the protocol URL for Skype is `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.
3.  Append the following string to the end of the URL for your app:
    -   For an HTTP format URL, append **`?cid=*my custom campaign ID*`**. For example, if Skype introduces a campaign ID with the value **custom\_campaign**, the new HTTP URL including the campaign ID would be: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.
    -   For a protocol format URL, append **`&cid=*my custom campaign ID*`**. For example, if Skype introduces a campaign ID with the value **custom\_campaign**, the new protocol URL including the campaign ID would be: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

## Programmatically retrieve the custom campaign ID for an app


If your app is a UWP app, you can programmatically retrieve the custom campaign ID associated with your app by using the [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) method. This method makes many analytics and monetization scenarios possible. For example, you can find out if the current user acquired your app after discovering it through your Facebook campaign, and then customize the app experience accordingly. Alternatively, if you are using a third-party app marketing provider, you can send data back to the provider.

> **Note**  The [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) method will return a campaign ID string only if the customer clicks your URL with the embedded campaign ID, is redirected to the Windows Store page for your app, and then installs your app without leaving this page. If the user leaves the page and then later returns and installs the app, this will not qualify as a conversion when using **GetAppPurchaseCampaignIdAsync**. For more information, see [Understanding conversions](#conversions) in this topic.

 

The following example demonstrates how to use the [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) method to retrieve the custom campaign ID for the app. If the app was not installed as part of a successful conversion, this method returns an empty string.

``` csharp
string campaignId = await CurrentApp.GetAppPurchaseCampaignIdAsync();
```

``` cpp
HString campaignId;
HRESULT hr = CurrentApp::GetAppPurchaseCampaignIdAsync(campaignId.GetAddressOf());
```

The [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) method accesses data from the Windows Store. Follow these guidelines when using this method:

-   Wrap this method call in an asynchronous operation to allow for the call to complete.
-   If your app is not yet published to the Windows Store and you are testing your custom campaign, use the [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) method of the [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) class instead of the [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) class. Follow these guidelines:
    -   Add an **AppPurchaseCampaignId** element to the WindowsStoreProxy.xml file, as shown in the following example. Assign the element's value to the custom campaign ID. When you run the app, the [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) method will always return this value. For more information about the WindowsStoreProxy.xml file, see the documentation for [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).

    ```        XML
    <CurrentApp>
        ....
        <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
    </CurrentApp>
    ```
    
    -   To easily switch between using [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) and [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), we recommend that you add the following statements to your code to define a **Store** alias, and then qualify your [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) calls with the **Store** alias.

    ```        CSharp
    #if DEBUG
            using Store = Windows.ApplicationMode.Store.CurrentAppSimulator;
            #else
            using Store = Windows.ApplicationMode.Store.CurrentApp;
            #endif   
    ```

## Test your custom campaign


Before you promote a custom campaign URL, we recommend that you test your custom campaign by doing the following:

1.  Sign in to your Microsoft Account on the computer or device you are using for testing.
2.  Click your custom campaign URL. Make sure the Windows Store loads your app page correctly, and then close the Windows Store app or the browser page.
3.  Click the URL several more times, closing the Windows Store app or the browser page after each visit to your app's page. During one of the visits to your app's page, install your app to generate a conversion. Count the total number of times you clicked the URL.
4.  If you are programmatically retrieving the custom campaign ID in your app, test this operation using the [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) method of the [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) class instead of the [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) class.

 

 







<!--HONumber=Aug16_HO3-->


