---
title: Listes de lignes
description: Une liste de lignes est une liste de segments de ligne rectilignes et isolés. Les listes de lignes sont utiles pour certaines tâches telles que l’ajout de neige fondue ou de pluie abondante dans une scène3D. Les applications créent une liste de lignes en remplissant un tableau de vertex.
ms.assetid: 42BF32A1-3535-42A3-82C5-3945CB309F2C
keywords:
- Listes de lignes
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5a514fb072dd95b9de2c29e097e8387c0f1068a4
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6649698"
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

 

 




