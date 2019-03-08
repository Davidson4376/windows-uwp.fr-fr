---
title: GameMessage (JSON)
assetID: c11606e6-701f-5807-4aef-5608c98ad831
permalink: en-us/docs/xboxlive/rest/json-gamemessage.html
description: " GameMessage (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a2bddd9e26b4716fd1e33c4b5bbde56672b5d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651294"
---
# <a name="gamemessage-json"></a>GameMessage (JSON)
Un objet JSON de définition des données d’un message dans la file d’attente des messages d’une session de jeu. 
<a id="ID4EN"></a>

  
 
L’objet JSON de GameMessage a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| données| tableau d’entiers non signés 8 bits| Les données codées en Base64 que le jeu client souhaite envoyer aux autres clients de jeu. Cette valeur est opaque pour le serveur. | 
| senderXuid| entier non signé 64 bits| L’ID d’utilisateur Xbox du lecteur envoi du message. | 
| sequenceNumber| entier signé 32 bits| Le numéro de séquence du message de jeu. Cette valeur est assignée par le serveur. Numéros de séquence sont garantis être monotone, mais ne peut ne pas être consécutives. Les numéros de séquence sont uniques au sein d’une file d’attente, mais pas entre les files d’attente. | 
| queueIndex| entier signé 32 bits| Index de la file d’attente de session pour le message. Les valeurs possibles sont 0-3.| 
| timeStamp| DateTime| Heure de création dans la file d’attente le message jeu par le serveur, au format UTC. | 
  
<a id="ID4ERC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "queueIndex": 0,
    "sequenceNumber": 5,
    "senderXuid": 65536,
    "timestamp": "2011-06-23T18:49:50Z",
    "data": null
}
    
```

  
<a id="ID4E1C"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E3C"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EGD"></a>

 
##### <a name="reference"></a>Référence 

[GameSession (JSON)](json-gamesession.md)

   