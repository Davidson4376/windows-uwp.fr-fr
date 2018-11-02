---
title: Habillage de texture
description: L’habillage de texture modifie la méthode de base avec laquelle Direct3D rastérise des polygones avec texture à l’aide de coordonnées de texture spécifiées pour chaque sommet.
ms.assetid: C28FB369-9A91-4D57-A96D-4A5D36484B35
keywords:
- Habillage de texture
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28dcb134b87ac136b341d5b1f349ac9d656ef642
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5997375"
---
# <a name="texture-wrapping"></a>Habillage de texture


L’habillage de texture modifie la méthode de base avec laquelle Direct3D rastérise des polygones avec texture à l’aide de coordonnées de texture spécifiées pour chaque sommet. Lors de la rastérisation d’un polygone, le système effectue une interpolation entre les coordonnées de texture à chaque sommet du polygone pour déterminer les texels devant être utilisés pour chaque pixel du polygone. Le système traite généralement la texture comme un plan 2D et effectue une interpolation des nouveaux texels en choisissant la distance la plus courte d’un point A à un point B dans une texture. Si le point A représente la position u, v (0.8, 0.1) et le point B est situé à (0.1, 0.1), la ligne d’interpolation ressemble au diagramme suivant.

![diagramme d’une ligne d’interpolation entre deux points](images/interp1.png)

Notez que la plus courte distance entre A et B dans l’illustration suivante passe à peu près par le milieu de la texture. L’activation de l’habillage des coordonnées de texture u ou v modifie la façon dont Direct3D perçoit la distance la plus courte entre les coordonnées de texture dans les direction u et v. Par définition, l’habillage de texture pousse le rastériseur à suivre la distance la plus courte entre les ensembles de coordonnées de texture, en supposant que 0.0 et 1.0 coïncident. Le dernier bit induit une difficulté. L’activation de l’habillage de texture dans une direction pousse le système à traiter la texture comme si elle était dotée d’un habillage autour d’un cylindre. Prenons, parexemple, le diagramme suivant.

![diagramme d’une texture et de deux points dotés d’un habillage autour d’un cylindre](images/interp2.png)

L’illustration ci-dessus montre comment l’habillage dans la direction u affecte l’interpolation des coordonnées de texture par le système. En utilisant les mêmes points que dans l’exemple pour les textures normales ou non dotées d’un habillage, vous pouvez voir que la distance la plus courte entre les points A et B n’est plus le milieu de la texture, mais la bordure où 0,0 et 1,0 coexistent. L’habillage dans la direction v est similaire, sauf qu’il recouvre la texture autour d’un cylindre situé sur le côté. L’habillage dans les directions u et v est plus complexe. Dans ce cas, vous pouvez envisager la texture sous forme d’une bague ou d’un anneau.

L’application la plus pratique pour l’habillage de texture consiste à effectuer un mappage de l’environnement. En règle générale, un objet avec une texture et un mappage d’environnement apparaît de façon très réfléchissante, en montrant une image en miroir de l’environnement de l’objet dans la scène. Pour illustrer cet exemple, dessinez une salle avec quatre murs, chacun d’entre eux contenant une des lettres R, G, B, Y peintes dans les couleurs correspondantes: rouge, vert, bleu et jaune. Le mappage d’environnement pour cet espace simple peut ressembler à l’illustration suivante.

![illustration de bandes verticales en rouge, vert, bleu et jaune](images/envmap.png)

Imaginez que le plafond de cette salle est maintenu par un pilier à quatre côtés, parfaitement réfléchissant. Le mappage de la texture de mappage de l’environnement au pilier est simple. Il est plus difficile de faire apparaître le pilier comme s’il reflétait les lettres et les couleurs. Le diagramme suivant illustre le cadre filaire du pilier avec les coordonnées de texture applicables listées près des sommets supérieurs. L’intersection de l’habillage avec les bords de la texture s’affiche en ligne pointillée.

![diagramme d’un rectangle avec une ligne pointillée bissectrice](images/seam.png)

Lorsque l’habillage est activé dans la direction u, le pilier avec texture affiche les couleurs et les symboles du mappage d’environnement associés et à l’intersection avant de la texture, le rastériseur choisit correctement la distance la plus courte entre les coordonnées de texture, en supposant que ces coordonnées u 0.0 et 1.0 partagent le même emplacement. Le pilier avec texture ressemble à l’illustration suivante.

![illustration d’un pilier composé de quadrants rouges, verts, bleus et jaunes](images/tex-seam.png)

Si l’habillage de texture n’est pas activé, le rastériseur n’effectue pas l’interpolation dans la direction nécessaire pour générer une image reflétée crédible. Au lieu de cela, la zone à l’avant du pilier contient une version compressée horizontalement des texels entre les coordonnées u 0.175 et 0.875, lorsqu’ils passent par le centre de la texture. L’effet d’habillage est effacé.

Ne confondez pas l’habillage de texture avec les modes d’adressage de texture portant les mêmes noms. L’habillage de texture est exécuté avant l’adressage de texture. Assurez-vous que l’habillage texture ne contient pas de coordonnées de texture en dehors de la plage de \[0.0, 1.0], car cela produirait des résultats inattendus. Pour plus d’informations sur l’adressage de texture, voir les [Modes d’adressage de texture](texture-addressing-modes.md).

## <a name="span-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspandisplacement-map-wrapping"></a><span id="Displacement_Map_Wrapping"></span><span id="displacement_map_wrapping"></span><span id="DISPLACEMENT_MAP_WRAPPING"></span>Habillage de mappage de déplacement


Les mappages de déplacement sont interpolés par le moteur de pavage. Le mode d’habillage ne pouvant être spécifié pour le moteur de pavage, l’habillage de texture ne peut pas être effectué avec les mappages de déplacement. Une application peut utiliser un ensemble de sommets qui force l’interpolation pour effectuer un habillage dans n’importe quelle direction. L’application peut également spécifier l’exécution d’une interpolation en tant qu’interpolation linéaire simple.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Textures](textures.md)

 

 




