---
title: POST (/users/me/scids/{scid}/clips)
assetID: 44535926-9fb8-5498-b1c8-514c0763e6c9
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipspost.html
description: " POST (/users/me/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7a8973390ccbf5dd9980410f60f03a7edd78c134
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608784"
---
# <a name="post-usersmescidsscidclips"></a>POST (/users/me/scids/{scid}/clips)
Effectuer une demande de chargement initial. Les domaines pour ces URI sont `gameclipsmetadata.xboxlive.com` et `gameclipstransfer.xboxlive.com`, selon la fonction de l’URI en question.
 
  * [Remarques](#ID4EX)
  * [Paramètres d’URI](#ID4EFB)
  * [Autorisation](#ID4EQB)
  * [En-têtes de demande nécessaires](#ID4EKC)
  * [En-têtes de demande facultatif](#ID4ENE)
  * [Corps de la demande](#ID4ENF)
  * [Exemple de demande](#ID4E1F)
  * [Codes d’état HTTP](#ID4EDG)
  * [Corps de la réponse](#ID4EVAAC)
  * [Exemple de réponse](#ID4EFBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Notes
 
Il s’agit de la première partie du processus de chargement de clip de jeu. Lors de la capture d’une vidéo, il est recommandé d’appeler le service de GameClips immédiatement, afin d’obtenir l’ID et l’URI pour le téléchargement des bits, même si le téléchargement n’est pas programmé pour démarrer immédiatement. Cet appel effectuera des vérifications de quota d’utilisateur et des autres via l’isolation de contenu, confidentialité, et ainsi de suite pour voir si une vidéo doit encore être planifiée pour le chargement par le client. Une réponse positive à partir de cet appel indique que le service est prêt à accepter le clip vidéo pour le chargement. Tous les clips téléchargés doivent être associés un logiciel spécifique (via un SCID) pour être acceptées dans le système.
 
Cet appel n’est pas idempotente ; les appels suivants entraîne différents ID et URI doit être émis. Nouvelles tentatives en cas d’échec doivent suivre côté client temporisation comportement standard.
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| chaîne| ID de configuration de service de la ressource est accédée. Doit correspondre à la SCID de l’utilisateur authentifié.| 
  
<a id="ID4EQB"></a>

 
## <a name="authorization"></a>Authorization
 
Les revendications suivantes sont requises pour cette méthode :
 
   * Xuid
   * DeviceType - doit être l’appareil pour télécharger
   * DeviceId
   * TitleId
   * TitleSandboxId
   
<a id="ID4EKC"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemples de valeurs <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.| 
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.| 
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.| 
  
<a id="ID4ENE"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| chaîne| Encodages de compression acceptable. Exemples de valeurs : gzip, deflate, d’identité.| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Le corps de la demande doit être un [InitialUploadRequest](../../json/json-initialuploadrequest.md) objet au format JSON.
  
<a id="ID4E1F"></a>

 
## <a name="sample-request"></a>Exemple de demande
 

```cpp
{
   "clipNameStringId": "1193045",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
      
```

  
<a id="ID4EDG"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| La session a été récupérée.| 
| 400| demande incorrecte| Une erreur s’est produite dans le corps de la demande, ou l’utilisateur dépasse son quota.| 
| 401| Non autorisé| Il existe un problème avec le format de jeton d’authentification dans la demande.| 
| 403| Interdit| Certains requis des revendications sont manquantes, ou type de périphérique n’est pas.| 
| 503| Non Acceptable| Le service ou des dépendances en aval sont arrêtés. Réessayez avec le comportement de temporisation standard.| 
  
<a id="ID4EVAAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
La réponse peut être un [InitialUploadResponse](../../json/json-initialuploadresponse.md) objet ou un [ServiceErrorResponse](../../json/json-serviceerrorresponse.md) objet au format JSON.
  
<a id="ID4EFBAC"></a>

 
## <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
   "clipName": "ClipName",
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752",  
   "titleName": "TitleName",
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
         
```

  
<a id="ID4EOBAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EQBAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

   