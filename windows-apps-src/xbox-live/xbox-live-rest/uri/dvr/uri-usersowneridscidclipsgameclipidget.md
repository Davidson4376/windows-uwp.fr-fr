---
title: GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
assetID: dbd60c93-9d8e-609b-0ae3-b3f7ee26ba2d
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipidget.html
description: " GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5d2858b11bf8fb902ea07a016c8f41db375013f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640954"
---
# <a name="get-usersowneridscidsscidclipsgameclipid"></a>GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
Obtenir un seul élément de jeu à partir du système si tous les ID pour le localiser sont connues. Les domaines pour ces URI sont `gameclipsmetadata.xboxlive.com` et `gameclipstransfer.xboxlive.com`, selon la fonction de l’URI en question.
 
  * [Remarques](#ID4EX)
  * [Paramètres d’URI](#ID4EVB)
  * [Autorisation](#ID4EAC)
  * [En-têtes de demande nécessaires](#ID4EUH)
  * [En-têtes de demande facultatif](#ID4EBCAC)
  * [Corps de la demande](#ID4ETDAC)
  * [Codes d’état HTTP](#ID4E5DAC)
  * [En-têtes de réponse requis](#ID4EQIAC)
  * [En-têtes de réponse facultative](#ID4EJLAC)
  * [Corps de la réponse](#ID4EJMAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Notes
 
Toutes les données à interroger pour l’élément est disponible dans le **clip de jeu** objets renvoyé à partir de n’importe quelle requête de métadonnées. **XUID**, **ServiceConfigId**, **GameClipId** et **SandboxId** dans les revendications de la requête sont nécessaires pour obtenir un seul élément de jeu via cette API. Si l’élément de jeu a été marqué pour l’application ou Isolation du contenu ou confidentialité vérifie déterminer que l’utilisateur n’est pas autorisé à obtenir le clip de jeu spécifique, l’API retourne un code d’état HTTP de 404 (introuvable).
 
**SandboxId** est désormais récupérées à partir de la revendication dans le XToken et appliquée. Si le **SandboxId** n’est pas présent, puis Services de découverte de divertissement (EDS) génère une erreur 400 demande incorrecte.
 
Cette API doit également être utilisée pour actualiser les URI ayant expiré. Lorsque la requête est terminée, n’importe quel URI ayant expiré pour l’élément de jeu sera actualisé en conséquence. 

> [!NOTE] 
> Une actualisation de l’URI peut prendre jusqu'à 30 à 40 secondes après que cette demande est effectuée. Si l’URI a expiré, essayez d’utiliser immédiatement pour les opérations de diffusion en continu obtiendra un code d’état HTTP 500 à partir des serveurs de diffusion en continu lisse IIS. Nous travaillons en collaboration sur les méthodes de raccourcir ce délai, et cette note sera mis à jour en conséquence en tant que ce travail progresse. 


  
<a id="ID4EVB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | 
| ownerId| chaîne| Identité de l’utilisateur de l’utilisateur dont la ressource est accédée. Formats pris en charge : « me » ou « xuid(123456789) ». Longueur maximum : 16.| 
| scid| chaîne| ID de configuration de service de la ressource est accédée. Doit correspondre à la SCID de l’utilisateur authentifié.| 
| gameClipId| chaîne| ID du clip de jeu de la ressource est accédée.| 
  
<a id="ID4EAC"></a>

 
## <a name="authorization"></a>Authorization
 
Revendications d’autorisation utilisées | Revendication| Type| Obligatoire ?| Exemple de valeur| Notes| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Xuid| entier signé 64 bits| oui| 1234567890|  | 
| TitleId| entier signé 64 bits| oui| 1234567890| Utilisé pour <b>d’Isolation du contenu</b> vérifier.| 
| SandboxId| hexadécimal binaire| oui|  | Force le système à la zone correcte pour les recherches et utilisé pour <b>d’Isolation du contenu</b> vérifier.| 
  
Effet des paramètres de confidentialité sur la ressource | Utilisateur demande| Cibler le paramètre de confidentialité de l’utilisateur| Comportement| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| Comme décrit.| 
| Friend| Tout le monde| Il est interdit.| 
| Friend| amis uniquement| Il est interdit.| 
| Friend| bloqué| Il est interdit.| 
| non friend utilisateur| Tout le monde| Il est interdit.| 
| non friend utilisateur| amis uniquement| Il est interdit.| 
| non friend utilisateur| bloqué| Il est interdit.| 
| site tiers| Tout le monde| Il est interdit.| 
| site tiers| amis uniquement| Il est interdit.| 
| site tiers| bloqué| Il est interdit.| 
 
<a id="ID4EUH"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemples de valeurs <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.| 
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.| 
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.| 
  
<a id="ID4EBCAC"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| chaîne| Encodages de compression acceptable. Exemples de valeurs : gzip, deflate, d’identité.| 
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple de valeur : « 686897696a7c876b7e ».| 
| Plage| chaîne|  | 
  
<a id="ID4ETDAC"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4E5DAC"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| La session a été récupérée.| 
| 301| Déplacé de façon permanente|  | 
| 307| Redirection temporaire|  | 
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.| 
| 408| Expiration du délai de la demande| La demande a pris trop de temps.| 
| 410| Effectué| La ressource demandée n’est plus disponible.| 
  
<a id="ID4EQIAC"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.| 
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.| 
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.| 
| Retry-After| chaîne| Indique le client d’essayer ultérieurement dans le cas d’un serveur non disponible. Exemple : <b>application/json</b>.| 
| Varier| chaîne| Indique comment mettre en cache des réponses à des proxys en aval. Exemple : <b>application/json</b>.| 
  
<a id="ID4EJLAC"></a>

 
## <a name="optional-response-headers"></a>En-têtes de réponse facultative
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple de valeur : « 686897696a7c876b7e ».| 
  
<a id="ID4EJMAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
<a id="ID4EPMAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
 "gameClip": {
   "xuid": "2716903703773872",
   "clipName": "1234567890",
   "titleName": "",
   "gameClipId": "cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
   "state": "Published",
   "dateRecorded": "2013-05-08T21:32:17.4201279Z",
   "lastModified": "2013-05-08T21:34:48.8117829Z",
   "userCaption": "",
   "type": "DeveloperInitiated",
   "source": "Console",
   "visibility": "Public",
   "durationInSeconds": 30,
   "scid": "00000000-0000-0012-0023-000000000070",
   "titleId": 0,
   "rating": 0,
   "ratingCount": 0,
   "views": 0,
   "titleData": "",
   "systemProperties": "",
   "savedByUser": false,
   "thumbnails": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/large",
       "fileSize": 0,
       "thumbnailType": "Large"
     },
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/small",
       "fileSize": 0,
       "thumbnailType": "Small"
     }
   ],
   "gameClipUris": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
       "fileSize": 9332015,
       "uriType": "Download",
       "expiration": "9999-12-31T23:59:59.9999999"
     }
   ]
 }
}
         
```

   
<a id="ID4EZMAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E2MAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

  
<a id="ID4EFNAC"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[URI de la place de marché](../marketplace/atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   