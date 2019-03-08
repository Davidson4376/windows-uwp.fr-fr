---
title: MediaAsset (JSON)
assetID: 858c720a-1648-738b-bb43-f050e7cac19e
permalink: en-us/docs/xboxlive/rest/json-mediaasset.html
description: " MediaAsset (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 308a65b7c049a6aba0405865bab63fb9d28b8506
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623474"
---
# <a name="mediaasset-json"></a>MediaAsset (JSON)
Les éléments multimédias associés à la réalisation ou ses récompenses.
<a id="ID4EN"></a>


## <a name="mediaasset"></a>MediaAsset

L’objet MediaAsset a la spécification suivante.

| Membre| Type| Description|
| --- | --- | --- |
| name| chaîne| Le nom de la MediaAsset, tels que « tile01 ».|
| type| Énumération de MediaAssetType| Le type de l’élément multimédia : <ul><li>icône (0) : Icône de réalisation.</li><li>art (1) : La ressource des images numériques.</li></ul> | 
| url| chaîne| L’URL de la MediaAsset.|

<a id="ID4EFC"></a>


## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON


```json
{
  "name":"Icon Name",
  "type":"Icon",
  "url":"https://www.xbox.com"
}

```


<a id="ID4EOC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EQC"></a>


##### <a name="parent"></a>Parent

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)
