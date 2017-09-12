---
author: mijacobs
title: "Tailles d’écran et points d’arrêt pour la conception réactive"
description: .
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
label: Screen sizes and break points
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: b56cdeeb9a3c3d3ca89e19d8057e3d93241e6c3c
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2017
---
#  <a name="screen-sizes-and-break-points-for-responsive-design"></a>Tailles d’écran et points d’arrêt pour la conception réactive

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Le nombre d’appareils cibles et de tailles d’écran dans l’écosystème Windows10 est trop élevé pour pouvoir optimiser votre interface utilisateur pour chacun d’eux. À la place, nous vous recommandons de concevoir une application pour plusieurs largeurs principales (également appelées «points d’arrêt»): 360, 640, 1024 et 1366 epx.

> [!TIP]
> Lors de la conception de l’application pour des points d’arrêt spécifiques, pensez à la quantité d’espace d’écran disponible pour votre application (fenêtre de l’application). Lorsque l’application s’exécute en mode plein écran, sa fenêtre a la même taille que l’écran, mais dans les autres cas, elle est plus petite.
 

Le tableau suivant décrit les différentes classes de taille et fournit des recommandations générales pour la personnalisation de ces classes de taille.

![Points d’arrêt de la conception réactive](images/rsp-design/rspd-breakpoints.png)

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Classe de taille</th>
<th align="left">petite</th>
<th align="left">moyenne</th>
<th align="left">grande</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">Taille d’écran standard (diagonale)</td>
<td style="vertical-align:top;">4&quot; à 6&quot;</td>
<td style="vertical-align:top;">7&quot; à 12&quot;, ou téléviseurs</td>
<td style="vertical-align:top;">13&quot; et supérieur</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Appareils standard</td>
<td style="vertical-align:top;">Téléphones</td>
<td style="vertical-align:top;">Phablettes, tablettes, téléviseurs</td>
<td style="vertical-align:top;">PC, ordinateurs portables, Surface Hub</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Tailles de fenêtre normales en pixels effectifs</td>
<td style="vertical-align:top;">320x569, 360x640, 480x854</td>
<td style="vertical-align:top;">960x540, 1024x640</td>
<td style="vertical-align:top;">1366x768, 1920x1080</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Points d’arrêt de largeur de fenêtre en pixels effectifs</td>
<td style="vertical-align:top;">640px maximum</td>
<td style="vertical-align:top;">641px à 1007px</td>
<td style="vertical-align:top;">1008px ou plus</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Recommandations générales</td>
<td style="vertical-align:top;"><ul>
<li>Centrez les éléments d’onglet.</li>
<li>Définissez une marge de 12px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.</li>
<li>Ancrez les [barres de l’application](../controls-and-patterns/app-bars.md) en bas de la fenêtre pour faciliter l’accessibilité.</li>
<li>Utiliser une colonne/zone à la fois</li>
<li>Utilisez une icône pour représenter la recherche (n’affichez pas de zone de recherche).</li>
<li>Placez le [volet de navigation](../controls-and-patterns/navigationview.md) en mode Superposition pour économiser l’espace à l’écran.</li>
<li>Si vous utilisez le [modèle Maître/Détails](../controls-and-patterns/master-details.md), utilisez le mode présentation empilée pour économiser l’espace d’écran.</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Alignez les éléments d’onglet à gauche.</li>
<li>Définissez une marge de 24px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.</li>
<li>Placez les éléments de commande comme les [barres de l’application](../controls-and-patterns/app-bars.md) en haut de la fenêtre de l’application.</li>
<li>Jusqu’à deuxcolonnes/zones</li>
<li>Affichez la zone de recherche.</li>
<li>Placez le [volet de navigation](../controls-and-patterns/navigationview.md) en mode partiel de manière à ce qu’une bande étroite d’icônes soit toujours affichée.</li>
<li>Pensez à personnaliser davantage votre application pour les [expériences télévisuelles](http://go.microsoft.com/fwlink/?LinkId=760736).</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Alignez les éléments d’onglet à gauche.</li>
<li>Définissez une marge de 24px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.</li>
<li>Placez les éléments de commande comme les [barres de l’application](../controls-and-patterns/app-bars.md) en haut de la fenêtre de l’application.</li>
<li>Jusqu’à troiscolonnes/zones</li>
<li>Affichez la zone de recherche.</li>
<li>Placez le [volet de navigation](../controls-and-patterns/navigationview.md) en mode ancré pour qu’il soit toujours affiché.</li>
</ul></td>
</tr>
</tbody>
</table>

Avec [**Continuum pour téléphones**](http://go.microsoft.com/fwlink/p/?LinkID=699431), une nouvelle expérience pour les appareils mobiles Windows10 compatibles, les utilisateurs peuvent connecter leurs téléphones à un moniteur, une souris et un clavier pour les utiliser comme des ordinateurs portables. N’oubliez pas cette nouvelle fonctionnalité lorsque vous concevez des points d’arrêt spécifiques: un téléphone mobile ne reste pas toujours dans la classe de petite taille.
 
