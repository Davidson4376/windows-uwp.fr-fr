---
title: Compression de mipmaps
description: Plusieurs mips (par tranche de tableau) peuvent être compressées dans plusieurs vignettes, en fonction des dimensions et du format de la ressource de diffusion en continu, du nombre de mipmaps et des tranches de tableau.
ms.assetid: 906C3CAC-4E84-4947-B508-06788551BE85
keywords:
- Compression de mipmaps
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8b2733da1f843062a1fa7f2b4a7969326523d54e
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8692341"
---
# <a name="mipmap-packing"></a>Compression de mipmaps


Plusieurs mips (par tranche de tableau) peuvent être compressées dans plusieurs vignettes, en fonction des dimensions et du format de la ressource de diffusion en continu, du nombre de mipmaps et des tranches de tableau.

En fonction du [niveau](streaming-resources-features-tiers.md) de prise en charge des ressources de diffusion en continu, les mipmaps de certaines dimensions ne respectent pas la forme standard des vignettes. Le cas échéant, elles sont compressées ensemble, selon une méthode inconnue de l’application. Des niveaux supérieurs de prise en charge procurent de meilleures garanties d’intégration des dimensions des types de surface dans le format standard des vignettes; un mappage individuel par les applications est alors possible.

Quelles sont les différences entre les implémentations? En fait, en fonction des dimensions et du format de la ressource, du nombre de mipmaps et des tranches de tableau, un nomM de mips (par tranche de tableau) peut être compressé dans un nombreN de vignettes. Lorsque vous récupérez les informations sur les vignettes des ressources pour un appareil donné, le pilote communique à l’application les valeurs M et N (parmi d’autres données relatives à la surface, indépendantes du fournisseur matériel). L’ensemble de vignettes correspondant aux mips compressés représente toujours 64Ko; les éléments peuvent être mappés individuellement à des emplacements distincts du pool de vignettes.

Cependant, la forme des pixels des vignettes et la manière dont les mipmaps s’intègrent dans l’ensemble de vignettes, spécifiques à chaque fournisseur matériel, sont trop complexes à présenter. Par conséquent, les applications doivent mapper aucune ou l’ensemble (simultanément) des vignettes considérées comme compressées. Dans le cas contraire, le comportement d’accès à la ressource de diffusion en continu reste indéfini.

Pour les surfaces sous forme de tableaux, l’ensemble de mips compressés et le nombre de vignettes stockant ces mips (M et N, tel que décrit précédemment) sont des valeurs s’appliquant séparément pour chaque tranche de tableau.

Les API dédiées à la copie des vignettes n’ont pas accès aux mips compressées. Les applications cherchant à copier des données vers et à partir des mips compressées peuvent solliciter les API dédiées aux ressources autres que celles de diffusion en continu pour la copie et le rendu sur les surfaces.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque](how-a-streaming-resource-s-area-is-tiled.md)

 

 




