---
title: LastSeenRecord (JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
description: " LastSeenRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e06de31cabaedb68ed57d3d4f2ff30614ceb6317
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613374"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord (JSON)
Informations sur lorsque le système vu dernière un utilisateur, disponible lorsque l’utilisateur ne dispose d’aucune DeviceRecord valide. 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
L’objet LastSeenRecord a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| deviceType| chaîne| Le type de l’appareil sur lequel l’utilisateur était présent dernière.| 
| titleId| entier non signé 32 bits| L’identificateur du titre sur lequel l’utilisateur était présent dernière.| 
| titleName| chaîne| Le nom du titre sur lequel l’utilisateur était présent dernière.| 
| Horodatage| DateTime| Horodatage UTC qui indique quand l’utilisateur était présent dernière.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
  deviceType:W8,    
  titleId:"23452345",
  titleName:"My Awesome Game",
  timestamp:"2012-09-17T07:15:23.4930000"
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>Référence   