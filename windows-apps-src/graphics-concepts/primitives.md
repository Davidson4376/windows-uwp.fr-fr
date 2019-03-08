---
title: Primitives
description: Une primitive 3D est un ensemble de sommets qui forme une entité 3D unique.
ms.assetid: A1FE6F49-B0D4-4665-90E1-40AD98E668B1
keywords:
- Primitives
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6c2bf3aa2f421efa7061f3003e6cab8c9f7b8c58
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648664"
---
# <a name="primitives"></a>Primitives


Une *primitive* 3D est un ensemble de vertex qui forme une entité 3D unique.

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
<td align="left"><p><a href="point-lists.md">Listes de points</a></p></td>
<td align="left"><p>Une liste de points est un ensemble de sommets qui sont rendus sous forme de points isolés. Votre application peut utiliser des listes de points dans les scènes 3D pour les champs d’étoiles, ou encore des lignes en pointillé sur la surface d’un polygone.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="line-lists.md">Listes de ligne</a></p></td>
<td align="left"><p>Une liste de lignes est une liste de segments de ligne rectilignes et isolés. Les listes de lignes sont utiles pour certaines tâches telles que l’ajout de neige fondue ou de pluie abondante dans une scène 3D. Les applications créent une liste de lignes en remplissant un tableau de vertex.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="line-strips.md">Bandes de ligne</a></p></td>
<td align="left"><p>Une bande de lignes est une primitive composée de segments de ligne connectés. Votre application peut utiliser des bandes de lignes pour créer des polygones non fermés. Un polygone fermé est un polygone dont le dernier vertex est connecté au premier vertex par un segment de ligne.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="triangle-lists.md">Listes de triangle</a></p></td>
<td align="left"><p>Une liste de triangles est une liste de triangles isolés. Les triangles isolés peuvent être proches ou non. Une liste de triangles doit avoir au moins trois vertex et le nombre total de vertex doit être divisible par trois.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-strips.md">Bandes de triangles</a></p></td>
<td align="left"><p>Une bande de triangles est une série de triangles connectés. Les triangles étant connectés, l'application n'a pas besoin de spécifier plusieurs fois les trois vertex pour chaque triangle.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et geometry](coordinate-systems-and-geometry.md)

 

 




