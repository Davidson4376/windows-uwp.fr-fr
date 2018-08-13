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
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b6f4b74d64b7a3f9768697b5b6f495a322686c59
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1042598"
---
# <a name="anisotropic-texture-filtering"></a>Filtrage de textures anisotropiques


L'*anisotropie* est la distorsion visible dans les éléments de texture d’un objet 3D dont la surface est orientée à un certain angle par rapport au plan de l’écran. Un pixel d’une primitive anisotropique apparaît déformé lorsqu'il est mappé à des texels. Direct3D mesure l’anisotropie d’un pixel par l’allongement (autrement dit, la longueur divisée par la largeur) d’un pixel de l'écran inversé dans l’espace de texture.

Vous pouvez utiliser le filtrage de textures anisotropiques parallèlement à un filtrage de textures linéaires ou à un filtrage de textures mipmap afin d'améliorer les résultats de rendu.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Filtrage de textures](texture-filtering.md)

 

 




