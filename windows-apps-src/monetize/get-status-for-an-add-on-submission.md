---
author: mcleanbyron
ms.assetid: 55315F38-6EC5-4889-A14E-7D8EC282FE98
description: Use this method in the Windows Store submission API to get the status of an add-on submission.
title: Get the status of an add-on submission using the Windows Store submission API
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: e5be4ee466c204255a074adae4d150860286ba0a

---

# Get the status of an add-on submission using the Windows Store submission API




Use this method in the Windows Store submission API to get the status of an add-on (also known as in-app product or IAP) submission. For more information about the process of process of creating an add-on submission by using the Windows Store submission API, see [Manage add-on submissions](manage-add-on-submissions.md).

## Prerequisites

To use this method, you need to first do the following:

* If you have not done so already, complete all the [prerequisites](create-and-manage-submissions-using-windows-store-services.md#prerequisites) for the Windows Store submission API.
* [Obtain an Azure AD access token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) to use in the request header for this method. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can obtain a new one.
* Create an add-on submission for an app in your Dev Center account. You can do this in the Dev Center dashboard, or you can do this by using the [Create an add-on submission](create-an-add-on-submission.md) method.

>**Note**&nbsp;&nbsp;This method can only be used for Windows Dev Center accounts that have been given permission to use the Windows Store submission API. Not all accounts have this permission enabled.

## Request

This method has the following syntax. See the following sections for usage examples and descriptions of the header and request body.

| Method | Request URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/status``` |

<span/>
 

### Request header

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Required. The Azure AD access token in the form **Bearer** &lt;*token*&gt;. |

<span/>

### Request parameters

| Name        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Required. The Store ID of the add-on that contains the submission for which you want to get the status. The Store ID is available on the Dev Center dashboard.  |
| submissionId | string | Required. The ID of the submission for which you want to get the status. This ID is available in the Dev Center dashboard, and it is included in the response data for requests to [Create an add-on submission](create-an-add-on-submission.md).  |

<span/>

### Request body

Do not provide a request body for this method.

### Request example

The following example demonstrates how to get the status of an add-on submission.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621243680/status HTTP/1.1
Authorization: Bearer <your access token>
```

## Response

The following example demonstrates the JSON response body for a successful call to this method. The response body contains information about the specified submission. For more details about the values in the response body, see the following sections.

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### Response body

| Value      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | The status of the submission. This can be one of the following values: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Contains additional details about the status of the submission, including information about any errors. For more information, see [Status details resource](manage-add-on-submissions.md#status-details-object). |

<span/>

## Error codes

If the request cannot be successfully completed, the response will contain one of the following HTTP error codes.

| Error code |  Description   |
|--------|------------------|
| 404  | The submission could not be found. |
| 409  | The add-on uses a Dev Center dashboard feature that is [currently not supported by the Windows Store submission API](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>


## Related topics

* [Create and manage submissions using Windows Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Get an add-on submission](get-an-add-on-submission.md)
* [Create an add-on submission](create-an-add-on-submission.md)
* [Commit an add-on submission](commit-an-add-on-submission.md)
* [Update an add-on submission](update-an-add-on-submission.md)
* [Delete an add-on submission](delete-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->


