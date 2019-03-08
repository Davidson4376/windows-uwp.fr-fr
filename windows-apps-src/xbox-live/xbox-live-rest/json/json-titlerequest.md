---
title: TitleRequest (JSON)
assetID: 43aeb6f9-726d-9260-e2ba-f005ea688bf1
permalink: en-us/docs/xboxlive/rest/json-titlerequest.html
description: " TitleRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a90f42c2f830ba6f04f77a1acaba067a2746a062
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593794"
---
# <a name="titlerequest-json"></a>TitleRequest (JSON)
Demande d’informations sur un titre. 
<a id="ID4EN"></a>

 
## <a name="titlerequest"></a>TitleRequest
 
L’objet TitleRequest a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| id| entier non signé 32 bits| Identificateur du titre.| 
| activité| [ActivityRequest](json-activityrequest.md)| Dans titre d’informations, y compris des informations enrichies de présence et de support, s’il est disponible.| 
| État| chaîne| Indique si un utilisateur est actif ou non. Ce champ est obligatoire pour marquer un utilisateur comme étant inactive. La valeur par défaut est « actif ».| 
| sélection élective| chaîne| Le mode de sélection élective du titre. Les valeurs possibles sont « complet », « remplit », « aligné » ou « en arrière-plan ». La valeur par défaut est « complet ».| 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
  id:"12341234",
  placement:"snapped",
  state:"active",
  activity:
  {
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"82b11353-08ba-48ca-9f1a-21627b189b0f"
    }
  }
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>Référence 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   