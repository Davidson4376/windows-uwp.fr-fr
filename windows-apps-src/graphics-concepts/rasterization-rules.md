---
title: Règles de rastérisation
description: Les règles de rastérisation définissent le mode de mappage des données vectorielles en données matricielles.
ms.assetid: B604725F-96A5-4DB6-B597-9EC57FBBC024
keywords:
- Règles de rastérisation
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c622c037f878d1ad34cdadf897dde10683532832
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8204788"
---
# <a name="rasterization-rules"></a>Règles de rastérisation


Les règles de rastérisation définissent le mode de mappage des données vectorielles en données matricielles. Les données matricielles sont ancrées à des emplacements d’entiers qui sont ensuite sélectionnés et rognés (pour dessiner le nombre minimal de pixels), puis les attributs par pixel sont interpolés (à partir des attributs par sommet) avant d’être transmis à un nuanceur de pixels.

Il existe plusieurs types de règles, en fonction du type de primitive en cours de mappage et de l’utilisation ou non par les données de l’échantillonnage multiple pour réduire le crénelage. Les illustrations suivantes montrent comment les cas particuliers sont gérés.

## <a name="span-idtrianglespanspan-idtrianglespanspan-idtrianglespantriangle-rasterization-rules-without-multisampling"></a><span id="Triangle"></span><span id="triangle"></span><span id="TRIANGLE"></span>Règles de rastérisation des triangles (sans échantillonnage multiple)


N’importe quel centre de pixel qui se trouve à l’intérieur d’un triangle est tracé; un pixel est considéré comme intérieur s’il respecte la règle haut/gauche. Selon la règle haut/gauche, un centre de pixel est défini comme étant à l’intérieur d’un triangle s’il se trouve sur le bord supérieur ou le bord gauche d’un triangle.

où:

-   Un bord supérieur est un bord qui est exactement horizontal et se situe au-dessus des autres bords.
-   Un bord gauche est un bord qui n’est pas exactement horizontal et se trouve sur le côté gauche du triangle. Un triangle peut avoir un ou deux bords gauche.

La règle haut/gauche permet de s’assurer que les triangles adjacents sont dessinés une seule fois.

L’illustration suivante montre des exemples de pixels qui sont dessinés car ils se trouvent à l’intérieur d’un triangle ou suivent la règle haut/gauche.

![exemples de rastérisation de triangle suivant la règle haut/gauche](images/d3d10-rasterrulestriangle.png)

La couche gris clair et gris foncé qui recouvre les pixels permet d’afficher ces derniers en tant que groupes pour indiquer dans quel triangle ils se trouvent.

## <a name="span-idline1spanspan-idline1spanspan-idline1spanline-rasterization-rules-aliased-without-multisampling"></a><span id="Line_1"></span><span id="line_1"></span><span id="LINE_1"></span>Règles de rastérisation des lignes (avec crénelage, sans échantillonnage multiple)


Les règles de rastérisation des lignes utilisent une zone de test en forme de losange pour déterminer si une ligne couvre un pixel. Pour les axes x principaux (axes avec -1 &lt;= inclinaison &lt;= + 1), la zone de test en forme de losange inclut (ligne continue sur le schéma) le bord inférieur gauche, le bord inférieur droit et l’angle inférieur; le losange exclut (ligne en pointillés sur le schéma) le bord supérieur gauche, le bord supérieur droit, l’angle supérieur, l’angle gauche et l’angle droit. Un axe y principal est n’importe quel axe qui n’est pas un axe x principal; la zone de test en forme de losange est identique à celle décrite pour l’axe x principal, la seule différence étant que l’angle droit est également inclus.

Si l’on considère la zone en forme de losange, une ligne couvre un pixel si elle sort de la zone de test en forme de losange du pixel lors du déplacement le long de la ligne du début vers la fin. Une bande de lignes se comporte de la même façon, car elle est dessinée comme une séquence de lignes.

L’illustration suivante montre quelques exemples.

![exemples de rastérisation de ligne avec crénelage](images/d3d10-rasterrulesline.png)

## <a name="span-idline2spanspan-idline2spanspan-idline2spanline-rasterization-rules-antialiased-without-multisampling"></a><span id="Line_2"></span><span id="line_2"></span><span id="LINE_2"></span>Règles de rastérisation des lignes (avec anticrénelage, sans échantillonnage multiple)


Une ligne avec anticrénelage est rastérisée comme s’il s’agissait d’un rectangle (avec largeur = 1). Le rectangle croise une cible de rendu, générant ainsi des valeurs de couverture par pixel, qui sont multipliées dans les composants alpha de sortie du nuanceur de pixels. Aucun anticrénelage n’est réalisé lorsque vous dessinez des lignes sur une cible de rendu échantillonnée plusieurs fois.

Il n’existe pas de «meilleure» méthode pour effectuer un rendu de ligne avec anticrénelage. En règle générale, Direct3D adopte la méthode présentée dans l’illustration suivante. Cette méthode a été dérivée de manière empirique et présente un certain nombre de propriétés visuelles jugées souhaitables.

Le matériel n’a pas besoin d’être parfaitement conforme cet algorithme; les tests effectués sur cette référence doivent avoir des tolérances «raisonnables» établies en s’appuyant sur certains des principes répertoriés ci-dessous, ce qui permet d’utiliser plusieurs implémentations matérielles et tailles de noyau de filtre. Toutefois, la flexibilité permise dans le cadre de l’implémentation matérielle peut s’étendre aux applications via Direct3D, au-delà du simple traçage de lignes et de l’observation/de la mesure de leur apparence.

![exemples de rastérisation de ligne avec anticrénelage](images/d3d10-rasterruleslineaa.png)

Cet algorithme génère des lignes relativement lisses, avec une intensité uniforme, ainsi que des phénomènes d'irrégularité des bords et de «tressage» réduits au minimum. L’effet de moiré des lignes proches est réduit au minimum. Les jonctions des segments de ligne mis bout à bout sont bien couverts.

Le noyau de filtre est un compromis raisonnable entre la quantité de flou des bords et les modifications apportées à l’intensité dues aux corrections gamma. La valeur de couverture est multipliée par la formule suivante dans le nuanceur de pixels o0.a (srcAlpha) par [l’étape de fusion de sortie (OM)](output-merger-stage--om-.md):

srcColor \* srcAlpha + destColor \* (1-srcAlpha)

## <a name="span-idpointspanspan-idpointspanspan-idpointspanpoint-rasterization-rules-without-multisampling"></a><span id="Point"></span><span id="point"></span><span id="POINT"></span>Règles de rastérisation des points (sans échantillonnage multiple)


Un point est interprété comme s’il était composé de deux triangles dans un modèle en Z, qui utilise les règles de rastérisation des triangles. La coordonnée identifie le centre d’un carré d’un pixel de largeur. Il n’y a pas d’élimination pour les points.

L’illustration suivante montre quelques exemples.

![exemples de rastérisation de points](images/d3d10-rasterrulespoint.png)

## <a name="span-idmultisamplespanspan-idmultisamplespanspan-idmultisamplespanmultisample-anti-aliasing-rasterization-rules"></a><span id="Multisample"></span><span id="multisample"></span><span id="MULTISAMPLE"></span>Règles de rastérisation avec anticrénelage à échantillonnage multiple


L’anticrénelage à échantillonnage multiple (MSAA) réduit le crénelage de la géométrie à l’aide des données de couverture des pixels et des tests de gabarit et de profondeur à plusieurs emplacements de sous-échantillonnage. Pour améliorer les performances, les calculs par pixel sont effectués une fois pour chaque pixel couvert, en partageant les sorties du nuanceurs entre les sous-pixels couverts. L’anticrénelage à échantillonnage multiple ne réduit pas le crénelage surfacique. Les emplacements d’échantillon et les fonctions de reconstruction sont dépendants de l’implémentation matérielle.

L’illustration suivante montre quelques exemples.

![exemples de rastérisation de type anticrénelage à échantillonnage multiple](images/d3d10-rasterrulesmsaa.png)

Le nombre d’emplacements d’échantillon dépend du mode d’échantillonnage multiple. Les attributs de sommet sont interpolés au niveau des centres de pixels, car c’est là que le nuanceur de pixels est appelé (cela devient de l’extrapolation si le centre n’est pas couvert). Les attributs peuvent être marqués dans le nuanceur de pixels comme ayant fait l’objet d’un échantillonnage par centroïde, ce qui entraîne une interpolation des pixels non couverts avec l’attribut à l’intersection de la zone de pixels et de la primitive.

Un nuanceur de pixels est exécuté pour chaque zone de 2x2 pixels afin de prendre en charge les calculs de dérivés (qui utilisent les deltas x et y). Cela signifie que les appels du nuanceur se produisent plus souvent que ce qui est indiqué afin de remplir les quanta minimum de 2x2 (indépendamment de l’échantillonnage multiple). Le résultat du nuanceur est écrit pour chaque échantillon couvert qui réussit le test de gabarit et de profondeur.

Les règles de rastérisation pour les primitives ne sont pas, en règle générale, modifiées par l’anticrénelage à échantillonnage multiple, sauf dans les cas suivants:

-   Pour un triangle, un test de couverture est réalisé pour chaque emplacement d’échantillon (et non pour un centre de pixel). Si plusieurs emplacements d’échantillon sont couverts, un nuanceur de pixels s’exécute une fois avec les attributs interpolés au centre du pixel. Le résultat est stocké (répliqué) pour chaque emplacement d’échantillon couvert dans le pixel réussit le test de profondeur/gabarit.

    Une ligne est traitée comme un rectangle constitué de deux triangles, avec une largeur de ligne de 1,4.

-   Pour un point, un test de couverture est réalisé pour chaque emplacement d’échantillon (et non pour un centre de pixel).

Les formats d’échantillonnage multiple peuvent être utilisés dans les cibles de rendu, qui peuvent être lues a posteriori dans les nuanceurs à l’aide du [chargement](https://msdn.microsoft.com/library/windows/desktop/bb509694), car aucune résolution n’est requise pour les échantillons individuels auxquels accède le nuanceur. Les formats de profondeur ne sont pas pris en charge pour les ressources à échantillonnage multiple, par conséquent, ils sont limités uniquement aux cibles de rendu.

Les formats sans type prennent en charge l’échantillonnage multiple pour permettre à une vue de ressource d’interpréter les données de différentes manières. Par exemple, vous pouvez créer une ressource à échantillonnage multiple à l’aide de R8G8B8A8\_TYPELESS, procéder au rendu à l’aide d’une ressource de vue de cible de rendu avec un format R8G8B8A8\_UINT, puis résoudre les contenus dans une autre ressource avec un format de données R8G8B8A8\_UNORM.

### <a name="span-idhardwaresupportspanspan-idhardwaresupportspanspan-idhardwaresupportspanhardware-support"></a><span id="Hardware_Support"></span><span id="hardware_support"></span><span id="HARDWARE_SUPPORT"></span>Prise en charge par le matériel

L’API utilise le nombre de niveaux de qualité pour indiquer le niveau de prise en charge de l’échantillonnage multiple par le matériel. Par exemple, un niveau de qualité de 0 signifie que le matériel ne prend pas en charge l’échantillonnage multiple (à un format et un niveau de qualité spécifiques). Un niveau de qualité de 3 signifie que le matériel prend en charge trois dispositions d’échantillons différentes et/ ou les algorithmes de résolution. Vous pouvez également prendre en compte les informations suivantes:

-   Tout format qui prend en charge l’échantillonnage multiple prend en charge le même nombre de niveaux de qualité pour chaque format de cette famille.
-   Tout format qui prend en charge l’échantillonnage multiple et contient les formats \_UNORM, \_SRGB, \_SNORM ou \_FLOAT prend également en charge la résolution.

### <a name="span-idcentroidsamplingspanspan-idcentroidsamplingspanspan-idcentroidsamplingspancentroid-sampling-of-attributes-when-multisample-antialiasing"></a><span id="Centroid_Sampling"></span><span id="centroid_sampling"></span><span id="CENTROID_SAMPLING"></span>Échantillonnage par centroïde des attributs lors de l’anticrénelage à échantillonnage multiple

Par défaut, les attributs de sommet sont interpolés vers un centre de pixel au cours de l’anticrénelage à échantillonnage multiple; si le centre de pixel n’est pas couvert, les attributs sont extrapolés vers un centre de pixel. Une entrée de nuanceur de pixels qui contient la valeur sémantique du centroïde (en supposant que le pixel n’est pas intégralement couvert) sera échantillonnée à l’intérieur de la zone couverte du pixel, éventuellement à l’un des emplacements de l’échantillon couvert. Un masque d’échantillon (spécifié par l’état du rastériseur) est appliqué avant le calcul du centroïde. Par conséquent, un échantillon masqué ne sera pas utilisé comme emplacement du centroïde.

Le rastériseur de référence sélectionne un emplacement d’échantillon pour l’échantillonnage par centroïde en procédant comme suit:

-   Le masque d’échantillon active tous les échantillons. Utilisez un centre de pixel si le pixel est couvert ou si aucun des échantillons n’est couvert. Sinon, le premier échantillon couvert est sélectionné, à partir du centre du pixel vers l’extérieur.
-   Le masque d’échantillonnage désactive tous les échantillons sauf un (un scénario courant). Une application peut implémenter le superéchantillonnage multipasse en parcourant les valeurs du masque d’échantillon à un bit et en effectuant à nouveau le rendu de la scène pour chaque échantillon à l’aide de l’échantillonnage par centroïde. Cela implique qu’une application ajuste les dérivés pour sélectionner correctement des mips de texture plus détaillés pour la densité d’échantillonnage de texture plus élevée.

### <a name="span-idderivativecalculationsspanspan-idderivativecalculationsspanspan-idderivativecalculationsspanderivative-calculations-when-multisampling"></a><span id="Derivative_Calculations"></span><span id="derivative_calculations"></span><span id="DERIVATIVE_CALCULATIONS"></span>Calculs de dérivés lors de l’échantillonnage multiple

Les nuanceurs de pixels utilisent toujours une zone de 2 x 2pixels au minimum pour prendre en charge les calculs de dérivés, qui sont réalisés en prenant les deltas entre les données à partir des pixels adjacents (en partant du principe que les données de chaque pixel ont été échantillonnées avec l’espacement d’unité, horizontalement ou verticalement). Cela n’est pas affecté par l’échantillonnage multiple.

Si des dérivés sont demandés sur un attribut ayant fait l’objet d’un échantillonnage par centroïde, le calcul du matériel n’est pas ajusté, ce qui peut générer des dérivés inexacts. Un nuanceur s’attend à recevoir un vecteur unitaire dans l’espace de cible de rendu, mais peut obtenir un vecteur non unitaire par rapport à un autre espace vectoriel. Par conséquent, il incombe aux applications de faire preuve de prudence en demandant des dérivés à partir d’attributs ayant fait l’objet d’un échantillonnage par centroïde.

En fait, il est recommandé de ne pas combiner dérivés et échantillonnage par centroïde. L’échantillonnage par centroïde peut être utile dans les situations où il est essentiel que les attributs interpolés d’une primitive ne soient pas extrapolés, mais cela implique certains compromis, par exemple des attributs qui semblent sauter à l’endroit où un bord de primitive croise un pixel (au lieu de se modifier en continu), ou encore des dérivés qui ne peuvent pas être utilisés par les opérations d’échantillonnage de texture qui déterminent le niveau de détail.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Annexes](appendix.md)

[Étape Rastériseur (RS)](rasterizer-stage--rs-.md)

 

 




