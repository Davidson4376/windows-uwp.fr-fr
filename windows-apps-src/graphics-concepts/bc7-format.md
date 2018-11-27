---
title: Format BC7
description: Le format BC7 est un format de compression de texture utilisé pour compresser des données RVB et RVBA en haute qualité.
ms.assetid: 788B6E8C-9A1F-45F9-BE49-742285E8D8A6
keywords:
- Format BC7
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2c55a12dfa7757a48874b6857c95af592e818c2b
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7711930"
---
# <a name="bc7-format"></a>Format BC7


Le format BC7 est un format de compression de texture utilisé pour compresser des données RVB et RVBA en haute qualité.

Pour plus d’informations sur les modes bloc du format BC7, consultez [Référence des modes de format BC7](https://msdn.microsoft.com/library/windows/desktop/hh308954).

## <a name="span-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanabout-bc7dxgiformatbc7"></a><span id="About-BC7-DXGI-FORMAT-BC7"></span><span id="about-bc7-dxgi-format-bc7"></span><span id="ABOUT-BC7-DXGI-FORMAT-BC7"></span>À propos de BC7/DXGI\_FORMAT\_BC7


Le format BC7 est spécifié par les valeurs d’énumération DXGI\_FORMAT suivantes:

-   **DXGI\_FORMAT\_BC7\_TYPELESS**.
-   **DXGI\_FORMAT\_BC7\_UNORM**.
-   **DXGI\_FORMAT\_BC7\_UNORM\_SRGB**.

Le format BC7 peut être utilisé pour les ressources de texture [Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277) (y compris les tableaux), Texture3D ou TextureCube (y compris les tableaux). De même, ce format s’applique à toutes les surfaces de carte MIP associées à ces ressources.

BC7 utilise une taille de bloc fixe de 16octets (128bits) et une taille de vignette fixe de 4x4texels. Comme pour les formats BC précédents, les images de texture d'une taille supérieure à la taille de vignette prise en charge (4x4) sont compressées à l’aide de plusieurs blocs. Cette identité d’adressage s’applique également aux images 3D, aux cartes MIP, aux cartes de cube et aux tableaux de textures. Toutes les vignettes de l’image doivent être au même format.

Le format BC7 compresse les images de données à virgule fixe sur trois canaux (RVB) et quatre canaux (RVBA). En règle générale, la source de données est de 8bits par composant de couleur (canal), bien que le format soit capable d'encoder des données source avec un plus grand nombre de bits par composant de couleur. Toutes les vignettes de l’image doivent être au même format.

Le décodeur BC7 effectue la décompression avant d’appliquer le filtrage de textures.

Le matériel de décompression BC7 doit avoir une précision au bit près; autrement dit, le matériel doit retourner des résultats identiques à ceux renvoyés par le décodeur décrit dans ce document.

## <a name="span-idbc7-implementationspanspan-idbc7-implementationspanspan-idbc7-implementationspanbc7-implementation"></a><span id="BC7-Implementation"></span><span id="bc7-implementation"></span><span id="BC7-IMPLEMENTATION"></span>Implémentation BC7


Une implémentation BC7 peut spécifier un des 8modes, dans le bit de poids faible du bloc de 16octets (128bits). Le mode est encodé par zéro ou plusieurs bits avec une valeur de 0, suivie d’un 1.

Un bloc BC7 peut contenir plusieurs paires de points de terminaison. L’ensemble des index correspondant à une paire de points de terminaison peut être appelé «sous-ensemble». En outre, dans certains modes de bloc, la représentation sous forme de point de terminaison est encodée dans une forme appelée «RBVP», où le bit «P» représente un bit de poids faible partagé pour les composants de couleur du point de terminaison. Par exemple, si la représentation sous forme de point de terminaison pour le format est «RVB 5.5.5.1», le point de terminaison est interprété comme une valeur RVB 6.6.6, où l’état du bit P définit le bit de poids faible de chaque composant. De même, pour des données source avec un canal alpha, si la représentation du format est «RVBAP 5.5.5.5.1», le point de terminaison est interprété comme étant «RVBA 6.6.6.6». Selon le mode de bloc, vous pouvez spécifier le bit de poids faible soit partagé pour deux points de terminaison d’un sous-ensemble individuellement (2bitsP par sous-ensemble), soit partagé entre les points de terminaison d’un sous-ensemble (1bitP par sous-ensemble).

Pour les blocs BC7 qui n'encodent pas explicitement le composant alpha, un bloc BC7 se compose de bits de mode, de bits de partition, de points de terminaison compressés, d'index compressés et d'un bitP facultatif. Dans ces blocs, les points de terminaison ont une représentation RVB uniquement et le composant alpha est décodé en 1.0 pour tous les texels dans les données source.

Pour les blocs BC7 comprenant une combinaison de composants de couleur et de composants alpha, un bloc comprend des bits de mode, des points de terminaison compressés, des index compressés, des bits de partition facultatifs et des bitsP. Dans ces blocs, les couleurs de point de terminaison sont exprimées au format RVBA et les valeurs des composants alpha sont interpolées avec les valeurs des composants de couleur.

Les blocs BC7 ont des composants de couleur et des composants alpha distincts; un bloc se compose de bits de mode, de bits de rotation, de points de terminaison compressés, d'index compressés et d'un bit de sélecteur d’index facultatif. Ces blocs ont un vecteur RVB effectif \[R, V, B\] et un canal alpha scalaire \[A\] encodés séparément.

Le tableau suivant répertorie les composants de chaque type de bloc.

| Le bloc BC7 contient...     | bits de mode | bits de rotation | bit de sélecteur d’index | bits de partition | points de terminaison compressés | bitP    | index compressés |
|---------------------------|-----------|---------------|--------------------|----------------|----------------------|----------|--------------------|
| composants de couleur uniquement     | obligatoire  | Non applicable           | Non applicable                | obligatoire       | obligatoire             | facultatif | obligatoire           |
| couleur + alpha combinés    | obligatoire  | Non applicable           | Non applicable                | facultatif       | obligatoire             | facultatif | obligatoire           |
| couleur et alpha distincts | obligatoire  | obligatoire      | facultatif           | Non applicable            | obligatoire             | Non applicable      | obligatoire           |

 

Le format BC7 définit une palette de couleurs sur une ligne approximative entre deux points de terminaison. La valeur du mode détermine le nombre d’interpolation des paires de point de terminaison par bloc. BC7 stocke un index de palette par texel.

Pour chaque sous-catégorie d’index correspondant à une paire de points de terminaison, l’encodeur résout l’état d’un bit des données d'index compressées pour ce sous-ensemble. Pour ce faire, il choisit une commande de point de terminaison qui permet à l’index correspondant à l’index de «correction» désigné de définir son bit de poids fort sur 0, qui peut ensuite être ignoré de manière à économiser un bit par sous-ensemble. Pour les modes de bloc avec un seul sous-ensemble, l’index de correction est toujours l’index 0.

## <a name="span-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspandecoding-the-bc7-format"></a><span id="Decoding-the-BC7-Format"></span><span id="decoding-the-bc7-format"></span><span id="DECODING-THE-BC7-FORMAT"></span>Décodage du format BC7


Le pseudo-code ci-dessous décrit les étapes permettant de décompresser le pixel (x, y) étant donné le bloc BC7 de 16octets.

``` syntax
decompress_bc7(x, y, block)
{
    mode = extract_mode(block);
    
    //decode partition data from explicit partition bits
    subset_index = 0;
    num_subsets = 1;
    
    if (mode.type == 0 OR == 1 OR == 2 OR == 3 OR == 7)
    {
        num_subsets = get_num_subsets(mode.type);
        partition_set_id = extract_partition_set_id(mode, block);
        subset_index = get_partition_index(num_subsets, partition_set_id, x, y);
    }
    
    //extract raw, compressed endpoint bits
    UINT8 endpoint_array[num_subsets][4] = extract_endpoints(mode, block);
    
    //decode endpoint color and alpha for each subset
    fully_decode_endpoints(endpoint_array, mode, block);
    
    //endpoints are now complete.
    UINT8 endpoint_start[4] = endpoint_array[2 * subset_index];
    UINT8 endpoint_end[4]   = endpoint_array[2 * subset_index + 1];
        
    //Determine the palette index for this pixel
    alpha_index     = get_alpha_index(block, mode, x, y);
    alpha_bitcount  = get_alpha_bitcount(block, mode);
    color_index     = get_color_index(block, mode, x, y);
    color_bitcount  = get_color_bitcount(block, mode);

    //determine output
    UINT8 output[4];
    output.rgb = interpolate(endpoint_start.rgb, endpoint_end.rgb, color_index, color_bitcount);
    output.a   = interpolate(endpoint_start.a,   endpoint_end.a,   alpha_index, alpha_bitcount);
    
    if (mode.type == 4 OR == 5)
    {
        //Decode the 2 color rotation bits as follows:
        // 00 – Block format is Scalar(A) Vector(RGB) - no swapping
        // 01 – Block format is Scalar(R) Vector(AGB) - swap A and R
        // 10 – Block format is Scalar(G) Vector(RAB) - swap A and G
        // 11 - Block format is Scalar(B) Vector(RGA) - swap A and B
        rotation = extract_rot_bits(mode, block);
        output = swap_channels(output, rotation);
    }
    
}
```

Le pseudo-code ci-dessous décrit les étapes permettant de décoder entièrement la couleur du point de terminaison et les composants alpha pour chaque sous-ensemble dans un bloc BC7 de 16octets.

``` syntax
fully_decode_endpoints(endpoint_array, mode, block)
{
    //first handle modes that have P-bits
    if (mode.type == 0 OR == 1 OR == 3 OR == 6 OR == 7)
    {
        for each endpoint i
        {
            //component-wise left-shift
            endpoint_array[i].rgba = endpoint_array[i].rgba << 1;
        }
        
        //if P-bit is shared
        if (mode.type == 1) 
        {
            pbit_zero = extract_pbit_zero(mode, block);
            pbit_one = extract_pbit_one(mode, block);
            
            //rgb component-wise insert pbits
            endpoint_array[0].rgb |= pbit_zero;
            endpoint_array[1].rgb |= pbit_zero;
            endpoint_array[2].rgb |= pbit_one;
            endpoint_array[3].rgb |= pbit_one;  
        }
        else //unique P-bit per endpoint
        {  
            pbit_array = extract_pbit_array(mode, block);
            for each endpoint i
            {
                endpoint_array[i].rgba |= pbit_array[i];
            }
        }
    }

    for each endpoint i
    {
        // Color_component_precision & alpha_component_precision includes pbit
        // left shift endpoint components so that their MSB lies in bit 7
        endpoint_array[i].rgb = endpoint_array[i].rgb << (8 - color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a << (8 - alpha_component_precision(mode));

        // Replicate each component's MSB into the LSBs revealed by the left-shift operation above
        endpoint_array[i].rgb = endpoint_array[i].rgb | (endpoint_array[i].rgb >> color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a | (endpoint_array[i].a >> alpha_component_precision(mode));
    }
        
    //If this mode does not explicitly define the alpha component
    //set alpha equal to 1.0
    if (mode.type == 0 OR == 1 OR == 2 OR == 3)
    {
        for each endpoint i
        {
            endpoint_array[i].a = 255; //i.e. alpha = 1.0f
        }
    }
}
```

Pour générer chaque composant interpolé pour chaque sous-ensemble, utilisez l’algorithme suivant: «c» étant le composant à générer; «e0» le composant du point de terminaison 0 du sous-ensemble; et «e1» le composant du point de terminaison 1 du sous-ensemble.

``` syntax
UINT16 aWeight2[] = {0, 21, 43, 64};
UINT16 aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
UINT16 aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

UINT8 interpolate(UINT8 e0, UINT8 e1, UINT8 index, UINT8 indexprecision)
{
    if(indexprecision == 2)
        return (UINT8) (((64 - aWeights2[index])*UINT16(e0) + aWeights2[index]*UINT16(e1) + 32) >> 6);
    else if(indexprecision == 3)
        return (UINT8) (((64 - aWeights3[index])*UINT16(e0) + aWeights3[index]*UINT16(e1) + 32) >> 6);
    else // indexprecision == 4
        return (UINT8) (((64 - aWeights4[index])*UINT16(e0) + aWeights4[index]*UINT16(e1) + 32) >> 6);
}
```

Le pseudo-code suivant illustre la manière d'extraire les index et le nombre de bits pour les composants de couleur et alpha. Les blocs utilisant des composants de couleur et alpha distincts ont également deux jeux de données d’index: un pour le canal de vecteur, l'autre pour le canal scalaire. Pour le Mode 4, ces index ont des largeurs variables (2 ou 3bits); un sélecteur d’un bit indique si les données de vecteur ou scalaires utilisent les index de 3bits. (L'extraction du nombre de bits alpha est similaire à l’extraction du nombre de bits de couleur, mais avec un comportement inverse en fonction du bit **idxMode**.)

``` syntax
bitcount get_color_bitcount(block, mode)
{
    if (mode.type == 0 OR == 1)
        return 3;
    
    if (mode.type == 2 OR == 3 OR == 5 OR == 7)
        return 2;
    
    if (mode.type == 6)
        return 4;
        
    //The only remaining case is Mode 4 with 1-bit index selector
    idxMode = extract_idxMode(block);
    if (idxMode == 0)
        return 2;
    else
        return 3;
}
```

## <a name="span-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanbc7-format-mode-reference"></a><span id="BC7-format-mode-reference"></span><span id="bc7-format-mode-reference"></span><span id="BC7-FORMAT-MODE-REFERENCE"></span>Référence du mode de format BC7


Cette section contient une liste des 8modes de bloc et des allocations de bits pour les blocs de format de compression de textures BC7.

Les couleurs pour chaque sous-ensemble dans un bloc sont représentées par deux couleurs de point de terminaison explicites liées entre elles par un jeu de couleurs interpolées. En fonction de la précision d’index du bloc, chaque sous-ensemble peut avoir 4, 8 ou 16couleurs possibles.

### <a name="span-idmode-0spanspan-idmode-0spanspan-idmode-0spanmode-0"></a><span id="Mode-0"></span><span id="mode-0"></span><span id="MODE-0"></span>Mode 0

Le mode0 du format BC7 présente les caractéristiques suivantes:

-   Composants de couleur uniquement (aucun alpha)
-   3sous-ensembles par bloc
-   Points de terminaison RVBP4.4.4.1 avec un bitP unique par point de terminaison
-   Index 3bits
-   16partitions

![disposition des bits du mode0](images/bc7-mode0.png)

### <a name="span-idmode-1spanspan-idmode-1spanspan-idmode-1spanmode-1"></a><span id="Mode-1"></span><span id="mode-1"></span><span id="MODE-1"></span>Mode 1

Le mode1 du format BC7 présente les caractéristiques suivantes:

-   Composants de couleur uniquement (aucun alpha)
-   2sous-ensembles par bloc
-   Points de terminaison RVBP 6.6.6.1 avec un bitP partagé par sous-ensemble
-   Index 3bits
-   64partitions

![disposition des bits du mode1](images/bc7-mode1.png)

### <a name="span-idmode-2spanspan-idmode-2spanspan-idmode-2spanmode-2"></a><span id="Mode-2"></span><span id="mode-2"></span><span id="MODE-2"></span>Mode 2

Le mode2 du format BC7 présente les caractéristiques suivantes:

-   Composants de couleur uniquement (aucun alpha)
-   3sous-ensembles par bloc
-   Points de terminaison RVB5.5.5
-   Index 2bits
-   64partitions

![disposition des bits du mode2](images/bc7-mode2.png)

### <a name="span-idmode-3spanspan-idmode-3spanspan-idmode-3spanmode-3"></a><span id="Mode-3"></span><span id="mode-3"></span><span id="MODE-3"></span>Mode 3

Le mode3 du format BC7 présente les caractéristiques suivantes:

-   Composants de couleur uniquement (aucun alpha)
-   2sous-ensembles par bloc
-   Points de terminaison RVBP7.7.7.1 avec un bitP unique par sous-ensemble
-   Index 2bits
-   64partitions

![disposition des bits du mode3](images/bc7-mode3.png)

### <a name="span-idmode-4spanspan-idmode-4spanspan-idmode-4spanmode-4"></a><span id="Mode-4"></span><span id="mode-4"></span><span id="MODE-4"></span>Mode 4

Le mode4 du format BC7 présente les caractéristiques suivantes:

-   Composants de couleur avec composant alpha distinct
-   1sous-ensemble par bloc
-   Points de terminaison de couleur RVB5.5.5
-   Points de terminaison alpha 6bits
-   Index 16x2bits
-   Index 16x3bits
-   Rotation de composants 2bits
-   Sélecteur d’index de 1bit (que les index 2 ou 3bits soient utilisés)

![disposition des bits du mode4](images/bc7-mode4.png)

### <a name="span-idmode-5spanspan-idmode-5spanspan-idmode-5spanmode-5"></a><span id="Mode-5"></span><span id="mode-5"></span><span id="MODE-5"></span>Mode 5

Le mode5 du format BC7 présente les caractéristiques suivantes:

-   Composants de couleur avec composant alpha distinct
-   1sous-ensemble par bloc
-   Points de terminaison de couleur RVB7.7.7
-   Points de terminaison alpha 6bits
-   Index de couleur de 16 x 2bits
-   Index alpha de 16 x 2bits
-   Rotation de composants 2bits

![disposition des bits du mode5](images/bc7-mode5.png)

### <a name="span-idmode-6spanspan-idmode-6spanspan-idmode-6spanmode-6"></a><span id="Mode-6"></span><span id="mode-6"></span><span id="MODE-6"></span>Mode 6

Le mode6 du format BC7 présente les caractéristiques suivantes:

-   Composants alpha et couleur combinés
-   Un sous-ensemble par bloc
-   Points de terminaison de couleur (et alpha) RVBAP7.7.7.7.1 (bitP unique par point de terminaison)
-   Index 16x4bits

![disposition des bits du mode6](images/bc7-mode6.png)

### <a name="span-idmode-7spanspan-idmode-7spanspan-idmode-7spanmode-7"></a><span id="Mode-7"></span><span id="mode-7"></span><span id="MODE-7"></span>Mode 7

Le mode7 du format BC7 présente les caractéristiques suivantes:

-   Composants alpha et couleur combinés
-   2sous-ensembles par bloc
-   Points de terminaison de couleur (et alpha) RVBAP5.5.5.5.1 (bitP unique par point de terminaison)
-   Index 2bits
-   64partitions

![disposition des bits du mode7](images/bc7-mode7.png)

### <a name="span-idremarksspanspan-idremarksspanspan-idremarksspanremarks"></a><span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Remarques

Le mode 8 (l’octet de poids faible est défini sur 0x00) est réservé. Ne l’utilisez pas dans votre encodeur. Si vous transmettez ce mode au matériel, un bloc initialisé à tous les zéros est retourné.

Dans BC7, vous pouvez encoder le composant alpha de l'une des manières suivantes:

-   Types de blocs sans encodage explicite du composant alpha. Dans ces blocs, les points de terminaison de couleur ont un encodage RVB uniquement, où le composant alpha est décodé sur 1.0 pour tous les texels.
-   Types de bloc avec composants alpha et de couleur combinés. Dans ces blocs, les valeurs de couleur du point de terminaison sont spécifiées dans le format RVBA, et les valeurs de composants alpha sont interpolées avec les valeurs de couleur.
-   Types de bloc avec composants alpha et de couleur distincts. Dans ces blocs, les valeurs de couleur et alpha sont spécifiées séparément, chacune disposant de leur propre jeu d’index. Par conséquent, le canal vectoriel et le canal scalaire sont encodés séparément; le vecteur spécifie généralement les canaux de couleur \[R, V, B\] et le canal scalaire spécifie le canal alpha \[A\]. Pour prendre en charge cette approche, un champ de 2bits distinct est fourni avec l'encodage pour permettre de spécifier l'encodage des canaux distincts sous forme de valeur scalaire. Par conséquent, le bloc peut avoir l'une des quatre représentations suivantes de cet encodage alpha (indiquées par le champ de 2bits):
    -   RVB|A: canal alpha distinct
    -   AVB|R: canal de couleur «rouge» distinct
    -   RAB|V: canal de couleur «vert» distinct
    -   RVA|B: canal de couleur «bleu» distinct

    Le décodeur réorganise l’ordre des canaux en RVBA après le décodage, de sorte que le format de bloc interne est invisible pour le développeur. Les blocs utilisant des composants de couleur et alpha distincts ont également deux jeux de données d’index: un pour l’ensemble de canaux vectoriels, l'autre pour le canal scalaire. (Dans le cas du mode4, ces index ont des largeurs variables \[2 ou 3bits\]. Le mode4 contient également un sélecteur de 1bit qui indique si le canal vectoriel ou scalaire utilise les indices de 3bits.)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Compression de blocs de texture](texture-block-compression.md)

 

 




