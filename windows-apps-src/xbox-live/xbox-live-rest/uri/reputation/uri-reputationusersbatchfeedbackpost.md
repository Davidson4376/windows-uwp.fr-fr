---
title: POST (/users/batchfeedback)
assetID: f94dcf19-a4e3-5bd0-5276-23e43bdcae52
permalink: en-us/docs/xboxlive/rest/uri-reputationusersbatchfeedbackpost.html
description: " POST (/users/batchfeedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0906d32a0e15b2eaaf9c33e7f658e9e9f0cd5124
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622724"
---
# <a name="post-usersbatchfeedback"></a>POST (/users/batchfeedback)
Utilisé par service de votre titre d’envoyer des commentaires sous forme de lot en dehors de l’interface de votre titre. Le domaine pour ces URI est `reputation.xboxlive.com`.
 
  * [Corps de la demande](#ID4EX)
  * [En-têtes obligatoires](#ID4E3E)
  * [Codes d’état HTTP](#ID4EWG)
  * [Corps de la réponse](#ID4EDAAC)
 
<a id="ID4EX"></a>

 
## <a name="request-body"></a>Corps de la requête 
 
Les appelants doivent inclure leur certificat de revendications dans la section ClientCertificates de leur objet de demande web.
 
<a id="ID4EBB"></a>

 
### <a name="required-members"></a>Membres requis 
 
La demande doit contenir un tableau de **BatchFeedback** objets. 
  
<a id="ID4EPB"></a>

 
### <a name="prohibited-members"></a>Membres interdits 
 
Tous les autres membres sont interdites dans une demande.
  
<a id="ID4E3B"></a>

 
### <a name="sample-request"></a>Exemple de demande 
 

```cpp
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Killed 19 team members in a single session",
            "evidenceId": null
        },
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayQuitter",
            "textReason": "Quit 6 times from 9 sessions",
            "evidenceId": null
        }
    ]
}

      
```

 
| <b>Champ</b>| <b>Type JSON</b>| <b>Description</b>| 
| --- | --- | --- | 
| éléments| tableau| Une collection de documents JSON de commentaires.| 
| targetXuid| chaîne| XUID l’utilisateur cible| 
| titleId| chaîne| Le titre de ce commentaire a été envoyé à partir de, ou NULL.| 
| sessionRef| objet| Objet décrivant la session MPSD ces commentaires concerne, ou NULL.| 
| feedbackType| chaîne| Version de chaîne d’une valeur dans l’énumération FeedbackType.| 
| textReason| chaîne| Texte fournies par les partenaires que l’expéditeur peut ajouter pour donner plus de détails sur les commentaires qui a été soumis.| 
| evidenceId| chaîne| L’ID d’une ressource qui peut être utilisée en tant que preuve des commentaires en cours de soumission. par exemple, l’ID d’un fichier vidéo.| 
   
<a id="ID4E3E"></a>

 
## <a name="required-headers"></a>En-têtes obligatoires
 
Les en-têtes suivants sont requis lors de l’établissement d’une demande de Services de Xbox Live. 

> [!NOTE] 
> Un certificat de revendications du partenaire doit être envoyé avec la demande afin d’envoyer des commentaires de traitement par lots. 


 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| Version de contrat d’API.| 
| Content-Type| application/json| Type de données en cours de soumission.| 
| Authorization| « XBL3.0 x =&lt;userhash > ;&lt; jeton > »| Informations d’identification pour l’authentification HTTP.| 
| X-RequestedServiceVersion| 101| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite.| 
  
<a id="ID4EWG"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 500| erreur de serveur interne| Le serveur a rencontré une situation inattendue qui l’a empêché de répondre à la demande.| 
| 503| Service non disponible| La demande a été limitée, la demande, puis réessayez la valeur de nouvelle tentative du client en secondes (par exemple, 5 secondes plus tard).| 
  
<a id="ID4EDAAC"></a>

 
## <a name="response-body"></a>Corps de la réponse 
 
Si l’appel ne réussit pas, aucun objet n’est retournées à partir de cette réponse. Sinon, le service retourne un [constatez](../../json/json-serviceerror.md) objet.
  
<a id="ID4EXAAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EZAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

  
<a id="ID4EFBAC"></a>

 
##### <a name="reference"></a>Référence 

[Commentaires (JSON)](../../json/json-feedback.md)

 [Constatez (JSON)](../../json/json-serviceerror.md)

   