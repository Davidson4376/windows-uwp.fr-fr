---
title: Mappage lumineux avec textures
description: Un mappage lumineux est une texture ou un groupe de textures contenant des informations sur l’éclairage dans une scène en 3D.
ms.assetid: 5C7518D2-AC92-4A97-B7AF-4469D213D7BD
keywords:
- Mappage lumineux avec textures
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6b5d245247d33f3c04839620615f2778ef7dfb59
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8465744"
---
# <a name="light-mapping-with-textures"></a>Mappage lumineux avec textures


Un mappage lumineux est une texture ou un groupe de textures contenant des informations sur l’éclairage dans une scène en 3D. Les mappages lumineux définissent des zones de lumière et d’ombre sur les primitives. La fusion de textures multiples et multipasse permet à votre application d’effectuer un rendu des scènes avec une apparence plus réaliste qu’avec les techniques d’ombrage.

Pour qu’une application retranscrive de manière réaliste une scène 3D, elle doit tenir compte de l’effet des sources de lumière sur l’apparence de la scène. Bien que les techniques telles que l’ombrage Gouraud s’avèrent être des outils utiles sur ce plan, elles ne sont pas forcément suffisantes. Direct3D prend en charge la fusion de textures multiples et multipasse. Grâce à ces fonctionnalités, votre application octroie une apparence plus réaliste à vos scènes, par rapport à l’utilisation exclusive des techniques d’ombrage. En appliquant un ou plusieurs mappages lumineux, votre application peut mapper des zones de lumière et d’ombre sur ses primitives.

Un mappage lumineux est une texture ou un groupe de textures contenant des informations sur l’éclairage dans une scène en 3D. Vous pouvez stocker les informations d’éclairage dans les valeurs alpha du mappage lumineux, dans les valeurs de couleur, ou dans les 2.

Si vous implémentez le mappage lumineux via la fusion de textures multipasse, votre application doit rendre le mappage lumineux sur ses primitives durant la première passe. Une seconde passe est alors utilisée pour rendre la texture de base. Le mappage de lumière spéculaire constitue une exception à cette pratique. Dans ce cas, la texture de base est rendue dans un premier temps, avant le mappage lumineux.

La fusion de textures multiples permet à votre application d’effectuer le rendu du mappage lumineux et de la texture de base en une seule passe. Si le matériel de l’utilisateur prend en charge la fusion de textures multiples, votre application doit valoriser cette fonctionnalité durant l’exécution du mappage lumineux. Cela améliore considérablement les performances de votre application.

En s’appuyant sur les mappages lumineux, une application Direct3D peut mettre en œuvre divers effets d’éclairage durant le rendu des primitives. Elle peut non seulement mapper les lumières monochromes et polychromes d’une scène, mais ajouter des détails tels que des surbrillances spéculaires ou un éclairage diffus.

Des détails sur l’utilisation de la fusion de texturesDirect3D pour exécuter le mappage lumineux sont présentés dans les rubriques suivantes.

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
<td align="left"><p><a href="monochrome-light-maps.md">Mappages lumineux monochromes</a></p></td>
<td align="left"><p>Les mappages lumineux monochromes permettent à des cartes plus anciennes d’exécuter des fusions de textures multipasse, lorsqu’une carte d’accélérateur3D ne prend pas en charge la fusion de textures à l’aide de la valeur alpha du pixel de destination.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="color-light-maps.md">Mappages de lumière en couleur</a></p></td>
<td align="left"><p>Un mappage de lumière en couleur utilise les données RVB dans le mappage lumineux pour ses informations d’éclairage. Une application qui utilise des mappages de lumière en couleur produit généralement des scènes 3D plus réalistes.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-light-maps.md">Mappages de lumière spéculaire</a></p></td>
<td align="left"><p>Lorsqu’ils sont éclairés par une source lumineuse, les objets brillants présentant des matériaux hautement réfléchissants reçoivent des surbrillances spéculaires. Parfois, il est possible d’obtenir des surbrillances plus précises en appliquant des mappages de lumière spéculaire sur les primitives, plutôt qu’en utilisant des surbrillances spéculaires produites par le module d’éclairage.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-light-maps.md">Mappages de lumière diffuse</a></p></td>
<td align="left"><p>Les surfaces mates présentent une réflexion lumineuse diffuse. La luminosité de la lumière diffuse dépend de la distance entre la source de lumière et l’angle formé par la normale de surface avec le vecteur de direction de la source de lumière. Les textures de luminosité peuvent simuler un éclairage diffus complexe.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Textures](textures.md)

 

 




