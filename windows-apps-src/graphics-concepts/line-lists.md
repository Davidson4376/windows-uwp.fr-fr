---
title: Listes de lignes
description: Une liste de lignes est une liste de segments de ligne rectilignes et isolés. Les listes de lignes sont utiles pour certaines tâches telles que l’ajout de neige fondue ou de pluie abondante dans une scène3D. Les applications créent une liste de lignes en remplissant un tableau de vertex.
ms.assetid: 42BF32A1-3535-42A3-82C5-3945CB309F2C
keywords:
- Listes de lignes
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 43013dc820c0f0f67df2e9502d3c57c77e03f250
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8873668"
---
# <a name="line-lists"></a>Listes de lignes


Une liste de lignes est une liste de segments de ligne rectilignes et isolés. Les listes de lignes sont utiles pour certaines tâches telles que l’ajout de neige fondue ou de pluie abondante dans une scène3D. Les applications créent une liste de lignes en remplissant un tableau de vertex. Notez que le nombre de vertex d’une ligne de listes peut être un nombre pair supérieur ou égal à 2.

-   [Exemple](#example)
-   [Rubriques connexes](#related-topics)

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


L’illustration suivante représente une liste de lignes rendue.

![illustration d’une liste de lignes](images/linelst.png)

Vous pouvez appliquer des matériaux et des textures à une liste de lignes. Les couleurs du matériau ou de la texture apparaissent le long des lignes tracées, pas entre ces dernières.

Le code suivant montre comment créer des vertex pour cette liste de lignes.

```
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

L’exemple de code ci-dessous montre comment effectuer le rendu d’une liste de lignes dans Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINELIST, 0, 3 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Primitives](primitives.md)

 

 




