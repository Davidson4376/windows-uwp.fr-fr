---
title: MultiplayerSessionRequest (JSON)
assetID: 2e311c6d-3427-5a39-1989-06dc08483057
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionrequest.html
description: " MultiplayerSessionRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 18929c060adeae47f0305422dd312e7410f93981
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628344"
---
# <a name="multiplayersessionrequest-json"></a>MultiplayerSessionRequest (JSON)
L’objet JSON de demande transmise pour une opération sur un **MultiplayerSession** objet. 
<a id="ID4EQ"></a>

  
 
L’objet JSON de MultiplayerSessionRequest a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| constants| objet| Paramètres en lecture seule qui sont fusionnés avec le modèle de session pour produire l’une des constantes pour la session. | 
| propriétés | objet | Modifications à fusionner dans les propriétés de session.| 
| members.me | objet| Constantes et des propriétés qui fonctionnent comme leurs équivalents de niveau supérieur. N’importe quelle méthode PUT, l’utilisateur doit être un membre de la session et ajoute l’utilisateur si nécessaire. Si « me » est spécifié comme null, le membre qui effectue la requête est supprimé de la session. | 
| membres | objet| Autres objets qui représentent les utilisateurs à ajouter à la session, indexé par un index de base zéro. Le nombre de membres dans une demande commence toujours par 0, même si la session déjà contient des membres. Les membres sont ajoutés à la session dans l’ordre dans lequel ils apparaissent dans la demande. Propriétés de membre peuvent uniquement être définies par l’utilisateur auquel ils appartiennent. | 
| serveurs | objet| Valeurs indiquant les mises à jour et des ajouts à la session du jeu de participants de serveur associé. Si un serveur est spécifié comme null, cette entrée de serveur est supprimée de la session. | 
  
<a id="ID4EZ"></a>

 
## <a name="request-structure"></a>Structure de la demande
 

```json
{
  "constants": { /* Property Bag */ },
  "properties": { /* Property Bag */ },
  "members": {
    // Requires a service principal. Existing members can be deleted by index.
    // Not available on large sessions.
    "5": null,

    // Reservation requests must start with zero. New users will get added in order to the end of the session's member list.
    // Large sessions don't support reservations.
    "reserve_0": {
      "constants": { /* Property Bag */ }
    },
    "reserve_1": {
      "constants": { /* Property Bag */ }
    },

    // Requires a user principal with a xuid claim. Can be 'null' to remove myself from the session.
    "me": {
      "constants": { /* Property Bag */ },
      "properties": { /* Property Bag */ },
    }
  },

  // Requires a server principal.
  "servers": {
    // Can be any name. The value can be 'null' to remove the server from the session.
    "name": {
      "constants": {  /* Property Bag */ },
      "properties": {  /* Property Bag */ }
    }
  }
}
```

  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EMB"></a>

 
##### <a name="reference"></a>Référence 

[MultiplayerSession (JSON)](json-multiplayersession.md)

 [PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](../uri/sessiondirectory/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

   