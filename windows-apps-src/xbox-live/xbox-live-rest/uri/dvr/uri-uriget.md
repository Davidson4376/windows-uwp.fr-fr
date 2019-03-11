---
title: GET (/{uri})
assetID: a67a3288-88f9-c504-5fa8-8fd06055d079
permalink: en-us/docs/xboxlive/rest/uri-uriget.html
description: " GET (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 757d84c9ad5a005e042b42d699ada08504dc57ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650614"
---
# <a name="get-uri"></a>GET (/{uri})
Télécharger un clip de jeu. Les domaines pour ces URI sont `gameclipsmetadata.xboxlive.com` et `gameclipstransfer.xboxlive.com`, selon la fonction de l’URI en question.
 
  * [Remarques](#ID4EX)
  * [Paramètres d’URI](#ID4EDB)
  * [En-têtes de demande nécessaires](#ID4EEC)
  * [En-têtes de demande facultatif](#ID4EQE)
  * [Corps de la demande](#ID4EZF)
  * [En-têtes de réponse requis](#ID4EEG)
  * [Codes d’état HTTP](#ID4EYAAC)
  * [En-têtes de réponse facultative](#ID4EOFAC)
  * [Corps de la réponse](#ID4EOGAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Notes
 
Le client peut télécharger n’importe quel élément ou la miniature qui a atteint l’état publié et est du type téléchargeable, tel que spécifié dans un **GameClipUri** objet. L’URI de demande le fichier est inclus dans le corps de réponse lors de la récupération d’une liste d’utilisateur ou des clips publiques.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| <b>uri</b>| chaîne| Le <b>uri</b> champ au sein de la <b>GameClipUri</b> objet.| 
  
<a id="ID4EEC"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemples de valeurs <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.| 
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.| 
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.| 
  
<a id="ID4EQE"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| chaîne| Encodages de compression acceptable. Exemples de valeurs : gzip, deflate, d’identité.| 
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple de valeur : « 686897696a7c876b7e ».| 
  
<a id="ID4EZF"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EEG"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.| 
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.| 
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.| 
| Retry-After| chaîne| Indique le client d’essayer ultérieurement dans le cas d’un serveur non disponible.| 
| Varier| chaîne| Indique comment mettre en cache des réponses à des proxys en aval.| 
  
<a id="ID4EYAAC"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| La session a été récupérée.| 
| 301| Déplacé de façon permanente| Le service a été déplacé vers un autre URI.| 
| 307| Redirection temporaire| Le service a été déplacé vers un autre URI.| 
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.| 
| 408| Expiration du délai de la demande| La demande a pris trop de temps.| 
| 410| Effectué| La ressource demandée n’est plus disponible.| 
  
<a id="ID4EOFAC"></a>

 
## <a name="optional-response-headers"></a>En-têtes de réponse facultative
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple : « 686897696a7c876b7e ».| 
  
<a id="ID4EOGAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
<a id="ID4EUGAC"></a>

  
 
Si l’opération réussit, le serveur retourne le clip vidéo, peut-être tronqué en fonction de l’en-tête de demande de plage. Pour un clip tronqué, la réponse sera contenu partiel (206). Si le serveur retourne l’intégralité du fichier, il répond OK (200). En cas d’erreur, un **GameClipsServiceErrorResponse** objet peut être retourné avec un code d’état HTTP approprié (par exemple, 416, plage demandée non satisfaisante).
   
<a id="ID4E4GAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E6GAC"></a>

 
##### <a name="parent"></a>Parent 

[/{uri}](uri-uri.md)

   