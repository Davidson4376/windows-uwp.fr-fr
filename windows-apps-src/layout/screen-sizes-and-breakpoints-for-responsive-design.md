---
author: mijacobs
title: "Tailles d’écran et points d’arrêt pour la conception réactive"
description: .
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
label: Screen sizes and break points
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 5977b6253e2ee10fa0153d79053f89b705c2e1a3

---

#  Tailles d’écran et points d’arrêt pour la conception réactive

Le nombre d’appareils cibles et de tailles d’écran dans l’écosystème Windows10 est trop élevé pour pouvoir optimiser votre interface utilisateur pour chacun d’eux. À la place, nous vous recommandons de concevoir une application pour plusieurs largeurs principales (également appelées «points d’arrêt»): 360, 640, 1024 et 1366epx.

**Conseil** Lors de la conception pour des points d’arrêt spécifiques, pensez à la quantité d’espace d’écran disponible pour votre application (fenêtre de l’application). Lorsque l’application s’exécute en mode plein écran, sa fenêtre a la même taille que l’écran, mais dans les autres cas, elle est plus petite.
 

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
<td align="left">Taille d’écran standard (diagonale)</td>
<td align="left">4&quot; à 6&quot;</td>
<td align="left">7&quot; à 12&quot;, ou téléviseurs</td>
<td align="left">13&quot; et supérieur</td>
</tr>
<tr class="even">
<td align="left">Appareils standard</td>
<td align="left">Téléphones</td>
<td align="left">Phablettes, tablettes, téléviseurs</td>
<td align="left">PC, ordinateurs portables, Surface Hub</td>
</tr>
<tr class="odd">
<td align="left">Tailles de fenêtre normales en pixels effectifs</td>
<td align="left">320x569, 360x640, 480x854</td>
<td align="left">960x540, 1024x640</td>
<td align="left">1366x768, 1920x1080</td>
</tr>
<tr class="even">
<td align="left">Points d’arrêt de largeur de fenêtre en pixels effectifs</td>
<td align="left">640px maximum</td>
<td align="left">641px à 1007px</td>
<td align="left">1008px ou plus</td>
</tr>
<tr class="odd">
<td align="left" valign="top">Recommandations générales</td>
<td align="left" valign="top"><ul>
<li>Centrez les éléments d’onglet.</li>
<li>Définissez une marge de 12px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.</li>
<li>Ancrez les [barres de l’application](../controls-and-patterns/app-bars.md) en bas de la fenêtre pour faciliter l’accessibilité.</li>
<li>Utiliser une colonne/zone à la fois</li>
<li>Utilisez une icône pour représenter la recherche (n’affichez pas de zone de recherche).</li>
<li>Placez le [volet de navigation](../controls-and-patterns/nav-pane.md) en mode Superposition pour économiser l’espace à l’écran.</li>
<li>Si vous utilisez le [modèle Maître/Détails](../controls-and-patterns/master-details.md), utilisez le mode présentation empilée pour économiser l’espace d’écran.</li>
</ul></td>
<td align="left" valign="top"><ul>
<li>Alignez les éléments d’onglet à gauche.</li>
<li>Définissez une marge de 24px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.</li>
<li>Placez les éléments de commande comme les [barres de l’application](../controls-and-patterns/app-bars.md) en haut de la fenêtre de l’application.</li>
<li>Jusqu’à deuxcolonnes/zones</li>
<li>Affichez la zone de recherche.</li>
<li>Placez le [volet de navigation](../controls-and-patterns/nav-pane.md) en mode partiel de manière à ce qu’une bande étroite d’icônes soit toujours affichée.</li>
<li>Pensez à personnaliser davantage votre application pour les [expériences télévisuelles](http://go.microsoft.com/fwlink/?LinkId=760736).</li>
</ul></td>
<td align="left" valign="top"><ul>
<li>Alignez les éléments d’onglet à gauche.</li>
<li>Définissez une marge de 24px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.</li>
<li>Placez les éléments de commande comme les [barres de l’application](../controls-and-patterns/app-bars.md) en haut de la fenêtre de l’application.</li>
<li>Jusqu’à troiscolonnes/zones</li>
<li>Affichez la zone de recherche.</li>
<li>Placez le [volet de navigation](../controls-and-patterns/nav-pane.md) en mode ancré pour qu’il soit toujours affiché.</li>
</ul></td>
</tr>
</tbody>
</table>

Avec [**Continuum pour téléphones**](http://go.microsoft.com/fwlink/p/?LinkID=699431), une nouvelle expérience pour les appareils mobiles Windows10 compatibles, les utilisateurs peuvent connecter leurs téléphones à un moniteur, une souris et un clavier pour les utiliser comme des ordinateurs portables. N’oubliez pas cette nouvelle fonctionnalité lorsque vous concevez des points d’arrêt spécifiques: un téléphone mobile ne reste pas toujours dans la classe de petite taille.
 



<!--HONumber=Aug16_HO3-->


