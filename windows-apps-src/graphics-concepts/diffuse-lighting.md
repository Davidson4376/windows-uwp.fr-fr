---
title: Éclairage diffus
description: L’éclairage diffus dépend de la direction de la lumière et de la normale à la surface de l’objet.
ms.assetid: 8AF78742-76B1-4BBB-86E3-94AE6F48B847
keywords:
- Éclairage diffus
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1785b06aa2217e8ec15aeaa560bd98a65522df2e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8741837"
---
# <a name="diffuse-lighting"></a>Éclairage diffus


L'*éclairage diffus* dépend de la direction de la lumière et de la surface de l’objet. L’éclairage diffus varie à la surface d’un objet, en réponse aux modifications de la direction d’éclairage et du vecteur numéral à la surface. Le calcul de l’éclairage diffus exige davantage de temps, car cette valeur varie pour chaque vertex d’objet. Parallèlement, il permet d’ombrer des objets et de leur octroyer une profondeur en 3dimensions.

Après avoir ajusté l’intensité lumineuse pour appliquer des effets d’atténuation, le moteur d’éclairage calcule la quantité de lumière restante reflétée par un vertex, en fonction de l’angle de la normale de vertex et de la direction de la lumière incidente. Le moteur d’éclairage ignore cette étape pour les lumières directionnelles, car ces dernières ne s’atténuent pas avec la distance. Le système considère deux types de réflexions, diffuse et spéculaire, et utilise une formule différente pour déterminer la quantité de lumière reflétée pour chacune.

Après avoir calculé les quantités de lumière reflétées, Direct3D applique ces nouvelles valeurs aux propriétés de facteur de réflexion diffuse et spéculaire du matériau actuel. Les valeurs de couleur qui en résultent sont les composants diffus et spéculaire que le rastériseur utilise pour produire l’ombrage Gouraud et le reflet spéculaire.

Il est possible de calculer l’éclairage diffus en utilisant l’équation suivante:

Éclairage diffus = sum\[C<sub>d</sub>\*L<sub>d</sub>\*(N<sup>.</sup>L<sub>dir</sub>)\*Atten\*Spot\]

| Paramètre       | Valeur par défaut | Type          | Description                                                                                      |
|-----------------|---------------|---------------|--------------------------------------------------------------------------------------------------|
| sum             | Non applicable           | Non applicable           | Somme des composants diffus de chaque lumière.                                                     |
| C<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | Couleur diffuse.                                                                                   |
| L<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | Couleur diffuse de la lumière.                                                                             |
| N               | Non applicable           | D3DVECTOR     | Normale de vertex.                                                                                    |
| L<sub>dir</sub> | Non applicable           | D3DVECTOR     | Vecteur de direction entre le vertex de l’objet et la lumière.                                                |
| Atten           | Non applicable           | FLOAT         | Atténuation de la lumière. Voir [Atténuation et facteur de point lumineux](attenuation-and-spotlight-factor.md). |
| Pt lum            | Non applicable           | FLOAT         | Facteur de point lumineux. Voir [Atténuation et facteur de point lumineux](attenuation-and-spotlight-factor.md).  |

 

Pour calculer l’atténuation (Atten) ou les caractéristiques de point lumineux (Spot), consultez l’article [Atténuation et facteur de point lumineux](attenuation-and-spotlight-factor.md).

Les composants diffus sont restreints à une plage de valeurs comprise entre 0 et 255. Ensuite, toutes les lumières sont traitées et interpolées séparément. La valeur d’éclairage diffus résultante est une combinaison des valeurs de lumière ambiante, diffuse et émissive.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


Dans cet exemple, l’objet est coloré à l’aide de la couleur diffuse de la lumière et d’une couleur diffuse de matériau.

Selon l’équation, la couleur obtenue pour les vertex d’objet combine la couleur du matériau et la couleur de la lumière.

Les deux illustrations suivantes montrent la couleur du matériau (grise) et la couleur de la lumière (rouge vif).

![Illustration d’une sphère grise](images/amb1.jpg)![Illustration d’une sphère rouge](images/lightred.jpg)

La scène obtenue est présentée dans l’illustration suivante. Le seul objet de la scène est une sphère. Le calcul de l’éclairage diffus considère la couleur diffuse du matériau et de la lumière et la modifie par l’angle entre la direction de la lumière et la normale de vertex à l’aide du produit scalaire. En conséquence, la partie arrière de la sphère s’assombrit à mesure que la surface de la sphère s’éloigne de la lumière.

![illustration d’une sphère avec un éclairage diffus](images/lightd.jpg)

La combinaison de l’éclairage diffus et de l’éclairage ambiant de l’exemple précédent entraîne l’ombrage de la totalité de la surface de l’objet. La lumière ambiante ombre la surface entière, et la lumière diffuse contribue à révéler la forme3D de l’objet, comme illustré dans la figure suivante.

![illustration d’une sphère avec un éclairage diffus et un éclairage ambiant](images/lightad.jpg)

L’éclairage diffus est plus difficile à calculer que l’éclairage ambiant. Étant donné qu’il dépend des normales de vertex et de la direction de la lumière, vous pouvez visualiser la géométrie des objets dans l’espace3D, ce qui produit un éclairage plus réaliste que l’éclairage ambiant. Vous pouvez accroître l’aspect réaliste en utilisant des reflets spéculaires.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Formules mathématiques d’éclairage](mathematics-of-lighting.md)

 

 




