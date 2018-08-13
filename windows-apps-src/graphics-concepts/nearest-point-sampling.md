---
title: Échantillonnage des points les plus proches
description: Les applications ne sont pas tenues d’utiliser le filtrage de textures.
ms.assetid: D7F88320-2C61-47E9-9B92-EC31D48DB079
keywords:
- Échantillonnage des points les plus proches
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 048ff2755343e5e6230900ea9acb819e85ba654b
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044408"
---
# <a name="span-iddirect3dconceptsnearest-pointsamplingspannearest-point-sampling"></a><span id="direct3dconcepts.nearest-point_sampling"></span>Échantillonnage des points les plus proches


Les applications ne sont pas tenues d’utiliser le filtrage de textures. Direct3D peut être configuré pour calculer l’adresse des texels, qui souvent ne correspond pas à des entiers, et pour copier la couleur des texels avec l’adresse à valeur entière la plus proche. Ce processus est appelé *échantillonnage des points les plus proches*. L’échantillonnage des points les plus proches peut constituer un moyen simple et efficace de traiter les textures, si la taille de la texture est similaire à la taille de l’image de la primitive sur l’écran. Si ce n’est pas le cas, la texture doit être agrandie ou réduite. Une non-concordance entre la taille de texture et la taille de l’image de la primitive peut générer une image indistincte, crénelée ou floue.

Recourez avec précaution à l’échantillonnage des points les plus proches, dans la mesure où cette opération peut parfois générer des artefacts graphiques quand la texture est échantillonnée au niveau de la limite entre 2texels. Cette limite correspond à la position le long de la texture (u ou v), à laquelle le texel échantillonné passe d’un texel au suivant. Lorsque l’échantillonnage des points est utilisé, le système choisit un des deux texels échantillonnés. Selon qu’il s’agisse de l’un ou de l’autre, le résultat peut changer abruptement, dans la mesure où la limite est franchie. Cela peut créer des artefacts graphiques non souhaités dans la structure affichée. Lorsque le filtrage linéaire est utilisé, le texel obtenu est calculé à partir des deuxtexels adjacents et se fond entre eux, alors que l’index de texture traverse la limite.

Cela peut se produire lors du mappage d’une structure très réduite sur un polygone très grand: une opération souvent appelée «grossissement». Par exemple, lorsque la texture utilisée est similaire à un damier, l’échantillonnage des points les plus proches génère un damier plus grand, qui laisse apparaître distinctement les bords. A contrario, le filtrage de texture linéaire génère une image sur laquelle les couleurs du damier varient légèrement sur le polygone.

Dans la plupart des cas, les applications obtiennent des résultats optimaux en évitant dans la mesure du possible de recourir à l’échantillonnage des points les plus proches. Aujourd’hui, la majorité des composants matériels sont optimisés pour le filtrage linéaire, ce qui réduit le risque de dégradation des performances de votre application. Si l’effet souhaité nécessite absolument le recours à l’échantillonnage des points les plus proches, comme lors de l’utilisation des textures pour l’affichage de caractères de texte lisibles, votre application doit éviter à tout prix d’échantillonner les limites de texel, afin d’éviter toute conséquence fâcheuse. L’illustration suivante représente des exemples de ces artefacts.

![illustration d’une zone à sixsections avec des lignes horizontales non continues dans les carrés du coin supérieur droit](images/ptrtfct.png)

Les deuxcarrés du coin supérieur droit du groupe présentent un aspect différent de leurs voisins; ils sont pourvus de décalages diagonaux. Pour empêcher les artefacts graphiques de ce type, vous devez maîtriser les règles d’échantillonnage de textures Direct3D pour le filtrage des points les plus proches. Direct3D mappe une coordonnée de texture à virgule flottante comprise entre \[0.0 et 1.0\] (inclus) sur une valeur d’espace de texel entier comprise entre \[ -0.5 et n -0.5\], où n est le nombre de texels d’une dimension données sur la texture. L’index de texture obtenu est arrondi au nombre entier le plus proche. Ce mappage peut introduire des inexactitudes d’échantillonnage sur les limites de texels.

Pour prendre un exemple simple, imaginons une application qui génère des polygones avec le mode d’adressage de texture Wrap. À l’aide du mappage employé par Direct3D, l’index de textureu mappe comme illustré dans le diagramme suivant pour une texture présentant une largeur de 4texels.

![coordonnées 0,0 et 1,0 du diagramme de texture à la limite entre les texels](images/ptsmpprb.png)

Les coordonnées de texture, 0.0 et 1.0 pour cette illustration, se trouvent exactement à la limite entre les texels. En appliquant la méthode valorisée par Direct3D pour mapper les valeurs, les coordonnées de texture sont comprises dans la tranche \[ - 0,5, 4 - 0,5\], où 4 correspond à la largeur de la texture. Dans ce cas, le texel échantillonné est le texel0 pour un index de texture de 1.0. Toutefois, si la coordonnée de la texture était légèrement inférieure à 1,0, le texel échantillonné sera le texeln, en lieu et place du texel0.

Par conséquent, l’agrandissement d’une petite structure avec des coordonnées de texture exactement égales à 0,0 et 1,0 à l’aide du filtrage des points les plus proches sur un triangle aligné de la taille de l’écran génère des pixels dont la carte de texture est échantillonnée à la limite entre les texels. Toute inexactitude dans le calcul des coordonnées de texture, même minime, produit des artefacts à proximité des zones de l’image affichée correspondant aux bords de texels d’une carte de texture.

L’opération de mappage avec une exactitude parfaite de ces coordonnées de texture à virgule flottante sur des texels d’entiers s’avère difficile, chronophage et généralement inutile. La plupart des implémentations matérielles valorisent une approche itérative pour calculer des coordonnées de texture à chaque emplacement de pixel d’un triangle. Les approches itératives ont tendance à masquer ces inexactitudes, dans la mesure où ces erreurs sont accumulées de manière régulière durant l’itération.

Le rastériseur de référence Direct3D applique une approche d’évaluation directe pour calculer les index de calcul à chaque emplacement de pixel. L’évaluation directe diffère de l’approche itérative, en ce sens qu’une inexactitude dans l’opération présente une distribution d’erreurs plus aléatoire. En conséquence, les erreurs d’échantillonnage se produisant sur les limites peuvent être plus flagrantes, car le rastériseur de référence n’exécute pas cette opération avec une exactitude parfaite.

La meilleure approche consiste à utiliser le filtrage des points les plus proches uniquement lorsque cela s’avère nécessaire. Lorsque vous devez recourir à cette méthode, nous vous recommandons de décaler légèrement les coordonnées de texture de la limite, ceci pour éviter les artefacts.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Filtrage de textures](texture-filtering.md)

 

 




