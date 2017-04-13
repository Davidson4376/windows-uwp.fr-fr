---
title: "Opérations disponibles sur les ressources de diffusion en continu"
description: "Cette section répertorie les opérations pouvant être effectuées sur les ressources de diffusion en continu."
ms.assetid: 700D8C54-0E20-4B2B-BEA3-20F6F72B8E24
keywords: "Opérations disponibles sur les ressources de diffusion en continu"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 86f610131d406db69cfcf23be51fca839d889772
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="operations-available-on-streaming-resources"></a>Opérations disponibles sur les ressources de diffusion en continu


Cette section répertorie les opérations pouvant être effectuées sur les ressources de diffusion en continu.

-   Opérations de mise à jour et de copie de mappages de vignettes renvoyant un void - Ces opérations dirigent des emplacements de vignettes d’une ressource de diffusion en continu vers des emplacements de pools de vignettes, ou vers NULL, ou vers les deux. Ces opérations peuvent mettre à jour un sous-réseau disjoint des pointeurs de vignettes.
-   Opérations de copie et de mise à jour - L’ensemble des API pouvant copier des données vers et à partir d’une surface de pool par défaut fonctionnent pour les ressources en continu. Les opérations de lecture à partir de vignettes non mappées produisent des valeurs nulles; les écritures sur les vignettes non mappées sont abandonnées.
-   Opérations de copie et de mise à jour de vignettes - Ces opérations existent pour la copie de vignettes à une granularité de 64Ko vers et à partir des ressources de diffusion en continu et de mémoire tampon dans une disposition de mémoire canonique. Le pilote d’affichage et le matériel exécutent le «panachage» de mémoire requis par la ressource de diffusion en continu.
-   Les liaisons de pipelineDirect3D et les liaisons/créations de vues qui s’avéreraient efficaces sur des ressources autres fonctionnent également sur les ressources de diffusion en continu.

Les contrôles de vignettes sont disponibles sur les contextes immédiats ou différés (tout comme les mises à jour de ressources standard) et sur les accès aux vignettes (non auparavant soumises) postérieurs à l’exécution.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Création de ressources de diffusion en continu](creating-streaming-resources.md)

 

 




