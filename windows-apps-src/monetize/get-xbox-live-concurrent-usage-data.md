---
author: Xansky
description: Utilisez cette méthode dans l'API d'analyse du MicrosoftStore pour obtenir les données d’utilisation simultanée Xbox Live.
title: Obtenir les données d’utilisation simultanée Xbox Live
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, services de MicrosoftStore, API d'analyse du MicrosoftStore, analyse Xbox Live, utilisation simultanée
ms.localizationpriority: medium
ms.openlocfilehash: 3e982d7c5eb1ff8365d2aa527f75d181905784a0
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5813977"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Obtenir les données d’utilisation simultanée Xbox Live


Utilisez cette méthode dans l'API d'analyse du MicrosoftStore pour obtenir des données d’utilisation en temps quasi-réel (avec une latence de 5 à 15minutes) sur le nombre moyen de clients jouant à votre [jeu Xbox Live](../xbox-live/index.md) toutes les minutes, heures ou chaque jour pendant une période spécifiée. Ces informations sont également disponibles dans le [rapport d'analyse Xbox](../publish/xbox-analytics-report.md) du tableau de bord du Centre de développement.

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
| applicationId | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous voulez récupérer des données d'utilisation simultanée Xbox Live.  |  Oui  |
| metricType | chaîne | Une chaîne qui spécifie le type de données d’analytique Xbox Live à récupérer. Pour cette méthode, spécifiez la valeur **concurrency **.  |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données d'utilisation simultanée. Consultez la description *aggregationLevel* pour le comportement par défaut. |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données d'utilisation simultanée. Consultez la description *aggregationLevel* pour le comportement par défaut. |  Non  |
| aggregationLevel | chaîne | Indique la plage de temps pour laquelle vous souhaitez récupérer les données agrégées. Il peut s’agit des chaînes suivantes: **minute**, **hour** ou **day**. Par défaut, la valeur est **day**. <p/><p/>Si vous ne spécifiez pas *startDate* ou *endDate*, le corps de réponse se présentera par défaut comme suit: <ul><li>**minute**: les 60derniers enregistrements de données disponibles.</li><li>**hour**: les 24derniers enregistrements de données disponibles.</li><li>**day**: les 7derniers enregistrements de données disponibles.</li></ul><p/>Les niveaux d’agrégation suivants comportent des limites de taille au niveau du nombre d’enregistrements qui peut être récupéré. Les enregistrements seront tronqués si l’intervalle de temps demandée est trop vaste. <ul><li>**minute**: jusqu'à 1440 enregistrements (24heures de données).</li><li>**hour**: jusqu'à 720 enregistrements (30jours de données).</li><li>**day**: jusqu'à 60 enregistrements (60jours de données).</li></ul>  |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données d’utilisation simultanée pour votre jeu prenant en charge Xbox Live. Cette requête récupère des données chaque minute écoulée entre le 1er et le 2 février 2018. Remplacez la valeur *applicationId* par l’ID Store de votre jeu.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Le corps de la réponse comporte un tableau d’objets contenant chacun un ensemble de données d’utilisation simultanée pour une minute, une heure ou un jour spécifié(e). Chaque objet contient les valeurs suivantes.

| Value      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Nombre      | nombre  | Le nombre moyen de clients jouant à votre jeu Xbox Live pour la minute, l'heure ou la journée spécifié(e). <p/><p/>**Remarque**&nbsp;&nbsp;Une valeur 0 indique qu'aucun utilisateur simultané n'était présent pendant l'intervalle spécifiée, ou bien que le système n'est pas parvenu à recueillir les données d'utilisation simultanée pour le jeu durant l'intervalle spécifiée. |
| Date  | chaîne | La date et l’heure qui spécifient l’heure, le minute ou le jour associé(e) aux données d'utilisation simultanée.  |
| SeriesName | chaîne    | Cet élément comporte toujours la valeur **UserConcurrency **. |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un exemple de corps de réponse JSON pour cette requête avec une agrégation des données par minute.

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>Rubriquesassociées

* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analytique Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données de réussite Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)
* [Obtenir des données de hub de jeux Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)
