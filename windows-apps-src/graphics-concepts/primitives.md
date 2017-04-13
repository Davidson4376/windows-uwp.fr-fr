---
title: Primitives
description: "Une primitive3D est un ensemble de sommets qui forme une entité3D unique."
ms.assetid: A1FE6F49-B0D4-4665-90E1-40AD98E668B1
keywords: Primitives
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 22f143030d3973cf77cd3b0857cf9b67ff5503fb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="primitives"></a>Primitives


Une *primitive* 3D est un ensemble de sommets qui forme une entité3D unique.

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
<td align="left"><p>[Listes de points](point-lists.md)</p></td>
<td align="left"><p>Une liste de points est un ensemble de sommets qui sont rendus sous forme de points isolés. Votre application peut utiliser des listes de points dans les scènes3D pour les champs d’étoiles, ou encore des lignes en pointillé sur la surface d’un polygone.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Listes de lignes](line-lists.md)</p></td>
<td align="left"><p>Une liste de lignes est une liste de segments de ligne rectilignes et isolés. Les listes de lignes sont utiles pour certaines tâches telles que l’ajout de neige fondue ou de pluie abondante dans une scène3D. Les applications créent une liste de lignes en remplissant un tableau de sommets.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Bandes de lignes](line-strips.md)</p></td>
<td align="left"><p>Une bande de lignes est une primitive composée de segments de ligne connectés. Votre application peut utiliser des bandes de lignes pour créer des polygones non fermés. Un polygone fermé est un polygone dont le dernier sommet est connecté au premier sommet par un segment de ligne.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Listes de triangles](triangle-lists.md)</p></td>
<td align="left"><p>Une liste de triangles est une liste de triangles isolés. Les triangles isolés peuvent être proches ou non. Une liste de triangles doit avoir au moins trois sommets, et le nombre total de sommets doit être divisible par trois.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Bandes de triangles](triangle-strips.md)</p></td>
<td align="left"><p>Une bande de triangles est une série de triangles connectés. Les triangles étant connectés, l’application n’a pas besoin de spécifier plusieurs fois les trois sommets pour chaque triangle.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 




