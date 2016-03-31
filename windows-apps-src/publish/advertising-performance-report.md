---
Description: To view performance data for the ad units in your apps, use the app-level and account-level advertising performance reports on the Windows Dev Center dashboard.
title: Advertising performance report
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
---

# Advertising performance report


To view performance data for the ad units in your apps, you can use the following reports on the Windows Dev Center dashboard:

-   [App-level advertising performance report](advertising-performance-report.md#app-level-advertising-performance-report). This report provides performance data for the ad units in the currently selected app in the dashboard.
-   [Account-level advertising performance report](advertising-performance-report.md#advertising-performance-summary). The **Advertising performance** section on your **Dashboard overview** page provides a summary of the account-level advertising performance report.

By default, the reports are filtered on performance from the last 30 days, on all devices. To change these filters, click **Page filters** and choose a different time frame or an individual device type. Note that all data is aggregated based using UTC, not your particular time zone.

The following sections provide more details about these reports.

## App-level advertising performance report


This report provides performance data in graph, world map, and table form for the ad units in the currently selected app in the dashboard. To view this report, select one of your apps in the dashboard and click **Analytics** &gt; **Advertising performance** in the navigation pane.

The data is obtained from the following seven performance metrics we track for the ads in your app:

-   **Estimated revenue**: The estimated amount of money you received from the ads running on your app.
-   **eCPM**: Effective cost per thousand impressions.
-   **Requests**: The number of times an ad request was sent to the Microsoft Ad Network from your app.
-   **Impressions**: The number of times an ad was shown in your app.
-   **Fill rate**: The percentage of ad requests sent from your app in which an ad was shown.
-   **Clicks**: The number of times someone clicked on an ad in your app.
-   **CTR**: Click-through rate, meaning the number of times an ad was clicked, divided by the number of impressions.

To review all these performance metrics for the ad units in your app, refer to the table below the graph and map views.

To analyze the data for one of these metrics in a graph or world map view, click **Graph** or **Map**. Click the headers above the graph or map to switch between the different metrics. By default, the graph and map views show performance data for all of the ad units in your app, but you can click **Choose ad units** to select up to six individual ad units to compare.

In the map view, darker shades represent higher values and lighter shades represent lower values. You can hover over a specific country or region on the map to analyze the value of the selected metric. You can also zoom in on any area of the map to view data for smaller countries.

## Account-level advertising performance report


This report provides performance data in graph, world map, and table form for all the ad units used in apps that are registered to your developer account. This report also includes performance data for any pubCenter ad units that were not successfully mapped to your Dev Center apps. To view this report, go to your dashboard overview page and click **Advertising performance** in the navigation pane.

This report shows the same seven performance metrics and views (graph, world map, and table) as the app-level advertising performance report described above. You can apply the following filters to this report:

-   **All ad units**. When you select this filter, you can choose to display data from all ad units or from up to six specific ad units.
-   **All apps**. When you select this filter, you can choose to display data from all apps or from up to six specific apps.
-   **Individual app**. When you select an app, you can choose to display data from all ad units used by the app or from up to six specific ad units used by the app.

### Migrating data from pubCenter

If you created ad units for an app using Microsoft pubCenter, it’s possible that not all of them were mapped successfully to your apps in Dev Center. In this report, these ad units are associated with app names that you specified in pubCenter, with the string **(pubCenter)** appended to the app name.

If you created ad units for an app using pubCenter, and you don’t see data for them in either report page, click **Link a pubCenter account** to link that account to your Dev Center account. Enter the email address you associated with this pubCenter account and your account linking code. You can find the account linking code by signing into this pubCenter account and going to the **My information** page.

For more information about migrating pubCenter accounts to Dev Center, see [pubCenter-DevCenter integration](pubcenter-dev-center-integration.md).

## Advertising performance summary


The **Advertising performance** section on your **Dashboard overview** page provides data that is similar to the account-level advertising performance report, with the following differences:

-   It provides the data only in graph form. For a table version of the data, use the account-level advertising performance report.
-   The only filters provided are for all apps or for individual apps. To filter by ad units, use the account-level advertising performance report.

 

 




<!--HONumber=Mar16_HO1-->
