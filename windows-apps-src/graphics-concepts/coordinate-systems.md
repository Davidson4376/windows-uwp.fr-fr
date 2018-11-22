---
title: Systèmes de coordonnées
description: "Les applications graphiques 3D utilisent généralement l'un des deux types de systèmes de coordonnées cartésiens: gaucher ou droitier. Dans les deux systèmes de coordonnées, l’axe x positif pointe vers la droite et l’axe y positif pointe vers le haut."
ms.assetid: 138D9B81-146F-4E9F-B742-1EDED8FBF2AE
keywords:
- Systèmes de coordonnées
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1b8faed0719419d4be8ac1e5d493610ec2660598
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7562628"
---
# <a name="coordinate-systems"></a>Systèmes de coordonnées


Les applications graphiques 3D utilisent généralement l'un des deux types de systèmes de coordonnées cartésiens: gaucher ou droitier. Dans les deux systèmes de coordonnées, l’axe x positif pointe vers la droite et l’axe y positif pointe vers le haut.

## <a name="span-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanleft-and-right-handed-coordinates"></a><span id="Left_and_right_handed_coordinates"></span><span id="left_and_right_handed_coordinates"></span><span id="LEFT_AND_RIGHT_HANDED_COORDINATES"></span>Systèmes de coordonnées gauchers et droitiers


Vous pouvez mémoriser la direction dans laquelle pointe l’axe z positif en pointant les doigts de votre main gauche ou de votre main droite dans la direction x positive et en les pliant dans la direction y positive. La direction vers laquelle pointe votre pouce (vers vous ou à l'opposé) correspond à la direction dans laquelle pointe l’axe z positif pour ce système de coordonnées. L’illustration suivante décrit ces deux systèmes de coordonnées.

![illustration des systèmes de coordonnées cartésiens droitier et gaucher](images/leftrght.png)

Direct3D utilise un système de coordonnées gaucher. Bien que les systèmes de coordonnées droitiers et gauchers soient les plus courants, divers autres systèmes de coordonnées sont utilisés dans le logiciel 3D. Par exemple, il n’est pas rare que les applications de modélisation 3D utilisent un système de coordonnées dans lequel l’axe y pointe vers l'observateur ou dans la direction opposée et où l’axe z pointe vers le haut.

## <a name="span-idverticesandvectorsspanspan-idverticesandvectorsspanspan-idverticesandvectorsspanvertices-and-vectors"></a><span id="Vertices_and_vectors"></span><span id="vertices_and_vectors"></span><span id="VERTICES_AND_VECTORS"></span>Vertex et vecteurs


Étant donné le système de coordonnées, les coordonnées x, y et z peuvent définir un point dans l’espace (un «vertex») ou une direction 3D (un «vecteur»).

Un ensemble de vertex peut être utilisée pour définir des lignes et des formes. Les objets les plus simples définissables par les vertex sont appelés [primitives](primitives.md), et les objets plus complexes définis par un ensemble de primitives sont appelés «maillage».

La translation, la rotation et la mise à l'échelle sont les principales opérations effectuées sur des maillages définis dans un système de coordonnées 3D. Vous pouvez combiner ces transformations de base pour créer une matrice de transformation. Pour plus d’informations, voir [Transformations](transforms.md).

Lorsque vous associez ces opérations, les résultats ne sont pas commutatifs; l’ordre dans lequel vous multipliez les matrices est important.

## <a name="span-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanporting-from-a-right-handed-coordinate-system"></a><span id="Porting_from_a_right-handed_coordinate_system"></span><span id="porting_from_a_right-handed_coordinate_system"></span><span id="PORTING_FROM_A_RIGHT-HANDED_COORDINATE_SYSTEM"></span>Portage d’un système de coordonnées droitier


Si vous portez une application reposant sur un système de coordonnées droitier, vous devez apporter deux modifications aux données transmises à Direct3D:

-   Inversez l’ordre des vertex des triangles de sorte que le système les parcoure dans le sens des aiguilles d’une montre depuis l’avant. En d’autres termes, si les vertex sont v0, v1, v2, passez-les à Direct3D en tant que v0, v2, v1.
-   Utilisez la matrice d'affichage pour mettre à l'échelle l'espace universel de -1dans la direction z. Pour ce faire, inversez le signe des membres \_31, \_32, \_33 et \_34de la structure de matrice que vous utilisez pour votre matrice d'affichage.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 




