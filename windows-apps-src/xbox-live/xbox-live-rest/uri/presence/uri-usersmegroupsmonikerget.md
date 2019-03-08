---
title: GET (/users/me/groups/{moniker} )
assetID: c2527a08-411b-d4e4-422f-a8438804bfe6
permalink: en-us/docs/xboxlive/rest/uri-usersmegroupsmonikerget.html
description: " GET (/users/me/groups/{moniker} )"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 553ff55610ce30461bc6523aaed732aedc1efd18
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595114"
---
# <a name="get-usersmegroupsmoniker-"></a>GET (/users/me/groups/{moniker} )
Obtient le PresenceRecord pour mon groupe. Le domaine pour ces URI est `userpresence.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4E5)
  * [Paramètres de chaîne de requête](#ID4EJB)
  * [Autorisation](#ID4EKC)
  * [Effet des paramètres de confidentialité sur la ressource](#ID4EQD)
  * [En-têtes de demande nécessaires](#ID4EEH)
  * [En-têtes de demande facultatif](#ID4EMBAC)
  * [Corps de la demande](#ID4EMCAC)
  * [Codes d’état HTTP](#ID4EXCAC)
  * [En-têtes de réponse requis](#ID4E3GAC)
  * [En-têtes de réponse facultative](#ID4EMJAC)
  * [Corps de la réponse](#ID4E5KAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Récupère les utilisateurs du groupe spécifié par le moniker relatives à l’utilisateur dans les revendications et retourne le PresenceRecord pour ces utilisateurs. Les données qui sont contrôlées par la confidentialité ou d’Isolation du contenu seront simplement pas renvoyées.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| moniker| chaîne| Chaîne définissant le groupe d’utilisateurs. Le moniker du acceptées uniquement à l’heure actuelle est « People », avec un « P » majuscule.| 
  
<a id="ID4EJB"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| level| chaîne| Retourne le niveau de détail, tel que spécifié par cette chaîne de requête. Options valides sont « user », « appareil », « title » et « all ». Le niveau « utilisateur » est l’objet PresenceRecord sans objet DeviceRecord imbriquée. Le niveau « appareil » est les objets PresenceRecord et DeviceRecord sans objet TitleRecord imbriquée. Le niveau « title » est les objets PresenceRecord, DeviceRecord et TitleRecord sans objet ActivityRecord imbriquée. Le niveau « all » est l’enregistrement complet, y compris tous les objets imbriqués. Si ce paramètre n’est pas fourni, le service par défaut est le niveau de titre (autrement dit, il renvoie la présence de cet utilisateur les détails d’un titre).| 
  
<a id="ID4EKC"></a>

 
## <a name="authorization"></a>Authorization
 
Revendications d’autorisation utilisées | Revendication| Type| Obligatoire ?| Exemple de valeur| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>Xuid</b>| entier signé 64 bits| oui| 1234567890| 
  
<a id="ID4EQD"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Effet des paramètres de confidentialité sur la ressource
 
Le service retourne toujours 200 OK si la demande proprement dite est bien formée. Toutefois, il filtrera les informations à partir de la réponse lorsque les contrôles de confidentialité ne passent pas.
 
Effet des paramètres de confidentialité sur la ressource | Utilisateur demande| Cibler le paramètre de confidentialité de l’utilisateur| Comportement| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| Comme décrit.| 
| Friend| Tout le monde| Comme décrit.| 
| Friend| amis uniquement| Comme décrit.| 
| Friend| bloqué| Comme décrit - le service filtrera les données.| 
| non friend utilisateur| Tout le monde| Comme décrit.| 
| non friend utilisateur| amis uniquement| Comme décrit - le service filtrera les données.| 
| non friend utilisateur| bloqué| Comme décrit - le service filtrera les données.| 
| site tiers| Tout le monde| Comme décrit - le service filtrera les données.| 
| site tiers| amis uniquement| Comme décrit - le service filtrera les données.| 
| site tiers| bloqué| Comme décrit - le service filtrera les données.| 
  
<a id="ID4EEH"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».| 
| x-xbl-contract-version| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Exemples de valeurs 3, vnext.| 
| Accept| chaîne| Types de contenu qui sont acceptables. Le prend en charge qu’une seule de présence est <b>application/json</b>, mais il doit toujours être spécifié dans l’en-tête.| 
| Accept-Language| chaîne| Paramètres régionaux acceptable pour les chaînes dans la réponse. Exemple de valeur : en-US.| 
| Host| chaîne| Nom de domaine du serveur. Exemple de valeur : userpresence.xboxlive.com.| 
  
<a id="ID4EMBAC"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.| 
  
<a id="ID4EMCAC"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EXCAC"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| La session a été récupérée.| 
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 405| Méthode non autorisée| Méthode HTTP a été utilisé sur un type de contenu non pris en charge.| 
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.| 
| 500| Expiration du délai de la demande| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 503| Expiration du délai de la demande| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
  
<a id="ID4E3GAC"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Exemples de valeurs 1, vnext.| 
| Content-Type| chaîne| Le type mime du corps de la demande. Exemple de valeur : <b>application/json</b>.| 
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache. Exemples de valeurs : « no-cache ».| 
| X-XblCorrelationId| chaîne| Valeur générée par le service pour mettre en corrélation de ce que le serveur retourne et ce qui est reçu par le client. Exemple de valeur : « 4106d0bc-1cb3-47bd-83fd-57d041c6febe ».| 
| X-Content-Type-Option| chaîne| Retourne la valeur conforme à SDL. Exemple de valeur : « nosniff ».| 
| Date| chaîne| La date/heure le message a été envoyé. Exemple de valeur : « Mardi 17 novembre 2012 10:33:31 GMT ».| 
  
<a id="ID4EMJAC"></a>

 
## <a name="optional-response-headers"></a>En-têtes de réponse facultative
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Retry-After| chaîne| Retournée sur les erreurs HTTP 503. Permet de savoir combien de temps à attendre avant de renouveler l’appel client. Exemples de valeurs "120".| 
| Content-Length| chaîne| Longueur du corps de réponse. Exemple de valeur : "527".| 
| Content-Encoding| chaîne| Type de codage de la réponse. Exemple de valeur : « gzip ».| 
  
<a id="ID4E5KAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Cette API retourne un tableau d’objets PresenceRecord, un pour chacune des XUIDs à partir de la demande.
 
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
[
     {
         xuid:"0123456789",
         state:"online",
         devices:
         [
             {
                 type:"D",
                 titles:
                 [
                     {
                         id:"12341234",
                         name:"Contoso 5",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement:"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Playing on Nirvana"
                         },
                     }
                 ]
             }
         ]
     },
     {
         xuid:"0123456780",
         state:"online",
         devices:
         [
             {
                 type:"D",
                 titles:
                 [
                     {
                         id:"12341235",
                         name:"Contoso Waypoint",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement;"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Viewing stats"
                         },
                     }
                 ]
             }
         ]
     },
     {
         xuid:"0123456781",
         state:"online",
         devices:
         [
             {
                 type:"Win8",
                 titles:
                 [
                     {
                         id:"23452345",
                         name:"Contoso Gamehelp",
                         state:"active",
                         timestamp:"2012-09-17T07:15:23.4930000",
                         activity:
                         {
                             richPresence:"Viewing help"
                         }
                     }
                 ]
             }
         ]
     }
 ]
         
```

   
<a id="ID4EQLAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ESLAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/groups/{moniker}](uri-usersmegroupsmoniker.md)

   