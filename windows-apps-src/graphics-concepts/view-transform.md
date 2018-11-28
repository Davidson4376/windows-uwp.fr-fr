---
title: Transformation de vue
description: Une transformation de vue localise l'observateur dans l’espace universel, transformant les vertex en espace de caméra.
ms.assetid: DA4C2051-4C28-4ABF-8C06-241C8CB87F2F
keywords:
- Transformation de vue
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 63660883f327547a82eac4a3accec475995a651a
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7839634"
---
# <a name="view-transform"></a>Transformation de vue


Une *transformation de vue* localise l'observateur dans l’espace universel, transformant les vertex en espace de caméra. Dans l'espace caméra, la caméra (ou l’observateur) se trouve à l’origine, dans la direction z positive. La matrice globale replace les objets dans le monde autour de la position (l’origine de l’espace de la caméra) et de l’orientation d'une caméra. Direct3D utilise un système de coordonnées gaucher, z est donc positif dans une scène.

Il existe de nombreuses façons de créer une matrice globale. Dans tous les cas, la caméra obéit à une certaine logique de position et d’orientation dans l’espace universel qui sert de point de départ pour créer une matrice globale qui sera appliquée aux modèles d'une scène. La matrice globale translate les objets et les fait pivoter pour les placer dans l’espace de caméra, où la caméra se trouve à l’origine. Une façon de créer une matrice globale consiste à combiner une matrice de translation avec des matrices de rotation pour chaque axe. Avec cette méthode, l’équation générale de matrice suivante s’applique.

![équation de la transformation de vue](images/viewtran.png)

Dans cette formule, V est la matrice globale en cours de création, T est une matrice de translation qui repositionne les objets dans le monde, et Rₓ jusqu'à R<sub>z</sub> sont des matrices de rotation qui font pivoter des objets sur les axes x, y et z. Les matrices de translation et de rotation sont basées sur la position et l’orientation logiques de la caméra dans l’espace universel. Par conséquent, si la position logique de la caméra dans le monde est &lt;10,20,100&gt;, l’objectif de la matrice de translation consiste à déplacer les objets de -10unités le long de l’axe x, de -20unités le long de l’axe y et de-100unités le long de l’axe z. Les matrices de rotation de la formule sont basées sur l’orientation de la caméra, c'est-à-dire sur la valeur de décalage des axes de l’espace de la caméra par rapport à l'espace universel. Par exemple, si la caméra mentionnée précédemment pointe directement vers le bas, son axe z est décalé de 90degrés (pi/2radians) de l’axe z de l’espace universel, comme le montre l’illustration suivante.

![Illustration de l’espace d’affichage de la caméra par rapport à l’espace universel](images/camtop.png)

Les matrices de rotation appliquent des rotations de magnitude égale, mais opposée, aux modèles de la scène. La matrice globale de cette caméra inclut une rotation de-90degrés autour de l’axe x. La matrice de rotation est combinée avec la matrice de translation pour créer une matrice globale qui adapte la position et l’orientation des objets dans la scène afin que leur partie haute se trouve face à la caméra, donnant l’impression que celle-ci est placée au-dessus du modèle.

## <a name="span-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspansetting-up-a-view-matrix"></a><span id="Setting_Up_a_View_Matrix"></span><span id="setting_up_a_view_matrix"></span><span id="SETTING_UP_A_VIEW_MATRIX"></span>Configuration d’une matrice globale


Direct3D utilise les matrices universelles et d'affichage pour configurer plusieurs structures de données internes. Chaque fois que vous définissez une nouvelle matrice universelle ou d'affichage, le système recalcule les structures internes associées. Redéfinir fréquemment ces matrices prend beaucoup de temps de calcul. Vous pouvez réduire le nombre de calculs requis en concaténant vos matrices universelles et d'affichage dans une matrice universelle-d'affichage que vous définissez comme matrice universelle, puis en définissant la matrice d'affichage sur l’identité. Conservez des copies en cache de chaque matrice universelle et d'affichage afin de pouvoir modifier, concaténer et réinitialiser la matrice universelle en fonction des besoins.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Transformations](transforms.md)

 

 




