---
description: Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour obtenir les données de réussite Xbox Live.
title: Obtenir des données de réussite Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, services de Microsoft Store, API d'analyse du Microsoft Store, analyse Xbox Live, réussites
ms.localizationpriority: medium
ms.openlocfilehash: 422024445be4662aab0a47b5527369c8b7091446
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317770"
---
# <a name="get-xbox-live-achievements-data"></a>Obtenir des données de réussite Xbox Live

Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour connaître le nombre de clients ayant déverrouillé chaque succès pour votre [jeu Xbox Live](https://docs.microsoft.com/gaming/xbox-live/index.md) au cours de la dernière journée pour laquelle des données de succès sont disponibles, pour les 30 jours précédant cette journée et pour la durée de vie totale de votre jeu jusqu'à ce jour. Ces informations sont également disponibles dans le [rapport d’analytique de Xbox](../publish/xbox-analytics-report.md) dans Partner Center.

> [!IMPORTANT]
> Cette méthode prend uniquement en charge les jeux pour Xbox et les jeux qui utilisent les services Xbox Live. Ces jeux doivent passer par le [processus d’approbation de concept](../gaming/concept-approval.md), qui inclut les jeux publiés par des [partenaires Microsoft](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#microsoft-partners) et les jeux soumis via le [programme ID@Xbox](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#id). Cette méthode ne prend actuellement pas en charge les jeux publiés via le [Programme Créateurs Xbox Live](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Demande


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>En-tête de requête

| Header        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête


| Paramètre        | type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous voulez récupérer des données de succès Xbox Live.  |  Oui  |
| metricType | chaîne | Une chaîne qui spécifie le type de données d’analytique Xbox Live à récupérer. Pour cette méthode, spécifiez la valeur **achievements** .  |  Oui  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention des données de succès pour les clients ayant déverrouillé les 10 premiers succès pour votre jeu Xbox Live. Remplacez la valeur *applicationId* par l’ID Store de votre jeu.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

| Value      | type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Un tableau d’objets contenant des données pour chaque succès de votre jeu. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant.                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 100 mais que la requête présente plus de 100 rangées de données. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête.  |


Les éléments du tableau *Value* comportent les valeurs suivantes :

| Value               | type   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | chaîne | L’ID Store du jeu pour laquelle vous récupérez les données de succès.     |
| reportDateTime     | chaîne |  La date pour les données de succès.    |
| achievementId          | nombre |  ID du succès. |
| achievementName           | chaîne | Le nom du succès.  |
| gamerscore           | nombre |  La récompense gamerscores pour le succès.  |
| dailyUnlocks           | nombre |  Le nombre de clients ayant déverrouillé le succès lors de la journée spécifiée par la valeur *reportDateTime*.  |
| monthlyUnlocks              | nombre |  Le nombre de clients ayant déverrouillé le succès au cours des 30 jours précédant la journée spécifiée par la valeur *reportDateTime*.   |
| totalUnlocks | nombre |  Le nombre de clients ayant déverrouillé le succès au cours de la durée de vie de votre jeu, jusqu'au jour spécifié par la valeur *reportDateTime*.   |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 6,
      "achievementName": "Yoink!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10310,
      "totalUnlocks": 1215360
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 7,
      "achievementName": "Ding!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10897,
      "totalUnlocks": 1282524
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 8,
      "achievementName": "End of the Beginning",
      "gamerscore": 30,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 9848,
      "totalUnlocks": 1105074
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analytique Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données de santé Xbox Live](get-xbox-live-health-data.md)
* [Obtenir des données de Hub de jeu Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données de multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)
