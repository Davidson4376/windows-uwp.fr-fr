---
author: jnHs
Description: You can create an ad campaign using the Dev Center dashboard to help promote your app and grow your app&quot;s user base.
title: Create an ad campaign for your app
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
translationtype: Human Translation
ms.sourcegitcommit: 65b82f422e602515e9531664e35f1e1c1e9f5932
ms.openlocfilehash: 3ea67f9e4f0d834bd77ef116c5e0b16008f4ae5f

---

# <a name="create-an-ad-campaign-for-your-app"></a>Create an ad campaign for your app


You can create an ad campaign using the Dev Center dashboard to help promote your app and grow your app's user base. By default, we will choose the target audience for your ads based on the settings for your app in the Dev Center dashboard, but you can optionally define your own audience. You can also use a default set of ad templates or upload your own ad designs. For more details about ad campaigns, see [Common questions about ad campaigns](common-questions.md).

> **Note**  You can create ad campaigns only for apps that have passed the final publishing phase of the [app certification process](the-app-certification-process.md).

Here's how to create an ad campaign to promote your app.

1.  From the left navigation menu on your app's page in the Dev Center dashboard, click **Monetization** &gt; **Promote your app**.
2.  Do one of the following:

    -   If you have not yet created an ad campaign for this app, the **Promote your app** page displays information about the benefits of ad campaigns. Click **Get started** or **Create an ad campaign**.
    -   If you have already created an ad campaign for this app, the **Promote your app** page lists your existing ad campaigns. Click **New campaign**.
3.  On the **New campaign** page, in the **Campaign objective** section, choose one of the following:
    -   **Increase installs for your app**. Select this option if your ad campaign is intended to get people to install your app.
    -   **Increase engagement in your app**. Select this option if your ad campaign is intended to get your customers to increase their usage of your app. When you select this option, you can target your ad campaign at specific [customer segments](create-customer-segments.md) that you define.

4.  In the **Campaign details** section, define the overall settings for your campaign.
    -   Name your ad campaign in the **Campaign name** field.
    -   Under **Campaign type**, choose one of these options:
        -   **Paid**: These ads will run in any app that matches your app’s device and category.
        -   **Community (free)**: These ads will run in apps published by other developers who also create community ad campaigns. Before you can select this option, you must first check the **Show community ads in my app** box in the **Monetize with ads** page of the dashboard. For more information, see [About community ads](about-community-ads.md).
        -   **House (free)**: These ads will only run in your apps (that match the advertised app’s device). House ads are free of charge. For more information, see [About house ads](about-house-ads.md).
    -   Under **Campaign duration**, choose one of these options:
        - **Custom**. If you choose this option, your campaign budget will be spent during the date and time range you specify. This option is only available to developers who have a premium account. For more info about premium accounts, see [Common questions about ad campaigns](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).
        - **Monthly**. If you choose this option, your campaign budget will be spent every month on a recurring basis until you stop the campaign.

    > **Note**  If your app is not yet published, you will receive an error message on the **New campaign** page. You must wait for your app to be published before you can create an ad campaign for it.

5.  If you chose **Increase app installations** as your campaign objective, we will choose the audience for your ads, based on the settings you selected when creating the app in the Dev Center dashboard. If you would rather choose the audience for your ads yourself, select **Manual** to expand the **Audience** section. If you want to go back to default targeting, select **Automatic**.

    If you select **Manual**, you can edit the following targeting information:

    -   Choose the countries or regions in which you want these ads to appear. You can choose up to 5. For a list of the supported countries or regions, see [Common questions about ad campaigns](common-questions.md#where-will-my-ad-appear).
    -   Choose the device types on which you want these ads to appear. Only the device types supported by your app are shown.
    -   Choose the operating system. Only the operating systems supported by your app are shown.
    -   Select the gender and age range of your desired audience.

    This section also displays an **Estimated Reach** graph. This graph shows the audience you can reach with your current targeting selections as a percentage of all Windows ad-enabled app users in the selected markets.

6.  If you chose **Increase app engagement** as your campaign objective, you can select one of your customer segments to target.

    > **Note**  Ads created using this campaign will be shown only to the customers who are included in the segment. Only one segment can be selected per ad campaign. For info about segments, see [Create customer segments](create-customer-segments.md).


7.  In the **Ad design** section, choose one of these options:
    -   **Custom**. Choose this option to use your own ad designs. Note that if you selected a customer segment in Step 6, you must use custom creatives. You can upload different files for each of the available ad sizes. The files must meet the following requirements and guidelines:
        -   Each file must be a .png or .jpg that is 40 KB or smaller.
        -   Your ad designs must meet the requirements specified in the [Microsoft Creative Acceptance Policy](http://go.microsoft.com/fwlink?LinkId=532595).
        -   The content in your ad designs must be relevant to the app you are promoting. Ad designs that are not related to the app will not be distributed to ads in other apps.
        -   All content in your ad designs should be clearly legible. For example, content should not be blurred, pixelated or stretched.
    -   **Auto-generated**. Choose this option to use ads from a list of default templates. You have the following options to customize the content in the ads. As you make selections, the previews of your ads will update automatically.
        -   In the **Language** drop-down, select the language of the ads. The text for the Windows Store badge and your custom tag line text (if specified) will show in the language you select.
        -   To add an extra line of text to your ad, enter the text in the **Custom tag line** field.
            > **Note**  The text you enter must be localized into the selected language. The custom tag line will be rejected if the text does not align with [Bing Ads policies](http://go.microsoft.com/fwlink?LinkId=398341). Consult this page for guidance on style and disallowed content.

        -   To further customize the ad, expand **Customize ad design / See all ad sizes** and choose any of the following:
            - **Background color**. Choose from the available options.
            - **Images**. The available images are the images you have associated with your app in the Store.
            - **Show my app rating**. Select this checkbox if you want to show the app's rating.
            - **Show that my app is free**. If your app is free in all the selected markets, you will also have the option to select this checkbox.
            - **Call to action**. If you chose **Increase app engagement** as your campaign objective, you can set your ad's call to action button to **Open**, **Play**, **Read**, **Listen**, or **Shop**.  

8.  If you have a [premium account](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign), you can use the **Destination URL** box to control what happens when a customer clicks your ad.
    - If you leave the box empty, when a customer clicks your ad, your app's Store listing will be displayed.
    - If you are using Kochava or Tune to measure install analytics for your app, enter your install tracking URL from Kochava or Tune. When you save the campaign, the tracking URL is validated to make sure that it resolves to the listing page for your app in the Windows Store. For more information about install tracking with Kochava and Tune, see the [Kochava](http://support.kochava.com/) and [Tune](https://help.tune.com/) documentation.
    - If you chose **Increase app engagement** as your campaign objective, you can specify a [deep-link URI](../launch-resume/handle-uri-activation.md) to redirect customers in the selected segment to a specific page within your app.
    - If you specify any destination that is not your app description page or a page inside of your app, your campaign will automatically be paused.

9.  Now choose your ad campaign's financial settings in the **Budget and payment** section.
   > **Note**  If you are creating a house campaign or community campaign, the **Budget and payment** section will not appear, since these campaigns are free of charge.

    -   Under **Budget**, use the slider to set the amount of money you want to spend each month to run this ad.

        The monthly budget is prorated for the month in which the ad campaign is created. In other words, if you create an ad campaign halfway through a calendar month, you will be charged for half of your monthly budget for that month.

    -   Set a payment instrument for your ad campaign by clicking **Add new payment instrument** and fill in your account details.
        > **Important**  The country/region of your payment instrument's billing address must match the country/region associated with your Dev Center account.
-   If you have received a coupon from a Microsoft representative to pay for an ad campaign, click **Use a coupon**, enter the coupon code, and click **Apply** to apply the coupon to the campaign.

10.  Finally, click **Review** to confirm your ad campaign's settings and, if it's a paid ad campaign, its budget and payment information. Click **Confirm** and your ads will typically start appearing on devices within a few hours.
   > **Tip**  To see how your campaigns are performing, in the top navigation menu of the dashboard, select **Promotions**. Select **Section filters** to scope what's included in the report by **Date**, **Campaign objective**, **App name**, **Campaign type**, or **Status**. In addition to seeing info about your campaign's **Impressions**, **Clicks**, **Conversions**, and **Spend**, you can use the report to **Pause** or **Resume** a campaign. To edit a campaign, select its name in the list.

## <a name="related-topics"></a>Related topics

* [Managing your ad campaign](managing-your-ad-campaign.md)
* [About house ads](about-house-ads.md)
* [App install ads report](app-install-ads-reports.md)
* [Common questions about ad campaigns](common-questions.md)
 

 



<!--HONumber=Dec16_HO1-->


