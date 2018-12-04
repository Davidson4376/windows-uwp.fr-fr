---
title: Vecteurs normaux de face et de vertex
description: Chaque face d'un maillage comporte un vecteur normal d'une unité perpendiculaire. La direction du vecteur est déterminée par l’ordre dans lequel les vertex sont définis et par le côté (droit ou gauche) sur lequel se trouve le système de coordonnées.
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- Vecteurs normaux de face et de vertex
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2347efc5d68abd53442f52ecabdc060393ee561b
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8475899"
---
# <a name="face-and-vertex-normal-vectors"></a>Vecteurs normaux de face et de vertex


Chaque face d'un maillage comporte un vecteur normal d'une unité perpendiculaire. La direction du vecteur est déterminée par l’ordre dans lequel les vertex sont définis et par le côté (droit ou gauche) sur lequel se trouve le système de coordonnées.

## <a name="span-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>Vecteur normal d’une unité perpendiculaire pour une face avant


Chaque face d’un maillage comporte un vecteur normal d’une unité perpendiculaire. La direction du vecteur est déterminée par l’ordre dans lequel les vertex sont définis et par le côté (droit ou gauche) sur lequel se trouve le système de coordonnées. La normale de la face pointe dans la direction opposée au côté avant de la face. Dans Direct3D, seul l’avant d’une face est visible. Une face avant est une face dont les vertex sont définis dans l’ordre des aiguilles d’une montre.

La figure ci-après illustre un vecteur normal d’une face avant:

![vecteur normal d’une face avant](images/nrmlvect.png)

## <a name="span-idcullingbackfacesspanspan-idcullingbackfacesspanspan-idcullingbackfacesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>Élimination des faces arrière


Toute face autre qu’une face avant est une face arrière. Direct3D ne procède pas systématiquement au rendu des faces arrière; dans ce cas, on parle d’élimination des faces arrière. L’élimination des faces arrière consiste à supprimer les faces arrière du rendu. Si vous le souhaitez, vous pouvez modifier le mode d’élimination de façon à obtenir le rendu des faces arrière. Pour plus d’informations, consultez l’article [État d’élimination](https://msdn.microsoft.com/library/windows/desktop/bb204882).

## <a name="span-idvertexunitnormalsspanspan-idvertexunitnormalsspanspan-idvertexunitnormalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>Normales d’unité de vertex


Direct3D utilise les normales d’unité de vertex pour les effets d’ombrage Gouraud, d’éclairage et d’application de textures.

La figure ci-après illustre des normales de vertex:

![normales de vertex](images/vertnrml.png)

Lorsque vous appliquez un ombrage Gouraud à un polygone, Direct3D utilise les normales de vertex pour calculer l’angle entre la source de lumière et la surface. Il calcule les valeurs de couleur et d’intensité pour les vertex et les interpole pour chaque point sur l’ensemble des surfaces de la primitive. Direct3D calcule la valeur de l’intensité lumineuse à l’aide de l’angle. Plus l’angle est important, moins la lumière brille sur la surface.

## <a name="span-idflatsurfacesspanspan-idflatsurfacesspanspan-idflatsurfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>Surfaces plates


Si vous créez un objet plat, définissez les normales de vertex pour qu’elles pointent perpendiculairement à la surface.

La figure ci-après illustre une surface plate composée de deux triangles avec des normales de vertex:

![surface plate composée de deux triangles avec des normales de vertex](images/flatvert.png)

## <a name="span-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>Ombrage lisse sur un objet non plat


Au lieu d’être plat, votre objet se compose plutôt probablement de bandes de triangles qui ne sont pas coplanaires. Un moyen simple d’appliquer un ombrage lisse à l’ensemble des triangles d’une bande consiste à commencer par calculer le vecteur normal de surface pour chaque face polygonale à laquelle le vertex est associé. Il est possible de définir la normale de vertex pour qu’elle forme un angle égal avec chaque normale de surface. Toutefois, cette méthode risque de ne pas se révéler suffisamment efficace pour les primitives complexes.

Cette méthode est illustrée par le diagramme ci-après, qui présente deux surfaces, S1 et S2, vues d’en haut par la tranche. Les vecteurs normaux des surfaces S1 et S2 sont affichés en bleu. Le vecteur normal de vertex est représenté en rouge. L’angle que forme le vecteur normal de vertex avec la normale de la surfaceS1 est identique à celui formé par la normale de vertex avec la normale de la surfaceS2. Lorsque ces deux surfaces sont éclairées et ombrées à l’aide d’un ombrage Gouraud, le résultat obtenu est une arête à ombrage et à arrondi lisses entre les deux surfaces.

La figure ci-après illustre deux surfaces (S1 et S2) avec leurs vecteurs normaux et le vecteur normal de vertex:

![deux surfaces (s1 et s2) avec leurs vecteurs normaux et le vecteur normal de vertex](images/gvert.png)

Si la normale de vertex est orientée vers l’une des faces à laquelle elle est associée, il en résulte une augmentation ou une diminution de l’intensité lumineuse appliquée aux points de cette surface, selon l’angle qu’elle forme avec la source de lumière. Le diagramme ci-après en présente un exemple. Ces surfaces sont vues par la tranche. La normale de vertex penche vers S1, formant ainsi un angle avec la source de lumière plus petit que si la normale de vertex présentait des angles égaux avec les normales de surface.

La figure ci-après illustre deux surfaces (S1 et S2) avec un vecteur normal de vertex orienté vers l’une des faces:

![deux surfaces (s1 et s2) avec un vecteur normal de vertex orienté vers l’une des faces](images/gvert2.png)

## <a name="span-idsharpedgesspanspan-idsharpedgesspanspan-idsharpedgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>Arêtes vives


Vous pouvez utiliser l’ombrage Gouraud pour afficher certains objets d’une scène3D avec des arêtes vives. Pour ce faire, dupliquez les vecteurs normaux de vertex à chaque intersection des faces où une arête vive est requise.

La figure ci-après illustre des vecteurs normaux de vertex dupliqués au niveau des arêtes vives:

![vecteurs normaux de vertex dupliqués au niveau des arêtes vives](images/shade1.png)

Si vous utilisez les méthodes DrawPrimitive pour effectuer le rendu de votre scène, définissez l’objet doté d’arêtes vives sous la forme d’une liste de triangles, et non d’une bande de triangles. Lorsque vous définissez un objet sous la forme d’une bande de triangles, Direct3D le traite comme un seul polygone constitué de plusieurs faces triangulaires. L’ombrage Gouraud est appliqué à la fois à chacune des faces du polygone et entre les faces adjacentes.

Le résultat obtenu est un objet soumis à un ombrage lisse d’une face à l’autre. Étant donné qu’une liste de triangles est un polygone composé d’une série de faces triangulaires disjointes, Direct3D applique un ombrage Gouraud à chacune des faces du polygone. Toutefois, cet ombrage n’est pas appliqué d’une face à l’autre. Si au moins deux triangles d’une liste de triangles sont adjacents, une arête vive apparaît entre eux.

Une autre solution consiste à opter pour un ombrage constant lors du rendu d’objets présentant des arêtes vives. Cette méthode se révèle la plus efficace d’un point de vue calcul, mais peut entraîner un rendu moins réaliste des objets de la scène que pour les objets soumis à un ombrage Gouraud.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 




