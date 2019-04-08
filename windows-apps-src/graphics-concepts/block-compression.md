---
title: Compression de blocs
description: La compression de blocs est une technique de compression de textures avec perte qui permet de réduire la taille de la texture et l'encombrement de la mémoire de manière à produire de meilleures performances. Une texture à blocs compressés peut être plus petite qu'une texture avec 32 bits par couleur.
ms.assetid: 2FAD6BE8-C6E4-4112-AF97-419CD27F7C73
keywords:
- Compression de blocs
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3f6a1277dbb2d756f0d3a4ffc1fd545f892a2096
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596504"
---
# <a name="block-compression"></a>Compression de blocs

La compression de blocs est une technique de compression de textures avec perte qui permet de réduire la taille de la texture et l'encombrement de la mémoire de manière à produire de meilleures performances. Une texture à blocs compressés peut être plus petite qu'une texture avec 32 bits par couleur.

La compression de blocs est une technique de compression de textures permettant de réduire la taille de la texture. Comparée à une texture avec 32 bits par couleur, la taille d'une texture à blocs compressés peut être réduite jusqu'à 75 %. Les applications bénéficient généralement de meilleures performances en utilisant la technique de la compression de blocs en raison du plus faible encombrement de la mémoire.

Bien qu'elle entraîne des pertes, la compression de blocs fonctionne correctement et est recommandée pour toutes les textures qui sont transformées et filtrées par le pipeline. Les textures qui sont directement mappées à l’écran (éléments d’interface utilisateur tels que les icônes et le texte) ne sont pas adaptées à une compression, car les artéfacts sont plus nets.

Une texture à blocs compressés doit être créée en tant que multiple de 4 dans toutes les dimensions et ne peut pas être utilisée en tant que sortie du pipeline.

## <a name="span-idbasicsspanspan-idbasicsspanspan-idbasicsspanhow-block-compression-works"></a><span id="Basics"></span><span id="basics"></span><span id="BASICS"></span>Comment bloquer la compression

La compression de blocs est une technique permettant de réduire la quantité de mémoire requise pour stocker les données de couleur. En stockant certaines couleurs dans leur taille d’origine et d'autres couleurs à l’aide d’un schéma d’encodage, vous pouvez considérablement réduire la quantité de mémoire nécessaire au stockage de l’image. Dans la mesure où le matériel décode automatiquement les données compressées, l’utilisation de textures compressées n'entraîne aucune perte de performances.

Pour connaître le principe de fonctionnement de la compression, examinez les deux exemples suivants. Le premier exemple décrit la quantité de mémoire utilisée pour le stockage de données non compressées ; le deuxième exemple décrit la quantité de mémoire utilisée pour le stockage des données compressées.

- [Stockage des données non compressées](#storing-uncompressed-data)
- [Stockage de données compressées](#storing-compressed-data)

### <a name="span-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoring-uncompressed-dataspanstoring-uncompressed-data"></a><span id="Storing_Uncompressed_Data"></span><span id="storing_uncompressed_data"></span><span id="STORING_UNCOMPRESSED_DATA"></span><span id="storing-uncompressed-data"></span>Stockage des données non compressées

L’illustration suivante représente une texture 4x4 non compressée. Partons du principe que chaque couleur contient un composant de couleur unique (rouge, par exemple) et est stockée dans un seul octet de mémoire.

![une texture 4x4 non compressée](images/d3d10-block-compress-1.png)

Les données non compressées sont disposées dans la mémoire de manière séquentielle et requièrent 16 octets, comme décrit dans l’illustration suivante.

![données non compressées dans la mémoire séquentielle](images/d3d10-block-compress-2.png)

### <a name="span-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoring-compressed-dataspanstoring-compressed-data"></a><span id="Storing_Compressed_Data"></span><span id="storing_compressed_data"></span><span id="STORING_COMPRESSED_DATA"></span><span id="storing-compressed-data"></span>Stockage de données compressées

Maintenant que vous connaissez la quantité de mémoire utilisée par une image non compressée, examinez la quantité de mémoire que l'on peut économiser avec une image compressée. Le format de compression [BC1](#bc1) stocke 2 couleurs (1 octet chacune) et 16 index de 3 bits (48 bits ou 6 octets) qui sont utilisés pour interpoler les couleurs d’origine de la texture, comme indiqué dans l’illustration suivante.

![le format de compression bc1](images/d3d10-block-compress-3.png)

L’espace total requis pour stocker les données compressées est de 8 octets, soit un gain de 50 % de mémoire par rapport à l’exemple de données non compressées. Le gain est encore plus significatif lorsque plusieurs composants de couleur sont utilisés.

Les économies de mémoire importantes garanties par la compression de blocs peuvent entraîner une augmentation des performances. Ces performances se font au détriment de la qualité de l’image (en raison de l’interpolation des couleurs) ; cependant, la perte de qualité est rarement perceptible.

La section suivante montre comment Direct3D prend en charge l'utilisation de la compression de blocs dans une application.

## <a name="span-idusingblockcompressionspanspan-idusingblockcompressionspanspan-idusingblockcompressionspanusing-block-compression"></a><span id="Using_Block_Compression"></span><span id="using_block_compression"></span><span id="USING_BLOCK_COMPRESSION"></span>À l’aide de la compression de bloc

Créez une texture à blocs compressés exactement comme vous le feriez pour une texture non compressée, mais en spécifiant un format de blocs compressés.

Créez ensuite un affichage pour lier la texture au pipeline. Comme une texture à blocs compressés peut être utilisée uniquement en tant qu’entrée au niveau du nuanceur, vous devez créer un affichage de ressources de nuanceur.

Utilisez une texture à bloc compressés de la même manière que vous utiliseriez une texture non compressée. Si votre application obtient un pointeur de mémoire vers des données à blocs compressés, vous devez tenir compte du remplissage de la mémoire dans un mipmap qui entraîne une différence entre la taille déclarée et la taille réelle.

- [Taille virtuelle par rapport à la taille physique](#virtual-size-versus-physical-size)

### <a name="span-idvirtualsizespanspan-idvirtualsizespanspan-idvirtualsizespanspan-idvirtual-size-versus-physical-sizespanvirtual-size-versus-physical-size"></a><span id="Virtual_Size"></span><span id="virtual_size"></span><span id="VIRTUAL_SIZE"></span><span id="virtual-size-versus-physical-size"></span>Taille virtuelle par rapport à la taille physique

Si votre code d’application utilise un pointeur de mémoire pour parcourir la mémoire d’une texture à blocs compressés, vous devrez peut-être apporter une modification dans le code de votre application. Une texture à blocs compressés doit être un multiple de 4 dans toutes les dimensions, car les algorithmes de compression de blocs fonctionnent sur des blocs de texels 4x4. Cela pose problème dans le cas d'un mipmap dont les dimensions d’origine sont divisibles par 4, mais pas les niveaux subdivisés. Le schéma suivant montre, dans une zone, la différence entre la taille virtuelle (déclarée) et la taille physique (réelle) de chaque niveau de mipmap.

![niveaux de mipmap compressés et non compressées](images/d3d10-block-compress-pad.png)

Le côté gauche du schéma illustre les tailles de niveau mipmap qui sont générées pour une texture 60×40 non compressée. La taille de niveau supérieur est obtenue à partir de l’appel d’API qui génère la texture ; chaque niveau suivant correspond à la moitié de la taille du niveau précédent. Pour une texture non compressée, il n’existe aucune différence entre la taille virtuelle (déclarée) et la taille physique (réelle).

Le côté droit du schéma illustre les tailles de niveau mipmap qui sont générées pour la même texture 60×40 avec une compression. Notez que les deuxième et troisième niveaux comportent un remplissage de mémoire pour produire les facteurs de tailles de 4 sur chaque niveau. Cela permet aux algorithmes de fonctionner sur des blocs de 4×4 texels. Cet aspect est tout particulièrement évident si vous envisagez des niveaux de mipmap inférieurs à 4x4 ; la taille de ces très petits niveaux de mipmap sera arrondie au facteur de 4 le plus proche lors de l'allocation de la mémoire de texture.

Le matériel d'échantillonnage utilise la taille virtuelle ; lorsque la texture est échantillonnée, le remplissage de la mémoire est ignoré. Pour les niveaux de mipmap inférieurs à 4x4, seuls les quatre premiers texels seront utilisés pour une carte de 2×2, et seul le premier texel sera utilisé par un bloc de 1x1. Il n’existe cependant aucune structure d’API qui expose la taille physique (y compris le remplissage de la mémoire).

En résumé, veillez à utiliser des blocs de mémoire alignés lors de la copie des régions contenant des données à blocs compressés. Pour procéder ainsi dans une application qui obtient un pointeur de mémoire, assurez-vous que le pointeur utilise l'inclinaison de la surface pour prendre en compte la taille de la mémoire physique.

## <a name="span-idcompressionalgorithmsspanspan-idcompressionalgorithmsspanspan-idcompressionalgorithmsspancompression-algorithms"></a><span id="Compression_Algorithms"></span><span id="compression_algorithms"></span><span id="COMPRESSION_ALGORITHMS"></span>Algorithmes de compression

Les techniques de compression de blocs dans Direct3D divisent les données de texture non compressées en blocs 4×4, compressent chaque bloc, puis stockent les données. Pour cette raison, les textures supposées être compressées doivent avoir des dimensions de texture correspondant à des multiples de 4.

![compression de blocs](images/d3d10-compression-1.png)

Le schéma précédent présente une texture partitionnée en blocs de texels. Le premier bloc montre la disposition des 16 texels libellés de a à p, mais chaque bloc présente la même organisation de données.

Direct3D implémente plusieurs schémas de compression, chacun appliquant un compromis différent entre le nombre de composants stockés, le nombre de bits par composant et la quantité de mémoire consommée. Utilisez ce tableau pour vous aider à choisir le format le plus adapté à votre type de données et pour sélectionner la résolution de données qui convient à votre application.

| Données source                     | Résolution de la compression de données (en bits) | Choisissez ce format de compression |
|---------------------------------|---------------------------------------|--------------------------------|
| Couleur et alpha trois composants | Couleur (5:6:5), Alpha (1) ou sans alpha  | [BC1](#bc1)                    |
| Couleur et alpha trois composants | Couleur (5:6:5), Alpha (4)              | [BC2](#bc2)                    |
| Couleur et alpha trois composants | Couleur (5:6:5), Alpha (8)              | [BC3](#bc3)                    |
| Couleur un composant             | Un composant (8)                     | [TEXTURES BC4](#bc4)                    |
| Couleur deux composants             | Deux composants (8:8)                  | [BC5](#bc5)                    |

- [BC1](#bc1)
- [BC2](#bc2)
- [BC3](#bc3)
- [TEXTURES BC4](#bc4)
- [BC5](#bc5)

### <a name="span-idbc1spanspan-idbc1spanbc1"></a><span id="BC1"></span><span id="bc1"></span>BC1

Utilisez le format de compression de bloc (BC1) premier (soit DXGI\_FORMAT\_BC1\_TYPELESS, DXGI\_FORMAT\_BC1\_UNORM ou DXGI\_BC1\_UNORM \_SRVB) pour stocker les données de couleur de trois composants à l’aide d’un 5 : couleur de 6:5 (5 bits rouge, 6 bits vert, bleu de 5 bits). Ceci se vérifie même si les données contiennent également un composant alpha 1 bit. En supposant une texture 4x4 utilisant le format de données le plus volumineux possible, le format BC1 réduit la quantité de mémoire requise de 48 octets (16 couleurs × 3 composants/couleur × 1 octet/composant) à 8 octets de mémoire.

L’algorithme fonctionne sur les blocs de texels 4×4. Au lieu de stocker les 16 couleurs, l’algorithme enregistre les couleurs de référence 2 (couleur\_0 et la couleur\_1) et de 16 indices de couleur de 2 bits (blocs a – p), comme indiqué dans le diagramme suivant.

![disposition pour la compression bc1](images/d3d10-compression-bc1.png)

Les index de couleur (a à p) sont utilisés pour rechercher les couleurs d’origine à partir d’une table de couleurs. La table des couleurs contient 4 couleurs. Les deux premières couleurs : couleurs\_0 et la couleur\_1 — sont les couleurs minimales et maximales. Les deux autres couleurs, couleur\_2 et la couleur\_3, sont des couleurs intermédiaires calculés avec une interpolation linéaire.

```cpp
color_2 = 2/3*color_0 + 1/3*color_1
color_3 = 1/3*color_0 + 2/3*color_1
```

Les quatre couleurs sont affectées à des valeurs d’index 2 bits qui seront enregistrées dans les blocs a à p.

```cpp
color_0 = 00
color_1 = 01
color_2 = 10
color_3 = 11
```

Enfin, chacune des couleurs contenues dans les blocs a à p est comparée aux quatre couleurs de la table des couleurs, et l’index de la couleur la plus proche est stocké dans les blocs de 2 bits.

Cet algorithme se prête à des données qui contiennent également des composants alpha 1 bit. La seule différence est que couleur\_3 est défini sur 0 (ce qui représente une couleur transparente) et la couleur\_2 est une combinaison linéaire de couleur\_0 et la couleur\_1.

```cpp
color_2 = 1/2*color_0 + 1/2*color_1;
color_3 = 0;
```

### <a name="span-idbc2spanspan-idbc2spanbc2"></a><span id="BC2"></span><span id="bc2"></span>BC2

Utilisez le format BC2 (soit DXGI\_FORMAT\_BC2\_TYPELESS, DXGI\_FORMAT\_BC2\_UNORM ou DXGI\_BC2\_UNORM\_sRVB) à stocker des données qui contiennent des données de couleur et alpha avec cohérence faible (utilisez [BC3](#bc3) pour les données alphabétiques-cohérent). Le format BC2 stocke les données RVB en tant que couleur 5:6:5 (5 bits rouges, 6 bits verts, 5 bits bleus) et les données alpha sous la forme d'une valeur de 4 bits distincte. En supposant une texture 4x4 utilisant le format de données le plus volumineux possible, cette technique de compression réduit la mémoire requise de 64 octets (16 couleurs × 4 composants/couleur × 1 octet/composant) à 16 octets de mémoire.

Le format BC2 stocke les couleurs avec le même nombre de bits et la même disposition des données que le format [BC1](#bc1) ; toutefois, le format BC2 requiert 64 bits supplémentaires de mémoire pour stocker les données alpha, comme indiqué dans le schéma suivant.

![disposition pour la compression bc2](images/d3d10-compression-bc2.png)

### <a name="span-idbc3spanspan-idbc3spanbc3"></a><span id="BC3"></span><span id="bc3"></span>BC3

Utilisez le format BC3 (soit DXGI\_FORMAT\_BC3\_TYPELESS, DXGI\_FORMAT\_BC3\_UNORM ou DXGI\_BC3\_UNORM\_sRVB) à stocker des données de couleur hautement cohérente (utilisez [BC2](#bc2) avec données alpha moins cohérentes). Le format BC3 stocke les données de couleur à l’aide d'une couleur 5:6:5 (5 bits rouges, 6 bits verts, 5 bits bleus) et les données alpha à l’aide d’un seul octet. En supposant une texture 4x4 utilisant le format de données le plus volumineux possible, cette technique de compression réduit la mémoire requise de 64 octets (16 couleurs × 4 composants/couleur × 1 octet/composant) à 16 octets de mémoire.

Le format BC3 stocke les couleurs avec le même nombre de bits et la même disposition de données que le format [BC1](#bc1) ; toutefois, le format BC3 nécessite 64 bits supplémentaires de mémoire pour le stockage des données alpha. Le format BC3 gère les données alpha en stockant deux valeurs de référence et en les interpolant entre elles (de la même manière que le format BC1 stocke la couleur RVB).

L’algorithme fonctionne sur les blocs de texels 4×4. Au lieu de stocker des valeurs alpha 16, l’algorithme stocke 2 prémultipliés référence (alpha\_alpha et 0\_1) et de 16 indices de couleur de 3 bits (alpha un p via), comme indiqué dans le diagramme suivant.

![disposition pour la compression bc3](images/d3d10-compression-bc3.png)

Le format BC3 utilise les index alpha (a à p) pour rechercher les couleurs d’origine à partir d’une table de recherche contenant 8 valeurs. Les deux premières valeurs — alpha\_alpha et 0\_1 — sont les valeurs minimales et maximales ; les autres six valeurs intermédiaires sont calculées en utilisant une interpolation linéaire.

L’algorithme détermine le nombre de valeurs alpha interpolées en examinant les deux valeurs alpha de référence. Si alpha\_0 est supérieure à alpha\_1, alors que BC3 interpole 6 valeurs alphabétiques ; sinon, il effectue une interpolation 4. Lorsque BC3 interpole seulement 4 valeurs alpha, il définit deux valeurs alpha supplémentaires (0 pour une couleur entièrement transparente et 255 pour une couleur totalement opaque). Le format BC3 compresse les valeurs alpha dans la zone de texel 4x4 en stockant le code de bit correspondant aux valeurs alpha interpolées qui correspondent le mieux à la valeur alpha d’origine pour un texel donné.

```cpp
if( alpha_0 > alpha_1 )
{
  // 6 interpolated alpha values.
  alpha_2 = 6/7*alpha_0 + 1/7*alpha_1; // bit code 010
  alpha_3 = 5/7*alpha_0 + 2/7*alpha_1; // bit code 011
  alpha_4 = 4/7*alpha_0 + 3/7*alpha_1; // bit code 100
  alpha_5 = 3/7*alpha_0 + 4/7*alpha_1; // bit code 101
  alpha_6 = 2/7*alpha_0 + 5/7*alpha_1; // bit code 110
  alpha_7 = 1/7*alpha_0 + 6/7*alpha_1; // bit code 111
}
else
{
  // 4 interpolated alpha values.
  alpha_2 = 4/5*alpha_0 + 1/5*alpha_1; // bit code 010
  alpha_3 = 3/5*alpha_0 + 2/5*alpha_1; // bit code 011
  alpha_4 = 2/5*alpha_0 + 3/5*alpha_1; // bit code 100
  alpha_5 = 1/5*alpha_0 + 4/5*alpha_1; // bit code 101
  alpha_6 = 0;                         // bit code 110
  alpha_7 = 255;                       // bit code 111
}
```

### <a name="span-idbc4spanspan-idbc4spanbc4"></a><span id="BC4"></span><span id="bc4"></span>TEXTURES BC4

Utilisez le format BC4 pour stocker des données de couleur à un composant en utilisant 8 bits pour chaque couleur. Suite à la précision accrue (par rapport à [BC1](#bc1)), textures BC4 est idéale pour stocker des données à virgule flottante dans la plage de \[0 à 1\] à l’aide de la DXGI\_FORMAT\_textures BC4\_Format UNORM et \[-1 à + 1\] à l’aide de la DXGI\_FORMAT\_textures BC4\_format SNORM. En supposant une texture 4x4 utilisant le format de données le plus volumineux possible, cette technique de compression réduit la mémoire requise de 16 octets (16 couleurs × 4 composants/couleur × 1 octet/composant) à 8 octets.

L’algorithme fonctionne sur les blocs de texels 4×4. Au lieu de stocker les 16 couleurs, l’algorithme stocke 2 couleurs de référence (rouge\_0 et rouge\_1) et 16 indices de 3 bits couleur (rouge un via p rouge), comme illustré dans le diagramme suivant.

![disposition pour la compression bc4](images/d3d10-compression-bc4.png)

L’algorithme utilise les index 3 bits pour rechercher des couleurs à partir d’une table de couleurs contenant 8 couleurs. Les deux premières couleurs : rouge\_0 et rouge\_1 — sont les couleurs minimales et maximales. L’algorithme calcule les couleurs restantes à l’aide d’une interpolation linéaire.

L’algorithme détermine le nombre de valeurs de couleur interpolées en examinant les deux valeurs de référence. Si rouge\_0 est supérieure à red\_1, alors que les textures BC4 interpole 6 valeurs de couleur ; sinon, il effectue une interpolation 4. Lorsque BC4 interpole seulement 4 valeurs de couleur, il définit deux valeurs de couleurs supplémentaires (0.0f pour une couleur entièrement transparente et 1.0f pour une couleur totalement opaque). Le format BC4 compresse les valeurs alpha dans la zone de texel 4x4 en stockant le code de bit correspondant aux valeurs alpha interpolées qui correspondent le mieux à la valeur alpha d’origine pour un texel donné.

- [TEXTURES BC4\_UNORM](#bc4-unorm)
- [TEXTURES BC4\_SNORM](#bc4-snorm)

### <a name="span-idbc4unormspanspan-idbc4unormspanspan-idbc4-unormspanbc4unorm"></a><span id="BC4_UNORM"></span><span id="bc4_unorm"></span><span id="bc4-unorm"></span>TEXTURES BC4\_UNORM

L’interpolation des données à simple composant s’effectue comme dans l’exemple de code suivant.

```cpp
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

Les couleurs de référence sont attribuées à des index de 3 bits (000 – 111 dans la mesure où il y a 8 valeurs) qui seront enregistrés dans les blocs rouge a à rouge p lors de la compression.

### <a name="span-idbc4snormspanspan-idbc4snormspanspan-idbc4-snormspanbc4snorm"></a><span id="BC4_SNORM"></span><span id="bc4_snorm"></span><span id="bc4-snorm"></span>TEXTURES BC4\_SNORM

Le DXGI\_FORMAT\_textures BC4\_SNORM est exactement identique, à ceci près que les données sont encodées dans la plage SNORM et lorsqu’il y a 4 valeurs de couleurs sont interpolées. L’interpolation des données à simple composant s’effectue comme dans l’exemple de code suivant.

```cpp
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

Les couleurs de référence sont attribuées à des index de 3 bits (000 – 111 dans la mesure où il y a 8 valeurs) qui seront enregistrés dans les blocs rouge a à rouge p lors de la compression.

### <a name="span-idbc5spanspan-idbc5spanbc5"></a><span id="BC5"></span><span id="bc5"></span>BC5

Utilisez le format BC5 pour stocker des données de couleur à deux composants en utilisant 8 bits pour chaque couleur. Suite à la précision accrue (par rapport à [BC1](#bc1)), BC5 est idéale pour stocker des données à virgule flottante dans la plage de \[0 à 1\] à l’aide de la DXGI\_FORMAT\_BC5\_Format UNORM et \[-1 à + 1\] à l’aide de la DXGI\_FORMAT\_BC5\_format SNORM. En supposant une texture 4x4 utilisant le format de données le plus volumineux possible, cette technique de compression réduit la mémoire requise de 32 octets (16 couleurs × 2 composants/couleur × 1 octet/composant) à 16 octets.

- [BC5\_UNORM](#bc5-unorm)
- [BC5\_SNORM](#bc5-snorm)

L’algorithme fonctionne sur les blocs de texels 4×4. Au lieu de stocker les 16 couleurs pour les deux composants, l’algorithme stocke 2 couleurs de référence pour chaque composant (rouge\_0, rouge\_1, vert\_0 et vert\_1) et 16 couleurs 3 bits indices pour chaque composant (rouge un via p rouge et vert un p vert via), comme illustré dans le diagramme suivant.

![disposition pour la compression bc5](images/d3d10-compression-bc5.png)

L’algorithme utilise les index 3 bits pour rechercher des couleurs à partir d’une table de couleurs contenant 8 couleurs. Les deux premières couleurs : rouge\_0 et rouge\_1 (ou vert\_0 et vert\_1), sont les couleurs minimales et maximales. L’algorithme calcule les couleurs restantes à l’aide d’une interpolation linéaire.

L’algorithme détermine le nombre de valeurs de couleur interpolées en examinant les deux valeurs de référence. Si rouge\_0 est supérieure à red\_1, BC5, puis effectue une interpolation 6 valeurs de couleur ; sinon, il effectue une interpolation 4. Lorsque BC5 interpole seulement 4 valeurs de couleur, il définit les deux valeurs de couleur restantes sur 0.0f et 1.0f.

### <a name="span-idbc5unormspanspan-idbc5unormspanspan-idbc5-unormspanbc5unorm"></a><span id="BC5_UNORM"></span><span id="bc5_unorm"></span><span id="bc5-unorm"></span>BC5\_UNORM

L’interpolation des données à simple composant s’effectue comme dans l’exemple de code suivant. Les calculs pour les composants verts sont similaires.

```cpp
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

Les couleurs de référence sont attribuées à des index de 3 bits (000 – 111 dans la mesure où il y a 8 valeurs) qui seront enregistrés dans les blocs rouge a à rouge p lors de la compression.

### <a name="span-idbc5snormspanspan-idbc5snormspanspan-idbc5-snormspanbc5snorm"></a><span id="BC5_SNORM"></span><span id="bc5_snorm"></span><span id="bc5-snorm"></span>BC5\_SNORM

Le DXGI\_FORMAT\_BC5\_SNORM est exactement le même, à ceci près que les données sont encodées dans la plage SNORM et lors de l’interpolation des valeurs de 4 données, les deux valeurs supplémentaires sont - 1.0f et 1.0f. L’interpolation des données à simple composant s’effectue comme dans l’exemple de code suivant. Les calculs pour les composants verts sont similaires.

```cpp
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

Les couleurs de référence sont attribuées à des index de 3 bits (000 – 111 dans la mesure où il y a 8 valeurs) qui seront enregistrés dans les blocs rouge a à rouge p lors de la compression.

## <a name="span-iddifferencesspanspan-iddifferencesspanspan-iddifferencesspanformat-conversion"></a><span id="Differences"></span><span id="differences"></span><span id="DIFFERENCES"></span>Conversion de format

Direct3D permet d'effectuer des copies entre des textures de type préstructuré et des textures à blocs compressés ayant la même largeur de bits.

Vous pouvez copier des ressources entre plusieurs types de format. Ce type d’opération de copie effectue un type de conversion de format qui réinterprète les données de ressources en tant que type de format différent. Observez l’exemple suivant qui montre la différence entre la réinterprétation des données et un type de comportement de conversion plus courant :

```cpp
FLOAT32 f = 1.0f;
UINT32 u;
```

Pour réinterpréter « f » comme type de « u », utilisez [memcpy](https://msdn.microsoft.com/library/dswaw1wk.aspx) :

```cpp
memcpy( &u, &f, sizeof( f ) ); // 'u' becomes equal to 0x3F800000.
```

Dans la réinterprétation précédente, la valeur sous-jacente des données ne change pas ; [memcpy](https://msdn.microsoft.com/library/dswaw1wk.aspx) réinterprète la virgule flottante en tant qu’entier non signé.

Pour effectuer le type de conversion le plus courant, utilisez l’attribution suivante :

```cpp
u = f; // 'u' becomes 1.
```

Dans la conversion précédente, la valeur sous-jacente des données ne change pas.

Le tableau suivant répertorie les formats source et de destination autorisés que vous pouvez utiliser dans ce type de réinterprétation de conversion de format. Vous devez encoder les valeurs correctement pour que la réinterprétation fonctionne comme prévu.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Largeur de bits</th>
<th align="left">Ressource non compressée</th>
<th align="left">Ressource à blocs compressés</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">32</td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p>
<p>DXGI_FORMAT_R32_SINT</p></td>
<td align="left">DXGI_FORMAT_R9G9B9E5_SHAREDEXP</td>
</tr>
<tr class="even">
<td align="left">64</td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UINT</p>
<p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<p>DXGI_FORMAT_R32G32_UINT</p>
<p>DXGI_FORMAT_R32G32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC4_UNORM</p>
<p>DXGI_FORMAT_BC4_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left">128</td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_UINT</p>
<p>DXGI_FORMAT_R32G32B32A32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC3_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC5_UNORM</p>
<p>DXGI_FORMAT_BC5_SNORM</p></td>
</tr>
</tbody>
</table>

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes

[Ressources de texture compressé](compressed-texture-resources.md)
