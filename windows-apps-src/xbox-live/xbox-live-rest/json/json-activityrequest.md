---
title: ActivityRequest (JSON)
assetID: 9eb03ab7-352c-4465-ec86-d544e76f63f9
permalink: en-us/docs/xboxlive/rest/json-activityrequest.html
description: " ActivityRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 791a566d278b92aeb34ab36d38719b44e9cc6c8f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628154"
---
# <a name="activityrequest-json"></a>ActivityRequest (JSON)
Une demande d’informations sur la présence riche d’un ou plusieurs utilisateurs. 
<a id="ID4EN"></a>

 
## <a name="activityrequest"></a>ActivityRequest
 
L’objet ActivityRequest a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| richPresence| [RichPresenceRequest](json-richpresencerequest.md)| Le nom convivial de la chaîne de présence riche qui doit être utilisé.| 
| Média| MediaRequest| Informations de support pour ce que l’utilisateur est à l’écoute à ou regarder.| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f"
    }
  }
    
```

  
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   