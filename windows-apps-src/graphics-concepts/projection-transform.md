---
title: Transformation de la projection
description: Une transformation de projection permet de contrôler les éléments internes de la caméra, notamment le choix d’un objectif. Il s’agit du plus complexe des trois types de transformations.
ms.assetid: 378F205D-3800-4477-9820-5EBE6528B14A
keywords:
- Transformation de la projection
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f0806c0aa7a130a080457f4361d17f64451846f9
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7970163"
---
# <a name="projection-transform"></a>Transformation de la projection


Une *transformation de projection* permet de contrôler les éléments internes de la caméra tels que le choix d'un objectif. Il s’agit du plus complexe des trois types de transformations.

La matrice de projection consiste principalement en une projection de l'échelle et de la perspective. La transformation de projection convertit le tronc de cône d’affichage en une forme cubique. L’extrémité proche du tronc de cône d’affichage étant plus petite que son extrémité éloignée, cela a pour effet de développer les objets situés à proximité de la caméra; c’est de cette manière que la perspective est appliquée à la scène.

Dans [le tronc de cône d’affichage](viewports-and-clipping.md), la distance entre l’appareil photo et l’origine de l’espace de transformation de l’affichage est définie de façon arbitraire sur la valeur D. La matrice de projection ressemble donc à l’illustration suivante.

![illustration de la matrice de projection](images/projmat1.png)

La matrice d’affichage translate la caméra vers l’origine en translatant dans la direction de l’axe z par - D. La matrice de translation ressemble à l’illustration suivante.

![illustration de la matrice de translation](images/projmat2.png)

En multipliant la matrice de translation par la matrice de projection (T\ * P), nous obtenons la matrice de projection composite, comme illustré ci-dessous.

![illustration de la matrice de projection composite](images/projmat3.png)

La transformation de perspective convertit un tronc de cône d’affichage en un nouvel espace de coordonnées. Notez que le tronc de cône devient cubique et que l’origine se déplace depuis le coin supérieur droit de la scène vers le centre, comme illustré dans le diagramme suivant.

![diagramme illustrant la manière dont la transformation de perspective convertit le tronc de cône d’affichage en un nouvel espace de coordonnées](images/cuboid.png)

Dans la transformation de perspective, les limites des axes x et y sont -1 et 1. Les limites de l’axe z sont 0 pour le plan avant et 1 pour le plan arrière.

Cette matrice translate et met à l’échelle les objets en fonction d’une distance spécifiée entre la caméra et le plan de coupe proche, mais elle ne tient pas compte du champ de vue, et les valeurs z qu’elle génère pour les objets éloignés peuvent être quasiment identiques, ce qui rend les comparaisons de profondeur difficiles. La matrice suivante permet de résoudre ces problèmes, et elle ajuste les sommets de façon à prendre en compte les proportions de la fenêtre d’affichage, ce qui la rend particulièrement adaptée pour la projection de perspective.

![illustration d’une matrice pour la projection de perspective](images/prjmatx1.png)

Dans cette matrice, Zₙ correspond à la valeur z du plan de coupe proche. Les variables w, h et Q ont les significations suivantes. Notez que fov<sub>w</sub> et fovₖ représentent le champ de vue horizontal et le champ de vue vertical de la fenêtre d’affichage, en radians.

![équations illustrant les significations des variables](images/prjmatx2.png)

Pour votre application, il est plus pratique d’utiliser les dimensions horizontales et verticales de la fenêtre d’affichage (dans l’espace de la caméra) plutôt que des angles de champ de vue pour définir les coefficients de mise à l’échelle des axes x et y. Lors de la résolution du calcul, les deux équations suivantes pour w et h utilisent les dimensions de la fenêtre d’affichage et sont équivalentes aux équations précédentes.

![équations illustrant les significations des variables w et h](images/prjmatx3.png)

Dans ces formules, Zₙ représente la position du plan de coupe proche, et les variables V<sub>w</sub> et Vₕ représentent la largeur et la hauteur de la fenêtre d’affichage dans l’espace de la caméra.

Quelle que soit la formule vous décidez d’utiliser, veillez à définir Zₙ sur la plus grande valeur possible, car les valeurs z très proches de la caméra ne varient pas beaucoup. Cela permet d’effectuer des comparaisons de profondeur à l’aide d’un z-buffer 16bits complexe.

## <a name="span-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspana-w-friendly-projection-matrix"></a><span id="A_W_Friendly_Projection_Matrix"></span><span id="a_w_friendly_projection_matrix"></span><span id="A_W_FRIENDLY_PROJECTION_MATRIX"></span>Une matrice de projection compatible avec w


Direct3D peut utiliser le composant w d’un sommet qui a été transformé par les matrices universelles, d’affichage et de projection pour effectuer des calculs basés sur la profondeur dans le tampon de profondeur ou réaliser des effets de brouillard. Pour effectuer des calculs de ce type, votre matrice de projection doit normaliser la variable w de façon à ce qu’elle soit équivalente à l'axe z de l’espace universel. En résumé, si votre matrice de projection inclut un coefficient (3,4) qui n’est pas 1, vous devez mettre à l’échelle tous les coefficients par l’inverse du coefficient (3,4) pour générer une matrice correcte. Si vous ne fournissez pas une matrice conforme, les effets de brouillard et mise en mémoire tampon de profondeur ne sont pas appliqués correctement.

L’illustration suivante montre une matrice de projection non conforme, puis la même matrice mise à l’échelle afin que le brouillard par rapport à l’œil soit activé.

![illustrations d’une matrice de projection non conforme et d’une matrice avec brouillard par rapport à l’œil](images/eyerlmx.png)

Dans les matrices précédentes, toutes les variables sont supposées être différentes de zéro. Pour plus d’informations sur la mise en mémoire tampon de profondeur basée sur w, consultez l’article [Tampons de profondeur](depth-buffers.md).

Direct3D utilise la matrice de projection définie pour effectuer ses calculs de profondeur basés sur w. Par conséquent, les applications doivent définir une matrice de projection conforme pour recevoir les fonctionnalités basées sur w souhaitées, même si elles n’utilisent pas Direct3D pour les transformations.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Transformations](transforms.md)

 

 




