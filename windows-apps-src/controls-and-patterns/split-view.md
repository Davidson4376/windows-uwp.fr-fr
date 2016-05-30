---
author: Jwmsft
title: Mode Fractionné
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: Un contrôle de mode Fractionné inclut un volet, qui peut être visible ou masqué, ainsi qu’une zone de contenu.
label: Split view
template: detail.hbs
---

# Recommandations en matière de contrôles SplitView



**API importantes**

-   [**Classe SplitView (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**Objet SplitView (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

Un contrôle de mode Fractionné inclut un volet, qui peut être visible ou masqué, ainsi qu’une zone de contenu. La zone de contenu est toujours visible. Le volet peut être visible ou masqué ; il est susceptible de s’afficher depuis le côté gauche ou droit de la fenêtre. Le volet comporte quatre modes :

-   **Superposition**

    Le volet est masqué jusqu’à ce qu’il soit ouvert. Lorsqu’il est ouvert, il recouvre la zone de contenu.

-   **Inclus**

    Le volet est toujours visible et ne recouvre pas la zone de contenu. Les zones de volet et de contenu divisent l’espace disponible à l’écran.

-   **Superposé compact (CompactOverlay)**

    Une mince partie du volet est toujours visible à l’état compact dans ce mode, sa largeur étant juste suffisante pour afficher les icônes. Par défaut, la largeur du volet fermé est de 48 pixels et peut être modifiée avec `CompactPaneLength`. Si le volet est ouvert, il se superpose à la zone de contenu.

-   **CompactInline (Inclus compact)**

    Une mince partie du volet est toujours visible à l’état compact dans ce mode, sa largeur étant juste suffisante pour afficher les icônes. Par défaut, la largeur du volet fermé est de 48 pixels et peut être modifiée avec `CompactPaneLength`. Si le volet est ouvert, l’espace disponible pour le contenu est réduit, le poussant en dehors.

## <span id="Is_this_the_right_control_"></span><span id="is_this_the_right_control_"></span><span id="IS_THIS_THE_RIGHT_CONTROL_"></span>Est-ce le contrôle approprié ?

Le contrôle de mode Fractionné peut être utilisé pour créer un [volet de navigation](nav-pane.md). Pour créer ce modèle, ajoutez un bouton Développer/réduire (le bouton « hamburger ») et un affichage Liste représentant les éléments de navigation.

Le contrôle de mode Fractionné peut également servir à créer toute expérience « à tiroirs » (c’est-à-dire que les utilisateurs peuvent ouvrir et fermer le volet supplémentaire).

## <span id="Examples"></span><span id="examples"></span><span id="EXAMPLES"></span>Exemples

Par défaut, le contrôle de mode Fractionné correspond à un conteneur de base. Voici un exemple de l’application Microsoft Edge utilisant SplitView pour afficher son Hub.

![Exemple de mode Fractionné Microsoft Edge](images/split_view_Edge.png)



## <span id="related_topics"></span>Rubriques connexes


* [Modèle de volet de navigation](nav-pane.md)
* [Affichage Liste](lists.md)
 

 


<!--HONumber=May16_HO2-->


