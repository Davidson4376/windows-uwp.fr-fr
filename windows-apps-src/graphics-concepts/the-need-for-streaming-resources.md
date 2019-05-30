---
title: Besoin en ressources de diffusion en continu
description: Les ressources de diffusion en continu sont nécessaires, afin que la mémoire GPU ne gaspille pas d'espace en stockant des régions de surfaces qui ne sont pas accessibles et pour indiquer au matériel comment filtrer les vignettes adjacentes.
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords:
- Besoin en ressources de diffusion en continu
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f5b44e60e3490f39a91724bf038aa8066de11bf0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370889"
---
# <a name="the-need-for-streaming-resources"></a>Besoin en ressources de diffusion en continu


Les ressources de diffusion en continu sont nécessaires, afin que la mémoire GPU ne gaspille pas d'espace en stockant des régions de surfaces qui ne sont pas accessibles et pour indiquer au matériel comment filtrer les vignettes adjacentes.

## <a name="span-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>Diffusion en continu de ressources ou des textures partiellement alloués


*Les ressources de diffusion en continu* (appelées *tiled resources* (ressources en tuiles) dans Direct3D 11), sont de grandes ressources logiques utilisant de petites quantités de mémoire physique.

Les ressources de diffusion en continu sont également appelées *textures fragmentées*. Le terme « fragmenté » indique à la fois que les ressources sont séparées en tuiles, ainsi que la raison principale de leur fragmentation : toutes les ressources ne seront pas mappées en même temps. Une application peut ainsi créer intentionnellement une ressource de diffusion en continu dans laquelle aucune donnée n’est consignée pour les régions et mips de la ressource. Par conséquent, le contenu peut être fragmenté, et le mappage du contenu dans la mémoire de l’unité de traitement graphique (GPU) à un moment donné est un sous-ensemble de ce contenu (encore plus fragmenté).

## <a name="span-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>Sans mosaïque, les allocations de mémoire sont gérées au niveau de granularité subresource


Dans un système graphique (autrement dit, le système d’exploitation, le pilote d’affichage et le matériel graphique) ne prenant pas en charge des ressources de diffusion en continu, le système graphique gère toutes les allocations de mémoire de Direct3D au niveau de la granularité des sous-ressources.

Pour une [mise en mémoire tampon](introduction-to-buffers.md), l’ensemble des données représente la sous-ressource.

Pour une [texture](textures.md) (par exemple, une [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d)), chaque niveau mip représente une sous-ressource. Pour un tableau de textures (par exemple [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray)), chaque niveau mip à une section de tableau donnée est une sous-ressource. Le système graphique permet uniquement de gérer le mappage des allocations à ce niveau de granularité de sous-ressources. Dans le contexte des ressources de diffusion en continu, le « mappage » fait référence à la visibilité des données par le GPU.

## <a name="span-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>Sans mosaïque, ne peut pas accéder uniquement une petite portion de chaîne mipmap


Supposons qu’une application sait qu’une opération de rendu spécifique nécessite d’accéder uniquement à une petite partie de la chaîne de mipmap d’une image (peut-être même pas la zone entière d’un mipmap donné). Dans l’idéal, l’application pourrait informer le système graphique de ce besoin. Le système graphique serait ensuite uniquement chargé de s’assurer que la mémoire nécessaire est mappée sur le GPU, sans effectuer une pagination avec trop de mémoire.

En réalité, sans la prise en charge des ressources de diffusion en continu, le système graphique peut uniquement être informé de la mémoire devant être mappée sur le GPU au niveau de granularité des sous-ressources (par exemple, une plage de niveaux de mipmap complète accessible). Il n’existe aucun défaut de demande dans le système graphique, donc potentiellement une grande quantité de mémoire GPU en excès doit être utilisée pour mapper des sous-ressources complètes avant l’exécution d’une commande de rendu référençant les parties de la mémoire. Il s’agit simplement d’un problème qui complique l’utilisation de larges allocations de mémoire dans Direct3D, sans la prise en charge des ressources de diffusion en continu.

## <a name="span-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>Pagination de logiciels pour diviser la surface en plus petits carreaux


La pagination logicielle permet de diviser la surface en tuiles suffisamment petites pour le matériel utilisé.

Direct3D prend en charge les surfaces [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) avec jusqu’à 16 384 pixels sur un côté donné. Une image de 16 384 de large par 16 384 de haut et de 4 octets par pixel consomme 1 Go de mémoire vidéo (et l’ajout de mipmaps double cette consommation). Dans la pratique, il est rare d’utiliser la totalité d’1 Go dans une simple opération de rendu.

Certains développeurs de jeu modèlent des surfaces de terrain d’une taille de 128 Ko par 128 Ko. Pour faire fonctionner ces surfaces sur le GPU existant, il faut diviser la surface en tuiles suffisamment petites pour le matériel utilisé. L’application doit déterminer les tuiles pouvant être nécessaires et les charger dans un cache de textures sur le GPU, un système de pagination logiciel.

Un inconvénient significatif de cette approche est proposée par le matériel ne sachant même ne pas quoi que ce soit sur la pagination qui se passe : Lorsqu’une partie d’une image doit être affiché à l’écran qui rapproche les vignettes, le matériel ne sait pas comment exécuter une fonction fixe (autrement dit, efficace) filtrage dans les mosaïques. Cela signifie que l’application gérant sa propre fragmentation logicielle doit recourir à un filtrage de texture manuel dans le code de nuanceur (ce qui est très coûteux si vous souhaitez utiliser un filtre anisotropiques de bonne qualité) et/ou perdre les marges d’écriture de la mémoire pour les tuiles contenant des données de tuiles voisines, afin que le filtrage matériel de fonction fixe puisse continuer à fournir une assistance.

## <a name="span-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>Fabrication en mosaïque représentation sous forme de surfaces allocations une fonctionnalité de première classe


Si la représentation en tuiles des allocations de surface est une fonctionnalité de première classe dans le système graphique, l’application peut indiquer au matériel les tuiles nécessaires. De cette façon, moins de mémoire GPU est gaspillée dans le stockage de régions de surfaces auxquelles l’application n’aura pas accès et le matériel peut comprendre comment filtrer les tuiles adjacentes, réduisant ainsi les difficultés rencontrées par les développeurs exécutant eux-mêmes la fragmentation logicielle.

Toutefois, pour fournir une solution complète, un problème doit être réglé. En effet, indépendamment de la prise en charge de la fragmentation dans une surface, la dimension de surface maximale est actuellement de 16 384, bien loin des plus de 128K souhaités pour les applications. Exiger le matériel nécessaire pour prendre en charge des tailles de texture supérieures peut représenter une approche. Cependant, les coûts et/ou compromis de cette solution sont élevés.

Le chemin de filtre de texture et le chemin du rendu de Direct3D sont déjà saturés en termes de précision à cause de la prise en charge des textures 16K et autres conditions requises, telles que la prise en charge des extensions de fenêtres d’affichage réduisant la surface au cours du rendu ou la prise en charge de l’habillage de texture du bord de la surface pendant le filtrage. Il est possible d’établir un compromis : lorsque la taille de texture augmente au-delà de 16 Ko, la fonctionnalité ou la précision est réduite en conséquence. Toutefois, même avec cette concession, des coûts matériels supplémentaires peuvent être nécessaires pour que le système matériel puisse prendre en charge des tailles de texture supérieures.

## <a name="span-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>Problème avec les textures volumineux : précision pour les emplacements sur la surface


Le problème qui se pose lorsque les textures deviennent très larges est que les coordonnées de texture à virgule flottante (et les interpolateurs associés pour la prise en charge de la rastérisation) manquent de précision pour spécifier des emplacements exacts sur la surface. Le filtrage de textures risque d’être instable. La prise en charge d’un double interpolateur de précision serait une option coûteuse. Il existe une alternative raisonnable.

## <a name="span-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>L’activation de plusieurs ressources de différentes dimensions pour partager la mémoire


Un autre scénario peut être choisi pour les ressources de diffusion en continu : l’activation de plusieurs ressources de différents formats/dimensions pour partager la même mémoire. Les applications disposent parfois d’ensembles de ressources qui ne sont pas utilisés en même temps ou de ressources qui sont créées uniquement pour une utilisation très courte, puis qui sont détruites et remplacées par d’autres ressources créées. Indépendamment des « ressources de diffusion en continu », il est généralement possible de permettre à l’utilisateur de pointer plusieurs ressources différentes vers la même mémoire (en chevauchement). En d’autres termes, la création et la destruction de « ressources » (qui définissent une dimension/un format, etc.) peuvent être dissociées de la gestion de la mémoire à la base des ressources du point de vue de l’application.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de streaming](streaming-resources.md)

 

 




