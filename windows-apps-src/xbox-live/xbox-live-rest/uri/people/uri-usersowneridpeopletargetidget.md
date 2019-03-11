---
title: GET (/users/{ownerId}/people/{targetid})
assetID: 2fd37b8e-b886-14f2-3399-59f530d85e4e
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetidget.html
description: " GET (/users/{ownerId}/people/{targetid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 408b4df30f53e27b04e2a1e654e9686d2b359637
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632674"
---
# <a name="get-usersowneridpeopletargetid"></a>GET (/users/{ownerId}/people/{targetid})
Obtient une personne par ID de la cible de collection de personnes de l’appelant. Le domaine pour ces URI est `social.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4E5)
  * [Autorisation](#ID4EJB)
  * [En-têtes de demande nécessaires](#ID4ERC)
  * [En-têtes de demande facultatif](#ID4EQD)
  * [Corps de la demande](#ID4EWE)
  * [Codes d’état HTTP](#ID4EBF)
  * [En-têtes de réponse requis](#ID4EDH)
  * [Corps de la réponse](#ID4EQAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Opérations GET ne peut pas modifier toutes les ressources donc cela génère les mêmes résultats si exécutée une ou plusieurs fois.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| ownerId| chaîne| Identificateur de l’utilisateur dont la ressource est accédée. Doit correspondre à l’utilisateur authentifié. Les valeurs possibles sont « me », xuid({xuid}) ou gt({gamertag}).| 
| targetid| chaîne| Identificateur de l’utilisateur dont les données sont récupérées à partir de la liste de personnes du propriétaire, un ID d’utilisateur Xbox (XUID) ou une identité. Exemples de valeurs : xuid(2603643534573581), gt(SomeGamertag).| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>Authorization
 
| Type| Obligatoire| Description| Cas d’absence de réponse| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| oui| L’appelant a l’ID d’utilisateur de Xbox (XUID l’utilisateur).| 401 non autorisé| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| Chaîne. Données d’autorisation pour Xbox LIVE. Il s’agit généralement d’un jeton XSTS chiffré. Exemple de valeur : <b>XBL3.0 x =&lt;userhash > ;&lt; jeton ></b>.| 
  
<a id="ID4EQD"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Valeur par défaut : 1.| 
| Accept| Chaîne. Types de contenu qui accepte de l’appelant dans la réponse. Toutes les réponses sont <b>application/json</b>.| 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Réussite.| 
| 400| demande incorrecte| ID d’utilisateur ont été mal formées.| 
| 403| Interdit| Revendication XUID n’a pas pu être analysée à partir de l’en-tête d’autorisation.| 
| 404| Introuvable| Utilisateur cible est introuvable dans la liste des contacts du propriétaire.| 
  
<a id="ID4EDH"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| entier non signé 32 bits| Longueur, en octets, du corps de réponse. Exemple de valeur : 22.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Il s’agit toujours <b>application/json</b>.| 
  
<a id="ID4EQAAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Si l’appel réussit, le service retourne la personne cible. Consultez [personne (JSON)](../../json/json-person.md).
 
<a id="ID4E3AAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
         
```

   
<a id="ID4EGBAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EIBAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/people/{targetid}](uri-usersowneridpeopletargetid.md)

   