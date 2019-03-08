---
title: Player (JSON)
assetID: eaf6d082-869b-d2d3-d548-5cef65e54541
permalink: en-us/docs/xboxlive/rest/json-player.html
description: " Player (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5967cbfecd47c5675926bd45939442c45dda7b6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622494"
---
# <a name="player-json"></a>Player (JSON)
Contient les données d’un lecteur dans une session de jeu. 
<a id="ID4EN"></a>

 
## <a name="player"></a>Lecteur
 
L’objet Player a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| customData| tableau d’entiers non signés 8 bits| 1 024 octets de Base64 codé des données des joueurs de jeu spécifique. Cette valeur est opaque pour le serveur.| 
| Gamertag| chaîne| Gamertag, un maximum de 15 caractères, du lecteur. Le client doit utiliser cette valeur dans l’interface utilisateur lors de l’identification du lecteur. | 
| isCurrentlyInSession| Valeur booléenne| Indique si le lecteur est actuellement dans la session ou gauche de la session.| 
| seatIndex| entier signé 32 bits| L’index de l’acteur dans la session.| 
| xuid| entier non signé 64 bits| L’ID d’utilisateur Xbox (XUID) du lecteur.| 
  
<a id="ID4E3C"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "xuid": 2533274790412952,
    "gamertag":"MyTestUser",
    "seatindex": 3
    "customData":"AIHJ2?iE?/jiKE!l5S=T..."
    "isCurrentlyInSession":"true"
}
    
```

  
<a id="ID4EFD"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EHD"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference"></a>Référence 

[GameSession (JSON)](json-gamesession.md)

   