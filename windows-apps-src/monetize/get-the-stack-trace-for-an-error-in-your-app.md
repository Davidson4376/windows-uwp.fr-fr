---
author: mcleanbyron
ms.assetid: 
description: Use this method in the Windows Store analytics API to get the stack trace for an error in your app.
title: Get the stack trace for an error in your app
translationtype: Human Translation
ms.sourcegitcommit: 767097f068630e5ec171415c05d6dc395c8b26b3
ms.openlocfilehash: 90481b5f85d010a142e86ca67ac94c3ec25d89c6

---

# <a name="get-the-stack-trace-for-an-error-in-your-app"></a>Get the stack trace for an error in your app

Use this method in the Windows Store analytics API to get the stack trace for an error in your app. This method can only download the stack trace for an app error that occurred in the last 30 days. Stack traces are also available in the **Failures** section of the [Health report](../publish/health-report.md) in the Windows Dev Center dashboard.

Before you can use this method, you must first use the [get details for an error in your app](get-details-for-an-error-in-your-app.md) method to retrieve the ID of the CAB file that is associated with the error for which you want to retrieve the stack trace.

## <a name="prerequisites"></a>Prerequisites


To use this method, you need to first do the following:

* If you have not done so already, complete all the [prerequisites](access-analytics-data-using-windows-store-services.md#prerequisites) for the Windows Store analytics API.
* [Obtain an Azure AD access token](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) to use in the request header for this method. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can obtain a new one.
* Get the ID of the CAB file that is associated with the error for which you want to retrieve the stack trace. To get this ID, use the [get details for an error in your app](get-details-for-an-error-in-your-app.md) method to retrieve details for a specific error in your app, and use the **cabId** value in the response body of that method.

## <a name="request"></a>Request


### <a name="request-syntax"></a>Request syntax

| Method | Request URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace``` |

<span/> 

### <a name="request-header"></a>Request header

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Required. The Azure AD access token in the form **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Request parameters

| Parameter        | Type   |  Description      |  Required  |
|---------------|--------|---------------|------|
| applicationId | string | The Store ID of the app for which you want to get the stack trace. The Store ID is available on the [App identity page](../publish/view-app-identity-details.md) of the Dev Center dashboard. An example Store ID is 9WZDNCRFJ3Q8. |  Yes  |
| cabId | string | The unique ID of the CAB file that is associated with the error for which you want to retrieve the stack trace. To get this ID, use the [get details for an error in your app](get-details-for-an-error-in-your-app.md) method to retrieve details for a specific error in your app, and use the **cabId** value in the response body of that method. |  Yes  |

<span/>
 
### <a name="request-example"></a>Request example

The following example demonstrates how to get a stack trace using this method. Replace the *applicationId* value with the Store ID for your app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Response body

| Value      | Type    | Description                  |
|------------|---------|--------------------------------|
| Value      | array   | An array of objects that each contain one frame of stack trace data. For more information about the data in each object, see the [stack trace values](#stack-trace-values) section below. |
| @nextLink  | string  | If there are additional pages of data, this string contains a URI that you can use to request the next page of data. For example, this value is returned if the **top** parameter of the request is set to 10 but there are more than 10 rows of errors for the query. |
| TotalCount | inumber | The total number of rows in the data result for the query.          |

<span/>

### <a name="stack-trace-values"></a>Stack trace values

Elements in the *Value* array contain the following values.

| Value           | Type    | Description      |
|-----------------|---------|----------------|
| level            | string  |  The frame number that this element represents in the call stack.  |
| image   | string  |   The name of the executable or library image that contains the function that is called in this stack frame.           |
| function | string  |  The name of the function that is called in this stack frame. This is available only if your app includes symbols for the executable or library.              |
| offset     | string  |  The byte offset of the current instruction relative to the start of the function.      |

<span/> 

### <a name="response-example"></a>Response example

The following example demonstrates an example JSON response body for this request.

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>Related topics

* [Health report](../publish/health-report.md)
* [Access analytics data using Windows Store services](access-analytics-data-using-windows-store-services.md)
* [Get error reporting data](get-error-reporting-data.md)
* [Get details for an error in your app](get-details-for-an-error-in-your-app.md)



<!--HONumber=Dec16_HO1-->


