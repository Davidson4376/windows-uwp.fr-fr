---
title: DELETE (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 486fac60-6884-2e3f-9ef8-8de5da0ad8af
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipiddelete.html
description: " DELETE (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 391e4d79a389c358dea83509b52782d086201ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622834"
---
# <a name="delete-usersmescidsscidclipsgameclipid"></a>DELETE (/users/me/scids/{scid}/clips/{gameClipId})
Clip de jeu de supprimer les domaines pour ces URI sont `gameclipsmetadata.xboxlive.com` et `gameclipstransfer.xboxlive.com`, selon la fonction de l’URI en question.
 
  * [Remarques](#ID4EX)
  * [Paramètres d’URI](#ID4ECB)
  * [Autorisation](#ID4ENB)
  * [En-têtes de demande nécessaires](#ID4EYB)
  * [En-têtes de demande facultatif](#ID4EEE)
  * [Corps de la demande](#ID4ENF)
  * [Codes d’état HTTP](#ID4EYF)
  * [En-têtes de réponse requis](#ID4EIAAC)
  * [En-têtes de réponse facultative](#ID4E2CAC)
  * [Corps de la réponse](#ID4E2DAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Notes
 
Fournit un mécanisme pour la suppression de la vidéo de l’utilisateur à partir du service GameClips. Lors de la suppression, toutes les métadonnées et les ressources vidéo réelles (générés et d’origine) sont supprimés à partir du système. Il s’agit d’une opération définitive. 

> [!NOTE] 
> L’ID de propriétaire spécifié doit correspondre à l’appelant dans le jeton d’autorisation pour la demande de suppression réussisse. 


  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | 
| scid| chaîne| ID de configuration de service de la ressource est accédée. Doit correspondre à la SCID de l’utilisateur authentifié.| 
| gameClipId| chaîne| ID du clip de jeu de la ressource est accédée.| 
  
<a id="ID4ENB"></a>

 
## <a name="authorization"></a>Authorization
 
Seule la revendication Xuid est requise pour cette méthode.
  
<a id="ID4EYB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemples de valeurs <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.| 
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.| 
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.| 
  
<a id="ID4EEE"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| chaîne| Encodages de compression acceptable. Exemples de valeurs : gzip, deflate, d’identité.| 
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple de valeur : « 686897696a7c876b7e ».| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EYF"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| OK| Suppression réussie de l’élément.| 
| 401| Non autorisé| Il existe un problème avec le format de jeton d’authentification dans la demande.| 
| 403| Interdit| Certains requis revendications manquantes.| 
| 404| Introuvable| L’élément spécifié dans l’URL n’était pas présente (ou il a été supprimé une deuxième fois).| 
| 503| Non Acceptable| Le service ou des dépendances en aval sont arrêtés. Réessayez avec le comportement de temporisation standard.| 
  
<a id="ID4EIAAC"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.| 
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.| 
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.| 
| Retry-After| chaîne| Indique le client d’essayer ultérieurement dans le cas d’un serveur non disponible.| 
| Varier| chaîne| Indique comment mettre en cache des réponses à des proxys en aval.| 
  
<a id="ID4E2CAC"></a>

 
## <a name="optional-response-headers"></a>En-têtes de réponse facultative
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple : « 686897696a7c876b7e ».| 
  
<a id="ID4E2DAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Le service répond avec un code d’état HTTP de 204 (aucun contenu) en cas de réussite. Vous essayez de supprimer le même objet ou un objet inexistant, 404.
 
En cas d’erreurs, un **ServiceErrorResponse** objet sera retourné.
  
<a id="ID4EJEAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ELEAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   