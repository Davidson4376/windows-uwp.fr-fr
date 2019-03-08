---
title: POST (/users/xuid({xuid})/feedback)
assetID: 48a7a510-a658-f2a3-c6bc-28a3610006e7
permalink: en-us/docs/xboxlive/rest/uri-reputationusersxuidfeedbackpost.html
description: " POST (/users/xuid({xuid})/feedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d8a6e118811203fd34c310840e8688c2255c6beb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660044"
---
# <a name="post-usersxuidxuidfeedback"></a>POST (/users/xuid({xuid})/feedback)
Utilisée à partir de votre titre si vous souhaitez ajouter une option de commentaires dans votre jeu, par opposition à l’aide de l’interpréteur de commandes. Le domaine pour ces URI est `reputation.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EZ)
  * [En-têtes de demande nécessaires](#ID4EEB)
  * [Corps de la demande](#ID4ENC)
  * [En-têtes obligatoires](#ID4EDE)
  * [Autorisation](#ID4EXF)
  * [Codes d’état HTTP](#ID4EEG)
  * [Corps de la réponse](#ID4EZH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| ulong| Xbox utilisateur ID (XUID) de l’utilisateur signalé.| 
  
<a id="ID4EEB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».| 
| X-RequestedServiceVersion|  | Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 101.| 
  
<a id="ID4ENC"></a>

 
## <a name="request-body"></a>Corps de la requête 
 
<a id="ID4EVC"></a>

 
### <a name="required-members"></a>Membres requis 
 
La demande doit contenir un [commentaires](../../json/json-feedback.md) objet. 
  
<a id="ID4EED"></a>

 
### <a name="prohibited-members"></a>Membres interdits 
 
Tous les autres membres sont interdites dans une demande.
  
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>Exemple de demande 
 

```cpp
{
    "sessionRef":
    {
        "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
        "templateName": "CaptureFlag5",
        "name": "Halo556932",
    },
    "feedbackType": "CommsAbusiveVoice",
    "textReason": "He called me a doodoo-head!",
    "voiceReasonId": null,
    "evidenceId": null
}

      
```

   
<a id="ID4EDE"></a>

 
## <a name="required-headers"></a>En-têtes obligatoires
 
Les en-têtes suivants sont requis lors de l’établissement d’une demande de Services de Xbox Live.
 
| <b>En-tête</b>| <b>Valeur</b>| <b>Deacription</b>| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| Version de contrat d’API.| 
| Authorization| XBL3.0 x = [hash] ; [jeton]| Jeton d’authentification STS. STSTokenString est remplacé par le jeton retourné par la demande d’authentification.| 
Content-Type| 
application/json| 
Type de données en cours de soumission.| 
  
<a id="ID4EXF"></a>

 
## <a name="authorization"></a>Authorization
 
La requête doit inclure un en-tête d’autorisation de Xbox Live valide. Si l’appelant n’est pas autorisé à accéder à cette ressource, le service retourne un code de 403 Interdit. Si l’en-tête est manquant ou non valide, le service retourne un code non autorisé 401.
  
<a id="ID4EEG"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| Aucun contenu| La demande est terminée, mais n’a pas de contenu à retourner.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.| 
| 408| Expiration du délai de la demande| La demande a pris trop de temps.| 
| 409| Conflit| Jeton de continuation n’est plus valide.| 
  
<a id="ID4EZH"></a>

 
## <a name="response-body"></a>Corps de la réponse 
 
Si l’appel ne réussit pas, aucun objet n’est retournées à partir de cette réponse. Sinon, le service retourne un [constatez](../../json/json-serviceerror.md) objet.
  
<a id="ID4EOAAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EQAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

  
<a id="ID4E3AAC"></a>

 
##### <a name="reference"></a>Référence 

[Commentaires (JSON)](../../json/json-feedback.md)

 [Constatez (JSON)](../../json/json-serviceerror.md)

   