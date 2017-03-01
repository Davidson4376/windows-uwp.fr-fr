---
title: "Espace d’adresse disponible pour les ressources de diffusion en continu"
description: "Cette section spécifie l’espace d’adresse virtuel disponible pour les ressources de diffusion en continu."
ms.assetid: 145EB4A3-3910-4126-BC7E-A4CF53E2A098
keywords:
- "Espace d’adresse disponible pour les ressources de diffusion en continu"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c120e79518abfed35cbb6b0da6fc659b13602a3f
ms.lasthandoff: 02/07/2017

---

# <a name="address-space-available-for-streaming-resources"></a>Espace d’adresse disponible pour les ressources de diffusion en continu


Cette section spécifie l’espace d’adresse virtuel disponible pour les ressources de diffusion en continu.

Les systèmes d’exploitation 64 bits offrent au moins 40 bits d’espace d’adresse virtuel (1 téraoctet) disponible.

Pour les systèmes d’exploitation 32 bits, l’espace d’adresse est de 32 bits (4 Go). Pour les systèmes ARM 32 bits, la création de ressources de diffusion en continu individuelles peut échouer si l’allocation utilise plus de 27 bits d’espace d’adresse (128 Mo). Ceci inclut tout remplissage masqué dans l’espace d’adresse que le matériel peut utiliser pour les mipmaps, le remplissage de vignettes compressées ainsi que, éventuellement, les dimensions de surfaces de remplissage à des puissances de 2.

Sur les systèmes graphiques comportant une table de page distinctes pour le processeur graphique (GPU), la majeure partie de cet espace d’adresse sera disponible aux ressources GPU utilisées par l’application, même si les allocations de GPU effectuées par le pilote d’affichage tiennent dans le même espace.

Sur les futurs systèmes où la table de pages sera partagée entre l'UC et le processeur graphique, l’espace d’adresse disponible sera partagée entre toutes les allocations UC et GPU d'un processus.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Paramètres de création de ressources de diffusion en continu](streaming-resource-creation-parameters.md)

 

 





