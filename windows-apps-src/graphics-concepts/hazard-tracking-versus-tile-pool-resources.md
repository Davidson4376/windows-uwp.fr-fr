---
title: Suivi des risques ou ressources d’un pool de vignettes
description: Pour les ressources autres que celles de diffusion en continu, Direct3D peut pallier certaines problématiques durant le rendu. Parallèlement, le suivi des risques se déroulant au niveau vignette pour les ressources de diffusion en continu, cette activité pourrait se révéler trop coûteuse sur ces dernières.
ms.assetid: 8B0C73D3-3F77-41E8-B17D-C595DEE39E49
keywords:
- Suivi des risques ou ressources d’un pool de vignettes
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e2ff4e2ff079e15c0af41e3232c4b70c582a6a25
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7153687"
---
# <a name="hazard-tracking-versus-tile-pool-resources"></a>Suivi des risques ou ressources d’un pool de vignettes


Pour les ressources autres que celles de diffusion en continu, Direct3D peut pallier certaines problématiques durant le rendu. Parallèlement, le suivi des risques se déroulant au niveau vignette pour les ressources de diffusion en continu, cette activité pourrait se révéler trop coûteuse sur ces dernières.

Par exemple, dans le cas des ressources autres que celles de diffusion en continu, le runtime n’autorise aucun élément SubResource donné à être lié à la fois en tant qu’entrée (telle qu’une vue de ressource de nuanceur) et en tant que sortie (par exemple, une vue de cible de rendu). Si cette situation se produit, le runtime annule la liaison de l’entrée. Ce traitement supplémentaire associé au suivi dans le runtime est bon marché et s’effectue au niveau sous-ressource. L’un des avantages de ce suivi réside dans la réduction des risques que des applications dépendent accidentellement de l’ordre d’exécution des nuanceurs sur le matériel. L’ordre d’exécution des nuanceurs sur le matériel peut varier, sinon sur un processeur graphique (GPU) donné, du moins entre différents GPU.

Le suivi de la façon dont les ressources sont liées peut se révéler trop coûteux pour les ressources de diffusion en continu, car le suivi s’effectue au niveau vignette. De nouveaux problèmes font leur apparition, tels que la validation possible de tentatives de rendu sur une vue de cible de rendu avec le mappage d’une même vignette sur plusieurs zones de la surface simultanément. Si le suivi des risques par vignette se révèle trop onéreux pour le runtime, dans l’idéal, ceci constituerait au moins une option dans la couche de débogage.

Lorsqu’une application a soumis une opération d’écriture ou de lecture à une ressource de diffusion en continu référençant une mémoire de pool de vignettes qui sera également référencée par d’autres ressources de diffusion en continu dans de futures opérations de lecture ou d’écriture, cette application doit informer le pilote d’affichage qu’elle attend la fin du traitement de la première opération avant que les opérations suivantes puissent démarrer.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Mappages dans un pool de vignettes](mappings-are-into-a-tile-pool.md)

 

 




