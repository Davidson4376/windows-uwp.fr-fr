---
title: GET (/users/{ownerId}/people)
assetID: c948d031-ec19-7571-31ef-23cb9c5ebfaf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopleget.html
description: " GET (/users/{ownerId}/people)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6c8672188a93b2e8d27a081ae068387e7ee7aa42
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623764"
---
# <a name="get-usersowneridpeople"></a>GET (/users/{ownerId}/people)
Obtient les personnes de l’appelant de collection.
Le domaine pour ces URI est `social.xboxlive.com`.

  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4E5)
  * [Paramètres de chaîne de requête](#ID4EJB)
  * [Autorisation](#ID4ERD)
  * [En-têtes de demande nécessaires](#ID4EZE)
  * [En-têtes de demande facultatif](#ID4EYF)
  * [Corps de la demande](#ID4E5G)
  * [Codes d’état HTTP](#ID4EJH)
  * [En-têtes de réponse requis](#ID4EBBAC)
  * [Corps de la réponse](#ID4ENCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Notes

Opérations GET ne peut pas modifier toutes les ressources donc cela génère les mêmes résultats si exécutée une ou plusieurs fois.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| ownerId| chaîne| Identificateur de l’utilisateur dont la ressource est accédée. Doit correspondre à l’utilisateur authentifié. Les valeurs possibles sont « me », xuid({xuid}) ou gt({gamertag}).|

<a id="ID4EJB"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Paramètre| Type| Description|
| --- | --- | --- | --- | --- | --- |
| affichage| chaîne| Retourner les personnes associées à une vue. La valeur par défaut est « all ». Les valeurs possibles sont : <ul><li><b>Tous les</b> -retourne toutes les personnes sur la liste des contacts de l’utilisateur. Il s'agit de la valeur par défaut.</li><li><b>Favoris</b> -retourne toutes les personnes sur la liste des contacts de l’utilisateur qui ont l’attribut favori.</li><li><b>LegacyXboxLiveFriends</b> -retourne toutes les personnes sur la liste des contacts de l’utilisateur qui sont également hérités amis Xbox LIVE.</li></br>**Remarque :**  Uniquement les **tous les** valeur est prise en charge si l’utilisateur appelant est différent de celui de l’utilisateur propriétaire.|
| startIndex| entier non signé 32 bits| Retourner les éléments en commençant à l’index donné.  
| maxItems| entier non signé 32 bits| Nombre maximal de personnes à retourner à partir de la collection à partir de l’index de début. Le service peut fournir une valeur par défaut si <b>maxItems</b> n’est pas présent et peut retourner moins de <b>maxItems</b> (même si la dernière page de résultats n’a pas encore été retournée).|

<a id="ID4ERD"></a>


## <a name="authorization"></a>Authorization

| Type| Obligatoire| Description| Cas d’absence de réponse|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| oui| L’appelant a l’ID d’utilisateur de Xbox (XUID l’utilisateur).| 401 non autorisé|

<a id="ID4EZE"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| Chaîne. Données d’autorisation pour Xbox LIVE. Il s’agit généralement d’un jeton XSTS chiffré. Exemple de valeur : <b>XBL3.0 x =&lt;userhash > ;&lt; jeton ></b>.|

<a id="ID4EYF"></a>


## <a name="optional-request-headers"></a>En-têtes de demande facultatif

| En-tête| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Valeur par défaut : 1.|
| Accept| Chaîne. Types de contenu qui accepte de l’appelant dans la réponse. Toutes les réponses sont <b>application/json</b>.|

<a id="ID4E5G"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4EJH"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Réussite.|
| 400| demande incorrecte| Paramètres de requête ou d’ID utilisateur ont été mal formées.|
| 403| Interdit| Revendication XUID n’a pas pu être analysée à partir de l’en-tête d’autorisation.|

<a id="ID4EBBAC"></a>


## <a name="required-response-headers"></a>En-têtes de réponse requis

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| entier non signé 32 bits| Longueur, en octets, du corps de réponse. Exemple de valeur : 22.|
| Content-Type| chaîne| Type MIME du corps de la réponse. Il s’agit toujours <b>application/json</b>.|

<a id="ID4ENCAC"></a>


## <a name="response-body"></a>Corps de la réponse

Si l’appel réussit, le service retourne le nombre total de personnes dans la collection de personnes de l’appelant et un tableau qui contient la collection de personnes de l’appelant. Consultez [PeopleList (JSON)](../../json/json-peoplelist.md).

<a id="ID4EZCAC"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFollowingCaller": false,
            "isFavorite": false
        },
    ],
    "totalCount": 3
}

```


<a id="ID4EDDAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EFDAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people](uri-usersowneridpeople.md)
