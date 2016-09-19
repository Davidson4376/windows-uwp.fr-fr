---
author: jnHs
Description: The Add-on acquisitions report in the Windows Dev Center dashboard lets you see how many add-ons you've sold, along with demographic and platform details.
title: Add-on acquisitions report
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
translationtype: Human Translation
ms.sourcegitcommit: 0edf45e997f36a82a8bfcb92c1d8fd2c79242461
ms.openlocfilehash: 144a8400acf0333fcd50e698b333c02942081ef3

---

# Add-on acquisitions report


The **Add-on acquisitions** report in the Windows Dev Center dashboard lets you see how many add-ons you've sold, along with demographic and platform details. You can view this data in your dashboard, or [download the report](download-analytic-reports.md) to view offline. Alternatively, you can programmatically retrieve this data by using the [Windows Store analytics REST API](../monetize/access-analytics-data-using-windows-store-services.md).

In this report, an add-on acquisition means a customer has purchased an add-on from you. Multiple purchases of the same consumable add-on by the same customer are counted as separate add-on acquisitions.

> **Important**  The **Add-on acquisitions** report does not include data about refunds, reversals, chargebacks, etc. To estimate your app proceeds, visit [Payout summary](payout-summary.md). In the **Reserved** section, click the **Download reserved transactions** link.

## Apply filters


Near the top of the page, you can expand **Apply filters** to filter all of the data on this page by date range and/or by device type. You can also filter to show only data for a specific add-on.

-   **Date**: The default filter is **Last 30 days**, but you can expand this up to **Last 12 months**.
-   **Add-on**: The default filter is **All add-ons**. If you want to show acquisition data for only one of your add-ons, you can choose a specific one here.
-   **Device type**: The default setting is **All devices**. If you want to show data for add-on acquisitions from a certain device type only, you can choose a specific one here.

The info in the charts listed below will reflect the period of time selected in the **Apply filters** section.

The info in all of the charts listed below will reflect the period of time selected in the **Apply filters** section. By default this will include data for all device types, unless you've used **Apply filters** to choose only one.

## Add-on acquisitions


The **Add-on acquisitions** chart shows the number of daily or weekly acquisitions of your add-ons over the selected period of time. (When you use **Apply filters** to filter the data over a longer duration, the data will be grouped by week.)

You can also see the lifetime number of acquisitions for your add-ons. This shows the cumulative total of all acquisitions, starting from when your app was first published.

The chart also shows the price that a customer paid to acquire the add-on.

You can optionally filter the results by market and/or by OS version.

## Top add-ons

The **Top add-ons** chart shows the total number of acquisitions for each of your add-ons over the selected period of time by market. By default, we show you the add-on which had the most acquisitions on top and continue downward from there. You can reverse this order by toggling the arrow in the **Acquisitions** column of this chart.

## Markets

The **Markets** chart shows the total number of add-on acquisitions over the selected period of time by market. By default, we show you the market which had the most acquisitions on top and continue downward from there. You can reverse this order by toggling the arrow in the **Acquisitions** column of this chart.

## Customer demographic

The **Customer demographic** chart shows demographic info about the people who acquired your add-on. You can see how many acquisitions (over the selected period of time) were made by people in a certain age group and by which gender.

> **Note**  Some customers have opted not to share this info. If we were unable to determine the age group or gender, the acquisition is categorized as **Unknown**.

## OS version

The **OS version** chart shows the total number of acquisitions based on the customer's operating system (or via [volume acquisition by organizations](organizational-licensing.md)). In some cases we may not be able to determine this info. In that case, the OS version will be listed as **Unknown**.

 

 



<!--HONumber=Aug16_HO3-->


