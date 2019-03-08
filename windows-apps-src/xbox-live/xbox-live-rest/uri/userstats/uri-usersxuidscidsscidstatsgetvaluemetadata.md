---
title: GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
assetID: 890e3f93-4fdc-955f-d849-ba9579d5c1eb
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsgetvaluemetadata.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 55ad44d4c29a2d7a43c76c4df2a78e08462fa65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612714"
---
# <a name="get-usersxuidxuidscidsscidstatsincludevaluemetadata"></a>GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
Obtient une liste des statistiques spécifié, notamment les métadonnées associées aux valeurs statistiques, d’un utilisateur dans une configuration de service spécifié.
Le domaine pour ces URI est `userstats.xboxlive.com`.

  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EAB)
  * [Paramètres de chaîne de requête](#ID4ELB)
  * [Autorisation](#ID4EWC)
  * [En-têtes de demande nécessaires](#ID4ERD)
  * [En-têtes de demande facultatif](#ID4EDF)
  * [Corps de la demande](#ID4EHG)
  * [Codes d’état HTTP](#ID4ESG)
  * [Corps de la réponse](#ID4EJCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Notes

Le ? inclure = valuemetadata paramètre de requête autorise la réponse inclure toutes les métadonnées associées aux valeurs statistiques utilisateur, telles que le modèle et la couleur d’une voiture permet d’atteindre une heure sur une piste de course.

Pour inclure des métadonnées de la valeur dans la réponse, l’appel de la demande doit également définir la valeur d’en-tête X-Xbl--Version de contrat à 3.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| xuid| GUID| Xbox utilisateur ID (XUID) de l’utilisateur au nom duquel pour accéder à la configuration de service.|
| scid| GUID| Identificateur de la configuration de service qui contient la ressource sollicitée.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Paramètre| Type| Description|
| --- | --- | --- | --- | --- | --- |
| statNames| chaîne| Liste des noms de statistiques d’utilisateur, séparés par une virgule. Par exemple, l’URI suivant informe le service que quatre statistiques sont demandées pour le compte de l’id d’utilisateur spécifié dans l’URI. { :: nomakrdown}<br/><br/>`https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots?include=valuemetadata`| 
| include=valuemetadata| chaîne| Indique que la réponse inclut des métadonnées de valeur associées aux valeurs stat utilisez.|

<a id="ID4EWC"></a>


## <a name="authorization"></a>Authorization

Il est logique d’autorisation implémentée pour les scénarios d’isolation du contenu et de contrôle d’accès.

   * Statistiques des classements et utilisateur peuvent être lues à partir de clients sur n’importe quelle plateforme, sous réserve que l’appelant envoie un jeton XSTS valide avec la demande. Les écritures sont limités aux clients pris en charge par la plateforme de données.
   * Les développeurs de titre peuvent marquer des statistiques comme ouvertes ou limitées avec XDP ou partenaires. Classements sont ouverts de statistiques. Ouvrir les statistiques sont accessibles par les applications web, ainsi qu’iOS, Android, Windows, Windows Phone et Smartglass, tant que l’utilisateur est autorisé au bac à sable. Autorisation de l’utilisateur à un bac à sable est gérée via XDP ou partenaires.

Pseudo-code pour le contrôle se présente comme suit :


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4ERD"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».|
| X-Xbl-Contract-Version| chaîne| Indique quelle version de l’API à utiliser. Cette valeur doit être définie à « 3 » afin d’inclure des métadonnées de la valeur dans la réponse.|

<a id="ID4EDF"></a>


## <a name="optional-request-headers"></a>En-têtes de demande facultatif

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nom/numéro du service auquel cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.|

<a id="ID4EHG"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4ESG"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| La session a été récupérée.|
| 304| Non modifié| Ressource ne pas été modifié depuis son dernier demandé.|
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.|
| 401| Non autorisé| La demande requiert une authentification utilisateur.|
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.|
| 404| Introuvable| Impossible de trouver la ressource spécifiée.|
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.|
| 408| Expiration du délai de la demande| Version de la ressource n’est pas pris en charge ; doit être rejetée par la couche MVC.|

<a id="ID4EJCAC"></a>


## <a name="response-body"></a>Corps de la réponse

<a id="ID4EPCAC"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
  "user": {
    "xuid": "123456789",
    "gamertag": "WarriorSaint",
    "stats": [
      {
        "statname": "Wins",
        "type": "Integer",
        "value": 40,
        "valuemetadata" : "{\"region\" : \"EU\", \"isRanked\" : true}"
      },
      {
        "statname": "Kills",
        "type": "Integer",
        "value": 700,
        "valuemetadata" : "{\"longestKillStreak" : 15, \"favoriteTarget\" : \"CrazyPigeon\"}"
      },
      {
        "statname": "KDRatio",
        "type": "Double",
        "value": 2.23,
        "valuemetadata" : "{\"totalKills\" : 700, \"totalDeaths\" : 314}"
      },
      {
        "statname": "Headshots",
        "type": "Integer",
        "value": 173,
        "valuemetadata" : ""
      }
    ],
  }
}

```


<a id="ID4EZCAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4E2CAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
