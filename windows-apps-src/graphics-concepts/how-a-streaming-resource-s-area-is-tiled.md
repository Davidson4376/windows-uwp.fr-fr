---
title: "Restitution de la surface d’une ressource de diffusion en continu sous forme de vignettes"
description: "Lorsque vous créez une ressource de diffusion en continu, les dimensions, la taille des éléments de format et le nombre de mipmaps et/ou de sections de tableau (le cas échéant) déterminent le nombre de vignettes requises pour couvrir toute la zone de la surface."
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- "Restitution de la surface d’une ressource de diffusion en continu sous forme de vignettes"
- "zone de ressource, découpage en vignettes"
- "tableaux de tailles, ressources, découpage en vignettes"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 257522f8d00596aa6bb60a87277d4e1a5ccdcf89
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>Restitution de la surface d’une ressource de diffusion en continu sous forme de vignettes


Lorsque vous créez une ressource de diffusion en continu, les dimensions, la taille des éléments de format et le nombre de mipmaps et/ou de sections de tableau (le cas échéant) déterminent le nombre de vignettes requises pour couvrir toute la zone de la surface. La disposition des pixels/octets au sein des vignettes est déterminée par l’implémentation. Le nombre de pixels qui tiennent dans une vignette, selon la taille des éléments de format, est fixe et identique, que vous effectuiez ou non un réagencement standard des données en surface.

Le nombre de vignettes qui seront utilisées par une taille de surface et une largeur d’élément de format spécifiques est parfaitement défini et prévisible grâce aux tableaux figurant dans les articles répertoriés ci-dessous. Pour les ressources qui contiennent des mipmaps ou pour les cas dans lesquels les dimensions de surface ne remplissent pas une vignette, il existe certaines contraintes; pour plus d’informations, consultez l’article [Compression de mipmaps](mipmap-packing.md).

Différentes ressources de diffusion en continu peuvent pointer sur une mémoire identique présentant différents formats, à condition que les applications ne s’appuient pas sur les résultats d’écriture en mémoire dans un format spécifique et sur les résultats de lecture en mémoire dans un autre format. Toutefois, les applications peuvent se fonder sur les résultats d’écriture en mémoire dans un format et sur les résultats de lecture dans un autre format si ces formats appartiennent à la même famille (autrement dit, s’ils ont le même format parent sans type). Par exemple, DXGI\_FORMAT\_R8G8B8A8\_UNORM et DXGI\_FORMAT\_R8G8B8A8\_UINT sont compatibles l’un avec l’autre, mais non avec DXGI\_FORMAT\_R16G16\_UNORM.

Il existe une exception bien définie à cette règle dans le cas où des données d’un format sont fusionnées dans un autre format: si tous les bits d’une vignette se composent uniquement de 0, cette vignette peut être utilisée avec n’importe quel format interprétant ce contenu en mémoire en tant que 0 (quelle que soit la configuration mémoire). Ainsi, si une vignette était écrasée par 0x00 avec le format DXGI\_FORMAT\_R8\_UNORM, puis utilisée avec un format tel que DXGI\_FORMAT\_R32G32\_FLOAT, elle continuerait de présenter le contenu (0.0f,0.0f).

La disposition des données dans une vignette ne dépend pas de l’emplacement de mappage de la vignette dans une ressource globale. Ceci permet donc par exemple de réutiliser une vignette à différents emplacements d’une surface en une seule opération et d’obtenir systématiquement un comportement cohérent.

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
<td align="left"><p>[Restitution des sous-ressources Texture2D et Texture2DArray sous forme de vignettes](texture2d-and-texture2darray-subresource-tiling.md)</p></td>
<td align="left"><p>Les tableaux fournis dans cet article décrivent la façon dont les sous-ressources [<strong>Texture2D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471525) et [<strong>Texture2DArray</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471526) sont découpées en vignettes.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Restitution de la sous-ressource Texture3D sous forme de vignettes](texture3d-subresource-tiling.md)</p></td>
<td align="left"><p>Le tableau fourni dans cet article décrit la façon dont les sous-ressources [<strong>Texture3D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471562) sont découpées en vignettes.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Découpage de mémoires tampons en vignettes](buffer-tiling.md)</p></td>
<td align="left"><p>Une ressource de [mémoire tampon](introduction-to-buffers.md) est divisée en vignettes de 64Ko, avec un espace vide dans la dernière vignette si la taille n’est pas un multiple de 64Ko.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Compression de mipmaps](mipmap-packing.md)</p></td>
<td align="left"><p>Il est possible de compresser plusieurs mips (par section de tableau) dans différentes vignettes, en fonction des dimensions et du format de la ressource de diffusion en continu, du nombre de mipmaps et des sections de tableau.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Création de ressources de diffusion en continu](creating-streaming-resources.md)

 

 




