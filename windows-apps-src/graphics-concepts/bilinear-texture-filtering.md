---
title: Filtrage de textures bilinéaires
description: Le filtrage bilinéaire calcul la moyenne pondérée des 4texels les plus proches du point d'échantillonnage.
ms.assetid: 0851AD28-8246-4547-A663-47884DDDFC3E
keywords:
- Filtrage de textures bilinéaires
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 437650883b4782ca02c0daf24cc8ebed01d954f6
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8699493"
---
# <a name="bilinear-texture-filtering"></a>Filtrage de textures bilinéaires


Le *filtrage bilinéaire* calcule la moyenne pondérée des 4éléments de texture les plus proches du point d’échantillonnage. Cette approche de filtrage est plus précise et courante que le filtrage des points les plus proches. Cette approche est efficace car elle est implémentée dans le matériel graphique moderne.


## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


Les textures sont toujours adressées de façon linéaire de (0.0, 0.0) en haut à gauche à (1.0, 1.0) en bas à droite. L'adressage linéaire d’une texture est indiqué dans l’illustration suivante.

![illustration d'une texture 4x4 avec des blocs de couleur unis](images/bilinear-fig7a.png)

Les textures sont généralement représentées comme si elles étaient composées de blocs de couleur unis, mais il est en fait plus approprié de les envisager de la même façon qu'une image ligne par ligne: chaque texel est défini dans le centre exact d’une cellule de grille, comme indiqué dans l’illustration suivante.

![illustration d'une texture 4x4 avec des texels définis dans le centre des cellules de grille](images/bilinear-fig7b.png)

Si vous demandez à l’échantillonneur de texture la couleur de cette texture aux coordonnées UV (0.375, 0.375), vous obtiendrez un rouge uni (255, 0, 0). Cela s'explique par le fait que le centre de la cellule texel rouge correspond aux coordonnées UV (0.375, 0.375). Que se passe-t-il si vous demandez à l’échantillonneur la couleur de la texture aux coordonnées UV (0,25, 0,25)? Cela n’est pas si simple, car le point aux coordonnées UV (0,25, 0,25) se trouve dans l’angle exact des 4texels.

L'approche la plus simple consiste à demander à l’échantillonneur de retourner la couleur du texel le plus proche; c'est ce que l'on appelle le filtrage de points (voir [Échantillonnage des points les plus proches](nearest-point-sampling.md)), qui n’est généralement pas recommandé en raison des grains ou des blocs qu'il produit. Un échantillonnage de points de notre texture aux coordonnées UV (0,25, 0,25) révèle un autre problème subtil du filtrage des points les plus proches: on obtient quatre texels équidistants par rapport au point d’échantillonnage, ce qui signifie qu'il n'y a pas un seul texel le plus proche. L'un de ces quatre texels sera sélectionné comme couleur retournée, mais la sélection dépend de la façon dont la coordonnée est arrondie, ce qui peut entraîner des artéfacts (voir l’article Échantillonnage des points les plus proches dans le Kit de développement).

Une approche de filtrage plus précise et courante consiste à calculer la moyenne pondérée des 4texels les plus proches du point d’échantillonnage; c'est ce que l'on appelle le *filtrage bilinéaire*. Les coûts de calcul supplémentaires associés au filtrage bilinéaire sont généralement négligeables car cette routine est implémentée dans le matériel graphique moderne. Voici les couleurs que nous obtenons à quelques points d’échantillonnage différents à l’aide d'un filtrage bilinéaire:

```
UV: (0.5, 0.5)
```

Ce point se trouve à la frontière exacte entre les texels rouge, vert, bleu et blanc. L'échantillonneur retourne la couleur grise:

```
  0.25 * (255, 0, 0)
  0.25 * (0, 255, 0) 
  0.25 * (0, 0, 255) 
## + 0.25 * (255, 255, 255) 
------------------------
= (128, 128, 128)
```

```
UV: (0.5, 0.375)
```

Ce point se trouve au milieu de la frontière entre les texels rouge et vert. L’échantillonneur retourne la couleur jaune-gris (notez que les contributions des texels bleu et blanc sont mises à l’échelle de 0):

```
  0.5 * (255, 0, 0)
  0.5 * (0, 255, 0) 
  0.0 * (0, 0, 255) 
## + 0.0 * (255, 255, 255) 
------------------------
= (128, 128, 0)
```

```
UV: (0.375, 0.375)
```

Il s'agit de l’adresse du texel rouge, qui est la couleur retournée (tous les autres texels du calcul de filtrage sont pondérés 0):

```
  1.0 * (255, 0, 0)
  0.0 * (0, 255, 0) 
  0.0 * (0, 0, 255) 
## + 0.0 * (255, 255, 255) 
------------------------
= (255, 0, 0)
```

Comparez ces calculs à l’illustration suivante, qui indique ce que l'on obtient si le calcul de filtrage bilinéaire est effectué à chaque adresse de texture dans la texture 4x4.

![illustration d'une texture 4x4 avec un filtrage bilinéaire effectué à chaque adresse de texture](images/bilinear-fig7c.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Filtrage de textures](texture-filtering.md)

 

 




