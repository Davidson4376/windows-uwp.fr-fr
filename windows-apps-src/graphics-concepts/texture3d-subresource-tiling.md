---
title: Restitution de la sous-ressource Texture3D sous forme de tuiles
description: "Ce tableau montre comment des sous-ressources Texture3D sont restituées sous forme de tuiles."
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Restitution de la sous-ressource Texture3D sous forme de tuiles
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 6f6c2648ceefae5b13b481decc37be4047d938d4
ms.lasthandoff: 02/07/2017

---

# <a name="texture3d-subresource-tiling"></a>Restitution de la sous-ressource Texture3D sous forme de tuiles


Ce tableau montre comment des sous-ressources [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) sont restituées sous forme de tuiles. Les valeurs figurant dans ce tableau ne prennent pas en compte le package de fin de mip.

Ce tableau prend les tuiles [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) et divise les dimensions x/y par 4 et ajoute 16 couches de profondeur. Toutes les tuiles du premier plan (plan 2D de tuiles définissant les 16 premières couches de profondeur) apparaissent avant les plans ultérieurs.

**Remarque**  La prise en charge de [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) dans les ressources de diffusion en continu n’est pas disponible dans l’implémentation initiale des ressources de diffusion en continu, mais les formes de tuiles souhaitées sont répertoriées ici pour la prise en charge possible avec une version ultérieure.

 

| Bits/pixel (1 exemple/pixel) | Dimensions de la tuile (Pixels, l x h x p) |
|-----------------------------|---------------------------------|
| 8                           | 64 x 32 x 32                        |
| 16                          | 32 x 32 x 32                        |
| 32                          | 32 x 32 x 16                        |
| 64                          | 32 x 16 x 16                        |
| 128                         | 16 x 16 x 16                        |
| BC1,4                       | 128 x 64 x 16                       |
| BC2,3,5,6,7                 | 64 x 64 x 16                        |

 

Les bits de format non pris en charge avec les ressources de diffusion en continu sont les formats de 96 bpp, les formats vidéo, DXGI\_FORMAT\_R1\_UNORM, DXGI\_FORMAT\_R8G8\_B8G8\_UNORM et DXGI\_FORMAT\_R8R8\_G8B8\_UNORM.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Restitution de la surface d’une ressource de diffusion en continu sous forme de tuiles](how-a-streaming-resource-s-area-is-tiled.md)

 

 





