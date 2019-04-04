---
title: Glossaire relatif au graphisme Direct3D
description: Définit les termes relatifs au graphisme utilisé par Microsoft Direct3D.
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- Glossaire relatif au graphisme Direct3D
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3cb6a2466ea201c9b5047f7eb159477a0d584429
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57582698"
---
# <a name="direct3d-graphics-glossary"></a>Glossaire relatif au graphisme Direct3D


Définit les termes relatifs au graphisme Microsoft Direct3D. Ce glossaire définit, à un niveau élémentaire, les termes généraux d’infographie 3D utilisés dans le développement d’applications et de jeux Direct3D.

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
<td align="left"><p><a href="coordinate-systems-and-geometry.md">Systèmes de coordonnées et géométrie</a></p></td>
<td align="left"><p>La programmation d’applications Direct3D nécessite une maîtrise des principes géométriques liés à la 3D. Cette section présente les concepts géométriques les plus importants associés à la création de scènes 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-and-index-buffers.md">Tampons vertex et tampons d’index</a></p></td>
<td align="left"><p>Les <em>tampons vertex</em> comportent des données vertex ; les vertex d’un tampon vertex sont traités dans le cadre des opérations de transformation, d’éclairage et de découpage. Les <em>tampons d’index</em> contiennent des données d’index, lesquelles sont des décalages d’entiers dans des tampons vertex. Ils sont utilisés pour le rendu des primitives.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="devices.md">Appareils</a></p></td>
<td align="left"><p>Un périphérique Direct3D est le composant de rendu de Direct3D. Un périphérique encapsule et stocke l’état de rendu, effectue des transformations et des opérations d’éclairage, et rastérise une image sur une surface.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="lights-and-materials.md">Éclairage</a></p></td>
<td align="left"><p>Les lumières sont utilisées pour illuminer les objets d’une scène. La couleur de chaque vertex d’objet est basée sur la carte de texture, les couleurs de vertex et les sources de lumière actuelles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="depth-and-stencil-buffers.md">Tampons de profondeur et stencil buffer</a></p></td>
<td align="left"><p>Un <em>tampon de profondeur</em> stocke des informations de profondeur pour définir les zones de polygones à afficher, plutôt que celles à masquer. Un <em>tampon stencil buffer</em> est utilisé pour masquer les pixels d’une image et pour produire des effets spéciaux, notamment la composition, le transfert, la dissolution, le fondu et le balayage, le contour et la silhouette ainsi que le stencil recto verso.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures.md">Textures</a></p></td>
<td align="left"><p>Les textures sont un puissant outil d’amélioration du réalisme des images 3D générées par ordinateur. Direct3D prend en charge un ensemble complet de fonctionnalités de texture, ce qui permet aux développeurs d’accéder facilement à des techniques avancées d’application de textures.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="graphics-pipeline.md">Pipeline graphique</a></p></td>
<td align="left"><p>Le pipeline graphique Direct3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données circulent de l’entrée vers la sortie en transitant par chacune des phases configurables ou programmables de ce pipeline.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="views.md">Vues</a></p></td>
<td align="left"><p>Le terme &quot;vue&quot; sert à désigner des &quot;données au format approprié&quot;. Par exemple, une vue de mémoire tampon constante (CBV) correspond à des données de mémoire tampon constante correctement formatées. Cette section décrit les vues les plus courantes et les plus utiles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compute-pipeline.md">Pipeline de calcul</a></p></td>
<td align="left"><p>Le pipeline de calcul Direct3D est conçu pour traiter les calculs pouvant être effectués principalement en parallèle du pipeline graphique.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="resources.md">Ressources</a></p></td>
<td align="left"><p>Une ressource est une zone de mémoire accessible au pipeline Direct3D. Pour permettre un accès efficace du pipeline à la mémoire, les données fournies au pipeline (géométrie d’entrée, ressources des nuanceurs et textures) doivent être stockées dans une ressource. Il existe deux types de ressource dont dérivent l’ensemble des ressources Direct3D : la mémoire tampon et la texture. Jusqu’à 128 ressources peuvent être actives à chaque phase du pipeline.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources.md">Ressources de streaming</a></p></td>
<td align="left"><p>Les <em>ressources de streaming</em> sont de grandes ressources logiques qui utilisent de petites quantités de mémoire physique. Pour éviter le passage d’une grande ressource dans son intégralité, de petites parties de cette ressource font l’objet d’un streaming selon les besoins. Les ressources de streaming s’appelaient <em>ressources sous forme de vignettes</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="appendix.md">Annexes</a></p></td>
<td align="left"><p>Les sections suivantes fournissent des informations techniques détaillées.</p></td>
</tr>
</tbody>
</table>

 

 

 
