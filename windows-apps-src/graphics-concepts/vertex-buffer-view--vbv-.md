---
title: Affichage d'une mémoire tampon de vertex (VBV) et d’index (IBV)
description: La mémoire tampon de vertex conserve les données d'une liste de sommets.
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- Affichage d'une mémoire tampon de vertex (VBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cfb92c4f876d85388ce325f151408fe7b9e8d8b4
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7704207"
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

 

 




