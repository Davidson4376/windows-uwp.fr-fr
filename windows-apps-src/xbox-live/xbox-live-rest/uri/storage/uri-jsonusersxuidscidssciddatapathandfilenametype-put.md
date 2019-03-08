---
title: PUT (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)
assetID: 02e43120-1f71-a3e7-c84e-96147b838b97
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapathandfilenametype-put.html
description: " PUT (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 35b1f8fdb8150d9f25fee010192e466adb2f2d86
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655664"
---
# <a name="put-jsonusersxuidxuidscidssciddatapathandfilenamejson"></a>PUT (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)
Télécharge un fichier. Téléchargement de bloc multiples n’est pas pris en charge pour les données de type json. Le domaine pour ces URI est `titlestorage.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EX)
  * [Autorisation](#ID4EEB)
  * [Paramètres de chaîne de requête facultative](#ID4ERB)
  * [En-têtes de demande nécessaires](#ID4EXC)
  * [En-têtes de demande facultatif](#ID4EAE)
  * [Corps de la demande](#ID4EDF)
  * [Codes d’état HTTP](#ID4EOF)
  * [Corps de la réponse](#ID4EBDAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI 
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| La Xbox ID de l’utilisateur (XUID) du lecteur qui effectue la demande.| 
| scid| GUID| ID de la configuration de service à rechercher.| 
| pathAndFileName| chaîne| Chemin d’accès et le nom de l’élément accessible. Caractères valides dans la partie chemin d’accès (jusqu'à et y compris la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_) et de la barre oblique (/). La partie chemin d’accès peut être vide. Caractères valides dans la partie du nom de fichier (tout ce qui suit la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_), point (.) et trait d’union (-). Le nom de fichier ne peut pas être vide, se terminer par un point ou contenir deux points consécutifs.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Authorization 
 
La requête doit inclure un en-tête d’autorisation de Xbox LIVE valide. Si l’appelant n’est pas autorisé à accéder à cette ressource, le service renverra une réponse 403 Interdit. Si l’en-tête n’est pas valide ou manquant, le service renvoie une réponse 401 non autorisé. 
  
<a id="ID4ERB"></a>

 
## <a name="optional-query-string-parameters"></a>Paramètres de chaîne de requête facultative 
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| Date/heure du fichier sur le client dernier téléchargement du fichier.| 
| displayName| chaîne| Nom du fichier qui doit être affiché à l’utilisateur.| 
  
<a id="ID4EXC"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Version de contrat d’API.| 
| Authorization| XBL3.0 x = [hash] ; [jeton]| Jeton d’authentification STS. STSTokenString est remplacé par le jeton retourné par la demande d’authentification. Pour plus d’informations sur la récupération d’un jeton STS et la création d’un en-tête d’autorisation, consultez authentification et autorisation de Xbox LIVE demandes relatives aux Services.| 
  
<a id="ID4EAE"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| Spécifie un ETag qui doit correspondre à un élément existant pour terminer l’opération.| 
| If-None-Match| Spécifie un ETag qui ne doit pas correspondre à tous les éléments existant pour terminer l’opération.| 
  
<a id="ID4EDF"></a>

 
## <a name="request-body"></a>Corps de la requête 
 
Le corps de demande contient le contenu complet du fichier en cours de chargement. 
  
<a id="ID4EOF"></a>

 
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
  
<a id="ID4EBDAC"></a>

 
## <a name="response-body"></a>Corps de la réponse 
 
Si le chargement est réussi, un 201 est retourné avec une réponse {}.
  
<a id="ID4EODAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EQDAC"></a>

 
##### <a name="parent"></a>Parent  

[/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersxuidscidssciddatapathandfilenametype.md)

   