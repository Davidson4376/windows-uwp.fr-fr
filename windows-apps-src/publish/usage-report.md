---
author: jnHs
Description: The Usage report in the Windows Dev Center dashboard lets you see how customers are using your app.
title: Usage report
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
translationtype: Human Translation
ms.sourcegitcommit: c413ff1d4fe709e92f7a306e671f9a4fe22a5999
ms.openlocfilehash: 21be2064914189abe8ef68c858d33346b947550c

---

# Usage report


The **Usage** report in the Windows Dev Center dashboard lets you see how customers on Windows 10 are using your app and get info about custom events that you've defined. You can view this data in your dashboard, or [download the report](download-analytic-reports.md) to view offline.

> **Note**  Previously, the **Usage** report only provided data if you activated the Visual Studio Application Insights SDK in your app. With the updated **Usage** report, this is no longer required.

## Apply filters


Near the top of the page, you can expand **Apply filters** to filter all of the data on this page by date range and/or by product group (related OS versions).

-   **Date**: The default filter is **Last 30 days**, but you can expand this up to **Last 3 months**.
-   **Package version**: The default setting is **All**. If your app includes more than one package, you can choose a specific one here.
-   **Device type**: The default setting is **All**, but you can choose to show data for only one specific device type.

The info in all of the charts listed below will reflect the period of time selected in **Apply filters**. By default this will include data for all of your package versions and supported device types, unless you've used the **Apply filters** section to filter for only one.

> **Note** Only usage data from customers on Windows 10 is included in this report.

## Total user sessions

The **Total user sessions** chart shows the number of daily user sessions for your app over the selected period of time.

Each user session represents a distinct period of time when a customer interacted with your app. Each user session is considered to end after a period of inactivity, so a single customer could have multiple user sessions over the same day. Note this chart does not track unique users for your app.

## Active users

The **Active users** chart shows the number of customers who used your app on a specific day during the selected period of time.

Each active user represents a customer who used your app that day. This chart does not track unique user sessions (that is, a customer is represented in this chart whether they used your app just once or multiple times that day).

## Custom events

The **Custom events** chart shows the total occurrences for any custom events that you have defined for your app. This may include multiple occurrences for the same customer.

Custom events are implemented using the [StoreServicesCustomEventLogger.Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) method in the [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md).



 



<!--HONumber=Aug16_HO3-->


