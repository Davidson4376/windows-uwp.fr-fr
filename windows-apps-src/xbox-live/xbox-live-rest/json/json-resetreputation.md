---
title: ResetReputation (JSON)
assetID: 15edb5e7-a00b-4188-9b49-9db5774c4a10
permalink: en-us/docs/xboxlive/rest/json-resetreputation.html
description: " ResetReputation (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d09c8bbc1130f91dfea3d4c35e391dcf9adcf127
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649414"
---
# <a name="resetreputation-json"></a>ResetReputation (JSON)
Contient les scores de réputation de le base à laquelle les scores existant d’un utilisateur doivent être modifiées. 
<a id="ID4EN"></a>

 
## <a name="resetreputation"></a>ResetReputation
 
L’objet ResetReputation a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| fairplayReputation| nombre| Souhaité nouveau score de base Fairplay réputation de l’utilisateur (plage 0 à 75 valide).| 
| commsReputation| nombre| Souhaité de base que score de réputation de communications pour l’utilisateur (plage 0 à 75 valide).| 
| userContentReputation| nombre| Souhaité nouveau score de base UserContent réputation de l’utilisateur (plage 0 à 75 valide).| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   