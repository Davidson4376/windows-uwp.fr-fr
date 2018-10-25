---
author: Xansky
description: Utilisez cette méthode dans l'API d'analyse du MicrosoftStore pour obtenir les données multijoueurs Xbox Live.
title: Obtenir des données multijoueur Xbox Live
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, services de MicrosoftStore, API d'analyse du MicrosoftStore, analyse Xbox Live, multijoueurs
ms.localizationpriority: medium
ms.openlocfilehash: 84255c97186accfcafc561eeb77e0fdb6924b5e9
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5481243"
---
# <a name="get-xbox-live-multiplayer-data"></a>Obtenir des données multijoueur Xbox Live


Utilisez cette méthode dans l'API d'analyse du MicrosoftStore pour obtenir les données multijoueurs pour votre  [jeu compatible Xbox Live](../xbox-live/index.md) chaque jour ou chaque mois. Ces informations sont également disponibles dans le [rapport d'analyse Xbox](../publish/xbox-analytics-report.md) du tableau de bord du Centre de développement.

> [!IMPORTANT]
> Cette méthode prend uniquement en charge les jeux pour Xbox et les jeux qui utilisent les services Xbox Live. Ces jeux doivent passer par le [processus d’approbation de concept](../gaming/concept-approval.md), qui inclut les jeux publiés par des [partenaires Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) et les jeux soumis via le [programme ID@Xbox](../xbox-live/developer-program-overview.md#id). Cette méthode ne prend actuellement pas en charge les jeux publiés via le [Programme Créateurs Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Éléments prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête


| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous voulez récupérer des données multijoueurs Xbox Live.  |  Oui  |
| metricType | chaîne | Une chaîne qui spécifie le type de données d’analytique Xbox Live à récupérer. Pour cette méthode, spécifiez la valeur **multiplayerdaily** pour obtenir des données multijoueur quotidiennes ou **multiplayermonthly** des données multijoueur mensuelles.  |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données multijoueurs. Pour **multiplayerdaily**, la valeur par défaut est de 3 mois avant la date actuelle. Pour **multiplayermonthly**, la valeur par défaut est de 1 an avant la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données multijoueurs. La valeur par défaut est la date du jour. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>marché</strong></li><li><strong>subscriptionName</strong></li></ul> | Non   |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>marché</strong></li><li><strong>subscriptionName</strong></li></ul><p/>Si vous spécifiez un ou plusieurs champs *groupby*, tous les autres champs *groupby* non spécifiés indiqueront la valeur **All** dans le corps de réponse. |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données multijoueurs pour votre jeu prenant en charge Xbox Live. Remplacez la valeur *applicationId* par l’ID Store de votre jeu.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

| Value      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | array  | Un tableau d’objets qui contient des données du mode multijoueur, où chaque objet représente un ensemble de données pour la période quotidienne ou mensuelle spécifiée qui est organisé selon les valeurs**filter** et **groupby**. Pour plus d’informations sur les données de chaque objet, consultez les sections [Analyse quotidienne des données multijoueurs](#daily-multiplayer-analytics) et [Analyse mensuelle des données multijoueurs](#monthly-multiplayer-analytics).     |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10000 mais que la requête présente plus de 10000rangées de données. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête. |


### <a name="daily-multiplayer-analytics"></a>Analyse quotidienne des données multijoueurs

Les éléments du tableau *Value* qui contiennent les valeurs suivantes lorsque vous demandez des données d'analyse quotidienne du mode multijoueur (autrement dit, lorsque vous spécifiez **multiplayerdaily** pour le paramètre **metricType**).

| Value               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| date                | chaîne | La date pour les données en mode multijoueur. |
| applicationId       | chaîne | L’ID Store du jeu pour laquelle vous récupérez les données multijoueurs.     |
| applicationName       | chaîne |  Le nom du jeu pour laquelle vous récupérez les données multijoueurs.     |
| marché       | chaîne | Le code pays à deux lettres ISO3166 du marché d'où proviennent les données du mode multijoueur.       |
| packageVersion     | chaîne |  La version du package en quatre parties du jeu.  |
| deviceType          | chaîne | Une des chaînes suivantes spécifiant le type d’appareil d'où proviennent les données du mode multijoueur:<p/><ul><li><strong>Console</strong></li><li><strong>PC</strong></li><li>**Inconnue**</li></ul>  |
| subscriptionName     | chaîne |  Le nom de l’abonnement utilisé pour les données en mode multijoueur. Les valeurs possibles sont **Xbox Game Pass** et **""** (aucun abonnement).  |
| dailySessionCount     | nombre |  Le nombre de sessions multijoueurs pour le jeu à la date spécifiée.  |
| engagementDurationMinutes     | nombre |  Le nombre total de minutes pendant lesquelles les clients se sont livrés à des sessions multijoueurs pour le jeu à la date spécifiée.  |
| dailyActiveUsers     | nombre |  Le nombre total d'utilisateurs multijoueurs actifs pour le jeu à la date spécifiée.  |
| dailyActiveDevices     | nombre |  Le nombre total d'appareils actifs où se sont déroulées des sessions multijoueurs pour le jeu à la date spécifiée.  |
| dailyNewUsers     | nombre |  Le nombre total de nouveaux utilisateurs multijoueurs actifs pour le jeu à la date spécifiée.  |
| monthlyActiveUsers     | nombre |  Le nombre total d'utilisateurs multijoueurs actifs pour le mois correspondant à la date spécifiée.  |
| monthlyActiveDevices     | nombre | Le nombre total d'appareils actifs où se sont déroulées des sessions multijoueurs pour le mois correspondant à la date spécifiée.   |
| monthlyNewUsers     | nombre |  Le nombre total de nouveaux utilisateurs multijoueurs pour le jeu, pendant le mois correspondant à la date spécifiée.  |


### <a name="monthly-multiplayer-analytics"></a>Analyse mensuelle des données multijoueurs

Les éléments du tableau *Value* qui contiennent les valeurs suivantes lorsque vous demandez des données d'analyse mensuelle du mode multijoueur (autrement dit, lorsque vous spécifiez **multiplayermonthly** pour le paramètre **metricType**).

| Value               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| date                | chaîne | Première date du mois pour les données multijoueurs. |
| applicationId       | chaîne | L’ID Store du jeu pour laquelle vous récupérez les données multijoueurs.     |
| applicationName       | chaîne |  Le nom du jeu pour laquelle vous récupérez les données multijoueurs.     |
| marché       | chaîne | Le code pays à deux lettres ISO3166 du marché d'où proviennent les données du mode multijoueur.       |
| packageVersion     | chaîne |  La version du package en quatre parties du jeu.  |
| deviceType          | chaîne | Une des chaînes suivantes spécifiant le type d’appareil d'où proviennent les données du mode multijoueur:<p/><ul><li><strong>Console</strong></li><li><strong>PC</strong></li><li>**Inconnue**</li></ul>  |
| subscriptionName     | chaîne |  Le nom de l’abonnement utilisé pour les données en mode multijoueur. Les valeurs possibles sont **Xbox Game Pass** et **""** (aucun abonnement).  |
| monthlySessionCount     | nombre |  Le nombre de sessions multijoueurs pour le jeu durant le mois spécifié.   |
| engagementDurationMinutes     | nombre |  Le nombre total de minutes pendant lesquelles les clients se sont livrés à des sessions multijoueurs durant le mois spécifié.  |
| monthlyActiveUsers     | nombre | Le nombre total d'utilisateurs multijoueurs actifs pour le jeu durant le mois spécifié.   |
| monthlyActiveDevices     | nombre | Le nombre total d'appareils actifs où se sont déroulées des sessions multijoueurs pour le jeu durant le mois spécifié.   |
| monthlyNewUsers     | nombre |  Le nombre total de nouveaux utilisateurs multijoueurs actifs pour le jeu durant le mois spécifié.  |
| averageDailyActiveUsers     | nombre |  Le nombre moyen d'utilisateurs multijoueurs quotidiens actifs pour le jeu durant le mois spécifié.  |
| averageDailyActiveDevices     | nombre |  Le nombre moyen d'appareils actifs où se sont déroulées des sessions multijoueurs pour le jeu durant le mois spécifié.  |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant montre un exemple de corps de réponse JSON pour la variante de mesures quotidiennes de cette requête (à savoir, lorsque vous spécifiez **multiplayerdaily** pour le paramètre **metricType**).

```json
{
  "Value": [
    {
      "date": "2018-01-07",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 976711,
      "engagementDurationMinutes": 16836064.5,
      "dailyActiveUsers": 180377,
      "dailyActiveDevices": 153359,
      "dailyNewUsers": 8638,
      "monthlyActiveUsers": 779984,
      "monthlyActiveDevices": 606495,
      "monthlyNewUsers": 212093
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 857433,
      "engagementDurationMinutes": 14087724.5,
      "dailyActiveUsers": 166630,
      "dailyActiveDevices": 143065,
      "dailyNewUsers": 9646,
      "monthlyActiveUsers": 764947,
      "monthlyActiveDevices": 595368,
      "monthlyNewUsers": 204248
    },
  ],
  "@nextLink": null,
  "TotalCount":2
}
```

## <a name="related-topics"></a>Rubriquesassociées

* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analytique Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données de réussite Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)
* [Obtenir des données de hub de jeux Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
