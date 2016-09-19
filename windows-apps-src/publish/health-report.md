---
author: jnHs
Description: The Health report in the Windows Dev Center dashboard lets you get data related to the performance and quality of your app, including crashes and unresponsive events.
title: Health report
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 72c5974da441e76f2fad8e844d3391999e42cd72

---

# Health report


The **Health** report in the Windows Dev Center dashboard lets you get data related to the performance and quality of your app, including crashes and unresponsive events. You can view this data in your dashboard, or [download the report](download-analytic-reports.md) to view offline. Where applicable, you can view stack traces for further debugging. Alternatively, you can programmatically retrieve this data by using the [Windows Store analytics REST API](../monetize/access-analytics-data-using-windows-store-services.md).

> **Note**  If you had previously published apps and viewed performance data in the earlier dashboards, you may notice an increased number of crashes and events reported here. This is because we are able to include more data in this report to give you a more complete picture.

## Apply filters


Near the top of the page, you can expand **Apply filters** to filter all of the data on this page by date range and/or by package version.

-   **Date**: The default filter is **Last 72 hours**, but you can expand this up to **Last 6 months**.
-   **Package version**: The default setting is **All versions**. If your app includes more than one package version, you can choose a specific one.

The info in all of the charts listed below will reflect the period of time selected in the **Apply filters** section. By default this will include data for all of your package versions, unless you've used the **Apply filters** to choose only one.

## Total crashes and events


The **Total crashes and events** chart shows the number of daily crashes and events that customers experienced when using your app during the selected period of time. Each type of event that your app experienced is tracked separately: crashes, hangs, JavaScript exceptions, or memory failures.

You can optionally filter the results by market and/or by OS version.

## Crash and event breakdown


The **Crash and event breakdown** chart lets you see charts that track specific details related to the customers' configurations when a crash or unresponsive event occurred. Click the section headings to see details about:

-   OS version
-   Device type
-   Memory (MB)
-   Mass storage (GB)
-   CPU speed (GHz)

You can optionally filter the results by **Crash type**: crashes, hangs, JavaScript exceptions, or memory failures. (The default setting is to show all crash types.)

## Market


The **Market** chart shows the total number of crashes and events over the selected period of time by market. By default, we show you the market which had the most acquisitions on top and continue downward from there. You can reverse this order by toggling the arrow in the **Crashes** column of this chart.

## Failure log


If you have included PDB symbol files, the **Failure log** chart will show details related to occurrences of specific symbols, including the total number of crashes and the average daily number of crashes per symbol.

This info is based on a percentage of your total events. Near the top of the chart, we'll indicate what percentage of events was sampled to provide this data.

 

 



<!--HONumber=Aug16_HO3-->


