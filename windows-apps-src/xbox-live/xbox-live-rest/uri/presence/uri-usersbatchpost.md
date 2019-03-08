---
title: POST (/users/batch)
assetID: bd0b18fe-8a6d-d591-5b13-bcd9643e945a
permalink: en-us/docs/xboxlive/rest/uri-usersbatchpost.html
description: " POST (/users/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47376338a1c515aa00a7e0247df4cc16ee0db8d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655674"
---
# <a name="post-usersbatch"></a>POST (/users/batch)
Obtenir la présence d’un lot d’utilisateurs.
Le domaine pour ces URI est `userpresence.xboxlive.com`.

  * [Remarques](#ID4EV)
  * [Autorisation](#ID4EAB)
  * [Effet des paramètres de confidentialité sur la ressource](#ID4EDC)
  * [En-têtes de demande nécessaires](#ID4EYF)
  * [En-têtes de demande facultatif](#ID4EGAAC)
  * [Corps de la demande](#ID4EGBAC)
  * [Corps de la réponse](#ID4ESEAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Notes

Cette méthode doit être utilisée par n’importe quel client, le service ou le titre qui veulent apprendre des informations de présence pour un lot d’utilisateurs.

Les réponses pour cette demande de lot peuvent être des filtres en profondeur et de chemin d’accès. Consommateurs peuvent l’utiliser pour rechercher et afficher la présence sur un ensemble d’utilisateurs. Les filtres sur cette API fonctionnent comme ORs dans une propriété, mais ANDs sur Propriétés.

<a id="ID4EAB"></a>


## <a name="authorization"></a>Authorization

| Type| Obligatoire| Description| Cas d’absence de réponse|
| --- | --- | --- | --- |
| XUID| Oui| ID d’utilisateur Xbox (XUID) de l’appelant| 403 Forbidden|

<a id="ID4EDC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Effet des paramètres de confidentialité sur la ressource

Cette méthode retourne 200 OK toujours, mais peut ne pas retourne de contenu dans le corps de réponse.

| Utilisateur demande| Cibler le paramètre de confidentialité de l’utilisateur| Comportement|
| --- | --- | --- | --- | --- | --- | --- |
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

<a id="ID4EYF"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».|
| x-xbl-contract-version| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Exemples de valeurs 3, vnext.|
| Accept| chaîne| Types de contenu qui sont acceptables. Le seul pris en charge par la présence est application/json, mais il doit être spécifié dans l’en-tête.|
| Accept-Language| chaîne| Paramètres régionaux acceptable pour les chaînes dans la réponse. Exemples de valeurs : en-US.|
| Host| chaîne| Nom de domaine du serveur. Exemple de valeur : presencebeta.xboxlive.com.|
| Content-Length| chaîne| Longueur du corps de la requête. Exemple de valeur : 312.|

<a id="ID4EGAAC"></a>


## <a name="optional-request-headers"></a>En-têtes de demande facultatif

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.|

<a id="ID4EGBAC"></a>


## <a name="request-body"></a>Corps de la requête

<a id="ID4EMBAC"></a>


### <a name="required-members"></a>Membres requis

| Membre| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| utilisateurs| XUIDs liste d’utilisateurs dont vous souhaitez en savoir plus, avec un maximum de XUIDs 1100 à la fois la présence.|

<a id="ID4EHCAC"></a>


### <a name="optional-members"></a>Membres facultatifs

| Membre| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| deviceTypes| Liste des types de périphérique utilisés par les utilisateurs que vous souhaitez connaître. Si le tableau est vide, par défaut à tous les types possibles de l’appareil (autrement dit, aucun sont filtrées).|
| titres| Liste des appareils dont les utilisateurs que vous voulez savoir sur les types. Si le tableau est vide, par défaut pour tous les titres possibles (autrement dit, aucun sont filtrées).|
| level| Valeurs possibles : <ul><li>utilisateur - get utilisateur nœuds</li><li>APPAREIL - utilisateur de get et les nœuds de l’appareil</li><li>titre - obtenir les informations au niveau de titre de base</li><li>All : obtenir des informations de présence riche, des informations sur le média ou les deux</li></ul><br> La valeur par défaut est « title ».|
| onlineOnly| Si cette propriété est true, l’opération de traitement par lots filtre les enregistrements pour les utilisateurs en mode hors connexion (y compris ceux qui sont masqués). S’il n’est pas fourni, les utilisateurs en ligne et hors connexion seront affichera.|

<a id="ID4E4DAC"></a>


### <a name="prohibited-members"></a>Membres interdits

Tous les autres membres sont interdites dans une demande.

<a id="ID4EIEAC"></a>


### <a name="sample-request"></a>Exemple de demande


```cpp
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}

```


<a id="ID4ESEAC"></a>


## <a name="response-body"></a>Corps de la réponse

<a id="ID4E1EAC"></a>


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


<a id="ID4EKFAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EMFAC"></a>


##### <a name="parent"></a>Parent

[/users/batch](uri-usersbatch.md)
