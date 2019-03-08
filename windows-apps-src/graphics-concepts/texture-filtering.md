---
title: Filtrage de textures
description: Le filtrage de textures génère une couleur pour chaque pixel dans l’image rendue en 2D de la primitive lorsqu’une primitive est affichée en mappant une primitive 3D sur un écran 2D.
ms.assetid: 1CCF4138-5D48-4B07-9490-996844F994D8
keywords:
- Filtrage de textures
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 449a31d92235efc50119bcd0db11b3532f523cd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633724"
---
# <a name="texture-filtering"></a>Filtrage de textures


Le filtrage de textures génère une couleur pour chaque pixel dans l’image rendue en 2D de la primitive lorsqu’une primitive est affichée en mappant une primitive 3D sur un écran 2D.

Lorsque Direct3D effectue le rendu d’une primitive, la primitive 3D est mappée sur un écran 2D. Si la primitive dispose d’une texture, Direct3D doit utiliser cette texture pour produire une couleur pour chaque pixel de l’image rendue en 2D de la primitive. Pour chaque pixel de l’image de la primitive sur l’écran, une valeur de couleur doit être obtenue de la texture. Ce processus est appelé *filtrage de textures*.

Lors de l’exécution d’une opération de filtrage de texture, la texture utilisée est généralement également agrandie ou réduite. En d’autres termes, elle est mappée vers une image primitive supérieure ou inférieure. L’agrandissement d’une texture peut entraîner le mappage d’un grand nombre de pixels dans un texel. Le résultat peut donner une apparence approximative. La réduction d’une texture signifie souvent qu’un seul pixel est mappé à nombreux texels. L’image obtenue permettre être floue ou crénelée. Pour résoudre ces problèmes, une fusion des couleurs de texels doit être effectuée pour obtenir une couleur de pixel.

Direct3D simplifie le processus complexe de filtrage de textures. Il vous fournit trois types de filtrage de textures : le filtrage linéaire, le filtrage anisotropique et le filtrage de mipmaps. Si vous ne sélectionnez aucun filtrage de textures, Direct3D utilise une technique appelée l’échantillonnage des points les plus proches.

Chaque type de filtrage de textures présente des avantages et des inconvénients. Par exemple, le filtrage linéaire de textures peut produire des bords irréguliers ou une apparence approximative dans l’image finale. Il existe toutefois une méthode de calcul peu coûteuse pour le filtrage de textures. Le filtrage avec mipmaps produit généralement de meilleurs résultats, en particulier lorsqu’il est combiné avec le filtrage anisotropique. Toutefois, il requiert davantage de mémoire des techniques prises en charge par Direct3D.

## <a name="span-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspantypes-of-texture-filtering"></a><span id="Types-of-texture-filtering"></span><span id="types-of-texture-filtering"></span><span id="TYPES-OF-TEXTURE-FILTERING"></span>Types de filtrage de texture


Direct3D prend en charge les approches de filtrage de textures suivantes.

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
<td align="left"><p><a href="nearest-point-sampling.md">Le plus proche de point d’échantillonnage</a></p></td>
<td align="left"><p>Les applications ne sont pas tenues d’utiliser le filtrage de textures. Direct3D peut être configuré pour calculer l’adresse des texels, qui souvent ne correspond pas à des entiers, et pour copier la couleur des texels avec l’adresse à valeur entière la plus proche. Ce processus est appelé <em>échantillonnage des points les plus proches</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="bilinear-texture-filtering.md">Filtrage de texture bilinéaire</a></p></td>
<td align="left"><p>Le <em>filtrage bilinéaire</em> calcule la moyenne pondérée des 4 éléments de texture les plus proches du point d’échantillonnage. Cette approche de filtrage est plus précise et courante que le filtrage des points les plus proches. Cette approche est efficace car elle est implémentée dans le matériel graphique moderne.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="anisotropic-texture-filtering.md">Filtrage de texture ANISOTROPIQUE</a></p></td>
<td align="left"><p>L'<em>anisotropie</em> est la distorsion visible dans les éléments de texture d’un objet 3D dont la surface est orientée à un certain angle par rapport au plan de l’écran. Un pixel d’une primitive anisotropique apparaît déformé lorsqu'il est mappé à des texels.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering-with-mipmaps.md">Filtrage avec mipmaps de texture</a></p></td>
<td align="left"><p>Un <em>mipmap</em> est une séquence de textures, chacune étant une représentation de résolution progressivement inférieure de la même image. La hauteur et la largeur de chaque image, ou le niveau dans le mipmap sont deux fois plus petits que le niveau précédent.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Textures](textures.md)

 

 




