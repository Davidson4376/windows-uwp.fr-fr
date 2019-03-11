---
title: POST (/users/{ownerId}/people/xuids)
assetID: e20bfb58-9c3b-14ed-6462-85d42fa6fe1a
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuidspost.html
description: " POST (/users/{ownerId}/people/xuids)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1cb160f3276d215e3aba5dfd671c67fa17d883b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589864"
---
# <a name="post-usersowneridpeoplexuids"></a>POST (/users/{ownerId}/people/xuids)
Obtient les personnes par XUID provenant de personnes de l’appelant de collection. Le domaine pour ces URI est `social.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4E5)
  * [Autorisation](#ID4EJB)
  * [En-têtes de demande nécessaires](#ID4ERC)
  * [En-têtes de demande facultatif](#ID4EBE)
  * [Corps de la demande](#ID4EHF)
  * [Codes d’état HTTP](#ID4EKH)
  * [En-têtes de réponse requis](#ID4ENBAC)
  * [Corps de la réponse](#ID4EZCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
VALIDER les opérations ne peut pas modifier toutes les ressources donc cela génère les mêmes résultats si exécutée une ou plusieurs fois.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| ownerId| chaîne| Identificateur de l’utilisateur dont la ressource est accédée. Doit correspondre à l’utilisateur authentifié. Les valeurs possibles sont « me », xuid({xuid}) ou gt({gamertag}).| 
  
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
| Content-Length| entier non signé 32 bits. Longueur, en octets, du corps de la demande. Exemple de valeur : 22.| 
| Content-Type| Chaîne. Type MIME du corps de la demande. Il doit s’agir <b>application/json</b>.| 
  
<a id="ID4EBE"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Valeur par défaut : 1.| 
| Accept| Chaîne. Types de contenu qui accepte de l’appelant dans la réponse. Toutes les réponses sont <b>application/json</b>.| 
  
<a id="ID4EHF"></a>

 
## <a name="request-body"></a>Corps de la requête
 
<a id="ID4ENF"></a>

 
### <a name="required-members"></a>Membres requis
 
| Membre| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| XuidList| Tableau de XUIDs qui identifient les personnes à retourner à partir de la collection de personnes de l’appelant. Consultez [XuidList (JSON)](../../json/json-xuidlist.md).| 
  
<a id="ID4EKG"></a>

 
### <a name="optional-members"></a>Membres facultatifs
 
Il n’existe aucun membre facultatif pour cette demande.
  
<a id="ID4EVG"></a>

 
### <a name="prohibited-members"></a>Membres interdits
 
Tous les autres membres sont interdites dans une demande.
  
<a id="ID4EAH"></a>

 
### <a name="sample-request"></a>Exemple de demande
 

```cpp
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
      
```

   
<a id="ID4EKH"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Réussite lors de la méthode est « get ».| 
| 204| Aucun contenu| Réussite lors de la méthode est « ajouter » ou « supprimer ».| 
| 400| demande incorrecte| Paramètre de méthode est manquant ou incorrect, ou ID d’utilisateur ont été mal formées.| 
| 403| Interdit| Revendication XUID n’a pas pu être analysée à partir de l’en-tête d’autorisation.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| entier non signé 32 bits| Longueur, en octets, du corps de réponse. Exemple de valeur : 22.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Il s’agit toujours <b>application/json</b>.| 
  
<a id="ID4EZCAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Un corps de réponse est envoyé uniquement lorsque la méthode de demande est « get ». Il n’existe aucun corps de réponse pour « ajouter » ou « supprimer ».
 
Si un appel de méthode « get » réussit, le service retourne le nombre total de personnes dans les personnes de l’appelant collection et un tableau qui contient la collection de personnes de l’appelant. Aucune réponse n’est retournée pour les méthodes « ajouter » et « supprimer ». Consultez [PeopleList (JSON)](../../json/json-peoplelist.md).
 
<a id="ID4EHDAC"></a>

 
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
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
         
```

   
<a id="ID4ERDAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ETDAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/people/xuids](uri-usersowneridpeoplexuids.md)

   