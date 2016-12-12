---
author: mcleanbyron
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Use this method in the Windows Store analytics API to get aggregate error reporting data for a given date range and other optional filters.
title: Get error reporting data
translationtype: Human Translation
ms.sourcegitcommit: dcf4c263ff3fd8df846d1d5620ba31a9da7a5e6c
ms.openlocfilehash: 800405bcac9b05af0e0295c88c27cbe3d2387947

---

# <a name="get-error-reporting-data"></a>Get error reporting data

Use this method in the Windows Store analytics API to get aggregate error reporting data for your app in JSON format for a given date range and other optional filters. This information is also available in the **Failures** section of the [Health report](../publish/health-report.md) in the Windows Dev Center dashboard.

You can retrieve additional error information by using the [get details for an error in your app](get-details-for-an-error-in-your-app.md) and [get the stack trace for an error in your app](get-the-stack-trace-for-an-error-in-your-app.md) methods.

## <a name="prerequisites"></a>Prerequisites


To use this method, you need to first do the following:

* If you have not done so already, complete all the [prerequisites](access-analytics-data-using-windows-store-services.md#prerequisites) for the Windows Store analytics API.
* [Obtain an Azure AD access token](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) to use in the request header for this method. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can obtain a new one.

## <a name="request"></a>Request


### <a name="request-syntax"></a>Request syntax

| Method | Request URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |

<span/> 

### <a name="request-header"></a>Request header

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Required. The Azure AD access token in the form **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Request parameters

| Parameter        | Type   |  Description      |  Required  
|---------------|--------|---------------|------|
| applicationId | string | The Store ID of the app for which you want to retrieve error reporting data. The Store ID is available on the [App identity page](../publish/view-app-identity-details.md) of the Dev Center dashboard. An example Store ID is 9WZDNCRFJ3Q8. |  Yes  |
| startDate | date | The start date in the date range of error reporting data to retrieve. The default is the current date. |  No  |
| endDate | date | The end date in the date range of error reporting data to retrieve. The default is the current date. |  No  |
| top | int | The number of rows of data to return in the request. The maximum value and the default value if not specified is 10000. If there are more rows in the query, the response body includes a next link that you can use to request the next page of data. |  No  |
| skip | int | The number of rows to skip in the query. Use this parameter to page through large data sets. For example, top=10000 and skip=0 retrieves the first 10000 rows of data, top=10000 and skip=10000 retrieves the next 10000 rows of data, and so on. |  No  |
| filter |string  | One or more statements that filter the rows in the response. For more information, see the [filter fields](#filter-fields) section below. | No   |
| aggregationLevel | string | Specifies the time range for which to retrieve aggregate data. Can be one of the following strings: <strong>day</strong>, <strong>week</strong>, or <strong>month</strong>. If unspecified, the default is <strong>day</strong>. If you specify <strong>week</strong> or <strong>month</strong>, the <em>failureName</em> and <em>failureHash</em> values are limited to 1000 buckets. | No |
| orderby | string | A statement that orders the result data values. The syntax is <em>orderby=field [order],field [order],...</em>. The <em>field</em> parameter can be one of the following strings:<ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>packageName</strong></li><li><strong>packageVersion</strong></li></ul><p>The <em>order</em> parameter is optional, and can be <strong>asc</strong> or <strong>desc</strong> to specify ascending or descending order for each field. The default is <strong>asc</strong>.</p><p>Here is an example <em>orderby</em> string: <em>orderby=date,market</em></p> |  No  |
| groupby | string | A statement that applies data aggregation only to the specified fields. You can specify the following fields:<ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>packageName</strong></li><li><strong>packageVersion</strong></li></ul><p>The returned data rows will contain the fields specified in the <em>groupby</em> parameter as well as the following:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>applicationName</strong></li><li><strong>deviceCount</strong></li><li><strong>eventCount</strong></li></ul><p>The <em>groupby</em> parameter can be used with the <em>aggregationLevel</em> parameter. For example: <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></p> |  No  |

<span/>
 
### <a name="filter-fields"></a>Filter fields

The *filter* parameter of the request contains one or more statements that filter the rows in the response. Each statement contains a field and value that are associated with the **eq** or **ne** operators, and statements can be combined using **and** or **or**. Here are some example *filter* parameters:

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Unknown') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'greater than 55' or ageGroup ne ‘less than 13’)*

For a list of the supported fields, see the following table. String values must be surrounded by single quotes in the *filter* parameter.

| Fields        |  Description        |
|---------------|-----------------|
| failureName | The name of the error. |
| failureHash | The unique identifier for the error. |
| symbol | The symbol assigned to this error. |
| osVersion | One of the following strings:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| eventType | One of the following strings:<ul><li><strong>crash</strong></li><li><strong>hang</strong></li><li><strong>memory</strong></li><li><strong>jse</strong></li></ul> |
| market | A string that contains the ISO 3166 country code of the market where the error occurred. |
| deviceType | One of the following strings:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| packageName | The unique name of the app package that is associated with this error. |
| packageVersion | The version of the app package that is associated with this error. |

<span/> 

### <a name="request-example"></a>Request example

The following examples demonstrate several requests for getting error reporting data. Replace the *applicationId* value with the Store ID for your app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Response body

| Value      | Type    | Description     |
|------------|---------|--------------|
| Value      | array   | An array of objects that contain aggregate error reporting data. For more information about the data in each object, see the [error values](#error-values) section below.     |
| @nextLink  | string  | If there are additional pages of data, this string contains a URI that you can use to request the next page of data. For example, this value is returned if the **top** parameter of the request is set to 10000 but there are more than 10000 rows of errors for the query. |
| TotalCount | inumber | The total number of rows in the data result for the query.     |

<span/>

### <a name="error-values"></a>Error values

Elements in the *Value* array contain the following values.

| Value           | Type    | Description        |
|-----------------|---------|---------------------|
| date            | string  | The first date in the date range for the error data. If the request specified a single day, this value is that date. If the request specified a week, month, or other date range, this value is the first date in that date range. |
| applicationId   | string  | The Store ID of the app for which you want to retrieve error data.   |
| applicationName | string  | The display name of the app.   |
| failureName     | string  | The name of the error.  |
| failureHash     | string  | The unique identifier for the error.   |
| symbol          | string  | The symbol assigned to this error. |
| osVersion       | string  | The OS version on which the error occurred. For a list of the supported strings, see the [filter fields](#filter-fields) section above.  |
| eventType       | string  | The type of error event. For a list of the supported strings, see the [filter fields](#filter-fields) section above.      |
| market          | string  | The ISO 3166 country code of the device market.   |
| deviceType      | string  | The type of device on which the error occurred. For a list of the supported strings, see the [filter fields](#filter-fields) section above.    |
| packageName     | string  | The unique name of the app package that is associated with this error.      |
| packageVersion  | string  | The version of the app package that is associated with this error.   |
| eventCount      | inumber | The number of events that are attributed to this error for the specified aggregation level.      |
| deviceCount     | inumber | The number of unique devices that correspond to this error for the specified aggregation level.  |

<span/> 

### <a name="response-example"></a>Response example

The following example demonstrates an example JSON response body for this request.

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows Phone 8",
      "eventType": "crash",
      "market": "US",
      "deviceType": "mobile",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 191753
}

```

## <a name="related-topics"></a>Related topics

* [Health report](../publish/health-report.md)
* [Get details for an error in your app](get-details-for-an-error-in-your-app.md)
* [Get the stack trace for an error in your app](get-the-stack-trace-for-an-error-in-your-app.md)
* [Access analytics data using Windows Store services](access-analytics-data-using-windows-store-services.md)
* [Get app acquisitions](get-app-acquisitions.md)
* [Get add-on acquisitions](get-in-app-acquisitions.md)
* [Get app ratings](get-app-ratings.md)
* [Get app reviews](get-app-reviews.md)



<!--HONumber=Dec16_HO1-->


