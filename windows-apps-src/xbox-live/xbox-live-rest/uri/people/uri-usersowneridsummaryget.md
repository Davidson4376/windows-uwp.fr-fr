---
title: GET (/users/{ownerId}/summary)
assetID: 754190c9-b15d-f34b-1dca-5c92f6f67d12
permalink: en-us/docs/xboxlive/rest/uri-usersowneridsummaryget.html
description: " GET (/users/{ownerId}/summary)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3b228adab7b035ec8f4e65fc8b7458228a677987
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613994"
---
# <a name="get-usersowneridsummary"></a>GET (/users/{ownerId}/summary)
Obtient les données de synthèse sur le propriétaire du point de vue de l’appelant.

  * [Paramètres d’URI](#ID4EQ)
  * [Autorisation](#ID4E2)
  * [En-têtes de demande nécessaires](#ID4EBC)
  * [En-têtes de demande facultatif](#ID4EHD)
  * [Corps de la demande](#ID4EXE)
  * [Codes d’état HTTP](#ID4ECF)
  * [En-têtes de réponse requis](#ID4EZG)
  * [Corps de la réponse](#ID4EGAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| ownerId| chaîne| Identificateur de l’utilisateur dont la ressource est accédée. Les valeurs possibles sont « me », xuid({xuid}) ou gt({gamertag}). Exemples de valeurs : <code>me</code>, <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>|

<a id="ID4E2"></a>


## <a name="authorization"></a>Authorization

| <b>Nom</b>| <b>Type</b>| <b>Description</b>|
| --- | --- | --- | --- | --- | --- |
| xuid| entier non signé 64 bits| Obligatoire. identificateur d’utilisateur de l’appelant. Exemple de valeur : 2533274790395904|

<a id="ID4EBC"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| chaîne| Données d’autorisation. Il s’agit généralement d’un jeton XSTS chiffré. Exemple de valeur : <b>XBL3.0 x = [hash] ; [jeton] </b>.|

<a id="ID4EHD"></a>


## <a name="optional-request-headers"></a>En-têtes de demande facultatif

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-xbl-contract-version| chaîne| Nom/numéro du service auquel cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples de valeurs 1|
| Accept| chaîne| Types de contenu qui sont acceptables. Toutes les réponses seront <code>application/json</code>.|

<a id="ID4EXE"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4ECF"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| La session a été récupérée.|
| 400| demande incorrecte| ID d’utilisateur ont été mal formées.|
| 403| Interdit| Revendication XUID n’a pas pu être analysée à partir de l’en-tête d’autorisation.|

<a id="ID4EZG"></a>


## <a name="required-response-headers"></a>En-têtes de réponse requis

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| chaîne| Le nombre d’octets envoyés dans la réponse. Exemple de valeur : 232.|
| Content-Type| chaîne| Type MIME du corps de la réponse. Il doit s’agir <b>application/json</b>.|

<a id="ID4EGAAC"></a>


## <a name="response-body"></a>Corps de la réponse

Consultez [PersonSummary (JSON)](../../json/json-personsummary.md).

<a id="ID4ESAAC"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}

```


<a id="ID4E3AAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4E5AAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/summary](uri-usersowneridsummary.md)
