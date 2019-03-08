---
title: Progression (JSON)
assetID: cdff6415-f12b-0a45-61f2-26dbf47b1b56
permalink: en-us/docs/xboxlive/rest/json-progression.html
description: " Progression (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dda124e5be9a4d21a1ee5b9d6130290207e31921
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593824"
---
# <a name="progression-json"></a>Progression (JSON)
Progression de l’utilisateur vers le déverrouillage de la prime. 
<a id="ID4EN"></a>

 
## <a name="progression"></a>Progression
 
L’objet de Progression a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| conditions requises| tableau de spécifications| La configuration requise pour obtenir la prime et la distance le long de l’utilisateur est vers déverrouillage.| 
| timeUnlocked| DateTime| Heure de que la prime abord est déverrouillée.| 
  
<a id="ID4ETB"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
  "requirements":
  [{
    "id":"12345678-1234-1234-1234-123456789012",
    "current":null,
    "target":"100"
  }],
  "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
}
    
```

  
<a id="ID4E3B"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E5B"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   