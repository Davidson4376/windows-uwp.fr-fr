---
author: mcleanbyron
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Use the Windows Store submission API to programmatically create and manage submissions for apps that are registered to your Windows Dev Center account.
title: Create and manage submissions using Windows Store services
translationtype: Human Translation
ms.sourcegitcommit: 47e0ac11178af98589e75cc562631c6904b40da4
ms.openlocfilehash: 0a566dfee8f7fe08c06ce4963435a70c30b1650d

---

# Create and manage submissions using Windows Store services


Use the *Windows Store submission API* to programmatically query and create submissions for apps, add-ons (also known as in-app products or IAPs) and package flights for your or your organization's Windows Dev Center account. This API is useful if your account manages many apps or add-ons, and you want to automate and optimize the submission process for these assets. This API uses Azure Active Directory (Azure AD) to authenticate the calls from your app or service.

The following steps describe the end-to-end process of using the Windows Store submission API:

1.  Make sure that you have completed all the [prerequisites](#prerequisites).
3.  Before you call a method in the Windows Store submission API, [obtain an Azure AD access token](#obtain-an-azure-ad-access-token). After you obtain a token, you have 60 minutes to use this token in calls to the Windows Store submission API before the token expires. After the token expires, you can generate a new token.
4.  [Call the Windows Store submission API](#call-the-windows-store-submission-api).



<span id="not_supported" />
>**Important**

> * This API can only be used for Windows Dev Center accounts that have been given permission to use the API. This permission is being enabled to developer accounts in stages, and not all accounts have this permission enabled at this time. To request earlier access, log on to the Dev Center dashboard, click **Feedback** at the bottom of the dashboard, select **Submission API** for the feedback area, and submit your request. You'll receive an email when this permission is enabled for your account.

> * This API cannot be used with apps or add-ons that use certain features that were introduced to the Dev Center dashboard in August 2016, including (but not limited to) mandatory app updates and Store-managed consumable add-ons. If you use the Windows Store submission API with an app or add-on that uses one of these features, the API will return a 409 error code. In this case, you must use the dashboard to manage the submissions for the app or add-on.

<span id="prerequisites" />
## Step 1: Complete prerequisites for using the Windows Store submission API

Before you start writing code to call the Windows Store submission API, make sure that you have completed the following prerequisites.

* You (or your organization) must have an Azure AD directory and you must have [Global administrator](http://go.microsoft.com/fwlink/?LinkId=746654) permission for the directory. If you already use Office 365 or other business services from Microsoft, you already have Azure AD directory. Otherwise, you can [create a new Azure AD in Dev Center](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) for no additional charge.

* You must [associate an Azure AD application with your Windows Dev Center account](#associate-an-azure-ad-application-with-your-windows-dev-center-account) and obtain your tenant ID, client ID and key. You need these values to obtain an Azure AD access token, which you will use in calls to the Windows Store submission API.

* Prepare your app for use with the Windows Store submission API:

  * If you your app does not yet exist in Dev Center, [create your app in the Dev Center dashboard](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). You cannot use the Windows Store submission API to create an app in Dev Center; you must create your app using the dashboard, and then you can use the API to access the app and programmatically create submissions for it. However, you can use the API to programmatically create add-ons and package flights before you create submissions for them.

  * Before you can create a submission for a given app using this API, you must first [create one submission for the app in the Dev Center dashboard](https://msdn.microsoft.com/windows/uwp/publish/app-submissions), including answering the [age ratings](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) questions. After you do this, you will be able to programmatically create new submissions for this app using the API. You do not need to create an add-on submission or package flight submission before using the API for those types of submissions.

  * If you are creating or updating an app submission and you need to include an app package, [prepare the app package](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements).

  * If you are creating or updating an app submission and you need to include screenshots or images for the Store listing, [prepare the app screenshots and images](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * If you are creating or updating an add-on submission and you need to include an icon, [prepare the icon](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon).

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />
### How to associate an Azure AD application with your Windows Dev Center account

Before you can use the Windows Store submission API, you must associate an Azure AD application with your Dev Center account, retrieve the tenant ID and client ID for the application and generate a key. The Azure AD application represents the app or service from which you want to call the Windows Store submission API. You need the tenant ID, client ID and key to obtain an Azure AD access token that you pass to the API.

>**Note**&nbsp;&nbsp;You only need to perform this task one time. After you have the tenant ID, client ID and key, you can reuse them any time you need to create a new Azure AD access token.

1.  In Dev Center, go to your **Account settings**, click **Manage users**, and associate your organization's Dev Center account with your organization's Azure AD directory. For detailed instructions, see [Manage account users](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  In the **Manage users** page, click **Add Azure AD applications**, add the Azure AD application that represents the app or service that you will use to access submissions for your Dev Center account, and assign it the **Manager** role. If this application already exists in your Azure AD directory, you can select it on the **Add Azure AD applications** page to add it to your Dev Center account. Otherwise, you can create a new Azure AD application on the **Add Azure AD applications** page. For more information, see [Add and manage Azure AD applications](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Return to the **Manage users** page, click the name of your Azure AD application to go to the application settings, and copy down the **Tenant ID** and **Client ID** values.

4. Click **Add new key**. On the following screen, copy down the **Key** value. You won't be able to access this info again after you leave this page. For more information, see the information about managing keys in [Add and manage Azure AD applications](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## Step 2: Obtain an Azure AD access token

Before you call any of the methods in the Windows Store submission API, you must first obtain an Azure AD access token that you pass to the **Authorization** header of each method in the API. After you obtain an access token, you have 60 minutes to use it before it expires. After the token expires, you can refresh the token so you can continue to use it in further calls to the API.

To obtain the access token, follow the instructions in [Service to Service Calls Using Client Credentials](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) to send an HTTP POST to the ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` endpoint. Here is a sample request.

```
POST https://login.microsoftonline.com/<your_tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

For the *tenant\_id*, *client\_id* and *client\_secret* parameters, specify the tenant ID, client ID and the key for your application that you retrieved from Dev Center in the previous section. For the *resource* parameter, you must specify the ```https://manage.devcenter.microsoft.com``` URI.

After your access token expires, you can refresh it by following the instructions [here](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-submission-api">
## Step 3: Use the Windows Store submission API

After you have an Azure AD access token, you can call methods in the Windows Store submission API. The API includes many methods that are grouped into scenarios for apps, add-ons, and package flights. To create or update submissions, you typically call multiple methods in the Windows Store submission API in a specific order. For information about each scenario and the syntax of each method, see the articles in the following table.

>**Note**&nbsp;&nbsp;After you obtain an access token, you have 60 minutes to call methods in the Windows Store submission API before the token expires.

| Scenario       | Description                                                                 |
|---------------|----------------------------------------------------------------------|
| Apps |  Retrieve data for all the apps that are registered to your Windows Dev Center account and create submissions for apps. For more information about these methods, see the following articles: <ul><li>[Get app data](get-app-data.md)</li><li>[Manage app submissions](manage-app-submissions.md)</li></ul> |
| Add-ons | Get, create, or delete add-ons for your apps, and then get, create, or delete submissions for the add-ons. For more information about these methods, see the following articles: <ul><li>[Manage add-ons](manage-add-ons.md)</li><li>[Manage add-on submissions](manage-add-on-submissions.md)</li></ul> |
| Package flights | Get, create, or delete package flights for your apps, and then get, create, or delete submissions for the package flights. For more information about these methods, see the following articles: <ul><li>[Manage package flights](manage-flights.md)</li><li>[Manage package flight submissions](manage-flight-submissions.md)</li></ul> |

<span />

## Code examples

The following articles provide detailed code examples that demonstrate how to use the Windows Store submission API in several different programming languages:

* [C# code examples](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java code examples](java-code-examples-for-the-windows-store-submission-api.md)
* [Python code examples](python-code-examples-for-the-windows-store-submission-api.md)

## Troubleshooting

| Issue      | Resolution                                          |
|---------------|---------------------------------------------|
| After calling the Windows Store submission API from PowerShell, the response data for the API is corrupted if you convert it from JSON format to a PowerShell object using the [ConvertFrom-Json](https://technet.microsoft.com/en-us/library/hh849898.aspx) cmdlet and then back to JSON format using the [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) cmdlet. |  By default, the *-Depth* parameter for the [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) cmdlet is set to 2 levels of objects, which is too shallow for most of the JSON objects that are returned by the Windows Store submission API. When you call the [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) cmdlet, set the *-Depth* parameter to a larger number, such as 20. |

## Additional help

If you have questions about the Windows Store submission API or need assistance managing your submissions with this API, use the following resources:

* Ask your questions on our [forums](https://social.msdn.microsoft.com/Forums/windowsapps/en-us/home?forum=wpsubmit).
* Visit our [support page](https://developer.microsoft.com/windows/support) and request one of the assisted support options for Dev Center dashboard. If you are prompted to choose a problem type and category, choose **App submission and certification** and **Submitting an app**, respectively.  

## Related topics

* [Get app data](get-app-data.md)
* [Manage app submissions](manage-app-submissions.md)
* [Manage add-ons](manage-add-ons.md)
* [Manage add-on submissions](manage-add-on-submissions.md)
* [Manage package flights](manage-flights.md)
* [Manage package flight submissions](manage-flight-submissions.md)
 



<!--HONumber=Sep16_HO1-->


