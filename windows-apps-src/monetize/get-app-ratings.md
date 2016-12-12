---
author: mcleanbyron
ms.assetid: DD4F6BC4-67CD-4AEF-9444-F184353B0072
description: Use this method in the Windows Store analytics API to get aggregate ratings data for a given date range and other optional filters.
title: Get app ratings
translationtype: Human Translation
ms.sourcegitcommit: 7d05c8953f1f50be0b388a044fe996f345d45006
ms.openlocfilehash: 86685984256459e0bb125340daa1616b09982429

---

# <a name="get-app-ratings"></a>Get app ratings

Use this method in the Windows Store analytics API to get aggregate ratings data in JSON format for a given date range and other optional filters. This information is also available in the [Ratings report](../publish/ratings-report.md) in the Windows Dev Center dashboard.

## <a name="prerequisites"></a>Prerequisites


To use this method, you need to first do the following:

* If you have not done so already, complete all the [prerequisites](access-analytics-data-using-windows-store-services.md#prerequisites) for the Windows Store analytics API.
* [Obtain an Azure AD access token](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) to use in the request header for this method. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can obtain a new one.


## <a name="request"></a>Request


### <a name="request-syntax"></a>Request syntax

| Method | Request URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings``` |

 

### <a name="request-header"></a>Request header

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Required. The Azure AD access token in the form **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Request parameters

| Parameter        | Type   |  Description      |  Required  
|---------------|--------|---------------|------|
| applicationId | string | The Store ID of the app for which you want to retrieve ratings data. The Store ID is available on the [App identity page](../publish/view-app-identity-details.md) of the Dev Center dashboard. An example Store ID is 9WZDNCRFJ3Q8. |  Yes  |
| startDate | date | The start date in the date range of ratings data to retrieve. The default is the current date. |  No  |
| endDate | date | The end date in the date range of ratings data to retrieve. The default is the current date. |  No  |
| top | int | The number of rows of data to return in the request. The maximum value and the default value if not specified is 10000. If there are more rows in the query, the response body includes a next link that you can use to request the next page of data. |  No  |
| skip | int | The number of rows to skip in the query. Use this parameter to page through large data sets. For example, top=10000 and skip=0 retrieves the first 10000 rows of data, top=10000 and skip=10000 retrieves the next 10000 rows of data, and so on. |  No  |
| filter | string  | One or more statements that filter the rows in the response. For more information, see the [filter fields](#filter-fields) section below. | No   |
| aggregationLevel | string | Specifies the time range for which to retrieve aggregate data. Can be one of the following strings: <strong>day</strong>, <strong>week</strong>, or <strong>month</strong>. If unspecified, the default is <strong>day</strong>. | No |
| orderby | string | A statement that orders the result data values for each rating. The syntax is <em>orderby=field [order],field [order],...</em>. The <em>field</em> parameter can be one of the following strings:<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li></ul><p>The <em>order</em> parameter is optional, and can be <strong>asc</strong> or <strong>desc</strong> to specify ascending or descending order for each field. The default is <strong>asc</strong>.</p><p>Here is an example <em>orderby</em> string: <em>orderby=date,market</em></p> |  No  |
| groupby | string | A statement that applies data aggregation only to the specified fields. You can specify the following fields:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li></ul><p>The returned data rows will contain the fields specified in the <em>groupby</em> parameter as well as the following:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>fiveStars</strong></li><li><strong>fourStars</strong></li><li><strong>threeStars</strong></li><li><strong>twoStars</strong></li><li><strong>oneStar</strong></li></ul><p>The <em>groupby</em> parameter can be used with the <em>aggregationLevel</em> parameter. For example: <em>&amp;groupby=osVersion,market&amp;aggregationLevel=week</em></p> |  No  |

<span/>
 
### <a name="filter-fields"></a>Filter fields

The *filter* parameter of the request contains one or more statements that filter the rows in the response. Each statement contains a field and value that are associated with the **eq** or **ne** operators, and statements can be combined using **and** or **or**.

Here is an example *filter* string: *filter=market eq 'US' and deviceType eq 'phone' and isRevised eq true*

For a list of the supported fields, see the following table. String values must be surrounded by single quotes in the *filter* parameter.

| Fields        |  Description        |
|---------------|-----------------|
| market | A string that contains the ISO 3166 country code of the market where your app was rated. |
| osVersion | One of the following strings:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | One of the following strings:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| isRevised | Specify <strong>true</strong> to filter for ratings that have been revised; otherwise <strong>false</strong>. |

<span/> 

### <a name="request-example"></a>Request example

The following examples demonstrate several requests for getting ratings data. Replace the *applicationId* value with the Store ID for your app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Response body

| Value      | Type   | Description                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Value      | array  | An array of objects that contain aggregate ratings data. For more information about the data in each object, see the [rating values](#rating-values) section below.                                                                                                                           |
| @nextLink  | string | If there are additional pages of data, this string contains a URI that you can use to request the next page of data. For example, this value is returned if the **top** parameter of the request is set to 10000 but there are more than 10000 rows of ratings data for the query. |
| TotalCount | int    | The total number of rows in the data result for the query.                                                                                                                                                                                                                             |

<span/>

### <a name="rating-values"></a>Rating values

Elements in the *Value* array contain the following values.

| Value           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date            | string  | The first date in the date range for the ratings data. If the request specified a single day, this value is that date. If the request specified a week, month, or other date range, this value is the first date in that date range. |
| applicationId   | string  | The Store ID of the app for which you are retrieving ratings data.                                                                                                                                                                 |
| applicationName | string  | The display name of the app.                                                                                                                                                                                                         |
| market          | string  | The ISO 3166 country code of the market where the rating was submitted.                                                                                                                                                              |
| osVersion       | string  | The OS version on which the rating was submitted. For a list of the supported strings, see the [filter fields](#filter-fields) section above.                                                                                               |
| deviceType      | string  | The type of device on which the rating was submitted. For a list of the supported strings, see the [filter fields](#filter-fields) section above.                                                                                           |
| isRevised       | Boolean | The value **true** indicates that the rating was revised; otherwise **false**.                                                                                                                                                       |
| oneStar         | number  | The number of one-star ratings.                                                                                                                                                                                                      |
| twoStars        | number  | The number of two-star ratings.                                                                                                                                                                                                      |
| threeStars      | number  | The number of three-star ratings.                                                                                                                                                                                                    |
| fourStars       | number  | The number of four-star ratings.                                                                                                                                                                                                     |
| fiveStars       | number  | The number of five-star ratings.                                                                                                                                                                                                     |
 
<span/>

### <a name="response-example"></a>Response example

The following example demonstrates an example JSON response body for this request.

```json
{
  "Value": [
    {
      "date": "2015-02-13",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "CN",
      "osVersion": "8.0.10517.0",
      "deviceType": "Phone",
      "isRevised": false,
      "oneStar": 0,
      "twoStars": 0,
      "threeStars": 0,
      "fourStars": 0,
      "fiveStars": 2
    }
  ],
  "@nextLink": "ratings?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 15242
}

```

## <a name="related-topics"></a>Related topics

* [Ratings report](../publish/ratings-report.md)
* [Access analytics data using Windows Store services](access-analytics-data-using-windows-store-services.md)
* [Get app acquisitions](get-app-acquisitions.md)
* [Get add-on acquisitions](get-in-app-acquisitions.md)
* [Get error reporting data](get-error-reporting-data.md)
* [Get app reviews](get-app-reviews.md)



<!--HONumber=Dec16_HO1-->


