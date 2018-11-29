---
title: Vue d’ensemble de l’éclairage
description: Lorsque vous recourez à l’éclairageDirect3D, vous autorisez Direct3D à gérer les paramètres d’illumination à votre place. S’ils le souhaitent, les utilisateurs avancés peuvent ajuster eux-mêmes l’éclairage.
ms.assetid: FCBF6A92-4EAC-4CCC-A76C-79985AF348AE
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e90e460cf5f5bda7d90447440d76cf6898a83747
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7990412"
---
# <a name="lighting-overview"></a>Vue d’ensemble de l’éclairage

Lorsque vous recourez à l’éclairageDirect3D, vous autorisez Direct3D à gérer les paramètres d’illumination à votre place. S’ils le souhaitent, les utilisateurs avancés peuvent ajuster eux-mêmes l’éclairage.

Lorsque l’éclairage est activé, Direct3D calcule la couleur de chaque vertex d’objet en fonction de la combinaison des éléments suivants:

-   Les texels d’une carte de texture associée.
-   Les couleurs diffuses et spéculaires au niveau du vertex, si spécifiées.
-   La couleur et l’intensité de l’éclairage produit par des sources de lumière dans la scène ou le niveau d’éclairage ambiant de la scène.

Vos méthodes d’ajustement de l’éclairage et de gestion des matériaux affectent grandement l’aspect de la scène rendue. La réflexion de la lumière sur une surface dépend du matériau considéré. Les niveaux de lumières directe et ambiante définissent la quantité de lumière réfléchie. SI l’éclairage est activé, vous devez utiliser des matériaux pour effectuer le rendu d’une scène.

Les lumières ne sont pas requises pour le rendu d’une scène, mais les détails d’une scène rendue sans éclairage ne sont pas visibles. Dans le meilleur des cas, le rendu d’une scène non éclairée comporte la silhouette des objets dans la scène. Pour la plupart des applications, ce niveau de détail ne suffit pas.

## <a name="span-iddirectlightvsambientlightspanspan-iddirectlightvsambientlightspandirect-light-vs-ambient-light"></a><span id="direct_light_vs._ambient_light"></span><span id="DIRECT_LIGHT_VS._AMBIENT_LIGHT"></span>Lumière directe et lumière ambiante


Si les deux types de lumière (directe et ambiante) illuminent les objets d’une scène, ils sont indépendants l’un de l’autre. Ils produisent des effets différents, et nécessitent un traitement distinct.

La *lumière directe* frappe directement l’objet. La lumière directe présente toujours une direction et une couleur. Elle est un facteur pour les algorithmes d’ombrage, tels que l’ombrage Gouraud. Différents types d’éclairage émettent de la lumière directe de différentes façons; des effets d’atténuation spéciaux sont produits.

La *lumière ambiante* est distribuée de manière uniforme dans une scène. La lumière ambiante correspond à un niveau général d’éclairage réparti sur une scène, indépendamment des objets internes et des emplacements. La lumière ambiante, qui ne présente ni position ni direction, présente uniquement une couleur et une intensité. Chacun des types de lumière s’ajoute à la lumière ambiante globale d’une scène.

La couleur de la lumière ambiante est représentée par une valeur RVBA, pour laquelle chaque composant est une valeur entière comprise entre 0 et 255. La plupart des valeurs de couleur de Direct3D ne présentent pas ce paramétrage.

La combinaison des composants de lumière rouge, verte et bleue constitue la couleur finale de la lumière ambiante. Le composant alpha contrôle la transparence de la couleur. Lorsque vous recourez à l’accélération matérielle ou à l’émulation RVB, le composant alpha est ignoré.

## <a name="span-iddirect3dlightmodelvsnaturespanspan-iddirect3dlightmodelvsnaturespandirect3d-light-model-vs-nature"></a><span id="direct3d_light_model_vs._nature"></span><span id="DIRECT3D_LIGHT_MODEL_VS._NATURE"></span>Modèle d’éclairage Direct3D et nature


Dans la nature, lorsque de la lumière est émise d’une source, elle est réfléchie sur des milliers voire des millions d’objets avant de frapper l’œil humain. Chaque fois qu’elle est reflétée, une portion est absorbée par la surface de l’objet, une autre est distribuée aléatoirement dans diverses directions, tandis que le reste est dirigé vers une autre surface ou l’œil humain. Ce processus se poursuit jusqu’à élimination totale de la lumière ou perception de cette dernière par l’œil humain.

Cela va de soi, les calculs requis par la simulation du comportement naturel de la lumière sont trop chronophages pour les graphismes en temps réel Direct3D. Par conséquent, ceci dans un souci de rapidité d’exécution, le modèle d’éclairageDirect3D reproduit de manière approximative la lumière du monde réel. Direct3D décompose la lumière en composants rouge, vert et bleu constituant la couleur finale.

Dans Direct3D, lorsque la lumière est réfléchie sur une surface, la couleur interagit mathématiquement avec ladite surface. Ce contact génère la couleur affichée à l’écran. Pour des informations spécifiques sur les algorithmes utilisés par Direct3D, voir [Formules mathématiques d’éclairage](mathematics-of-lighting.md).

Le modèle d’éclairageDirect3D généralise la lumière en 2types distincts: la lumière ambiante et la lumière directe. Chacun possède des attributs différents et interagit de manière propre avec le matériau d’une surface. La lumière ambiante a été tellement distribuée qu’elle présente une direction et une source indéterminées: son niveau d’intensité est bas à tout endroit de la scène. L’éclairage indirect utilisé par les photographes est un bon exemple de lumière ambiante.

La lumière ambiante dans Direct3D, comme celle de la nature, ne présente ni direction ni source réelle. Uniquement une couleur et une intensité. En fait, le niveau de lumière ambiante est complètement indépendant des objets d’une scène générant de la lumière. La lumière ambiante n’affecte pas la réflexion spéculaire.

La lumière directe est la lumière générée par une source au sein d’une scène. Elle présente une couleur et une intensité, et est dirigée dans une direction spécifique. La lumière directe interagit avec le matériau d’une surface pour créer des surbrillances spéculaires. Sa direction est utilisée en tant que facteur pour les algorithmes d’ombrage, comme l’algorithme Gouraud. La réflexion de la lumière directe n’affecte en rien le niveau de lumière ambiante d’une scène. Les sources d’une scène générant de la lumière directe présentent différentes caractéristiques qui affectent la manière dont elles illuminent la scène.

Par ailleurs, le matériau d’un polygone présente des propriétés qui affectent la manière dont l’objet réfléchit la lumière qu’il reçoit. Vous définissez un facteur de réflexion unique qui définit la réflexion par le matériau de la lumière ambiante, et paramétrez les facteurs de réflexion des lumières diffuse et spéculaire du matériau.

## <a name="span-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspancolor-values-for-lights-and-materials"></a><span id="Color_Values_for_Lights_and_Materials"></span><span id="color_values_for_lights_and_materials"></span><span id="COLOR_VALUES_FOR_LIGHTS_AND_MATERIALS"></span>Valeurs de couleur pour les lumières et les matériaux


Direct3D décompose la couleur en quatrecomposants (rouge, vert, bleu et alpha) qui constituent une couleur finale. Chaque composant est défini sur une valeur comprise entre 0,0 et 1,0. Bien que les lumières et les matériaux utilisent la même structure pour décrire la couleur, les valeurs sont valorisées légèrement différemment.

Les valeurs de couleur des sources de lumière représentent la quantité émise d’un composant spécifique. Dans la mesure où les lumières n’utilisent aucun composant alpha, seuls les composants rouge, vert et bleu importent. Vous pouvez visualiser les troiscomposants sous la forme de filtres rouge, vert et bleu sur un écran de projection. Chacun des filtres peut être désactivé (une valeur de 0,0 dans l’élément inapproprié), présenter une brillance maximale (valeur de 1,0) ou se situer entre les deux.

Les couleurs provenant des filtres sont combinées pour produire la couleur finale de l’éclairage. Une combinaison comme R (1,0), V (1,0), B (1,0) génère une lumière blanche, tandis qu’une combinaison comme R (0,0), V (0,0), B (0,0) ne provoque aucune émission de lumière. Vous pouvez définir une lumière émettant uniquement un composant, afin d’obtenir un éclairage rouge, vert ou bleu pur. Sinon, l’éclairage peut valoriser des combinaisons générant du jaune ou du violet, par exemple. Il est même possible de définir un composant de couleur négatif afin de créer un éclairage sombre, qui a pour effet de supprimer la luminosité d’une scène. Sinon, si vous décidez de paramétrer les composants sur des valeurs supérieures à 1,0, vous créez un éclairage extrêmement puissant.

Avec des matériaux, en revanche, les valeurs de couleur indiquent la proportion d’un composant de couleur réfléchie par une surface affichée. Un matériau dont les composants de couleur sont R (1,0), V (1,0), B (1,0), A (1,0) réfléchit l’intégralité de la lumière qu’il reçoit. De la même manière, un matériau présentant les composants R (0,0), V (1,0), B (0,0), A (1,0) réfléchit l’intégralité de la lumière verte qu’il reçoit. Les matériaux possèdent plusieurs facteurs de réflexion, utilisés pour générer différents types d’effets.

Voir [Types de lumière](light-types.md) et [Propriétés de lumière](light-properties.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Lumières et matériaux](lights-and-materials.md)

 

 




