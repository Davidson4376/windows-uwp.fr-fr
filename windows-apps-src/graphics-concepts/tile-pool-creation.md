---
title: "Création d’un pool de tuiles"
description: "Les applications peuvent créer un ou plusieurs pools de tuiles par appareil Direct3D. La taille totale de chaque pool de tuiles est limitée à la taille des ressources de Direct3D 11, qui est d’environ 1/4 de RAM de l’unité de traitement graphique (GPU)."
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords: "Création d’un pool de tuiles"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4336d38bca354da3c30cfe2d7e4b092cff15af83
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
---
# <a name="tile-pool-creation"></a>Création d’un pool de tuiles


Les applications peuvent créer un ou plusieurs pools de tuiles par appareil Direct3D. La taille totale de chaque pool de tuiles est limitée à la taille des ressources de Direct3D 11, qui est d’environ 1/4 de RAM de l’unité de traitement graphique (GPU).

Un pool de tuiles est constitué de tuiles de 64Ko, mais le système d’exploitation (pilote d’affichage) gère la totalité du pool en tant qu’une ou plusieurs allocations en arrière-plan. Les détails ne sont pas visibles pour les applications. Les ressources de diffusion en continu définissent le contenu en indiquant les tuiles au sein d’un pool de tuiles. La suppression du mappage d’une tuile à partir d’une ressource de diffusion en continu est effectuée en définissant la tuile sur **NULL**. Ces tuiles non mappées disposent de règles liées au comportement des lectures ou écritures; voir [Suivi des risques ou ressources d’un pool de tuiles](hazard-tracking-versus-tile-pool-resources.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Mappages dans un pool de tuiles](mappings-are-into-a-tile-pool.md)

 

 




