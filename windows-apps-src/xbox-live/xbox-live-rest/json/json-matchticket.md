---
title: MatchTicket (JSON)
assetID: 12617677-47f2-e517-af53-5ab9687eea2a
permalink: en-us/docs/xboxlive/rest/json-matchticket.html
description: " MatchTicket (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4bc638dfe7735856295ed92f35e244213be7bc1e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608334"
---
# <a name="matchticket-json"></a>MatchTicket (JSON)
Un objet JSON représentant un ticket de correspondance, permet de localiser d’autres joueurs via l’annuaire de sessions multijoueur (MPSD) par les lecteurs. 
<a id="ID4EN"></a>

  
 
L’objet JSON de MatchTicket a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| serviceConfig| GUID| Identificateur de configuration service (SCID) pour la session.| 
| hopperName| chaîne| Nom de la hopper dans lequel ce ticket doit être placé.| 
| giveUpDuration| entier signé 32 bits| Délai d’attente maximal (nombre entier de secondes).| 
| preserveSession| énumération| Une valeur qui indique si la session doit être réutilisée en tant que la session dans laquelle faire correspondre. Les valeurs possibles sont « toujours » ou « jamais ». | 
| ticketSessionRef| MultiplayerSessionReference| <b>MultiplayerSessionReference</b> objet pour la session dans laquelle le lecteur ou le groupe est en cours de lecture. Ce membre est toujours requis. | 
| ticketAttributes| tableau d’objets| Collection d’attributs fourni par l’utilisateur et les valeurs sur les tickets pour les joueurs.| 
| joueurs| tableau d’objets| Collection d’objets de lecteur, chacun avec un jeu de propriétés d’attributs fourni par l’utilisateur. | 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
        "serviceConfig": "07617C5B-3423-4505-B6C6-10A16E1E5DDB",
        "hopperName": "TestHopper",
        "giveUpDuration": 10,
        "preserveSession": "never",
        "ticketSessionRef": {
        "scid": "AFFEABDF-0000-0000-0000-000000000001",
        "templateName": "TestTemplate",
        "sessionName": "5E551041-0000-0000-0000-000000000001"
    },
    "ticketAttributes": {
        "desiredMap": "Test Map",
        "desiredGameType": "Test GameType"
    },
    "players": [
        {
            "xuid": 123412345123,
            "playerAttributes": {
                "skill": 5,
                "ageRange": "teenager"
            }
        },
        {
          "xuid": 123412345124,
          "playerAttributes": {
              "skill": 15,
              "ageRange": "twenty-something"
          }
        }
      ]
    }
  
    
```

  
<a id="ID4EEB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EGB"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   