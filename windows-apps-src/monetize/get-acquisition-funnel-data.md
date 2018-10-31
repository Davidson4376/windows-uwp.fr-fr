---
author: Xansky
description: Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour obtenir les données de synthèse d’acquisition d’une application pour une plage de dates données, et en fonction de filtres facultatifs.
title: Obtenir les données relatives à l'entonnoir d'acquisition d'applications
ms.author: mhopkins
ms.date: 08/04/2017
ms.topic: article
keywords: windows 10, uwp, services du MicrosoftStore, API d'analyse du MicrosoftStore, acquisitions, synthèse
ms.localizationpriority: medium
ms.openlocfilehash: 20c07425542c207c2289c6b512102e10f64bab66
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5829654"
---
# <a name="get-app-acquisition-funnel-data"></a>Obtenir les données relatives à l'entonnoir d'acquisition d'applications

Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour obtenir les données de synthèse d’acquisition d’une application pour une plage de dates données, et en fonction de filtres facultatifs. Ces informations sont également disponibles dans le [rapport Acquisitions](../publish/acquisitions-report.md#acquisition-funnel) du tableau de bord du Centre de développement.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous voulez récupérer des données relatives à l'entonnoir d’acquisition. Exemple d’ID Store: 9WZDNCRFJ3Q8. |  Oui  |
| startDate | date | Dans la plage de dates, date de début de la récupération des données relatives à l'entonnoir d’acquisition. La valeur par défaut est la date du jour. |  Non  |
| endDate | date | Dans la plage de dates, date de fin de la récupération des données relatives à l'entonnoir d’acquisition. La valeur par défaut est la date du jour. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous. | Non   |

 
### <a name="filter-fields"></a>Champs de filtrage

Le paramètre *filter* de la requête contient une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ et une valeur qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**.

Les champs de filtre pris en charge sont les suivants. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*.

| Champs        |  Description        |
|---------------|-----------------|
| campaignId | La chaîne d’ID d'une [campagne de promotion d'application personnalisée](../publish/create-a-custom-app-promotion-campaign.md) qui est associée à l’acquisition. |
| market | Chaîne contenant le code pays ISO3166 du marché de l’acquisition. |
| deviceType | Une des chaînes suivantes spécifiant le type d’appareil sur lequel l’acquisition s’est produite:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Inconnu</strong></li></ul> |
| ageGroup | L'une des chaînes suivantes qui spécifie le groupe d'âge de l'utilisateur ayant effectué l’acquisition:<ul><li><strong>0 – 17</strong></li><li><strong>18 – 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 – 49</strong></li><li><strong>50ans ou plus</strong></li><li><strong>Inconnu</strong></li></ul> |
| gender | L'une des chaînes suivantes qui spécifie le sexe de l'utilisateur ayant effectué l’acquisition:<ul><li><strong>M</strong></li><li><strong>F</strong></li><li><strong>Inconnu</strong></li></ul> |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs demandes d’obtention des données relatives à l'entonnoir d’acquisition pour une application. Remplacez la valeur *applicationId* par l’ID Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets contenant des données relatives à l'entonnoir d’acquisition pour l'application. Pour plus d’informations sur les données de chaque objet, consultez la section [valeurs d'entonnoir](#funnel-values) ci-dessous.                  |
| TotalCount | entier    | Le nombre total d’objets dans le tableau *Valeur*.        |


### <a name="funnel-values"></a>Valeurs d'entonnoir

Les objets du tableau *Valeur* contiennent les valeurs suivantes:

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | chaîne | Une des chaînes suivantes qui spécifie le [type de données de l’entonnoir](../publish/acquisitions-report.md#acquisition-funnel) inclus dans cet objet:<ul><li><strong>PageView</strong></li><li><strong>Acquisition</strong></li><li><strong>Install</strong></li><li><strong>Usage</strong></li></ul> |
| UserCount       | chaîne | Nombre d’utilisateurs ayant effectué l’étape de l’entonnoir spécifié par la valeur *MetricType*.             |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "MetricType": "PageView",
      "UserCount": 100
    },
    {
      "MetricType": "Acquisition",
      "UserCount": 80
    },
    {
      "MetricType": "Install",
      "UserCount": 50
    },
    {
      "MetricType": "Usage",
      "UserCount": 10
    }
  ],
  "TotalCount": 4
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport Acquisitions](../publish/acquisitions-report.md)
* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
