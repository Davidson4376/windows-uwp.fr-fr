---
author: mcleanbyron
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des informations concernant votre application de bureau.
title: Obtenir les informations concernant votre application de bureau
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, services du Windows Store, analytique du Microsoft Store API, des informations
ms.localizationpriority: medium
ms.openlocfilehash: e7ca6eed40af37276b5b4c98ec7b1b709bdadfb9
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3383405"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Obtenir les informations concernant votre application de bureau

Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des informations sur les données relatives aux mesures d’intégrité pour une application de bureau que vous avez ajoutée au [programme d’Application de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Ces données sont également disponibles dans le [rapport d’intégrité](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report) pour les applications de bureau dans le tableau de bord du centre de développement Windows.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID de produit de l’application de bureau pour laquelle vous souhaitez obtenir les informations. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un [rapport d'analyse du centre de développement relatif à votre application de bureau](https://msdn.microsoft.com/library/windows/desktop/mt826504) (comme le **rapport d’intégrité**) et récupérez l’ID de produit à partir de l’URL. Si vous ne spécifiez pas ce paramètre, le corps de réponse contient les données des informations pour toutes les applications inscrites dans votre compte.  |  Non  |
| startDate | date | La date de début dans la plage de dates des données des informations de récupération. La valeur par défaut est de 30jours avant la date actuelle. |  Non  |
| endDate | date | La date de fin dans la plage de dates des données des informations de récupération. La valeur par défaut est la date actuelle |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Par exemple, *filter = dataType eq 'acquisition'*. <p/><p/>Cette méthode prend uniquement en **l’intégrité**filtre.  | Non   |

### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention des données des informations. Remplacez la valeur de *applicationId* avec la valeur appropriée pour votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | array  | Tableau d’objets qui contiennent des données des informations de l’application. Pour plus d’informations sur les données de chaque objet, consultez la section de [valeurs de Insight](#insight-values) ci-dessous.                                                                                                                      |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête.                 |


### <a name="insight-values"></a>Valeurs Insight

Les éléments du tableau *Value* ont les valeurs suivantes:

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | chaîne | L’ID de produit de l’application de bureau pour laquelle vous souhaitez récupérer des données des informations.     |
| insightDate                | chaîne | La date à laquelle nous avons identifié la modification d’un indicateur spécifique. Cette date représente la fin de la semaine dans lequel nous avons détecté une augmentation importante ou diminuer dans une unité de mesure par rapport à la semaine. |
| type de données     | chaîne | Une chaîne qui spécifie la zone d’analytique générale qui informe cette insight. Actuellement, cette méthode prend uniquement en charge **la santé**.    |
| insightDetail          | array | Une ou plusieurs [valeurs de InsightDetail](#insightdetail-values) qui représentent les détails pour insight actuel.    |


### <a name="insightdetail-values"></a>Valeurs de InsightDetail

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| FactName           | chaîne | Une chaîne qui indique la métrique décrivant l’insight actuelle ou la dimension actuelle. Actuellement, cette méthode prend uniquement en charge la valeur du **nombre d’accès**.  |
| SubDimensions         | array |  Un ou plusieurs objets qui décrivent un indicateur unique pour l’aperçu.   |
| ChangementPourcentage            | chaîne |  Le pourcentage de la métrique modifiée sur votre base de clients.  |
| Nomdimension           | chaîne |  Le nom de la métrique décrit dans la dimension actuelle. Exemples **EventType**, **marché**, **DeviceType**et **PackageVersion**.   |
| DimensionValue              | chaîne | La valeur de la métrique qui est décrit dans la dimension actuelle. Par exemple, si **NomDimension** est **EventType**, **DimensionValue** peut être **incident** ou **Raccrocher**.   |
| FactValue     | chaîne | La valeur absolue de la métrique sur la date de que détection de l’aperçu.  |
| Direction | chaîne |  La direction de la modification (**positif** ou **négatif**).   |
| Date              | chaîne |  La date à laquelle nous avons identifié la modification relatives à l’insight actuelle ou la dimension actuelle.   |

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

* [Programme d’Application de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Rapport d’intégrité](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
