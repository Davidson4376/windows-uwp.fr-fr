---
author: Xansky
description: Utilisez cette méthode dans l'API d'analyse du MicrosoftStore pour obtenir les données de réussite Xbox Live.
title: Obtenir des données de réussite Xbox Live
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, services de MicrosoftStore, API d'analyse du MicrosoftStore, analyse Xbox Live, réussites
ms.localizationpriority: medium
ms.openlocfilehash: 6b635a659a8516184998b5f0b05d2d7692a42af1
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6662601"
---
# <a name="get-xbox-live-achievements-data"></a>Obtenir des données de réussite Xbox Live

Utilisez cette méthode dans l'API d'analyse du MicrosoftStore pour connaître le nombre de clients ayant déverrouillé chaque succès pour votre [jeu Xbox Live](../xbox-live/index.md) au cours de la dernière journée pour laquelle des données de succès sont disponibles, pour les 30jours précédant cette journée et pour la durée de vie totale de votre jeu jusqu'à ce jour. Ces informations sont également disponibles dans le [rapport d’analytique Xbox](../publish/xbox-analytics-report.md) dans l’espace partenaires.

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
| applicationId | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous voulez récupérer des données de succès Xbox Live.  |  Oui  |
| metricType | chaîne | Une chaîne qui spécifie le type de données d’analytique Xbox Live à récupérer. Pour cette méthode, spécifiez la valeur **achievements **.  |  Oui  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention des données de succès pour les clients ayant déverrouillé les 10premiers succès pour votre jeu Xbox Live. Remplacez la valeur *applicationId* par l’ID Store de votre jeu.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

| Value      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | array  | Un tableau d’objets contenant des données pour chaque succès de votre jeu. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant.                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 100 mais que la requête présente plus de 100rangées de données. |
| TotalCount | entier    | Nombre total de lignes des résultats de données pour la requête.  |


Les éléments du tableau *Value* comportent les valeurs suivantes:

| Valeur               | Type   | Description                           |
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

## <a name="related-topics"></a>Rubriquesassociées

* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analytique Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)
* [Obtenir des données de hub de jeux Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)
