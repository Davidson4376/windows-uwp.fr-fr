---
title: Création d’un pool de tuiles
description: Les applications peuvent créer un ou plusieurs pools de vignettes par appareil Direct3D. La taille totale de chaque pool de tuiles est limitée à la limite de taille de ressource de Direct3D11, qui est d’environ 1/4 de l’unité de traitement graphique (GPU) RAM.
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- Création d’un pool de tuiles
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5ce3824ab2d435b42df9586a6c229b68db10a0c9
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8796500"
---
# <a name="tile-pool-creation"></a>Création d’un pool de tuiles


Les applications peuvent créer un ou plusieurs pools de vignettes par appareil Direct3D. La taille totale de chaque pool de tuiles est limitée à la limite de taille de ressource de Direct3D11, qui est d’environ 1/4 de l’unité de traitement graphique (GPU) RAM.

Un pool de tuiles est constitué de tuiles de 64Ko, mais le système d’exploitation (pilote d’affichage) gère la totalité du pool en tant qu’une ou plusieurs allocations en arrière-plan. Les détails ne sont pas visibles pour les applications. Les ressources de diffusion en continu définissent le contenu en indiquant les tuiles au sein d’un pool de tuiles. La suppression du mappage d’une tuile à partir d’une ressource de diffusion en continu est effectuée en définissant la tuile sur **NULL**. Ces tuiles non mappées disposent de règles liées au comportement des lectures ou écritures; voir [Suivi des risques ou ressources d’un pool de tuiles](hazard-tracking-versus-tile-pool-resources.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Mappages dans un pool de vignettes](mappings-are-into-a-tile-pool.md)

 

 




