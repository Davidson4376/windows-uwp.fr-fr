---
title: ServiceErrorResponse (JSON)
assetID: a2077df8-f76c-0233-8e41-68267b681862
permalink: en-us/docs/xboxlive/rest/json-serviceerrorresponse.html
description: " ServiceErrorResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 86f9389f6f76c1c51955a6c784393e9b05909298
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597504"
---
# <a name="serviceerrorresponse-json"></a>ServiceErrorResponse (JSON)
Lorsqu’une erreur de service est rencontrée, un code d’erreur HTTP approprié s’affichera. Si vous le souhaitez, le service peut également inclure un objet ServiceErrorResponse tel que défini ci-dessous. Dans les environnements de production, moins de données peut être inclus. 
<a id="ID4EN"></a>

 
## <a name="serviceerrorresponse"></a>ServiceErrorResponse
 
L’objet ServiceErrorResponse a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| <b>errorCode</b>| entier signé 32 bits| Code associé à l’erreur (peut être null).| 
| <b>errorMessage</b>| chaîne| Obtenir des détails supplémentaires sur l’erreur.| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
   "errorCode": 8377,
   "errorMessage": "XUID specified in the claim does not match URI XUID."
 }
    
```

  
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   