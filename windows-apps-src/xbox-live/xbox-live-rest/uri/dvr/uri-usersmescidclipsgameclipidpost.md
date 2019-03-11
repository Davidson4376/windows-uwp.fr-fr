---
title: POST (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 410aecad-57f9-c3dc-f35f-19c4d8dfb704
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipidpost.html
description: " POST (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89f3b53631f5570ab6d0d0619f6678fc3e3c2dd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645824"
---
# <a name="post-usersmescidsscidclipsgameclipid"></a>POST (/users/me/scids/{scid}/clips/{gameClipId})
Mettre à jour les métadonnées d’élément de jeu pour les données de l’utilisateur. Les domaines pour ces URI sont `gameclipsmetadata.xboxlive.com` et `gameclipstransfer.xboxlive.com`, selon la fonction de l’URI en question.
 
  * [Remarques](#ID4EX)
  * [Paramètres d’URI](#ID4EAB)
  * [En-têtes de demande nécessaires](#ID4ELB)
  * [En-têtes de demande facultatif](#ID4EXD)
  * [Corps de la demande](#ID4EAF)
  * [En-têtes de réponse requis](#ID4EVF)
  * [En-têtes de réponse facultative](#ID4EJAAC)
  * [Corps de la réponse](#ID4EJBAC)
  * [URI connexes](#ID4EWBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Notes
 
L’API de mise à jour des métadonnées d’élément de jeu se divise en deux catégories, la mise à jour les métadonnées du jeu clips telles que l’accessibilité et de titre et la mise à jour des attributs publics (comme appliquant une évaluation ou incrémentant le nombre de vue) de n’importe quel autre élément de jeu. Si le XUID dans l’URI ne correspond pas à la XUID dans la revendication, uniquement les données publiques peuvent être modifiées et toute demande à modifier toutes les autres données sera refusée. Essaient de plusieurs champs dans le cas être modifié et un d’eux n’est pas valide pour la demande, la requête entière échoue.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| chaîne| ID de configuration de service de la ressource est accédée. Doit correspondre à la SCID de l’utilisateur authentifié.| 
| gameClipId| chaîne| ID du clip de jeu de la ressource est accédée.| 
  
<a id="ID4ELB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemples de valeurs <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.| 
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.| 
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.| 
  
<a id="ID4EXD"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| chaîne| Encodages de compression acceptable. Exemples de valeurs : gzip, deflate, d’identité.| 
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple de valeur : « 686897696a7c876b7e ».| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Le corps de la demande doit être un [UpdateMetadataRequest](../../json/json-updatemetadatarequest.md) objet au format JSON. Exemples :
 
Modification de nom de l’élément et la visibilité :
 

```cpp
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
Permet de modifier uniquement les propriétés titre (il s’agit juste d’un exemple, étant donné que le schéma de ce champ est à l’appelant) :
 

```cpp
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EVF"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.| 
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.| 
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.| 
| Retry-After| chaîne| Indique le client d’essayer ultérieurement dans le cas d’un serveur non disponible.| 
| Varier| chaîne| Indique comment mettre en cache des réponses à des proxys en aval.| 
  
<a id="ID4EJAAC"></a>

 
## <a name="optional-response-headers"></a>En-têtes de réponse facultative
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple : « 686897696a7c876b7e ».| 
  
<a id="ID4EJBAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Lors de la mise à jour réussie des métadonnées, un code d’état HTTP 200 est retournée.
 
Sinon, un objet ServiceErrorResponse au format JSON sera retourné avec un code d’état HTTP approprié.
  
<a id="ID4EWBAC"></a>

 
## <a name="related-uris"></a>URI connexes
 
Les URI suivants mettre à jour les champs publics dans les métadonnées. Il n’est pas obligatoire pour ces demandes de corps. Lors de la mise à jour réussie des métadonnées, un code d’état HTTP 200 est retournée. Sinon, un objet ServiceErrorResponse au format JSON sera retourné avec un code d’état HTTP approprié.
 
   * **POST /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} /ratings/ {valeur d’évaluation}** -applique le contrôle d’accès spécifié à l’élément spécifié. Évaluation de valeur doit être un entier compris entre 1 et 5.
   * **VALIDEZ /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} / indicateur** -indicateurs de l’élément pour qu’il contienne du contenu douteuses à vérifier par l’application.
   * **POST /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} / vues** -incrémente le nombre d’affichage pour l’élément de jeu spécifié. Il est recommandé qu’on parle pas droit au démarrage de la lecture, mais lorsque 75 à 80 % de la lecture a été effectuée.
   
<a id="ID4EMCAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EOCAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   