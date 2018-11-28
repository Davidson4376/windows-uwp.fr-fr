---
title: Restrictions d’accès aux tuiles avec des mappages en double
description: Il existe des restrictions d’accès aux tuiles avec des mappages en double, par exemple en cas de copie de ressources de diffusion en continu avec une source et une destination se superposant, ou lors du rendu de tuiles partagées dans la zone de rendu.
ms.assetid: 6E40B1DC-BCF1-4B09-82A8-7B2D9B209A61
keywords:
- Restrictions d’accès aux tuiles avec des mappages en double
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d5563a9909ba3d6cb3deaae43bcf9e55b4b2c880
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7966757"
---
# <a name="tile-access-limitations-with-duplicate-mappings"></a>Restrictions d’accès aux tuiles avec des mappages en double


Il existe des restrictions d’accès aux tuiles avec des mappages en double, par exemple en cas de copie de ressources de diffusion en continu avec une source et une destination se superposant, ou lors du rendu de tuiles partagées dans la zone de rendu.

## <a name="span-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspancopying-streaming-resources-with-overlapping-source-and-destination"></a><span id="Copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="COPYING_STREAMING_RESOURCES_WITH_OVERLAPPING_SOURCE_AND_DESTINATION"></span>Copie de ressources de diffusion en continu avec source et destination se superposant


Si lors d’une opération Copy\*, des tuiles dans la zone de source et de destination comptent des mappages en double dans la zone de copie qui seraient superposés même si les deux ressources n’étaient pas des ressources de diffusion en continu et si l’opération Copy*\ prenait en charge les copies se superposant, l’opération Copy*\ se comportent correctement (comme si la source est copiée vers un emplacement temporaire avant de passer dans la zone de destination). Cependant, si la superposition n’est pas évidente (par exemple, les ressources source et de destination sont différentes, mais partagent des mappages ou des mappages sont en double sur une surface donnée), les résultats de l’opération de copie sur les tuiles partagées ne sont pas définis.

## <a name="span-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspancopying-to-streaming-resource-with-duplicated-tiles-in-destination-area"></a><span id="Copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="COPYING_TO_STREAMING_RESOURCE_WITH_DUPLICATED_TILES_IN_DESTINATION_AREA"></span>Copie vers des ressources de diffusion en continu avec des tuiles en double dans la zone de destination


La copie vers une ressource de diffusion en continu avec des tuiles en double dans la zone de destination génère des résultats non définis dans ces tuiles, sauf si les données sont identiques. Des tuiles différentes peuvent écrire les tuiles dans des ordres différents.

## <a name="span-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanuav-accesses-to-duplicate-tiles-mappings"></a><span id="UAV_accesses_to_duplicate_tiles_mappings"></span><span id="uav_accesses_to_duplicate_tiles_mappings"></span><span id="UAV_ACCESSES_TO_DUPLICATE_TILES_MAPPINGS"></span>Accès sans ordre (UAV) aux mappages de tuiles en double


Supposons qu’un affichage d’accès sans ordre (UAV) sur une ressource de diffusion en continu compte des mappages de tuiles en double dans sa zone ou avec d’autres ressources liées au pipeline. Le classement des accès à ces tuiles en double n’est pas défini s’il est effectué par différents threads, comme tout classement d’accès sans ordre à la mémoire est généralement non trié.

## <a name="span-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanrendering-after-tile-mapping-changes-or-content-updates-from-outside-mappings"></a><span id="Rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="RENDERING_AFTER_TILE_MAPPING_CHANGES_OR_CONTENT_UPDATES_FROM_OUTSIDE_MAPPINGS"></span>Rendu après des modifications de mappage de tuiles ou mises à jour de contenu à partir de mappages externes


Si des mappages de tuiles de ressources de diffusion en continu ont été modifiés ou si le contenu de tuiles de pool de tuiles mappées a été modifié par le biais d’autres mappages de ressources de diffusion en continu, et que la ressource de diffusion en continu va être rendue via un affichage cible de rendu ou une vue de profondeur/gabarit, l’application doit effacer (à l’aide de la fonction fixe Effacer les API) ou copier entièrement à l’aide de l’opération Copy\*/Update\* APIs les tuiles qui ont été modifiées dans la zone de rendu (mappées ou non).

L’échec de la suppression ou de la copie par une application entraîne l’obsolescence des structures d’optimisation du matériel pour l’affichage cible du rendu ou la vue de profondeur/gabarit donnée et provoque le nettoyage des résultats de rendu sur certains matériels et une incohérence entre les différents matériels. Ces structures de données d’optimisation masquées utilisées par le matériel peuvent être locales pour les mappages individuels et non visibles pour les autres mappages dans la même mémoire.

Lorsque vous effacez un affichage de ressources (en définissant tous les éléments d’un affichage de ressources sur une seule valeur), vous pouvez effacer les affichages cibles de rendu avec des rectangles. Pour le matériel prenant en charge les ressources de diffusion en continu, l’effacement d’un affichage de ressource doit également inclure l’effacement des vues de gabarit/profondeur avec des rectangles, pour les surfaces de profondeur uniquement (sans gabarit). Cette opération permet aux applications d’effacer uniquement la zone d’une surface requise.

Si une application doit conserver les contenus de mémoire de zones existants dans une ressource de diffusion en continu où les mappages ont été modifiés, cette application doit contourner l’opération d’effacement. L’application peut pour ce faire enregistrer les contenus dans lesquels les mappages de tuiles ont été modifiés (en les copiant sur une surface temporaire), exécuter la commande d’effacement requise, puis copier de nouveau le contenu. Cette opération permet de conserver des contenus de surface pour un rendu incrémentiel, cependant l’inconvénient est que les performances de rendu sur la surface peuvent être réduites, car les optimisations de rendu peuvent être perdues.

Si une tuile est mappée dans plusieurs ressources de diffusion en continu en même temps et que les contenus des tuiles sont traités par tout moyen (rendu, copie, etc.) via l’une des ressources de diffusion en continu, et si cette même tuile est rendue par le biais d’une autre ressource de diffusion en continu, cette tuile doit être effacée comme décrit précédemment.

## <a name="span-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanrendering-to-tiles-shared-outside-render-area"></a><span id="Rendering_to_tiles_shared_outside_render_area"></span><span id="rendering_to_tiles_shared_outside_render_area"></span><span id="RENDERING_TO_TILES_SHARED_OUTSIDE_RENDER_AREA"></span>Rendu de tuiles partagées en dehors de la zone de rendu


Supposons qu’une zone dans une ressource de diffusion en continu est rendue et que les tuiles d’un pool de tuiles référencées par la zone de rendu sont également mappées en dehors de la zone de rendu (y compris via d’autres ressources de diffusion en continu, en même temps ou non). Les données restituées dans ces tuiles risquent de ne pas s’afficher correctement lorsqu’elles sont affichées par d’autres mappages, même si la disposition de la mémoire sous-jacente est compatible. Cela est dû aux structures de données d’optimisation utilisées par certains matériels et qui peuvent être locales pour les mappages individuels de surfaces pouvant être rendues, et non visibles par les autres mappages dans le même emplacement de mémoire.

Vous pouvez contourner cette restriction en copiant les mappages de rendu vers tous les autres mappages dans la même mémoire accessible (ou en effaçant cette mémoire ou en copiant les autres données dans cette mémoire si les anciens contenus ne sont plus nécessaires). Même si cette solution semble redondante, elle permet à tous les autres mappages de la même mémoire de comprendre comment accéder correctement à leurs contenus, et permet des économies de mémoire grâce à une seule sauvegarde de mémoire physique.

Lorsque vous utilisez différentes ressources de diffusion en continu partageant des mappages (au moins la lecture), vous devez spécifier une contrainte de classement d’accès aux données (une barrière) pour les multiples ressources à tuiles, entre les basculements.

## <a name="span-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanrendering-to-tiles-shared-within-render-area"></a><span id="Rendering_to_tiles_shared_within_render_area"></span><span id="rendering_to_tiles_shared_within_render_area"></span><span id="RENDERING_TO_TILES_SHARED_WITHIN_RENDER_AREA"></span>Rendu de tuiles partagées dans la zone de rendu


Si une zone dans une ressource de diffusion en continu est affichée et que plusieurs tuiles sont mappées dans la zone de rendu vers le même emplacement de pool de tuiles, les résultats du rendu ne sont pas définis pour ces tuiles.

## <a name="span-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspandata-compatibility-across-streaming-resources-sharing-tiles"></a><span id="Data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="DATA_COMPATIBILITY_ACROSS_STREAMING_RESOURCES_SHARING_TILES"></span>Compatibilité des données entre des ressources de diffusion en continu partageant des tuiles


Supposons que plusieurs ressources de diffusion en continu ont des mappages aux mêmes emplacements de pool de tuiles et que chaque ressource est utilisée pour accéder aux mêmes données. Ce scénario est valide uniquement si les autres règles liées à la prévention des problèmes avec les structures d’optimisation du matériel n’existent pas, des barrières appropriées sont spécifiées (avec une contrainte de classement d’accès aux données pour les multiples ressources à tuiles) et les ressources de diffusion en continu sont compatibles entre elles.

L’incompatibilité des ressources de diffusion en continu partageant des tuiles est expliquée ici. Les conditions d’incompatibilité d’accès aux mêmes données entre des mappages de tuiles en double reposent sur l’utilisation de différents formats ou dimensions de surface ou sur la présence ou non d’indicateurs de liaison de profondeur/gabarit ou de rendu cible dans les ressources. L’écriture dans la mémoire avec un type de mappage produit des résultats non définis si vous lisez ou effectuez un rendu via un mappage d’une ressource incompatible.

Si les autres ressource partageant des mappages sont initialisées avec de nouvelles données (recyclage de la mémoire pour un rôle différent), les opérations de lecture ou de rendu suivantes sont correctes dans la mesure où les données ne sont pas fusionnées dans des interprétations incompatibles. Toutefois, lorsque vous basculez entre l’accès à des mappages incompatibles, vous devez spécifier des barrières (avec une contrainte de classement d’accès de données entre plusieurs ressources à tuiles).

Si l’indicateur de liaison de profondeur/gabarit ou de rendu cible n’est pas défini sur les ressources partageant des mappages, il existe beaucoup moins de restrictions. Dans la mesure où les types de format et de surface (par exemple, Texture2D) sont identiques, les tuiles peuvent être partagées. Les différents formats compatibles sont les surfaces BC\* et le format équivalent non compressé de 32bits ou 16bits par composant, comme BC6H et R32G32B32A32. De nombreux formats de 32bits par élément peuvent disposer d’un alias avec R32\_\*, ainsi que (R10G10B10A2\_\*, R8G8B8A8\_\*, B8G8R8A8\_\*,B8G8R8X8\_\*,R16G16\_\*). Cette opération a toujours été autorisée pour les ressources non à diffusion en continu.

Le partage entre des tuiles compressées et non compressées fonctionne correctement si les formats sont compatibles et si les tuiles sont remplies avec une couleur unie.

Enfin, si des ressources partageant des mappages de tuiles n’ont rien en commun, sauf qu’aucune ne présente d’indicateur de liaison de profondeur/gabarit ou de rendu cible, seule une mémoire remplie avec 0 peut être partagée en toute sécurité. Le mappage s’affiche avec une valeur de décodage de0 pour la définition du format de ressource donné (généralement 0).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Accès du pipeline aux ressources de diffusion en continu](pipeline-access-to-streaming-resources.md)

 

 




