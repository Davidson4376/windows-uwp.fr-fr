---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: "Utilisez l’API d’analyse du WindowsStore pour récupérer par programmation les données d’analyse pour les applications qui sont enregistrées sur votre compte personnel ou compte d’organisation du Centre de développement Windows."
title: "Accéder aux données d’analyse à l’aide des services du Windows Store"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, services du Windows Store, API d&quot;analyse du WindowsStore
ms.openlocfilehash: aa33af63a49d890b3c60ec1bee32528cfc78af93
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="access-analytics-data-using-store-services"></a>Accéder aux données d’analyse à l’aide des services du Windows Store

Utilisez l’*API d’analyse du Windows Store* pour récupérer par programmation les données d’analyse des applications inscrites dans votre compte, ou celui de votre organisation, du Centre de développement Windows. Cette API permet de récupérer des données sur les acquisitions, les erreurs, les évaluations et les avis sur les applications et les extensions (également connues sous le nom PIA, produit in-app). Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout:

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
2.  Avant d’appeler une méthode dans l’API d’analyse du Windows Store, [procurez-vous un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token). Une fois le jeton obtenu, vous avez 60minutes pour l’utiliser dans les appels à l’API d’analyse du Windows Store avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
3.  [Appelez l’API d’analyse du Windows Store](#call-the-windows-store-analytics-api).

<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-analytics-api"></a>Étape: Remplir les conditions préalables à l’utilisation de l’API d’analyse du Windows Store

Avant d’écrire le code d’appel de l’API d’analyse du Windows Store, vérifiez que vous remplissez bien les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un annuaire Azure AD dans le Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) sans frais supplémentaires.

* Vous devez associer une application Azure AD à votre compte du Centre de développement, récupérer l’ID de locataire et l’ID client pour l’application et générer une clé. L’application Azure AD est l’application ou le service à partir duquel vous allez appeler l’API d’analyse du Windows Store. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.

  >**Remarque**&nbsp;&nbsp;Vous n’avez besoin d’effectuer cette tâche qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID client et la clé à disposition, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.

Pour associer une application Azure AD à votre compte du Centre de développement Windows et récupérer les valeurs nécessaires:

1.  Dans le Centre de développement, accédez à vos **Paramètres du compte**, cliquez sur **Gérer les utilisateurs**, puis associez le compte du Centre de développement de votre organisation à son annuaire Azure AD. Pour obtenir des instructions détaillées, voir [Gérer les utilisateurs de comptes](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  Dans la page **Gérer les utilisateurs**, cliquez sur **Ajouter des applications AzureAD**, ajoutez l’application AzureAD qui représente l’application ou le service que vous allez utiliser pour accéder aux données d’analyse de votre compte du Centre de développement, puis affectez-lui le rôle **Gestionnaire**. Si cette application existe déjà dans votre annuaire AzureAD, vous pouvez la sélectionner dans la page **Ajouter des applications Azure AD** pour l’ajouter à votre compte du Centre de développement. Sinon, vous pouvez créer une application Azure AD dans la page **Ajouter des applications Azure AD**. Pour plus d’informations, voir [Ajouter et gérer des applications Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Revenez à la page **Gérer les utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application, puis notez les valeurs des champs **ID de locataire** et **ID client**.

4. Cliquez sur **Ajouter une clé**. Dans l’écran suivant, notez la valeur du champ **Clé**. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page. Pour en savoir plus, consultez les informations sur la gestion des clés dans l’article [Ajouter et gérer des applications Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape2: Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes dans l’API d’analyse du Windows Store, vous devez d’abord obtenir un jeton d’accès Azure AD pour le passer à l’en-tête **Autorisation** de chaque méthode de l’API. Après avoir obtenu un jeton d’accès, vous disposez de 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

Pour obtenir le jeton d’accès, suivez les instructions présentées dans l’article [Appels de service à service à l’aide des informations d’identification du client](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) pour envoyer une requête HTTP POST au point de terminaison ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Voici un exemple de requête.

```
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Pour la valeur *tenant\_id* dans l’URI POST et les paramètres *client\_id* et *client\_secret*, spécifiez l’ID de locataire, l’ID client et la clé pour votre application que vous avez récupérés à partir du Centre de développement à l’étape précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />
## <a name="step-3-call-the-windows-store-analytics-api"></a>Étape3: Appeler l’API d’analyse du Windows Store

Une fois que vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler l’API d’analyse du Windows Store. Pour plus d’informations sur la syntaxe de chacune de ces méthodes, voir les articles suivants. Vous devez transmettre le jeton d’accès à l’en-tête **Autorisation** de chaque méthode.

| Scénario       | Méthodes      |
|---------------|--------------------|
| Acquisitions et installations |  <ul><li>[Obtenir des acquisitions d’applications](get-app-acquisitions.md)</li><li>[Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)</li><li>[Obtenir des installations d’application](get-app-installs.md)</li></ul> |
| Erreurs d’application | <ul><li>[Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)</li><li>[Obtenir les informations sur une erreur de votre application](get-details-for-an-error-in-your-app.md)</li><li>[Obtenir la trace de pile concernant une erreur dans votre application](get-the-stack-trace-for-an-error-in-your-app.md)</li></ul> |
| Évaluations et avis | <ul><li>[Obtenir les évaluations des applications](get-app-ratings.md)</li><li>[Obtenir les avis sur les applications](get-app-reviews.md)</li></ul> |
| Publicités dans l'application et campagnes publicitaires | <ul><li>[Obtenir les données relatives aux performances publicitaires](get-ad-performance-data.md)</li><li>[Obtenir les données relatives aux performances de la campagne publicitaire](get-ad-campaign-performance-data.md)</li></ul> |

Les méthodes supplémentaires suivantes sont disponibles pour les comptes de développeur qui participent au [programme du centre de développement matériel Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

| Scénario       | Description      |
|---------------|--------------------|
| Erreurs dans les pilotes de Windows10 (pour les fabricants de matériel) |  <ul><li>[Obtenir les données de rapport d’erreurs pour les pilotes Windows 10](get-error-reporting-data-for-windows-10-drivers.md)</li><li>[Obtenir des détails sur une erreur de pilote Windows10](get-details-for-a-windows-10-driver-error.md)</li><li>[Téléchargez le fichier CAB pour une erreur de pilote Windows10](download-the-cab-file-for-a-windows-10-driver-error.md)</li></ul> |
| Erreurs dans les pilotes Windows7/Windows8.x (pour les fabricants de matériel) |  <ul><li>[Récupérer des données de rapport d'erreur pour les pilotes Windows7 et Windows8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)</li><li>[Obtenir des informations sur une erreur de pilote Windows7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md)</li><li>[Téléchargez le fichier CAB pour une erreur de pilote Windows 7 ou Windows8.x](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)</li></ul> |
| Erreurs matérielles (pour les fabricants OEM) |  <ul><li>[Obtenir les données de rapport d’erreurs matérielles de fabricant d'ordinateurs (OEM)](get-oem-hardware-error-reporting-data.md)</li><li>[Obtenir des informations détaillées sur une erreur matérielle d'OEM](get-details-for-an-oem-hardware-error.md)</li><li>[Téléchargez le fichier CAB pour une erreur matérielle d'OEM](download-the-cab-file-for-an-oem-hardware-error.md)</li></ul> |

## <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment obtenir un jeton d’accès AzureAD et appeler l’API d’analyse du WindowsStore à partir d’une application de console C#. Pour utiliser cet exemple de code, affectez les variables *tenantId*, *clientId*, *clientSecret*, et *appID* aux valeurs appropriées pour votre scénario. Cet exemple requiert le [package Json.NET](http://www.newtonsoft.com/json) de Newtonsoft afin de désérialiser les données JSON renvoyées par l’API d’analyse du Windows Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Réponses d’erreur

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
