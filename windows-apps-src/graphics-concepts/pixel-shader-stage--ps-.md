---
title: Étape du nuanceur de pixels (PS)
description: L’étape du nuanceur de pixels (PS, Pixel Shader) reçoit les données interpolées pour une primitive et génère des données par pixel telles que la couleur.
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords:
- Étape du nuanceur de pixels (PS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e1f7e787f2ee80a3168d38a9afd9a249dc0e6de0
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8329169"
---
# <a name="pixel-shader-ps-stage"></a>Étape du nuanceur de pixels (PS)


L’étape du nuanceur de pixels (PS, Pixel Shader) reçoit les données interpolées pour une primitive et génère des données par pixel telles que la couleur.

Il s’agit d’une étape de nuanceur programmable. Elle figure sous forme d’un bloc arrondi dans le diagramme du [pipeline graphique](graphics-pipeline.md). Cette étape du nuanceur expose sa propre fonctionnalité unique, qui repose sur le modèle de nuanceur4.0 (voir l’article [Noyau de nuanceur commun](https://msdn.microsoft.com/library/windows/desktop/bb509580)).

L’étape du nuanceur de pixels permet d’appliquer des techniques d’ombrage enrichies telles que l’éclairage par pixel et le post-traitement. Le nuanceur de pixels est un programme qui combine des constantes, des données de texture, des valeurs interpolées par sommet et d’autres données pour produire des sorties par pixel. L’[étape du rastériseur (RS)](rasterizer-stage--rs-.md) appelle un nuanceur de pixels une fois pour tous les pixels dans une primitive. Toutefois, il est possible de spécifier un nuanceur **NULL** lorsque l’on ne souhaite pas exécuter de nuanceur.

Lors de l’échantillonnage multiple d’une texture, un nuanceur de pixels est appelé une fois par pixel inclus, et un test de profondeur/gabarit est exécuté pour chaque échantillon multiple. Les échantillons qui réussissent le test de profondeur/gabarit se voient appliquer la couleur de sortie du nuanceur de pixels.

Les fonctions du nuanceur de pixels produisent ou utilisent des quantités dérivées respectant l’espace x et y de l’écran. Les dérivés sont généralement utilisés pour calculer les niveaux de détail pour l’échantillonnage de texture. Dans le cas d’un filtrage anisotropique, les dérivés permettent de sélectionner des échantillons dans l’axe d’anisotropie. En règle générale, le matériel exécute un nuanceur de pixels sur plusieurs pixels (par exemple dans une grille de 2x2) simultanément. Par conséquent, les dérivés de quantités calculés dans le nuanceur de pixels peuvent faire raisonnablement l’objet d’une approximation à l’aide des deltas des valeurs au même point d’exécution dans les pixels adjacents.

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>Entrées


Lorsque le pipeline est configuré sans nuanceur de géométrie, un nuanceur de pixels est limité à 16entrées 32bits de 4composants. Dans le cas contraire, un nuanceur de pixels peut prendre en charge jusqu’à 32entrées 32bits de 4composants.

Les données d’entrée du nuanceur de pixels comprennent les attributs de vertex (qui peuvent être interpolés avec ou sans correction de la perspective) ou peuvent être traitées en tant que constantes par primitive. Les entrées du nuanceur de pixels sont interpolées à partir des attributs de vertex de la primitive faisant l’objet de la rastérisation, en fonction du mode d’interpolation déclaré. Si une primitive est découpée avant la rastérisation, le mode d’interpolation est également respecté pendant le processus de découpage.

Les attributs des sommets sont interpolés (ou évalués) aux emplacements centraux du nuanceur de pixels. Les modes d’interpolation d’attributs du nuanceur de pixels sont déclarés dans une déclaration d’enregistrement d’entrée pour chaque élément, soit dans un [argument](https://msdn.microsoft.com/library/windows/desktop/bb509606), soit dans une [structure d’entrée](https://msdn.microsoft.com/library/windows/desktop/bb509668). Les attributs peuvent être interpolés de façon linéaire, ou avec l’échantillonnage par centroïde. Consultez la section «Échantillonnage par centroïde des attributs lors de l’anticrénelage à échantillonnage multiple» dans les [règles de rastérisation](rasterization-rules.md). L’évaluation du centroïde est pertinente uniquement lors de l’échantillonnage multiple pour prendre en compte les cas dans lesquels un pixel est inclus dans une primitive, mais pas forcément un centre de pixel. L’évaluation du centroïde est effectuée à un emplacement aussi proche que possible du centre de pixel (non inclus).

Les entrées peuvent également être déclarées avec une [sémantique de valeurs système](https://msdn.microsoft.com/library/windows/desktop/bb509647), qui marque un paramètre utilisé par d’autres étapes du pipeline. Par exemple, une position de pixel doit être marquée avec la sémantique SV\_Position. L’[étape de l’assembleur d’entrée (IA)](input-assembler-stage--ia-.md) peut produire un scalaire pour un nuanceur de pixels (avec SV\_PrimitiveID). L’[étape du rastériseur (RS)](rasterizer-stage--rs-.md) peut également générer un scalaire pour un nuanceur de pixels (avec SV\_IsFrontFace).

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>Sorties


Un nuanceur de pixels peut sortir jusqu’à 8couleurs 32bits de 4composants, ou aucune couleur si le pixel est ignoré. Les composants du registre de sortie du nuanceur de pixels doivent être déclarés avant d’être utilisés; un masque d’écriture de sortie distinct est autorisé pour chaque registre.

Utilisez l’étape d’activation de l’écriture de profondeur (à l’[étape de fusion de sortie (OM)](output-merger-stage--om-.md)) pour contrôler les données de profondeur écrites sur un tampon de profondeur (ou utilisez l’instruction discard pour ignorer les données pour le pixel concerné). Un nuanceur de pixels peut également sortir une valeur de profondeur facultative à virgule flottante (de 32bits et à 1composant) pour le test de profondeur (avec la sémantique SV\_Depth). La valeur de profondeur de sortie est placée dans le registre oDepth et remplace la valeur de profondeur interpolée pour le test de profondeur (en supposant que le test de profondeur est activé). Il n’existe aucun moyen de modifier dynamiquement la fonction utilisée (profondeur à fonction fixe ou oDepth de nuanceur).

Un nuanceur de pixels ne peut pas générer une valeur de gabarit en sortie.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 




