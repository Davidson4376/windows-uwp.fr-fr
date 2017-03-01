---
title: "Textures opaques et alpha 1 bit"
description: "Le format BC1 est dédié aux structures opaques ou présentant une couleur transparente unique."
ms.assetid: 8C53ACDD-72ED-4307-B4F3-2FCF9A9F53EC
keywords:
- "Textures opaques et alpha 1 bit"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: f3718785527cd3d686fd81c3706f3f9284b96fba
ms.lasthandoff: 02/07/2017

---

# <a name="span-iddirect3dconceptsopaqueand1-bitalphatexturesspanopaque-and-1-bit-alpha-textures"></a><span id="direct3dconcepts.opaque_and_1-bit_alpha_textures"></span>Textures opaques et alpha 1 bit


Le format BC1 est dédié aux structures opaques ou présentant une couleur transparente unique.

Pour chaque bloc opaque ou alpha 1 bit, deux valeurs 16 bits (format RVB 5:6:5) et une image bitmap 4x4 présentant 2 bits par pixel sont stockées. Cela constitue un total de 64 bits pour 16 texels, soit 4 bits par texel. L’image bitmap de bloc présente 2 bits par texel, de couleur, à sélectionner parmi les 4 disponibles ; 2 d’entre elles sont stockées dans les données codées. Les 2 autres couleurs sont dérivées, par interpolation linéaire, de ces couleurs stockées. Cette disposition est représentée dans le schéma suivant.

![schéma de la disposition bitmap](images/colors1.png)

Pour distinguer le format alpha 1 bit du format opaque, il suffit de comparer les 2 valeurs de couleur 16 bits stockées dans le bloc. Elles sont traitées comme des entiers non signés. Si la première couleur est supérieure à la seconde, cela signifie que seuls des texels opaques sont définis. Ainsi, 4 couleurs sont utilisées pour représenter les texels. Le codage à 4 couleurs comporte 2 couleurs dérivées ; les 4 couleurs sont distribuées de manière égale dans l’espace colorimétrique RVB. Ce format est analogue au format RVB 5:6:5. Sinon, pour une transparence alpha 1 bit, 3 couleurs sont utilisées, tandis que la 4e sert à représenter les texels transparents.

Dans le codage à 3 couleurs, vous trouvez une couleur dérivée. Le 4e code à 2 bits sert à indiquer un texel transparent (informations alpha). Ce format est analogue au format RVBA 5:5:5:1, dans lequel le bit final est utilisé pour coder le masque alpha.

L’exemple de code suivant illustre l’algorithme utilisé pour la sélection du codage à 3 ou 4 couleurs :

```
if (color_0 > color_1) 
{
    // Four-color block: derive the other two colors. 
    
    // 00 = color_0, 01 = color_1, 10 = color_2, 11 = color_3
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block.
    color_2 = (2 * color_0 + color_1 + 1) / 3;
    color_3 = (color_0 + 2 * color_1 + 1) / 3;
}    
else
{ 
    // Three-color block: derive the other color.
    // 00 = color_0,  01 = color_1,  10 = color_2,  
    // 11 = transparent.
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block. 
    color_2 = (color_0 + color_1) / 2;    
    color_3 = transparent;    

}
```

Nous vous recommandons de définir les composants RVBA du pixel de transparence sur 0 avant de procéder à la fusion.

Les tableaux suivants représentent la disposition de mémoire pour le bloc de 8 octets. Nous supposons que le premier index correspond à la coordonnée y et que le second index correspond à la coordonnée x. Par exemple, Texel\[1\]\[2\] fait référence au pixel de la carte de texture de coordonnées (x,y) = (2,1).

Voici la disposition de mémoire pour le bloc à 8 octets (64 bits) :

| Adresse du mot | Mot de 16 bits    |
|--------------|----------------|
| 0            | Color\_0       |
| 1            | Color\_1       |
| 2            | Bitmap Word\_0 |
| 3            | Bitmap Word\_1 |

 

Color\_0 et Color\_1, les couleurs des 2 extrêmes, sont disposées comme suit :

| Bits        | Couleur                 |
|-------------|-----------------------|
| 4:0 (LSB\*) | Composant de couleur bleue  |
| 10:5        | Composant de couleur vert |
| 15:11       | Composant de couleur rouge   |

 

\*bit de poids faible

Bitmap Word\_0 est disposé comme suit :

| Bits          | Texel           |
|---------------|-----------------|
| 1:0 (LSB)     | Texel\[0\]\[0\] |
| 3:2           | Texel\[0\]\[1\] |
| 5:4           | Texel\[0\]\[2\] |
| 7:6           | Texel\[0\]\[3\] |
| 9:8           | Texel\[1\]\[0\] |
| 11:10         | Texel\[1\]\[1\] |
| 13:12         | Texel\[1\]\[2\] |
| 15:14 (MSB\*) | Texel\[1\]\[3\] |

 

\*bit de poids fort (MSB)

Bitmap Word\_1 est disposé comme suit :

| Bits        | Texel           |
|-------------|-----------------|
| 1:0 (LSB)   | Texel\[2\]\[0\] |
| 3:2         | Texel\[2\]\[1\] |
| 5:4         | Texel\[2\]\[2\] |
| 7:6         | Texel\[2\]\[3\] |
| 9:8         | Texel\[3\]\[0\] |
| 11:10       | Texel\[3\]\[1\] |
| 13:12       | Texel\[3\]\[2\] |
| 15:14 (MSB) | Texel\[3\]\[3\] |

 

## <a name="span-idexampleofopaquecolorencodingspanspan-idexampleofopaquecolorencodingspanspan-idexampleofopaquecolorencodingspanexample-of-opaque-color-encoding"></a><span id="Example_of_Opaque_Color_Encoding"></span><span id="example_of_opaque_color_encoding"></span><span id="EXAMPLE_OF_OPAQUE_COLOR_ENCODING"></span>Exemple de codage de couleur opaque


Dans cet exemple de codage opaque, supposons que les couleurs rouge et noire sont situées aux extrêmes. Le rouge correspond à color\_0, tandis que le noir correspond à color\_1. Quatre couleurs interpolées forment le dégradé uniformément distribué les séparant. Pour déterminer les valeurs de l’image bitmap 4x4, les calculs suivants sont appliqués :

```
00 ? color_0
01 ? color_1
10 ? 2/3 color_0 + 1/3 color_1
11 ? 1/3 color_0 + 2/3 color_1
```

L’image bitmap obtenu ressemble au schéma suivant.

![schéma de disposition de l’image bitmap développée](images/colors2.png)

Cela ressemble à la série de couleurs illustrée suivante.

**Remarque**   Dans une image, le pixel (0, 0) apparaît sur le coin supérieur gauche.

 

![illustration d’un dégradé codé opaque](images/redsquares.png)

## <a name="span-idexampleof1bitalphaencodingspanspan-idexampleof1bitalphaencodingspanspan-idexampleof1bitalphaencodingspanexample-of-1-bit-alpha-encoding"></a><span id="Example_of_1_Bit_Alpha_Encoding"></span><span id="example_of_1_bit_alpha_encoding"></span><span id="EXAMPLE_OF_1_BIT_ALPHA_ENCODING"></span>Exemple de codage alpha 1 bit


Ce format est sélectionné lorsque l’entier 16 bits non signé, color\_0, est inférieur à l’entier 16 bits non signé color\_1. Par exemple, ce format peut être utilisé pour représenter des feuilles sur un arbre, avec un ciel bleu pour fond. Certains texels peuvent être marqués comme transparents, tandis que 3 nuances de vert restent disponibles pour les feuilles. 2 couleurs fixent les extrêmes, la 3e est une couleur interpolée.

L’illustration suivante est un exemple de ce type d’image.

![illustration de codage alpha 1 bit](images/greenthing.png)

Quand l’image s’affiche en blanc, le texel est codé comme transparent. Les composants RVBA des texels transparents doivent être défini sur 0 avant la fusion.

Le codage bitmap de ces couleurs et la transparence sont définis au moyen des calculs suivants.

```
00 ? color_0
01 ? color_1
10 ? 1/2 color_0 + 1/2 color_1
11   ?   Transparent
```

L’image bitmap obtenu ressemble au schéma suivant.

![schéma de disposition de l’image bitmap développée](images/colors3.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de texture compressées](compressed-texture-resources.md)

 

 





