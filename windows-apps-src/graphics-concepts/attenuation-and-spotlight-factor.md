---
title: Atténuation et facteur de point lumineux
description: Les composants d’éclairage diffus et spéculaire de l’équation d'illumination globale contiennent des termes qui décrivent l’atténuation de lumière et le cône de point lumineux.
ms.assetid: F61D4ACB-09AB-4087-9E2D-224E472D6196
keywords:
- Atténuation et facteur de point lumineux
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8126ac8fa738a2b8a9680d215179fe23f77c5d44
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8749605"
---
# <a name="attenuation-and-spotlight-factor"></a>Atténuation et facteur de point lumineux


Les composants d’éclairage diffus et spéculaire de l’équation d'illumination globale contiennent des termes qui décrivent l’atténuation de lumière et le cône de point lumineux. Ces termes sont décrits ci-après.

## <a name="span-idattenuationspanspan-idattenuationspanspan-idattenuationspanattenuation"></a><span id="Attenuation"></span><span id="attenuation"></span><span id="ATTENUATION"></span>Atténuation


L’atténuation de lumière varie selon le type de lumière et la distance entre la lumière et la position du vertex. Pour calculer l’atténuation, utilisez l'une des équations suivantes.

Atten = 1/( att0<sub>i</sub> + att1<sub>i</sub> \* d + att2<sub>i</sub> \* d²)

Où:

| Paramètre        | Valeur par défaut | Type           | Description                                     | Plage          |
|------------------|---------------|----------------|-------------------------------------------------|----------------|
| att0<sub>i</sub> | 0,0           | Virgule flottante | Facteur d’atténuation constante                     | 0 à +infini |
| att1<sub>i</sub> | 0,0           | Virgule flottante | Facteur d’atténuation linéaire                       | 0 à +infini |
| att2<sub>i</sub> | 0,0           | Virgule flottante | Facteur d’atténuation quadratique                    | 0 à +infini |
| d                | Non applicable           | Virgule flottante | Distance entre la position du vertex et la position de la lumière | Non applicable            |

 

-   Atten = 1 si la lumière est une lumière directionnelle.
-   Atten = 0 si la distance entre la lumière et le vertex dépasse la plage de la lumière.

La distance entre la lumière et la position du vertex est toujours positive.

d = | L<sub>dir</sub> |

Où:

| Paramètre       | Valeur par défaut | Type                                             | Description                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dir</sub> | Non applicable           | Vecteur 3D avec des valeurs à virgule flottante x, y et z | Vecteur de direction entre la position du vertex et la position de la lumière |

 

Si d est supérieur à la plage de la lumière, Direct3D n'effectue aucun autre calcul d'atténuation et n'applique aucun effet de lumière sur le vertex.

Les constantes d'atténuation agissent comme des coefficients dans la formule: vous pouvez produire diverses courbes d'atténuation en y apportant de simples ajustements. Vous pouvez définir Attenuation1 sur 1,0 pour créer une lumière qui ne s'atténuera pas, mais qui restera limitée par la plage; vous pouvez aussi tester différentes valeurs pour obtenir des effets d’atténuation différents.

L’atténuation à la plage maximale de la lumière n’est pas égale à 0,0. Pour éviter que des lumières apparaissent subitement lorsqu’elles se trouvent dans la plage de lumière, une application peut augmenter la plage de lumière. Sinon, l’application peut définir des constantes d'atténuation afin que le facteur d’atténuation soit proche de 0,0 à la plage de lumière. La valeur d’atténuation est multipliée par les composants rouges, verts et bleus de la couleur de la lumière pour mettre à l’échelle l’intensité de la lumière en tant que facteur de la distance parcourue par la lumière pour atteindre un vertex.

## <a name="span-idspotlight-factorspanspan-idspotlight-factorspanspan-idspotlight-factorspanspotlight-factor"></a><span id="Spotlight-Factor"></span><span id="spotlight-factor"></span><span id="SPOTLIGHT-FACTOR"></span>Facteur de point lumineux


L’équation suivante spécifie le facteur de point lumineux.

![équation du facteur de point lumineux](images/dx8light9.png)

| Paramètre         | Valeur par défaut | Type           | Description                              | Plage                    |
|-------------------|---------------|----------------|------------------------------------------|--------------------------|
| rho<sub>i</sub>   | Non applicable           | Virgule flottante | cosinus (angle) du point lumineux i            | Non applicable                      |
| phi<sub>i</sub>   | 0,0           | Virgule flottante | Angle de pénombre du point lumineuxi en radians | \[theta<sub>i</sub>, pi) |
| theta<sub>i</sub> | 0,0           | Virgule flottante | Angle d'ombre du point lumineuxi en radians    | \[0, pi)                 |
| atténuation           | 0,0           | Virgule flottante | Facteur d’atténuation                           | (-infini, +infini)   |

 

Où:

rho = norm(L<sub>dcs</sub>)<sup>.</sup>norm(L<sub>dir</sub>)

et:

| Paramètre       | Valeur par défaut | Type                                             | Description                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dcs</sub> | Non applicable           | Vecteur 3D avec des valeurs à virgule flottante x, y et z | La valeur négative de la direction de la lumière dans l’espace de la caméra         |
| L<sub>dir</sub> | Non applicable           | Vecteur 3D avec des valeurs à virgule flottante x, y et z | Vecteur de direction entre la position du vertex et la position de la lumière |

 

Après avoir calculé l’atténuation de lumière, Direct3D prend également en compte les effets de point lumineux, le cas échéant, l’angle de réflexion de la lumière sur une surface et le facteur de réflexion du matériau actuel afin de calculer les composants diffus et spéculaires du vertex. Dans [types de lumière](light-types.md), voir «Point lumineux».

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Formules mathématiques d’éclairage](mathematics-of-lighting.md)

 

 




