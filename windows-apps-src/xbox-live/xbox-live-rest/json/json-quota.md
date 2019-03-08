---
title: quotaInfo (JSON)
assetID: 3147ab78-e671-e142-0a3a-688dc4079451
permalink: en-us/docs/xboxlive/rest/json-quota.html
description: " quotaInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f3499fdba972d6e953813fc490d080910921698e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654114"
---
# <a name="quotainfo-json"></a>quotaInfo (JSON)
Contient des informations de quota sur un groupe de titre. 
<a id="ID4EN"></a>

 
## <a name="quotainfo"></a>quotaInfo
 
L’objet quotaInfo présente les spécifications suivantes.
 
Pour le stockage global
 
| Membre| Type| Description| 
| --- | --- | --- | 
| quotaBytes| entier signé 32 bits | Nombre maximal d’octets utilisables par le titre.| 
| usedBytes| entier signé 32 bits | Nombre d’octets utilisés par le titre.| 
  
<a id="ID4EXB"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 
L’exemple suivant montre la réponse pour le stockage global :
 

```json
{
    "quotaInfo":
    {
        "usedBytes":4194304,
        "quotaBytes":536870912
    }
}
      
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   