---
author: jnHs
Description: The Acquisitions report in the Windows Dev Center dashboard lets you see who has acquired your app, along with demographic and platform details.
title: Acquisitions report
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 2556e7bd8f827287c917c3b9ffa2da980d0d5d88

---

# Acquisitions report


The **Acquisitions** report in the Windows Dev Center dashboard lets you see who has acquired your app, along with demographic and platform details. You can view this data in your dashboard, or [download the report](download-analytic-reports.md) to view offline. Alternatively, you can programmatically retrieve this data by using the [Windows Store analytics REST API](../monetize/access-analytics-data-using-windows-store-services.md).

In this report, an acquisition means a new customer has obtained a license to your app (whether you charged money or you've offered it for free).

> **Important**  The **Acquisitions** report does not include data about refunds, reversals, chargebacks, etc. To estimate your app proceeds, visit [Payout summary](payout-summary.md). In the **Reserved** section, click the **Download reserved transactions** link.



## Apply filters


Near the top of the page, you can expand **Apply filters** to filter all of the data on this page by date range and/or by device type.

-   **Date**: The default filter is **Last 30 days**, but you can expand this up to **Last 12 months**.
-   **Device type**: The default setting is **All devices**. If you want to show data for acquisitions from a certain device type only, you can choose a specific one here.

The info in the charts listed below will reflect the period of time selected in the **Apply filters** section.

The info in all of the charts listed below will reflect the period of time selected in the **Apply filters** section. By default this will include data for all device types, unless you've used **Apply filters** to choose only one.

## Acquisitions


The **Acquisitions** chart shows the number of daily or weekly acquisitions of your app over the selected period of time. (When you use **Apply filters** to filter the data over a longer duration, the data will be grouped by week.)

You can also see the lifetime number of acquisitions for your app. This shows the cumulative total of all acquisitions, starting from when your app was first published.

You can optionally filter the results by market and/or by OS version.

## Customer demographic


The **Customer demographic** chart shows demographic info about the people who acquired your app. You can see how many acquisitions (over the selected period of time) were made by people in a certain age group and by which gender.

> **Note**  Some customers have opted not to share this info. If we were unable to determine the age group or gender, the acquisition is categorized as **Unknown**.

 

## Markets


The **Markets** chart shows the total number of acquisitions over the selected period of time by market. By default, we show you the market which had the most acquisitions on top and continue downward from there. You can reverse this order by toggling the arrow in the **Acquisitions** column of this chart.

## OS version


The **OS version** chart shows the total number of acquisitions based on the customer's operating system (or via [volume acquisition by organizations](organizational-licensing.md)). In some cases we may not be able to determine this info. In that case, the OS version will be listed as **Unknown**.



 

 



<!--HONumber=Aug16_HO3-->


