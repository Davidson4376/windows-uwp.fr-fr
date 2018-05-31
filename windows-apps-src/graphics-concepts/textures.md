---
title: Textures
description: Les textures sont un puissant outil d’amélioration du réalisme des images3D générées par ordinateur. Direct3D prend en charge un ensemble complet de fonctionnalités de texture, grâce auquel les développeurs accèdent facilement à des techniques avancées d’application de textures.
ms.assetid: B9E85C9E-B779-4852-9166-6FA2240B7046
keywords:
- Textures
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c8d48114a6d4bc7386951264a5fc94990bd0b7f5
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653038"
---
# <a name="textures"></a>Textures


Les textures sont un puissant outil d’amélioration du réalisme des images3D générées par ordinateur. Direct3D prend en charge un ensemble complet de fonctionnalités de texture, grâce auquel les développeurs accèdent facilement à des techniques avancées d’application de textures.

Pour améliorer vos performances, vous pouvez envisager d’utiliser des textures dynamiques. Une texture dynamique peut être verrouillée, écrite et déverrouillée sur chaque image.

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
<td align="left"><p><a href="introduction-to-textures.md">Introduction aux textures</a></p></td>
<td align="left"><p>Une ressource de texture est une structure de données permettant de stocker des texels, qui représentent la plus petite unité d’une texture pouvant être lue ou écrite. Lorsque la texture est lue par un nuanceur, elle peut être filtrée par des échantillons de textures.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="basic-texturing-concepts.md">Concepts de texture de base</a></p></td>
<td align="left"><p>Les premières images 3D générées par ordinateur, bien qu’assez sophistiquées pour leur époque, avaient tendance à présenter un aspect plastique brillant. Il leur manquait les marques (rayures, fissures, empreintes digitales et traînées) qui donnent aux objets 3D une complexité visuelle réaliste. Les textures sont connues pour améliorer le réalisme des images 3D générées par ordinateur.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-addressing-modes.md">Modes d’adressage de texture</a></p></td>
<td align="left"><p>Votre application Direct3D peut attribuer des coordonnées de texture à tous les sommets d’une primitive. Les coordonnées de texture u et v que vous attribuez à un sommet sont généralement comprises entre 0,0 à 1,0inclus. Si vous attribuez des coordonnées de texture en dehors de cette plage, vous pouvez créer des effets de texture spéciaux.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering.md">Filtrage de textures</a></p></td>
<td align="left"><p>Le filtrage de textures génère une couleur pour chaque pixel dans l’image rendue en 2D de la primitive lorsqu’une primitive est affichée en mappant une primitive 3D sur un écran 2D.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-resources.md">Ressources de texture</a></p></td>
<td align="left"><p>Les textures sont un type de ressource utilisé pour le rendu.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-wrapping.md">Habillage de texture</a></p></td>
<td align="left"><p>L’habillage de texture modifie la méthode de base avec laquelle Direct3D rastérise des polygones avec texture à l’aide de coordonnées de texture spécifiées pour chaque sommet. Lors de la rastérisation d’un polygone, le système effectue une interpolation entre les coordonnées de texture à chaque sommet du polygone pour déterminer les texels devant être utilisés pour chaque pixel du polygone.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-blending.md">Fusion de textures</a></p></td>
<td align="left"><p>Direct3D peut fusionner huit textures sur des primitives dans une seule passe. L’utilisation de plusieurs fusions de textures peut augmenter de manière significative la fréquence d’images d’une application Direct3D. Une application utilise plusieurs fusions de textures pour appliquer des textures, des ombres, des éclairages spéculaires, des éclairages diffus et autres effets spéciaux dans une seule passe.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="light-mapping-with-textures.md">Mappage lumineux avec textures</a></p></td>
<td align="left"><p>Un mappage lumineux est une texture ou un groupe de textures contenant des informations sur l’éclairage dans une scène en 3D. Les mappages lumineux définissent des zones de lumière et d’ombre sur les primitives. La fusion de textures multiples et multipasse permet à votre application d’effectuer un rendu des scènes avec une apparence plus réaliste que les techniques d’ombrage.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compressed-texture-resources.md">Ressources de texture compressées</a></p></td>
<td align="left"><p>Les mappages de textures sont des images numérisées dessinées sur des formes 3D pour ajouter des détails visuels. Ils sont associés à ces formes lors de la rastérisation et le processus peut consommer de grandes quantités de bus système et de mémoire. Pour réduire la quantité de mémoire consommée par les textures, Direct3D prend en charge la compression des surfaces de texture. Certains appareils Direct3D prennent en charge des surfaces de texture compressées en mode natif.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Guide d’apprentissage des graphiques Direct3D](index.md)

 

 




