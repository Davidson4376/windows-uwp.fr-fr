---
title: "Niveaux de fonctionnalité des ressources de diffusion en continu"
description: "Direct3D prend en charge les ressources de diffusion en continu sur trois niveaux de fonctionnalités."
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords: "Niveaux de fonctionnalité des ressources de diffusion en continu"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 671c679938860057a5fe8f083eebfe47dc518e05
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="streaming-resources-features-tiers"></a>Niveaux de fonctionnalité des ressources de diffusion en continu


Direct3D prend en charge les ressources de diffusion en continu sur trois niveaux de fonctionnalités.

Le niveau1 fournit des fonctionnalités de base pour les ressources de diffusion en continu.

La niveau2 ajoute des fonctionnalités par rapport au niveau1, telles que la garantie d'un mipmap de texture non compressé lorsque la taille est au minimum de forme de tuile standard, les instructions de nuanceur pour le niveau de détail Clamp et pour obtenir l'état de l’opération du nuanceur et la lecture à partir de vignettes mappées NULL qui ont échantillonné une valeur de zéro.

Le niveau3 ajoute des fonctionnalités Texture3D, au-delà du niveau2.

Les fonctions de requête sont disponibles dans les versions de Direct3D, pour valider la prise en charge du matériel et des pilotes pour les ressources de diffusion en continu selon le niveau.

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
<td align="left"><p>[Niveau1](tier-1.md)</p></td>
<td align="left"><p>Cette section décrit la prise en charge du niveau1.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Niveau2](tier-2.md)</p></td>
<td align="left"><p>La prise en charge du niveau2 pour les ressources de diffusion en continu ajoute des fonctionnalités par rapport au niveau1, telles que la garantie d'un mipmap de texture non compressé lorsque la taille est au minimum de forme de tuile standard, les instructions de nuanceur pour le niveau de détail Clamp et pour obtenir l'état de l’opération du nuanceur et la lecture à partir de vignettes mappées NULL qui ont échantillonné une valeur de zéro.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Niveau3](tier-3.md)</p></td>
<td align="left"><p>Le niveau3 comprend la prise en charge de Texture3D pour les ressources de diffusion en continu, en plus des capacités de [niveau2](tier-2.md) .</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de diffusion en continu](streaming-resources.md)

 

 




