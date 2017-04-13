---
title: "Création d’un pool de tuiles"
description: "Les applications peuvent créer un ou plusieurs pools de tuiles par appareil Direct3D. La taille totale de chaque pool de tuiles est limitée à la taille des ressources de Direct3D 11, qui est d’environ 1/4 de RAM de l’unité de traitement graphique (GPU)."
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords: "Création d’un pool de tuiles"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 315c1b2e1a2b8c89b432a89278ae1b3b240c5ad5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="tile-pool-creation"></a>Création d’un pool de tuiles


Les applications peuvent créer un ou plusieurs pools de tuiles par appareil Direct3D. La taille totale de chaque pool de tuiles est limitée à la taille des ressources de Direct3D 11, qui est d’environ 1/4 de RAM de l’unité de traitement graphique (GPU).

Un pool de tuiles est constitué de tuiles de 64Ko, mais le système d’exploitation (pilote d’affichage) gère la totalité du pool en tant qu’une ou plusieurs allocations en arrière-plan. Les détails ne sont pas visibles pour les applications. Les ressources de diffusion en continu définissent le contenu en indiquant les tuiles au sein d’un pool de tuiles. La suppression du mappage d’une tuile à partir d’une ressource de diffusion en continu est effectuée en définissant la tuile sur **NULL**. Ces tuiles non mappées disposent de règles liées au comportement des lectures ou écritures; voir [Suivi des risques ou ressources d’un pool de tuiles](hazard-tracking-versus-tile-pool-resources.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Mappages dans un pool de tuiles](mappings-are-into-a-tile-pool.md)

 

 




