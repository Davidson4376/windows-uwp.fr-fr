---
title: UserList (JSON)
assetID: 894f5a39-4eed-0c5b-fc45-cb0097dc46fd
permalink: en-us/docs/xboxlive/rest/json-userlist.html
description: " UserList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 46e5323f4eae8e91b61295c4112b5bacfc8a1759
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598494"
---
# <a name="userlist-json"></a>UserList (JSON)
Une collection de [utilisateur](json-user.md) objets. 
<a id="ID4ER"></a>

 
## <a name="userlist"></a>UserList
 
L’objet UserList a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| utilisateurs| Tableau de [utilisateur (JSON)](json-user.md)| Consultez l’exemple JSON ci-dessous.| 
  
<a id="ID4EPB"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ] 
}
    
```

  
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   