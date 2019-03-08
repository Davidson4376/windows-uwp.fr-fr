---
title: POST (/titles/{titleId}/clusters)
assetID: 0977b0b0-872d-f7ad-9ba0-30d56cff4912
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidclusters-post.html
description: " POST (/titles/{titleId}/clusters)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 91d7c49628914f887c5d2243942e10e47d47b095
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608904"
---
# <a name="post-titlestitleidclusters"></a>POST (/titles/{titleId}/clusters)
URI qui permet à un client créer une instance de serveur Xbox Live Compute. Le domaine pour ces URI est `gameserverms.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EX)
  * [En-têtes de demande nécessaires](#ID4EGB)
  * [Autorisation](#ID4ELD)
  * [Corps de la demande](#ID4EWD)
  * [En-têtes de réponse requis](#ID4EZE)
  * [Corps de la réponse](#ID4E5G)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Description| 
| --- | --- | 
| titleId| ID du titre qui doit traiter la demande.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nom d'hôte

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
Lorsque vous élaborez une demande, les en-têtes affichés dans le tableau suivant sont requis.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | 
| User-Agent|  | Informations sur l’agent de l’utilisateur qui effectue la requête.| 
| Content-Type| application/json| Type de données en cours de soumission.| 
| Host| gameserverms.xboxlive.com|  | 
| Content-Length|  | Longueur de l’objet de demande.| 
| x-xbl-contract-version| 1| Version de contrat d’API.| 
| Authorization| XBL3.0 x = [hash] ; [jeton]| Jeton d’authentification.| 
  
<a id="ID4ELD"></a>

 
## <a name="authorization"></a>Authorization
 
La requête doit inclure un en-tête d’autorisation de Xbox Live valide. Si l’appelant n’est pas autorisé à accéder à cette ressource, le service retourne 403 interdit en réponse. Si l’en-tête est manquant ou non valide, le service retourne 401 non autorisé dans la réponse.
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>Corps de la demande
 
La demande doit contenir un objet JSON avec les membres suivants.
 
| Membre| Description| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| Identificateur de session à partir de la MPSD.| 
| abortIfQueued| Paramètre facultatif, qui, quand la valeur true indique GSMS ne pas à cette session pour une ressource de file d’attente si elle ne peut pas être traitée immédiatement. Si la requête est abandonnée, car cette valeur est true, l’objet de réponse contiendra <code>"fulfillmentState" : "Aborted"</code>. | 
 
<a id="ID4ERE"></a>

 
### <a name="sample-request"></a>Exemple de demande
 

```cpp
{
  "sessionId" : "/serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/session/scott1",
  "abortIfQueued" : "true"
}

      
```

   
<a id="ID4EZE"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
Une réponse inclut toujours les en-têtes affichés dans le tableau suivant.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Cache-Control|  | Directives devant être respectées par tous les mécanismes de mise en cache le long de la chaîne de requête/réponse.| 
| Content-Type| application/json| Type de données dans la réponse.| 
| Content-Length|  | Longueur du corps de réponse.| 
| X-Content-Type-Options|  |  | 
| X-XblCorrelationId|  | Le type mime du corps de réponse.| 
| Date|  |  | 
  
<a id="ID4E5G"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Si l’appel réussit, le service retourne un objet JSON avec les membres suivants.
 
| Membre| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| pollIntervalMilliseconds| Intervalle recommandé, en ms à interroger l’exécution. Notez que cela n’est pas une estimation de lorsque le cluster sera prêt, mais plutôt une recommandation pour la fréquence à laquelle l’appelant doit interroger pour une mise à jour d’état étant donné le pool d’abonnements et les taux de demande et de traitement actuel.| 
| fulfillmentState| Indique si la session fournie a été allouée à une ressource immédiatement, « Remplies, » ajouté à la file d’attente pour la disponibilité d’une ressource futures, « en attente », ou abandonnée, « Abandonnée » en raison de l’incapacité à répondre à la demande immédiatement lors de la demande abortIfQueued spécifié comme « true ». | 
 
<a id="ID4EWH"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
  "pollIntervalMilliseconds" : "1000",
  "fulfillmentState" : "Fulfilled" | "Queued" | "Aborted"
}
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>Notes
 
Un titre doit réessayer uniquement l’appel au service lors de la réception des codes de réponse suivants :
 
   * 408 : délai d’attente du serveur
   * 429 : trop de demandes
   * 500 : erreur de serveur
   * 502-Passerelle incorrecte
   * 503 : Service non disponible
   * 504-expiration de la passerelle
   
<a id="ID4EFBAC"></a>

 
## <a name="see-also"></a>Voir également
 [/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

  