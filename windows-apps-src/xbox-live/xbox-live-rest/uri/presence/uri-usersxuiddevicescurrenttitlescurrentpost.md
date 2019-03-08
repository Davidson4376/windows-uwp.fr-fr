---
title: POST (/users/xuid({xuid})/devices/current/titles/current)
assetID: d5e2d12d-ba75-2d04-2805-c69a4c143f73
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentpost.html
description: " POST (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b29a0bc796712d7b7c44a1fe8512f99bf09eb4fc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649524"
---
# <a name="post-usersxuidxuiddevicescurrenttitlescurrent"></a>POST (/users/xuid({xuid})/devices/current/titles/current)
Mettre à jour un titre avec la présence d’un utilisateur. Le domaine pour ces URI est `userpresence.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EEB)
  * [Autorisation](#ID4EPB)
  * [En-têtes de demande nécessaires](#ID4ENE)
  * [En-têtes de demande facultatif](#ID4ERG)
  * [Corps de la demande](#ID4ERH)
  * [Corps de la réponse](#ID4EKAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Cet URI peut être utilisé par tous les titres sur les plateformes non-console pour ajouter et mettre à jour de la présence, la présence riche et la présence de données de média pour un titre.
 
**SandboxId** est désormais récupérées à partir de la revendication dans le XToken et appliquée. Si le **SandboxId** n’est pas présent, puis Services de découverte de divertissement (EDS) génère une erreur 400 demande incorrecte.
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur cible.| 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>Authorization
 
| Type| Obligatoire| Description| Cas d’absence de réponse| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| Oui| ID d’utilisateur Xbox (XUID) de l’appelant| 403 Forbidden| 
| titleId| Oui| n ° titre du titre| 403 Forbidden| 
| deviceId| Oui pour tout sauf Windows et Web| deviceId de l’appelant| 403 Forbidden| 
| deviceType| Oui, à l’exception Web| type d’appareil de l’appelant| 403 Forbidden| 
| sandboxId| Oui pour les appels provenant de | bac à sable de l’appelant| 403 Forbidden| 
  
<a id="ID4ENE"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».| 
| x-xbl-contract-version| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Exemples de valeurs 3, vnext.| 
| Content-Type| chaîne| Le type mime du corps de la valeur de l’exemple de demande : application/json.| 
| Content-Length| chaîne| Longueur du corps de la requête. Exemple de valeur : 312.| 
| Host| chaîne| Nom de domaine du serveur. Exemple de valeur : presencebeta.xboxlive.com.| 
  
<a id="ID4ERG"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.| 
  
<a id="ID4ERH"></a>

 
## <a name="request-body"></a>Corps de la requête
 
L’objet de requête est un [TitleRequest](../../json/json-titlerequest.md). Seules les propriétés réellement présentes dans le corps sont mis à jour. Toutes les propriétés que sont ne fait pas partie du corps, mais existe sur le serveur ne sera pas modifié.
 
<a id="ID4EAAAC"></a>

 
### <a name="sample-request"></a>Exemple de demande
 

```cpp
{
  id:"12341234",
  placement:"snapped",
  state:"active"
}
      
```

   
<a id="ID4EKAAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
En cas de succès, un code d’état HTTP 200 ou 201 créé est retourné, le cas échéant.
 
En cas d’erreur (HTTP 4xx ou 5xx), les informations d’erreur approprié sont retournées dans le corps de réponse.
  
<a id="ID4EVAAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EXAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

  
<a id="ID4EBBAC"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[URI de la place de marché](../marketplace/atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   