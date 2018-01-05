---
title: "Opérations disponibles sur les pools de vignettes"
description: "Vous pouvez redimensionner un pool de vignettes, offrir des ressources (octroi temporaire de mémoire au système pour l’intégralité du pool de vignettes) et récupérer des ressources."
ms.assetid: 90347F7F-C991-4287-BD70-494533ECDC8A
keywords: "Opérations disponibles sur les pools de vignettes"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3ae2c34a6df243914fdef6b2cbb990d8be94ec33
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
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

 

 




