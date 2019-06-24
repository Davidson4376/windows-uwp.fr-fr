---
description: Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour obtenir des données de club Xbox Live.
title: Obtenir des données de club Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, services de Microsoft Store, API d'analyse du Microsoft Store, analyse Xbox Live, clubs
ms.localizationpriority: medium
ms.openlocfilehash: e5fc116c2b868ddf093aabea09d59934301f49ec
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321867"
---
# <a name="get-xbox-live-club-data"></a>Obtenir des données de club Xbox Live

Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour obtenir les données de club pour votre  [jeu compatible Xbox Live](https://docs.microsoft.com/gaming/xbox-live/index.md). Ces informations sont également disponibles dans le [rapport d’analytique de Xbox](../publish/xbox-analytics-report.md) dans Partner Center.

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
| applicationId | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous voulez récupérer des données de club Xbox Live.  |  Oui  |
| metricType | chaîne | Une chaîne qui spécifie le type de données d’analytique Xbox Live à récupérer. Pour cette méthode, spécifiez la valeur **communitymanagerclub**.  |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données de club. La valeur par défaut est de 30 jours avant la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, la date de début de la récupération des données de club. La valeur par défaut est la date du jour. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données de club pour votre jeu prenant en charge Xbox Live. Remplacez la valeur *applicationId* par l’ID Store de votre jeu.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

| Value      | type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Un tableau qui contient un objet [ProductData](#productdata) avec les données de clubs associées à votre jeu et un objet [XboxwideData](#xboxwidedata) qui contient les données de club pour tous les clients Xbox Live. Ces données permettent d'établir des comparaisons avec les données de votre jeu.  |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10000 mais que la requête présente plus de 10000 rangées de données. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête. |


### <a name="productdata"></a>ProductData

Cette ressource contient des données de club pour votre jeu.

| Value           | type    | Description        |
|-----------------|---------|------|
| date            |  chaîne |   La date pour les données de club.   |
|  applicationId               |    chaîne     |  L'[ID Store](in-app-purchases-and-trials.md#store-ids) du jeu dont vous souhaitez récupérer des données de club.   |
|  clubsWithTitleActivity               |    entier     |  Le nombre de clubs qui interagissant via les réseaux sociaux avec votre jeu.   |     
|  clubsExclusiveToGame               |   entier      |  Le nombre de clubs qui interagissant via les réseaux sociaux exclusivement avec votre jeu.   |     
|  clubFacts               |   array      |   Contient un ou plusieurs objets [ClubFacts](#clubfacts) sur chacun des clubs interagissant via les réseaux sociaux avec votre jeu.   |


### <a name="xboxwidedata"></a>XboxwideData

Cette ressource contient des données de club moyennes sur tous les clients Xbox Live.

| Value           | type    | Description        |
|-----------------|---------|------|
| date            |  chaîne |   La date pour les données de club.   |
|  applicationId  |    chaîne     |   Dans l'objet **XboxwideData**, cette chaîne est toujours la valeur **XBOXWIDE**.  |
|  clubsWithTitleActivity               |   entier     |  En moyenne, le nombre de clubs avec lesquels les clients interagissent via les réseaux sociaux avec un jeu Xbox Live.    |     
|  clubsExclusiveToGame               |   entier      |  En moyenne, le nombre de clubs avec lesquels les clients interagissent via les réseaux sociaux exclusivement avec un jeu Xbox Live.   |     
|  clubFacts               |   objet      |  Contient un objet [ClubFacts](#clubfacts). Cet objet n’a aucune signification dans le contexte de l'objet **XboxwideData** et comporte des valeurs par défaut.  |


### <a name="clubfacts"></a>ClubFacts

Dans l'objet **ProductData**, cet objet contient des données relatives à un club spécifique, dont les activités sont liées à votre jeu. Cet objet n’a aucune signification dans le contexte de l'objet **XboxwideData** et comporte des valeurs par défaut.

| Value           | type    | Description        |
|-----------------|---------|--------------------|
|  name            |  chaîne  |   Dans l'objet **ProductData**, il s'agit du nom de la club. Dans l'objet **XboxwideData**, cet élément comporte toujours la valeur **XBOXWIDE**.           |
|  memberCount               |    entier     | Dans l'objet **ProductData**, il s’agit du nombre de membres dans le club ; les utilisateurs qui visitent simplement le club sans en être membre ne sont pas comptabilisés. Dans l'objet **XboxwideData**, la valeur est toujours 0.    |
|  titleSocialActionsCount               |    entier     |  Dans l'objet **ProductData**, il s’agit du nombre d’actions sociales liées à votre jeu qui ont été effectuées par les membres dans le club. Dans l'objet **XboxwideData**, la valeur est toujours 0   |
|  isExclusiveToGame               |    Valeur booléenne     |  Dans l'objet **ProductData**, cela indique si le club actuel interagit via les réseaux sociaux exclusivement avec votre jeu. Dans l'objet **XboxwideData**, la valeur est toujours true.  |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "ProductData": [
        {
          "date": "2018-01-30",
          "applicationId": "9NBLGGGZ5QDR",
          "clubsWithTitleActivity": 3,
          "clubsExclusiveToGame": 1,
          "clubFacts": [
            {
              "name": "Club Contoso Racing",
              "memberCount": 1,
              "titleSocialActionsCount": 1,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Sports",
              "memberCount": 12,
              "titleSocialActionsCount": 9,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Maze",
              "memberCount": 4,
              "titleSocialActionsCount": 2,
              "isExclusiveToGame": false
            }
          ]
        }
      ],
      "XboxwideData": [
        {
          "date": "2018-01-30",
          "applicationId": "XBOXWIDE",
          "clubsWithTitleActivity": 25,
          "clubsExclusiveToGame": 3,
          "clubFacts": [
            {
              "name": "XBOXWIDE",
              "memberCount": 0,
              "titleSocialActionsCount": 0,
              "isExclusiveToGame": true
            }
          ]
        }
      ]
    }
  ],
  "@nextLink": "gameanalytics?applicationId=9NBLGGGZ5QDR&metricType=communitymanagerclub&top=1&skip=1&startDate=2018/01/04&endDate=2018/02/02&orderby=date desc",
  "TotalCount": 27
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analytique Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données de primes Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données de santé Xbox Live](get-xbox-live-health-data.md)
* [Obtenir des données de hub jeux Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)
