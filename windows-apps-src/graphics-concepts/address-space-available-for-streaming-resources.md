---
title: Espace d’adresse disponible pour les ressources de diffusion en continu
description: Cette section spécifie l’espace d’adresse virtuel disponible pour les ressources de diffusion en continu.
ms.assetid: 145EB4A3-3910-4126-BC7E-A4CF53E2A098
keywords:
- Espace d’adresse disponible pour les ressources de diffusion en continu
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0b6e3f8080d33f9aadf22d5d5b1ebdd9a4739e16
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6996942"
---
# <a name="address-space-available-for-streaming-resources"></a>Espace d’adresse disponible pour les ressources de diffusion en continu


Cette section spécifie l’espace d’adresse virtuel disponible pour les ressources de diffusion en continu.

Les systèmes d’exploitation 64bits offrent au moins 40bits d’espace d’adresse virtuel (1téraoctet) disponible.

Pour les systèmes d’exploitation 32bits, l’espace d’adresse est de 32bits (4Go). Pour les systèmesARM 32bits, la création de ressources de diffusion en continu individuelles peut échouer si l’allocation utilise plus de 27bits d’espace d’adresse (128Mo). Ceci inclut tout remplissage masqué dans l’espace d’adresse que le matériel peut utiliser pour les mipmaps, le remplissage de vignettes compressées ainsi que, éventuellement, les dimensions de surfaces de remplissage à des puissances de 2.

Sur les systèmes graphiques comportant une table de page distinctes pour le processeur graphique (GPU), la majeure partie de cet espace d’adresse sera disponible aux ressources GPU utilisées par l’application, même si les allocations de GPU effectuées par le pilote d’affichage tiennent dans le même espace.

Sur les futurs systèmes où la table de pages sera partagée entre l'UC et le processeur graphique, l’espace d’adresse disponible sera partagée entre toutes les allocations UC et GPU d'un processus.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Paramètres de création de ressources de diffusion en continu](streaming-resource-creation-parameters.md)

 

 




