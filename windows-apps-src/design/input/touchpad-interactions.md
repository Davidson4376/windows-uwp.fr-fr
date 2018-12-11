---
Description: Create Universal Windows Platform (UWP) apps with intuitive and distinctive user interaction experiences that are optimized for touchpad but are functionally consistent across input devices.
title: Interactions du pavé tactile
ms.assetid: CEDEA30A-FE94-4553-A7FB-6C1FA44F06AB
label: Touchpad interactions
template: detail.hbs
keywords: pavé tactile, PTP, entrées tactiles, pointeur, entrées, interactions avec l’utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8dfc98127133790deba9274ddba7801bea34f9cd
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8885084"
---
# <a name="touchpad-design-guidelines"></a>Recommandations en matière de conception pour le pavé tactile


Concevez votre application afin que les utilisateurs puissent interagir avec elle par le biais d’un pavé tactile. Un pavé tactile combine l’entrée tactile multipoint indirecte et l’entrée de précision d’un dispositif de pointage comme la souris. Grâce à cette combinaison, le pavé tactile est adapté à l’interface utilisateur optimisée pour l’interaction tactile et aux cibles d’applications de productivité plus petites.

 

![Pavé tactile](images/input-patterns/input-touchpad.jpg)


Les interactions de pavé tactile nécessitent trois éléments:

-   Un pavé tactile standard ou un pavé tactile de précision Windows.

    Les pavés tactiles de précision sont optimisés pour les périphériques de plateforme Windows universelle (UWP). Ils permettent au système de gérer certains aspects de l’expérience de pavé tactile en mode natif, comme le suivi du doigt et la détection de la paume, pour une expérience plus cohérente sur tous les périphériques.

-   Le contact direct d’un ou plusieurs doigts sur le pavé tactile.
-   Déplacement des contacts tactiles (ou absence de déplacement, basé sur un seuil de temps).

Les données d’entrée fournies par le capteur du pavé tactile peuvent :

-   Être interprétées comme un mouvement physique de manipulation directe d’un ou plusieurs éléments d’interface utilisateur (par exemple, mouvement panoramique, rotation, redimensionnement ou déplacement). En revanche, l’interaction avec un élément par le biais de sa fenêtre de propriétés ou d’une autre boîte de dialogue est considérée comme étant une manipulation indirecte.
-   Faire office de méthode d’entrée alternative, à la manière d’une souris ou d’un stylet.
-   Compléter ou modifier des aspects d’autres méthodes d’entrée, par exemple en maculant un trait d’encre dessiné avec un stylet.

Un pavé tactile combine l’entrée tactile multipoint indirecte et l’entrée de précision d’un dispositif de pointage comme la souris. Fort de cette combinaison, le pavé tactile est adapté à l’interface utilisateur optimisée pour l’interaction tactile et aux cibles d’applications de productivité, généralement plus petites, et à l’environnement de bureau. Optimisez la conception de votre application UWP pour l’entrée tactile et tirez parti de la prise en charge par défaut du pavé tactile.

Outre la prise en charge intégrée de l’entrée tactile, nous vous recommandons d’utiliser l’événement [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968) pour fournir des commandes d’interface utilisateur accessibles par la souris grâce à la convergence des expériences d’interaction prises en charge par les pavés tactiles. Par exemple, utilisez les boutons Précédent et Suivant pour permettre aux utilisateurs de tourner les pages de contenu ou d’effectuer un mouvement panoramique de ce contenu.

Les mouvements et les instructions abordés ici permettent de vérifier que votre application prend en charge les entrées du pavé tactile de façon transparente, et avec un minimum de code.

## <a name="the-touchpad-language"></a>Langue du pavé tactile


Un ensemble concis d’interactions avec le pavé tactile est utilisé de façon uniforme dans l’ensemble du système. Optimisez votre application pour les entrées tactiles et de la souris. Ce langage confère un aspect familier à votre application, qui sera plus facile à appréhender. Les utilisateurs n’en seront que plus confiants.

Le pavé tactile de précision permet aux utilisateurs de définir bien plus de mouvements et de comportements d’interaction que le pavé tactile standard. Ces deux images illustrent les différents paramètres disponibles dans Paramètres &gt; Périphériques &gt; Souris et pavé tactile pour un pavé tactile standard et un pavé tactile de précision, respectivement.

![paramètres du pavé tactile standard](images/mouse-touchpad-settings-standard.png)

<sup>Paramètres\\ du pavé tactile\\ standard</sup>

![paramètres du pavé tactile de précision Windows](images/mouse-touchpad-settings-ptp.png)

<sup>Paramètres\\ du pavé tactile\\ de précision\\ Windows</sup>

Voici quelques exemples de mouvements optimisés pour le pavé tactile qui permettent d’effectuer des tâches courantes.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Terme</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Appui avec trois doigts</p></td>
<td align="left"><p>Préférence de l’utilisateur pour effectuer une recherche avec <strong>Cortana</strong> ou pour afficher le <strong>centre de notifications</strong>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Glissement avec trois doigts</p></td>
<td align="left"><p>Préférence de l’utilisateur pour ouvrir les applications actives sur le bureau virtuel, afficher le bureau, ou basculer entre des applications ouvertes.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Appui avec un seul doigt pour l’action principale</p></td>
<td align="left"><p>Appuyez avec un seul doigt sur un élément pour appeler son action principale (par exemple, le lancement d’une application ou l’exécution d’une commande).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Appui avec deux doigts pour effectuer un clic droit</p></td>
<td align="left"><p>Appuyez avec deux doigts simultanément sur un élément pour le sélectionner et afficher les commandes contextuelles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Glissement avec deux doigts pour effectuer un mouvement panoramique</p></td>
<td align="left"><p>Le glissement est principalement utilisé pour les interactions de type panoramique mais peut également l’être pour le déplacement, le dessin ou l’écriture.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Pincer et étirer pour agrandir</p></td>
<td align="left"><p>Les mouvements de pincement et d’étirement sont souvent utilisés pour le redimensionnement et le zoom sémantique.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Appuyer avec un seul doigt et faire glisser pour réorganiser</p></td>
<td align="left"><p>Faites glisser un élément.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Appuyer avec un seul doigt et faites-le glisser pour sélectionner du texte</p></td>
<td align="left"><p>Appuyez sur le texte à sélectionner et faites glisser le doigt pour le sélectionner. Appuyez deux fois pour sélectionner un mot.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Zone de clic avec les boutons gauche et droit</p></td>
<td align="left"><p>Émulez la fonctionnalité de boutons gauche et droit d’un dispositif de pointage.</p></td>
</tr>
</tbody>
</table>

 

## <a name="hardware"></a>Matériel


Interrogez les fonctionnalités de la souris ([**MouseCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225626)) pour identifier les aspects de l’interface utilisateur de votre application auxquels le pavé tactile peut accéder directement. Nous vous conseillons de fournir une interface utilisateur pour les entrées tactiles et de la souris.

Pour plus d’informations concernant l’interrogation des fonctionnalités du périphérique, voir [Identifier des périphériques d’entrée](identify-input-devices.md).

## <a name="visual-feedback"></a>Retour visuel


-   Quand des événements de déplacement ou de pointage permettent de détecter un curseur de pavé tactile, affichez une interface utilisateur spécifique à la souris pour indiquer la fonctionnalité exposée par l’élément. Si le curseur de pavé tactile ne bouge pas pendant un certain temps ou si l’utilisateur commence une interaction tactile, estompez progressivement l’interface utilisateur du pavé tactile. Cela maintient l’interface utilisateur propre et aérée.
-   N’utilisez pas le curseur pour le retour de pointage, car le retour fourni par l’élément est suffisant (voir Curseurs, ci-dessous).
-   N’affichez pas de retour visuel si un élément ne prend pas en charge l’interaction (par exemple, du texte statique).
-   N’utilisez pas de rectangles de sélection avec les interactions de pavé tactile. Réservez ceux-ci aux interactions avec le clavier.
-   Affichez un retour visuel simultanément pour tous les éléments qui représentent la même cible d’entrée.

Pour obtenir des conseils plus généraux concernant le retour visuel, voir [Recommandations en matière de retour visuel](https://msdn.microsoft.com/library/windows/apps/hh465342).

## <a name="cursors"></a>Curseurs


Un ensemble de curseurs standard est disponible pour servir de pointeurs de pavé tactile. Ces derniers sont utilisés pour indiquer l’action principale d’un élément.

Chaque curseur standard possède une image par défaut correspondante qui lui est associée. L’utilisateur ou une application peut remplacer à tout moment l’image par défaut associée à n’importe quel curseur standard. Les applications UWP spécifient une image de curseur par le biais de la fonction [**PointerCursor**](https://msdn.microsoft.com/library/windows/apps/br208273).

Si vous avez besoin de personnaliser le curseur de la souris:

-   Utilisez toujours le curseur en forme de flèche (![Curseur en forme de flèche](images/cursor-arrow.png)) pour les éléments interactifs. N’utilisez pas le curseur en forme de main (![Curseur en forme de main](images/cursor-pointinghand.png)) pour les liens ou pour d’autres éléments interactifs. À la place, utilisez les effets de pointage (décrits précédemment).
-   Utilisez le curseur texte (![Curseur texte](images/cursor-text.png)) pour le texte sélectionnable.
-   Utilisez le curseur de déplacement (![Curseur de déplacement](images/cursor-move.png)) lorsque l’action principale correspond à un déplacement (par exemple, un glisser-déplacer ou un rognage). N’utilisez pas le curseur de déplacement pour des éléments lorsque l’action principale correspond à une navigation (tels que les vignettes de l’écran de démarrage).
-   Utilisez les curseurs de redimensionnement horizontal, vertical et diagonal (![Curseur de redimensionnement vertical](images/cursor-vertical.png), ![Curseur de redimensionnement horizontal](images/cursor-horizontal.png), ![Curseur de redimensionnement diagonal (du coin inférieur gauche au coin supérieur droit)](images/cursor-diagonal2.png), ![Curseur de redimensionnement diagonal (du coin supérieur gauche au coin inférieur droit)](images/cursor-diagonal1.png)) lorsqu’un objet est redimensionnable.
-   Utilisez les curseurs en forme de main de saisie (![Curseur en forme de main de saisie (ouverte)](images/cursor-pan1.png), ![Curseur en forme de main de saisie (fermée)](images/cursor-pan2.png)) lors d’un mouvement panoramique de contenu au sein d’une zone de dessin fixe (telle qu’une carte).

## <a name="related-articles"></a>Articles connexes


* [Gérer les entrées du pointeur](handle-pointer-input.md)
* [Identifier des périphériques d’entrée](identify-input-devices.md)
**Exemples**
* [Exemple d’entrée de base](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple de mode d’interaction utilisateur](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**Exemples d’archive**
* [Entrée : exemple de fonctionnalités de périphériques](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrée : exemple d’événements d’entrée utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Exemple de zoom, de panoramique et de défilement XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrée : mouvements et manipulations avec GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 



