---
title: "Mémoires tampons de vertex et d&quot;index"
description: "Les mémoires tampons de vertex comportent des données de vertex ; les vertex sont traités dans le cadre des opérations de transformation, d&quot;éclairage et de découpage."
ms.assetid: 8A39CD23-85FB-4424-9AC3-37919704CD68
keywords:
- "Mémoires tampons de vertex et d&quot;index"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: cdf65da6504114029c4937ee7db531a1339ed4c2
ms.lasthandoff: 02/07/2017

---

# <a name="vertex-and-index-buffers"></a>Mémoires tampons de vertex et d'index


Les *mémoires tampons de vertex* comportent des données de vertex ; les vertex sont traités dans le cadre des opérations de transformation, d'éclairage et de découpage. Les *mémoires tampons* d'index sont des mémoires comportant des données d'index, qui sont des décalages d'entiers dans des mémoires tampons de vertex, utilisés pour l'affichage des primitives.

Les mémoires tampons de vertex peuvent contenir tout type de vertex (transformé ou non transformé, allumé ou éteint) qui peut être rendu. Vous pouvez traiter les sommets dans une mémoire tampon de vertex pour effectuer des opérations telles que la transformation, l'éclairage, ou la génération d'indicateurs de découpage. La transformation est toujours effectuée.

La flexibilité des mémoires tampons de vertex en fait des points d’arrêt parfaits pour réutiliser une géométrie transformée. Vous pouvez créer une mémoire tampon de vertex unique, transformer, éclairer et découper les sommets dedans, puis restituer le modèle dans la scène autant de fois que nécessaire, sans avoir à le retransformer, même avec des changements d’état de rendu entrelacés. Cela se révèle particulièrement utile lors du rendu de modèles qui utilisent plusieurs textures : la géométrie est transformée une seule fois, puis des parties de celle-ci peuvent être rendues en fonction des besoins, entrelacées avec les modifications de texture requises. Les changements d’état de rendu effectués après le traitement des sommets prennent effet au prochain traitement des sommets.

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
<td align="left"><p>[Introduction aux mémoires tampons](introduction-to-buffers.md)</p></td>
<td align="left"><p>Une ressource de mémoire tampon est un ensemble de données dont le type a été intégralement spécifié, regroupées en éléments. Les mémoires tampons stockent des données, telles que les coordonnées de texture dans une <em>mémoire tampon de vertex</em>, les index dans une <em>mémoire tampon d’index</em>, les données constantes de nuanceur dans une <em>mémoire tampon constante</em>, les vecteurs de position, les vecteurs normaux ou l’état de l’appareil.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Mémoires tampons d’index](index-buffers.md)</p></td>
<td align="left"><p><em>Les mémoires tampons d'index</em> sont des mémoires comportant des données d'index, qui sont des décalages d'entiers dans des mémoires tampons de vertex, utilisés pour l'affichage des primitives.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage des graphiques Direct3D](index.md)

 

 





