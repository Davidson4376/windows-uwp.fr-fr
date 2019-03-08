---
title: XuidList (JSON)
assetID: 06938a52-e582-a15b-ec7f-4b053dfc28ad
permalink: en-us/docs/xboxlive/rest/json-xuidlist.html
description: " XuidList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d8172063d40f8df77827ab845c4dfd0c0799811
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627914"
---
# <a name="xuidlist-json"></a>XuidList (JSON)
Liste des XUIDs sur laquelle effectuer une opération. 
<a id="ID4EN"></a>

 
## <a name="xuidlist"></a>XuidList
 
L’objet XuidList a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| xuids| tableau de chaînes| Liste de valeurs d’ID d’utilisateur Xbox (XUID) sur lequel une opération doit être effectuée ou données doivent être retournées.| 
  
<a id="ID4EMB"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
    
```

  
<a id="ID4EVB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EXB"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EBC"></a>

 
##### <a name="reference"></a>Référence 

[POST (/users/ {ownerId} / people/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   