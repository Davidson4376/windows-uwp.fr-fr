---
title: Transformations
description: La partie de Direct3D qui pousse la géométrie à travers le pipeline de géométrie à fonctions fixes est le moteur de transformation.
ms.assetid: 0DF2A99A-335C-4D14-9720-6D7996DD635A
keywords:
- Transformations
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9eb129609b2fa8564b04d128c3cb06251b044360
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7288085"
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
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="transform-overview.md">Vue d’ensemble de la transformation</a></p></td>
<td align="left"><p>Les transformations de matrice gèrent une grande partie des calculs de bas niveau des graphiques3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="world-transform.md">Transformation universelle</a></p></td>
<td align="left"><p>Une transformation universelle change les coordonnées à partir de l'espace du modèle, dans lequel les vertex sont définis par rapport à l’origine locale d’un modèle, vers l'espace universel. Dans l’espace universel, les vertex sont définis par rapport à l’origine commune de tous les objets d'une scène. La transformation universelle place un modèle dans le monde.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="view-transform.md">Transformation de vue</a></p></td>
<td align="left"><p>Une <em>transformation de vue</em> localise l'observateur dans l’espace universel, transformant les vertex en espace de caméra. Dans l'espace caméra, la caméra (ou l’observateur) se trouve à l’origine, dans la direction z positive. La matrice replace les objets dans le monde autour de la position (l’origine de l’espace de la caméra) et de l’orientation d'une caméra.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="projection-transform.md">Transformation de projection</a></p></td>
<td align="left"><p>Une <em>transformation de projection</em> permet de contrôler les éléments internes de la caméra tels que le choix d'un objectif. Il s'agit du plus complexe des trois types de transformations.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 




