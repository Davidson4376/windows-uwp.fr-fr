---
title: Ressources
description: Une ressource est une zone de mémoire accessible par le pipeline Direct3D.
ms.assetid: 2E68E5A8-83DA-4DC8-B7F3-B8988CF8090C
keywords:
- Ressources
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a56b03de29e110a2768ebe71f4e61d8099ca1cf8
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6460962"
---
# <a name="resources"></a>Ressources


Une ressource est une zone de mémoire accessible par le pipeline Direct3D. Pour permettre un accès efficace du pipeline à la mémoire, les données fournies au pipeline (géométrie d’entrée, ressources des nuanceurs et textures) doivent être stockées dans une ressource. Il existe 2types de ressources à partir desquelles l’ensemble des ressources Direct3D dérivent: une mémoire tampon et une texture. Jusqu’à 128ressources peuvent être actives à chaque étape du pipeline.

Chaque application crée généralement un grand nombre de ressources. Exemples de ressources: tampons de sommet, tampon d’index, tampon de constante textures et ressources de nuanceur. La façon dont les ressources peuvent être utilisées est déterminée par plusieurs options. Vous pouvez créer des ressources plus ou moins typées, contrôler si les ressources disposent d’un accès en lecture et écriture, et rendre des ressources accessibles à l’UC, au processeur graphique ou aux deux. Naturellement, vous devrez faire des compromis entre vitesse et fonctionnalité: plus vous accordez de ressources à une fonctionnalité, moins les performances sont optimales.

Étant donné qu’une application utilise souvent de nombreuses textures, Direct3D propose également un concept de tableau de textures pour simplifier la gestion. Un tableau de textures contient une ou plusieurs textures (toutes de même type et mêmes dimensions) qui peuvent être indexés à partir d’une application ou par les nuanceurs. Les tableaux de textures vous permettent d’accéder à de nombreuses textures via une interface unique avec plusieurs index. Vous pouvez créer autant de tableaux de textures que nécessaire pour gérer vos différents types de textures.

Une fois que vous avez créé les ressources pour votre application, vous connectez ou liez chaque ressource aux étapes du pipeline qui l’utiliseront. Cette opération est effectuée en appelant une API de liaison, qui prend un pointeur vers la ressource. Étant donné que plusieurs étapes du pipeline peuvent avoir besoin d’accéder à la même ressource, Direct3D propose un concept de vue de ressources. Une vue identifie la partie d’une ressource qui est accessible. Vous pouvez créer *m* vues ou une ressource et les lier à *n* étapes du pipeline, à condition que vous suiviez les règles de liaison pour une ressource partagée (dans le cas contraire, vous obtiendrez des erreurs au moment de la compilation).

Une vue de ressources fournit un modèle général d’accès à une ressource (par exemple, les textures ou les tampons). Vous pouvez utiliser une vue pour indiquer à quelles données l’application doit accéder et comment elle doit procéder. Par conséquent, les vues de ressources vous permettent de créer des ressources moins typées. En d’autres termes, vous pouvez créer des ressources pour une taille donnée au moment de la compilation, puis déclarer le type de données dans la ressource au moment où celle-ci est liée au pipeline. Les vues offrent de nombreuses possibilités pour utiliser les ressources. Par exemple, vous pouvez lire a posteriori les surfaces profondeur/gabarit dans le nuanceur, générer des cartes de cube dynamiques en transmettant une seule fois les données et générer simultanément le rendu de plusieurs sections d’un volume.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Article</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="resource-types.md">Types de ressource</a></p></td>
<td align="left"><p>Les différents types de ressources ont une disposition (ou un encombrement mémoire) distinct(e). Toutes les ressources utilisées par le pipeline Direct3D dérivent de deux types de ressources de base: les <a href="resource-types.md#buffer-resources">tampons</a> et les <a href="resource-types.md#texture-resources">textures</a>. Un tampon est un ensemble de données brutes (éléments); une texture est un ensemble de texels (éléments de texture).</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="choosing-a-resource.md">Choix d’une ressource</a></p></td>
<td align="left"><p>Une ressource est une collection de données utilisées par le pipeline 3D. La création des ressources et la définition de leur comportement constituent la première étape de programmation de votre application. Ce guide aborde les rubriques de base permettant de choisir les ressources dont a besoin votre application.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="copying-and-accessing-resource-data.md">Copie et consultation des données des ressources</a></p></td>
<td align="left"><p>Les indicateurs d’utilisation spécifient la manière dont l’application va utiliser les données de ressources, de manière à placer les ressources dans la zone de mémoire la plus performante possible. Les données de ressource sont copiées dans les ressources de sorte que l’UC ou le processeur graphique puisse y accéder sans affecter les performances.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-views.md">Affichages de texture</a></p></td>
<td align="left"><p>Dans Direct3D, les ressources de texture sont disponibles avec une vue, qui est un mécanisme d’interprétation matérielle d’une ressource de la mémoire. Une vue permet à une étape du pipeline spécifique d’accéder uniquement aux <a href="resource-types.md">sous-ressources</a> nécessaires, dans la représentation souhaitée par l’application.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées](coordinate-systems.md)

[Guide d’apprentissage des graphismes Direct3D](index.md)

[Règles en matière de virgule flottante](floating-point-rules.md)

[Conversion de types de données](data-type-conversion.md)
