---
author: mcleanbyron
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Use this method in the Windows Store submission API to create an add-on for an app that is registered to your Windows Dev Center account.
title: Create an add-on using the Windows Store submission API
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 11cf25fbeacbe3145c9cc3f4a80bdcce3028bf55

---

# Create an add-on using the Windows Store submission API




Use this method in the Windows Store submission API to create an add-on (also known as in-app product or IAP) for an app that is registered to your Windows Dev Center account.

>**Note**&nbsp;&nbsp;This method creates an add-on without any submissions. To create a submission for an add-on, see the methods in [Manage add-on submissions](manage-add-on-submissions.md).

## Prerequisites

To use this method, you need to first do the following:

* If you have not done so already, complete all the [prerequisites](create-and-manage-submissions-using-windows-store-services.md#prerequisites) for the Windows Store submission API.
* [Obtain an Azure AD access token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) to use in the request header for this method. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can obtain a new one.

>**Note**&nbsp;&nbsp;This method can only be used for Windows Dev Center accounts that have been given permission to use the Windows Store submission API. Not all accounts have this permission enabled.

## Request

This method has the following syntax. See the following sections for usage examples and descriptions of the header and request body.

| Method | Request URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |

<span/>
 

### Request header

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Required. The Azure AD access token in the form **Bearer** &lt;*token*&gt;. |

<span/>

### Request body

The request body has the following parameters.
 
|  Parameter  |  Type  |  Description  |  Required  |
|------|------|------|------|
|  applicationIds  |  array  |  An array that contains the Store ID of the app that this add-on is associated with. Only one item is supported in this array.   |  Yes  |
|  productId  |  string  |  The product ID of the add-on. This is an identifier that can use in code to refer to the add-on. For more information, see [Set your product type and product ID](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Yes  |
|  productType  |  string  |  The product type of the add-on. The following values are supported: **Durable** and **Consumable**.  |  Yes  |

<span/>

### Request example

The following example demonstrates how to create a new consumable add-on for an app.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## Response

The following example demonstrates the JSON response body for a successful call to this method. For more details about the values in the response body, see [add-on resource](manage-add-ons.md#add-on-object).

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## Error codes

If the request cannot be successfully completed, the response will contain one of the following HTTP error codes.

| Error code |  Description                                                                                                                                                                           |
|--------|------------------|
| 400  | The request is invalid. |
| 409  | The add-on could not be created because of its current state, or the add-on uses a Dev Center dashboard feature that is [currently not supported by the Windows Store submission API](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## Related topics

* [Create and manage submissions using Windows Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Manage add-on submissions](manage-add-on-submissions.md)
* [Get all add-ons](get-all-add-ons.md)
* [Get an add-on](get-an-add-on.md)
* [Delete an add-on](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->


