---
author: mcleanbyron
ms.assetid: 16D4C3B9-FC9B-46ED-9F87-1517E1B549FA
description: Use this method in the Windows Store submission API to delete an add-on for an app that is registered to your Windows Dev Center account.
title: Delete an add-on using the Windows Store submission API
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 8c28205ca004bd63b3f6a3fea2971cf927f7e2e4

---

# Delete an add-on using the Windows Store submission API




Use this method in the Windows Store submission API to delete an add-on (also known as in-app product or IAP) for an app that is registered to your Windows Dev Center account.

## Prerequisites

To use this method, you need to first do the following:

* If you have not done so already, complete all the [prerequisites](create-and-manage-submissions-using-windows-store-services.md#prerequisites) for the Windows Store submission API.
* [Obtain an Azure AD access token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) to use in the request header for this method. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can obtain a new one.

>**Note**&nbsp;&nbsp;This method can only be used for Windows Dev Center accounts that have been given permission to use the Windows Store submission API. Not all accounts have this permission enabled.

## Request

This method has the following syntax. See the following sections for usage examples and descriptions of the header and request body.

| Method | Request URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |

<span/>
 

### Request header

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Required. The Azure AD access token in the form **Bearer** &lt;*token*&gt;. |

<span/>

### Request parameters

| Name        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | string | Required. The Store ID of the add-on to delete. The Store ID is available on the Dev Center dashboard.  |

<span/>

### Request body

Do not provide a request body for this method.

<span/>

### Request example

The following example demonstrates how to delete an add-on.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## Response

If successful, this method returns an empty response body.

## Error codes

If the request cannot be successfully completed, the response will contain one of the following HTTP error codes.

| Error code |  Description                                                                                                                                                                           |
|--------|------------------|
| 400  | The request is invalid. |
| 404  | The specified add-on could not be found.  |
| 409  | The specified add-on was found but it could not be deleted in its current state, or the add-on uses a Dev Center dashboard feature that is [currently not supported by the Windows Store submission API](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## Related topics

* [Create and manage submissions using Windows Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Get all add-ons](get-all-add-ons.md)
* [Get an add-on](get-an-add-on.md)
* [Create an add-on](create-an-add-on.md)



<!--HONumber=Aug16_HO5-->


