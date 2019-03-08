---
description: Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour obtenir les données d’utilisation simultanée Xbox Live.
title: Obtenir les données d’utilisation simultanée Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, services de Microsoft Store, API d'analyse du Microsoft Store, analyse Xbox Live, utilisation simultanée
ms.localizationpriority: medium
ms.openlocfilehash: 40d35b45065566db22aef791a94faa1cc0fa5c62
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655654"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Obtenir les données d’utilisation simultanée Xbox Live


Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour obtenir des données d’utilisation en temps quasi-réel (avec une latence de 5 à 15 minutes) sur le nombre moyen de clients jouant à votre [jeu Xbox Live](../xbox-live/index.md) toutes les minutes, heures ou chaque jour pendant une période spécifiée. Ces informations sont également disponibles dans le [rapport d’analytique de Xbox](../publish/xbox-analytics-report.md) dans Partner Center.

> [!IMPORTANT]
> Cette méthode prend uniquement en charge les jeux pour Xbox et les jeux qui utilisent les services Xbox Live. Ces jeux doivent passer par le [processus d’approbation de concept](../gaming/concept-approval.md), qui inclut les jeux publiés par des [partenaires Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) et les jeux soumis via le [programme ID@Xbox](../xbox-live/developer-program-overview.md#id). Cette méthode ne prend actuellement pas en charge les jeux publiés via le [Programme Créateurs Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête


| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous voulez récupérer des données d'utilisation simultanée Xbox Live.  |  Oui  |
| metricType | chaîne | Une chaîne qui spécifie le type de données d’analytique Xbox Live à récupérer. Pour cette méthode, spécifiez la valeur **concurrency** .  |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données d'utilisation simultanée. Consultez la description *aggregationLevel* pour le comportement par défaut. |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données d'utilisation simultanée. Consultez la description *aggregationLevel* pour le comportement par défaut. |  Non  |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : **minute**, **hour** ou **day**. Par défaut, la valeur est **day**. <p/><p/>Si vous ne spécifiez pas *startDate* ou *endDate*, le corps de réponse se présentera par défaut comme suit : <ul><li>**minute**: Les 60 derniers enregistrements de données disponibles.</li><li>**heure**: Les 24 derniers enregistrements de données disponibles.</li><li>**jour**: Les 7 derniers enregistrements de données disponibles.</li></ul><p/>Les niveaux d’agrégation suivants comportent des limites de taille au niveau du nombre d’enregistrements qui peut être récupéré. Les enregistrements seront tronqués si l’intervalle de temps demandée est trop vaste. <ul><li>**minute**: Enregistrements de 1440 (24 heures de données).</li><li>**heure**: Enregistrements jusqu'à 720 (30 jours de données).</li><li>**jour**: Enregistrements jusqu'à 60 (60 jours de données).</li></ul>  |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données d’utilisation simultanée pour votre jeu prenant en charge Xbox Live. Cette requête récupère des données chaque minute écoulée entre le 1er et le 2 février 2018. Remplacez la valeur *applicationId* par l’ID Store de votre jeu.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Le corps de la réponse comporte un tableau d’objets contenant chacun un ensemble de données d’utilisation simultanée pour une minute, une heure ou un jour spécifié(e). Chaque objet contient les valeurs suivantes.

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Nombre      | nombre  | Le nombre moyen de clients jouant à votre jeu Xbox Live pour la minute, l'heure ou la journée spécifié(e). <p/><p/>**Remarque**&nbsp;&nbsp;Une valeur 0 indique qu'aucun utilisateur simultané n'était présent pendant l'intervalle spécifiée, ou bien que le système n'est pas parvenu à recueillir les données d'utilisation simultanée pour le jeu durant l'intervalle spécifiée. |
| Date  | chaîne | La date et l’heure qui spécifient l’heure, le minute ou le jour associé(e) aux données d'utilisation simultanée.  |
| SeriesName | chaîne    | Cet élément comporte toujours la valeur **UserConcurrency** . |


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

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analytique Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données de primes Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données de santé Xbox Live](get-xbox-live-health-data.md)
* [Obtenir des données de hub jeux Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données de multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)
