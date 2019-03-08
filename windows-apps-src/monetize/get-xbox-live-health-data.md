---
description: Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour obtenir les données d'intégrité Xbox Live.
title: Obtenir des données d'intégrité Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, services de Microsoft Store, API d'analyse du Microsoft Store, analyse Xbox Live, intégrité, erreurs client
ms.localizationpriority: medium
ms.openlocfilehash: 3b996d85776cb49d45cc5b699709b4eb107e7086
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650904"
---
# <a name="get-xbox-live-health-data"></a>Obtenir des données d'intégrité Xbox Live


Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour obtenir les données d'intégrité pour votre  [jeu compatible Xbox Live](../xbox-live/index.md). Ces informations sont également disponibles dans le [rapport d’analytique de Xbox](../publish/xbox-analytics-report.md) dans Partner Center.

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
| applicationId | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous voulez récupérer des données d'intégrité Xbox Live.  |  Oui  |
| metricType | chaîne | Une chaîne qui spécifie le type de données d’analytique Xbox Live à récupérer. Pour cette méthode, spécifiez la valeur **callingpattern**.  |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données d'intégrité. La valeur par défaut est de 30 jours avant la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, la date de début de la récupération des données d'intégrité. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul> | Non   |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul><p/>Si vous spécifiez un ou plusieurs champs *groupby*, tous les autres champs *groupby* non spécifiés indiqueront la valeur **All** dans le corps de réponse. |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données d'intégrité pour votre jeu prenant en charge Xbox Live. Remplacez la valeur *applicationId* par l’ID Store de votre jeu.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=callingpattern&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Un tableau d’objets comportant des données d'intégrité. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant.                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10000 mais que la requête présente plus de 10000 rangées de données. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de la requête.   |


Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | chaîne | L’ID Store du jeu pour lequel vous récupérez les données d'intégrité.     |
| date                | chaîne | Première date dans la plage de dates des données d’intégrité. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| deviceType          | chaîne | Une des chaînes suivantes spécifiant le type d’appareil sur lequel votre jeu a été utilisé :<p/><ul><li><strong>XboxOne</strong></li><li><strong>WindowsOneCore</strong> (cette valeur indique un PC)</li><li><strong>Inconnu</strong></li></ul>  |
| sandboxId     | chaîne |   L’ID de bac à sable créé pour le jeu. Cet élément peut comporter la valeur RETAIL ou l'ID de bac à sable privé.   |
| packageVersion     | chaîne |  La version du package en quatre parties du jeu.  |
| callingPattern     | objet |  Un objet [callingPattern](#callingpattern) qui fournit des données sur les réponses du service, les périphériques et les utilisateurs pour chaque code d’état renvoyé par chaque service Xbox Live utilisé par votre jeu pour la plage de dates spécifiée.     |


### <a name="callingpattern"></a>callingPattern

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| service      | chaîne  |   Le nom du service Xbox Live associé aux données d’intégrité.       |
| endpoint      | chaîne  |   Le point de terminaison du service Xbox Live associé aux données d’intégrité.        |
| httpStatusCode      | chaîne  |  Le code d'état HTTP associé à cet ensemble de données d'intégrité.<p/><p/>**Remarque**&nbsp;&nbsp;le code d’état **429E** indique que l’appel au service a réussi uniquement en raison de l'exemption de la [limitation de vitesse de granularité fine](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md) lors de l’appel. La limitation de vitesse de granularité fine peut être appliquée à l’avenir si le service rencontre un volume élevé, et dans ce cas l’appel entraînerait un [code d’état HTTP 429](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md#http-429-response-object).         |
| serviceResponses      | nombre  | Le nombre de réponses de service ayant renvoyé le code d’état spécifié.         |
| uniqueDevices      | nombre  |  Le nombre de périphériques uniques ayant appelé le service et reçu le code d’état spécifié.       |
| uniqueUsers      | nombre  |   Le nombre d’utilisateurs uniques ayant reçu le code d’état spécifié.       |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête. Les noms de service et les points de terminaison illustrés dans cet exemple ne sont pas réels et sont utilisés à titre d’exemple uniquement.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "date": "2018-01-30",
      "deviceType": "All",
      "sandboxId": "RETAIL",
      "packageVersion": "Unknown",
      "callingpattern": [
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchReadHandler.POST",
          "httpStatusCode": "200",
          "serviceResponses": 160891,
          "uniqueDevices": 410,
          "uniqueUsers": 410
        },
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchStatReadHandler.GET",
          "httpStatusCode": "200",
          "serviceResponses": 422,
          "uniqueDevices": 0,
          "uniqueUsers": 30
        }
      ],
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analytique Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données de primes Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données de Hub de jeu Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données de multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)
