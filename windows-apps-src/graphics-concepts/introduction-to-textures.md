---
title: Introduction aux textures
description: Une ressource de texture est une structure de données permettant de stocker des texels, qui représentent la plus petite unité d’une texture pouvant être lue ou écrite. Lorsque la texture est lue par un nuanceur, elle peut être filtrée par des échantillons de textures.
ms.assetid: 6F3C76A8-F762-4296-AE02-BFBD6476A5A8
keywords:
- Introduction aux textures
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f88cccc3f32449d09c01450bf159b3fca6a3d59f
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5689636"
---
# <a name="introduction-to-textures"></a>Introduction aux textures


Une ressource de texture est une structure de données permettant de stocker des texels, qui représentent la plus petite unité d’une texture pouvant être lue ou écrite. Lorsque la texture est lue par un nuanceur, elle peut être filtrée par des échantillons de textures.

Une ressource de texture est un ensemble de données structuré conçu pour stocker des texels. Un texel représente la plus petite unité de texture pouvant être lue ou écrite par le pipeline. Contrairement aux tampons, les textures peuvent être filtrées par échantillons de textures, car elles sont lues par des unités de nuanceur. Le type de texture a une incidence sur la façon dont la texture est filtrée. Chaque texel comporte 1 à 4composants, organisés dans l’un des formatsDXGI définis par l’énumération DXGI\_FORMAT.

Les textures sont créées en tant que ressources structurées d’une taille connue. Chaque texture peut toutefois être typée ou non typée au moment de la création de la ressource, dès lors que le type est intégralement spécifié à l’aide d’une vue lorsque la texture est liée au pipeline.

## <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span>Types de textures


Direct3D prend en charge plusieurs représentations à virgule flottante. Tous les calculs à virgule flottante fonctionnent sous un sous-ensemble défini de règles de représentation des nombres à virgule flottante 32bits simple précision conformément à l’IEEE754.

Il existe plusieurs types de textures (1D, 2D et 3D), et chacune de ces textures peut être créée avec ou sans mipmaps. Direct3D prend également en charge les tableaux de textures et les textures échantillonnées plusieurs fois.

-   [Textures 1D](#texture1d-resource)
-   [Tableaux de textures1D](#texture1d-array-resource)
-   [Textures2D et tableaux de textures 2D](#texture2d-resource)
-   [Textures 3D](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-textures"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>Textures 1D

Dans sa forme la plus simple, une texture1D contient des données de texture qui peuvent être traitées par une coordonnée de texture unique; elle peut être affichée sous la forme d’un tableau de texels, comme illustré ci-dessous.

L’illustration suivante représente une texture1D:

![une texture 1d](images/d3d10-1d-texture.png)

Chaque texel contient un certain nombre de composants colorimétriques, en fonction du format des données stockées. Pour ajouter davantage de complexité, vous pouvez créer une texture1D avec des niveaux de mipmap, comme illustré ci-dessous.

![texture 1d avec des niveaux de mipmap](images/d3d10-resource-texture1d.png)

Un niveau de mipmap est une texture deux fois plus petite que le niveau immédiatement supérieur. Le niveau supérieur est celui qui comporte le plus d’informations; chacun des niveaux inférieurs est de taille plus réduite. Pour un mipmap 1D, le niveau le plus petit contient 1texel. En outre, les niveaux MIP sont toujours réduits à une échelle 1:1.

Lorsque les mipmaps sont générés pour une texture de dimension non standard, le niveau immédiatement inférieur présente toujours une taille uniforme (sauf quand le niveau le plus bas atteint la valeur de 1). Par exemple, le schéma représente une texture 5x1 dont le niveau immédiatement inférieur est une texture 2x1, dont le niveau MIP immédiatement inférieur (le dernier) est une texture 1x1. Les différents niveaux sont identifiés au moyen d’un index appelé LOD (Level-of-Detail); vous pouvez utiliser l’index LOD pour accéder à une texture plus petite lors du processus de rendu d’une géométrie qui n’est pas aussi proche de la caméra.

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-arrays"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>Tableaux de textures1D

Direct3D prend également en charge les tableaux de textures. D’un point de vue conceptuel, un tableau de textures1D ressemble à l’illustration suivante.

![tableau de texture 1d](images/d3d10-resource-texture1darray.png)

Ce tableau de textures contient trois textures. Chacune des trois textures présente une largeur de 5 (qui représente le nombre d’éléments dans la première couche). Chaque texture contient également un mipmap de couche3.

Dans Direct3D, tous les tableaux de textures sont homogènes; cela signifie que chaque texture contenue dans un tableau de textures doit avoir le même format de données et la même taille (notamment au niveau de la largeur et du nombre de niveaux de mipmap). Vous pouvez créer des tableaux de textures de différentes tailles, dès lors que toutes les textures contenues dans chaque tableau sont de la même taille.

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-textures-and-2d-texture-arrays"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>Textures2D et tableaux de textures 2D

Une ressource Texture2D contient une grille2D de texels. Chaque texel est adressable par vecteur u, v. Dans la mesure où il s’agit d’une ressource de texture, elle peut contenir des niveaux de mipmap et des sous-ressources. Une ressource de texture2D entièrement remplie ressemble à l’illustration suivante.

![ressource de texture 2d](images/d3d10-resource-texture2d.png)

Cette ressource de texture contient une seule texture3x5 avec trois niveaux de mipmap.

Une ressource de texture 2D est un tableau de textures 2D homogène; autrement dit, toutes les textures ont le même format de données et les mêmes dimensions (niveaux de mipmap compris). Ce tableau présente une disposition similaire à celle du tableau de texture 1D, à la différence près que les textures contiennent désormais des données2D, tel qu’indiqué dans l’illustration suivante.

![tableau de texture 2d](images/d3d10-resource-texture2darray.png)

Ce tableau de textures contient trois textures; chaque texture est de type 3x5 avec deux niveaux de mipmap.

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-2d-texture-array-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Utilisation d’un tableau de texture 2D en tant que cube de texture

Un cube de texture est un tableau de textures2D qui contient 6textures, une pour chaque face du cube. Un cube de texture entièrement rempli ressemble à l’illustration suivante.

![tableau de textures 2d représentant un cube de texture](images/d3d10-resource-texturecube.png)

Un tableau de textures2D qui contient 6textures peut être lu à partir des nuanceurs à l’aide des fonctions intrinsèques de mappage de cube, une fois que ces nuanceurs sont liés au pipeline à l’aide d’une vue de texture de cube. Les cubes de texture sont adressés à partir du nuanceur à l’aide d’un vecteur3D qui part du centre du cube de texture.

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-textures"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>Textures 3D

Une ressource de texture 3D (également appelée texture volumique) contient un volume3D de texels. Dans la mesure où il s’agit d’une ressource de texture, elle peut contenir des niveaux de mipmap. Une texture3D entièrement remplie ressemble à l’illustration suivante.

![ressource de texture 3d](images/d3d10-resource-texture3d.png)

Lorsqu’une section de mipmap de texture3D est liée en tant que sortie de cible de rendu (avec une vue de cible de rendu), la texture3D se comporte de la même façon qu’un tableau de textures2D comportant nsections. La sélection de la section de rendu spécifique s’effectue lors de l’étape du nuanceur de géométrie.

Un tableau de textures3D n’a pas de concept. Par conséquent, une sous-ressource de texture3D est un niveau de mipmap unique.

Les systèmes de coordonnées pour Direct3D sont définis pour les pixels et les texels.

## <a name="span-idpixelspanspan-idpixelspanspan-idpixelspanpixel-coordinate-system"></a><span id="Pixel"></span><span id="pixel"></span><span id="PIXEL"></span>Système de coordonnées de pixels


Le système de coordonnées de pixels de Direct3D définit l’origine d’une cible de rendu dans le coin supérieur gauche, tel qu’illustré dans le schéma suivant. Le centre des pixels est décalé (0,5f, 0,5f) des emplacements d’entiers.

![schéma du système de coordonnées de pixels dans direct3d 10](images/d3d10-coordspix10.png)

## <a name="span-idtexelspanspan-idtexelspanspan-idtexelspantexel-coordinate-system"></a><span id="Texel"></span><span id="texel"></span><span id="TEXEL"></span>Système de coordonnées de texels


Le système de coordonnées de texels possède son origine dans le coin supérieur gauche de la texture, tel qu’illustré dans le schéma suivant. Ainsi, le rendu des textures alignées sur l’écran est simple, dans la mesure où le système de coordonnées de pixels est aligné sur le système de coordonnées de texels.

![schéma du système de coordonnées de texels](images/d3d10-coordstex10.png)

Les coordonnées de textures sont représentées à l’aide d’un nombre normalisé ou calibré; chaque coordonnée est mappée sur un texel spécifique, comme suit:

Pour une coordonnée normalisée:

-   Échantillonnage de points: n°texel=sol(Uxlargeur)
-   Échantillonnage linéaire: n° du texel de gauche=sol(U*largeur), n° du texel de droite=n° du texel de gauche+1

Pour une coordonnée calibrée:

-   Échantillonnage de points: n°texel=sol(U)
-   Échantillonnage linéaire: n° du texel de gauche=sol(U - 0,5), n° du texel de droite=n° du texel de gauche+1

Où la largeur correspond à la largeur de la texture (en texels).

L’habillage d’adresse de textures survient après le calcul des emplacements de texels.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Textures](textures.md)
