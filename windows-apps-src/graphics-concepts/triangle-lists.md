---
title: Listes de triangles
description: Une liste de triangles est une liste de triangles isolés. Les triangles isolés peuvent être proches ou non. Une liste de triangles doit avoir au moins trois vertex et le nombre total de vertex doit être divisible par trois.
ms.assetid: BC50D532-9E9C-4AAE-B466-9E8C4AD1862A
keywords:
- Listes de triangles
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: aae797db890c6bee141c3b4a79a6a85a55a6b512
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8938374"
---
# <a name="triangle-lists"></a>Listes de triangles


Une liste de triangles est une liste de triangles isolés. Les triangles isolés peuvent être proches ou non. Une liste de triangles doit avoir au moins trois vertex et le nombre total de vertex doit être divisible par trois.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


Utilise les listes de triangles pour créer un objet composé de pièces disjointes. Par exemple, un moyen de créer un mur de champ de force dans un jeu3D consiste à spécifier une longue liste de petits triangles non connectés. On applique ensuite un matériau et une texture qui semblent émettre une lumière vers la liste de triangles. Chaque triangle du mur semble briller. La scène derrière le mur devient partiellement visible à travers les intervalles séparant les triangles, comme un joueur peut s’y attendre en regardant un champ de force.

Les listes de triangles sont également utiles pour créer des primitives aux bords nets, ombrées à l'aide un ombrage de Gouraud. Voir [Vecteurs normaux à une face ou un sommet](face-and-vertex-normal-vectors.md).

L’illustration suivante représente une liste de triangles rendue.

![Illustration d’une liste de triangles rendue](images/trilist.png)

Le code suivant montre comment créer des sommets pour cette liste de triangles.

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

L’exemple de code ci-dessous montre comment rendre cette liste de triangles dans Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLELIST, 0, 2 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Primitives](primitives.md)

 

 




