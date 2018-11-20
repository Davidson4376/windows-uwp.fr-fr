---
title: Filtrage de textures anisotropiques
description: L'anisotropie désigne la distorsion visible dans les texels d'un objet3D dont la surface est orientée à un certain angle par rapport au plan de l'écran. Un pixel d’une primitive anisotropique apparaît déformé lorsqu'il est mappé à des texels.
ms.assetid: 58923809-EF76-4C16-BCE7-922A66425F83
keywords:
- Filtrage de textures anisotropiques
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6e91c707b31de859d61ae926518c40812758320e
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7303153"
---
# <a name="anisotropic-texture-filtering"></a>Filtrage de textures anisotropiques


L'*anisotropie* est la distorsion visible dans les éléments de texture d’un objet 3D dont la surface est orientée à un certain angle par rapport au plan de l’écran. Un pixel d’une primitive anisotropique apparaît déformé lorsqu'il est mappé à des texels. Direct3D mesure l’anisotropie d’un pixel par l’allongement (autrement dit, la longueur divisée par la largeur) d’un pixel de l'écran inversé dans l’espace de texture.

Vous pouvez utiliser le filtrage de textures anisotropiques parallèlement à un filtrage de textures linéaires ou à un filtrage de textures mipmap afin d'améliorer les résultats de rendu.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Filtrage de textures](texture-filtering.md)

 

 




