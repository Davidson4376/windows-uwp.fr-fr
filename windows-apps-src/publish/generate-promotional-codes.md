---
author: jnHs
Description: You can generate promotional codes for an app or add-on that you have published in the Windows Store.
title: Generate promotional codes
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
translationtype: Human Translation
ms.sourcegitcommit: a92f642b2d28eb801106388648455752c10e013a
ms.openlocfilehash: cf1f6cf680d4ae57513dde2c6a356e8ba4b1cb56

---

# Generate promotional codes


You can generate promotional codes for an app or add-on that you have published in the Windows Store. Promotional codes are an easy way to give influential users free access to your app or add-on. You might also use promotional codes to address customer service scenarios by giving users free access to your app or add-on, or for [beta testing](beta-testing-and-targeted-distribution.md) with Windows 10.

Each promotional code has a corresponding unique redeemable URL that you can distribute to a single user or to a group of users. The user can simply click the URL to redeem the code and install your app or add-on from the Windows Store.

On the Windows Dev Center dashboard, you can:

-   Order a set of promotional codes for your app.
-   Download a fulfilled promotional codes order.
-   Review promotional code usage for your apps, including:
    -   Summaries of promotional code orders for all your apps (on the **Dashboard overview** page) and for each app individually (on the **App overview** page for each app).
    -   A detailed summary of promotional code orders for each app (on the **Promotional codes** page for each app).

> **Note**  You can generate promotional codes even if you have selected the **Hide this app and prevent acquisition. Customers with a promotional code can still download it on Windows 10 devices** option on the [Pricing and availability](set-app-pricing-and-availability.md) dashboard page for your app. Your app must pass the final publishing phase of the [app certification process](the-app-certification-process.md) before users can redeem a promotional code to install it.

## Promotional code policies


Be aware of the following policies for promotional codes:

-   You can generate promotional codes for any app or add-on that you published to the Windows Store. Users can redeem the codes on any versions of Windows that are supported by your app or add-on.
-   Promotional codes expire 6 months after the date you order them (unless you choose an earlier expiration date).
-   For each of your apps or add-ons, you can generate up to 500 promotional codes every 6 months. The 6 month period begins when the first promotional code order is submitted.
-   You must follow the requirements defined in the [App Developer Agreement](https://msdn.microsoft.com/library/windows/apps/hh694058), including section **3k. Promotional Codes**.

## Order promotional codes


To order promotional codes for an app or add-on that you published to the Windows Store:

1.  On Windows Dev Center dashboard, do one of the following:
    -   On the **App overview** page for your app, locate the **Promotional codes** section and click **Manage codes**.
    -   On any dashboard page for your app, in the left navigation menu, expand **Monetization** and click **Promotional codes**. On the **Promotional codes** page, click **Order codes**.

2.  On the **New promotional codes order** page, enter the following:
    -   Select the app or add-on for which you want to generate codes.
    -   Specify a name for the order. You can use this name to differentiate between different orders of codes when reviewing your promotional code usage data.
    -   Select the order type. You can choose to generate a set of promo codes that can each be used once or you can choose to generate one promo code that can be used multiple times.
    -   Specify the quantity of codes to order.
    -   Specify when the promotional codes should become active. To choose a specific start date and time, clear the **Codes are active immediately** check box.
    -   Specify when the promotional codes should expire. To choose a specific expire date and time, clear the **Codes expire after 6 months** check box.

3.  Click **Order codes**. The order is submitted and the dashboard navigates you to the **Promotional codes** page, where the new order is listed as **Pending** in the summary table of promotional code orders.

Promotional codes are usually available for download within 60 minutes of placing the order, although some orders may take longer to process. After your order is fulfilled and the codes are available for download, the order status changes to **Available**.

## Download and distribute promotional codes


To download a fulfilled promotional codes order and distribute the codes to users of your app:

1.  On Windows Dev Center dashboard, return to the **Promotional codes** page for your app (expand **Monetization** and click **Promotional codes**).
2.  Confirm that your order's status is **Available**. Click the **Download** link for your order and save the provided file to your computer. This file contains information about your promotional codes order in tab-separated value (TSV) format.
3.  Open the TSV file in the editor of your choice. For the best experience, open the TSV file in an application that can display the data in a tabular structure, such as Microsoft Excel. However, you can alternatively open the file in any text editor.

    The file contains the following columns of data for each code:

    -   **Product name**: The name of the app or add-on that the code is associated with.
    -   **Order name**: The name of the order in which this code was fulfilled.
    -   **Promotional code**: The code itself. This is a 5x5 string of alphanumeric characters separated by hyphens. For example:

        DM3GY-M2GYM-6YMW6-4QHHT-23W2Z

    -   **Redeemable URL**: The URL that a user can use to redeem the code and install your app or add-on. The URL has the following format:

        https://account.microsoft.com/billing/redeem?mstoken=&lt;promotional_code>

    -   **Start date**: The date this code starts.
    -   **Expire date**: The date this code expires.
    -   **Code ID**: A unique ID for this code.
    -   **Order ID**: A unique ID for the order in which this code was fulfilled.
    -   **Given to**: An empty field that you can fill with a value that identifies the user you gave the code to.
    -   **Available**: The number of codes still available to redeem.
    -   **Redeemed**: The number of codes that have been redeemed.

4.  Distribute the redeemable URLs to your users via any communication format you prefer (for example, email, SMS message, or printed cards). We recommend that your communication includes the following:
    -   An explanation of which app or add-on the promotional code is for, and optionally a description of why the user is receiving the code.
    -   The redeemable URL for the code.
    -   Instructions that guide the user to visit the redeemable URL, log in using their Microsoft account, and follow the instructions to download and install your app.

## Code redemption user experience


After you distribute a redeemable URL to a user, the following steps describe the experience the user will follow to redeem your app.

1.  The user clicks the redeemable URL.

    The browser opens to an authenticated **Redeem your code** page at <https://account.microsoft.com/billing/redeem>. This page includes a description of the app the user is about to redeem.

2.  The user clicks **Redeem.**

    The browser navigates to a **Thank you** page with a **Get** ***&lt;your app name&gt;*** link.

    > **Note**  Users will receive an error at this step if your app is not yet published.

3.  The user clicks **Get** ***&lt;your app name&gt;***.

4.  If the user is on a computer with the Windows Store for Windows 10 or Windows 8.1 installed, the Windows Store opens to the overview page for the app. The user can click **Install** to install the app for no charge.

    If the user is on a computer or device that does not have the Windows Store installed, the browser opens to the Windows Store web page for the app. The user can click **Install** to install the app for no charge.

    > **Note**  In some cases the app page might display a **Buy** button instead of **Install**, even though the app was successfully redeemed via the promotional code. The user can click **Buy** to install the app for no charge.

## Review your promotional codes


There are several different ways to review your promotional code usage.

-   To review a summary of promotional code orders for all your apps, visit the **Dashboard overview** page and locate the **Promotional codes** section on this page. This section displays the remaining active promotional codes for all your apps, the total number of redeemed promotional codes for all your apps, and the total number of promotional code orders you have placed for all your apps.
-   To review a summary of promotional code orders for a specific app, navigate to the **App overview** page for the app and locate the **Promotional codes** section on this page. This section displays the remaining active promotional codes for the app, the total number of redeemed promotional codes for the app, and the total number of promotional code orders you have placed for the app.
-   To review a detailed summary of promotional code orders for a specific app, navigate to the **Promotional codes** page for your app (expand **Monetization** and click **Promotional codes**). You can review the following details for all current and inactive promotional codes for the app:
    -   Order name
    -   App or add-on name
    -   Order date
    -   Expire date
    -   Status

You can also download an active order from this table.

 

 







<!--HONumber=Aug16_HO4-->


