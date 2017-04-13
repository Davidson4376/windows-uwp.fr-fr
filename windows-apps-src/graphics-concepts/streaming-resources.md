---
title: Ressources de diffusion en continu
description: "Les ressources de diffusion en continu sont de grandes ressources logiques utilisant de petites quantités de mémoire physique. En lieu et place de l’affichage d’une grande ressource dans son intégralité, de petites portions de la ressource sont diffusées en continu. Les ressources de diffusion en continu étaient auparavant appelées des ressources sous forme de vignettes."
ms.assetid: 04F0486E-4B71-4073-88DA-2AF505F4F0C1
keywords:
- Ressources de diffusion en continu
- ressources, diffusion en continu
- ressources, sous forme de vignettes
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: f45c1d955d901366b0160d148cef88ce015aeaa2
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="streaming-resources"></a>Ressources de diffusion en continu


Les *ressources de diffusion en continu* sont de grandes ressources logiques utilisant de petites quantités de mémoire physique. En lieu et place de l’affichage d’une grande ressource dans son intégralité, de petites portions de la ressource sont diffusées en continu. Les ressources de diffusion en continu étaient auparavant appelées des *ressources sous forme de vignettes*.

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
<td align="left"><p>[Besoin en ressources de diffusion en continu](the-need-for-streaming-resources.md)</p></td>
<td align="left"><p>Les ressources de diffusion en continu sont nécessaires, afin que la mémoire GPU ne gaspille pas d'espace en stockant des régions de surfaces qui ne sont pas accessibles et pour indiquer au matériel comment filtrer les vignettes adjacentes.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Création de ressources de diffusion en continu](creating-streaming-resources.md)</p></td>
<td align="left"><p>Les ressources de diffusion en continu sont créées en spécifiant un indicateur au moment de la création, pour indiquer que la ressource est une ressource de diffusion en continu.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Accès du pipeline aux ressources de diffusion en continu](pipeline-access-to-streaming-resources.md)</p></td>
<td align="left"><p>Les ressources de diffusion en continu peuvent être utilisés dans les vues de ressource de nuanceur (SRV), les vues de cible de rendu (RTV), les vues de gabarit/profondeur (DSV) et les affichages des accès sans ordre (UAV), ainsi que dans certains points de liaison où les vues ne sont pas utilisées, notamment les liaisons de tampon de sommet.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Niveaux de fonctionnalité des ressources de diffusion en continu](streaming-resources-features-tiers.md)</p></td>
<td align="left"><p>Direct3D prend en charge les ressources de diffusion en continu sur trois niveaux de fonctionnalités.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage des graphismes Direct3D](index.md)

[Ressources](resources.md)

 

 




