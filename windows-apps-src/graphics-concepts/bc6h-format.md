---
title: Format BC6H
description: Le format BC6H est un format de compression de texture conçu pour prendre en charge les espaces de couleur à plage dynamique élevée (HDR) dans les données source.
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- Format BC6H
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: be88f06cd5893f2f67697a54754826440bdf7d18
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5814099"
---
# <a name="bc6h-format"></a>Format BC6H


Le format BC6H est un format de compression de texture conçu pour prendre en charge les espaces de couleur à plage dynamique élevée (HDR) dans les données source.

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgiformatbc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>À propos de BC6H/DXGI\_FORMAT\_BC6H


Le format BC6H fournit une compression de haute qualité pour les images qui utilisent trois canaux de couleurs HDR, avec une valeur de 16bits pour chaque canal de couleur de la valeur (16: 16:16). Le canaux alpha ne sont pas pris en charge.

Le format BC6H est spécifié par les valeurs d’énumération DXGI\_FORMAT suivantes:

-   **DXGI\_FORMAT\_BC6H\_TYPELESS**.
-   **DXGI\_FORMAT\_BC6H\_UF16**. Ce format BC6H n’utilise pas de bit de signe dans les valeurs de canal de couleur à virgule flottante de 16bits.
-   **DXGI\_FORMAT\_BC6H\_SF16**. Ce format BC6H utilise un bit de signe dans les valeurs de canal de couleur à virgule flottante de 16bits.

**Remarque**  l’à virgule flottante pour les canaux de couleur de 16 bits est souvent désigné comme format à virgule flottante «moitié». Ce format présente la disposition de bits suivante:
|                       |                                                 |
|-----------------------|-------------------------------------------------|
| UF16 (flottant non signé) | 5bits exposants + 11bits mantisse              |
| SF16 (flottant signé)   | 1bit de signe + 5bits exposants + 10bits mantisse |

 

 

Le format BC6H peut être utilisé pour les ressources de texture [Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277) (y compris les tableaux), Texture3D ou TextureCube (y compris les tableaux). De même, ce format s’applique à toutes les surfaces de carte MIP associées à ces ressources.

BC6H utilise une taille de bloc fixe de 16octets (128bits) et une taille de vignette fixe de 4x4texels. Comme pour les formats BC précédents, les images de texture d'une taille supérieure à la taille de vignette prise en charge (4x4) sont compressées à l’aide de plusieurs blocs. Cette identité d’adressage s’applique également aux images 3D, aux mipmaps, aux cartes de cube et aux tableaux de textures. Toutes les vignettes de l’image doivent être au même format.

Avertissements concernant le format BC6H:

-   Le format BC6H prend en charge la dénormalisation à virgule flottante, mais ni la représentation INF (infini) ni la représentation NaN («Not a Number»). L’exception est le mode signé de BC6H (DXGI\_FORMAT\_BC6H\_SF16), qui prend en charge l'infini négatif (-INF). Cette prise en charge de -INF est simplement un artéfact du format lui-même et n’est pas spécifiquement prise en charge par les encodeurs pour ce format. En règle générale, quand un encodeur rencontre des données d'entrée INF (positif ou négatif) ou NaN, l’encodeur doit convertir ces données dans la valeur de représentation Non INF maximale autorisée et mapper NaN à la valeur 0 avant la compression.
-   BC6H ne prend pas en charge le canal alpha.
-   Le décodeur BC6H effectue la décompression avant d'appliquer le filtrage de textures.
-   La décompression BC6H doit être précise au bit près; autrement dit, le matériel doit retourner des résultats identiques au décodeur décrit dans cette documentation.

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>Implémentation BC6H


Un bloc BC6H se compose de bits de mode, de points de terminaison compressés, d'indices compressés et d'un index de partition facultatif. Ce format spécifie 14modes différents.

Une couleur de point de terminaison est stockée sous la forme d'un triplet RVB. Le format BC6H définit une palette de couleurs sur une ligne approximative à travers un certain nombre de points de terminaison de couleur définis. En outre, selon le mode, une vignette peut être divisée en deux zones ou traitée comme une seule région, dans laquelle une vignette à deux zones comporte pour chaque zone un ensemble distinct de points de terminaison de couleur. BC6H stocke un index de palette par texel.

Dans le cas d'une vignette à deux zones, 32partitions sont possibles.

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>Décodage du format BC6H


Le pseudo-code ci-dessous illustre les étapes permettant de décompresser le pixel (x, y) étant donné le bloc BC6H de 16octets.

``` syntax
decompress_bc6h(x, y, block)
{
    mode = extract_mode(block);
    endpoints;
    index;
    
    if(mode.type == ONE)
    {
        endpoints = extract_compressed_endpoints(mode, block);
        index = extract_index_ONE(x, y, block);
    }
    else //mode.type == TWO
    {
        partition = extract_partition(block);
        region = get_region(partition, x, y);
        endpoints = extract_compressed_endpoints(mode, region, block);
        index = extract_index_TWO(x, y, partition, block);
    }
    
    unquantize(endpoints);
    color = interpolate(index, endpoints);
    finish_unquantize(color);
}
```

Le tableau suivant contient le nombre et les valeurs de bits pour chacun des 14formats possibles pour les blocs BC6H.

| Mode | Index de partition | Partition | Points de terminaison de couleur                  | Bits de mode      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46bits           | 5bits    | 75bits (10.555, 10.555, 10.555) | 2bits (00)    |
| 2    | 46bits           | 5bits    | 75bits (7666, 7666, 7666)       | 2bits (01)    |
| 3    | 46bits           | 5bits    | 72bits (11.555, 11.444, 11.444) | 5bits (00010) |
| 4    | 46bits           | 5bits    | 72bits (11.444, 11.555, 11.444) | 5bits (00110) |
| 5    | 46bits           | 5bits    | 72bits (11.444, 11.444, 11.555) | 5bits (01010) |
| 6    | 46bits           | 5bits    | 72bits (9555, 9555, 9555)       | 5bits (01110) |
| 7    | 46bits           | 5bits    | 72bits (8666, 8555, 8555)       | 5bits (10010) |
| 8    | 46bits           | 5bits    | 72bits (8555, 8666, 8555)       | 5bits (10110) |
| 9    | 46bits           | 5bits    | 72bits (8555, 8555, 8666)       | 5bits (11010) |
| 10   | 46bits           | 5bits    | 72bits (6666, 6666, 6666)       | 5bits (11110) |
| 11   | 63bits           | 0bits    | 60bits (10.10, 10.10, 10.10)    | 5bits (00011) |
| 12   | 63bits           | 0bits    | 60bits (11.9, 11.9, 11.9)       | 5bits (00111) |
| 13   | 63bits           | 0bits    | 60bits (12.8, 12.8, 12.8)       | 5bits (01011) |
| 14   | 63bits           | 0bits    | 60bits (16.4, 16.4, 16.4)       | 5bits (01111) |

 

Chaque format présenté dans ce tableau peut être identifié de manière unique par les bits de mode. Les dix premiers modes sont utilisés pour les vignettes à deux zones, et le champ de bits de mode peut contenir deux ou cinq bits. Ces blocs comportent également des champs pour les points de terminaison de couleur compressés (72 ou 75bits), la partition (5bits) et les index de partition (46bits).

Pour les points de terminaison de couleur compressés, les valeurs données dans le tableau précédent indiquent la précision des points de terminaison RVB stockés et le nombre de bits utilisés pour chaque valeur de couleur. Par exemple, le mode3 spécifie un niveau de précision de point de terminaison de couleur de 11 et le nombre de bits utilisés pour stocker les valeurs de delta des points de terminaison transformés pour les couleurs rouge, bleu et vert (5, 4 et 4, respectivement). Le mode10 n’utilise pas de compression delta et stocke les quatre points de terminaison de couleur de manière explicite.

Les quatre derniers modes de bloc sont utilisés pour les vignettes à une zone, où le champ de mode est de 5bits. Ces blocs comportent des champs pour les points de terminaison (60bits) et les index compressés (63bits). Le mode11 (comme le mode10) n’utilise pas de compression delta et stocke deux points de terminaison de couleur de manière explicite.

Les modes10011, 10111, 11011 et 11111 (non illustrés) sont réservés. Ne les utilisez pas dans votre encodeur. Si le matériel reçoit des blocs où l’un de ces modes est spécifié, le bloc décompressé obtenu doit contenir tous les zéros dans tous les canaux, à l’exception du canal alpha.

Pour le format BC6H, le canal alpha doit toujours retourner 1.0, quel que soit le mode.

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>Jeu de partitions BC6H

Une vignette à deux zones peut comporter 32jeux de partitions possibles, que vous trouverez dans le tableau ci-dessous. Chaque bloc de 4x4 représente une forme unique.

![Tableau des jeux de partitions BC6H](images/bc6h-partition-sets.png)

Dans ce tableau de jeux de partitions, les entrées en gras et soulignées représentent l’emplacement de l’index de correction pour un sous-ensemble1 (qui est spécifié avec un bit de moins). L’index de correction pour le sous-ensemble0 est toujours l'index0, puisque le partitionnement est toujours organisé de sorte que cet index0 soit toujours dans le sous-ensemble0. L'ordre des partitions se lit de haut à gauche vers le bas à droite, puis de gauche à droite et de haut en bas.

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>Format de point de terminaison compressé BC6H


![Champs de bits pour les formats de point de terminaison BC6H compressés](images/bc6h-headers-med.png)

Ce tableau indique les champs de bits pour les points de terminaison compressés en fonction du format de point de terminaison, où chaque colonne spécifie un encodage et chaque ligne un champ de bits. Cette approche occupe 82bits pour les vignettes à deux zones et 65bits pour les vignettes à une zone. Par exemple, les 5premiers bits pour l'encodage à une zone \[16 4\] ci-dessus (plus spécifiquement, la colonne la plus à droite) sont les bits m\[4:0\], les 10bits suivants sont les bits rw\[9:0\] et ainsi de suite avec les 6derniers bits 6 contenant bw\[10:15\].

Les noms de champs dans le tableau ci-dessus sont définis comme suit:

| Champ | Variable          |
|-------|-------------------|
| m     | mode              |
| d     | index de forme       |
| rw    | endpt\[0\].A\[0\] |
| rx    | endpt\[0\].B\[0\] |
| ry    | endpt\[1\].A\[0\] |
| rz    | endpt\[1\].B\[0\] |
| gw    | endpt\[0\].A\[1\] |
| gx    | endpt\[0\].B\[1\] |
| gy    | endpt\[1\].A\[1\] |
| gz    | endpt\[1\].B\[1\] |
| bw    | endpt\[0\].A\[2\] |
| bx    | endpt\[0\].B\[2\] |
| by    | endpt\[1\].A\[2\] |
| bz    | endpt\[1\].B\[2\] |

 

Endpt\[i\], où i est égal à 0 ou à 1, se réfère respectivement au 0e ou 1e ensemble de points de terminaison.
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>Extension de signe pour les valeurs de point de terminaison


Pour les vignettes à deux zones, quatre valeurs de point de terminaison peuvent avoir une extension de signe. Endpt\[0\].A est signé uniquement si le format est un format signé; les autres points de terminaison sont signés uniquement si le point de terminaison a été transformé, ou si le format est un format signé. Le code ci-dessous illustre l’algorithme permettant d’étendre le signe des valeurs de point de terminaison à deux zones.

``` syntax
static void sign_extend_two_region(Pattern &p, IntEndpts endpts[NREGIONS_TWO])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
      if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
      if (p.transformed || BC6H::FORMAT == SIGNED_F16)
      {
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
        endpts[1].A[i] = SIGN_EXTEND(endpts[1].A[i], p.chan[i].delta[1]);
        endpts[1].B[i] = SIGN_EXTEND(endpts[1].B[i], p.chan[i].delta[2]);
      }
    }
}
```

Pour les vignettes à une zone, le comportement est le même; seul endpt\[1\] est supprimé.

``` syntax
static void sign_extend_one_region(Pattern &p, IntEndpts endpts[NREGIONS_ONE])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
    if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
    if (p.transformed || BC6H::FORMAT == SIGNED_F16) 
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
    }
}
```

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>Inversion de la transformation pour les valeurs de point de terminaison


Pour les vignettes à deux zones, la transformation applique l’inverse de l'encodage de différence, en ajoutant la valeur de base au niveau du paramètre endpt\[0\].A aux trois autres entrées, soit un total de 9opérations d'ajout. Dans l’image ci-dessous, la valeur de base est représentée par «A0» et est associée à la plus haute précision de virgule flottante. «A1», «B0» et «B1» sont tous des deltas calculés à partir de la valeur d’ancrage, et ces valeurs delta sont représentées avec une précision plus faible. (A0 correspond à endpt\[0\].A, B0 correspond à endpt\[0\].B, A1 correspond à endpt\[1\].A et B1 correspond à endpt\[1\].B.)

![Calcul des valeurs de point de terminaison pour l'inversion de transformation](images/bc6h-transform-inverse.png)

Pour les vignettes à une zone, il n'y a qu'un seul décalage delta, et par conséquent seulement 3opérations d'ajout.

Le décompresseur doit s’assurer que les résultats de la transformation inverse n'augmenteront pas excessivement la précision de endpt\[0\].a. Dans le cas contraire, les valeurs résultant de la transformation inverse doivent être encapsulées dans le même nombre de bits. Si la précision de A0 est «p bits», l’algorithme de transformation est le suivant:

`B0 = (B0 + A0) & ((1 << p) - 1)`

Pour les formats signés, une extension du signe doit également être appliquée aux résultats du calcul du delta. Si l’opération d’extension du signe considère une extension des deux signes, où 0 est positif et 1 est négatif, l’extension de signe de 0 s’occupe du clamp ci-dessus. De la même manière, après le clamp ci-dessus, seule une valeur de 1 (négative) doit avoir une extension de signe.

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>Déquantification des points de terminaison de couleur


Étant donnés les points de terminaison non compressés, l’étape suivante consiste à effectuer une déquantification initiale des points de terminaison de couleur. Cette opération comprend trois étapes:

-   Déquantification des palettes de couleurs
-   Interpolation des palettes
-   Finalisation de la déquantification

La séparation du processus de déquantification en deux parties (déquantification des palettes de couleurs avant interpolation et déquantification finale après interpolation) permet de réduire le nombre d’opérations de multiplication nécessaires par rapport à un processus de déquantification complet avant l’interpolation des palettes.

Le code ci-dessous illustre le processus de récupération des estimations des valeurs de couleur 16bits d’origine, qui utilise ensuite les valeurs de poids spécifiées pour ajouter 6valeurs de couleurs supplémentaires à la palette. La même opération est effectuée sur chaque canal.

``` syntax
int aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
int aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

// c1, c2: endpoints of a component
void generate_palette_unquantized(UINT8 uNumIndices, int c1, int c2, int prec, UINT16 palette[NINDICES])
{
    int* aWeights;
    if(uNumIndices == 8)
        aWeights = aWeight3;
    else  // uNumIndices == 16
        aWeights = aWeight4;

    int a = unquantize(c1, prec); 
    int b = unquantize(c2, prec);

    // interpolate
    for(int i = 0; i < uNumIndices; ++i)
        palette[i] = finish_unquantize((a * (64 - aWeights[i]) + b * aWeights[i] + 32) >> 6);
}
```

L’exemple de code suivant illustre le processus d’interpolation, avec les observations suivantes:

-   Puisque la plage complète des valeurs de couleur pour la fonction **unquantize** (ci-dessous) est comprise entre-32768 et 65535, l’interpolateur est implémenté à l’aide d'une arithmétique signée de 17bits.
-   Après l’interpolation, les valeurs sont transmises à la fonction **finish\_unquantize** (décrite dans le troisième exemple de cette section), qui s’applique à la mise à l’échelle finale.
-   Tous les décompresseurs matériels sont nécessaires pour renvoyer des résultats d'une précision au bit près avec ces fonctions.

``` syntax
int unquantize(int comp, int uBitsPerComp)
{
    int unq, s = 0;
    switch(BC6H::FORMAT)
    {
    case UNSIGNED_F16:
        if(uBitsPerComp >= 15)
            unq = comp;
        else if(comp == 0)
            unq = 0;
        else if(comp == ((1 << uBitsPerComp) - 1))
            unq = 0xFFFF;
        else
            unq = ((comp << 16) + 0x8000) >> uBitsPerComp;
        break;

    case SIGNED_F16:
        if(uBitsPerComp >= 16)
            unq = comp;
        else
        {
            if(comp < 0)
            {
                s = 1;
                comp = -comp;
            }

            if(comp == 0)
                unq = 0;
            else if(comp >= ((1 << (uBitsPerComp - 1)) - 1))
                unq = 0x7FFF;
            else
                unq = ((comp << 15) + 0x4000) >> (uBitsPerComp-1);

            if(s)
                unq = -unq;
        }
        break;
    }
    return unq;
}
```

La fonction **finish\_unquantize** est appelée après l’interpolation des palettes. La fonction **unquantize** diffère la mise à l’échelle de 31/32 pour les bits signés et de 31/64 pour les bits non signés. Ce comportement est nécessaire pour obtenir la valeur finale dans la demi-plage valide (-0x7BFF ~ 0x7BFF) après l'interpolation des palettes afin de réduire le nombre de multiplications nécessaires. La fonction **finish\_unquantize** applique la mise à l’échelle finale et retourne une valeur **unsigned short** qui est réinterprétée en **half**.

``` syntax
unsigned short finish_unquantize(int comp)
{
    if(BC6H::FORMAT == UNSIGNED_F16)
    {
        comp = (comp * 31) >> 6;                                         // scale the magnitude by 31/64
        return (unsigned short) comp;
    }
    else // (BC6H::FORMAT == SIGNED_F16)
    {
        comp = (comp < 0) ? -(((-comp) * 31) >> 5) : (comp * 31) >> 5;   // scale the magnitude by 31/32
        int s = 0;
        if(comp < 0)
        {
            s = 0x8000;
            comp = -comp;
        }
        return (unsigned short) (s | comp);
    }
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Compression de blocs de texture](texture-block-compression.md)

 

 




