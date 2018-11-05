---
title: Vue d’ensemble de la transformation
description: Les transformations de matrice gèrent une grande partie des calculs de bas niveau des graphiques3D.
ms.assetid: B5220EE8-2533-4B55-BF58-A3F9F612B977
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 32f55b0a387221b792e37072f129edddf285195b
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6027985"
---
# <a name="transform-overview"></a>Vue d’ensemble de la transformation


Les transformations de matrice gèrent une grande partie des calculs de bas niveau des graphiques3D.

Le pipeline de géométrie prend les vertex en entrée. Le moteur de transformation applique les transformations universelles, de vue et de projection aux vertex, découpe le résultat et transmet le tout au rastériseur.

| Transformation et espace                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Coordonnées du modèle dans l’espace modèle              | Au début du pipeline, les sommets d’un modèle sont déclarés par rapport à un système de coordonnées local. Il s’agit d’un point d’origine local et d'une orientation. Cette orientation de coordonnées est souvent appelée *espace du modèle*. Les coordonnées individuelles sont appelées *coordonnées du modèle*.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Transformation universelle dans l’espace universel              | La première étape du pipeline de géométrie transforme les vertex d’un modèle à partir de leur système de coordonnées local en un système de coordonnées utilisé par tous les objets d'une scène. Le processus de réorientation des vertex est appelé [transformation universelle](world-transform.md), qui convertit l'espace du modèle en une nouvelle orientation appelée *espace universel*. Chaque vertex de l’espace universel est déclaré à l’aide de *coordonnées universelles*.                                                                                                                                                                                                                                                                                                                           |
| Transformation de vue dans l’espace d’affichage (espace de la caméra) | Dans l’étape suivante, les vertex qui décrivent votre monde en3D sont adaptés à la caméra. Autrement dit, votre application choisit un point de vue de la scène, puis les coordonnées de l'espace universel sont déplacées et pivotent autour de la vue de la caméra, transformant l'espace universel en *espace de vue* (également appelé *espace de la caméra*). Il s’agit de la [transformation de vue](view-transform.md), qui convertit l’espace universel en espace de vue.                                                                                                                                                                                                                                                                                                                        |
| Transformation de la projection en espace de projection    | L’étape suivante est la [transformation de la projection](projection-transform.md), qui convertit l’espace de vue en espace de projection. Dans cette partie du pipeline, les objets sont généralement mis à l’échelle en fonction de leur distance par rapport à l'observateur afin de donner une illusion de profondeur à une scène; l'opération fait apparaître les objets proches plus grands que les objets distants. Par souci de simplicité, cette documentation fait référence à l’espace dans lequel les vertex existent après la transformation de la projection comme *espace de projection*. Certains livres graphiques peuvent se référer à l’espace de projection comme *espace homogène post-perspective*. Toutes les transformations de projection ne mettent pas la taille des objets à l’échelle dans une scène. Ce type de projection est parfois appelé *projection affine* ou *orthogonale*. |
| Découpage dans l’espace d’écran                      | Dans la partie finale du pipeline, les vertex qui ne seront pas visibles sur l’écran sont supprimés afin que le module de rastérisation ne passe pas de temps à calculer les couleurs et l’ombrage d’un élément qui ne sera jamais affiché. Ce processus est nommé *découpage*. Après découpage, les vertex restants sont mis à l’échelle selon les paramètres de la fenêtre d’affichage et convertis en coordonnées d’écran. Les vertex résultants, visibles sur l’écran lorsque la scène est rastérisée, existent dans l'*espace d’écran*.                                                                                                                                                                                                                                                    |

 

Les transformations sont utilisées pour convertir la géométrie de l’objet d’un espace de coordonnées à un autre. Direct3D utilise des matrices pour effectuer les transformations3D. Les matrices créent des transformations3D. Vous pouvez combiner des matrices pour produire une matrice unique qui englobe plusieurs transformations.

Vous pouvez transformer les coordonnées entre l’espace du modèle, l'espace universel et l'espace de vue.

-   [Transformation universelle](world-transform.md): convertit de l'espace du modèle à l'espace universel.
-   [Transformation de vue](view-transform.md): convertit de l’espace universel à l’espace de vue.
-   [Transformation de projection](projection-transform.md): convertit de l’espace de vue à l'espace de projection.

## <a name="span-idmatrixtransformsspanspan-idmatrixtransformsspanspan-idmatrixtransformsspanmatrix-transforms"></a><span id="Matrix_Transforms"></span><span id="matrix_transforms"></span><span id="MATRIX_TRANSFORMS"></span>Transformations de matrices


Dans les applications qui fonctionnent avec des graphiques3D, vous pouvez utiliser les transformations géométriques pour effectuer les opérations suivantes:

-   Exprimer l’emplacement d’un objet par rapport à un autre.
-   Faire pivoter et dimensionner des objets.
-   Modifier les positions d’affichage, les directions et les perspectives.

Vous pouvez transformer n’importe quel point (x, y, z) en un autre (x», y», z») à l’aide d’une matrice 4x4, comme le montre l’équation suivante.

![équation de la transformation de n’importe quel point en un autre point](images/matmult.png)

Effectuer les équations suivantes sur (x, y, z) et la matrice pour produire le point (x», y», z»).

![équations du nouveau point](images/matexpnd.png)

Les transformations les plus courants sont la translation, la rotation et la mise à l’échelle. Vous pouvez combiner les matrices qui produisent ces effets en une matrice unique pour calculer plusieurs transformations à la fois. Par exemple, vous pouvez créer une matrice unique pour traduire et faire pivoter une série de points.

Les matrices s'écrivent dans l’ordre rangée-colonne. Une matrice qui s’adapte uniformément à des sommets le long de chaque axe, ce que l'on appelle mise à l’échelle uniforme, est représentée par la matrice suivante à l’aide d'une notation mathématique.

![équation d’une matrice de mise à l’échelle uniforme](images/matrix.png)

En langage C++, Direct3D déclare les matrices sous forme de tableau à deux dimensions à l’aide d’une structure de matrice. L’exemple suivant montre comment initialiser une structure [**D3DMATRIX**](https://msdn.microsoft.com/library/windows/desktop/bb172573) pour qu'elle agisse comme une mise à l’échelle uniforme de matrice (facteur d’échelle «s»).

```
D3DMATRIX scale = {
    5.0f,            0.0f,            0.0f,            0.0f,
    0.0f,            5.0f,            0.0f,            0.0f,
    0.0f,            0.0f,            5.0f,            0.0f,
    0.0f,            0.0f,            0.0f,            1.0f
};
```

## <a name="span-idtranslatespanspan-idtranslatespanspan-idtranslatespantranslate"></a><span id="Translate"></span><span id="translate"></span><span id="TRANSLATE"></span>Translater


L’équation suivante convertit le point (x, y, z) en un nouveau point (x», y», z»).

![équation d’une matrice de translation pour un nouveau point](images/transl8.png)

Vous pouvez créer manuellement une matrice de translation en C++. L’exemple suivant montre le code source d'une fonction qui crée une matrice pour opérer une translation de vertex.

```
D3DXMATRIX Translate(const float dx, const float dy, const float dz) {
    D3DXMATRIX ret;

    D3DXMatrixIdentity(&ret);
    ret(3, 0) = dx;
    ret(3, 1) = dy;
    ret(3, 2) = dz;
    return ret;
}    // End of Translate
```

## <a name="span-idscalespanspan-idscalespanspan-idscalespanscale"></a><span id="Scale"></span><span id="scale"></span><span id="SCALE"></span>Mettre à l'échelle


L’équation suivante met à l'échelle le point (x, y, z) à l'aide de valeurs arbitraires dans les directions x, y et z vers un nouveau point (x», y», z»).

![équation d’une matrice de mise à l'échelle pour un nouveau point](images/matscale.png)

## <a name="span-idrotatespanspan-idrotatespanspan-idrotatespanrotate"></a><span id="Rotate"></span><span id="rotate"></span><span id="ROTATE"></span>Pivoter


Les transformations décrites ici conviennent à des systèmes de coordonnées pour gaucher et peuvent donc différer des matrices de transformation que vous avez vues ailleurs.

L’équation suivante fait pivoter le point (x, y, z) autour de l’axe x, produisant un nouveau point (x», y», z»).

![équation d’une matrice de rotation x pour un nouveau point](images/matxrot.png)

L’équation suivante fait pivoter le point autour de l’axe y.

![équation d’une matrice de rotation y pour un nouveau point](images/matyrot.png)

L’équation suivante fait pivoter le point autour de l’axe z.

![équation d’une matrice de rotation z pour un nouveau point](images/matzrot.png)

Dans ces exemples de matrices, la lettre grecque thêta symbolise l’angle de rotation, en radians. Les angles sont mesurés dans le sens des aiguilles d'une montre en regardant le long de l’axe de rotation et en remontant vers l’origine.

Le code suivant montre une fonction permettant de gérer la rotation autour de l’axe X.

```
    // Inputs are a pointer to a matrix (pOut) and an angle in radians.
    float sin, cos;
    sincosf(angle, &sin, &cos);  // Determine sin and cos of angle

    pOut->_11 = 1.0f; pOut->_12 =  0.0f;   pOut->_13 = 0.0f; pOut->_14 = 0.0f;
    pOut->_21 = 0.0f; pOut->_22 =  cos;    pOut->_23 = sin;  pOut->_24 = 0.0f;
    pOut->_31 = 0.0f; pOut->_32 = -sin;    pOut->_33 = cos;  pOut->_34 = 0.0f;
    pOut->_41 = 0.0f; pOut->_42 =  0.0f;   pOut->_43 = 0.0f; pOut->_44 = 1.0f;

    return pOut;
}
```

## <a name="span-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanconcatenating-matrices"></a><span id="Concatenating_Matrices"></span><span id="concatenating_matrices"></span><span id="CONCATENATING_MATRICES"></span>Concaténation de matrices


L’un des avantages de l’utilisation de matrices est que vous pouvez combiner les effets de deux matrices (ou davantage) en les multipliant. Cela signifie que, pour faire pivoter un modèle et le translater vers un emplacement, vous n’avez pas besoin d’appliquer deux matrices. Il suffit de multiplier les matrices de rotation et la translation pour produire une matrice composite qui contient tous leurs effets. Ce processus, appelé concaténation de matrice, peut s'écrire avec l’équation suivante.

![équation d'une concaténation de matrices](images/matrxcat.png)

Dans cette équation, C est la matrice composite en cours de création et les matrices M₁ à Mₙ sont les matrices individuelles. Dans la plupart des cas, seules deux ou trois matrices sont concaténées, mais il n’existe aucune limite.

L’ordre dans lequel les matrices sont multipliées est essentiel. La formule précédente reflète la règle de concaténation de matrices de gauche à droite. Autrement dit, les effets visibles des matrices que vous utilisez pour créer une matrice composite se produisent de gauche à droite. L’exemple suivant montre une matrice universelle standard. Supposons que vous souhaitiez créer la matrice universelle d'un stéréotype de soucoupe volante. Vous voudrez probablement faire tourner cette soucoupe volante autour de son centre (l’axe y de l’espace du modèle) et le déplacer vers un autre emplacement de votre scène. Pour réaliser cet effet, vous devez tout d’abord créer une matrice de rotation et la multiplier par une matrice de translation, comme dans l’équation suivante.

![équation de rotation basé sur une matrice de rotation et une matrice de translation](images/wrldexpl.png)

Dans cette formule, R<sub>y</sub> est une matrice de rotation autour de l’axe y et T<sub>w</sub> est une translation vers une position en coordonnées universelles.

L’ordre dans lequel vous multipliez les matrices est important car, contrairement à la multiplication de deux valeurs scalaires, la multiplication de matrices n’est pas commutative. La multiplication des matrices dans l’ordre inverse donne l'impression de voir translater la soucoupe volante vers sa position dans l’espace universel, puis de la voir pivoter autour de l’origine du monde.

Quel que soit le type de matrice que vous créez, n’oubliez pas la règle de gauche à droite pour être sûr d'obtenir les effets escomptés.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Transformations](transforms.md)

 

 




