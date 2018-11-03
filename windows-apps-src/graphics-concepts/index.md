---
title: Glossaire de graphiques Direct3D
description: Définit les termes de graphiques utilisées par Microsoft Direct3D.
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- Glossaire de graphiques Direct3D
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0e325494cefeab4cf3ed40939f5b24de5e921b68
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5986045"
---
# <a name="direct3d-graphics-glossary"></a>Glossaire de graphiques Direct3D


Définit les termes de graphiques de Microsoft Direct3D. Ce glossaire définit, à un termes de graphiques de haut niveau, général informatiques 3D qui sont utilisés dans le développement de jeux et d’applications Direct3D.

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
<td align="left"><p>La programmation des applicationsDirect3D nécessite une maîtrise des principes géométriques3D. Cette section présente les concepts géométriques les plus importants associés à la création de scènes3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-and-index-buffers.md">Mémoires tampons de vertex et d’index</a></p></td>
<td align="left"><p>Les <em>mémoires tampons de vertex</em> comportent des données de vertex; les vertex sont traités dans le cadre des opérations de transformation, d'éclairage et de découpage. Les <em>mémoires tampons d’index</em> sont des mémoires tampons qui contiennent des données d’index, lesquelles sont des décalages entiers dans des mémoires tampons de vertex, utilisés pour le rendu des primitives.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="devices.md">Appareils</a></p></td>
<td align="left"><p>Un périphérique Direct3D est le composant de rendu de Direct3D. Un appareil encapsule et stocke l’état de rendu, exécute des transformations, des opérations d’éclairage et rastérise une image sur une surface.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="lights-and-materials.md">Éclairage</a></p></td>
<td align="left"><p>Les lumières sont utilisées pour illuminer les objets d’une scène. La couleur de chaque vertex d’objet est basée sur la carte de texture, les couleurs de vertex et les sources de lumière actuelles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="depth-and-stencil-buffers.md">Mémoires tampons de profondeurs et de gabarits</a></p></td>
<td align="left"><p>Un <em>tampon de profondeur</em> stocke des informations de profondeur afin de définir les zones de polygones à afficher, plutôt que celles à masquer. Un <em>tampon stencil buffer</em> est utilisé pour masquer les pixels d’une image et pour produire des effets spéciaux, notamment la composition, le transfert, la dissolution, le fondu et le balayage, le contour et la silhouette, ainsi que le stencil recto verso.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures.md">Textures</a></p></td>
<td align="left"><p>Les textures sont un puissant outil d’amélioration du réalisme des images3D générées par ordinateur. Direct3D prend en charge un ensemble complet de fonctionnalités de texture, grâce auquel les développeurs accèdent facilement à des techniques avancées d’application de textures.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="graphics-pipeline.md">Pipeline graphique</a></p></td>
<td align="left"><p>Le pipeline graphiqueDirect3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données circulent de l’entrée à la sortie, via chacune des étapes configurables ou programmables.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="views.md">Affichages</a></p></td>
<td align="left"><p>Le terme «affichage» est utilisé pour désigner des «données au format requis». Par exemple, un affichage des mémoires tampons de constantes correspond à des données correctement formatées de mémoire tampon de constante. Cette section décrit les affichages les plus courants et les plus utiles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compute-pipeline.md">Pipeline de calcul</a></p></td>
<td align="left"><p>Le pipeline de calculDirect3D est conçu pour traiter les calculs pouvant être effectués principalement en parallèle du pipeline graphique.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="resources.md">Ressources</a></p></td>
<td align="left"><p>Une ressource est une zone de mémoire accessible par le pipeline Direct3D. Pour permettre un accès efficace du pipeline à la mémoire, les données fournies au pipeline (géométrie d’entrée, ressources des nuanceurs et textures) doivent être stockées dans une ressource. Il existe 2types de ressources à partir desquelles l’ensemble des ressources Direct3D dérivent: une mémoire tampon et une texture. Jusqu’à 128ressources peuvent être actives à chaque étape du pipeline.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources.md">Ressources de diffusion en continu</a></p></td>
<td align="left"><p>Les <em>ressources de diffusion en continu</em> sont de grandes ressources logiques utilisant de petites quantités de mémoire physique. En lieu et place de l’affichage d’une grande ressource dans son intégralité, de petites portions de la ressource sont diffusées en continu. Les ressources de diffusion en continu étaient auparavant appelées des <em>ressources sous forme de vignettes</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="appendix.md">Annexes</a></p></td>
<td align="left"><p>Les sections suivantes fournissent des informations techniques détaillées.</p></td>
</tr>
</tbody>
</table>

 

 

 
