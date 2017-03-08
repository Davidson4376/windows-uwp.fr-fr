---
title: Transformations
description: "La partie de Direct3D qui pousse la géométrie à travers le pipeline de géométrie à fonctions fixes est le moteur de transformation."
ms.assetid: 0DF2A99A-335C-4D14-9720-6D7996DD635A
keywords:
- Transformations
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2036573c0a5b2967bda38473b126b85f259c278c
ms.lasthandoff: 02/07/2017

---

# <a name="transforms"></a>Transformations


La partie de Direct3D qui pousse la géométrie à travers le pipeline de géométrie à fonctions fixes est le moteur de transformation. Il localise le modèle et l'observateur dans l'univers, projette les vertex pour l’affichage à l’écran et les découpe pour la fenêtre d’affichage. Le moteur de transformation effectue également des calculs d’éclairage afin de déterminer les composants diffus et spéculaires à chaque vertex.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Article</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Vue d’ensemble de la transformation](transform-overview.md)</p></td>
<td align="left"><p>Les transformations de matrice gèrent une grande partie des calculs de bas niveau des graphiques 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Transformation universelle](world-transform.md)</p></td>
<td align="left"><p>Une transformation universelle change les coordonnées à partir de l'espace du modèle, dans lequel les vertex sont définis par rapport à l’origine locale d’un modèle, vers l'espace universel. Dans l’espace universel, les vertex sont définis par rapport à l’origine commune de tous les objets d'une scène. La transformation universelle place un modèle dans le monde.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Transformation de vue](view-transform.md)</p></td>
<td align="left"><p>Une <em>transformation de vue</em> localise l'observateur dans l’espace universel, transformant les vertex en espace de caméra. Dans l'espace caméra, la caméra (ou l’observateur) se trouve à l’origine, dans la direction z positive. La matrice replace les objets dans le monde autour de la position (l’origine de l’espace de la caméra) et de l’orientation d'une caméra.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Transformation de projection](projection-transform.md)</p></td>
<td align="left"><p>Une <em>transformation de projection</em> permet de contrôler les éléments internes de la caméra tels que le choix d'un objectif. Il s'agit du plus complexe des trois types de transformations.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 





