---
title: Person (JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
description: " Person (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 175d66ffc7744ca8203fe7681fcb0167e150f012
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640864"
---
# <a name="person-json"></a>Person (JSON)
Métadonnées relatives à une seule personne dans le système de personnes. 
<a id="ID4EN"></a>

 
## <a name="person"></a>Person
 
L’objet Person a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| xuid| chaîne| Obligatoire. Xbox utilisateur ID (XUID) sous forme décimale. Exemple de valeur : 2603643534573573.| 
| isFavorite| Valeur booléenne| Obligatoire. Indique si cette personne est celle qui intéressent l’utilisateur plus. Étant donné que les utilisateurs peuvent avoir un très grand nombre de personnes dans leur liste de personnes, des personnes Favoris doivent avoir par ordre de priorité dans les expériences et affiché avant d’autres qui ne sont pas des Favoris.| 
| isFollowingCaller| Valeur booléenne| Facultatif. Si cette personne est suivent l’utilisateur au nom duquel l’appel d’API a été effectué.| 
| socialNetworks| tableau de chaînes| Facultatif. Dans les réseaux externes l’utilisateur et cette personne ont une relation.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>Référence 

[/users/{ownerId}/people/{targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/{ownerId}/people/xuids](../uri/people/uri-usersowneridpeoplexuids.md)

   