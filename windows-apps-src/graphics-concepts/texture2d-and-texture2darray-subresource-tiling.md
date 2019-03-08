---
title: Restitution des sous-ressources Texture2D et Texture2DArray sous forme de mosaïque
description: Les tableaux suivants montrent comment des sous-ressources Texture2D et Texture2DArray sont restituées sous forme de tuiles.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Restitution des sous-ressources Texture2D et Texture2DArray sous forme de mosaïque
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2175ce19824068a850ff70340b467f09e5c76540
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592744"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Restitution des sous-ressources Texture2D et Texture2DArray sous forme de mosaïque


Les tableaux suivants montrent comment des sous-ressources [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) et [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) sont restituées sous forme de tuiles. Les valeurs figurant dans ces tableaux ne prennent pas en compte le package de fin de mip.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>Sous-ressources avec le nombre d’échantillonnage multiple de 1


Le tableau suivant montre comment des sous-ressources [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) et [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) avec un nombre d’échantillonnage multiple de 1 sont restituées sous forme de tuiles.

| Bits/pixel (1 exemple/pixel) | Dimensions de la tuile (pixels, l x h) |
|-----------------------------|-------------------------------|
| 8                           | 256 x 256                       |
| 16                          | 256 x 128                       |
| 32                          | 128 x 128                       |
| 64                          | 128 x 64                        |
| 128                         | 64 x 64                         |
| BC1,4                       | 512 x 256                       |
| BC2,3,5,6,7                 | 256 x 256                       |

 

Nombre de bits de format non pris en charge avec la diffusion en continu de ressources est 96 bpp formats, les formats vidéo, DXGI\_FORMAT\_R1\_UNORM, DXGI\_FORMAT\_R8G8\_B8G8\_UNORM, et DXGI\_FORMAT\_R8R8\_G8B8\_UNORM.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>Sous-ressources avec différents nombres échantillonnage multiple


Le tableau suivant montre comment des sous-ressources [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) et [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) avec plusieurs échantillonnages multiples sont restituées sous forme de tuiles.

| Bits/pixel (1 exemple/pixel) | Dimensions de la tuile (pixels, l x h) |
|-----------------------------|-------------------------------|
| 1                           | 1 x 1                           |
| 2                           | 2 x 1                           |
| 4                           | 2 x 2                           |
| 8                           | 4 x 2                           |
| 16                          | 4 x 4                           |

 

Seul un nombre d’échantillonnage compris entre 1 et 4 est requis (et autorisé) et peut être pris en charge avec les ressources de diffusion en continu. Les ressources de diffusion en continu ne prennent actuellement pas en charge les nombres d’échantillonnage de 2, 8 et 16, même si elles sont affichées.

Les implémentations peuvent choisir de prendre en charge 2, 8 ou 16 modes d’anticrénelage multi-échantillon (MSAA) pour des ressources autres que de diffusion en continu, même si les ressources de diffusion en continu ne les prennent pas en charge.

Les ressources de diffusion en continu avec un nombre d’échantillons supérieur à 1 ne peuvent pas utiliser les formats de 128 bpp.

Les contraintes sur le nombre d’échantillons et les formats pris en charge sont dues à des incohérences matérielles par rapport à la spécification.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[La zone de diffusion en continu d’une ressource est affichée en mosaïque](how-a-streaming-resource-s-area-is-tiled.md)

 

 




