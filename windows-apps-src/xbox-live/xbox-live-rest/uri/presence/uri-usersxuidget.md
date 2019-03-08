---
title: GET (/users/xuid({xuid}))
assetID: c97ef943-8bea-8a41-90d7-faea874284c8
permalink: en-us/docs/xboxlive/rest/uri-usersxuidget.html
description: " GET (/users/xuid({xuid}))"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5fcc5d3b6a172eccab0656da39e6896b4df50840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650924"
---
# <a name="get-usersxuidxuid"></a>GET (/users/xuid({xuid}))
Détecter la présence d’un autre utilisateur ou un client.
Le domaine pour ces URI est `userpresence.xboxlive.com`.

  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EDB)
  * [Paramètres de chaîne de requête](#ID4EOB)
  * [Autorisation](#ID4E4C)
  * [Effet des paramètres de confidentialité sur la ressource](#ID4EAE)
  * [En-têtes de demande nécessaires](#ID4EVH)
  * [En-têtes de demande facultatif](#ID4E1BAC)
  * [Corps de la demande](#ID4E1CAC)
  * [Corps de la réponse](#ID4EFDAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Notes

La réponse peut être filtrée pour fournir une partie de la [PresenceRecord](../../json/json-presencerecord.md) si le consommateur n’est pas intéressé par l’objet entier.

> [!NOTE] 
> Les données retournées sont limitées par les règles d’isolation de confidentialité et de contenu.



<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur cible.|

<a id="ID4EOB"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Paramètre| Type| Description|
| --- | --- | --- | --- | --- | --- | --- |
| level| chaîne| Facultatif. <ul><li><b>utilisateur</b>: Retourne uniquement le nœud de l’utilisateur.</li><li><b>APPAREIL</b>: Retourne des nœuds de nœud et d’appareils utilisateur.</li><li><b>titre</b>: valeur par défaut. Retourne l’intégralité de l’arborescence, à l’exception d’activité.</li><li><b>tous les</b>: Retourne l’intégralité de l’arborescence, y compris la présence de niveau d’activité.</li></ul> |

<a id="ID4E4C"></a>


## <a name="authorization"></a>Authorization

| Type| Obligatoire| Description| Cas d’absence de réponse|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| Oui| ID d’utilisateur Xbox (XUID) de l’appelant| 403 Forbidden|

<a id="ID4EAE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Effet des paramètres de confidentialité sur la ressource

Cette méthode retourne 200 OK toujours, mais peut ne pas retourne de contenu dans le corps de réponse.

| Utilisateur demande| Cibler le paramètre de confidentialité de l’utilisateur| Comportement|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Me| -| 200 OK|
| Friend| Tout le monde| 200 OK|
| Friend| amis uniquement| 200 OK|
| Friend| bloqué| 200 OK|
| non friend utilisateur| Tout le monde| 200 OK|
| non friend utilisateur| amis uniquement| 200 OK|
| non friend utilisateur| bloqué| 200 OK|
| site tiers| Tout le monde| 200 OK|
| site tiers| amis uniquement| 200 OK|
| site tiers| bloqué| 200 OK|

<a id="ID4EVH"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».|
| x-xbl-contract-version| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Exemples de valeurs 3, vnext.|
| Accept| chaîne| Types de contenu qui sont acceptables. Le seul pris en charge par la présence est application/json, mais il doit être spécifié dans l’en-tête.|
| Accept-Language| chaîne| Paramètres régionaux acceptable pour les chaînes dans la réponse. Exemples de valeurs : en-US.|
| Host| chaîne| Nom de domaine du serveur. Exemple de valeur : presencebeta.xboxlive.com.|

<a id="ID4E1BAC"></a>


## <a name="optional-request-headers"></a>En-têtes de demande facultatif

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.|

<a id="ID4E1CAC"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4EFDAC"></a>


## <a name="response-body"></a>Corps de la réponse

<a id="ID4ELDAC"></a>


### <a name="sample-response"></a>Exemple de réponse

S’il n’existe aucun enregistrement existant pour l’utilisateur, un enregistrement avec aucun appareil n’est retourné.


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


<a id="ID4EXDAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EZDAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})](uri-usersxuid.md)
