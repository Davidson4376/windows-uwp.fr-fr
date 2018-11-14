---
title: Étape de l’assembleur d’entrée (IA)
description: L’étape de l’assembleur d’entrée (IA, Input Assembler) fournit au pipeline des données de primitive et de contiguïté, telles que des triangles, des lignes et des points, incluant des ID sémantiques pour améliorer l’efficacité des nuanceurs en réduisant le traitement aux primitives qui n’ont pas encore été traitées.
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- Étape de l’assembleur d’entrée (IA)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: def755f868c7ea30679f19877cec84b20faa44f5
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6267664"
---
# <a name="input-assembler-ia-stage"></a>Étape de l’assembleur d’entrée (IA)


L’étape de l’assembleur d’entrée (IA, Input Assembler) fournit au pipeline des données de primitive et de contiguïté, telles que des triangles, des lignes et des points, incluant des ID sémantiques pour améliorer l’efficacité des nuanceurs en réduisant le traitement aux primitives qui n’ont pas encore été traitées.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Objectif et cas d’utilisation


L’étape de l’assembleur d’entrée a pour vocation de lire les données de primitives (points, lignes et triangles) des mémoires tampons renseignées par les utilisateurs et d’assembler les données en primitives utilisées par d’autres étapes de pipelines, avant d’enfin associer les [valeurs générées par le système](https://msdn.microsoft.com/library/windows/desktop/bb509647) pour accroître l’efficacité des nuanceurs. Les valeurs générées par le système sont des chaînes textuelles, également appelées sémantiques. Les étapes programmables de nuanceur sont développées à partir d’un noyau de nuanceur commun, qui utilise les valeurs générées par le système (comme des ID de primitive, d’instance ou de vertex). De cette manière, l’étape de nuanceur peut dédier le traitement uniquement à ces primitives, instances ou vertex non encore traités.

L’étape IA peut assembler des vertex en différents [types de primitives](primitive-topologies.md) (comme des listes de lignes, des triangles ou des primitives avec voisinage). Les types de primitives tels qu’une liste de triangles avec voisinage et une liste de lignes avec voisinage prennent en charge l’[étape du nuanceur de géométrie](geometry-shader-stage--gs-.md).

Les informations de voisinage sont visibles par les applications uniquement dans un nuanceur de géométrie. Si un nuanceur de géométrie a été appelé avec un triangle avec voisinage, par exemple, les données d’entrée comportent 3vertex pour chaque triangle et 3vertex pour les données de voisinage par triangle.

Lorsque l’étapeIA est sollicitée pour la production des données de voisinage, les données d’entrée doivent inclure les données de voisinage. Il vous faudra éventuellement fournir un vertex factice (formant un triangle dégénéré), ou éventuellement marquer l’existence ou la non-existence du vertex sur l’un de ses attributs. Cette opération devra également être détectée et gérée par un nuanceur de géométrie, bien que l’élimination de la géométrie dégénérée intervient à l’étape du rastériseur.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


L’étape IA lit les données de la mémoire: données de primitives (points, lignes et/ou triangles) des mémoires tampons renseignées par les utilisateurs.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


L’étape IA assemble les données en primitives et associe les valeurs générées par le système, pour en définitive produire des primitives mises à profit par l’[étape du nuanceur de vertex](vertex-shader-stage--vs-.md), puis d’autres étapes de pipeline.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="primitive-topologies.md">Topologies de primitive</a></p></td>
<td align="left"><p>Direct3D prend en charge plusieurs topologies de primitives, qui définissent l’interprétation et le rendu des vertex par le pipeline, en listes de points, listes de lignes ou triangles.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="using-system-generated-values.md">Utilisation de valeurs générées par le système</a></p></td>
<td align="left"><p>Les valeurs générées par le système sont produites par l’étape d’assembleur d’entrée (reposant sur les <a href="https://msdn.microsoft.com/library/windows/desktop/bb509647">sémantiques</a> d’entrée fournies par l’utilisateur) afin d’accroître l’efficacité des opérations du nuanceur. L’association des données, comme un ID d’instance (visible par l’<a href="vertex-shader-stage--vs-.md">étape du nuanceur de vertex</a>), un ID de vertex (visible par le nuanceur de vertex) ou un ID de primitive (visible par l’<a href="geometry-shader-stage--gs-.md">étape du nuanceur de vertex</a>/<a href="pixel-shader-stage--ps-.md">du nuanceur de pixel</a>) permet à une étape ultérieure de nuanceur de rechercher ces valeurs système afin d’optimiser son traitement.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 




