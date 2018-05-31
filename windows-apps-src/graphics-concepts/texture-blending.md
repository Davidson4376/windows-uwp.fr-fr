---
title: Fusion de textures
description: Direct3D peut fusionner huit textures sur des primitives dans une seule passe.
ms.assetid: 9AD388FA-B2B9-44A9-B73E-EDBD7357ACFB
keywords:
- Fusion de textures
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1014ed205c5cf0eda2c9b71c8406a98394b1463
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652898"
---
# <a name="texture-blending"></a>Fusion de textures


Direct3D peut fusionner huit textures sur des primitives dans une seule passe. L’utilisation de plusieurs fusions de textures peut augmenter de manière significative la fréquence d’images d’une application Direct3D. Une application utilise plusieurs fusions de textures pour appliquer des textures, des ombres, des éclairages spéculaires, des éclairages diffus et autres effets spéciaux dans une seule passe.

Pour utiliser la fusion de textures, votre application doit d’abord déterminer si le matériel de l’utilisateur prend en charge cette option.

## <a name="span-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespantexture-stages-and-the-texture-blending-cascade"></a><span id="Texture-Stages-and-the-Texture-Blending-Cascade"></span><span id="texture-stages-and-the-texture-blending-cascade"></span><span id="TEXTURE-STAGES-AND-THE-TEXTURE-BLENDING-CASCADE"></span>Étapes de texture et cascade de fusion de textures


Direct3D prend en charge les fusions de textures multiples sur une seule passe par le biais des étapes de textures. Une étape de texture définit deux arguments et effectue une opération de fusion de ces arguments, en transférant le résultat pour traitement supplémentaire ou rastérisation. Vous pouvez visualiser une étape de texture comme indiqué dans le diagramme suivant.

![diagramme d’une étape de texture](images/texstg.png)

Comme indiqué dans le diagramme précédent, les étapes de texture fusionnent deux arguments à l’aide d’un opérateur spécifié. Les opérations courantes incluent généralement une modulation simple ou l’ajout de couleurs ou de composants alpha aux arguments. Cependant, plus d’une vingtaine d’opérations sont prises en charge. Les arguments d’une étape peuvent être associés à une texture, la couleur itérée ou alpha (itérée pendant l’ombrage Gouraud), une couleur arbitraire et alpha ou le résultat de l’étape de texture précédente.

**Remarque**   Direct3D distingue la fusion de couleur de la fusion alpha. Les applications définissent des opérations de fusion et des arguments de couleur et alpha de façon individuelle et les résultats de ces configurations sont indépendants les uns des autres.

 

La combinaison d’arguments et d’opérations utilisés par plusieurs étapes de fusion définissent un langage de fusion simple basé sur le flux. Les résultats d’une étape passent à l’étape suivante, et ainsi de suite. Le concept de transmission des résultats d’une étape à une autre pour éventuellement être rastérisé sur un polygone est souvent appelé la cascade de fusion de textures. Le diagramme suivant montre comment chaque étape de texture constitue une cascade de fusion de textures.

![diagramme des étapes de texture dans une cascade de fusion de textures](images/tcascade.png)

Chaque étape dans un appareil comprend un index commençant par zéro. Direct3D autorise jusqu’à huit étapes de fusion. Vous devez cependant toujours vérifier les fonctionnalités de l’appareil pour déterminer le nombre d’étapes prises en charge par le matériel. La première étape de fusion est l’index0, le deuxième est l’index1 et ainsi de suite jusqu’à l’index7. Le système fusionne les étapes dans l’ordre d’index croissant.

Utilisez uniquement le nombre d’étapes nécessaires. Les étapes de fusion inutilisées sont désactivées par défaut. Par conséquent, si votre application utilise uniquement les deux premières étapes, il suffit de définir les opérations et les arguments des phases 0 et 1. Le système fusionne les deux étapes et ignore les étapes désactivées.

Si le nombre d’étapes utilisées par votre application varie selon différentes situations, par exemple, quatre étapes pour certains objets et uniquement deux pour d’autres objets, vous n’avez pas besoin de désactiver explicitement les étapes précédemment utilisées. Une option permet de désactiver l’opération de couleur pour la première étape inutilisée, puis pour toutes les étapes ayant un index supérieur. Une autre option consiste à désactiver tout le mappage de texture en définissant l’opération de couleur pour la première étape de texture (étape0) sur l’état désactivé.

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
<td align="left"><p><a href="blending-stages.md">Étapes de fusion</a></p></td>
<td align="left"><p>Une étape de fusion désigne un ensemble d’opérations de texture et leurs arguments qui définissent la façon dont les textures sont fusionnées.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multipass-texture-blending.md">Fusion de textures multipasse</a></p></td>
<td align="left"><p>Les applications Direct3D peuvent obtenir de nombreux effets spéciaux en appliquant différentes textures à une primitive au cours de plusieurs passages de rendu. Cette opération est généralement appelée <em>fusion de textures multipasse</em>. La fusion de textures multipasse permet généralement d’émuler les effets des modèles d’éclairage et d’ombrage complexes en appliquant plusieurs couleurs à partir de plusieurs textures différentes. Une telle application est appelée <em>mappage lumineux</em>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Textures](textures.md)

 

 




