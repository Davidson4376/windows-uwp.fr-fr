---
author: Xansky
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’analyse pour votre application.
title: Obtenir des données d’analyse
ms.author: mhopkins
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10, uwp, services du Windows Store, analytique du Microsoft Store API, des informations
ms.localizationpriority: medium
ms.openlocfilehash: 3d74d1f9cbdd374dfd363e6da1e98aafcb226d5e
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6659247"
---
# <a name="get-insights-data"></a>Obtenir des données d’analyse

Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’analyse en rapport avec les acquisitions, intégrité et les mesures de l’utilisation d’une application au cours d’une plage de dates données et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport de perspectives](../publish/insights-report.md) dans l’espace partenaires.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | L' [ID Windows Store](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez récupérer des données d’analyse. Si vous ne spécifiez pas ce paramètre, le corps de réponse contient les données d’analyse pour toutes les applications inscrites dans votre compte.  |  Non  |
| startDate | date | La date de début dans la plage de dates des données d’analyse à récupérer. La valeur par défaut est de 30jours avant la date actuelle. |  Non  |
| endDate | date | La date de fin dans la plage de dates des données d’analyse à récupérer. La valeur par défaut est la date actuelle |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Par exemple, *filter = dataType eq 'acquisition'*. <p/><p/>Vous pouvez spécifier les champs de filtre suivants:<p/><ul><li><strong>acquisition</strong></li><li><strong>santé</strong></li><li><strong>utilisation</strong></li></ul> | Non   |

### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention des données d’analyse. Remplacez la valeur *applicationId* par l’ID WindowsStore de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Un tableau d’objets contenant des données d’analyse de l’application. Pour plus d’informations sur les données de chaque objet, consultez la section de [valeurs de Insight](#insight-values) ci-dessous.                                                                                                                      |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête.                 |


### <a name="insight-values"></a>Valeurs des informations

Les éléments du tableau *Value* ont les valeurs suivantes:

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | chaîne | L’ID Windows Store de l’application pour laquelle vous récupérez les données d’analyse.     |
| insightDate                | chaîne | La date à laquelle nous avons identifié la modification d’un indicateur spécifique. Cette date représente la fin de la semaine dans lequel nous avons détecté une augmentation importante ou diminuer dans une unité de mesure par rapport à la semaine. |
| type de données     | chaîne | L’une des chaînes suivantes qui spécifie la zone d’analytique générale qui décrit ce insight:<p/><ul><li><strong>acquisition</strong></li><li><strong>santé</strong></li><li><strong>utilisation</strong></li></ul>   |
| insightDetail          | tableau | Une ou plusieurs [valeurs de InsightDetail](#insightdetail-values) qui représentent les détails de l’analyse en cours.    |


### <a name="insightdetail-values"></a>Valeurs de InsightDetail

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| FactName           | chaîne | Une des valeurs suivantes qui indique la métrique décrivant l’insight actuelle ou la dimension actuelle, basée sur la valeur de **type de données** .<ul><li>**Intégrité**, cette valeur est toujours **nombre d’accès**.</li><li>Pour l' **acquisition**, cette valeur est toujours **AcquisitionQuantity**.</li><li>Pour une **utilisation**, cette valeur peut être une des chaînes suivantes:<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | tableau |  Un ou plusieurs objets qui décrivent un indicateur unique pour l’analyse.   |
| ChangementPourcentage            | chaîne |  Le pourcentage de la métrique modifiée sur votre base de clients.  |
| Nomdimension           | chaîne |  Le nom de la métrique décrit dans la dimension actuelle. Des exemples **EventType**, **marché**, **type d’appareil**, **PackageVersion**, **AcquisitionType**, **AgeGroup** et **sexe**.   |
| DimensionValue              | chaîne | La valeur de la métrique qui est décrit dans la dimension actuelle. Par exemple, si **NomDimension** est **EventType**, **DimensionValue** peut-être **incident** ou **Raccrocher**.   |
| FactValue     | chaîne | La valeur absolue de la métrique à la date de que l’analyse a été détectée.  |
| Direction | chaîne |  La direction de la modification (**positif** ou **négatif**).   |
| Date              | chaîne |  La date à laquelle nous avons identifié la modification relatives à l’analyse en cours ou la dimension actuelle.   |

### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Rubriques associées

* [Rapport de perspectives](../publish/insights-report.md)
* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
