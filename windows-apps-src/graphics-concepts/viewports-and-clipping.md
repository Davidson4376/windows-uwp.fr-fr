---
title: Fenêtres d’affichage et découpage
description: Une fenêtre d’affichage est un rectangle en deux dimensions (2D) dans lequel une scène3D est projetée.
ms.assetid: D0DD646E-13AE-452A-AD22-8C35000D0BA9
keywords:
- Fenêtres d’affichage et découpage
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4dd319c686bebf2a30431017f399f48b08618cb6
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7156370"
---
# <a name="viewports-and-clipping"></a>Fenêtres d’affichage et découpage


Une *fenêtre d’affichage* est un rectangle en deux dimensions (2D) dans lequel une scène3D est projetée. Dans Direct3D, le rectangle existe en tant que coordonnées au sein d’une surface Direct3D que le système utilise en tant que cible de rendu. La transformation de projection convertit les vertex dans le système de coordonnées utilisé pour la fenêtre d’affichage. Une fenêtre d’affichage est également utilisée pour spécifier la plage de valeurs de profondeur sur une surface de cible de rendu dans laquelle une scène sera rendue (généralement de0,0 à1,0).

## <a name="span-idtheviewingfrustumspanspan-idtheviewingfrustumspanspan-idtheviewingfrustumspanthe-viewing-frustum"></a><span id="The_Viewing_Frustum"></span><span id="the_viewing_frustum"></span><span id="THE_VIEWING_FRUSTUM"></span>Le tronc de cône d’affichage


Un tronc de cône d'affichage est un volume3D dans une scène positionnée par rapport à la fenêtre d’affichage de la caméra. La forme du volume affecte la façon dont les modèles sont projetés depuis l’espace de la caméra sur l’écran. Le type le plus courant de projection, la projection en perspective, fait que les objets proches de la caméra apparaissent plus gros que les objets distants. Pour un affichage en perspective, le tronc de cône d'affichage peut être visualisé comme une pyramide, avec la caméra placée à la pointe comme le montre l’illustration suivante. Cette pyramide est coupée par un plan de coupe avant et arrière. Le volume au sein de la pyramide entre les plans de découpage avant et arrière est le tronc de cône de visualisation. Les objets ne sont visibles que s'ils se trouvent dans ce volume.

![Illustration d’un tronc de cône d'affichage avec un plan de découpage avant et arrière](images/frustum.png)

Imaginez que vous vous trouviez dans une salle sombre et que vous regardiez par une fenêtre carrée. Ce que vous voyez est un tronc de cône d'affichage. Dans cette analogie, le plan de découpage proche est la fenêtre et le plan de découpage arrière est tout ce qui vous masque finalement la vue: le gratte-ciel de l'autre côté de la rue, les montagnes au loin ou rien du tout. Vous pouvez voir tous les éléments à l’intérieur de la pyramide tronquée qui commence à la fenêtre et se termine avec ce qui vous masque la vue. Vous ne pouvez rien voir d’autre.

Le tronc de cône d'affichage est défini par le champ de vue (fov) et par les distances des plans de découpage avant et arrière, spécifiées en coordonnées-z, comme dans le diagramme suivant.

![diagramme du tronc de cône d’affichage](images/fovdiag.png)

Dans ce diagramme, la variable D est la distance entre la caméra et l’origine de l’espace défini dans la dernière partie du pipeline de géométrie: la transformation d’affichage. Il s’agit de l’espace autour de laquelle vous organisez les limites de votre tronc de cône d'affichage. Pour en savoir plus sur la façon dont cette variable D permet de générer la matrice de projection, consultez [Transformation de projection](projection-transform.md)

## <a name="span-idviewportrectanglespanspan-idviewportrectanglespanspan-idviewportrectanglespanviewport-rectangle"></a><span id="Viewport_Rectangle"></span><span id="viewport_rectangle"></span><span id="VIEWPORT_RECTANGLE"></span>Rectangle de la fenêtre d’affichage


Une structure de fenêtre d’affichage contient quatre membres (X, Y, largeur, hauteur) qui définissent la zone de la surface de la cible de rendu dans laquelle une scène sera rendue. Ces valeurs correspondent au rectangle de destination, ou rectangle de la fenêtre d’affichage, comme dans le diagramme suivant.

![diagramme du rectangle de la fenêtre d’affichage](images/destrect.png)

Les valeurs que vous spécifiez pour les membres X, Y, largeur, hauteur sont des coordonnées d’écran par rapport au coin supérieur gauche de la surface de la cible de rendu. La structure définit deux membres supplémentaires (MinZ et MaxZ) qui indiquent les plages de profondeur dans lesquelles la scène sera rendue.

Direct3D suppose que le volume de découpage de la fenêtre d’affichage est compris entre -1,0 et 1,0dans X et entre 1,0 est -1,0dans Y. Il s’agit des paramètres les plus souvent utilisés autrefois par les applications. Vous pouvez ajuster les proportions de la fenêtre d’affichage avant découpage en utilisant la [transformation de projection](projection-transform.md).

**Remarque**  MaxZ et MinZ indiquent les plages de profondeur dans lesquelles la scène sera rendue et ne sont pas utilisés pour le découpage. La plupart des applications attribuent0,0 et1,0 à ces valeurs pour permettre au système de réaliser le rendu sur l’ensemble des valeurs de profondeur du tampon de profondeur. Dans certains cas, vous pouvez obtenir des effets spéciaux en utilisant d’autres plages de profondeur. Par exemple, pour restituer un affichage à tête haute dans un jeu, vous pouvez attribuer0,0 aux deux valeurs pour forcer le système à rendre les objets dans une scène au premier plan, ou leur attribuer1.0 pour rendre un objet qui doit toujours se trouver en arrière-plan.

 

Les dimensions utilisées dans les membres X, Y, largeur, hauteur d’une structure de fenêtre d’affichage définissent l’emplacement et les dimensions de la fenêtre d’affichage sur la surface de la cible de rendu. Ces valeurs sont exprimées en coordonnées d’écran par rapport à l’angle supérieur gauche de la surface.

Direct3D utilise l’emplacement et les dimensions de la fenêtre d’affichage pour mettre les vertex à l’échelle en fonction d’une scène rendue à l’emplacement approprié sur la surface cible. En interne, Direct3D insère ces valeurs dans la matrice suivante appliquée à chaque vertex.

![équation de la matrice appliquée à chaque vertex.](images/vpscale.png)

Cette matrice adapte les vertex en fonction des dimensions de la fenêtre d’affichage et de la plage de profondeur désirée et les translate vers l’emplacement approprié sur la surface de la cible de rendu. La matrice renverse également la coordonnéey afin de refléter une origine de l’écran dans le coin supérieur gauche avec ycroissant vers le bas. Une fois cette matrice appliquée, les vertex sont toujours homogènes (autrement dit, ils existent toujours en tant que sommets \[x,y,z,w\]) et doivent être convertis en coordonnées non homogènes avant d’être envoyés vers le rastériseur.

**Remarque**  Applications attribuent généralement MinZ et MaxZ à 0,0 et 1,0 respectivement afin que le système pour le rendu à la plage de profondeur entière. Vous pouvez cependant utiliser d'autres valeurs pour obtenir certains effets. Par exemple, vous pouvez attribuer 0,0 aux deux valeurs pour forcer tous les objets au premier plan, ou leur attribuer 1,0 pour afficher tous les objets à l'arrière-plan.

 

## <a name="span-idclearingaviewportspanspan-idclearingaviewportspanspan-idclearingaviewportspanclearing-a-viewport"></a><span id="Clearing_a_Viewport"></span><span id="clearing_a_viewport"></span><span id="CLEARING_A_VIEWPORT"></span>Effacement d’une fenêtre d’affichage


L'effacement de la fenêtre d’affichage réinitialise le contenu du rectangle de fenêtre d’affichage sur la surface de la cible de rendu. Il peut également effacer le rectangle des surfaces de la mémoire tampon de profondeur et de gabarit.

## <a name="span-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanset-up-the-viewport-for-clipping"></a><span id="Set_Up_the_Viewport_for_Clipping"></span><span id="set_up_the_viewport_for_clipping"></span><span id="SET_UP_THE_VIEWPORT_FOR_CLIPPING"></span>Configurer la fenêtre d’affichage pour le découpage


Les résultats de la matrice de projection déterminent le volume de découpage dans l’espace de projection en tant que:

-w<sub>c</sub>&lt;= x<sub>c</sub>&lt;= w<sub>c</sub>

-w<sub>c</sub>&lt;= y<sub>c</sub>&lt;= w<sub>c</sub>

0 &lt;= z<sub>c</sub>&lt;= w<sub>c</sub>

Où: x, y, z et w représentent le vertex des coordonnées après l'application de la transformation de projection. Les vertex dont un composant x, y ou z se trouve en dehors de ces plages sont découpés, si le découpage est activé (comportement par défaut).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 




