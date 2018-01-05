---
title: Bandes de lignes
description: "Une bande de lignes est une primitive composée de segments de ligne connectés. Votre application peut utiliser des bandes de lignes pour créer des polygones non fermés. Un polygone fermé est un polygone dont le dernier vertex est connecté au premier vertex par un segment de ligne."
ms.assetid: 6E8C58E1-B463-44FD-A69F-81CCBF25D856
keywords: Bandes de lignes
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7eec406faf1f695a473154dd23322bcdf9189049
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
---
# <a name="line-strips"></a>Bandes de lignes


Une bande de lignes est une primitive composée de segments de ligne connectés. Votre application peut utiliser des bandes de lignes pour créer des polygones non fermés. Un polygone fermé est un polygone dont le dernier vertex est connecté au premier vertex par un segment de ligne. Si votre application génère des polygones basés sur des bandes de lignes, les vertex ne seront pas obligatoirement coplanaires.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


L’illustration suivante représente une bande de lignes rendue.

![illustration d’une bande de ligne](images/linstrip.gif)

Le code suivant montre comment créer des vertex pour cette bande de lignes.

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

L’exemple de code ci-dessous montre comment effectuer le rendu d’une bande de lignes dans Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINESTRIP, 0, 5 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Primitives](primitives.md)

 

 




