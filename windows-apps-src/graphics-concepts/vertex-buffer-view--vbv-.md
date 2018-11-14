---
title: Affichage d'une mémoire tampon de vertex (VBV) et d’index (IBV)
description: La mémoire tampon de vertex conserve les données d'une liste de sommets.
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- Affichage d'une mémoire tampon de vertex (VBV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7956c7a03256da04e98a5b8f9a33951d7e0216d3
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6162834"
---
# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>Affichage d'une mémoire tampon de vertex (VBV) et d’index (IBV)


La mémoire tampon de vertex conserve les données d'une liste de sommets. Les données de chaque vertex sont notamment la position, la couleur, le vecteur normal, les coordonnées de texture, etc. La mémoire tampon d’index conserve des index entiers (décalages) dans une mémoire tampon de vertex et permet de définir et rendre un objet constitué d’un sous-ensemble de la liste complète des sommets.

La définition d’un seul vertex dépend souvent de l’application, par exemple:

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

La définition de CUSTOMVERTEX serait transmise au pilote graphique lors de la création de mémoires tampons de vertex.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Affichages](views.md)

 

 




