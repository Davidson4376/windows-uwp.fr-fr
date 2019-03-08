---
title: DELETE (/users/xuid({xuid})/devices/current/titles/current)
assetID: 3bf75247-0a2a-0e4c-afcc-9e7654a89648
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentdelete.html
description: " DELETE (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dd916fee5455276d45e4437e4ee90cacbde30bf9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604214"
---
# <a name="delete-usersxuidxuiddevicescurrenttitlescurrent"></a>DELETE (/users/xuid({xuid})/devices/current/titles/current)
Supprimer la présence d’un titre de fermeture, au lieu d’attendre le [PresenceRecord](../../json/json-presencerecord.md) le point d’expirer. Le domaine pour ces URI est `userpresence.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EZ)
  * [Autorisation](#ID4EEB)
  * [En-têtes de demande nécessaires](#ID4ERD)
  * [En-têtes de demande facultatif](#ID4EVF)
  * [Corps de la demande](#ID4EVG)
  * [Corps de la réponse](#ID4EAH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur cible.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Authorization
 
| Type| Obligatoire| Description| Cas d’absence de réponse| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| Oui| ID d’utilisateur Xbox (XUID) de l’appelant| 403 Forbidden| 
| titleId| Oui| n ° titre du titre| 403 Forbidden| 
| deviceId| Oui pour tout sauf Windows et Web| deviceId de l’appelant| 403 Forbidden| 
| deviceType| Oui, à l’exception Web| type d’appareil de l’appelant| 403 Forbidden| 
  
<a id="ID4ERD"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».| 
| x-xbl-contract-version| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Exemples de valeurs 3, vnext.| 
| Content-Type| chaîne| Le type mime du corps de la valeur de l’exemple de demande : application/json.| 
| Content-Length| chaîne| Longueur du corps de la requête. Exemple de valeur : 312.| 
| Host| chaîne| Nom de domaine du serveur. Exemple de valeur : presencebeta.xboxlive.com.| 
  
<a id="ID4EVF"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.| 
  
<a id="ID4EVG"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EAH"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
En cas de réussite, code d’état HTTP est retourné sans corps de réponse.
 
En cas d’erreur (HTTP 4xx ou 5xx), les informations d’erreur appropriées sont retournées dans le corps de réponse.
  
<a id="ID4ELH"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ENH"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

   