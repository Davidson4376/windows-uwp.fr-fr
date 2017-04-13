---
title: "Comportement du SRV avec les vignettes non mappées"
description: "Le comportement des lectures d&quot;une vue de ressource du nuanceur (SRV) impliquant des vignettes non mappées varie selon le niveau de prise en charge matérielle."
ms.assetid: 0E1D64BE-EB09-4F9D-9800-BD23A3B374EE
keywords: "Comportement du SRV avec les vignettes non mappées"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: e6c4334446ef3bc302602d0b59b81164c9a28f3d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="span-iddirect3dconceptssrvbehaviorwithnon-mappedtilesspansrv-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.srv_behavior_with_non-mapped_tiles"></span>Comportement du SRV avec les vignettes non mappées


Le comportement des lectures d'une vue de ressource du nuanceur (SRV) impliquant des vignettes non mappées varie selon le niveau de prise en charge matérielle. Pour une analyse des conditions requises, reportez-vous au comportement des lectures pour [Niveaux de fonctionnalité des ressources de diffusion en continu](streaming-resources-features-tiers.md). Cette section résume le comportement idéal, ce qui [niveau 2](tier-2.md) requiert.

Tenez compte d'une opération de filtre de texture qui lit à partir d’un ensemble de texels dans un SRV. Les texels qui interviennent dans des vignettes non mappées contribuent à 0dans tous les composants non manquants du format (et la valeur par défaut pour les composants manquants) dans l’opération de filtre global avec les contributions à partir des texels mappés. Tous les texels sont pondérés et combinés, indépendamment du fait que les données proviennent des vignettes mappées ou non mappées.

Certains matériaux [niveau 2](tier-2.md) de première génération ne répondent pas à cette condition spécifique et renvoient la valeur 0 avec les valeurs par défaut décrites précédemment en tant que le résultat du filtre global si les texels (avec une pondération différente de zéro) se trouvent sur des vignettes non mappées. Aucun autre matériel n’est autorisé à manquer à cette exigence d’inclure tous les texels (pondérés et différents de zéro) dans le filtre.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Accès du pipeline aux ressources de diffusion en continu](pipeline-access-to-streaming-resources.md)

 

 




