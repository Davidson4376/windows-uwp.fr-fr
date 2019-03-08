---
title: User (JSON)
assetID: dbc733e4-0348-0e3d-1f55-17b465e599d6
permalink: en-us/docs/xboxlive/rest/json-user.html
description: " User (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5c1b3a34ef329696d51e615dd79d57783a132d05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641354"
---
# <a name="user-json"></a>User (JSON)
Contient les données de classement utilisateur. 
<a id="ID4EN"></a>

 
## <a name="user"></a>Utilisateur
 
L’objet utilisateur a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| Gamertag| chaîne| Identité de l’acteur (15 caractères maximum). Le client doit utiliser cette valeur dans l’interface utilisateur lors de l’identification du lecteur.| 
| rang| entier signé 32 bits| Le rang de l’utilisateur par rapport à l’utilisateur qui demande les données de classement.| 
| rating| chaîne| Évaluation de l’utilisateur.| 
| xuid| entier non signé 64 bits| L’ID d’utilisateur Xbox (XUID) de l’utilisateur.| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{ 
   "gamertag":"TrueBlue402",
   "rank":2,
   "rating":"2:19:21.17",
   "xuid":1234567890123456 
}
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   