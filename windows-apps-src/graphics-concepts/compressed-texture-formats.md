---
title: Formats de texture compressés
description: Cette section contient des informations sur l’organisation interne des formats de texture compressés.
ms.assetid: 24D17B9F-8CA7-4006-9E0F-178C6B3CAEC9
keywords:
- Formats de texture compressés
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3171eba376911157a6ad2687fe3879df751615ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662194"
---
# <a name="compressed-texture-formats"></a>Formats de texture compressés


Cette section contient des informations sur l’organisation interne des formats de texture compressés. Vous n'avez pas besoin de ces informations pour utiliser des textures compressées, car vous pouvez utiliser les fonctions Direct3D pour effectuer la conversion entre les formats compressés. Ces informations vous seront cependant utiles si vous souhaitez utiliser directement des données de surface compressées.

Direct3D utilise un format de compression qui divise les mappages de texture en blocs de 4 x 4 texels. Si la texture ne contient pas de transparence (si elle est opaque) ou si la transparence est spécifiée par un alpha 1 bit, un bloc de 8 octets représente le bloc de la carte de texture. Si la carte de texture contient des texels transparents, elle est représentée par un bloc de 16 octets utilisant un canal alpha.

Toute texture unique doit spécifier que ses données sont stockées sous la forme de 64 ou 128 bits par groupe de 16 texels. Si des blocs de 64 bits (format BC1) sont utilisés pour la texture, il est possible de combiner les formats opaque et alpha 1 bit pour chaque bloc au sein de la même texture. En d’autres termes, la comparaison de l’ampleur de l’entier non signé de couleur\_0 et la couleur\_1 est effectuée de façon unique pour chaque bloc de 16 texels.

Lorsque des blocs de 128 bits sont utilisés, le canal alpha doit être spécifié au format explicite (BC2) ou en mode interpolé (format BC3) pour l'ensemble de la texture. Comme pour la couleur, lorsque le mode interpolé est sélectionné, il est possible d'appliquer, bloc par bloc, le mode huit valeurs alpha interpolées ou six valeurs alpha interpolées. À nouveau la comparaison de grandeur d’une valeur alpha\_alpha et 0\_1 s’effectue de façon unique sur une base de bloc par bloc.

Le pas pour les formats BCn est mesuré en octets (et non en blocs). Par exemple, si vous avez une largeur de 16, puis avoir une tonalité de quatre blocs (4\*8 pour BC1, 4\*16 pour BC2 ou BC3).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de texture compressé](compressed-texture-resources.md)

 

 




