---
title: "Systèmes de coordonnées et géométrie"
description: "La programmation des applicationsDirect3D nécessite une maîtrise des principes géométriques3D. Cette section présente les concepts géométriques les plus importants associés à la création de scènes3D."
ms.assetid: E82EB0A9-0678-496B-96B3-8993BA580099
keywords: "Systèmes de coordonnées et géométrie"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 01885f6e2014e1659b1e59cc13bc6ce275f1ad03
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="coordinate-systems-and-geometry"></a>Systèmes de coordonnées et géométrie


La programmation des applicationsDirect3D nécessite une maîtrise des principes géométriques3D. Cette section présente les concepts géométriques les plus importants associés à la création de scènes3D.

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
<td align="left"><p>[Systèmes de coordonnées](coordinate-systems.md)</p></td>
<td align="left"><p>Les applications graphiques 3D utilisent généralement l'un des deux types de systèmes de coordonnées cartésiens: gaucher ou droitier. Dans les deux systèmes de coordonnées, l’axe x positif pointe vers la droite et l’axe y positif pointe vers le haut.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Primitives](primitives.md)</p></td>
<td align="left"><p>Une <em>primitive</em> 3D est un ensemble de vertex qui forme une entité3D unique.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Vecteurs normaux de face et de vertex](face-and-vertex-normal-vectors.md)</p></td>
<td align="left"><p>Chaque face d'un maillage comporte un vecteur normal d'une unité perpendiculaire. La direction du vecteur est déterminée par l'ordre dans lequel les vertex sont définis et de si le système de coordonnées se trouve à droite ou à gauche.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Rectangles](rectangles.md)</p></td>
<td align="left"><p>Dans la programmationDirect3D et Windows, les objets à l'écran sont appelés «rectangles englobants». Les côtés d'un rectangle englobant sont toujours parallèles aux côtés de l'écran, ce qui signifie que le rectangle peut être décrit par deux points: l'angle supérieur gauche et l'angle inférieur droit.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Interpolation de triangles](triangle-interpolation.md)</p></td>
<td align="left"><p>Pendant le rendu, le pipeline interpole les données des vertex pour chaque triangle. Les données de vertex peuvent représenter un large éventail de données et inclure (sans s'y limiter) les données suivantes: la couleur diffuse, la couleur spéculaire, l'alpha diffus (opacité du triangle), l'alpha spéculaire et un facteur brouillard.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Vecteurs, sommets et quaternions](vectors--vertices--and-quaternions.md)</p></td>
<td align="left"><p>Dans Direct3D, les sommets décrivent la position et l’orientation. Chaque vertex dans une primitive est décrite par un vecteur qui donne sa position, sa couleur, ses coordonnées de texture et par un vecteur normal qui donne son orientation.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Transformations](transforms.md)</p></td>
<td align="left"><p>La partie de Direct3D qui pousse la géométrie à travers le pipeline de géométrie à fonctions fixes est le moteur de transformation. Il localise le modèle et l'observateur dans l'univers, projette les vertex pour l’affichage à l’écran et les découpe pour la fenêtre d’affichage. Le moteur de transformation effectue également des calculs d’éclairage afin de déterminer les composants diffus et spéculaires à chaque vertex.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Fenêtres d’affichage et découpage](viewports-and-clipping.md)</p></td>
<td align="left"><p>Une <em>fenêtre d’affichage</em> est un rectangle en deux dimensions (2D) dans lequel une scène3D est projetée. Dans Direct3D, le rectangle existe en tant que coordonnées au sein d’une surface Direct3D que le système utilise en tant que cible de rendu. La transformation de projection convertit les vertex dans le système de coordonnées utilisé pour la fenêtre d’affichage. Une fenêtre d’affichage est également utilisée pour spécifier la plage de valeurs de profondeur sur une surface de cible de rendu dans laquelle une scène sera rendue (généralement de0,0 à1,0).</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage des graphismes Direct3D](index.md)

 

 




