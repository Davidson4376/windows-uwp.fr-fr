---
title: UserSettings (JSON)
assetID: 17c030cb-05e0-f78e-5ab1-cdbd8b801ceb
permalink: en-us/docs/xboxlive/rest/json-usersettings.html
description: " UserSettings (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5451c59ab608105677a657ade41154bd2b622f5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655044"
---
# <a name="usersettings-json"></a>UserSettings (JSON)
Retourne les paramètres pour un utilisateur authentifié actuel. 
<a id="ID4EN"></a>

 
## <a name="usersettings"></a>UserSettings
 
L’objet UserSettings a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| id| entier non signé 32 bits| L’identificateur du paramètre.| 
| Source| entier non signé 32 bits| Représente la source du paramètre. | 
| titleId| entier non signé 32 bits| L’identificateur du titre associé au paramètre. | 
| value| tableau d’entiers non signés 8 bits| Représente la valeur du paramètre. Les clients de la récupération des paramètres doivent comprendre le format de représentation pour être en mesure de lire les données. | 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
   "id":268697600,
   "source":1,
   "titleId":1297287259,
   "value":"CAAAAA=="
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   