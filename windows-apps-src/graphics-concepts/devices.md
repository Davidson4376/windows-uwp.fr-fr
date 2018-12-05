---
title: Périphériques
description: Un périphérique Direct3D est le composant de rendu de Direct3D. Un périphérique encapsule et stocke l’état de rendu, exécute des transformations et des opérations d’éclairage, et rastérise une image sur une surface.
ms.assetid: BC903462-A32A-46BA-8411-FB294F5D2CD9
keywords:
- Périphériques
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9e49a9dcaa2638065946f01797cbea084a1432a6
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8695287"
---
# <a name="devices"></a>Périphériques


Un périphérique Direct3D est le composant de rendu de Direct3D. Un périphérique encapsule et stocke l’état de rendu, exécute des transformations et des opérations d’éclairage, et rastérise une image sur une surface.

D’un point de vue architectural, les périphériques Direct3D contiennent un module de transformation, un module d’éclairage et un module de rastérisation, comme illustré dans le diagramme ci-dessous.

![diagramme de l’architecture d’un périphérique direct3d](images/d3ddev.png)

Direct3D prend en charge deux principaux types de périphériques Direct3D:

-   Périphérique de couche d’abstraction matérielle (HAL) avec ombrage et rastérisation à accélération matérielle combinant un traitement de vertex matériel et logiciel
-   Périphérique de référence

Ces périphériques correspondent à deux pilotes distincts. Les périphériques logiciels et de référence sont représentés par des pilotes logiciels, tandis que le périphérique HAL est représenté par un pilote matériel. La méthode d’exploitation la plus courante de ces périphériques consiste à utiliser les périphériques HAL pour les applications distribuées, et les périphériques de référence pour le test de fonctionnalités. Ces derniers sont fournis par des tiers afin d’émuler des périphériques spécifiques, par exemple un matériel expérimental qui n’a pas encore été commercialisé.

Le périphérique Direct3D créé par une application doit correspondre aux capacités du matériel sur lequel l’application est en cours d’exécution. Direct3D fournit des fonctionnalités de rendu, soit en accédant à un matériel3D installé sur l’ordinateur, soit en émulant les capacités du matériel3D dans le logiciel. Par conséquent, Direct3D offre des périphériques à la fois pour l’accès au matériel et pour l’émulation logicielle.

Les périphériques à accélération matérielle offrent de meilleures performances que les périphériques logiciels. Le type de périphérique HAL est disponible sur toutes les cartes graphiques prises en charge par Direct3D. Dans la plupart des cas, les applications ciblent des ordinateurs qui disposent d’une accélération matérielle et s’appuient sur une émulation logicielle pour prendre en charge les ordinateurs moins élaborés.

À l’exception du périphérique de référence, les périphériques logiciels ne prennent pas toujours en charge les mêmes fonctionnalités qu’un périphérique matériel. Les applications doivent systématiquement rechercher les capacités du périphérique pour déterminer les fonctionnalités prises en charge.

Étant donné que le comportement des périphériques logiciels et de référence fournis avec Direct3D9 est identique à celui du périphérique HAL, le code d’application conçu pour fonctionner avec le périphérique HAL fonctionnera avec les périphériques logiciels ou de référence sans aucune modification. Malgré leur similitude de comportement, les périphériques logiciels ou de référence n’offrent pas les mêmes capacités que le périphérique HAL, et certains périphériques logiciels implémentent parfois un nombre de capacités sensiblement moins élevé.

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
<td align="left"><p><a href="device-types.md">Types de périphériques</a></p></td>
<td align="left"><p>Les types de périphériques Direct3D comprennent les périphériques de couche d’abstraction matérielle (HAL) et le module de rastérisation de référence.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="windowed-vs--full-screen-mode.md">Mode fenêtré ou plein écran</a></p></td>
<td align="left"><p>Les applications Direct3D peuvent s’exécuter dans deux modes: fenêtré ou plein écran. En <em>mode fenêtré</em>, l’application partage l’espace disponible sur l’écran du bureau avec toutes les applications en cours d’exécution. En <em>mode plein écran</em>, la fenêtre dans laquelle s’exécute l’application couvre la totalité du bureau et masque toutes les applications en cours d’exécution (y compris votre environnement de développement).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="lost-devices.md">Périphériques perdus</a></p></td>
<td align="left"><p>Un périphérique Direct3D peut présenter l’état opérationnel ou perdu. L’état <em>opérationnel</em> correspond à l’état normal du périphérique, dans lequel ce dernier s’exécute correctement et présente la totalité du rendu attendu. Le périphérique passe à l’état <em>perdu</em> lorsqu’un événement, tel que la perte du focus clavier dans une application en plein écran, empêche le rendu.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="swap-chains.md">Chaînes d’échange</a></p></td>
<td align="left"><p>Une chaîne d’échange est une collection de mémoires tampons utilisées pour la présentation des images à l’utilisateur. Chaque fois qu’une application présente une nouvelle image à afficher, la première mémoire tampon de la chaîne d’échange prend la place de la mémoire tampon affichée. Ce processus est désigné sous le terme d’<em>échange</em> ou d’<em>inversion</em>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="introduction-to-rasterization-rules.md">Présentation des règles de rastérisation</a></p></td>
<td align="left"><p>Il est fréquent que les points spécifiés pour les vertex ne correspondent pas précisément aux pixels à l’écran. Dans ce cas, Direct3D applique des règles de rastérisation de triangle afin de déterminer les pixels qui s’appliquent à un triangle spécifique.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage des graphismes Direct3D](index.md)

 

 




