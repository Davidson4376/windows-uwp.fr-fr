---
title: Restitution de la sous-ressource Texture3D sous forme de mosaïque
description: Ce tableau montre comment des sous-ressources Texture3D sont restituées sous forme de tuiles.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Restitution de la sous-ressource Texture3D sous forme de mosaïque
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5b63fdeeffd4b95afab6556b6f0318732ff988b0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370904"
---
# <a name="texture3d-subresource-tiling"></a>Restitution de la sous-ressource Texture3D sous forme de mosaïque


Ce tableau montre comment des sous-ressources [**Texture3D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d) sont restituées sous forme de tuiles. Les valeurs figurant dans ce tableau ne prennent pas en compte le package de fin de mip.

Ce tableau prend les tuiles [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) et divise les dimensions x/y par 4 et ajoute 16 couches de profondeur. Toutes les tuiles du premier plan (plan 2D de tuiles définissant les 16 premières couches de profondeur) apparaissent avant les plans ultérieurs.

**Remarque**  La prise en charge de   [**Texture3D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d) dans les ressources de diffusion en continu n’est pas disponible dans l’implémentation initiale des ressources de diffusion en continu, mais les formes de tuiles souhaitées sont répertoriées ici pour la prise en charge possible avec une version ultérieure.

 

| Bits/pixel (1 exemple/pixel) | Dimensions de la tuile (Pixels, l x h x p) |
|-----------------------------|---------------------------------|
| 8                           | 64 x 32 x 32                        |
| 16                          | 32 x 32 x 32                        |
| 32                          | 32 x 32 x 16                        |
| 64                          | 32 x 16 x 16                        |
| 128                         | 16 x 16 x 16                        |
| BC1,4                       | 128 x 64 x 16                       |
| BC2,3,5,6,7                 | 64 x 64 x 16                        |

 

Nombre de bits de format non pris en charge avec la diffusion en continu de ressources est 96 bpp formats, les formats vidéo, DXGI\_FORMAT\_R1\_UNORM, DXGI\_FORMAT\_R8G8\_B8G8\_UNORM, et DXGI\_FORMAT\_R8R8\_G8B8\_UNORM.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[La zone de diffusion en continu d’une ressource est affichée en mosaïque](how-a-streaming-resource-s-area-is-tiled.md)

 

 




