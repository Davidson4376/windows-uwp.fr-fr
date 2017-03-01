---
title: "Guide d’apprentissage des graphiques Direct3D"
description: "Décrit les concepts graphiques sur lesquels reposent Microsoft Direct3D."
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- "Guide d’apprentissage des graphiques Direct3D"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e62f9cfde35580dd384ef69fe6e5658d927ce3d8
ms.lasthandoff: 02/07/2017

---

# <a name="direct3d-graphics-learning-guide"></a>Guide d’apprentissage des graphiques Direct3D


Décrit les concepts graphiques sur lesquels reposent Microsoft Direct3D. Cette documentation, largement indépendante des versions Direct3D, est destinée aux développeurs graphiques recherchant à compléter les informations générales fournies dans les ressources d’API spécifiques aux versions.

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
<td align="left"><p>[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)</p></td>
<td align="left"><p>La programmation des applications Direct3D nécessite une maîtrise des principes géométriques 3D. Cette section présente les concepts géométriques les plus importants associés à la création de scènes 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Mémoires tampons de vertex et d’index](vertex-and-index-buffers.md)</p></td>
<td align="left"><p><em>Les mémoires tampons de vertex</em> comportent des données de vertex ; les vertex sont traités dans le cadre des opérations de transformation, d’éclairage et de découpage. <em>Les mémoires tampons d’index</em> sont des mémoires comportant des données d’index, qui sont des décalages d’entiers dans des mémoires tampons de vertex, utilisés pour l’affichage des primitives.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Appareils](devices.md)</p></td>
<td align="left"><p>Un périphérique Direct3D est le composant de rendu de Direct3D. Un appareil encapsule et stocke l’état de rendu, exécute des transformations, des opérations d’éclairage et rastérise une image sur une surface.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Éclairage](lights-and-materials.md)</p></td>
<td align="left"><p>Les lumières sont utilisées pour illuminer les objets d’une scène. La couleur de chaque vertex d’objet est basée sur la carte de texture, les couleurs de vertex et les sources de lumière actuelles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Mémoires tampons de profondeurs et de gabarits](depth-and-stencil-buffers.md)</p></td>
<td align="left"><p>Une <em>mémoire tampon de profondeur</em> stocke des informations de profondeur afin de définir les zones de polygones à afficher, non pas celles à masquer. Une <em>mémoire tampon de gabarit</em> est utilisée pour masquer les pixels d’une image, pour produire des effets spéciaux, notamment la composition, le transfert, la dissolution, le fondu et le balayage, le contour et la silhouette, ainsi que le gabarit recto verso.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Textures](textures.md)</p></td>
<td align="left"><p>Les textures sont un puissant outil d’amélioration du réalisme des images 3D générées par ordinateur. Direct3D prend en charge un ensemble complet de fonctionnalités de texture, grâce auquel les développeurs accèdent facilement à des techniques avancées d’application de textures.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Pipeline graphique](graphics-pipeline.md)</p></td>
<td align="left"><p>Le pipeline graphique Direct3D est conçu pour générer des graphiques pour les applications de jeu en temps réel. Les données circulent de l’entrée à la sortie, via chacune des étapes configurables ou programmables.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Affichages](views.md)</p></td>
<td align="left"><p>Le terme « affichage » est utilisé pour désigner des « données au format requis ». Par exemple, un affichage des mémoires tampons de constantes correspond à des données correctement formatées de mémoire tampon de constante. Cette section décrit les affichages les plus courants et les plus utiles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Pipeline de calcul](compute-pipeline.md)</p></td>
<td align="left"><p>Le pipeline de calcul Direct3D est conçu pour traiter les calculs pouvant être effectués principalement en parallèle du pipeline graphique.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Ressources](resources.md)</p></td>
<td align="left"><p>Une ressource est une zone de mémoire accessible par le pipeline Direct3D. Pour permettre un accès efficace du pipeline à la mémoire, les données fournies au pipeline (géométrie d’entrée, ressources des nuanceurs et textures) doivent être stockées dans une ressource. Il existe 2 types de ressources à partir desquelles l’ensemble des ressources Direct3D dérivent : une mémoire tampon et une texture. Jusqu’à 128 ressources peuvent être actives à chaque étape du pipeline.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Ressources de diffusion en continu](streaming-resources.md)</p></td>
<td align="left"><p><em>Les ressources de diffusion en continu</em> sont de grandes ressources logiques utilisant de petites quantités de mémoire physique. En lieu et place de l’affichage d’une grande ressource dans son intégralité, de petites portions de la ressource sont diffusées en continu. Les ressources de diffusion en continu étaient auparavant appelées des <em>ressources sous forme de vignettes</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Annexes](appendix.md)</p></td>
<td align="left"><p>Les sections suivantes fournissent des informations techniques détaillées.</p></td>
</tr>
</tbody>
</table>

 

 

 

