---
title: Niveau1
description: Cette section décrit la prise en charge du niveau1.
ms.assetid: 153DA429-0C99-4691-AEB4-124FD9B17BE2
keywords:
- Niveau1
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9aab54e23d59b21901e3aa4700581d2d7eeeee8e
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7417668"
---
# <a name="tier-1"></a>Niveau1


Cette section décrit la prise en charge du niveau1.

## <a name="span-idtier1generallimitationsspanspan-idtier1generallimitationsspanspan-idtier1generallimitationsspantier-1-general-limitations"></a><span id="Tier_1_general_limitations"></span><span id="tier_1_general_limitations"></span><span id="TIER_1_GENERAL_LIMITATIONS"></span>Limitations générales du niveau1


-   Matériel au niveau de fonctionnalité 11.0 minimum.
-   Aucune prise en charge du matelassage.
-   Aucune prise en charge de la Texture1D ou Texture3D.
-   Aucune prise en charge de l’anticrénelage multi-échantillon (MSAA) à 2, 8 ou 16échantillons. Seules les valeurs 4x sont requises, excepté les formats 128bpp.
-   Aucun modèle de panachage standard (disposition dans des tuiles de 64Ko et package de fin de mip pris en charge par le fabricant du matériel).
-   Restrictions d’accès aux tuiles lorsqu’il existe des mappages en double. Voir [Restrictions d’accès aux tuiles avec des mappages en double](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idspecificlimitationsaffectingtier1onlyspanspan-idspecificlimitationsaffectingtier1onlyspanspan-idspecificlimitationsaffectingtier1onlyspanspecific-limitations-affecting-tier-1-only"></a><span id="Specific_limitations_affecting_tier_1_only"></span><span id="specific_limitations_affecting_tier_1_only"></span><span id="SPECIFIC_LIMITATIONS_AFFECTING_TIER_1_ONLY"></span>Limitations spécifiques concernant uniquement le niveau1


### <a name="span-idreadingwritingtostreamingresourcesthathavenullmappingsspanspan-idreadingwritingtostreamingresourcesthathavenullmappingsspanspan-idreadingwritingtostreamingresourcesthathavenullmappingsspanreadingwriting-to-streaming-resources-that-have-null-mappings"></a><span id="Reading_writing_to_streaming_resources_that_have_NULL_mappings"></span><span id="reading_writing_to_streaming_resources_that_have_null_mappings"></span><span id="READING_WRITING_TO_STREAMING_RESOURCES_THAT_HAVE_NULL_MAPPINGS"></span>Lecture/écriture de ressources de diffusion en continu avec des mappages NULL

Les ressources de diffusion en continu peuvent avoir des mappages **NULL**, mais leur lecture ou écriture génère des résultats indéfinis, y compris la suppression d’appareil. Les applications peuvent résoudre ce problème en mappant une seule page factice pour toutes les zones vides. Faites attention si vous écrivez et effectuez le rendu sur une page qui est mappée à plusieurs emplacements cibles de rendu, car l’ordre des écritures ne sera pas défini.

### <a name="span-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanspan-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanspan-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanno-shader-instructions-for-clamping-lod-and-mapped-status-feedback"></a><span id="No_shader_instructions_for_clamping_LOD_and_mapped_status_feedback"></span><span id="no_shader_instructions_for_clamping_lod_and_mapped_status_feedback"></span><span id="NO_SHADER_INSTRUCTIONS_FOR_CLAMPING_LOD_AND_MAPPED_STATUS_FEEDBACK"></span>Aucune instruction de nuanceur pour les commentaires sur l’état mappé et le niveau de détail Clamp

Les instructions du nuanceur pour les commentaires sur l’état mappé et le niveau de détail Clamp ne sont pas disponibles. Voir [Exposition des ressources de diffusion en continu HLSL](hlsl-streaming-resources-exposure.md).

### <a name="span-idalignmentconstraintsforstandardtileshapesspanspan-idalignmentconstraintsforstandardtileshapesspanspan-idalignmentconstraintsforstandardtileshapesspanalignment-constraints-for-standard-tile-shapes"></a><span id="Alignment_constraints_for_standard_tile_shapes"></span><span id="alignment_constraints_for_standard_tile_shapes"></span><span id="ALIGNMENT_CONSTRAINTS_FOR_STANDARD_TILE_SHAPES"></span>Contraintes d’alignement des formes de tuile standard

Seuls les mips (en commençant par le plus petit) dont les dimensions sont des multiples de la taille de tuile standard prennent en charge les formes de tuile standard et peuvent disposer de tuiles individuelles arbitrairement mappées/non mappées. Le premier mipmap dans une ressource de diffusion en continu dont la dimension n’est pas un multiple de la taille de tuile standard, ainsi que tous les mipmaps plus grands, peuvent avoir une forme de tuile non standard, applicable à des tuiles N 64Ko pour cet ensemble de mips (N signalé pour cette application). Ces tuiles N sont considérées comme compressées en une seule unité, qui doit être entièrement mappée ou non mappée par l’application à un moment donné, même si les mappages de chaque tuile N peuvent se trouver à des emplacements disjoints arbitraires dans un pool de tuiles.

### <a name="span-idarrayofmipmapsthatarentamultipleofstandardtilesizespanspan-idarrayofmipmapsthatarentamultipleofstandardtilesizespanspan-idarrayofmipmapsthatarentamultipleofstandardtilesizespanarray-of-mipmaps-that-arent-a-multiple-of-standard-tile-size"></a><span id="Array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="ARRAY_OF_MIPMAPS_THAT_AREN_T_A_MULTIPLE_OF_STANDARD_TILE_SIZE"></span>Tableau des mipmaps n’étant pas des multiples de la taille de tuile standard

Les ressources de diffusion en continu avec des mipmaps n’étant pas des multiples de la taille de tuile standard dans toutes les dimensions ne sont pas autorisées à avoir une taille de tableau supérieure à 1.

### <a name="span-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanspan-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanspan-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanswitching-between-referencing-tiles-in-a-tile-pool-via-a-buffer-and-texture-resource"></a><span id="Switching_between_referencing_tiles_in_a_tile_pool_via_a_Buffer_and_Texture_resource"></span><span id="switching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resource"></span><span id="SWITCHING_BETWEEN_REFERENCING_TILES_IN_A_TILE_POOL_VIA_A_BUFFER_AND_TEXTURE_RESOURCE"></span>Basculement entre les tuiles de référencement dans un pool de tuiles via une ressource de mémoire tampon et de texture

Pour basculer entre les tuiles de référencement dans un pool de tuiles via une ressource de [mémoire tampon](introduction-to-buffers.md) pour référencer les mêmes tuiles via une ressource de [texture](introduction-to-textures.md), ou vice versa, la mise à jour ou la copie des mappages de tuile la plus récente définissant les mappages de tuiles de pool de tuiles doit être pour la même dimension de ressource (mémoire tampon ou texture\*) que la dimension de ressource qui sera utilisée pour accéder aux tuiles. Sinon, le comportement ne sera pas défini, y compris la possibilité de réinitialiser l’appareil.

Par exemple, il n’est pas correct de mettre à jour les mappages de tuiles pour définir des mappages de tuiles pour une mémoire tampon, puis de mettre à jour les mappages de tuiles pour les mêmes tuiles dans le pool de tuiles via une ressource [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) et d’accéder aux tuiles via la mémoire tampon. Les solutions alternatives consistent à redéfinir les mappages de tuiles pour une ressource lors du basculement entre la mémoire tampon et la texture (ou vice versa) partageant les tuiles ou simplement ne jamais de partager des tuiles dans un pool de tuiles entre des ressources de mémoire tampon et des ressources de texture.

### <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrage de réduction max/min.

Le filtrage de réduction min/max n’est pas pris en charge. Voir [Fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu](streaming-resources-texture-sampling-features.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Niveaux de fonctionnalité des ressources de diffusion en continu](streaming-resources-features-tiers.md)

 

 




