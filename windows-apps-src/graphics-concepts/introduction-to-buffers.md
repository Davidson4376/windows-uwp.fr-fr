---
title: "Introduction aux mémoires tampons"
description: "Une ressource de mémoire tampon est un ensemble de données dont le type a été intégralement spécifié, regroupées en éléments."
ms.assetid: 494FDF57-0FBE-434C-B568-06F977B40263
keywords:
- "Introduction aux mémoires tampons"
author: mtoepke
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ec2d181daca8f9ece8e1d364cd58234feb85977a
ms.lasthandoff: 02/07/2017

---

# <a name="introduction-to-buffers"></a>Introduction aux mémoires tampons


Une ressource de mémoire tampon est un ensemble de données dont le type a été intégralement spécifié, regroupées en éléments. Les mémoires tampons stockent des données, telles que les coordonnées de texture dans une *mémoire tampon de vertex*, les index dans une *mémoire tampon d’index*, les données constantes de nuanceur dans une *mémoire tampon constante*, les vecteurs de position, les vecteurs normaux ou l’état de l’appareil.

Un élément de mémoire tampon est constitué de 1 à 4 éléments. Les éléments de mémoire tampon peuvent comporter des valeurs de données compressées (comme des valeurs de surface R8G8B8A8), des entiers 8 bits uniques ou quatre valeurs à virgule flottante de 32 bits.

Un tampon est créé en tant que ressource non structurée. Dans la mesure où il n’est pas structuré, un tampon ne peut pas contenir de niveaux de mipmap, n’est pas filtré lors de la lecture et ne peut pas être échantillonné plusieurs fois.

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Types de mémoires tampons


Voici les types de ressources de mémoire tampon pris en charge par Direct3D 11.

-   [Tampon de vertex](#vertex-buffer)
-   [Tampon d’index](#index-buffer)
-   [Mémoire tampon constante](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Tampon de vertex

Une mémoire tampon de vertex comporte les données de vertex utilisées pour définir votre géométrie. Les données de vertex comprennent les coordonnées de position, les données de couleur, les données de coordonnées de texture, les données normales, etc.

Considérons l’exemple le plus simple de tampon de vertex : celui qui contient uniquement les données de position. Ce type de tampon peut être illustré comme suit.

![illustration d’un tampon de vertex qui contient des données de position](images/d3d10-resources-single-element-vb2.png)

Plus souvent, un tampon de vertex contient toutes les données nécessaires pour spécifier intégralement des vertex 3D. Un exemple de ce cas de figure pourrait être celui d’un tampon de vertex contenant la position de chaque vertex ainsi que des coordonnées normales et de texture. Ces données sont généralement organisées en ensembles d’éléments de vertex, comme illustré ci-dessous.

![illustration d’un tampon de vertex contenant les données de texture, normales et de position](images/d3d10-vertex-buffer-element.png)

Ce tampon de vertex contient les données spécifiques au vertex. Chaque vertex stocke 3 éléments (coordonnées de position, normales et de texture). Généralement, les coordonnées de position et normales sont spécifiées à l’aide de trois valeurs flottantes de 32 bits et les coordonnées de texture à l’aide de deux valeurs flottantes de 32 bits.

Pour accéder aux données à partir d’un tampon de vertex, vous devez identifier le vertex et connaître les paramètres supplémentaires suivants de mémoire tampon :

-   Offset - Le nombre d’octets entre le début du tampon et les données du premier vertex.
-   BaseVertexLocation - Le nombre d’octets entre l’offset et le premier vertex utilisé par l’appel de dessin approprié.

Avant de créer un tampon de vertex, vous devez définir sa disposition. Une fois l’objet de disposition d’entrée créé, vous le liez à l’[étape de l’assembleur d’entrée](input-assembler-stage--ia-.md).

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Tampon d’index

Les mémoires tampons d’index comportent des décalages d’entier dans les tampons de vertex ; elles ont vocation à accroître l’efficacité du rendu des primitives. Un tampon d’index contient un ensemble d’index séquentiel de 16 ou 32 bits ; chaque index est utilisé pour identifier un vertex dans un tampon de vertex. Un tampon d’index peut être illustré comme suit.

![illustration d’un tampon d’index](images/d3d10-index-buffer.png)

Les index séquentiels stockés dans un tampon d’index se trouvent avec les paramètres suivants :

-   Offset - Le nombre d’octets de l’adresse de base du tampon d’index.
-   StartIndexLocation - Spécifie le premier élément de tampon d’index de l’adresse de base avec le décalage. L’emplacement de départ représente le premier index à afficher.
-   IndexCount - Le nombre d’index à afficher.

Début du tampon d’index = Adresse de base du tampon d’index + décalage (octets) + StartIndexLocation \* ElementSize (octets).

Dans ce calcul, ElementSize représente la taille de chaque élément de tampon d’index, égale à 2 ou 4 octets.

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Mémoire tampon constante

Une mémoire tampon constante vous permet de fournir efficacement des données de constante de nuanceur au pipeline. Utilisez une mémoire tampon constante pour stocker les résultats de l’étape de sortie de flux. D’un point de vue conceptuel, une mémoire tampon constante ressemble à un tampon de vertex à élément unique, tel que représenté dans l’illustration suivante.

![illustration d’une mémoire tampon constante de nuanceur](images/d3d10-shader-resource-buffer.png)

Chaque élément stocke une constante de 1 à 4 composants, en fonction du format des données stockées.

Une mémoire tampon constante peut utiliser uniquement un seul indicateur de liaison, qui ne peut être combiné avec aucun autre indicateur.

Pour lire une mémoire tampon constante de nuanceur à partir d’un nuanceur, utilisez une fonction de chargement HLSL. Chaque étape du nuanceur permet d’utiliser jusqu’à 15 mémoires tampons constantes ; chaque tampon peut contenir jusqu’à 4 096 constantes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Mémoires tampons de vertex et d’index](vertex-and-index-buffers.md)

 

 





