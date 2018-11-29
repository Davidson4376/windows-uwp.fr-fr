---
title: Étape du rastériseur (RS)
description: Le rastériseur extrait les primitives qui ne sont pas dans la vue, les prépare pour l’étape du nuanceur de pixels (PS) et détermine le mode d’invocation des nuanceurs de pixels.
ms.assetid: 7E80724B-5696-4A99-91AF-49744B5CD3A9
keywords:
- Étape du rastériseur (RS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 499d361bddbecef50ee8cdf44b56530a98cfccd1
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7978369"
---
# <a name="rasterizer-rs-stage"></a>Étape du rastériseur (RS)


Le rastériseur extrait les primitives qui ne sont pas dans la vue, les prépare pour l’étape du [nuanceur de pixels (PS)](pixel-shader-stage--ps-.md) et détermine le mode d’invocation des nuanceurs de pixels. L’étape de rastérisation convertit des informations de vecteur (composées de formes ou de primitives) en une image raster (constituée de pixels) afin d’afficher des graphiques3D en temps réel.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Objectif et cas d’utilisation


Lors de la rastérisation, chaque primitive est convertie en pixels, et les valeurs par sommet sont interpolées pour chaque primitive. La rastérisation inclut le détourage des sommets dans le tronc de cône de vue, la division par z pour fournir une perspective, le mappage de primitives dans une fenêtre d’affichage2D et la détermination du mode d’invocation du nuanceur de pixels. Même si l’utilisation d’un nuanceur de pixels est facultative, l’étape du rastériseur effectue toujours un détourage, une division de la perspective pour transformer les points en espace homogène et un mappage des sommets dans la fenêtre d’affichage.

Vous pouvez désactiver la rastérisation en indiquant au pipeline qu’il n’existe pas de nuanceur de pixels (définissez l’[étape du nuanceur de pixels](pixel-shader-stage--ps-.md) sur **NULL** et désactivez le test de profondeur et de gabarit). Quand la rastérisation est désactivée, les compteurs du pipeline relatif à la rastérisation ne sont pas mis à jour.

Sur un matériel qui implémente des optimisations hiérarchiques du tampon Z, vous pouvez activer le préchargement du tampon Z en définissant l’étape du nuanceur de pixels sur **NULL** lors de l’activation du test de profondeur et de gabarit.

Voir [Règles de rastérisation](rasterization-rules.md).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


Les sommets (X,Y,Z,W) introduits dans l’étape du rastériseur sont supposés se trouver dans un espace de détourage homogène. Dans cet espace de coordonnées, l’axeX pointe vers la droite, l’axeY pointe vers le haut et l’axeZ pointe dans la direction opposée à la caméra.

L’étape du rastériseur à fonctions fixes est alimentée par l’étape de sortie de flux (SO) et/ou l’étape précédente du pipeline, par exemple l’étape du [nuanceur de géométrie (GS)](geometry-shader-stage--gs-.md). Si l’étape du nuanceur de géométrie n’est pas utilisée, l’étape du rastériseur est alimentée par [l’étape du nuanceur de domaine (DS)](domain-shader-stage--ds-.md). Si l’étape du nuanceur de domaine n’est pas utilisée, l’étape du rastériseur est alimentée par [l’étape du nuanceur de sommets (VS)](vertex-shader-stage--vs-.md).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


L’utilisation du nuanceur de pixels (PS) est facultative; l’étape du rastériseur peut, à la place, passer directement à l’[étape de fusion de sortie (OM)](output-merger-stage--om-.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Règles de rastérisation](rasterization-rules.md)

[Pipeline graphique](graphics-pipeline.md)

 

 




