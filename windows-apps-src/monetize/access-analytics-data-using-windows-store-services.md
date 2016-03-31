---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Use the Windows Store analytics API to programmatically retrieve analytics data for apps that are registered to your or your organization''s Windows Dev Center account.
title: Access analytics data using Windows Store services
---

# Access analytics data using Windows Store services


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Use the *Windows Store analytics API* to programmatically retrieve analytics data for apps that are registered to your or your organization's Windows Dev Center account. This API enables you to retrieve data for app and IAP acquisitions, errors, app ratings and reviews. This API uses Azure Active Directory (Azure AD) to authenticate the calls from your app or service.

## Prerequisites for using the Windows Store analytics API


-   You (or your organization) must have an Azure AD directory. If you already use Office 365 or other business services from Microsoft, you already have Azure AD directory. Otherwise, you can [get one for free](http://go.microsoft.com/fwlink/p/?LinkId=703757).
-   You must have a [user account](https://azure.microsoft.com/documentation/articles/active-directory-create-users/) in the Azure AD directory that you want to associate with your Windows Dev Center account.

## Using the Windows Store analytics API


Before you can use the Windows Store analytics API, you must associate an Azure AD application with your Dev Center account and obtain an Azure AD access token. The Azure AD application represents the app or service from which you want to call the Windows Store analytics API. After you have an access token, you can call the Windows Store analytics API from your app or service.

The following steps describe the end-to-end process:

1.  [Associate an Azure AD application with your Windows Dev Center account](#associate-an-azure-ad-application-with-your-windows-dev-center-account).
2.  [Obtain an Azure AD access token](#obtain-an-azure-ad-access-token).
3.  [Call the Windows Store analytics API](#call-the-windows-store-analytics-api).


### Associate an Azure AD application with your Windows Dev Center account

1.  In Dev Center, go to your **Account settings**, click **Manage users**, and associate your organization's Dev Center account with your organization's Azure AD directory. For detailed instructions, see [Manage account users](https://msdn.microsoft.com/library/windows/apps/mt489008). You can optionally add other users from your organization's Azure AD directory so they can also access the Dev Center account.

    **Note**  Only one Dev Center account can be associated with an Azure Active Directory. Similarly, only one Azure Active Directory can be associated with a Dev Center account. After you establish this association, you won't be able to remove it without contacting support.

     

2.  In the **Manage users** page, click **Add Azure AD applications**, add the Azure AD application that you will use to access analytics data for your Dev Center account, and assign it the **Manager** role. If this application already exists in your Azure AD directory, you can select it on the **Add Azure AD applications** to add it to your Dev Center account. Otherwise, you can create a new Azure AD application on the **Add Azure AD applications** page. For more information, see the section about managing Azure AD applications in [Manage account users](https://msdn.microsoft.com/library/windows/apps/mt489008).

3.  Return to the **Manage users** page, click the name of your Azure AD application to go to the application settings, and click **Add new key**. On the following screen, copy down the **Client ID** and **Key** values. For more information, see the section about managing Azure AD applications in [Manage account users](https://msdn.microsoft.com/library/windows/apps/mt489008). You need these the client ID and key to obtain an Azure AD access token to use when calling the Windows Store analytics API. You won't be able to access this info again after you leave this page.


### Obtain an Azure AD access token

After you associate an Azure AD application with your Dev Center account and you retrieve the client ID and key for the application, you can use this info to obtain an Azure AD access token. You need an access token before you can call any of the methods in the Windows Store analytics API.

To obtain the access token, follow the instructions in [Service to Service Calls Using Client Credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx) to send an HTTP POST to the following Azure AD endpoint.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

-   To get your tenant ID, log in to the [Azure Management Portal](http://manage.windowsazure.com/), navigate to **Active Directory**, and click the directory that you linked to your Dev Center account. The tenant ID for this directory is embedded in the URL for this page, as shown by the *your\_tenant\_ID* string in the example below.

  ```
  https://manage.windowsazure.com/@&lt;your_tenant_name&gt;#Workspaces/ActiveDirectoryExtension/Directory/&lt;your_tenant_ID&gt;/directoryQuickStart
  ```

-   For the *client\_id* and *client\_secret* parameters, specify the client ID and the key for your application that you retrieved earlier from Dev Center.
-   For the *resource* parameter, specify the following URI: **https://manage.devcenter.microsoft.com**.


### Call the Windows Store analytics API

After you have an Azure AD access token, you are ready to call the Windows Store analytics API. For information about the syntax of each method, see the following articles. You must pass the access token to the **Authorization** header of each method.

-   [Get app acquisitions](get-app-acquisitions.md)
-   [Get IAP acquisitions](get-in-app-acquisitions.md)
-   [Get error reporting data](get-error-reporting-data.md)
-   [Get app ratings](get-app-ratings.md)
-   [Get app reviews](get-app-reviews.md)

## Code example


The following code example demonstrates how to obtain an Azure AD access token and call the Windows Store analytics API from a C\# console app. To use this code example, assign the *tenantId*, *clientId*, *clientSecret*, and *appID* variables to the appropriate values for your scenario. This example requires the [Json.NET package](http://www.newtonsoft.com/json) from Newtonsoft to deserialize the JSON data returned by the Windows Store analytics API.

```CSharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace TestAnalyticsAPI
{
    class Program
    {
        static void Main(string[] args)
        {
            string tenantId = "<your tenant ID>";
            string clientId = "<your client ID>";
            string clientSecret = "<your secret>";

            string scope = "https://manage.devcenter.microsoft.com";

            // Retrieve an Azure AD access token
            string accessToken = GetClientCredentialAccessToken(
                    tenantId,
                    clientId,
                    clientSecret,
                    scope).Result;

            // This is your app&#39;s product ID. This ID is embedded in the app&#39;s listing link
            // on the App identity page of the Dev Center dashboard.
            string appID = "<your app&#39;s product ID>";

            DateTime startDate = DateTime.Parse("08-01-2015");
            DateTime endDate = DateTime.Parse("11-01-2015");
            int pageSize = 1000;
            int startPageIndex = 0;

            // Call the Windows Store analytics API
            CallAnalyticsAPI(accessToken, appID, startDate, endDate, pageSize, startPageIndex);

            Console.Read();
        }

        private static void CallAnalyticsAPI(string accessToken, string appID, DateTime startDate, DateTime endDate, int top, int skip)
        {
            string requestURI;

            // Get app acquisitions
            requestURI = string.Format(
                "https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId={0}&amp;startDate={1}&amp;endDate={2}&amp;top={3}&amp;skip={4}",
                appID, startDate, endDate, top, skip);

            //// Get IAP acquisitions
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId={0}&amp;startDate={1}&amp;endDate={2}&amp;top={3}&amp;skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app failures
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId={0}&amp;startDate={1}&amp;endDate={2}&amp;top={3}&amp;skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app ratings
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId={0}&amp;startDate={1}&amp;endDate={2}&amp;top={3}&amp;skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app reviews
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId={0}&amp;startDate={1}&amp;endDate={2}&amp;top={3}&amp;skip={4}",
            //    appID, startDate, endDate, top, skip);

            HttpRequestMessage requestMessage = new HttpRequestMessage(HttpMethod.Get, requestURI);
            requestMessage.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            WebRequestHandler handler = new WebRequestHandler();
            HttpClient httpClient = new HttpClient(handler);

            HttpResponseMessage response = httpClient.SendAsync(requestMessage).Result;

            Console.WriteLine(response);
            Console.WriteLine(response.Content.ReadAsStringAsync().Result);

            response.Dispose();
        }

        public static async Task<string> GetClientCredentialAccessToken(string tenantId, string clientId, string clientSecret, string scope)
        {
            string tokenEndpointFormat = "https://login.microsoftonline.com/{0}/oauth2/token";
            string tokenEndpoint = string.Format(tokenEndpointFormat, tenantId);

            dynamic result;
            using (HttpClient client = new HttpClient())
            {
                string tokenUrl = tokenEndpoint;
                using (
                    HttpRequestMessage request = new HttpRequestMessage(
                        HttpMethod.Post,
                        tokenUrl))
                {
                    string content =
                        string.Format(
                            "grant_type=client_credentials&amp;client_id={0}&amp;client_secret={1}&amp;resource={2}",
                            clientId,
                            clientSecret,
                            scope);

                    request.Content = new StringContent(content, Encoding.UTF8, "application/x-www-form-urlencoded");

                    using (HttpResponseMessage response = await client.SendAsync(request))
                    {
                        string responseContent = await response.Content.ReadAsStringAsync();
                        result = JsonConvert.DeserializeObject(responseContent);
                    }
                }
            }

            return result.access_token;
        }
    }
}
```

## Error responses


The Windows Store analytics API returns error responses in a JSON object that contains error codes and messages. The following example demonstrates an error response caused by an invalid parameter.

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```

## Related topics

* [Get app acquisitions](get-app-acquisitions.md)
* [Get IAP acquisitions](get-in-app-acquisitions.md)
* [Get error reporting data](get-error-reporting-data.md)
* [Get app ratings](get-app-ratings.md)
* [Get app reviews](get-app-reviews.md)
 
<!--HONumber=Mar16_HO1-->
