---
title: Textures avec canaux alpha
description: Il existe deux façons de coder les mappages de texture qui présentent une transparence plus complexe.
ms.assetid: 768A774A-4F21-4DDE-B863-14211DA92926
keywords:
- Textures avec canaux alpha
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 88d150383d2be219e7f382e0e690771acbc9d2ee
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8800540"
---
# <a name="textures-with-alpha-channels"></a>Textures avec canaux alpha


Il existe deux façons de coder les mappages de texture qui présentent une transparence plus complexe. Dans chaque cas, un bloc décrivant la transparence précède le bloc de 64bits déjà décrit. La transparence est représentée sous forme d’un bitmap 4x4 avec 4bits par pixel (codage explicite) ou avec moins de bits et une interpolation linéaire similaire à ce qui est utilisé pour le codage de couleurs.

Le bloc de transparence et le bloc de couleurs sont organisés comme indiqué dans le tableau suivant.

| Adresse du mot | Bloc de 64bits                      |
|--------------|-----------------------------------|
| 3:0          | Bloc de transparence                |
| 7:4          | Bloc de 64bits décrit précédemment |

 

## <a name="span-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanexplicit-texture-encoding"></a><span id="Explicit-Texture-Encoding"></span><span id="explicit-texture-encoding"></span><span id="EXPLICIT-TEXTURE-ENCODING"></span>Codage de texture explicite


Pour un codage de texture explicite (format BC2), les composants alpha des texels qui décrivent la transparence sont codés dans un bitmap 4x4 avec 4bits par texel. Ces quatre bits peuvent être obtenus par le biais de différents moyens, comme un tramage ou à l’aide des quatre bits les plus importants des données alpha. Ils sont toutefois générés et utilisés comme ils sont, sans aucune interpolation.

Le diagramme suivant illustre un bloc de transparence de 64bits.

![diagramme d’un bloc de transparence de 64bits](images/colors4.png)

**Remarque**  la méthode de compression de Direct3D utilise les quatre bits les plus importants.

 

Les tableaux suivants illustrent la façon dont les informations alpha sont disposées dans la mémoire, pour chaque mot de 16bits.

Disposition pour le mot 0:

| Bits          | Alpha      |
|---------------|------------|
| 3:0 (LSB\*)   | \[0\]\[0\] |
| 7:4           | \[0\]\[1\] |
| 11:8          | \[0\]\[2\] |
| 15:12 (MSB\*) | \[0\]\[3\] |

 

\*Bit le moins important, bit le plus important (MSB)

Disposition pour le mot 1:

| Bits        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[1\]\[0\] |
| 7:4         | \[1\]\[1\] |
| 11:8        | \[1\]\[2\] |
| 15:12 (MSB) | \[1\]\[3\] |

 

Disposition pour le mot 2:

| Bits        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[2\]\[0\] |
| 7:4         | \[2\]\[1\] |
| 11:8        | \[2\]\[2\] |
| 15:12 (MSB) | \[2\]\[3\] |

 

Disposition pour le mot 3:

| Bits        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[3\]\[0\] |
| 7:4         | \[3\]\[1\] |
| 11:8        | \[3\]\[2\] |
| 15:12 (MSB) | \[3\]\[3\] |

 

La comparaison de couleurs utilisée dans BC1 permet de déterminer si le texel transparent n’est pas utilisé dans ce format. Nous supposons que sans la comparaison de couleurs, les données de couleurs sont toujours traitées avec un mode à 4couleurs.

## <a name="span-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanthree-bit-linear-alpha-interpolation"></a><span id="Three-Bit-Linear-Alpha-Interpolation"></span><span id="three-bit-linear-alpha-interpolation"></span><span id="THREE-BIT-LINEAR-ALPHA-INTERPOLATION"></span>Interpolation alpha linéaire à trois bits


Le codage de transparence pour le format BC3 est basé sur un concept similaire au codage linéaire utilisé pour la couleur. Deux valeurs alpha 8bits et un bitmap 4x4 avec trois bits par pixel sont stockés dans les huit premiers octets du bloc. Des valeurs alpha représentatives sont utilisées pour interpoler les valeurs alpha intermédiaires. Des informations supplémentaires sur la manière dont les deux valeurs alpha sont stockées sont disponibles. Si alpha\_0 est supérieur à alpha\_1, alors six valeurs alpha intermédiaires sont créées par l’interpolation. Sinon, quatre valeurs alpha intermédiaires sont interpolées entre les extrêmes alpha spécifiés. Les deux autres valeurs alpha implicites sont 0 (complètement transparent) et 255 (complètement opaque).

L’exemple de code suivant illustre cet algorithme.

```
// 8-alpha or 6-alpha block?    
if (alpha_0 > alpha_1) {    
    // 8-alpha block:  derive the other six alphas.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (6 * alpha_0 + 1 * alpha_1 + 3) / 7;    // bit code 010
    alpha_3 = (5 * alpha_0 + 2 * alpha_1 + 3) / 7;    // bit code 011
    alpha_4 = (4 * alpha_0 + 3 * alpha_1 + 3) / 7;    // bit code 100
    alpha_5 = (3 * alpha_0 + 4 * alpha_1 + 3) / 7;    // bit code 101
    alpha_6 = (2 * alpha_0 + 5 * alpha_1 + 3) / 7;    // bit code 110
    alpha_7 = (1 * alpha_0 + 6 * alpha_1 + 3) / 7;    // bit code 111  
}    
else {  
    // 6-alpha block.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (4 * alpha_0 + 1 * alpha_1 + 2) / 5;    // Bit code 010
    alpha_3 = (3 * alpha_0 + 2 * alpha_1 + 2) / 5;    // Bit code 011
    alpha_4 = (2 * alpha_0 + 3 * alpha_1 + 2) / 5;    // Bit code 100
    alpha_5 = (1 * alpha_0 + 4 * alpha_1 + 2) / 5;    // Bit code 101
    alpha_6 = 0;                                      // Bit code 110
    alpha_7 = 255;                                    // Bit code 111
}
```

La disposition de la mémoire du bloc alpha est la suivante:

| Octet | Alpha                                                          |
|------|----------------------------------------------------------------|
| 0    | Alpha\_0                                                       |
| 1    | Alpha\_1                                                       |
| 2    | \[0\]\[2\] (2 MSBs), \[0\]\[1\], \[0\]\[0\]                    |
| 3    | \[1\]\[1\] (1 MSB), \[1\]\[0\], \[0\]\[3\], \[0\]\[2\] (1 LSB) |
| 4    | \[1\]\[3\], \[1\]\[2\], \[1\]\[1\] (2 LSBs)                    |
| 5    | \[2\]\[2\] (2 MSBs), \[2\]\[1\], \[2\]\[0\]                    |
| 6    | \[3\]\[1\] (1 MSB), \[3\]\[0\], \[2\]\[3\], \[2\]\[2\] (1 LSB) |
| 7    | \[3\]\[3\], \[3\]\[2\], \[3\]\[1\] (2 LSBs)                    |

 

La comparaison de couleurs utilisée dans BC1 permet de déterminer si le texel transparent n’est pas utilisé avec ces formats. Nous supposons que sans la comparaison de couleurs, les données de couleurs sont toujours traitées en tant que mode à 4couleurs.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Ressources de texture compressées](compressed-texture-resources.md)

 

 




