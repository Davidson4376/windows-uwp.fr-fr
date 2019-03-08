---
title: GET (/global/scids/{scid}/data/{pathAndFileName},{type})
assetID: 5ca8e0dd-3c45-1b7b-022e-d5d61414fd7d
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapathandfilenametype-get.html
description: " GET (/global/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c7d83d6ee4b0b3787fe86c3ff21e64e6524d2a25
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628004"
---
# <a name="get-globalscidssciddatapathandfilenametype"></a>GET (/global/scids/{scid}/data/{pathAndFileName},{type})
Télécharge un fichier. Le domaine pour ces URI est `titlestorage.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EX)
  * [Autorisation](#ID4ECB)
  * [Paramètres de chaîne de requête facultative](#ID4EPB)
  * [En-têtes de demande nécessaires](#ID4EZC)
  * [En-têtes de demande facultatif](#ID4ECE)
  * [Corps de la demande](#ID4EMF)
  * [Codes d’état HTTP](#ID4EZF)
  * [En-têtes de réponse](#ID4EMDAC)
  * [Corps de la réponse](#ID4EPEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| GUID| ID de la configuration de service à rechercher.| 
| pathAndFileName| chaîne| Chemin d’accès et le nom de l’élément accessible. Caractères valides dans la partie chemin d’accès (jusqu'à et y compris la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_) et de la barre oblique (/). La partie chemin d’accès peut être vide. Caractères valides dans la partie du nom de fichier (tout ce qui suit la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_), point (.) et trait d’union (-). Le nom de fichier ne peut pas être vide, se terminer par un point ou contenir deux points consécutifs.| 
| type| chaîne| Le format des données. Les valeurs possibles sont : binaire, config ou json.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Authorization 
 
La requête doit inclure un en-tête d’autorisation de Xbox LIVE valide. Si l’appelant n’est pas autorisé à accéder à cette ressource, le service renverra une réponse 403 Interdit. Si l’en-tête n’est pas valide ou manquant, le service renvoie une réponse 401 non autorisé. 
  
<a id="ID4EPB"></a>

 
## <a name="optional-query-string-parameters"></a>Paramètres de chaîne de requête facultative 
 
Varie en fonction du type d’objet blob. Objets BLOB binaire ne gèrent pas les paramètres de requête.
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| sans perte de données| chaîne| Utilisable uniquement lorsque le type est json. Spécifie que la réponse doit contenir uniquement une certaine valeur/propriété de l’objet JSON, comme déterminé par ce paramètre. Utilisez un « point » (.) pour spécifier des sous-propriétés et des crochets ('[' et ']') pour spécifier les index de tableau. Par exemple, « tableau1 .prop2 [4] » Spécifie la propriété « prop2 » de l’index 4 du tableau « tableau1 ».| 
| customSelector| chaîne| Pour les fichiers de type de configuration. Indique quels nœuds virtuels personnalisés à inclure. Pour plus d’informations sur les fichiers de type de configuration, consultez stockage de titre.| 
  
<a id="ID4EZC"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Version de contrat d’API.| 
| Authorization| XBL3.0 x = [hash] ; [jeton]| Jeton d’authentification STS. STSTokenString est remplacé par le jeton retourné par la demande d’authentification. Pour plus d’informations sur la récupération d’un jeton STS et la création d’un en-tête d’autorisation, consultez authentification et autorisation de Xbox LIVE demandes relatives aux Services.| 
  
<a id="ID4ECE"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| Spécifie un ETag qui doit correspondre à un élément existant pour terminer l’opération.| 
| If-None-Match| Spécifie un ETag qui ne doit pas correspondre à tous les éléments existant pour terminer l’opération.| 
| Plage| Spécifie la plage d’octets à télécharger. Suit le format d’en-tête de plage HTTP standard.| 
  
<a id="ID4EMF"></a>

 
## <a name="request-body"></a>Corps de la requête 
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EZF"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP 
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EMDAC"></a>

 
## <a name="response-headers"></a>En-têtes de réponse
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag est un identificateur opaque affecté par un serveur web vers une version spécifique d’une ressource liée à une URL. Si le contenu de la ressource à cette URL change, un ETag nouvel et différent est affecté.| 
| Content-Range| S’il s’agissait d’un téléchargement partiel, cet en-tête spécifie la plage d’octets téléchargés.| 
  
<a id="ID4EPEAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
Si l’appel réussit, le service retourne le contenu du fichier.  
<a id="ID4EYEAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E1EAC"></a>

 
##### <a name="parent"></a>Parent  

[/global/scids/{scid}/data/{pathAndFileName},{type}](uri-globalscidssciddatapathandfilenametype.md)

  
<a id="ID4EGFAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>Référence [TitleBlob (JSON)](../../json/json-titleblob.md)

   