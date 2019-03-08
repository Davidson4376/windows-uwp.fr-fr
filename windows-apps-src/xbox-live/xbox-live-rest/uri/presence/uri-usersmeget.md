---
title: GET (/users/me)
assetID: 726c279b-73fb-02ea-cbff-700ff2dc31af
permalink: en-us/docs/xboxlive/rest/uri-usersmeget.html
description: " GET (/users/me)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b06305fde989d0c30570beda5d4b0aabe7bf0518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606724"
---
# <a name="get-usersme"></a>GET (/users/me)
Obtenir de l’utilisateur actuel [PresenceRecord](../../json/json-presencerecord.md) sans avoir à connaître XUID l’utilisateur.
Le domaine pour ces URI est `userpresence.xboxlive.com`.

  * [Paramètres de chaîne de requête](#ID4EZ)
  * [Autorisation](#ID4EIC)
  * [En-têtes de demande nécessaires](#ID4ELD)
  * [En-têtes de demande facultatif](#ID4EPF)
  * [Corps de la demande](#ID4EPG)
  * [Corps de la réponse](#ID4E1G)

<a id="ID4EZ"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Paramètre| Type| Description|
| --- | --- | --- |
| level| chaîne| Facultatif. <ul><li><b>utilisateur</b>: Retourne uniquement le nœud de l’utilisateur.</li><li><b>APPAREIL</b>: Retourne des nœuds de nœud et d’appareils utilisateur.</li><li><b>titre</b>: valeur par défaut. Retourne l’intégralité de l’arborescence, à l’exception d’activité.</li><li><b>tous les</b>: Retourne l’intégralité de l’arborescence, y compris la présence de niveau d’activité.</li></ul> | 

<a id="ID4EIC"></a>


## <a name="authorization"></a>Authorization

| Type| Obligatoire| Description| Cas d’absence de réponse|
| --- | --- | --- | --- | --- | --- | --- |
| XUID| Oui| ID d’utilisateur Xbox (XUID) de l’appelant| 403 Forbidden|

<a id="ID4ELD"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».|
| x-xbl-contract-version| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Exemples de valeurs 3, vnext.|
| Accept| chaîne| Types de contenu qui sont acceptables. Le seul pris en charge par la présence est application/json, mais il doit être spécifié dans l’en-tête.|
| Accept-Language| chaîne| Paramètres régionaux acceptable pour les chaînes dans la réponse. Exemples de valeurs : en-US.|
| Host| chaîne| Nom de domaine du serveur. Exemple de valeur : presencebeta.xboxlive.com.|

<a id="ID4EPF"></a>


## <a name="optional-request-headers"></a>En-têtes de demande facultatif

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.|

<a id="ID4EPG"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4E1G"></a>


## <a name="response-body"></a>Corps de la réponse

<a id="ID4EAH"></a>


### <a name="sample-response"></a>Exemple de réponse

Cette méthode retourne un [PresenceRecord](../../json/json-presencerecord.md).


```cpp
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:W8,
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page",
      }
    }]
  }]
}

```


<a id="ID4EQH"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ESH"></a>


##### <a name="parent"></a>Parent

[/users/me](uri-usersme.md)
