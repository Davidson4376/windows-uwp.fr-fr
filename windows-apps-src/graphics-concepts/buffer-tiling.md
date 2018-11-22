---
title: Mosaïque de mémoires tampons
description: Une ressource de mémoire tampon est divisée en mosaïques de 64Ko, avec un espace vide dans la dernière mosaïque si la taille n’est pas un multiple de 64Ko.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- Mosaïque de mémoires tampons
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1817f501962ccae4cfaf9c0ce075724abd5e7672
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2018
ms.locfileid: "7580619"
---
# <a name="buffer-tiling"></a>Mosaïque de mémoires tampons


Une ressource de [mémoire tampon](introduction-to-buffers.md) est divisée en mosaïques de 64Ko, avec un espace vide dans la dernière mosaïque si la taille n’est pas un multiple de 64Ko.

Les mémoires tampons structurées ne doivent avoir aucune contrainte sur le stride devant être restitué sous forme de mosaïques. Mais le gain potentiel de performances matérielles lié à l’utilisation de [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) peut être sacrifié si les ressources sont d'abord restituées sous forme de mosaïque.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque](how-a-streaming-resource-s-area-is-tiled.md)

 

 




