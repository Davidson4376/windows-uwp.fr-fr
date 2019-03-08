---
title: GET (/global/scids/{scid})
assetID: 7c8cd028-e94a-45e1-fe34-c9deae2d6042
permalink: en-us/docs/xboxlive/rest/uri-globalscidsscid-get.html
description: " GET (/global/scids/{scid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a91e13c33c02a34f844773e7a54ac55bf8e98338
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645974"
---
# <a name="get-globalscidsscid"></a>GET (/global/scids/{scid})
Récupère les informations de quota pour ce type de stockage. Le domaine pour ces URI est `titlestorage.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EX)
  * [Autorisation](#ID4ECB)
  * [En-têtes de demande nécessaires](#ID4ENB)
  * [Corps de la demande](#ID4EWC)
  * [Codes d’état HTTP](#ID4EBD)
  * [Corps de la réponse](#ID4EUAAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| GUID| ID de la configuration de service à rechercher.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Authorization
 
La requête doit inclure un en-tête d’autorisation de Xbox LIVE valide. Si l’appelant n’est pas autorisé à accéder à cette ressource, le service renverra une réponse 403 Interdit. Si l’en-tête n’est pas valide ou manquant, le service renvoie une réponse 401 non autorisé. 
  
<a id="ID4ENB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Version de contrat d’API.| 
| Authorization| XBL3.0 x = [hash] ; [jeton]| Jeton d’authentification STS. STSTokenString est remplacé par le jeton retourné par la demande d’authentification. Pour plus d’informations sur la récupération d’un jeton STS et la création d’un en-tête d’autorisation, consultez authentification et autorisation de Xbox LIVE demandes relatives aux Services.| 
  
<a id="ID4EWC"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EBD"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP 
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK | La demande a réussi.| 
| 201| Créé le | L’entité a été créée.| 
| 400| demande incorrecte | Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé | La demande requiert une authentification utilisateur.| 
| 403| Interdit | La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable | Impossible de trouver la ressource spécifiée.| 
| 406| Non Acceptable | Version de la ressource n’est pas pris en charge.| 
| 408| Expiration du délai de la demande | La demande a pris trop de temps.| 
| 500| erreur de serveur interne | Le serveur a rencontré une situation inattendue qui l’a empêché de répondre à la demande.| 
| 503| Service non disponible | La demande a été limitée, la demande, puis réessayez la valeur de nouvelle tentative du client en secondes (par exemple, 5 secondes plus tard).| 
  
<a id="ID4EUAAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Si l’appel réussit, le service renverra un [quotaInfo](../../json/json-quota.md) objet. 
 
<a id="ID4ECBAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
  "quotaInfo":
  {
    "usedBytes":619,
    "quotaBytes":16777216
  }
}
         
```

   
<a id="ID4EOBAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EQBAC"></a>

 
##### <a name="parent"></a>Parent 

[/ global/scids / {scid}](uri-globalscidsscid.md)

  
<a id="ID4E1BAC"></a>

 
##### <a name="reference"></a>Référence 

[quotaInfo (JSON)](../../json/json-quota.md)

   