---
title: UserClaims (JSON)
assetID: f88d5ee0-2875-fcfb-3098-3cd6afce8748
permalink: en-us/docs/xboxlive/rest/json-userclaims.html
description: " UserClaims (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 21b4322d002747145c3b09e0f3cd7eb03874380b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623344"
---
# <a name="userclaims-json"></a>UserClaims (JSON)
Retourne des informations sur l’utilisateur authentifié actuel. 
<a id="ID4EN"></a>

 
## <a name="userclaims"></a>Userclaims.
 
L’objet UserClaims a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| Gamertag| chaîne| identité de l’utilisateur.| 
| xuid| entier non signé 64 bits| L’ID d’utilisateur Xbox (XUID) de l’utilisateur.| 
  
<a id="ID4EZB"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
   "xuid" : 2533274790412952,
   "gamertag" : "gamer"

}
    
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   