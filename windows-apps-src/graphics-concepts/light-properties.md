---
title: Propriétés de lumière
description: Les propriétés de lumière décrivent le type de source lumineuse (à points, directionnelle, projecteur), l’atténuation, la couleur, la direction, la position et l’étendue.
ms.assetid: E832C3FD-9921-41C4-87B8-056E16B61B77
keywords:
- Propriétés de lumière
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 07a465d8fdcd1d425ed62e8d83cadd261f316da2
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7171397"
---
# <a name="light-properties"></a>Propriétés de lumière


Les propriétés de lumière décrivent le type de source lumineuse (à points, directionnelle, projecteur), l’atténuation, la couleur, la direction, la position et l’étendue. Selon le type de lumière utilisé, l’éclairage peut présenter des propriétés pour l’atténuation ou l’étendue, ou encore pour des effets de projecteur. Tous les types de lumière n’utilisent pas l’ensemble des propriétés.

Les propriétés de position, d’étendue et d’atténuation définissent l’emplacement de la lumière dans l’espace du monde et l’évolution de l’émission en fonction de la distance.

## <a name="span-idlightattenuationspanspan-idlightattenuationspanspan-idlightattenuationspanlight-attenuation"></a><span id="Light_Attenuation"></span><span id="light_attenuation"></span><span id="LIGHT_ATTENUATION"></span>Atténuation de la lumière


Les contrôles d’atténuation définissent la manière dont l’intensité de la lumière décroît jusqu’à la distance maximale spécifiée par la propriété d’étendue. Trois valeurs à virgule flottante sont parfois utilisées pour représenter l’atténuation de la lumière: Attenuation0, Attenuation1 et Attenuation2. Ces valeurs, comprises entre 0,0 et l’infini, contrôlent l’atténuation de la lumière. Certaines applications définissent l’élément Attenuation1 sur 1.0 et les autres sur 0.0. Le cas échéant, l’intensité de la lumière est fonction de la fraction 1/D, où D est la distance entre la source de lumière et le vertex. L’intensité maximale, obtenue à la source, décroît jusqu’à 1/(étendue de la lumière) à la distance maximale parcourue par la lumière.

Si en règle générale une application définit Attenuation0 sur 0,0, Attenuation1 sur une valeur constante et Attenuation2 sur 0.0, différents effets de lumière peuvent être obtenus en modifiant ces valeurs. La combinaison des valeurs d’atténuation permet de produire des effets plus complexes. Sinon, vous pouvez également paramétrer des valeurs en dehors des plages standard afin de créer des effets d’atténuation encore plus surprenants. Toutefois, les valeurs négatives d’atténuation ne sont pas autorisées. Voir [Atténuation et facteur de point lumineux](attenuation-and-spotlight-factor.md).

## <a name="span-idlightcolorspanspan-idlightcolorspanspan-idlightcolorspanlight-color"></a><span id="Light_Color"></span><span id="light_color"></span><span id="LIGHT_COLOR"></span>Couleur de lumière


Dans Direct3D, les lumières émettent 3couleurs qui sont utilisées de manière indépendante dans les calculs d’éclairage du système: une couleur diffuse, une couleur ambiante et une couleur spéculaire. Chacune est intégrée par le module d’éclairageDirect3D et interagit avec un élément équivalent du matériau actuel. De cette rencontre naît la couleur finale utilisée pour le rendu. La couleur diffuse interagit avec la propriété de réflexion diffuse du matériau actuel, la couleur spéculaire avec la propriété de réflexion spéculaire du matériau, etc. Pour plus d’informations sur l’application par Direct3D de ces couleurs, voir [Formules mathématiques d’éclairage](mathematics-of-lighting.md).

Une application Direct3D prend généralement en charge 3valeurs de couleur: les couleurs diffuse, ambiante et spéculaire, qui définissent la couleur finale émise.

Le type qui nécessite le volume le plus important de calculs du système est la couleur diffuse. La couleur diffuse la plus courante est le blanc (R: 1,0 V: 1.0 B: 1,0), mais vous pouvez créer des couleurs personnalisées afin d’obtenir les effets souhaités. Par exemple, vous utilisez la lumière rouge pour un foyer ou la lumière verte pour un feu tricolore.

Généralement, vous définissez les composants de couleur de la lumière sur des valeurs comprises entre 0,0 et 1,0, mais ce n’est pas obligatoire. Par exemple, vous pouvez définir l’ensemble des composants sur 2,0, afin de créer une lumière «plus blanche que le blanc». Ce type de définition peut s’avérer particulièrement utile lorsque vous recourez à des paramètres d’atténuation non constants.

Bien que Direct3D utilise des valeurs RVBA pour les lumières, le composant de couleur alpha n’est pas utilisé.

Habituellement, les couleurs de matériau sont utilisées pour l’éclairage. Toutefois, vous pouvez décider d’écraser les lumières émissive, ambiante, diffuse et spéculaire du matériau par les couleurs de vertex diffuse ou spéculaire.

La valeur alpha/de transparence provient toujours exclusivement du canal alpha de couleur diffuse.

La valeur de brouillard provient toujours exclusivement du canal alpha de couleur spéculaire.

## <a name="span-idlightdirectionspanspan-idlightdirectionspanspan-idlightdirectionspanlight-direction"></a><span id="Light_Direction"></span><span id="light_direction"></span><span id="LIGHT_DIRECTION"></span>Direction de la lumière


La propriété de direction détermine la direction de la lumière émise par l’objet dans l’espace du monde. La direction, utilisée uniquement par les lumières directionnelles et les projecteurs, est décrite avec un vecteur.

Définissez la direction de la lumière en tant que vecteur. Les vecteurs de direction sont décrits comme des distances depuis une origine logique, indépendamment de la position de la lumière dans la scène. Par conséquent, un projecteur pointant droit dans une scène (le long de l’axez positif) présente un vecteur de direction de &lt;0,0,1&gt;, quelle que soit sa position. De la même manière, vous pouvez simuler les rayons du soleil directement dans une scène, en paramétrant une lumière directionnelle de vecteur &lt;0,-1,0&gt;. Il n’est pas nécessaire de créer des lumières dirigées le long des axes de coordonnées. Vous pouvez combiner les valeurs afin de créer un éclairage défini selon des angles plus intéressants.

Si vous n’avez pas besoin de normaliser un vecteur de direction de lumière, veillez à lui affecter une magnitude. En d’autres termes, n’utilisez pas un vecteur de direction de &lt;0,0,0&gt;.

## <a name="span-idlightpositionspanspan-idlightpositionspanspan-idlightpositionspanlight-position"></a><span id="Light_Position"></span><span id="light_position"></span><span id="LIGHT_POSITION"></span>Position de la lumière


La position de la lumière est décrite à l’aide d’une structure de vecteurs. Les coordonnées x, y et z sont considérées comme étant dans l’espace du monde. La lumière directionnelle est le seul type qui n’utilise pas de propriété de position.

## <a name="span-idlightrangespanspan-idlightrangespanspan-idlightrangespanlight-range"></a><span id="Light_Range"></span><span id="light_range"></span><span id="LIGHT_RANGE"></span>Étendue de lumière


Une propriété d’étendue de lumière détermine la distance, dans l’espace du monde, à laquelle le maillage d’une scène ne reçoit plus la lumière émise par l’objet considéré. Les lumières directionnelles n’utilisent pas la propriété d’étendue.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Lumières et matériaux](lights-and-materials.md)

 

 




