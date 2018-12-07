---
title: Éclairage spéculaire
description: L'éclairage spéculaire désigne les reflets spéculaires vifs qui surviennent lorsque la lumière atteint la surface d'un objet et se reflète sur l'appareil photo.
ms.assetid: 71F87137-B00F-48CE-8E6A-F98A139A742A
keywords:
- Éclairage spéculaire
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7f28f1f46cfd34ee1aab614c57dc99019dbd6111
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8782289"
---
# <a name="specular-lighting"></a>Éclairage spéculaire


L'*éclairage spéculaire* identifie les points spéculaires lumineux qui apparaissent lorsque la lumière atteint la surface de l’objet et la reflète vers l’appareil photo. L’éclairage spéculaire est plus intense que la lumière diffuse et tombe plus rapidement sur la surface de l’objet. Le calcul de l’éclairage spéculaire prend plus de temps que celui de l'éclairage diffus. Toutefois, le fait de l'utiliser permet de préciser des informations importantes sur une surface.

La modélisation de la réflexion spéculaire nécessite que le système connaisse la direction dans laquelle la lumière se déplace et la direction vers les yeux de l’observateur. Le système utilise une version simplifiée du modèle de réflexion spéculaire Phong, qui utilise un vecteur à mi-chemin pour estimer l’intensité de réflexion spéculaire.

L’état d’éclairage par défaut ne calcule pas les reflets spéculaires.

## <a name="span-idspecularlightingequationspanspan-idspecularlightingequationspanspan-idspecularlightingequationspanspecular-lighting-equation"></a><span id="Specular_Lighting_Equation"></span><span id="specular_lighting_equation"></span><span id="SPECULAR_LIGHTING_EQUATION"></span>Équation de l'éclairage spéculaire


Il est possible de calculer l'éclairage spéculaire en utilisant l’équation suivante:

|                                                                             |
|-----------------------------------------------------------------------------|
| Éclairage spéculaire = Cₛ \ * sum\ [Lₛ \ * (N · H)<sup>P</sup> \ * Attén \ * Pt Lum\] |

 

Les variables, leurs types et leurs plages sont les suivantes:

| Paramètre    | Valeur par défaut | Type                                                             | Description                                                                                            |
|--------------|---------------|------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Cₛ           | (0,0,0,0)     | Rouge, vert, bleu et transparence alpha (valeurs à virgule flottante) | Couleur spéculaire.                                                                                        |
| sum          | Non applicable           | Non applicable                                                              | Somme des composants spéculaires de chaque lumière.                                                          |
| N            | Non applicable           | Vecteur 3D (valeurs à virgule flottante x, y et z)                    | Normale du vertex.                                                                                         |
| H            | Non applicable           | Vecteur 3D (valeurs à virgule flottante x, y et z)                    | Vecteur à mi-chemin. Consultez la section sur le vecteur à mi-chemin.                                                |
| <sup>P</sup> | 0,0           | Virgule flottante                                                   | Puissance de réflexion spéculaire. Est comprise entre 0 et + l'infini                                                     |
| Lₛ           | (0,0,0,0)     | Rouge, vert, bleu et transparence alpha (valeurs à virgule flottante) | Couleur de la lumière spéculaire.                                                                                  |
| Attén        | Non applicable           | Virgule flottante                                                   | Valeur d'atténuation de la lumière. Voir [Atténuation et facteur de point lumineux](attenuation-and-spotlight-factor.md). |
| Pt lum         | Non applicable           | Virgule flottante                                                   | Facteur de point lumineux. Voir [Atténuation et facteur de point lumineux](attenuation-and-spotlight-factor.md).        |

 

La valeur pour Cₛ possède l'une des valeurs suivantes:

-   couleur1 du vertex, si la source du matériau spéculaire est la couleur du vertex diffus et que la première couleur du vertex est indiquée dans la déclaration du vertex.
-   couleur2 du vertex, si la source du matériau spéculaire est la couleur du vertex spéculaire, et que la deuxième couleur du vertex est indiquée dans la déclaration du vertex.
-   couleur spéculaire du matériau

**Remarque**  si l’option de source du matériau spéculaire est utilisée et la couleur du vertex n’est pas fournie, la couleur spéculaire du matériau est utilisée.

 

Les composants spéculaires sont restreints pour être compris entre 0 et 255. Ensuite, les lumières sont traitées et interpolées séparément.

## <a name="span-idthehalfwayvectorspanspan-idthehalfwayvectorspanspan-idthehalfwayvectorspanthe-halfway-vector"></a><span id="The_Halfway_Vector"></span><span id="the_halfway_vector"></span><span id="THE_HALFWAY_VECTOR"></span>Le vecteur à mi-chemin


Le vecteur à mi-chemin (H) se situe à mi-chemin entre deux vecteurs: le vecteur qui relie un vertex d’objet à la source de lumière et le vecteur qui relie un vertex d’objet à la position de l'appareil photo. Direct3D propose deux méthodes de calcul du vecteur à mi-chemin. Lorsque les reflets liés à l'appareil photo sont activés (plutôt que les reflets spéculaires orthogonaux), le système calcule le vecteur à mi-chemin à l'aide de la position de l'appareil photo et de la position du vertex, ainsi que le vecteur de direction de la lumière. La formule suivante permet d'illustrer cela:

|                                           |
|-------------------------------------------|
| H = norm(norm(Cₚ - Vₚ) + L<sub>dir</sub>) |

 

| Paramètre       | Valeur par défaut | Type                                          | Description                                                  |
|-----------------|---------------|-----------------------------------------------|--------------------------------------------------------------|
| Cₚ              | Non applicable           | Vecteur 3D (valeurs à virgule flottante x, y et z) | Position de l'appareil photo.                                             |
| Vₚ              | Non applicable           | Vecteur 3D (valeurs à virgule flottante x, y et z) | Position du vertex.                                             |
| L<sub>dir</sub> | Non applicable           | Vecteur 3D (valeurs à virgule flottante x, y et z) | Vecteur de direction entre la position du vertex et la position de la lumière. |

 

Le fait de déterminer le vecteur à mi-chemin de cette manière peut s'avérer extrêmement gourmand en calcul. Vous avez également la possibilité d'utiliser des reflets spéculaires orthogonaux (plutôt que des reflets spéculaires liés à l'appareil photo) pour indiquer au système d'agir comme si le point de vue était infiniment distant sur l'axe z. Cette définition est illustrée par la formule suivante :

|                                     |
|-------------------------------------|
| H = norm((0,0,1) + L<sub>dir</sub>) |

 

Ce paramètre est moins gourmand en calcul, mais beaucoup moins précis. Il est donc préférable de l'utiliser avec des applications qui utilisent la projection orthogonale.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


Dans cet exemple, l’objet est colorié à l'aide de la couleur de la lumière spéculaire de la scène et une couleur spéculaire du matériau.

Selon l’équation, la couleur obtenue pour les vertex d’objet combine la couleur du matériau et la couleur de la lumière.

Les deux illustrations suivantes présentent la couleur du matériau spéculaire, qui est grise, et la couleur de la lumière spéculaire, qui est blanche.

![illustration d’une sphère grise](images/amb1.jpg)![illustration d’une sphère blanche](images/lightwhite.jpg)

Le reflet spéculaire qui en résulte est présenté sur l’illustration suivante.

![illustration du reflet spéculaire](images/lights.jpg)

La combinaison du reflet spéculaire et de l'éclairage ambiant et diffuse produit est présentée sur l'illustration suivante. Une fois que les trois types d'éclairage sont appliqués, cela ressemble plus clairement à un objet réaliste.

![illustration de la combinaison d'un reflet spéculaire, d'un éclairage ambiant et d'un éclairage diffus](images/lightads.jpg)

L'éclairage spéculaire est plus difficile à calculer que l'éclairage diffus. Il est généralement utilisé pour fournir des indices visuels sur le matériau de la surface. La taille et la couleur du reflet spéculaire varie en fonction du matériau de la surface.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Formules mathématiques d’éclairage](mathematics-of-lighting.md)

 

 




