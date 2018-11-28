---
title: Topologies de primitives
description: Direct3D prend en charge plusieurs topologies de primitives, qui définissent le mode d’interprétation et de rendu des sommets par le pipeline. Il peut s’agir par exemple de listes de points, de listes de lignes ou de bandes de triangles.
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords:
- Topologies de primitives
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 85d1c41fc10f509f3872fb1e4a0af5fa1e1e7c30
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7834140"
---
# <a name="primitive-topologies"></a>Topologies de primitives


Direct3D prend en charge plusieurs topologies de primitives, qui définissent l’interprétation et le rendu des sommets par le pipeline. Il peut s’agir par exemple de listes de points, de listes de lignes ou de bandes de triangles.

## <a name="span-idprimitivetypesspanspan-idprimitivetypesspanspan-idprimitivetypesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>Topologies de primitives de base


Les topologies de primitives de base suivantes (ou les types de primitives) sont pris(es) en charge:

-   [Listes de points](point-lists.md)
-   [Listes de lignes](line-lists.md)
-   [Bandes de lignes](line-strips.md)
-   [Listes de triangles](triangle-lists.md)
-   [Bandes de triangles](triangle-strips.md)

Pour obtenir une visualisation de chaque type primitive, consultez le diagramme plus loin dans cette rubrique, dans [Sens de l’enroulement et positions du sommet principal](#winding-direction-and-leading-vertex-positions).

L’[étape de l’assembleur d’entrée(IA)](input-assembler-stage--ia-.md) lit les données à partir des tampons de sommet et d’index, assemble les données dans ces primitives, puis les envoie aux étapes du pipeline restantes.

## <a name="span-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>Voisinage des primitives


Tous les types de primitives Direct3D (à l’exception de la liste de points) sont disponibles dans deux versions: avec voisinage et sans voisinage. Les primitives avec voisinage contiennent certains des sommets adjacents, tandis que les primitives sans voisinage contiennent uniquement les sommets de la primitive cible. Par exemple, la primitive de type liste de lignes possède une primitive de type liste de lignes correspondante qui inclut le voisinage.

Les primitives adjacentes sont conçues pour fournir davantage d’informations sur la géométrie et ne sont visibles que par le biais d’un nuanceur de géométrie. Le voisinage est utile pour les nuanceurs de géométrie qui utilisent la détection de silhouettes, l’extrusion du volume d’ombre, etc.

Par exemple, supposons que vous souhaitiez dessiner une liste de triangles avec voisinage. Une liste de triangles qui contient 36sommets (avec voisinage) générera 6primitives complètes. Les primitives avec voisinage (à l’exception des bandes de lignes) contiennent exactement deux fois plus de sommets que la primitive équivalente sans voisinage, où chaque sommet supplémentaire est un sommet adjacent.

## <a name="span-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>Sens de l’enroulement et positions du sommet principal


Comme indiqué dans l’illustration suivante, un sommet principal est le premier sommet non adjacent dans une primitive. Plusieurs sommets principaux peuvent être définis pour un type de primitive, dans la mesure où chacun d’entre eux est utilisé pour une primitive différente.

-   Pour une bande de triangles avec voisinage, les sommets principaux sont 0, 2, 4, 6 et ainsi de suite.
-   Pour une bande de lignes avec voisinage, les sommets principaux sont 1, 2, 3 et ainsi de suite.
-   En revanche, une primitive adjacente n’a pas de sommet principal.

L’illustration suivante montre le classement des sommets pour tous les types de primitives que peut produire l’assembleur d’entrée.

![diagramme du classement des sommets par type de primitive](images/d3d10-primitive-topologies.png)

Les symboles contenus dans l’illustration précédente sont décrits dans le tableau suivant.

| Symbole                                                                                   | Nom              | Description                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![symbole de sommet](images/d3d10-primitive-topologies-vertex.png)                     | Sommet            | Un point dans l’espace 3D.                                                                |
| ![symbole du sens de l’enroulement](images/d3d10-primitive-topologies-winding-direction.png) | Sens de l’enroulement | L’ordre des sommets lors de l’assemblage d’une primitive. Peut être dans le sens des aiguilles d’une montre ou dans le sens inverse |
| ![symbole de sommet principal](images/d3d10-primitive-topologies-leading-vertex.png)       | Sommet principal    | Le premier sommet non adjacent dans une primitive qui contient des données par constante.       |

 

## <a name="span-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>Génération de plusieurs bandes


La fonction de coupe par bandes permet de générer plusieurs bandes. Vous pouvez effectuer une coupe par bandes en appelant explicitement la fonction HLSL[RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660), ou encore en insérant une valeur d’index spéciale dans le tampon d’index. Cette valeur est -1, qui correspond à 0xffffffff pour les index de 32bits ou à 0xffff pour les index de 16bits.

Un index de – 1 indique une fonction explicite ’cut’ ou ’restart’ de la bande actuelle. L’index précédent termine la primitive ou la bande précédente, et l’index suivant démarre une nouvelle primitive ou une nouvelle bande.

Pour plus d’informations sur la génération de plusieurs bandes, consultez l’article [Étape du nuanceur de géométrie (GS)](geometry-shader-stage--gs-.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Étape de l’assembleur d’entrée (IA)](input-assembler-stage--ia-.md)

[Pipeline graphique](graphics-pipeline.md)

 

 




