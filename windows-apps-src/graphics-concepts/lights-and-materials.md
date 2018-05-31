---
title: Éclairage
description: Les lumières sont utilisées pour illuminer les objets d’une scène. La couleur de chaque vertex d’objet est basée sur la carte de texture, les couleurs de vertex et les sources de lumière actuelles.
ms.assetid: AB16CF5B-47CE-455C-A8BD-36305E54BEA9
keywords:
- Éclairage
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: e2750de79dbdb01081aa099ff955fcef2999dcbf
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652493"
---
# <a name="lighting"></a>Éclairage


Les lumières sont utilisées pour illuminer les objets d’une scène. La couleur de chaque vertex d’objet est basée sur la carte de texture, les couleurs de vertex et les sources de lumière actuelles.

**Remarque**   Cette section concerne uniquement le pipeline à fonction fixe. Les nuanceurs programmables prennent explicitement en charge toutes les fonctions d’éclairage.

 

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
<td align="left"><p><a href="lighting-overview.md">Vue d’ensemble de l’éclairage</a></p></td>
<td align="left"><p>Lorsque vous recourez à l’éclairageDirect3D, vous autorisez Direct3D à gérer les paramètres d’illumination à votre place. S’ils le souhaitent, les utilisateurs avancés peuvent ajuster eux-mêmes l’éclairage.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="light-types.md">Types de lumière</a></p></td>
<td align="left"><p>La propriété du type de lumière définit le type de source lumineuse utilisé. Direct3D prend en charge 3types de lumières: les lumières à points, les projecteurs et les lumières directionnelles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="light-properties.md">Propriétés de lumière</a></p></td>
<td align="left"><p>Les propriétés de lumière décrivent le type de source lumineuse (à points, directionnelle, projecteur), l’atténuation, la couleur, la direction, la position et l’étendue.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mathematics-of-lighting.md">Formules mathématiques d’éclairage</a></p></td>
<td align="left"><p>Le modèle d’éclairage Direct3D prend en charge les éclairages ambiant, diffus, spéculaire et émissif. La souplesse de la configuration permet de résoudre de nombreuses situations d’éclairage. Dans une scène, la quantité globale d’éclairage s’appelle l’<em>illumination globale</em>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage des graphismes Direct3D](index.md)

 

 




