---
title: GET (/sessions/{sessionId}/scids/{scid}/data/{path})
assetID: 007821b8-16f0-2fe1-5196-890743d77775
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapath-get.html
description: " GET (/sessions/{sessionId}/scids/{scid}/data/{path})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 28a279347fa5a463c0d482a624af831c0cdb0fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623714"
---
# <a name="get-sessionssessionidscidssciddatapath"></a>GET (/sessions/{sessionId}/scids/{scid}/data/{path})
Répertorie des informations de fichier dans un chemin d’accès spécifié. Le domaine pour ces URI est `titlestorage.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EX)
  * [Paramètres de chaîne de requête facultative](#ID4ECB)
  * [Autorisation](#ID4EWC)
  * [En-têtes de demande nécessaires](#ID4EDD)
  * [Corps de la demande](#ID4EME)
  * [Codes d’état HTTP](#ID4EZE)
  * [Corps de la réponse](#ID4EUBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| sessionId| chaîne| l’ID de la session à rechercher.| 
| scid| GUID| ID de la configuration de service à rechercher.| 
| path| chaîne| Le chemin d’accès pour les éléments de données à retourner. Tous les répertoires et sous-répertoires correspondant retournées. Caractères valides sont les lettres majuscules (A-Z), des lettres minuscules (a-z), chiffres (0-9), trait de soulignement (_) et par une barre oblique (/). Peut être vide. Longueur maximale de 256.| 
  
<a id="ID4ECB"></a>

 
## <a name="optional-query-string-parameters"></a>Paramètres de chaîne de requête facultative 
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| entier| Retourner les éléments en commençant à N + 1 dans la collection, par exemple, ignorer les éléments de N.| 
| continuationToken| chaîne| Retourner les éléments commençant le jeton de continuation donné. Le paramètre continuationToken est prioritaire sur skipItems si les deux sont fournies. En d’autres termes, le paramètre skipItems est ignoré si le paramètre continuationToken est présent.| 
| maxItems| entier| Nombre maximal d’éléments à retourner à partir de la collection, qui peut être combinée avec skipItems et continuationToken pour retourner une plage d’éléments. Le service peut fournir une valeur par défaut si maxItems n’est pas présent et peut renvoyer moins de maxItems, même si la dernière page de résultats n’a pas encore été retournée. | 
  
<a id="ID4EWC"></a>

 
## <a name="authorization"></a>Authorization 
 
La requête doit inclure un en-tête d’autorisation de Xbox LIVE valide. Si l’appelant n’est pas autorisé à accéder à cette ressource, le service renverra une réponse 403 Interdit. Si l’en-tête n’est pas valide ou manquant, le service renvoie une réponse 401 non autorisé. 
  
<a id="ID4EDD"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Version de contrat d’API.| 
| Authorization| XBL3.0 x = [hash] ; [jeton]| Jeton d’authentification STS. STSTokenString est remplacé par le jeton retourné par la demande d’authentification. Pour plus d’informations sur la récupération d’un jeton STS et la création d’un en-tête d’autorisation, consultez authentification et autorisation de Xbox LIVE demandes relatives aux Services.| 
  
<a id="ID4EME"></a>

 
## <a name="request-body"></a>Corps de la requête 
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EZE"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| La demande a réussi.| 
| 201| Créé le| L’entité a été créée.| 
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.| 
| 408| Expiration du délai de la demande| La demande a pris trop de temps.| 
| 500| erreur de serveur interne| Le serveur a rencontré une situation inattendue qui l’a empêché de répondre à la demande.| 
| 503| Service non disponible| La demande a été limitée, la demande, puis réessayez la valeur de nouvelle tentative du client en secondes (par exemple, 5 secondes plus tard).| 
  
<a id="ID4EUBAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Si l’appel réussit, le service renvoie un tableau de [TitleBlob](../../json/json-titleblob.md) objets. 
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
"blobs":
[
    {
        "fileName":"foo\bar\blob.txt,binary",
        "clientFileTime":"2012-01-01T01:02:03.1234567Z",
        "displayName":"Friendly Name",
        "size":12,
        "etag":"0x8CEB3E4F8F3A5BF"
    },
    {
        "fileName":"foo\bar\blob2.txt,binary",
        "displayName":"Blob 2",
        "size":4,
        "etag":"0x8CEB3FE57F1A142"
    },
    {
        "fileName":"foo\jsonblob.txt,json",
        "size":15,
        "etag":"0x8CEB40152B4A6F8"
    }
],
"pagingInfo":
    {
        "continuationToken":"54",
    }
}
         
```

   
<a id="ID4EOCAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EQCAC"></a>

 
##### <a name="parent"></a>Parent  

[/sessions/{sessionId}/scids/{scid}/data/{path}](uri-sessionssessionidscidssciddatapath.md)

  
<a id="ID4E3CAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>Référence [TitleBlob (JSON)](../../json/json-titleblob.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

   