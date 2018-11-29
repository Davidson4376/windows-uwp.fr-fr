---
title: Opérations disponibles sur les pools de vignettes
description: Vous pouvez redimensionner un pool de vignettes, offrir des ressources (octroi temporaire de mémoire au système pour l’intégralité du pool de vignettes) et récupérer des ressources.
ms.assetid: 90347F7F-C991-4287-BD70-494533ECDC8A
keywords:
- Opérations disponibles sur les pools de vignettes
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6b8b0c6f4fa578e4ec483492b320dc9bc346ab66
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7989112"
---
# <a name="operations-available-on-tile-pools"></a>Opérations disponibles sur les pools de vignettes


Vous pouvez redimensionner un pool de vignettes, offrir des ressources (octroi temporaire de mémoire au système pour l’intégralité du pool de vignettes) et récupérer des ressources.

-   La durée de vie des pools de ressources est similaire à celles des autres ressourcesDirect3D, avec l’appui du décompte de référence, comprenant dans ce cas le suivi des mappages des ressources de diffusion en continu. Dans les situations où l’application ne référence plus aucun pool de vignettes, tandis que les mappages de vignettes sur la mémoire sont supprimés et que les accès aux unités de traitement graphique(GPU) sont octroyés, le système d’exploitation libère le pool de vignettes.
-   Les API associées au partage et à la synchronisation des surfaces fonctionnent pour les pools de vignettes (pas directement sur les ressources de diffusion en continu). Les commandesDirect3D qui accèdent aux ressources de diffusion en continu pointant vers un pool de vignettes sont abandonnées si le pool a été partagé et est actuellement acquis par un autre appareil ou processus; ce comportement est similaire à celui respecté pour les pools de vignettes offerts.
-   Redimensionnement d’un pool de vignettes.
-   Offre et récupération de ressources - Ces opérations d’octroi temporaire de mémoire au système s’exécutent sur l’intégralité du pool de vignettes; elles ne sont pas disponibles pour les ressources individuelles de diffusion en continu. Si une ressource de diffusion en continu pointe vers une vignette d’un pool de vignettes offert, ladite ressource se comporte comme si elle était offerte (par exemple, l’exécution abandonne la commande qui la référence).

Les données ne peuvent pas être copiées directement d’une mémoire de pool de vignettes ou à partir de cette dernière. Les accès à la mémoire sont toujours octroyés via les ressources de diffusion en continu.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Création de ressources de diffusion en continu](creating-streaming-resources.md)

 

 




