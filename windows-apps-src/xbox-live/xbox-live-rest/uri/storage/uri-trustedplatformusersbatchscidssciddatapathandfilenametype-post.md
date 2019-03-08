---
title: POST (/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})
assetID: 0c89b845-c40f-b28e-f102-d2a96f58dcf9
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersbatchscidssciddatapathandfilenametype-post.html
description: " POST (/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 504a73534b9ee536970caec1b5fd1ea6ce731b31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650544"
---
# <a name="post-trustedplatformusersbatchscidssciddatapathandfilenametype"></a>POST (/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})
Télécharge plusieurs fichiers à partir de plusieurs utilisateurs avec le même nom de fichier. Le domaine pour ces URI est `titlestorage.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EX)
  * [Autorisation](#ID4ECB)
  * [Corps de la demande](#ID4EPB)
  * [Codes d’état HTTP](#ID4E3C)
  * [En-têtes de réponse requis](#ID4EPAAC)
  * [En-têtes de réponse facultative](#ID4ESBAC)
  * [Corps de la réponse](#ID4E3CAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| GUID| ID de la configuration de service à rechercher.| 
| pathAndFileName| chaîne| Chemin d’accès et le nom de l’élément accessible. Caractères valides dans la partie chemin d’accès (jusqu'à et y compris la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_) et de la barre oblique (/). La partie chemin d’accès peut être vide. Caractères valides dans la partie du nom de fichier (tout ce qui suit la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_), point (.) et trait d’union (-). Le nom de fichier ne peut pas être vide, se terminer par un point ou contenir deux points consécutifs.| 
| type| chaîne| Le format des données. Les valeurs possibles sont binaire ou json.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Authorization 
 
La requête doit inclure un en-tête d’autorisation de Xbox LIVE valide. Si l’appelant n’est pas autorisé à accéder à cette ressource, le service renverra une réponse 403 Interdit. Si l’en-tête n’est pas valide ou manquant, le service renvoie une réponse 401 non autorisé. 
  
<a id="ID4EPB"></a>

 
## <a name="request-body"></a>Corps de la requête
 
| Propriété| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| xuids| tableau d’entiers 64 bits non signés| La liste des XUIDs pour lequel télécharger les fichiers.| 
 
<a id="ID4EQC"></a>

 
### <a name="sample-request"></a>Exemple de demande
 

```cpp
{
    "xuids" : 
    [
      12345,
      45678,
      78901
    ]
}
      
```

   
<a id="ID4E3C"></a>

 
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
  
<a id="ID4EPAAC"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Disposition| Décrit le contenu de la partie. Les parties « name » et « filename » de l’en-tête sont le XUID de l’utilisateur auquel ce fichier appartient.| 
| HttpStatusCode| Le code d’état HTTP lié à la récupération de ce fichier particulier.| 
  
<a id="ID4ESBAC"></a>

 
## <a name="optional-response-headers"></a>En-têtes de réponse facultative
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag est un identificateur opaque affecté par un serveur web vers une version spécifique d’une ressource liée à une URL. Si le contenu de la ressource à cette URL change, un ETag nouvel et différent est affecté.| 
| Content-Type| Si le fichier a été récupéré avec succès, cela est le Type de contenu du fichier.| 
| Content-Range| Si le fichier a été récupéré avec succès et est un téléchargement partiel, il s’agit de la plage d’octets du fichier contenu dans la réponse. | 
  
<a id="ID4E3CAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Si l’appel réussit, le service retourne le contenu des fichiers demandés dans une réponse en plusieurs partie.
 
<a id="ID4EGDAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse 
 

```cpp
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=c0a9fd75-d036-4294-8b7b-85fea15a31bb

228
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="123"; filename="123"
HttpStatusCode: 200
ETag: 0x8CF327717411C31
Content-Type: application/octet-stream

asdf123
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="456"; filename="456"
HttpStatusCode: 200
ETag: 0x8CF32771E954BB8
Content-Type: application/octet-stream

asdf456
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="789"; filename="789"
HttpStatusCode: 404


--c0a9fd75-d036-4294-8b7b-85fea15a31bb--

0

```

   
<a id="ID4EUDAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EWDAC"></a>

 
##### <a name="parent"></a>Parent 

[/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersbatchscidssciddatapathandfilenametype.md)

   