---
title: GameClipUri (JSON)
assetID: 03c097e8-7f29-1026-7a77-5c785b8511e9
permalink: en-us/docs/xboxlive/rest/json-gameclipuri.html
description: " GameClipUri (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1c74d0831c3b841ad2a1366bd2e03fb8a9b0448d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623494"
---
# <a name="gameclipuri-json"></a>GameClipUri (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclipuri"></a>GameClipUri
 
L’objet GameClipUri a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| <b>uri</b>| chaîne| URI à l’emplacement de l’élément multimédia vidéo.| 
| <b>fileSize</b>| entier non signé 32 bits| La taille totale des fichiers de l’image miniature.| 
| <b>uriType</b>| GameClipUriType| Le type de l’URI.| 
| <b>expiration</b>| DateTime| Le délai d’expiration de l’URI qui est inclus dans cette réponse. Si l’URL est vide ou considéré comme expiré avant la lecture, les appelants doivent appeler l’API RefreshUrl.| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   