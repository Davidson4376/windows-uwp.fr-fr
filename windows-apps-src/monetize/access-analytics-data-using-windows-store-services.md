---
author: Xansky
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Utiliser l’API d’analytique Microsoft Store pour récupérer par programmation les données d’analytique pour les applications qui sont inscrites dans votre organisation '' s compte espace partenaires de Windows.
title: Accéder aux données d’analyse à l’aide des services du Windows Store
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, services du MicrosoftStore, API d'analyse du MicrosoftStore
ms.localizationpriority: medium
ms.openlocfilehash: a5d2689f5041035ba78cd9aef1f0610f0e1619dd
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6968479"
---
# <a name="access-analytics-data-using-store-services"></a>Accéder aux données d’analyse à l’aide des services du Windows Store

Utiliser *Microsoft Store analytique API* pour récupérer par programmation les données d’analytique pour les applications qui sont enregistrées sur le compte du centre de l’espace Windows de vous ou votre organisation. Cette API permet de récupérer des données sur les acquisitions, les erreurs, les évaluations et les avis sur les applications et les extensions (également connues sous le nom PIA, produit in-app). Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout:

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
2.  Avant d’appeler une méthode dans l’API d’analyse du MicrosoftStore, [procurez-vous un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token). Une fois le jeton obtenu, vous avez 60minutes pour l’utiliser dans les appels à l’API d’analyse du MicrosoftStore avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
3.  [Appelez l’API d’analyse du MicrosoftStore](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Étape: Remplir les conditions préalables à l’utilisation de l’API d’analyse du MicrosoftStore

Avant d’écrire le code d’appel de l’API d’analyse du MicrosoftStore, vérifiez que vous remplissez bien les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un nouvel Azure AD dans l’espace partenaires](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) pour sans frais supplémentaires.

* Vous devez associer une application Azure AD avec votre compte espace partenaires, récupérer l’ID de locataire et ID de client pour l’application et générer une clé. L’application Azure AD est l’app ou le service à partir duquel vous allez appeler l’API d’analyse du MicrosoftStore. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.
    > [!NOTE]
    > Cette tâche ne doit être effectuée qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID client et la clé à disposition, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.

Pour associer une application Azure AD avec votre compte espace partenaires et récupérer les valeurs nécessaires:

1.  Dans l’espace partenaires, [Associez le compte de l’espace partenaires de votre organisation avec un annuaire Azure AD de votre organisation](../publish/associate-azure-ad-with-dev-center.md).

2.  Ensuite, à partir de la page **utilisateurs** dans la section **paramètres du compte** de l’espace partenaires, [Ajoutez l’application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) qui représente l’application ou le service que vous allez utiliser pour accéder aux données d’analytique pour votre compte espace partenaires. Assurez-vous d'attribuer à cette application le rôle **Manager**. Si l’application n’existe pas encore dans votre répertoire Azure Active directory, vous pouvez [créer une nouvelle application Azure AD dans l’espace partenaires](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application, puis notez les valeurs des champs **ID de locataire** et **ID client**.

4. Cliquez sur **Ajouter une clé**. Dans l’écran suivant, notez la valeur du champ **Clé**. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape2: Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes dans l’API d’analyse du MicrosoftStore, vous devez d’abord obtenir un jeton d’accès Azure AD pour le passer à l’en-tête **Autorisation** de chaque méthode de l’API. Après avoir obtenu un jeton d’accès, vous disposez de 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

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

Pour la valeur *tenant\_id* dans l’URI POST et les paramètres *client\_id* et *client\_secret* , spécifiez l’ID de locataire, ID client et la clé pour votre application que vous avez récupérées à partir de l’espace partenaires dans la section précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Étape3: Appeler l’API d’analyse du MicrosoftStore

Une fois que vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler l’API d’analyse du MicrosoftStore. Vous devez transmettre le jeton d’accès à l’en-tête **Autorisation** de chaque méthode.

### <a name="methods-for-uwp-apps"></a>Méthodes d'analyse des apps UWP

Les méthodes analytique suivantes sont disponibles pour les applications UWP dans l’espace partenaires.

| Scénario       | Méthodes      |
|---------------|--------------------|
| Acquisitions, conversions, installations et utilisation |  <ul><li>[Obtenir les acquisitions d’applications](get-app-acquisitions.md)</li><li>[Obtenir les données relatives à l'entonnoir d'acquisition d'applications](get-acquisition-funnel-data.md)</li><li>[Obtenir les conversions d’applications par canal](get-app-conversions-by-channel.md)</li><li>[Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)</li><li>[Obtenir des acquisitions d’extensions d'inscription](get-subscription-acquisitions.md)</li><li>[Obtenir des conversions d'extensions par canal](get-add-on-conversions-by-channel.md)</li><li>[Obtenir les installations d’application](get-app-installs.md)</li><li>[Obtenir l’utilisation d’applications quotidiennes](get-app-usage-daily.md)</li><li>[Obtenir l’utilisation d’applications mensuelles](get-app-usage-monthly.md)</li></ul> |
| Erreurs d’application | <ul><li>[Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)</li><li>[Obtenir les informations sur une erreur de votre application](get-details-for-an-error-in-your-app.md)</li><li>[Obtenir la trace de pile concernant une erreur dans votre application](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Télécharger le fichier CAB concernant une erreur dans votre application](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Informations | <ul><li>[Obtenir les données d’analyse pour votre application](get-insights-data-for-your-app.md)</li></ul>  |
| Évaluations et avis | <ul><li>[Obtenir les évaluations des applications](get-app-ratings.md)</li><li>[Obtenir les avis sur les applications](get-app-reviews.md)</li></ul> |
| Publicités dans l'application et campagnes publicitaires | <ul><li>[Obtenir les données relatives aux performances publicitaires](get-ad-performance-data.md)</li><li>[Obtenir les données relatives aux performances de la campagne publicitaire](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Méthodes pour les applications de bureau

Les méthodes d'analyse suivantes peuvent être utilisées par des comptes de développeur appartenant au [programme d'application de bureau Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504).

| Scénario       | Méthodes      |
|---------------|--------------------|
| Installations |  <ul><li>[Obtenir des installations d’application de bureau](get-desktop-app-installs.md)</li></ul> |
| Blocs |  <ul><li>[Obtenir les blocs de mise à niveau pour votre application de bureau](get-desktop-block-data.md)</li><li>[Obtenir les détails des blocs de mise à niveau pour votre application de bureau](get-desktop-block-data-details.md)</li></ul> |
| Erreurs des applications |  <ul><li>[Obtenir des données de rapport d'erreur pour votre application de bureau](get-desktop-application-error-reporting-data.md)</li><li>[Obtenir les informations sur une erreur de votre application de bureau](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Obtenir la trace de pile concernant une erreur dans votre application de bureau](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Télécharger le fichier CAB concernant une erreur dans votre application de bureau](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Informations | <ul><li>[Obtenir les données d’analyse pour votre application de bureau](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Méthodes pour les services Xbox Live

Les méthodes supplémentaires suivantes sont disponibles pour les comptes de développeur, avec les jeux utilisant les [services Xbox Live ](../xbox-live/developer-program-overview.md).

| Scénario       | Méthodes      |
|---------------|--------------------|
| Analyse générale |  <ul><li>[Obtenir des données d'analyse Xbox Live](get-xbox-live-analytics.md)</li><li>[Obtenir des données de succès Xbox Live](get-xbox-live-achievements-data.md)</li><li>[Obtenir les données d’utilisation simultanée Xbox Live](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Analyse de l’intégrité |  <ul><li>[Obtenir des données d'intégrité Xbox Live](get-xbox-live-health-data.md)</li></ul> |
| Analyses de la communauté |  <ul><li>[Obtenir des données du hub de jeux Xbox Live](get-xbox-live-game-hub-data.md)</li><li>[Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)</li><li>[Obtenir des données multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Méthodes pour les jeux pour XboxOne

Les méthodes supplémentaires suivantes sont disponibles pour les comptes de développeur avec les jeux Xbox One intégrés par le biais du portail de développeur Xbox (XDP) et dans le tableau de bord XDP Analytique.

| Scénario       | Méthodes      |
|---------------|--------------------|
| Acquisitions |  <ul><li>[Obtenir des acquisitions de jeu Xbox One](get-xbox-one-game-acquisitions.md)</li><li>[Obtenir des acquisitions d’extensions XboxOne](get-xbox-one-add-on-acquisitions.md)</li></ul> |
| Erreurs |  <ul><li>[Obtenir les données rapport d’erreurs pour votre console Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md)</li><li>[Obtenir les détails d’une erreur dans votre console Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md)</li><li>[Obtenir la trace de pile concernant une erreur dans votre console Xbox One du jeu](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)</li><li>[Télécharger le fichier CAB concernant une erreur dans votre jeu Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>Méthodes pour le matériel et les pilotes

Comptes de développeur qui font partie du [programme du centre de développement matériel Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) ont accès à un ensemble supplémentaire de méthodes pour récupérer des données d’analytique pour le matériel et les pilotes. Pour plus d’informations, consultez le [tableau de bord du matériel API](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment obtenir un jeton d’accès AzureAD et appeler l’API d’analyse du MicrosoftStore à partir d’une app de console C#. Pour utiliser cet exemple de code, affectez les variables *tenantId*, *clientId*, *clientSecret*, et *appID* aux valeurs appropriées pour votre scénario. Cet exemple requiert le [package Json.NET](http://www.newtonsoft.com/json) de Newtonsoft afin de désérialiser les données JSON renvoyées par l’API d’analyse du MicrosoftStore.

> [!div class="tabbedCodeSnippets"]
[!code-cs[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Réponses d’erreur

L’API d’analyse du MicrosoftStore renvoie les réponses d’erreur dans un objet JSON qui contient des messages et des codes d’erreur. L’exemple suivant montre une réponse d’erreur causée par un paramètre non valide.

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
