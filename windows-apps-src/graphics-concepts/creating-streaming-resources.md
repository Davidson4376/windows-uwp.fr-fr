---
title: Création de ressources de diffusion en continu
description: Les ressources de diffusion en continu sont créées en spécifiant un indicateur au moment de la création, pour indiquer que la ressource est une ressource de diffusion en continu.
ms.assetid: B3F3E43C-54D4-458C-9E16-E13CB382C83F
keywords:
- Création de ressources de diffusion en continu
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: d78c6e070dd5d29a8e615f70f67507830e5f3938
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653088"
---
# <a name="creating-streaming-resources"></a>Création de ressources de diffusion en continu


Les ressources de diffusion en continu sont créées en spécifiant un indicateur au moment de la création, pour indiquer que la ressource est une ressource de diffusion en continu.

Les restrictions applicables à la création d'une ressource de diffusion en continu sont décrites dans l'article [Paramètres de création de ressources de diffusion en continu](streaming-resource-creation-parameters.md).

Le stockage d’une ressource autre que celles de diffusion en continu est alloué dans le système graphique lors de la création de la ressource (par exemple, allocation d’un tableau de textures 2D).

Lorsqu'une ressource de diffusion en continu est créée, le système graphique n’alloue pas le stockage pour le contenu de la ressource. Au lieu de cela, lorsqu’une application crée une ressource de diffusion en continu, le système graphique effectue une réservation d’espace d'adresse uniquement pour la zone de la surface de vignettes, avant de laisser l'application contrôler le mappage des vignettes. Le «mappage» d’une vignette désigne simplement l’emplacement physique dans la mémoire vers lequel pointe une vignette logique d'une ressource (ou **NULL** pour une vignette non mappée).

Ne confondez pas ce concept avec la notion de mappage d’une ressource Direct3D pour l’accès UC, qui est un processus totalement indépendant bien qu'il porte le même nom. Vous serez en mesure de définir et modifier si besoin le mappage de chaque vignette individuellement, en sachant qu'il n'est pas nécessaire de mapper simultanément toutes les vignettes d’une surface (ce qui permet d'utiliser efficacement la quantité de mémoire disponible).

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
<td align="left"><p><a href="mappings-are-into-a-tile-pool.md">Mappages dans un pool de vignettes</a></p></td>
<td align="left"><p>Lorsque qu'une ressource est créée en tant que ressource de diffusion en continu, les vignettes qui la constituent proviennent du pointage dans des emplacements d'un pool de vignettes. Un pool de vignettes est un pool de mémoire (pris en charge par une ou plusieurs allocations masquées, auxquelles l'application n'a pas accès).</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-creation-parameters.md">Paramètres de création de ressources de diffusion en continu</a></p></td>
<td align="left"><p>Il existe certaines contraintes quant au type de ressources Direct3D que vous pouvez créer en tant que ressource de diffusion en continu.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tile-pool-creation-parameters.md">Paramètres de création d’un pool de vignettes</a></p></td>
<td align="left"><p>Utilisez les paramètres de cette section pour définir des pools de vignettes lors de la création d’une mémoire tampon.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-cross-process-and-device-sharing.md">Partage de ressources de diffusion en continu avec des processus et des appareils</a></p></td>
<td align="left"><p>Les pools de vignettes peuvent être partagés avec d’autres processus comme n'importe quelle ressource traditionnelle. Les ressources de diffusion en continu qui font référence à des pools de vignettes ne peuvent être partagées entre les appareils et les processus.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="operations-available-on-streaming-resources.md">Opérations disponibles sur les ressources de diffusion en continu</a></p></td>
<td align="left"><p>Cette section répertorie les opérations pouvant être effectuées sur les ressources de diffusion en continu.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="operations-available-on-tile-pools.md">Opérations disponibles sur les pools de vignettes</a></p></td>
<td align="left"><p>Vous pouvez redimensionner un pool de vignettes, offrir des ressources (octroi temporaire de mémoire au système pour l'intégralité du pool de vignettes) et récupérer des ressources.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="how-a-streaming-resource-s-area-is-tiled.md">Restitution de la surface d’une ressource de diffusion en continu sous forme de vignettes</a></p></td>
<td align="left"><p>Lorsque vous créez une ressource de diffusion en continu, les dimensions, la taille des éléments de format et le nombre de mipmaps et/ou de sections de tableau (le cas échéant) déterminent le nombre de vignettes requises pour couvrir toute la zone de la surface.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de diffusion en continu](streaming-resources.md)

 

 




