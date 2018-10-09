---
title: Accès du pipeline aux ressources de diffusion en continu
description: Les ressources de diffusion en continu peuvent être utilisés dans les vues de ressource de nuanceur (SRV), les vues de cible de rendu (RTV), les vues de gabarit/profondeur (DSV) et les affichages des accès sans ordre (UAV), ainsi que dans certains points de liaison où les vues ne sont pas utilisées, notamment les liaisons de tampon de sommet.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- Accès du pipeline aux ressources de diffusion en continu
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b650188f6e4306a8dbdaae58e333128408d0f076
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: Auto
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652698"
---
# <a name="pipeline-access-to-streaming-resources"></a>Accès du pipeline aux ressources de diffusion en continu


Les ressources de diffusion en continu peuvent être utilisés dans les vues de ressource de nuanceur (SRV), les vues de cible de rendu (RTV), les vues de gabarit/profondeur (DSV) et les affichages des accès sans ordre (UAV), ainsi que dans certains points de liaison où les vues ne sont pas utilisées, notamment les liaisons de tampon de sommet. Pour obtenir la liste des liaisons prises en charge, consultez l’article [Paramètres de création de ressources de diffusion en continu](streaming-resource-creation-parameters.md). Les diverses opérations de copie de D3D fonctionnent également sur les ressources de diffusion en continu.

Si plusieurs coordonnées de vignettes dans une ou plusieurs vues sont liées au même emplacement mémoire, les lectures et écritures à partir de chemins d’accès différents vers la même mémoire se produisent dans un ordre non déterministe et non reproductible d’accès mémoire.

Si toutes les vignettes situées derrière l’encombrement d’accès mémoire d’un nuanceur sont mappées vers des vignettes uniques, le comportement est identique sur toutes les implémentations à la surface possédant les mêmes contenus de mémoire, mais non découpés en vignettes.

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
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">Comportement de la SRV avec les vignettes non mappées</a></p></td>
<td align="left"><p>Le comportement des lectures d’une vue de ressource du nuanceur (SRV) impliquant des vignettes non mappées varie selon le niveau de prise en charge matérielle.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">Comportement de l’UAV avec les vignettes non mappées</a></p></td>
<td align="left"><p>Le comportement des lectures et écritures d’un accès sans ordre (UAV) varie selon le niveau de prise en charge du matériel.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">Comportement du rastériseur avec les vignettes non mappées</a></p></td>
<td align="left"><p>Cette section décrit le comportement du rastériseur avec les vignettes non mappées.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">Restrictions d’accès aux vignettes avec des mappages en double</a></p></td>
<td align="left"><p>Il existe des restrictions d’accès aux vignettes avec des mappages en double, par exemple en cas de copie de ressources de diffusion en continu avec une source et une destination se superposant, ou lors du rendu de tuiles partagées dans la zone de rendu.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">Fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu</a></p></td>
<td align="left"><p>Les fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu incluent l’obtention des commentaires sur l’état du nuanceur concernant les zones mappées, la possibilité de vérifier que toutes les données auxquelles vous accédez ont été mappées dans la ressource, l’utilisation du déplacement de couleur pour aider les nuanceurs à éviter les zones mipmappées dans les ressources de diffusion en continu connues pour ne pas être mappées, et la détection du niveau de détail minimal mappé entièrement pour un encombrement de filtre de texture entier.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">Exposition des ressources de diffusion en continu HLSL</a></p></td>
<td align="left"><p>La prise en charge des ressources de diffusion en continu dans <a href="https://msdn.microsoft.com/library/windows/desktop/ff471356">Shader Model5</a> requiert une syntaxe Microsoft HLSL (High Level Shader Language, langage de nuanceur de haut niveau) spécifique.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de diffusion en continu](streaming-resources.md)

 

 




