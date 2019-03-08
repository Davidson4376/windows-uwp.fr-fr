---
title: InitialUploadResponse (JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
description: " InitialUploadResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab59fefb389cf550a1bc4fc6429f6b0970f50ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589834"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse (JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
L’objet InitialUploadResponse a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| <b>gameClipId</b>| chaîne| ID affecté pour la demande de données de chargement.| 
| <b>uploadUri</b>| URI| Emplacement dans lequel le clip de jeu doit être téléchargé.| 
| <b>largeThumbnailUri</b>| URI| Facultatif. Emplacement dans lequel la grande vignette doit être téléchargée. Présence de ce champ est déterminée par le [ThumbnailSource énumération](../enums/gvr-enum-thumbnailsource.md) valeur dans le <b>InitialUploadRequest</b> (sera présent lorsque le téléchargement est spécifié).| 
| <b>smallThumbnailUri</b>| URI| Facultatif. Emplacement auquel la petite vignette doit être chargée. Présence de ce champ est déterminée par le [ThumbnailSource énumération](../enums/gvr-enum-thumbnailsource.md) valeur dans le <b>InitialUploadRequest</b> (sera présent lorsque le téléchargement est spécifié).| 
  
<a id="ID4EYC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752"  ,  
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
    
```

  
<a id="ID4EBD"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EDD"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   