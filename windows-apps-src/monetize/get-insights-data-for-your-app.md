---
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données insights pour votre application.
title: Obtenir des données insights
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10 uwp, Store, aux services analytique Microsoft Store API, insights
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1847f22f52eb066115b5681e745e74ec74f77f7d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662844"
---
# <a name="get-insights-data"></a>Obtenir des données insights

Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des insights données relatifs acquisitions, d’intégrité et les métriques d’utilisation pour une application pendant une période donnée et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport Insights](../publish/insights-report.md) dans Partner Center.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | Le [Store ID](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez récupérer des données insights. Si vous ne spécifiez pas ce paramètre, le corps de réponse contiennent des données insights pour toutes les applications inscrites à votre compte.  |  Non  |
| startDate | date | La date de début dans la plage de dates des données insights à récupérer. La valeur par défaut est de 30 jours avant la date actuelle. |  Non  |
| endDate | date | La date de fin dans la plage de dates des données insights à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Par exemple, *filtre = eq 'd’acquisition de type de données'*. <p/><p/>Vous pouvez spécifier les champs de filtre suivants :<p/><ul><li><strong>acquisition</strong></li><li><strong>health</strong></li><li><strong>usage</strong></li></ul> | Non   |

### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande pour l’obtention des données insights. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Un tableau d’objets qui contiennent des données pour l’application insights. Pour plus d’informations sur les données de chaque objet, consultez le [les valeurs Insight](#insight-values) section ci-dessous.                                                                                                                      |
| TotalCount | entier    | Nombre total de lignes dans les résultats de la requête.                 |


### <a name="insight-values"></a>Valeurs Insight

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | chaîne | ID de l’application pour laquelle vous récupérez des données insights Store.     |
| insightDate                | chaîne | La date sur laquelle nous avons identifié la modification dans une mesure spécifique. Cette date représente la fin de la semaine dans lequel nous avons détecté une augmentation significative ou diminuer dans une mesure par rapport à la semaine auparavant. |
| dataType     | chaîne | Une des chaînes suivantes qui spécifie la zone analytique général qui décrit cet insight :<p/><ul><li><strong>acquisition</strong></li><li><strong>health</strong></li><li><strong>usage</strong></li></ul>   |
| insightDetail          | tableau | Un ou plusieurs [InsightDetail valeurs](#insightdetail-values) qui représentent les détails pour obtenir des informations en cours.    |


### <a name="insightdetail-values"></a>Valeurs InsightDetail

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| FactName           | chaîne | Une des valeurs suivantes qui indique la mesure décrivant l’insight actuelle ou la dimension actuelle, basée sur le **dataType** valeur.<ul><li>Pour **intégrité**, cette valeur est toujours **Comptetoucher**.</li><li>Pour **acquisition**, cette valeur est toujours **AcquisitionQuantity**.</li><li>Pour **utilisation**, cette valeur peut être une des chaînes suivantes :<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | tableau |  Un ou plusieurs objets qui décrivent une mesure unique pour l’analyse.   |
| ChangementPourcentage            | chaîne |  Pourcentage de la métrique ont changé dans votre base de clients complète.  |
| DimensionName           | chaîne |  Le nom de la mesure décrit dans la dimension actuelle. Exemples **EventType**, **marché**, **DeviceType**, **PackageVersion**, **AcquisitionType**, **AgeGroup** et **sexe**.   |
| DimensionValue              | chaîne | La valeur de la métrique qui est décrite dans la dimension actuelle. Par exemple, si **DimensionName** est **EventType**, **DimensionValue** peut être **incident** ou **blocage** .   |
| FactValue     | chaîne | La valeur absolue de la mesure à la date de que l’information a été détectée.  |
| Direction | chaîne |  La direction de la modification (**positif** ou **négatif**).   |
| Date              | chaîne |  La date sur laquelle nous avons identifié les modifications liées à l’analyse actuelle ou de la dimension actuelle.   |

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

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’Insights](../publish/insights-report.md)
* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
