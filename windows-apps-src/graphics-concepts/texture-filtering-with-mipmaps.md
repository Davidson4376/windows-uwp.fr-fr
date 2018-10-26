---
title: Filtrage de textures avec mipmaps
description: Un mipmap est une séquence de textures, chacune étant une représentation de résolution progressivement inférieure de la même image. La hauteur et la largeur de chaque image, ou le niveau dans le mipmap sont deux fois plus petits que le niveau précédent.
ms.assetid: 28E863A2-C776-40E4-8551-9851DF7EC93E
keywords:
- Filtrage de textures avec mipmaps
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d32d5a77fe9bc840ea676c7156c1b59e498d07e1
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5571638"
---
# <a name="texture-filtering-with-mipmaps"></a>Filtrage de textures avec mipmaps


Un *mipmap* est une séquence de textures, chacune étant une représentation de résolution progressivement inférieure de la même image. La hauteur et la largeur de chaque image (ou niveau) dans le mipmap sont deux fois plus petits que le niveau précédent. Les mipmaps ne sont pas nécessairement carrés.

Une image de mipmap haute résolution est utilisée pour les objets qui sont proches de l’utilisateur. Des images de résolution inférieure sont utilisées lorsque l’objet apparaît plus loin. La fonction mipmap améliore la qualité de rendu des textures, mais utilise plus de mémoire.

Direct3D représente les mipmaps comme une chaîne de surfaces jointes. La texture à la résolution la plus élevée est à la tête de la chaîne et le niveau suivant du mipmap y est joint. À son tour, ce niveau est joint au niveau suivant dans le mipmap, et ainsi de suite jusqu’au niveau de résolution le plus bas du mipmap.

Les illustrations suivantes montrent un exemple de ces niveaux. Les textures bitmap représentent un panneau sur un conteneur dans un jeu 3D à la première personne. Lorsqu’elle est créée en tant que mipmap, la texture de la résolution la plus élevée figure en premier dans l’ensemble. Chaque texture suivante dans l’ensemble de mipmaps est deux fois plus petite en hauteur et en largeur. Dans ce cas, le mipmap de résolution maximale est de 256x256pixels. La texture suivante est de 128x128. La dernière texture dans la chaîne est de 64x64.

Ce panneau est visible à une distance maximale. Si l’utilisateur est d’abord éloigné de ce panneau, le jeu affiche la plus petite texture dans la chaîne mipmap, qui dans ce cas est la texture de 64x64.

![Illustration d’une texture de 64x64 d’un signe danger](images/mip1.jpg)

Lorsque l’utilisateur s’approche de ce panneau, les textures de plus haute résolution dans la chaîne de mipmaps sont progressivement utilisées. La résolution dans l’illustration suivante est de 128x128.

![Illustration d’une texture de 128x128 d’un panneau Danger](images/mip2.jpg)

La texture à la résolution la plus élevée est utilisée lorsque le point de vue de l’utilisateur se trouve à la distance minimale autorisée de ce panneau, comme indiqué dans l’illustration suivante.

![Illustration d’une texture de 256x256 d’un panneau Danger](images/mip3.jpg)

Il s’agit d’un moyen efficace de simulation de perspective pour les textures. Au lieu de restituer une texture unique avec plusieurs résolutions, il est plus rapide d’utiliser plusieurs textures avec différentes résolutions.

Direct3D peut déterminer, parmi un ensemble de mipmaps, la texture dont la résolution est la plus proche de la sortie souhaitée, et peut mapper des pixels dans son espace de texel. Si la résolution de l’image finale se trouve entre les résolutions des textures de l’ensemble mipmap, Direct3D peut examiner les texels dans les deux mipmaps et fusionner leurs valeurs de couleur.

Pour utiliser les mipmaps, votre application doit générer un ensemble de mipmaps. Les applications appliquent les mipmaps en sélectionnant l’ensemble mipmap défini comme première texture dans l’ensemble de textures actuel. Voir [Fusion de textures](texture-blending.md).

Ensuite, votre application définit la méthode de filtrage que Direct3D utilise pour échantillonner les texels. La méthode la plus rapide de filtrage des mipmaps est la sélection par Direct3D du texel le plus proche. Utilisez la valeur énumérée D3DTEXF\_POINT pour sélectionner cette option. Direct3D peut produire de meilleurs résultats de filtrage si votre application utilise la valeur énumérée D3DTEXF\_LINEAR. Celle-ci sélectionne le mipmap le plus proche, puis calcule une moyenne pondérée des texels autour de l’emplacement dans la texture à laquelle le pixel en cours est mappé.

Les textures mipmap sont utilisées dans les scènes 3D afin de réduire le temps nécessaire au rendu d’une scène. Elles améliorent également le réalisme de la scène. Toutefois, elles nécessitent souvent de grandes quantités de mémoire.

**Remarque**  chaque surface dans une chaîne de mipmaps présente des dimensions sont réduites celui de la surface précédente dans la chaîne. Si le mipmap de niveau supérieur possède des dimensions de 256x128, les dimensions du mipmap de deuxième niveau sont de 128x64, celles du troisième niveau sont de 64x32 et ainsi de suite, jusqu’à 1x1. Vous ne pouvez pas demander un nombre de niveaux de mipmap dans Levels qui entraînerait une largeur ou une hauteur de mipmap inférieure à 1 dans la chaîne. Dans le cas d’une surface de mipmap de niveau supérieur de 4x2, la valeur maximale autorisée pour Levels est de trois. Les dimensions de niveau supérieur sont de 4x2, les dimensions de second niveau sont de 2x1 et les dimensions de troisième niveau sont de 1x1. Une valeur supérieure à 3niveaux entraînerait une valeur fractionnaire de la hauteur du deuxième niveau de mipmap et n’est donc pas autorisée.

 

Direct3D peut effectuer automatiquement le filtrage de textures mipmap. Les applications peuvent parcourir manuellement une chaîne de mipmaps pour charger les données bitmap dans chaque surface de la chaîne. Il s’agit souvent de la seule raison de parcourir la chaîne. La génération automatique de mipmaps au moment de la création de la texture exploite le filtrage matériel car le mipmap se trouve dans la mémoire vidéo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Filtrage de textures](texture-filtering.md)

 

 




