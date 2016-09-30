---
author: mcleanbyron
ms.assetid: DAF92881-6AF6-44C7-B466-215F5226AE04
description: Use this method in the Windows Store submission API to retrieve information about a specific app that is registered to your Windows Dev Center account.
title: Get an app using the Windows Store submission API
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: ef0c9ff463b89854c9aaa7ee2c8307f4af30fadf

---

# Get an app using the Windows Store submission API




Use this method in the Windows Store submission API to retrieve information about a specific app that is registered to your Windows Dev Center account.

## Prerequisites

To use this method, you need to first do the following:

* If you have not done so already, complete all the [prerequisites](create-and-manage-submissions-using-windows-store-services.md#prerequisites) for the Windows Store submission API.
* [Obtain an Azure AD access token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) to use in the request header for this method. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can obtain a new one.

>**Note**&nbsp;&nbsp;This method can only be used for Windows Dev Center accounts that have been given permission to use the Windows Store submission API. Not all accounts have this permission enabled.

## Request

This method has the following syntax. See the following sections for usage examples and descriptions of the header and request body.

| Method | Request URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}``` |

<span/>
 

### Request header

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Required. The Azure AD access token in the form **Bearer** &lt;*token*&gt;. |

<span/>

### Request parameters

| Name        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Required. The Store ID of the app to retrieve. For more information about the Store ID, see [View app identity details](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |

<span/>

### Request body

Do not provide a request body for this method.

<span/>

### Request example

The following example demonstrates how to retrieve information about an app with the Store ID value 9WZDNCRD91MD.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315 HTTP/1.1
Authorization: Bearer <your access token>
```

## Response

The following example demonstrates the JSON response body for a successful call to this method. For more details about the values in the response body, see [Application resource](get-app-data.md#application_object).

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  }
}
```

## Error codes

If the request cannot be successfully completed, the response will contain one of the following HTTP error codes.

| Error code |  Description   |
|--------|------------------|
| 404  | The specified app could not be found. |
| 409  | The app uses a Dev Center dashboard feature that is [currently not supported by the Windows Store submission API](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>

## Related topics

* [Create and manage submissions using Windows Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Get all apps](get-all-apps.md)
* [Get package flights for an app](get-flights-for-an-app.md)
* [Get add-ons for an app](get-add-ons-for-an-app.md)



<!--HONumber=Aug16_HO5-->


