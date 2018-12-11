---
title: Niveau2
description: La prise en charge du niveau2 pour les ressources de diffusion en continu ajoute des fonctionnalités par rapport au niveau1, telles que la garantie d’un mipmap de texture non compressé lorsque la taille est au minimum de forme de tuile standard, les instructions de nuanceur pour le niveau de détail Clamp et pour obtenir l’état de l’opération du nuanceur et la lecture à partir de tuiles mappées NULL qui ont échantillonné une valeur de zéro.
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- Niveau2
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6f9f9a69c0e30459929d1e31084ea88b3f7ebbd0
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8900258"
---
# <a name="tier-2"></a>Niveau2


La prise en charge du niveau2 pour les ressources de diffusion en continu ajoute des fonctionnalités par rapport au niveau1, telles que la garantie d’un mipmap de texture non compressé lorsque la taille est au minimum de forme de tuile standard, les instructions de nuanceur pour le niveau de détail Clamp et pour obtenir l’état de l’opération du nuanceur et la lecture à partir de tuiles mappées NULL qui ont échantillonné une valeur de zéro.

## <a name="span-idtier2generalsupportspanspan-idtier2generalsupportspanspan-idtier2generalsupportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>Prise en charge générale du niveau2


Le niveau2 prend en charge les éléments suivants.

-   Matériel au niveau de fonctionnalité 11.1 minimum.
-   Toutes les fonctionnalités du niveau précédent (sans les limitations spécifiques au [niveau1](tier-1.md)), ainsi que les ajouts des éléments suivants:
-   Les instructions du nuanceur pour les commentaires sur l’état mappé et le niveau de détail Clamp sont disponibles. Voir [Exposition des ressources de diffusion en continu HLSL](hlsl-streaming-resources-exposure.md).

Voici quelques problèmes de prise en charge spécifiques.

## <a name="span-idnon-mappedtilesspanspan-idnon-mappedtilesspanspan-idnon-mappedtilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>Tuiles non mappées


La lecture de tuiles non mappées renvoie0 dans tous les composants non manquants du format et la valeur par défaut des composants manquants.

Les écritures dans les tuiles non mappées ne sont plus enregistrées dans la mémoire, mais peuvent se retrouver dans des caches que les lectures suivantes à la même adresse peuvent ou non récupérer.

## <a name="span-idtexturefilteringspanspan-idtexturefilteringspanspan-idtexturefilteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>Filtrage de textures


Le filtrage de textures avec un encombrement qui superpose des tuiles **NULL** et non-**NULL** contribue à la valeur 0 (avec des valeurs par défaut pour les composants de format manquants) pour les texels sur les tuiles **NULL** dans l’opération de filtre globale. Certains matériels antérieurs ne répondent pas à cette exigence et renvoient la valeur 0 (avec des valeurs par défaut pour les composants de format manquants) pour le résultat du filtre complet si des texels (avec une pondération différente de zéro) se trouvent sur une tuile **NULL**. Aucun autre matériel n’est autorisé à manquer à cette exigence d’inclure tous les texels (pondérés et différents de zéro) dans l’opération de filtre.

Des accès aux texels **NULL** entraînent une réponse False à l’opération [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) sur les commentaires sur l’état pour une lecture de texture. Cette réponse est indépendante de la façon dont le résultat d’accès à la texture est écrit en masqué dans le nuanceur et du nombre de composants dans le format de la texture (cette combinaison peut révéler qu’un accès à la texture n’est pas nécessaire).

## <a name="span-idalignmentconstraintsspanspan-idalignmentconstraintsspanspan-idalignmentconstraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>Contraintes d’alignement


Contraintes d’alignement des formes de tuile standard: les mipmaps qui remplissent au moins une tuile standard dans toutes les dimensions utilisent systématiquement la tuile standard, les autres étant considérées comme compressées en tant qu’**unité** dans les tuiles N (N signalé dans l’application). L’application peut mapper les tuiles N à des emplacements disjoints arbitraires dans un pool de tuiles, mais doit mapper toutes les tuiles compressées ou aucune tuile compressée. Le package mip est un ensemble unique de tuiles compressées par section de tableau.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrage de réduction max/min.


Le filtrage de réduction min/max est pris en charge. Voir [Fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu](streaming-resources-texture-sampling-features.md).

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>Limitations


Les ressources de diffusion en continu avec des mipmaps inférieurs à la taille de tuile standard dans toutes les dimensions ne sont pas autorisées à avoir une taille de tableau supérieure à 1.

Les limitations sur le mode d’accès des tuiles lorsqu’il existe des mappages en double s’appliquent toujours. Voir [Restrictions d’accès aux tuiles avec des mappages en double](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Niveaux de fonctionnalité des ressources de diffusion en continu](streaming-resources-features-tiers.md)

 

 




