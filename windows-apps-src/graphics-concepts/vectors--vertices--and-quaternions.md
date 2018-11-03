---
title: Vecteurs, sommets et quaternions
description: Dans Direct3D, les sommets décrivent la position et l’orientation. Chaque vertex dans une primitive est décrite par un vecteur qui donne sa position, sa couleur, ses coordonnées de texture et par un vecteur normal qui donne son orientation.
ms.assetid: 94EC3D59-43FC-4509-A233-916E9FA8381E
keywords:
- Vecteurs, sommets et quaternions
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2373d18b51015652bc1ef3035402e1da95a54abf
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5985877"
---
# <a name="vectors-vertices-and-quaternions"></a>Vecteurs, sommets et quaternions


Dans Direct3D, les sommets décrivent la position et l’orientation. Chaque vertex dans une primitive est décrite par un vecteur qui donne sa position, sa couleur, ses coordonnées de texture et par un vecteur normal qui donne son orientation.

Les quaternions ajoutent un quatrième élément aux valeurs \[x, y, z] qui définissent un vecteur à trois composants. Les quaternions peuvent remplacer les méthodes de matrice qui sont généralement utilisées pour les rotations3D. Un quaternion représente un axe dans l’espace 3D et une rotation autour de cet axe. Par exemple, un quaternion peut représenter un axe (1,1,2) et une rotation de 1radian. Les quaternions transmettent des informations précieuses, mais leur puissance réelle vient des deux opérations que vous pouvez effectuer sur eux: composition et interpolation.

L'exécution d'une composition sur des quaternions ressemble leur combinaison. La composition de deux quaternions est notée comme dans l’illustration suivante.

![Illustration d'une notation de quaternion](images/quateq.png)

La composition de deux quaternions appliquée à une géométrie signifie «faire pivoter la géométrie autour de l'axe₂ par la rotation₂, puis la faire pivoter autour de l'axe₁ par la rotation₁». Dans ce cas, Q représente une rotation autour d’un axe unique qui est le résultat de l’application de q₂, puis de q₁ à la géométrie.

À l’aide d’une interpolation de quaternion, une application peut calculer un chemin souple et raisonnable pour passer d'un axe et d'une orientation à d'autres. Par conséquent, l’interpolation entre q₁ et q₂ permet de réaliser facilement une animation en basculant d'une orientation à une autre.

Lorsque vous utilisez la composition et l'interpolation ensemble, cela permet de manipuler facilement une géométrie d'une manière qui semble complexe. Par exemple, imaginons que vous disposez d’une géométrie que vous souhaitez faire pivoter dans une orientation donnée. Vous savez que vous souhaitez la faire pivoter de r₂ degrés autour de l'axis₂, puis la faire pivoter de r₁ degrés autour de l'axis₁, mais vous ne connaissez pas le quaternion final. En utilisant la composition, vous pouvez combiner les deux rotations sur la géométrie pour obtenir un quaternion unique qui est le résultat. Ensuite, vous pouvez interpoler entre le quaternion d'origine et le quaternion composé pour effectuer une transition fluide de l'un à l'autre.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 




