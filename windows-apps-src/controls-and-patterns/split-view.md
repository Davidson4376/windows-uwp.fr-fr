---
author: Jwmsft
title: "Mode Fractionné"
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "Un contrôle de mode Fractionné inclut un volet avec une fonction développer/réduire ainsi qu’une zone de contenu."
label: Split view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 61ada7839f76df92041b606066f5cbd54448cd93

---
# <a name="split-view-control"></a>Contrôle de mode Fractionné

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Un contrôle de mode Fractionné inclut un volet pouvant être développé/réduit ainsi qu’une zone de contenu.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Classe SplitView**](https://msdn.microsoft.com/library/windows/apps/dn864360)</li>
</ul>
</div>

Voici un exemple de l’application Microsoft Edge utilisant SplitView pour afficher son Hub.

![Exemple de mode Fractionné Microsoft Edge](images/split_view_Edge.png)


 La zone de contenu du mode Fractionné est toujours visible. Le volet peut être développé ou réduit ou rester ouvert, et peut s’afficher à gauche ou à droite de la fenêtre d’application. Le volet comporte quatre modes :

-   **Superposition**

    Le volet est masqué jusqu’à ce qu’il soit ouvert. Lorsqu’il est ouvert, il recouvre la zone de contenu.

-   **Inclus**

    Le volet est toujours visible et ne recouvre pas la zone de contenu. Les zones de volet et de contenu divisent l’espace disponible à l’écran.

-   **Superposé compact (CompactOverlay)**

    Une mince partie du volet est toujours visible à l’état compact dans ce mode, sa largeur étant juste suffisante pour afficher les icônes. Par défaut, la largeur du volet fermé est de 48 pixels et peut être modifiée avec `CompactPaneLength`. Si le volet est ouvert, il se superpose à la zone de contenu.

-   **CompactInline (Inclus compact)**

    Une mince partie du volet est toujours visible à l’état compact dans ce mode, sa largeur étant juste suffisante pour afficher les icônes. Par défaut, la largeur du volet fermé est de 48 pixels et peut être modifiée avec `CompactPaneLength`. Si le volet est ouvert, l’espace disponible pour le contenu est réduit, le poussant en dehors.

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Le contrôle de mode Fractionné peut être utilisé pour créer un [volet de navigation](nav-pane.md). Pour créer ce modèle, ajoutez un bouton Développer/réduire (le bouton « hamburger ») et un affichage Liste représentant les éléments de navigation.

Le contrôle de mode Fractionné peut également servir à créer toute expérience « à tiroirs » (c’est-à-dire que les utilisateurs peuvent ouvrir et fermer le volet supplémentaire).

## <a name="create-a-split-view"></a>Créer une vue en mode Fractionné

Voici un contrôle SplitView avec un volet ouvert, qui s’affiche en ligne en regard du contenu.
```xaml
<SplitView IsPaneOpen="True"
           DisplayMode="Inline"
           OpenPaneLength="296">
    <SplitView.Pane>
        <TextBlock Text="Pane"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </SplitView.Pane>

    <Grid>
        <TextBlock Text="Content"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </Grid>
</SplitView>
```



## <a name="related-topics"></a>Rubriques connexes
* [Modèle de volet de navigation](nav-pane.md)
* [Affichage Liste](lists.md)
 

 



<!--HONumber=Dec16_HO2-->


