---
title: PeopleList (JSON)
assetID: ac538652-c10c-44e5-c1e3-5314ebe8ba83
permalink: en-us/docs/xboxlive/rest/json-peoplelist.html
description: " PeopleList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f9ab412088707752d62cc20fd54da2639f26ddc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613324"
---
# <a name="peoplelist-json"></a>PeopleList (JSON)
Collection de [personne](json-person.md) objets. 
<a id="ID4ER"></a>

 
## <a name="peoplelist"></a>PeopleList
 
L’objet PeopleList a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| contacts| tableau de [personne](json-person.md)| Le [personne](json-person.md) objets qui composent la liste de personnes.| 
| totalCount| entier non signé 32 bits| Nombre total de [personne](json-person.md) disponible dans le jeu d’objets. Cette valeur peut être utilisée par les clients pour la pagination, car il représente la taille de l’ensemble, pas seulement la réponse la plus récente. Exemple de valeur : 680.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVC"></a>

 
##### <a name="reference"></a>Référence 

[OBTENIR (/users/ {ownerId} / personnes)](../uri/people/uri-usersowneridpeopleget.md)

 [POST (/users/ {ownerId} / people/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   