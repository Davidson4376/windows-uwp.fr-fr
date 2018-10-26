---
title: Bandes de triangles
description: Une bande de triangles est une série de triangles connectés. Les triangles étant connectés, l'application n'a pas besoin de spécifier plusieurs fois les trois vertex pour chaque triangle.
ms.assetid: BACC74C5-14E5-4ECC-9139-C2FD1808DB82
keywords:
- Bandes de triangles
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f60f0f65868d4dec67bf77a329d4b952c20ec44a
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5552593"
---
# <a name="triangle-strips"></a>Bandes de triangles


Une bande de triangles est une série de triangles connectés. Les triangles étant connectés, l'application n'a pas besoin de spécifier plusieurs fois les trois vertex pour chaque triangle. Par exemple, il suffit de sept vertex pour définir la bande de triangles suivantes.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


![Illustration d’une bande de triangles à sept vertex](images/tristrip.png)

Le système utilise les vertex v1 v2 et v3 pour tracer le premier triangle; v2, v4 et v3 pour tracer le deuxième triangle; v3 et v4 v5 pour dessiner le troisième; v4 et v6 v5 pour dessiner le quatrième; et ainsi de suite. Notez que les vertex des deuxième et quatrième triangles sont hors service; cela est nécessaire pour que tous les triangles soient tracés dans le sens des aiguilles d'une montre.

La plupart des objets d'une scène 3D sont composées de triangles. En effet, les bandes de triangles peuvent être utilisées pour spécifier des objets complexes de manière à utilise efficacement la mémoire et le temps de traitement.

L’illustration suivante représente une bande de triangles rendue.

![Illustration d’une bande de triangles rendue](images/tstrip2.png)

Le code suivant montre comment créer des vertex pour cette bande de triangles.

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

L’exemple de code ci-dessous montre comment rendre cette bande de triangles dans Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLESTRIP, 0, 4);
```

## <a name="span-idpolygonsspanspan-idpolygonsspanspan-idpolygonsspanpolygons"></a><span id="Polygons"></span><span id="polygons"></span><span id="POLYGONS"></span>Polygones


Souvent, des triangles sont utilisés pour générer des polygones. Un polygone est une figure3D fermée délimitée par au moins trois sommets. Le polygone le plus simple est un triangle. MicrosoftDirect3D utilise les triangles pour composer la plupart de ses polygones, car les trois sommets d'un triangle sont systématiquement coplanaires. Le rendu de sommets non planaires n’est pas efficace. Vous pouvez combiner des triangles pour former des maillages et des polygones larges et complexes.

L’illustration suivante montre un cube. Chaque face du cube est formée de deux triangles. L’ensemble des triangles constitue une primitive cubique. Vous pouvez appliquer des textures aux surfaces de primitives pour qu'elles apparaissent comme une forme unique pleine. Pour plus d’informations, voir [Textures](textures.md).

![Illustration d’un cube avec deux triangles sur chaque face](images/cube3d.png)

Vous pouvez également utiliser les triangles pour créer des primitives dont les surfaces semblent être des courbes lisses. L’illustration suivante montre comment une sphère peut être simulée à l’aide de triangles. Après l'application d'un matériau, la sphère peut prendre un aspect courbe après rendu.

![Illustration d’une sphère simulée à l’aide de triangles](images/sphere3d.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Primitives](primitives.md)

 

 




