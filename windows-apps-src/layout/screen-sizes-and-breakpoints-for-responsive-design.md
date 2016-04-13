---
title: 'Tailles d’écran et points d’arrêt pour la conception réactive'
description: .
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
label: Screen sizes and break points
template: detail.hbs
---

#  Tailles d’écran et points d’arrêt pour la conception réactive


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]



Le nombre d’appareils cibles et de tailles d’écran dans l’écosystème Windows 10 est trop élevé pour pouvoir optimiser votre interface utilisateur pour chacun d’eux. À la place, nous vous recommandons de concevoir une application pour plusieurs largeurs principales (également appelées « points d’arrêt ») : 360, 640, 1024 et 1366 epx.

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
<td align="left">320 x 569, 360 x 640, 480 x 854</td>
<td align="left">960 x 540, 1 024 x 640</td>
<td align="left">1 366 x 768, 1 920 x 1 080</td>
</tr>
<tr class="even">
<td align="left">Points d’arrêt de largeur de fenêtre en pixels effectifs</td>
<td align="left">640 px maximum</td>
<td align="left">641 px à 1 007 px</td>
<td align="left">1 008 px ou plus</td>
</tr>
<tr class="odd">
<td align="left" valign="top">Recommandations générales</td>
<td align="left" valign="top"><ul>
<li>Centrez les éléments d’onglet.</li>
<li>Définissez une marge de 12 px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.</li>
<li>Ancrez les [app bars](../controls-and-patterns/app-bars.md) en bas de la fenêtre pour faciliter l’accessibilité</li>
<li>Utiliser une colonne/zone à la fois</li>
<li>Utilisez une icône pour représenter la recherche (n’affichez pas de zone de recherche).</li>
<li>Placez le [navigation pane](../controls-and-patterns/nav-pane.md) en mode Superposition pour économiser l’espace à l’écran.</li>
<li>Si vous utilisez l’[master details pattern](../controls-and-patterns/master-details.md), utilisez le mode présentation empilée pour économiser l’espace d’écran.</li>
</ul></td>
<td align="left" valign="top"><ul>
<li>Alignez les éléments d’onglet à gauche.</li>
<li>Définissez une marge de 24 px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.</li>
<li>Placez les éléments de commande comme les [app bars](../controls-and-patterns/app-bars.md) en haut de la fenêtre de l’application.</li>
<li>Jusqu’à deux colonnes/zones</li>
<li>Affichez la zone de recherche.</li>
<li>Placez le [navigation pane](../controls-and-patterns/nav-pane.md) en mode partiel de manière à ce qu’une bande étroite d’icônes soit toujours affichée.</li>

</ul></td>
<td align="left" valign="top"><ul>
<li>Alignez les éléments d’onglet à gauche.</li>
<li>Définissez une marge de 24 px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.</li>
<li>Placez les éléments de commande comme les [app bars](../controls-and-patterns/app-bars.md) en haut de la fenêtre de l’application.</li>
<li>Jusqu’à trois colonnes/zones</li>
<li>Affichez la zone de recherche.</li>
<li>Placez le [navigation pane](../controls-and-patterns/nav-pane.md) en mode ancré pour qu’il soit toujours affiché.</li>
</ul></td>
</tr>
</tbody>
</table>

Avec [**Continuum pour téléphones**](http://go.microsoft.com/fwlink/p/?LinkID=699431), une nouvelle expérience pour les appareils mobiles Windows 10 compatibles, les utilisateurs peuvent connecter leurs téléphones à un moniteur, une souris et un clavier pour les utiliser comme des ordinateurs portables. N’oubliez pas cette nouvelle fonctionnalité lorsque vous concevez des points d’arrêt spécifiques : un téléphone mobile ne reste pas toujours dans la classe de petite taille.
 


<!--HONumber=Mar16_HO4-->


