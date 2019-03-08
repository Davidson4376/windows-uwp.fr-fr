---
title: Types de ressource
description: Les différents types de ressources ont une disposition (ou un encombrement mémoire) distinct(e).
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- Types de ressource
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4138fd7275f7e1f9addb7685ff0846e756701003
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661984"
---
# <a name="resource-types"></a>Types de ressource


Les différents types de ressources ont une disposition (ou un encombrement mémoire) distinct(e). Toutes les ressources utilisées par le pipeline Direct3D dérivent de deux types de ressources de base : les [tampons](#buffer-resources) et les [textures](#texture-resources). Un tampon est un ensemble de données brutes (éléments) ; une texture est un ensemble de texels (éléments de texture).

Il existe deux façons de spécifier intégralement la disposition (ou l’encombrement mémoire) d’une ressource :

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Élément</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Typed</p></td>
<td align="left"><p>Spécifiez intégralement le type lors de la création de la ressource.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>Sans type</p></td>
<td align="left"><p>Spécifiez intégralement le type lorsque de la ressource est liée au pipeline.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbufferresourcesspanspan-idbufferresourcesspanspan-idbufferresourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>Ressources de la mémoire tampon


Une ressource tampon est un ensemble de données dont le type a été intégralement spécifié ; en interne, un tampon contient des éléments. Un élément est constitué de 1 à 4 composants. Exemples de types de données d’élément : une valeur de données compressée (par exemple R8G8B8A8), un nombre entier unique de 8 bits, quatre valeurs flottantes de 32 bits. Ces types de données sont utilisés pour stocker des données, par exemple un vecteur de position, un vecteur normal, une coordonnée de texture dans un tampon de sommet, un index dans un tampon d’index ou un état d’appareil.

Un tampon est créé en tant que ressource non structurée. Dans la mesure où il n’est pas structuré, un tampon ne peut pas contenir de niveaux de mipmap, n’est pas filtré lors de la lecture et ne peut pas être échantillonné plusieurs fois.

### <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Types de mémoires tampons

-   [Mémoire tampon de vertex](#vertex-buffer)
-   [Mémoire tampon d’index](#index-buffer)
-   [Mémoire tampon constante](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Mémoire tampon de vertex

Un tampon est un ensemble d’éléments ; un tampon de sommet contient les données par sommet. L’exemple le plus simple est un tampon de sommet qui contient un type de données, par exemple des données de position. Ce type de tampon peut être illustré comme suit.

![illustration d’un tampon de vertex qui contient des données de position](images/d3d10-resources-single-element-vb2.png)

Plus souvent, un tampon de vertex contient toutes les données nécessaires pour spécifier intégralement des vertex 3D. Un exemple de ce cas de figure pourrait être celui d’un tampon de vertex contenant la position de chaque vertex ainsi que des coordonnées normales et de texture. Ces données sont généralement organisées en ensembles d’éléments de vertex, comme illustré ci-dessous.

![illustration d’un tampon de vertex contenant les données de texture, normales et de position](images/d3d10-vertex-buffer-element.png)

Ce tampon de sommet contient les données par sommet pour huit sommets ; chaque sommet stocke trois éléments (coordonnées de position, normales et de texture). Généralement, les coordonnées de position et normales sont spécifiées à l’aide de trois valeurs flottantes de 32 bits et les coordonnées de texture à l’aide de deux valeurs flottantes de 32 bits.

Pour accéder aux données à partir d’un tampon de sommet, vous devez savoir à quel sommet vous devez accéder et connaître les paramètres de tampon suivants :

-   *Offset* - le nombre d’octets entre le début du tampon et les données du premier sommet.
-   *BaseVertexLocation* -le nombre d’octets entre le décalage et le premier sommet utilisé par l’appel de dessin approprié.

Avant de créer un tampon de sommet, vous devez définir sa disposition en créant un objet de disposition d’entrée. Une fois que l’objet de disposition d’entrée est créé, liez-le à l’étape de l’assembleur d’entrée (IA).

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Mémoire tampon d’index

Un tampon d’index contient un ensemble séquentiel d’index de 16 ou 32 bits ; chaque index est utilisé pour identifier un sommet dans un tampon de sommet. Le fait d’utiliser un tampon d’index avec un ou plusieurs tampons de sommet pour fournir des données à l’étape de l’assembleur d’entrée (IA) est appelé « indexation ». Un tampon d’index peut être illustré comme suit.

![illustration d’un tampon d’index](images/d3d10-index-buffer.png)

Les index séquentiels stockés dans un tampon d’index se trouvent avec les paramètres suivants :

-   *Offset* - le nombre d’octets entre le début du tampon et le premier index.
-   *StartIndexLocation* -le nombre d’octets entre le décalage et le premier sommet utilisé par l’appel de dessin approprié.
-   *IndexCount* -le nombre d’index à afficher.

Un tampon d’index peut combiner plusieurs bandes de triangles ou de lignes ([topologies de primitive](primitive-topologies.md)) en les séparant les unes des autres à l’aide d’un index de découpe de bandes. Un index de découpe de bandes permet de dessiner plusieurs bandes de lignes ou de triangles à l’aide d’un seul appel de dessin. Un index de découpe de bandes est la valeur maximale possible pour l’index (0xffff pour un index de 16 bits, 0xffffffff pour un index de 32 bits). L’index de découpe de bandes redéfinit l’ordre d’enroulement dans les primitives indexées, et il peut être utilisé pour éviter le recours aux triangles dégénérés qui, sinon, seraient nécessaires pour conserver un ordre d’enroulement correct dans une bande de triangles. L’illustration suivante montre un exemple d’index de découpe de bandes.

![illustration d’un index de découpe de bandes](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Mémoire tampon constante

Le logiciel Direct3D est doté d’un tampon permettant de fournir des constantes de nuanceur, ou tampon constant. D’un point de vue conceptuel, ce tampon ressemble à un tampon de sommet à un seul élément, comme illustré ci-dessous.

![illustration d’une mémoire tampon constante de nuanceur](images/d3d10-shader-resource-buffer.png)

Chaque élément stocke une constante de 1 à 4 composants, en fonction du format des données stockées.

Les tampons constants réduisent la bande passante requise pour mettre à jour les constantes de nuanceur en permettant à ces dernières d’être regroupées et validées simultanément, ce qui évite d’avoir à valider chaque constante séparément au moyen d’appels individuels.

Un nuanceur lit toujours les variables contenues dans un tampon constant directement par le nom de la variable, de la même façon que sont lues les variables qui ne font pas partie d’un tampon constant.

Chaque étape du nuanceur permet d’utiliser jusqu’à 15 mémoires tampons constantes ; chaque tampon peut contenir jusqu’à 4 096 constantes.

Utilisez un tampon constant pour stocker les résultats de l’étape de sortie de flux.

Consultez l’article [Constantes de nuanceur (DirectX HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581) pour obtenir un exemple de déclaration de tampon constant dans un nuanceur.

## <a name="span-idtextureresourcesspanspan-idtextureresourcesspanspan-idtextureresourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>Ressources de texture


Une ressource de texture est un ensemble de données structuré conçu pour stocker des texels. Contrairement aux tampons, les textures peuvent être filtrées par échantillons de textures, car elles sont lues par des unités de nuanceur. Le type de texture a une incidence sur la façon dont la texture est filtrée. Un texel représente la plus petite unité de texture pouvant être lue ou écrite par le pipeline. Chaque texel contient 1 à 4 composants, organisés dans un des formats DXGI (consultez [ **DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)).

Les textures sont créés en tant que ressource structurée, ce qui permet de connaître leur taille. Chaque texture peut toutefois être typée ou non typée au moment de la création de la ressource, dès lors que le type est intégralement spécifié à l’aide d’une vue lorsque la texture est liée au pipeline.

-   [Types de texture](#texture-types)
-   [Sous-ressources](#subresources)
-   [Vs forts. Typage faible](#typed)

### <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>Types de texture

Il existe plusieurs types de textures : 1D, 2D, 3D, chacun d'entre eux peut être créé avec ou sans les mipmaps. Direct3D prend également en charge les tableaux de textures et les textures échantillonnées plusieurs fois.

-   [Texture 1D](#texture1d-resource)
-   [Tableau de textures 1D](#texture1d-array-resource)
-   [Texture 2D et tableau de textures 2D](#texture2d-resource)
-   [Texture 3D](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>Texture 1D

Dans sa forme la plus simple, une texture 1D contient des données de texture qui peuvent être traitées par une coordonnée de texture unique ; elle peut être affichée sous la forme d’un tableau de texels, comme illustré ci-dessous.

![illustration d’une texture 1D](images/d3d10-1d-texture.png)

Chaque texel contient un certain nombre de composants colorimétriques, en fonction du format des données stockées. Pour ajouter davantage de complexité, vous pouvez créer une texture 1D avec des niveaux de mipmap, comme illustré ci-dessous.

![illustration d’une texture 1D avec des niveaux de mipmap](images/d3d10-resource-texture1d.png)

Un niveau de mipmap est une texture deux fois plus petite que le niveau immédiatement supérieur. Le niveau le plus élevé est celui qui contient plus d’informations, et chaque niveau suivant est plus petit ; pour un mipmap 1D, le plus petit niveau contient un texel. Les différents niveaux sont identifiés au moyen d’un index appelé niveau de détail (LOD, Level-of-Detail) ; vous pouvez utiliser l’index LOD pour accéder à une texture plus petite lors du processus de rendu d’une géométrie qui n’est pas très proche de la caméra.

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>Tableau de textures 1D

Le logiciel Direct3D 10 offre également une nouvelle structure de données destinée aux tableaux de textures. D’un point de vue conceptuel, un tableau de textures 1D ressemble à l’illustration suivante.

![illustration d’un tableau de textures 1D](images/d3d10-resource-texture1darray.png)

Ce tableau de textures contient trois textures. Chacune des trois textures présente une largeur de 5 (qui représente le nombre d’éléments dans la première couche). Chaque texture contient également un mipmap à 3 couches.

Dans Direct3D, tous les tableaux de textures sont homogènes ; cela signifie que chaque texture contenue dans un tableau de textures doit avoir le même format de données et la même taille (notamment au niveau de la largeur et du nombre de niveaux de mipmap). Vous pouvez créer des tableaux de textures de différentes tailles, dès lors que toutes les textures contenues dans chaque tableau sont de la même taille.

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>Texture 2D et tableau de textures 2D

Une ressource Texture2D contient une grille 2D de texels. Chaque texel est adressable par vecteur u, v. Dans la mesure où il s’agit d’une ressource de texture, il peut contenir des niveaux de mipmap et des sous-ressources. Une ressource de texture 2D entièrement remplie ressemble à l’illustration suivante.

![Illustration d’une ressource de texture 2D](images/d3d10-resource-texture2d.png)

Cette ressource de texture contient une seule texture 3x5 avec trois niveaux de mipmap.

Un tableau de ressources Texture2DArray est un tableau de textures 2D homogène ; autrement dit, toutes les textures ont le même format de données et les mêmes dimensions (niveaux de mipmap compris). Il est doté de la même disposition que le tableau de textures 1D, la seule différence étant que les textures contiennent désormais des données 2D ; il ressemble par conséquent à l’illustration suivante.

![illustration d’un tableau de ressources de textures 2D](images/d3d10-resource-texture2darray.png)

Ce tableau de textures contient trois textures ; chaque texture est de type 3x5 avec deux niveaux de mipmap.

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>À l’aide d’un Texture2DArray comme un Cube de Texture

Un cube de texture est un tableau de textures 2D qui contient 6 textures, une pour chaque face du cube. Un cube de texture entièrement rempli ressemble à l’illustration suivante.

![illustration d’un tableau de ressources de textures 2D représentant un cube de texture](images/d3d10-resource-texturecube.png)

Un tableau de textures 2D qui contient 6 textures peut être lu à partir des nuanceurs à l’aide des fonctions intrinsèques de mappage de cube, une fois que ces nuanceurs sont liés au pipeline à l’aide d’une vue de texture de cube. Les cubes de texture sont adressés à partir du nuanceur à l’aide d’un vecteur 3D qui part du centre du cube de texture.

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>Texture 3D

Une ressource Texture3D (également appelée texture volumique) contient un volume 3D de texels. Dans la mesure où il s’agit d’une ressource de texture, elle peut contenir des niveaux de mipmap. Une texture 3D entièrement remplie ressemble à l’illustration suivante.

![Illustration d’une ressource de texture 3D](images/d3d10-resource-texture3d.png)

Lorsqu’une section de mipmap de texture 3D est liée en tant que sortie de cible de rendu (avec une vue de cible de rendu), la texture 3D se comporte de la même façon qu’un tableau de textures 2D comportant n sections. La sélection de la section de rendu spécifique s’effectue lors de l’étape du nuanceur de géométrie.

Un tableau de textures 3D n’a pas de concept. Par conséquent, une sous-ressource de texture 3D est un niveau de mipmap unique.

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>Sous-ressources

L’API Direct3D référence des ressources entières ou des sous-ensembles de ressources. Pour spécifier une partie des ressources, Direct3D emploie le terme *sous-ressources*, qui désigne un sous-ensemble de ressource.

Un tampon est défini en tant que sous-ressource unique. Les textures sont un peu plus complexes, car il existe plusieurs types de textures différents (1D, 2D, etc.), dont certains prennent en charge les niveaux de mipmap et/ou les tableaux de textures. Pour commencer par le cas le plus simple, une texture 1D est définie comme une sous-ressource unique, comme illustré ci-dessous.

![illustration d’une texture 1D](images/d3d10-1d-texture.png)

Cela signifie que les tableaux de texels qui composent une texture 1D sont contenus dans une seule sous-ressource.

Si vous développez une texture 1D avec trois niveaux de mipmap, vous pouvez obtenir le résultat illustré ci-dessous.

![illustration d’une texture 1D avec des niveaux de mipmap](images/d3d10-resource-texture1d.png)

Vous pouvez considérer qu’il s’agit d’une seule texture constituée de trois sous-textures. Chaque sous-texture étant comptabilisée comme une sous-ressource, cette texture 1D contient 3 sous-ressources. Une sous-texture (ou sous-ressource) peut être indexée à l’aide du niveau de détail (LOD) pour une seule texture. Lors de l’utilisation d’un tableau de textures, l’accès à une sous-texture spécifique nécessite à la fois le niveau de détail et la texture concernée. L’API peut également combiner ces deux informations dans un index de sous-ressource unique commençant par zéro, comme illustré ci-dessous.

![illustration d’un index de sous-ressource commençant par zéro](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselectingsubresourcesspanspan-idselectingsubresourcesspanspan-idselectingsubresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>En sélectionnant sous-ressources

Certaines API accèdent à une ressource entière, d’autres à une partie d’une ressource. Les API qui accèdent à une partie d’une ressource utilisent généralement une description de la vue pour spécifier les sous-ressources auxquelles accéder.

Les figures suivantes illustrent les termes qui sont utilisés par une description de la vue lors de l’accès à un tableau de textures.

### <a name="span-idarrayslicespanspan-idarrayslicespanspan-idarrayslicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>Section de tableau

Si l’on considère un tableau de textures, chaque texture comportant des mipmaps, une section de tableau (représentée par le rectangle blanc) inclut une texture et toutes ses sous-textures, comme illustré ci-dessous.

![illustration d’une section de tableau](images/d3d10-resource-array-slice.png)

### <a name="span-idmipslicespanspan-idmipslicespanspan-idmipslicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>Section MIP

Une section de mip (représentée par le rectangle blanc) inclut un niveau de mipmap pour chacune des textures contenues dans un tableau, comme illustré ci-dessous.

![illustration d’une section de mip](images/d3d10-resource-mip-slice.png)

### <a name="span-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>En sélectionnant un Subresource unique

Vous pouvez utiliser ces deux types de sections pour sélectionner une seule sous-ressource, comme illustré ci-dessous.

![illustration de la sélection d’une ressource via une section de tableau et une section de mip](images/d3d10-resource-subresources-1.png)

### <a name="span-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>En sélectionnant plusieurs sous-ressources

Vous pouvez également utiliser ces deux types de sections avec le nombre de niveaux de mipmap et/ou le nombre de textures pour sélectionner plusieurs sous-ressources.

![illustration de la sélection de plusieurs sous-ressources](images/d3d10-resource-subresources-2.png)

Quel que soit le type de texture que vous utilisez, avec ou sans mipmaps, avec ou sans tableau de textures, des fonctions d’assistance sont souvent fournies pour calculer l’index d’une sous-ressource spécifique.

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Vs forts. Typage faible

La création d’une ressource entièrement typée restreint la ressource au format dans lequel elle a été créée. Cela permet à l’exécution d’optimiser l’accès, en particulier si la ressource est créée avec des indicateurs montrant qu’elle ne peut pas être mappée par l’application. Les ressources créées avec un type spécifique ne peuvent pas être réinterprétées à l’aide du mécanisme de vue.

Dans une ressource non typée, le type de données n’est pas connu lorsque la ressource est créée pour la première fois. L’application doit choisir parmi les formats non typés disponibles. Vous devez spécifier la taille de la mémoire à allouer et indiquer si l’exécution devra générer les sous-textures dans un mipmap.

Toutefois, le format de données exact (indiquant si la mémoire sera interprétée comme des nombres entiers, des valeurs à virgule flottante, des nombres entiers non signés, etc.) n’est pas déterminé jusqu’à ce que la ressource soit liée au pipeline à l’aide d’une vue. Le format de texture reste flexible jusqu’à ce que la texture soit liée au pipeline, c’est pourquoi la ressource est appelée « stockage faiblement typé ». Un stockage faiblement typé présente l’avantage de pouvoir être réutilisé ou réinterprété (dans un autre format), dans la mesure où le nombre de bits des composants du nouveau format correspond au nombre de bits de l’ancien format.

Un seule ressource peut être liée à plusieurs étapes du pipeline, dans la mesure où chacune de ces étapes est associée à une vue unique, qui qualifie intégralement les formats à chaque emplacement. Par exemple, une ressource créée avec un format non typé peut être utilisée simultanément comme format FLOAT et format UINT à différents emplacements du pipeline.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources](resources.md)
