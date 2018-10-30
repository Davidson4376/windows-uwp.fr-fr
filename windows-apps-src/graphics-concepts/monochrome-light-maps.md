---
title: Mappages de lumière monochrome
description: Les mappages de lumière monochrome permettent à des cartes plus anciennes d’exécuter des fusions de textures multipasse, lorsqu’une carte d’accélérateur3D ne prend pas en charge la fusion de textures à l’aide de la valeur alpha du pixel de destination.
ms.assetid: 60F8F8F6-9DB7-452B-8DC0-407FFAA4BFE1
keywords:
- Cartes de lumière monochrome
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 72bea3badb19991883e2a6cc62463ab2dc58ec4a
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5759535"
---
# <a name="monochrome-light-maps"></a>Cartes de lumière monochrome


Les mappages de lumière monochrome permettent à des cartes plus anciennes d’exécuter des fusions de textures multipasse, lorsqu’une carte d’accélérateur3D ne prend pas en charge la fusion de textures à l’aide de la valeur alpha du pixel de destination.

Certaines cartes d’accélérateur 3D plus anciennes ne prennent pas en charge la fusion de textures à l’aide de la valeur alpha du pixel de destination. Ces cartes, généralement, ne prennent pas en charge la fusion de textures multiples. Si votre application est exécutée sur une carte de ce type, elle peut recourir à la fusion de textures multipasse pour exécuter le mappage de lumière monochrome.

Pour exécuter un mappage de lumière monochrome, une application stocke les informations d’éclairage dans les données alpha de ses textures de mappage lumineux. L’application valorise les fonctionnalités de filtrage de texture de Direct3D pour exécuter un mappage de chaque pixel de l’image de la primitive sur un texel correspondant de la carte de lumière. Elle définit le facteur de fusion source sur la valeur alpha du texel correspondant.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Mappage lumineux avec textures](light-mapping-with-textures.md)

 

 




