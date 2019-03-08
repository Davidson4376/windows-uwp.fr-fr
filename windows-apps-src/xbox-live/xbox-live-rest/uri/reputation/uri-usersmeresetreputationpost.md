---
title: POST (/users/me/resetreputation)
assetID: 1a4fed45-f608-dac2-3384-2ee493112f7b
permalink: en-us/docs/xboxlive/rest/uri-usersmeresetreputationpost.html
description: " POST (/users/me/resetreputation)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fc770673ac7e4034004f58032c7fa66cb28413e7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632684"
---
# <a name="post-usersmeresetreputation"></a>POST (/users/me/resetreputation)
Permet à l’équipe de mise en œuvre définir les Scores de réputation de l’utilisateur actuel sur certaines des valeurs arbitraires après un détournement de compte (par exemple). Le domaine pour ces URI est `reputation.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Autorisation](#ID4E5)
  * [En-têtes de demande nécessaires](#ID4ETB)
  * [Corps de la demande](#ID4END)
  * [Codes d’état HTTP](#ID4EDE)
  * [Corps de la réponse](#ID4EFH)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Cette méthode peut également être appelée par les autres partenaires pour tous les bacs à sable, à l’exception de vente au détail et par les utilisateurs dans tous les bacs à sable, à l’exception de la vente au détail, à des fins de test. Notez que cette requête définit l’utilisateur « base » des scores de réputation et son poids est commentaires positifs est tous être remis à zéro. Réputation réelle de l’utilisateur après avoir établi la cet appel sera ces scores de base ainsi que son bonus Ambassadeur plus son bonus esclave.
  
<a id="ID4E5"></a>

 
## <a name="authorization"></a>Authorization
 
À partir de partenaire : Pour le bac à sable de la vente au détail, **PartnerClaim** à partir de l’équipe de mise en œuvre ; pour tous les autres bacs à sable, **PartnerClaim**.
 
À partir de l’utilisateur : Pour tous les bacs à sable, à l’exception de la vente au détail, **XuidClaim** et **TitleClaim**.
  
<a id="ID4ETB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
À partir de toutes les : **Content-Type : application/json**.
 
À partir de partenaire : **Version X-Xbl-contrat** (version actuelle est 101), **X-Xbl-bac à sable**.
 
À partir de l’utilisateur : **Version X-Xbl-contrat** (version actuelle est 101).
 
| En-tête| Type| Description| 
| --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».| 
| X-RequestedServiceVersion|  | Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 101.| 
  
<a id="ID4END"></a>

 
## <a name="request-body"></a>Corps de la requête
 
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>Exemple de demande
 
Le corps de la demande est une simple [ResetReputation (JSON)](../../json/json-resetreputation.md) document.
 

```cpp
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
      
```

   
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | 
| 200| OK| OK.| 
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 500| erreur de serveur interne| Le serveur a rencontré une situation inattendue qui l’a empêché de répondre à la demande.| 
| 503| Service non disponible| La demande a été limitée, la demande, puis réessayez la valeur de nouvelle tentative du client en secondes (par exemple, 5 secondes plus tard).| 
  
<a id="ID4EFH"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
En cas de réussite, le corps de réponse est vide. En cas d’échec, un [constatez (JSON)](../../json/json-serviceerror.md) document est retourné.
 
<a id="ID4ERH"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
         
```

   
<a id="ID4E2H"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E4H"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/resetreputation](uri-usersmeresetreputation.md)

   