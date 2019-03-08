---
title: GameClipThumbnail (JSON)
assetID: 3ed87fc1-734c-d8b5-d908-0ae3359769ed
permalink: en-us/docs/xboxlive/rest/json-gameclipthumbnail.html
description: " GameClipThumbnail (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ad2d35431cb4c40690978f4f3920f2e47f2b9bc0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606564"
---
# <a name="gameclipthumbnail-json"></a>GameClipThumbnail (JSON)
Contient les informations relatives à une miniature individuelle. Il peut y avoir plusieurs tailles de chaque élément, et il incombe au client de sélectionner celui approprié pour l’affichage. 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
L’objet GameClipThumbnail a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| <b>uri</b>| chaîne| L’URI de l’image miniature.| 
| <b>fileSize</b>| entier non signé 32 bits| La taille totale des fichiers de l’image miniature.| 
| <b>thumbnailType</b>| ThumbnailType| Le type d’image miniature.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   