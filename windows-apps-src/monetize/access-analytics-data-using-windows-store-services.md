---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: "Utilisez l’API d’analyse du WindowsStore pour récupérer par programme les données d’analyse pour les applications qui sont enregistrées sur votre compte personnel ou compte d’organisation du Centre de développement Windows."
title: "Accéder aux données d’analyse à l’aide des services du Windows Store"
ms.sourcegitcommit: 204bace243fb082d3ca3b4259982d457f9c533da
ms.openlocfilehash: 30388a975e9623c5511abe608aa1b21956e2c974

---

# Accéder aux données d’analyse à l’aide des services du Windows Store


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Utilisez l*’API d’analyse du Windows Store* pour récupérer par programme les données d’analyse pour les applications qui sont enregistrées sur votre compte, ou celui de votre organisation, du Centre de développement Windows. Cette API permet de récupérer les données pour les acquisitions de produits in-app et d’applications, les erreurs, ainsi que les évaluations et avis relatifs aux applications. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels à partir de votre application ou service.

## Conditions préalables à l’utilisation de l’API d’analyse du Windows Store


-   Vous (ou votre organisation) devez disposer d’un annuaire Azure AD. Si vous utilisez Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Sinon, vous pouvez l’[obtenir gratuitement](http://go.microsoft.com/fwlink/p/?LinkId=703757).
-   Vous devez disposer d’un [compte d’utilisateur](https://azure.microsoft.com/documentation/articles/active-directory-create-users/) dans l’annuaire Azure AD que vous voulez associer à votre compte du Centre de développement Windows.

## Utilisation de l’API d’analyse du Windows Store


Avant de pouvoir utiliser l’API d’analyse du Windows Store, vous devez associer une application Azure AD à votre compte du Centre de développement et obtenir un jeton d’accès Azure AD. L’application Azure AD est l’application ou le service à partir duquel vous voulez appeler l’API d’analyse du Windows Store. Une fois que vous disposez d’un jeton d’accès, vous pouvez appeler l’API d’analyse du Windows Store à partir de votre service ou application.

Les étapes suivantes décrivent l’ensemble du processus:

1.  [Associez une application Azure AD à votre compte du Centre de développement Windows](#associate-an-azure-ad-application-with-your-windows-dev-center-account).
2.  [Obtenez un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token).
3.  [Appelez l’API d’analyse du Windows Store](#call-the-windows-store-analytics-api).


### Associer une application Azure AD à votre compte du Centre de développement Windows

1.  Dans le Centre de développement, accédez aux **Paramètres du compte**, cliquez sur **Gérer les utilisateurs**, et associez votre compte du Centre de développement à l’annuaire Azure AD de votre organisation. Pour obtenir des instructions détaillées, voir [Gérer les utilisateurs de comptes](https://msdn.microsoft.com/library/windows/apps/mt489008). Vous pouvez éventuellement ajouter d’autres utilisateurs à partir de l’annuaire Azure AD de votre organisation afin qu’ils puissent également accéder au compte du Centre de développement.

    > **Remarque** Vous ne pouvez associer qu’un seul compte du Centre de développement à un service Azure Active Directory. De même, il n’est possible d’associer qu’un seul service Azure Active Directory à un compte du Centre de développement. Une fois cette association établie, vous ne pouvez plus la supprimer sans prendre contact avec le support.

     

2.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des applications AzureAD**, ajoutez l’application AzureAD qui représente l’application ou le service que vous utiliserez pour accéder aux données d’analyse de votre compte du Centre de développement, puis affectez-lui le rôle **Gestionnaire**. Si cette application existe déjà dans votre annuaire AzureAD, vous pouvez la sélectionner dans la page **Ajouter des applications Azure AD** pour l’ajouter à votre compte du Centre de développement. Sinon, vous pouvez créer une application Azure AD sur la page **Ajouter des applications Azure AD**. Pour en savoir plus, voir la section sur la gestion des applications Azure AD dans [Gérer les utilisateurs de comptes](https://msdn.microsoft.com/library/windows/apps/mt489008).

3.  Revenez à la page **Gérer les utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres, puis cliquez sur **Ajouter une nouvelle clé**. Sur l’écran suivant, copiez les valeurs de l’**ID client** et de la **clé**. Pour en savoir plus, voir la section sur la gestion des applications Azure AD dans [Gérer les utilisateurs de comptes](https://msdn.microsoft.com/library/windows/apps/mt489008). L’ID client et la clé permettent d’obtenir un jeton d’accès Azure AD à utiliser lors de l’appel de l’API d’analyse du Windows Store. Vous ne serez plus en mesure d’accéder à ces informations une fois que vous aurez quitté cette page.


### Obtenir un jeton d’accès Azure AD

Après avoir associé l’application Azure AD à votre compte du Centre de développement et récupéré l’ID client et la clé de l’application, vous pouvez utiliser ces informations pour obtenir un jeton d’accès Azure AD. Vous avez besoin d’un jeton d’accès pour pouvoir appeler les méthodes dans l’API d’analyse du Windows Store. Après avoir créé un jeton d’accès, vous disposez de 60minutes pour l’utiliser avant son expiration.

Pour obtenir le jeton d’accès, suivez les instructions de [Appels de service à service à l’aide des informations d’identification du client](https://msdn.microsoft.com/library/azure/dn645543.aspx) pour envoyer une requête HTTP POST au point de terminaison Azure AD suivant.

```syntax
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

-   Pour obtenir votre ID de locataire, se connecter au [portail de gestion Azure](http://manage.windowsazure.com/), accédez à **Active Directory**, puis cliquez sur l’annuaire que vous avez lié à votre compte du Centre de développement. L’ID de locataire pour ce répertoire est intégré à l’URL de cette page, comme illustré par la chaîne *your\_tenant\_ID* dans l’exemple ci-dessous.

  ```syntax
  https://manage.windowsazure.com/@<your_tenant_name>#Workspaces/ActiveDirectoryExtension/Directory/<your_tenant_ID>/directoryQuickStart
  ```

-   Pour les paramètres *client\_id* et *client\_secret*, spécifiez l’ID client et la clé de votre application récupérée précédemment à partir du Centre de développement.
-   Pour le paramètre *resource*, spécifiez l’URI suivant : ```https://manage.devcenter.microsoft.com```.


### Appeler l’API d’analyse du WindowsStore

Une fois que vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler l’API d’analyse du Windows Store. Pour plus d’informations sur la syntaxe de chacune de ces méthodes, voir les articles suivants. Vous devez transmettre le jeton d’accès à l’en-tête **Authorization** de chaque méthode.

-   [Obtenir les acquisitions d’applications](get-app-acquisitions.md)
-   [Obtenir les acquisitions de produits in-app](get-in-app-acquisitions.md)
-   [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
-   [Obtenir les classifications des applications](get-app-ratings.md)
-   [Obtenir les avis sur les applications](get-app-reviews.md)

## Exemple de code


L’exemple de code suivant montre comment obtenir un jeton d’accès AzureAD et appeler l’API d’analyse du WindowsStore à partir d’une application de console C#. Pour utiliser cet exemple de code, affectez les variables *tenantId*, *clientId*, *clientSecret*, et *appID* aux valeurs appropriées pour votre scénario. Cet exemple requiert le [package Json.NET](http://www.newtonsoft.com/json) de Newtonsoft afin de désérialiser les données JSON renvoyées par l’API d’analyse du Windows Store.

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

            // This is your app's Store ID. This ID is available on
            // the App identity page of the Dev Center dashboard.
            string appID = "<your app's Store ID>";

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
                "https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
                appID, startDate, endDate, top, skip);

            //// Get IAP acquisitions
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app failures
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app ratings
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId={0}&startDate={1}&endDate={2}top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app reviews
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
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
                            "grant_type=client_credentials&client_id={0}&client_secret={1}&resource={2}",
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

## Réponses d’erreur


L’API d’analyse du Windows Store renvoie les réponses d’erreur dans un objet JSON qui contient des messages et des codes d’erreur. L’exemple suivant montre une réponse d’erreur causée par un paramètre non valide.

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

## Rubriques connexes

* [Obtenir les acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir les acquisitions de produits in-app](get-in-app-acquisitions.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir les classifications des applications](get-app-ratings.md)
* [Obtenir les avis sur les applications](get-app-reviews.md)
 



<!--HONumber=Jun16_HO5-->


