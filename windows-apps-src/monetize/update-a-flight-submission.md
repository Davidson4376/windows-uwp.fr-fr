---
author: mcleanbyron
ms.assetid: 24C5F796-5FB8-4B5D-B428-C3154B3098BD
description: Use this method in the Windows Store submission API to update an existing package flight submission.
title: Update a package flight submission using the Windows Store submission API
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 9dfad9c0cc6b6e03a2196946ac578ca3170fccc5

---

# Update a package flight submission using the Windows Store submission API


Use this method in the Windows Store submission API to update an existing package flight submission. After you successfully update a submission by using this method, you must [commit the submission](commit-a-flight-submission.md) for ingestion and publishing.

For more information about how this method fits into the process of creating a package flight submission by using the Windows Store submission API, see [Manage package flight submissions](manage-flight-submissions.md).

## Prerequisites

To use this method, you need to first do the following:

* If you have not done so already, complete all the [prerequisites](create-and-manage-submissions-using-windows-store-services.md#prerequisites) for the Windows Store submission API.
* [Obtain an Azure AD access token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) to use in the request header for this method. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can obtain a new one.
* Create a package flight submission for an app in your Dev Center account. You can do this in the Dev Center dashboard, or you can do this by using the [create a package flight submission](create-a-flight-submission.md) method.

>**Note**&nbsp;&nbsp;This method can only be used for Windows Dev Center accounts that have been given permission to use the Windows Store submission API. Not all accounts have this permission enabled.

## Request

This method has the following syntax. See the following sections for usage examples and descriptions of the header and request body.

| Method | Request URI                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions{submissionId}``` |

<span/>
 

### Request header

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Required. The Azure AD access token in the form **Bearer** &lt;*token*&gt;. |

<span/>

### Request parameters

| Name        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Required. The Store ID of the app for which you want to update a package flight submission. For more information about the Store ID, see [View app identity details](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Required. The ID of the package flight for which you want to update a submission. This ID is available in the Dev Center dashboard, and it is included in the response data for requests to [create a package flight](create-a-flight.md) and [get package flights for an app](get-flights-for-an-app.md).  |
| submissionId | string | Required. The ID of the submission to update. This ID is available in the Dev Center dashboard, and it is included in the response data for requests to [create a package flight submission](create-a-flight-submission.md).  |

<span/>

### Request body

The request body has the following parameters.

| Value      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightPackages           | array  | Contains objects that provide details about each package in the submission. For more details about the values in the response body, see [Flight package resource](manage-flight-submissions.md#flight-package-object). When calling this method to update an app submission, only the *fileName*, *fileStatus*, *minimumDirectXVersion*, and *minimumSystemRam* values of these objects are required in the request body. The other values are populated by Dev Center. |
| targetPublishMode           | string  | The publish mode for the submission. This can be one of the following values: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | The publish date for the submission in in ISO 8601 format, if the *targetPublishMode* is set to SpecificDate.  |
| notesForCertification           | string  |  Provides additional info for the certification testers, such as test account credentials and steps to access and verify features. For more information, see [Notes for certification](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span/>

### Request example

The following example demonstrates how to update a package flight submission for an app.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## Response

The following example demonstrates the JSON response body for a successful call to this method. The response body contains information about the updated submission. For more details about the values in the response body, see [Package flight submission resource](manage-flight-submissions.md#flight-submission-object).

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## Error codes

If the request cannot be successfully completed, the response will contain one of the following HTTP error codes.

| Error code |  Description   |
|--------|------------------|
| 400  | The package flight submission could not be updated because the request is invalid. |
| 409  | The package flight submission could not be updated because of the current state of the app, or the app uses a Dev Center dashboard feature that is [currently not supported by the Windows Store submission API](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## Related topics

* [Create and manage submissions using Windows Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Manage package flight submissions](manage-flight-submissions.md)
* [Get a package flight submission](get-a-flight-submission.md)
* [Create a package flight submission](create-a-flight-submission.md)
* [Commit a package flight submission](commit-a-flight-submission.md)
* [Delete a package flight submission](delete-a-flight-submission.md)
* [Get the status of a package flight submission](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


