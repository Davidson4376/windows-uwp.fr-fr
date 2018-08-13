---
title: Introduction aux mémoires tampons
description: Une ressource de mémoire tampon est un ensemble de données dont le type a été intégralement spécifié, regroupées en éléments.
ms.assetid: 494FDF57-0FBE-434C-B568-06F977B40263
keywords:
- Introduction aux mémoires tampons
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 677eea4757220320bc364fef20bf5a5c8d4daaa2
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044338"
---
# <a name="introduction-to-buffers"></a>Introduction aux mémoires tampons


Une ressource de mémoire tampon est un ensemble de données dont le type a été intégralement spécifié, regroupées en éléments. Les mémoires tampons stockent des données, telles que les coordonnées de texture dans une *mémoire tampon de vertex*, les index dans une *mémoire tampon d’index*, les données constantes de nuanceur dans une *mémoire tampon constante*, les vecteurs de position, les vecteurs normaux ou l’état de l’appareil.

Un élément de la mémoire tampon est constitué des composants de 1 à 4. Les éléments de mémoire tampon peuvent comporter des valeurs de données compressées (comme des valeurs de surface R8G8B8A8), des entiers8bits uniques ou quatre valeurs à virgule flottante de 32bits.

Un tampon est créé en tant que ressource non structurée. Car il s’agit non structuré, un tampon ne peut pas contenir tous les niveaux mipmap, il ne peut pas obtenir filtré lors de la lecture et il ne peut pas être échantillonnage multiple.

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Types de mémoire tampon


Voici les types de ressources mémoire tampon pris en charge par Direct3D 11.

-   [Mémoire tampon de vertex](#vertex-buffer)
-   [Mémoire tampon d’index](#index-buffer)
-   [Constante tampon](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Mémoire tampon sommet

Un tampon sommet contient les données de sommet utilisées pour définir la géométrie. Les données de vertex comprennent les coordonnées de position, les données de couleur, les données de coordonnées de texture, les données normales, etc.

Un exemple simple d’un tampon sommet est celui qui contient uniquement les données de position. Ce type de tampon peut être illustré comme suit.

![illustration d’un tampon de vertex qui contient des données de position](images/d3d10-resources-single-element-vb2.png)

Plus souvent, un tampon de vertex contient toutes les données nécessaires pour spécifier intégralement des vertex3D. Un exemple de ce cas de figure pourrait être celui d’un tampon de vertex contenant la position de chaque vertex ainsi que des coordonnées normales et de texture. Ces données sont généralement organisées en ensembles d’éléments de vertex, comme illustré ci-dessous.

![illustration d’un tampon de sommet contenant les données de texture, normales et de position](images/d3d10-vertex-buffer-element.png)

Ce tampon sommet contient des données de par sommet; chaque sommet stocke les trois éléments (position normale et les coordonnées de texture). Généralement, les coordonnées de position et normales sont spécifiées à l’aide de trois valeurs flottantes de 32bits et les coordonnées de texture à l’aide de deux valeurs flottantes de 32bits.

Pour accéder aux données à partir d’un tampon sommet, vous devez connaître le sommet pour access, ainsi que les paramètres de mémoire tampon supplémentaires suivants:

-   Offset - le nombre d’octets entre le début du tampon et les données du premier sommet.
-   BaseVertexLocation -le nombre d’octets entre le décalage et le premier sommet utilisé par l’appel de dessin approprié.

Avant de créer un tampon sommet, vous devez définir sa présentation. Une fois que l’objet de mise en entrée est créé, liez-le à la [phase d’entrée assembleur (IA)](input-assembler-stage--ia-.md).

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Tampon d’index

Tampons d’index contiennent les décalages entier dans des tampons sommet et sont utilisés pour restituer primitives plus efficacement. Un tampon d’index contient un ensemble séquentiel d’index de 16 ou 32bits; chaque index est utilisé pour identifier un sommet dans un tampon de sommet. Un tampon d’index peut être illustré comme suit.

![illustration d’un tampon d’index](images/d3d10-index-buffer.png)

Les index séquentiels stockés dans un tampon d’index se trouvent avec les paramètres suivants:

-   Décalage - le nombre d’octets à partir de l’adresse de base de la mémoire tampon d’index.
-   StartIndexLocation - Spécifie le premier élément de mémoire tampon d’index à partir de l’adresse de base et le décalage. Emplacement de départ représente le premier index pour l’afficher.
-   IndexCount -le nombre d’index à afficher.

Début de la mémoire tampon d’Index = adresse de Base de mémoire tampon Index décalage (octets) + StartIndexLocation \ * ElementSize (octets);

Dans ce calcul, ElementSize est la taille de chaque élément de mémoire tampon index, qui est de deux ou quatre octets.

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Constante tampon

Un tampon constant permet de fournir efficacement des données de constantes shader dans le pipeline. Vous pouvez utiliser une mémoire tampon de constant pour stocker les résultats de la phase de flux de sortie. De façon conceptuelle, un tampon constant ressemble à un seul élément sommet tampon, comme indiqué dans l’illustration suivante.

![illustration d’une mémoire tampon constante de nuanceur](images/d3d10-shader-resource-buffer.png)

Chaque élément stocke une constante de 1 à 4composants, en fonction du format des données stockées.

Un tampon constant permet uniquement un indicateur de liaison unique, qui ne peuvent pas être combiné avec n’importe quel autre indicateur de liaison.

Pour lire un tampon constante shader un shader, utilisez une fonction de la charge HLSL. Chaque étape du nuanceur permet d’utiliser jusqu’à 15tampons constants; chaque tampon peut contenir jusqu’à 4096constantes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Mémoires tampons de vertex et d’index](vertex-and-index-buffers.md)

 

 




