---
author: jnHs
Description: To view performance data for the ad units in your apps, use the app-level and account-level advertising performance reports on the Windows Dev Center dashboard.
title: Advertising performance report
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
translationtype: Human Translation
ms.sourcegitcommit: 6b354b1b009bf9e4f2899f7ef97ef8791712f52b
ms.openlocfilehash: b75d9310e577c77d1caed3e9f34762e925eb11ae

---

# Advertising performance report


To view performance data for the ad units in your apps, you can use the following reports on the Windows Dev Center dashboard:

-   [App-level advertising performance report](advertising-performance-report.md#app-level-advertising-performance-report). This report provides performance data for the Microsoft ad units in the currently selected app in the dashboard.
-   [Account-level advertising performance report](advertising-performance-report.md#account-level-advertising-performance-report). This report provides detailed performance data for Microsoft ad units and community ads for all apps that are registered to your developer account.

By default, the reports are filtered on performance from the last 30 days, on all devices. To change these filters, select **Apply filters**, and then choose a different time frame (one of the preset periods or a custom date range) or choose an individual device type. 

> [!TIP]
> To do deeper analysis of your data, select **Download report**, then open the CSV (comma-separated values) file in Microsoft Excel or a similar program.

The following sections provide more details about these reports.

## App-level advertising performance report

This page provides performance data in graph, world map, and table form for the Microsoft ad units in the currently selected app in the dashboard. To view this report, select one of your apps in the dashboard and click **Analytics** &gt; **Advertising performance** in the navigation pane.

The data is obtained from the following performance metrics we track for the ads in your app:

-   **Estimated revenue**: The estimated amount of money you received from the ads running on your app.
-   **eCPM**: Effective cost per thousand impressions.
-   **Requests**: The number of times an ad request was sent from your app.
-   **Impressions**: The number of times an ad was shown in your app.
-   **Fill rate**: The percentage of ad requests sent from your app in which an ad was shown.
-   **Clicks**: The number of times someone clicked on an ad in your app.
-   **CTR**: Click-through rate, meaning the number of times an ad was clicked, divided by the number of impressions.

To review all these performance metrics for the ad units in your app, refer to the table below the graph and map views.

To analyze the data for one of these metrics in a graph or world map view, click **Graph** or **Map**. Click the headers above the graph or map to switch between the different metrics. By default, the graph and map views show performance data for all of the ad units in your app, but you can click **Choose ad units** to select up to six individual ad units to compare.

In the map view, darker shades represent higher values and lighter shades represent lower values. You can hover over a specific country or region on the map to analyze the value of the selected metric. You can also zoom in on any area of the map to view data for smaller countries.

To do deeper analysis of your data, select **Download report**, then open the CSV (comma-separated values) file in Microsoft Excel or a similar program.

## Account-level advertising performance report

This page provides performance data for Microsoft ad units and community ads used in apps that are registered to your developer account. To view this report, go to your dashboard overview page and click **Advertising performance** in the navigation pane.

This page includes the following sections.

### Microsoft Advertising

This report provides performance data for all the Microsoft ad units used in your apps. This report also includes performance data for any pubCenter ad units that were not successfully mapped to your Dev Center apps.

This report shows the same seven performance metrics and views (graph, world map, and table) as the app-level advertising performance report described above. You can apply the following filters to this report:

-   **All ad units**. When you select this filter, you can choose to display data from all ad units or from up to six specific ad units.
-   **All apps**. When you select this filter, you can choose to display data from all apps or from up to six specific apps.
-   **Individual app**. When you select an app, you can choose to display data from all ad units used by the app or from up to six specific ad units used by the app.

If you created ad units for an app using Microsoft pubCenter, it’s possible that not all of them were mapped successfully to your apps in Dev Center. In this report, these ad units are associated with app names that you specified in pubCenter, with the string **(pubCenter)** appended to the app name.

If you created ad units for an app using pubCenter, and you don’t see data for them in either report page, click **Link a pubCenter account** to link that account to your Dev Center account. Enter the email address you associated with this pubCenter account and your account linking code. You can find the account linking code by signing into this pubCenter account and going to the **My information** page.

For more information about migrating pubCenter accounts to Dev Center, see [pubCenter-DevCenter integration](pubcenter-dev-center-integration.md).

To do deeper analysis of your data, select **Download report**, then open the CSV (comma-separated values) file in Microsoft Excel or a similar program.

### Microsoft community ads

This section provides performance data in graph and world map form for community ads in the currently selected app in the dashboard. For more information about community ads, see [About community ads](about-community-ads.md).

The data is obtained from the following performance metrics we track for the ads in your app:

-   **Requests**: The number of times a community ad request was sent from your app.
-   **Fill rate**: The percentage of community ad requests sent from your app in which an ad was shown.
-   **Clicks**: The number of times someone clicked on a community ad in your app.
-   **CTR**: Click-through rate, meaning the number of times a community ad was clicked, divided by the number of impressions.
-   **Credits earned**: The number of community ad credits you have earned from this app. For more details about how credits are earned, see [About community ads](about-community-ads.md).
-   **Credits spent**: The number of community ad credits you have spent for this app. For more details about how credits are spent, see [About community ads](about-community-ads.md).

To analyze the data for one of these metrics in a graph or world map view, click **Graph** or **Map**. Click the headers above the graph or map to switch between the different metrics. In the map view, darker shades represent higher values and lighter shades represent lower values. You can hover over a specific country or region on the map to analyze the value of the selected metric. You can also zoom in on any area of the map to view data for smaller countries.

## Notes about the reports

Here are a few things to keep in mind when using the advertising performance reports.

- There may be differences between advertising performance reports in Dev Center and pubCenter. Advertising performance data in Dev Center is aggregated based on UTC (not your particular time zone), while pubCenter reports are aggregated based on your particular time zone.
- Reporting data for the last three days might change as we receive and process new data from various sources.
- Data restatements can happen up to 90 days in the past.

 

 



<!--HONumber=Aug16_HO3-->


