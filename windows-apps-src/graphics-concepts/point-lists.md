---
title: Listes de points
description: "Une liste de points est un ensemble de sommets qui sont rendus sous forme de points isolés. Votre application peut utiliser des listes de points dans les scènes 3D pour les champs d’étoiles, ou encore des lignes en pointillé sur la surface d’un polygone."
ms.assetid: 332954AE-019F-4A91-B773-E3A7C92F3297
keywords:
- Listes de points
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: da7fd4dfd4835e39a76daddfa8855415f0e3234b
ms.lasthandoff: 02/07/2017

---

# <a name="point-lists"></a>Listes de points


Une liste de points est un ensemble de sommets qui sont rendus sous forme de points isolés. Votre application peut utiliser des listes de points dans les scènes 3D pour les champs d’étoiles, ou encore des lignes en pointillé sur la surface d’un polygone.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


L’illustration suivante représente une liste de points rendue.

![illustration d’une liste de points](images/pointlst.png)

Votre application peut appliquer des matériaux et des textures à une liste de points. Les couleurs du matériau ou de la texture apparaissent uniquement au niveau des points dessinés, et pas n’importe où entre les points.

Le code suivant montre comment créer des sommets pour cette liste de points.

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

L’exemple de code ci-dessous montre comment effectuer le rendu de cette liste de points dans Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_POINTLIST, 0, 6 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Primitives](primitives.md)

 

 





