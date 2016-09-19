---
author: mcleanbyron
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: Use this method in the Windows Store analytics API to get review data for a given date range and other optional filters.
title: Get app reviews
translationtype: Human Translation
ms.sourcegitcommit: 6d0fa3d3b57bcc01234aac7d6856416fcf9f4419
ms.openlocfilehash: 4190012c08e22f4efb086c711183332b23ccee38

---

# Get app reviews




Use this method in the Windows Store analytics API to get review data for a given date range and other optional filters. This method returns the data in JSON format.

## Prerequisites


To use this method, you need to first do the following:

* If you have not done so already, complete all the [prerequisites](access-analytics-data-using-windows-store-services.md#prerequisites) for the Windows Store analytics API.
* [Obtain an Azure AD access token](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) to use in the request header for this method. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can obtain a new one.

## Request


### Request syntax

| Method | Request URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |

<span/> 

### Request header

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Required. The Azure AD access token in the form **Bearer** &lt;*token*&gt;. |

<span/> 

### Request parameters

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">Type</th>
<th align="left">Description</th>
<th align="left">Required</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">applicationId</td>
<td align="left">string</td>
<td align="left">The Store ID of the app for which you want to retrieve review data. The Store ID is available on the [App identity page](../publish/view-app-identity-details.md) of the Dev Center dashboard. An example Store ID is 9WZDNCRFJ3Q8.</td>
<td align="left">Yes</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">The start date in the date range of review data to retrieve. The default is the current date.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">The end date in the date range of review data to retrieve. The default is the current date.</td>
<td align="left">No</td>
</tr>
<tr class="even">
<td align="left">top</td>
<td align="left">int</td>
<td align="left">The number of rows of data to return in the request. The maximum value and the default value if not specified is 10000. If there are more rows in the query, the response body includes a next link that you can use to request the next page of data.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">skip</td>
<td align="left">int</td>
<td align="left">The number of rows to skip in the query. Use this parameter to page through large data sets. For example, top=10000 and skip=0 retrieves the first 10000 rows of data, top=10000 and skip=10000 retrieves the next 10000 rows of data, and so on.</td>
<td align="left">No</td>
</tr>
<tr class="even">
<td align="left">filter</td>
<td align="left">string</td>
<td align="left">One or more statements that filter the rows in the response. For more information, see the [filter fields](#filter-fields) section below.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">string</td>
<td align="left">A statement that orders the result data values for each rating. The syntax is <em>orderby=field [order],field [order],...</em>. The <em>field</em> parameter can be one of the following strings:
<ul>
<li><strong>date</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>isRevised</strong></li>
<li><strong>packageVersion</strong></li>
<li><strong>deviceModel</strong></li>
<li><strong>productFamily</strong></li>
<li><strong>deviceScreenResolution</strong></li>
<li><strong>isTouchEnabled</strong></li>
<li><strong>reviewerName</strong></li>
<li><strong>reviewTitle</strong></li>
<li><strong>reviewText</strong></li>
<li><strong>helpfulCount</strong></li>
<li><strong>notHelpfulCount</strong></li>
<li><strong>responseDate</strong></li>
<li><strong>responseText</strong></li>
<li><strong>deviceRAM</strong></li>
<li><strong>deviceStorageCapacity</strong></li>
<li><strong>rating</strong></li>
</ul>
<p>The <em>order</em> parameter is optional, and can be <strong>asc</strong> or <strong>desc</strong> to specify ascending or descending order for each field. The default is <strong>asc</strong>.</p>
<p>Here is an example <em>orderby</em> string: <em>orderby=date,market</em></p></td>
<td align="left">No</td>
</tr>
</tbody>
</table>

<span/>
 
### Filter fields

The *filter* parameter of the request contains one or more statements that filter the rows in the response. Each statement contains a field and value that are associated with the **eq** or **ne** operators, and some fields also support the **contains**, **gt**, **lt**, **ge**, and **le** operators. Statements can be combined using **and** or **or**.

Here is an example *filter* string: *filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

For a list of the supported fields and support operators for each field, see the following table. String values must be surrounded by single quotes in the *filter* parameter.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Fields</th>
<th align="left">Supported operators</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">market</td>
<td align="left">eq, ne</td>
<td align="left">A string that contains the ISO 3166 country code of the device market.</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">eq, ne</td>
<td align="left">One of the following strings:
<ul>
<li><strong>Windows Phone 7.5</strong></li>
<li><strong>Windows Phone 8</strong></li>
<li><strong>Windows Phone 8.1</strong></li>
<li><strong>Windows Phone 10</strong></li>
<li><strong>Windows 8</strong></li>
<li><strong>Windows 8.1</strong></li>
<li><strong>Windows 10</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">deviceType</td>
<td align="left">eq, ne</td>
<td align="left">One of the following strings:
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>IoT</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">isRevised</td>
<td align="left">eq, ne</td>
<td align="left">Specify <strong>true</strong> to filter for reviews that have been revised; otherwise <strong>false</strong>.</td>
</tr>
<tr class="odd">
<td align="left">packageVersion</td>
<td align="left">eq, ne</td>
<td align="left">The version of the app package that was reviewed.</td>
</tr>
<tr class="even">
<td align="left">deviceModel</td>
<td align="left">eq, ne</td>
<td align="left">The type of device on which the app was reviewed.</td>
</tr>
<tr class="odd">
<td align="left">productFamily</td>
<td align="left">eq, ne</td>
<td align="left">One of the following strings:
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">deviceScreenResolution</td>
<td align="left">eq, ne</td>
<td align="left">The device screen resolution in the format &quot;<em>width</em> x <em>height</em>&quot;.</td>
</tr>
<tr class="odd">
<td align="left">isTouchEnabled</td>
<td align="left">eq, ne</td>
<td align="left">Specify <strong>true</strong> to filter for touch-enabled devices; otherwise <strong>false</strong>.</td>
</tr>
<tr class="even">
<td align="left">reviewerName</td>
<td align="left">eq, ne</td>
<td align="left">The reviewer name.</td>
</tr>
<tr class="odd">
<td align="left">helpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">The number of times the review was marked helpful.</td>
</tr>
<tr class="even">
<td align="left">notHelpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">The number of times the review was marked not helpful.</td>
</tr>
<tr class="odd">
<td align="left">reviewTitle</td>
<td align="left">eq, ne, contains</td>
<td align="left">The title of the review.</td>
</tr>
<tr class="even">
<td align="left">reviewText</td>
<td align="left">eq, ne, contains</td>
<td align="left">The text contents of the review.</td>
</tr>
<tr class="odd">
<td align="left">responseText</td>
<td align="left">eq, ne, contains</td>
<td align="left">The text contents of the response.</td>
</tr>
<tr class="even">
<td align="left">responseDate</td>
<td align="left">eq, ne</td>
<td align="left">The date that the response was submitted.</td>
</tr>
<tr class="odd">
<td align="left">deviceRAM</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">The physical RAM, in MB.</td>
</tr>
<tr class="even">
<td align="left">deviceStorageCapacity</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">The capacity of the primary storage disk, in GB.</td>
</tr>
<tr class="odd">
<td align="left">rating</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">The app rating, in stars.</td>
</tr>
</tbody>
</table>

<span/> 

### Request example

The following examples demonstrate several requests for getting review data. Replace the *applicationId* value with the Store ID for your app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## Response


### Response body

| Value      | Type   | Description                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Value      | array  | An array of objects that contain review data. For more information about the data in each object, see the [review values](#review-values) section below.                                                                                                                                      |
| @nextLink  | string | If there are additional pages of data, this string contains a URI that you can use to request the next page of data. For example, this value is returned if the **top** parameter of the request is set to 10000 but there are more than 10000 rows of acquisition data for the query. |
| TotalCount | int    | The total number of rows in the data result for the query.                                                                                                                                                                                                                             |

<span/>
 
### Review values

Elements in the *Value* array contain the following values.

| Value                  | Type    | Description                                                                                                                                                                                                                          |
|------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                   | string  | The first date in the date range for the ratings data. If the request specified a single day, this value is that date. If the request specified a week, month, or other date range, this value is the first date in that date range. |
| applicationId          | string  | The Store ID of the app for which you are retrieving ratings data.                                                                                                                                                                 |
| applicationName        | string  | The display name of the app.                                                                                                                                                                                                         |
| market                 | string  | The ISO 3166 country code of the market where the rating was submitted.                                                                                                                                                              |
| osVersion              | string  | The OS version on which the rating was submitted. For a list of the supported strings, see the [filter fields](#filter-fields) section above.                                                                                               |
| deviceType             | string  | The type of device on which the rating was submitted. For a list of the supported strings, see the [filter fields](#filter-fields) section above.                                                                                           |
| isRevised              | Boolean | The value **true** indicates that the review was revised; otherwise **false**.                                                                                                                                                       |
| packageVersion         | string  | The version of the app package that was reviewed.                                                                                                                                                                                    |
| deviceModel            | string  | The type of device on which the app was reviewed.                                                                                                                                                                                    |
| productFamily          | string  | The device family name. For a list of the supported strings, see the [filter fields](#filter-fields) section above.                                                                                                                         |
| deviceScreenResolution | string  | The device screen resolution in the format "*width* x *height*".                                                                                                                                                                     |
| isTouchEnabled         | Boolean | The value **true** indicates that touch is enabled; otherwise **false**.                                                                                                                                                             |
| reviewerName           | string  | The reviewer name.                                                                                                                                                                                                                   |
| helpfulCount           | number  | The number of times the review was marked helpful.                                                                                                                                                                                   |
| notHelpfulCount        | number  | The number of times the review was marked not helpful.                                                                                                                                                                               |
| reviewTitle            | string  | The title of the review.                                                                                                                                                                                                             |
| reviewText             | string  | The text contents of the review.                                                                                                                                                                                                     |
| responseText           | string  | The text contents of the response.                                                                                                                                                                                                   |
| responseDate           | string  | The date a response was submitted.                                                                                                                                                                                                   |
| deviceRAM              | number  | The physical RAM, in MB.                                                                                                                                                                                                             |
| deviceStorageCapacity  | number  | The capacity of the primary storage disk, in GB.                                                                                                                                                                                     |
| rating                 | number  | The app rating, in stars.                                                                                                                                                                                                            |

<span/> 

### Response example

The following example demonstrates an example JSON response body for this request.

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## Related topics

* [Access analytics data using Windows Store services](access-analytics-data-using-windows-store-services.md)
* [Get app acquisitions](get-app-acquisitions.md)
* [Get add-on acquisitions](get-in-app-acquisitions.md)
* [Get error reporting data](get-error-reporting-data.md)
* [Get app ratings](get-app-ratings.md)



<!--HONumber=Aug16_HO5-->


