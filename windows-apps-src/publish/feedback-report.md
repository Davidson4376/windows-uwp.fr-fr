---
author: jnHs
Description: The Feedback report in the Windows Dev Center dashboard lets you see the problems, suggestions, and upvotes that your Windows 10 customers have submitted through Feedback Hub.
title: Feedback report
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
translationtype: Human Translation
ms.sourcegitcommit: 70020d3c6e0fb0fea321ce1951720803fd25f9c0
ms.openlocfilehash: e6266ff7c45a49b3eece8ffaf3d0603d55a04761

---

# Feedback report

The **Feedback report** in the Windows Dev Center dashboard lets you see the problems, suggestions, and upvotes that your Windows 10 customers have submitted through Feedback Hub. You can view this data in your dashboard, or export the data to view offline.

Encouraging your customers to give you feedback about your app is a great way to learn about the problems and features that are most important to them. When your customers know they can send you feedback directly, they may be less likely to leave that feedback as a negative review.

> **Note** You can also [respond to feedback](respond-to-customer-feedback.md) directly from this report to let your customers know you're listening.

You can use the Feedback API in the [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) to let customers [directly launch Feedback Hub from your app](../monetize/launch-feedback-hub-from-your-app.md). Keep in mind that any customer who has downloaded your app on a Windows 10 device that supports Feedback Hub has the ability to leave feedback for it by using the Feedback Hub app. Because of this, you may see customer feedback in this report, even if you have not specifically asked for feedback from within your app.

> **Tip** Feedback becomes especially valuable if you are using [package flighting](package-flights.md), since the Feedback report will show you the specific package that each customer had installed on their device when they left the feedback.

## Viewing feedback details

In the **Details** section of this report, you’ll find the individual feedback left by your customers. To the left of the feedback text, you’ll see the number of times the feedback was upvoted by other customers in the Feedback Hub. You can sort the feedback in three ways:

- **Upvoted** (default): Shows feedback that has been upvoted by other customers, starting with the feedback which received the most upvotes.
- **Trending**: Shows feedback that has been upvoted by other customers in the last seven days, starting with the feedback which has been getting the most recent activity.
- **Most recent**: Shows all feedback, starting with the feedback most recently left.

Next to each comment you’ll see the date on which the feedback was left, and the type of feedback. You’ll also see the customer’s market, the specific package of your app that was installed on the device they were using when they left the feedback, the type of that device, and **Windows Insider** if the customer submitting the feedback is a member of the Windows Insider program.


## Apply filters

Near the top of the page, you can expand **Apply filters** to filter all of the data on this page.

> **Tip** If you don't see any feedback on the page, check to make sure your filters haven't excluded all of your feedback. For example, if you filter by a **Device type** that your app doesn't support, you won't see any feedback.

- **Date**: The default filter is **All time**. You can select shorter timeframes ranging from **Last 30 days** up to **Last 12 months**.
- **Feedback type**: The default setting is **All**. You can select **Problem** or **Suggestion** to show only that type of feedback.
- **Device type**: The default setting is **All devices**. You can select **Phone**, **PC**, or **Tablet** to show only feedback left from that type of device.
- **Package version**: The default setting is **All packages**. You can select one of your packages to show only feedback left from customers who were using that particular package when they left feedback.
- **Market**: The default setting is **All markets**. You can choose a specific to show only feedback from customers in that market.
- **Group**: The default setting is **All**. You can choose to view only feedback submitted by [Windows Insiders](http://insider.windows.com).

## Translating feedback

By default, reviews that were not written in your preferred language are translated for you. If you prefer, feedback translation can be disabled by unchecking the **Translate reviews** checkbox at the upper right, above the list of feedback.

Please note that feedback is translated by an automatic translation system, and the resulting translation may not always be accurate. The original text is provided if you wish to compare it to the translation, or translate it through some other means.

## Launching Feedback Hub directly from your app

As noted above, we recommend incorporating a link to Feedback Hub directly in your app to encourage customers to provide feedback. For more info, see [Launch Feedback Hub from your app](../monetize/launch-feedback-hub-from-your-app.md).



<!--HONumber=Aug16_HO5-->


