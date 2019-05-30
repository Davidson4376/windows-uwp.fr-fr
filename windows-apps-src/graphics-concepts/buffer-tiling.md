---
title: Mosaïque de mémoires tampons
description: Une ressource de mémoire tampon est divisée en mosaïques de 64 Ko, avec un espace vide dans la dernière mosaïque si la taille n’est pas un multiple de 64 Ko.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- Mosaïque de mémoires tampons
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 34201c889ed984b27d50126bd2a04e9b320a17a3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370654"
---
# <a name="buffer-tiling"></a>Mosaïque de mémoires tampons


Une ressource de [mémoire tampon](introduction-to-buffers.md) est divisée en vignettes de 64 Ko, avec un espace vide dans la dernière vignette si la taille n’est pas un multiple de 64 Ko.

Les mémoires tampons structurées ne doivent avoir aucune contrainte sur le stride devant être restitué sous forme de mosaïques. Mais le gain potentiel de performances matérielles lié à l’utilisation de [**StructuredBuffers**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-structuredbuffer) peut être sacrifié si les ressources sont d'abord restituées sous forme de mosaïque.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[La zone de diffusion en continu d’une ressource est affichée en mosaïque](how-a-streaming-resource-s-area-is-tiled.md)

 

 




