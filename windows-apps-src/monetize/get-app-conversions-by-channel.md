---
author: Xansky
description: Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour obtenir les données de conversion agrégées par canal d’une application pour une plage de dates données, et en fonction de filtres facultatifs.
title: Obtenir les conversions d’applications par canal
ms.author: mhopkins
ms.date: 08/04/2017
ms.topic: article
keywords: windows 10, uwp, services du MicrosoftStore, API d'analyse du MicrosoftStore, conversions des apps, canal
ms.localizationpriority: medium
ms.openlocfilehash: ecb5d5dfbfcbabbd3fa3004c84e2a1a5fff9f2d6
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6854542"
---
# <a name="get-app-conversions-by-channel"></a>Obtenir les conversions d’applications par canal

Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour obtenir les conversion agrégées par canal d’une application pour une plage de dates données, et en fonction de filtres facultatifs.

* Une *conversion* signifie qu’un nouveau client (connecté avec un compte Microsoft) a obtenu une licence pour votre application (qu’elle soit payante ou gratuite).
* Le *canal* est la méthode par laquelle un client est arrivé à la page de description de votre application (par exemple, via le Windows Store ou une [campagne de promotion d'application personnalisée](../publish/create-a-custom-app-promotion-campaign.md)).

Ces informations sont également disponibles dans le [rapport Acquisitions disponible](../publish/acquisitions-report.md#app-page-views-and-conversions-by-channel) dans l’espace partenaires.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez récupérer des données de conversion. Exemple d’ID Store: 9WZDNCRFJ3Q8. |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données de conversion. La valeur par défaut est 1/1/2016. |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données de conversion. La valeur par défaut est la date du jour. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent le corps de la réponse. Chaque instruction peut utiliser les opérateurs **eq** ou **ne**; les instructions peuvent être combinées à l’aide de **and** ou **or**. Vous pouvez spécifier les chaînes suivantes dans les instructions de filtre. Pour obtenir une description, consultez la section [valeurs de conversion](#conversion-values) dans cet article. <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>Voici un exemple de paramètre *filter*: <em>filter=deviceType eq 'PC'</em>.</p> | Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | chaîne | Une instruction qui commande les valeurs de données de résultats pour chaque conversion. La syntaxe est <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut être l’une des chaînes suivantes:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur <strong>asc</strong> ou <strong>desc</strong> pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple: <em>groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs demandes d’obtention des données de conversion d’application. Remplacez la valeur *applicationId* par l’ID Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets contenant des données de conversion agrégées pour l'application. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs de conversion](#conversion-values) ci-dessous.                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur10, mais que plus de10lignes de données de conversion sont associées à la requête. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête.      |


### <a name="conversion-values"></a>Valeurs de conversion

Les objets du tableau *Valeur* contiennent les valeurs suivantes:

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| date                | chaîne | Première date dans la plage de dates des données de conversion. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId       | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous récupérez les données de conversion.     |
| applicationName     | chaîne | Nom d'affichage de l’application pour laquelle vous récupérez des données de conversion.        |
| appType          | chaîne |  Type de produit pour lequel vous récupérez les données de conversion. Pour cette méthode, la seule valeur prise en charge est **App**            |
| customCampaignId           | chaîne |  La chaîne d’ID d'une [campagne de promotion d'application personnalisée](../publish/create-a-custom-app-promotion-campaign.md) qui est associée à l’application.   |
| referrerUriDomain           | chaîne |  Spécifie le domaine dans lequel la description d'application avec l’ID de campagne de promotion d'application personnalisée a été activée.   |
| channelType           | chaîne |  L’une des chaînes suivantes spécifiant le canal de la conversion:<ul><li><strong>CustomCampaignId</strong></li><li><strong>Store Traffic</strong></li><li><strong>Other</strong></li></ul>    |
| storeClient         | chaîne | La version du Windows Store dans laquelle la conversion s’est produite. Actuellement, la seule valeur prise en charge est **SFC**.    |
| deviceType          | chaîne | Une des chaînes suivantes:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>            |
| market              | chaîne | Le code pays ISO3166 du marché dans lequel la conversion s’est produite.    |
| clickCount              | nombre  |     Le nombre de clics de clients sur le lien de la description de votre application.      |           
| conversionCount            | nombre  |   Le nombre de conversions de client.         |          


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "App",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "US",
      "clickCount": 7,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport Acquisitions](../publish/acquisitions-report.md)
* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
