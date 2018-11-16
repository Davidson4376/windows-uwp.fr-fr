---
title: Fusion de textures multipasse
description: Les applications Direct3D peuvent obtenir de nombreux effets spéciaux en appliquant différentes textures à une primitive au cours de plusieurs passages de rendu.
ms.assetid: FB4D6E3F-4EF5-4D20-BF7E-1008E790E30C
keywords:
- Fusion de textures multipasse
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c55f371e97daba5f81945812f8179eb708bbadd6
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7099823"
---
# <a name="multipass-texture-blending"></a>Fusion de textures multipasse


Les applications Direct3D peuvent obtenir de nombreux effets spéciaux en appliquant différentes textures à une primitive au cours de plusieurs passages de rendu. Cette opération est généralement appelée *fusion de textures multipasse*. La fusion de textures multipasse permet généralement d’émuler les effets des modèles d’éclairage et d’ombrage complexes en appliquant plusieurs couleurs à partir de plusieurs textures différentes. Une telle application est appelée *mappage lumineux*. Voir [Mappage lumineux avec textures](light-mapping-with-textures.md).

**Remarque**  certains appareils sont en mesure d’appliquer plusieurs textures aux primitives dans une seule passe. Voir [Fusion de textures](texture-blending.md).

 

Si le matériel de l’utilisateur ne prend pas en charge la fusion de plusieurs textures, votre application peut recourir à la fusion de textures multipasse pour obtenir les mêmes effets visuels. Toutefois, l’application ne prend pas en charge les fréquences d’images proposées par la fusion de textures multiples.

Pour effectuer la fusion de textures multipasse dans une application C/C++:

1.  Définissez une texture sur l’étape 0 de texture.
2.  Sélectionnez la couleur souhaitée, ainsi que les arguments et les opérations de fusion alpha. Les paramètres par défaut sont parfaitement adaptés à la fusion de textures multipasse.
3.  Affichez les objets appropriés dans la scène.
4.  Définissez la texture suivante sur l’étape0 de texture.
5.  Définissez les états de rendu afin de régler, au besoin, les facteurs de fusion source et de destination. Le système fusionne les nouvelles textures avec les pixels existants dans la surface de rendu cible, suivant ces paramètres.
6.  Répétez les étapes3, 4 et 5 avec autant de textures que nécessaire.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Fusion de textures](texture-blending.md)

 

 




