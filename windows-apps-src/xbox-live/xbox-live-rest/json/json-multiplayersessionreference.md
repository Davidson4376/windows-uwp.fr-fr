---
title: MultiplayerSessionReference (JSON)
assetID: 6e03e060-8c9b-b394-415f-af7e85be569f
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionreference.html
description: " MultiplayerSessionReference (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5986079e1cae3338d8cc24a9e85f6941cf4fbec4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651244"
---
# <a name="multiplayersessionreference-json"></a>MultiplayerSessionReference (JSON)
Un objet JSON qui représente le **MultiplayerSessionReference**. 
<a id="ID4EQ"></a>

  
 
L’objet JSON de MultiplayerSessionReference a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| scid| GUID| Identificateur de configuration de service (SCID). Partie 1 de l’identificateur de session.| 
| templateName | chaîne | Nom de l’instance actuelle du modèle de session. Partie 2 de l’identificateur de session. | 
| name | chaîne | Nom de la session. Partie 3 de l’identificateur de session. | 
  
<a id="ID4EZ"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON 
 

```json
{
  "scid": "8d050174-412b-4d51-a29b-d55a34edfdb7",
  "templateName": "integration",
  "name": "19de0095d8bb41048f19edbbb6bc6b04"
}
  
    
```

  
<a id="ID4EJB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ELB"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVB"></a>

 
##### <a name="reference"></a>Référence 

[MultiplayerSession (JSON)](json-multiplayersession.md)

   