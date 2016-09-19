---
author: jnHs
Description: The Reviews report in the Windows Dev Center dashboard lets you see the comments that customers entered when rating your app in the Store.
title: Reviews report
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
translationtype: Human Translation
ms.sourcegitcommit: ccadaad34ac0854ab95646eda4e3451d1b178b7e
ms.openlocfilehash: d08eb446977ebab2eeee346f8f17ff79ae57c19b

---

# Reviews report


The **Reviews** report in the Windows Dev Center dashboard lets you see the comments that customers entered when rating your app in the Store. You can view this data in your dashboard, or [download the report](download-analytic-reports.md) to view offline. Alternatively, you can programmatically retrieve this data by using the [Windows Store analytics REST API](../monetize/access-analytics-data-using-windows-store-services.md).

> **Note**  You can also [respond to customer reviews](respond-to-customer-reviews.md) from this page.

This report shows the number of stars that a customer rated your app when leaving a review, but does not analyze star ratings across your app; for statistics about your ratings, see the [Ratings report](ratings-report.md).

Note that customers can leave a rating for your app without adding any comments, so you will typically see fewer reviews than ratings. By default, this page also shows ratings that don't include review content, but you can use the **Page filters** to show only ratings that include reviews, as described below.

Each customer review contains:

-   The title and review text provided by the customer. (Reviews written by customers on Windows Phone 8.1 and earlier will not have a title.)
-   The date of the review.
-   The name of the reviewer as it appears in the Windows Store.
-   The reviewer's country/region.
-   The package version of the app on the customer's device at the time the review was left. (This info is not available for reviews submitted online or submitted by customers on Windows 8.1 and earlier.)
-   The OS version of the device which the customer was using when the review was left.
-   The name of the device which the customer was using when the review was left. (This info is not available for reviews submitted online or submitted by customers on Windows 8.1 and earlier.)
-   The review's "usefulness count," as rated by other customers when reading that review. These are shown as a series of two numbers: the first number shows how many customers rated it as useful, and the second number is the total number of customers who rated the review. For example, a usefulness count of 4/10 means that out of 10 raters, 4 found the review useful and 6 did not. (If there are no usefulness votes for a review, no usefulness count is displayed.)

> **Note** You may occasionally see reviews disappear from this report. This can happen because Microsoft removes reviews from the Store that are written by customers running certain pre-release and Insider builds of Windows 10. We do this to reduce the possibility of a negative review that is caused by a problem in a pre-release Windows build. We may also remove reviews from the Store that have been identified as spam, inappropriate, offensive or have other policy violations. We expect this action will result in a better customer experience.

## Apply filters


Near the top of the page, you can expand **Apply filters** to filter all of the data on this page.

>**Tip**  If you don't see any reviews on the page, check to make sure your filters haven't excluded all of your reviews. For example, if you filter by a Target OS that your app doesn't support, you won't see any reviews.

-   **Rating**: By default all star ratings are checked, but you can check and uncheck specific ratings (from 1 to 5 stars) if you want to only see reviews associated with particular star ratings.
-   **From**: The default value (blank) will show reviews starting from when your app was published. You can choose a different date if you only want to see reviews that were left on or after the date you choose.
-   **To**: The default value (blank) will show reviews up to the current date. You can choose a different date if you only want to see reviews that were left before or on the date you choose. 
-   **Review content**: The default setting is **All**, which includes ratings without review text added. You can select **Ratings with review content** to only show ratings that include written review content.
-   **Target OS**: The default setting is **All**. You can choose a specific targeted operating system if you want this page to only show ratings from customers using your package(s) which target that OS.
-   **Responses**: The default setting is **All**. You can choose to filter the reviews to only show the reviews where you have [responded to customers](respond-to-customer-reviews.md), or only those where you have not yet responded.
-   **Updates**: The default setting is **All**. You can choose to filter the reviews to only show the reviews which have been updated by the customer since you [responded to a review](respond-to-customer-reviews.md), or only those which have not yet been updated by the customer.
-   **Market**: The default setting is **All markets**. You can choose a specific market if you want this page to only show reviews from customers in that market.
-   **Device type**: The default filter is **All devices**. You can choose a specific device type if you want this page to only show reviews left by customers using that type of device.
-   **Package version**: The default filter is **All packages**. You can choose a specific package if you want this page to only show reviews left by customers who had that package when they reviewed your app.

The info in all of the charts listed below will reflect the period of time selected in the **Apply filters** section, and will reflect any other filters you've chosen here.

> **Note**  The average rating that a customer sees in the Store takes into account the customer’s market and device type, and considers ratings over the past year, so it may differ from what you see in this report. To see how the average rating will appear in the Store for a given customer, you’ll need to apply filters to select a specific market and device type, and to set the **Date** to **Last 12 months**.

## Translating reviews


By default, reviews that were not written in your preferred language are translated for you. If you prefer, review translation can be disabled by unchecking the **Translate reviews** checkbox at the upper right, above the list of reviews.

Please note that reviews are translated by an automatic translation system, and the resulting translation may not always be accurate. The original text is provided if you wish to compare it to the translation, or translate it through some other means.

## Sorting reviews


You can sort the reviews on the page by date and/or by rating, in ascending or descending order. Click the **Sort by** link to view options to sort by Date and/or Rating. When you click a radio button in the Date or Rating section, the sorting criteria will be applied and you will see the sorting label shown next to the **Sort by** heading. You can remove the sorting criteria altogether by clicking the **X** that appears on each label.

## Responding to customer reviews

You can use the Windows Store Dev Center dashboard to send responses to many of your customers' reviews. For more info, see [Respond to customer reviews](respond-to-customer-reviews.md).

Here are some additional actions you may wish to consider, based on the ratings and reviews you're seeing.

-   If you notice many reviews that suggest a new or changed feature, or complain about a problem, consider releasing a new version that addresses the specific feedback. (Be sure to update your app's [description](create-app-descriptions.md) to indicate that the issue has been fixed.)
-   If the average rating is high, but your number of downloads is low, you might want to look for ways to [expose your app to more people](app-promotion-and-customer-engagement.md), since it's been well-received by those who have tried it out.


 

 

 



<!--HONumber=Aug16_HO5-->


