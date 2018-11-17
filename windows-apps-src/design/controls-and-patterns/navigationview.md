---
author: jwmsft
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: Affichage de navigation
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d8164571644c4e8ef84cae896fb7dce2265219e5
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7165349"
---
# <a name="navigation-view"></a>Affichage de navigation

Le contrôle NavigationView fournit la navigation de niveau supérieur pour votre application. Elle s’adapte à différentes tailles d’écran et prend en charge les styles de navigation de _gauche_ et _en haut_ .

![navigation en haut](images/nav-view-header.png)<br/>
_Prend en charge l’affichage de navigation en haut et le volet de navigation de gauche ou menu_

> **API de la plateforme**: [Windows.UI.Xaml.Controls.NavigationView classe](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **API de bibliothèque de l’interface utilisateur de Windows**: [Microsoft.UI.Xaml.Controls.NavigationView classe](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Certaines fonctionnalités de NavigationView, par exemple, navigation _supérieure_ , requièrent Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou de la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

NavigationView est un contrôle de navigation adaptatif qui fonctionne bien pour:

- La fourniture d’une expérience de navigation cohérente dans toute votre application.
- Conservation de l’espace écran sur les fenêtres plus petites.
- Organisation d’accéder à de nombreuses catégories de navigation.

Pour les autres modèles de navigation, voir [les bases de conception de Navigation](../basics/navigation-basics.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/NavigationView">ouvrir l’application et voir l'objet NavigationView en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Les modes d’affichage

> La propriété PaneDisplayMode nécessite Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou de la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Vous pouvez utiliser la propriété PaneDisplayMode pour configurer les styles de navigation différentes ou modes d’affichage, pour l’objet NavigationView.

:::row:::
    :::column:::
    ### <a name="top"></a>Top
    Le volet est placé au-dessus du contenu.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de navigation supérieur](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

Nous vous recommandons de navigation _supérieure_ lorsque:

- Vous avez 5 ou moins de catégories de navigation de niveau supérieur sont tout aussi importants et n’importe quel navigation de niveau supérieur supplémentaires catégories se retrouver dans le menu de dépassement de liste déroulante sont considérés comme moins importants.
- Vous devez afficher toutes les options de navigation à l’écran.
- Vous souhaitez plus d’espace pour le contenu de votre application.
- Les icônes ne peut pas décrivent clairement les catégories de navigation de votre application.

:::row:::
    :::column:::
    ### <a name="left"></a>Vers la gauche
    Le volet est développé et positionné à gauche du contenu.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de volet de navigation de gauche développée](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Nous vous recommandons de navigation de _gauche_ lorsque:

- Vous avez des catégories de navigation de niveau supérieur tout aussi important de 5 à 10.
- Vous souhaitez que les catégories de navigation pour être très bien visible, avec moins d’espace pour d’autres contenus d’application.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    Le volet affiche uniquement les icônes jusqu'à ce qu’ouvre et est positionné à gauche du contenu.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de volet de navigation de gauche compact](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Seul le bouton de menu s’affiche jusqu'à ce que le volet est ouvert. Ouvert, il est placé à gauche du contenu.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de volet de navigation de gauche minimale](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automatique

Par défaut, PaneDisplayMode est défini sur Auto. En mode Auto, le mode de navigation s’adapte entre LeftMinimal lorsque la fenêtre est étroite, pour LeftCompact et puis de gauche que la fenêtre est plus large. Pour plus d’informations, consultez la section de [comportement ADAPTATIF](#adaptive-behavior) .

![Comportement adaptatif par défaut de navigation de gauche](images/displaymode-auto.png)<br/>
_Comportement adaptatif de navigation vue par défaut_

## <a name="anatomy"></a>Anatomie

Ces images montrent la disposition du volet de, en-tête et des zones de contenu du contrôle lorsque configuré pour la navigation _en haut_ ou _vers la gauche_ .

![Disposition d’affichage de navigation supérieur](images/topnav-anatomy.png)<br/>
_Disposition de navigation supérieur_

![Disposition d’affichage de navigation de gauche](images/leftnav-anatomy.png)<br/>
_Disposition de navigation de gauche_

### <a name="pane"></a>Volet

Vous pouvez utiliser la propriété PaneDisplayMode pour positionner le volet au-dessus du contenu ou à gauche du contenu.

Le volet NavigationView peut contenir:

- Objets [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) . Éléments de navigation pour accéder à des pages spécifiques.
- Objets [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) . Séparateurs pour regrouper les éléments de navigation. Définissez la propriété [Opacity](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity) sur 0 pour restituer le séparateur sous la forme d’espace.
- Objets [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) . En-têtes pour libeller des groupes d’éléments.
- Un contrôle [AutoSuggestBox](auto-suggest-box.md) facultatif permettant de recherche au niveau de l’application. Affectez le contrôle à la propriété [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) .
- Un point d’entrée facultatif pour les [paramètres d’application](../app-settings/app-settings-and-data.md). Pour masquer l’élément paramètres, définissez la propriété [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) sur **false**.

Il contient également le volet de gauche:

- Un bouton de menu pour activer/désactiver le volet ouvert et fermé. Sur les grandes fenêtres d’application lorsque le volet est ouvert, vous pouvez choisir de masquer ce bouton à l’aide de la propriété [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

Le mode de navigation comporte un bouton précédent est placée dans le coin supérieur gauche du volet. Toutefois, il ne pas automatiquement gérer la navigation vers l’arrière et ajouter du contenu à la pile back. Pour activer la navigation vers l’arrière, voir la [vers l’arrière navigation](#backwards-navigation) section.

Voici l’anatomie volet détaillées pour les postes de volet supérieur et gauche.

#### <a name="top-navigation-pane"></a>Volet de navigation supérieur

![Anatomie de volet supérieur affichage navigation](images/navview-pane-anatomy-horizontal.png)

1. En-têtes
1. Éléments de navigation
1. Séparateurs
1. AutoSuggestBox (facultatif)
1. Bouton de paramètres (facultatif)

#### <a name="left-navigation-pane"></a>Volet de navigation gauche

![Affichage de navigation gauche Anatomie du volet](images/navview-pane-anatomy-vertical.png)

1. Bouton Menu
1. Éléments de navigation
1. Séparateurs
1. En-têtes
1. AutoSuggestBox (facultatif)
1. Bouton de paramètres (facultatif)

#### <a name="pane-footer"></a>Pied de page volet

Vous pouvez placer du contenu de forme libre dans le pied de page du volet en l’ajoutant à la propriété [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) .

:::row:::
    :::column:::
    ![Navigation supérieure de volet pied de page](images/navview-freeform-footer-top.png)<br>
     _Pied de page volet supérieur_<br>
    :::column-end:::
    :::column:::
    ![Volet de navigation de gauche pied de page](images/navview-freeform-footer-left.png)<br>
    _Volet de gauche le pied de page_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>En-tête et du titre du volet

Vous pouvez placer du contenu de texte dans la zone d’en-tête volet en définissant la propriété [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) . Elle prend une chaîne et affiche le texte en regard du bouton de menu.

Pour ajouter du contenu non textuelles, par exemple, une image ou un logo, vous pouvez placer n’importe quel élément dans l’en-tête du volet en l’ajoutant à la propriété [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) .

Si PaneTitle et PaneHeader sont définies, le contenu est empilé horizontalement en regard du bouton de menu, avec le PaneTitle le plus proche au bouton de menu.

:::row:::
    :::column:::
    ![Navigation supérieure de volet en-tête](images/navview-freeform-header-top.png)<br>
     _En-tête du volet supérieur_<br>
    :::column-end:::
    :::column:::
    ![Volet de navigation de gauche en-tête](images/navview-freeform-header-left.png)<br>
    _En-tête du volet de gauche_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Volet de contenu

Vous pouvez placer du contenu de forme libre dans le volet en l’ajoutant à la propriété [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) .

:::row:::
    :::column:::
    ![Volet navigation supérieure contenue personnalisée](images/navview-freeform-pane-top.png)<br>
     _Contenu personnalisé volet supérieur_<br>
    :::column-end:::
    :::column:::
    ![Volet de contenu personnalisé nav à gauche](images/navview-freeform-pane-left.png)<br>
    _Contenu personnalisé de volet de gauche_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>En-tête

Vous pouvez ajouter un titre de la page en définissant la propriété [Header](/uwp/api/windows.ui.xaml.controls.navigationview.header) .

![Exemple de la zone d’en-tête de mode de navigation](images/nav-header.png)<br/>
_En-tête d’affichage de navigation_

La zone d’en-tête est alignée verticalement avec le bouton de navigation à la position du volet de gauche et se trouve sous le volet à la position du volet supérieur. Il possède une hauteur fixe de 52 px. Elle contient le titre de la page de la catégorie de navigation sélectionnée. L’en-tête est ancré sur le haut de la page et se comporte comme un point de découpage de défilement pour la zone de contenu.

L’en-tête est visible à tout moment que le NavigationView est en mode d’affichage Minimal. Vous pouvez choisir de masquer l’en-tête dans les autres modes, utilisés pour des fenêtres de largeur supérieure. Pour masquer l’en-tête, définissez la propriété [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) sur **false**.

### <a name="content"></a>Contenu

![Exemple de zone de contenu de mode de navigation](images/nav-content.png)<br/>
_Contenu de l’affichage navigation_

La zone de contenu présente la plupart des informations relatives à la catégorie de navigation sélectionnée.

Nous recommandons des marges de 12px pour votre zone de contenu lorsque NavigationView est en mode **Minimal** et 24 px dans le cas contraire.

## <a name="adaptive-behavior"></a>Comportement adaptatif

Par défaut, le mode de navigation change automatiquement son mode d’affichage selon la quantité d’espace d’écran à sa disposition. Les propriétés [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) et [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) spécifient les points d’arrêt à laquelle le mode d’affichage change. Vous pouvez modifier ces valeurs pour personnaliser le comportement de mode d’affichage adaptatif.

### <a name="default"></a>Par défaut

Lorsque PaneDisplayMode est défini sur sa valeur par défaut de **Auto**, le comportement ADAPTATIF consiste à afficher:

- Un volet de gauche développé sur les grandes fenêtres étroites (1008px ou une version ultérieure).
- A gauche, l’icône seule, le volet de navigation (LeftCompact) des largeurs de la fenêtre de taille moyenne (641 px à 1007 px).
- Uniquement un bouton de menu (LeftMinimal) sur les fenêtres étroites (640 pixels ou moins).

Pour plus d’informations sur les tailles de fenêtre pour un comportement ADAPTATIF, voir les [tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Comportement adaptatif par défaut de navigation de gauche](images/displaymode-auto.png)<br/>
_Comportement adaptatif de navigation vue par défaut_

### <a name="minimal"></a>Minimal

Courant ADAPTATIF deuxième consiste à utiliser un volet de gauche développé sur les grandes fenêtres étroites et un bouton de menu à la fois des largeurs de fenêtre de petites et moyennes.

Nous recommandons cette quand:

- Vous souhaitez plus d’espace pour le contenu de l’application des largeurs de plus petite fenêtre.
- Vos catégories de navigation ne peut pas être représentés clairement avec des icônes.

![Comportement ADAPTATIF minimal de navigation de gauche](images/adaptive-behavior-minimal.png)<br/>
_Comportement ADAPTATIF «minimal» d’affichage navigation_

Pour configurer ce comportement, définissez CompactModeThresholdWidth sur la largeur à laquelle vous souhaitez que le volet réduit. Ici, il est modifié à partir de la valeur par défaut de 640 à 1007. Vous devez également définir ExpandedModeThresholdWidth pour vous assurer que les valeurs ne sont pas en conflit.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Compact

Courant ADAPTATIF tiers consiste à utiliser un volet de gauche développé sur les grandes fenêtres étroites et un LeftCompact, icône seule, le volet de navigation à la fois des largeurs de fenêtre de petites et moyennes.

Nous recommandons cette quand:

- Il est important de toujours afficher toutes les options de navigation à l’écran.
- Vos catégories de navigation peuvent être représentés clairement avec des icônes.

![Comportement ADAPTATIF compact de navigation de gauche](images/adaptive-behavior-compact.png)<br/>
_Comportement ADAPTATIF «compact» affichage navigation_

Pour configurer ce comportement, définissez CompactModeThresholdWidth sur 0.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Aucun comportement ADAPTATIF

Pour désactiver le comportement adaptatif automatique, définissez PaneDisplayMode sur une valeur autre qu’automatique. Ici, elle est définie à LeftMinimal, afin que seuls le bouton de menu s’affiche, quel que soit la largeur de fenêtre.

![Barre de navigation de gauche aucun comportement ADAPTATIF](images/adaptive-behavior-none.png)<br/>
_Affichage de navigation avec PaneDisplayMode défini sur LeftMinimal_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Comme décrit précédemment dans la section de _modes d’affichage_ , vous pouvez définir le volet afin d’être toujours sur supérieur, toujours développé, toujours compact ou toujours minimal. Vous pouvez également gérer les modes d’affichage vous-même dans le code de votre application. Un exemple de ce s’affiche dans la section suivante.

### <a name="top-to-left-navigation"></a>En haut de la navigation de gauche

Lorsque vous utilisez la navigation en haut de votre application, les éléments de navigation se réduit en un menu de dépassement en tant que la baisse de largeur de fenêtre. Lors de la fenêtre de votre application est étroite, il peut fournir une meilleure expérience utilisateur pour basculer le PaneDisplayMode à partir du haut de la navigation LeftMinimal plutôt que de laisser tous les éléments se réduit en le menu de dépassement.

Nous recommandons l’utilisation de navigation supérieur sur les grandes tailles de fenêtres et de navigation de gauche sur les petites tailles de fenêtre lorsque:

- Vous avez un ensemble de tout aussi les catégories de navigation de niveau supérieur important s’affichent ensemble, telles que si une seule catégorie dans ce jeu ne tient pas à l’écran, vous réduisez de navigation de gauche à leur donner une importance égale.
- Vous souhaitez conserver en tant que contenu beaucoup d’espace que possible dans les tailles de petite fenêtre.

Cet exemple montre comment utiliser une propriété [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) et [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) pour basculer entre la navigation en haut et LeftMinimal.

![Exemple de comportement ADAPTATIF 1 haut ou vers la gauche](images/navigation-top-to-left.png)

```xaml
<Grid >
    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>

```

> [!TIP]
> Lorsque vous utilisez AdaptiveTrigger.MinWindowWidth, l’état visuel est déclenché lorsque la fenêtre est plus large que la largeur minimale spécifiée. Cela signifie que la valeur par défaut XAML définit la fenêtre étroite et le VisualState les modifications sont appliquées lors de la fenêtre devient plus large. La valeur par défaut PaneDisplayMode pour l’affichage de navigation est automatique, par conséquent, lorsque la largeur de la fenêtre est inférieure ou égale à CompactModeThresholdWidth, LeftMinimal navigation est utilisée. Lorsque la fenêtre devient plus large, le VisualState remplace la valeur par défaut, et navigation en haut est utilisée.

## <a name="navigation"></a>Navigation

Le mode de navigation n’effectue pas automatiquement toutes les tâches de navigation. Lorsque l’utilisateur appuie sur un élément de navigation, le mode de navigation montre cet élément comme étant sélectionné et déclenche un événement [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) . Si l’appui entraîne un nouvel élément sélectionné, un événement [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) est également déclenché.

Vous pouvez gérer ces événements soit pour effectuer des tâches liées à la navigation demandée. Celui que vous devez gérer dépend du comportement de que votre choix pour votre application. En règle générale, vous accédez à la page demandée et mettre à jour de l’en-tête d’affichage de navigation en réponse à ces événements.

**ItemInvoked** est déclenché chaque fois que l’utilisateur appuie sur un élément de navigation, même s’il est déjà sélectionné. (L’élément peut également être appelé avec une action équivalente à l’aide de la souris, clavier ou autres entrées. Pour plus d’informations, consultez [entrée et interactions](../input/index.md).) Si vous naviguez dans le Gestionnaire d’événements ItemInvoked, par défaut, la page sera rechargée, et une entrée dupliquée est ajoutée à la pile de navigation. Si vous naviguez lorsqu’un élément est appelé, vous devez interdire le rechargement de la page, ou vous assurer qu’une entrée en double n’est créée dans le backstack de navigation lorsque la page est rechargée. (Voir les exemples de code).

**SelectionChanged** peut être déclenché par un utilisateur appel d’un élément qui n’est pas actuellement sélectionné, ou en modifiant par programme l’élément sélectionné. Si la modification de la sélection se produit dans la mesure où un utilisateur appelé un élément, l’événement ItemInvoked se produit en premier. Si la modification de sélection est par programmation, ItemInvoked n’est pas déclenché.

### <a name="backwards-navigation"></a>Navigation vers l’arrière

NavigationView a un bouton précédent intégré; Toutefois, comme avec la navigation vers l’avant, il n’effectue pas vers l’arrière navigation automatiquement. Lorsque l’utilisateur appuie sur le bouton précédent, l’événement [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) est déclenché. Vous gérez cet événement pour effectuer la navigation vers l’arrière. Pour plus d’informations et exemples de code, voir [l’historique de Navigation et vers l’arrière navigation](../basics/navigation-history-and-backwards-navigation.md).

En mode Minimal ou Compact, le mode de navigation volet est ouvert sous un menu volant. Dans ce cas, en cliquant sur le bouton précédent sera fermer le volet et déclenche l’événement **PaneClosing** à la place.

Vous pouvez masquer ou désactiver le bouton précédent en définissant ces propriétés:

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): permet d’afficher et masquer le bouton précédent. Cette propriété prend une valeur de l’énumération [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) et est définie sur **Auto** par défaut. Lorsque le bouton est réduit, aucun espace n’est réservé pour lui dans la disposition.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): permet d’activer ou désactiver le bouton précédent. Vous pouvez lier les données de cette propriété à la propriété [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) de votre image de navigation. **BackRequested** n’est pas déclenché si **IsBackEnabled** a la **valeur false**.

:::row:::
    :::column:::
        ![Navigation view back button in the left navigation pane](images/leftnav-back.png)<br/>
        _The back button in the left navigation pane_
    :::column-end:::
    :::column:::
        ![Navigation view back button in the top navigation pane](images/topnav-back.png)<br/>
        _The back button in the top navigation pane_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Exemple de code

Cet exemple montre comment utiliser NavigationView avec un volet de navigation supérieur de grandes tailles de fenêtres et un volet de navigation de gauche sur les tailles de petite fenêtre. Il peut être adaptée à la navigation de gauche seulement en supprimant les paramètres de navigation _en haut_ dans le VisualStateManager.

L’exemple illustre une méthode recommandée pour configurer les données de navigation qui fonctionnent pour de nombreux scénarios courants. Il montre également comment implémenter la navigation avec la navigation au clavier et le bouton précédent de NavigationView vers l’arrière.

Ce code suppose que votre application contienne des pages avec les noms suivants pour naviguer vers: _page d’accueil_, _AppsPage_, _GamesPage_, _MusicPage_, _MyContentPage_et _paramètres_. Code de ces pages n’est pas affiché.

> [!IMPORTANT]
> Informations sur les pages de l’application sont stockées dans un [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple). Cette structure nécessite que la version minimale pour votre projet d’application doit être SDK 17763 ou une version ultérieure. Si vous utilisez la version WinUI de NavigationView pour cibler des versions antérieures de Windows 10, vous pouvez utiliser le [package NuGet de System.ValueTuple](https://www.nuget.org/packages/System.ValueTuple/) à la place.

> [!IMPORTANT]
> Ce code montre comment utiliser la version de la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/) de NavigationView. Si vous utilisez la version de la plateforme de NavigationView au lieu de cela, la version minimale pour votre projet d’application doit être SDK 17763 ou une version ultérieure. Pour utiliser la version de la plateforme, supprimez toutes les références à `muxc:`.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Grid>
    <muxc:NavigationView x:Name="NavView"
                         Loaded="NavView_Loaded"
                         ItemInvoked="NavView_ItemInvoked"
                         BackRequested="NavView_BackRequested">
        <muxc:NavigationView.MenuItems>
            <muxc:NavigationViewItem Tag="home" Icon="Home" Content="Home"/>
            <muxc:NavigationViewItemSeparator/>
            <muxc:NavigationViewItemHeader x:Name="MainPagesHeader"
                                           Content="Main pages"/>
            <muxc:NavigationViewItem Tag="apps" Content="Apps">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB3C;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="games" Content="Games">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7FC;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="music" Icon="Audio" Content="Music"/>
        </muxc:NavigationView.MenuItems>

        <muxc:NavigationView.AutoSuggestBox>
            <!-- See AutoSuggestBox documentation for
                 more info about how to implement search. -->
            <AutoSuggestBox x:Name="NavViewSearchBox" QueryIcon="Find"/>
        </muxc:NavigationView.AutoSuggestBox>

        <ScrollViewer>
            <Frame x:Name="ContentFrame" Padding="12,0,12,24" IsTabStop="True"
                   NavigationFailed="ContentFrame_NavigationFailed"/>
        </ScrollViewer>
    </muxc:NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <!-- Remove the next 3 lines for left-only navigation. -->
                    <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    <Setter Target="NavViewSearchBox.Width" Value="200"/>
                    <Setter Target="MainPagesHeader.Visibility" Value="Collapsed"/>
                    <!-- Leave the next line for left-only navigation. -->
                    <Setter Target="ContentFrame.Padding" Value="24,0,24,24"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

> [!IMPORTANT]
> Ce code montre comment utiliser la version de la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/) de NavigationView. Si vous utilisez la version de la plateforme de NavigationView au lieu de cela, la version minimale pour votre projet d’application doit être SDK 17763 ou une version ultérieure. Pour utiliser la version de la plateforme, supprimez toutes les références à `muxc`.

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private void ContentFrame_NavigationFailed(object sender, NavigationFailedEventArgs e)
{
    throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
}

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page
private readonly List<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code.
    NavView.MenuItems.Add(new muxc.NavigationViewItemSeparator());
    NavView.MenuItems.Add(new muxc.NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon((Symbol)0xF1AD),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    // Add handler for ContentFrame navigation.
    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default, so load home page.
    NavView.SelectedItem = NavView.MenuItems[0];
    // If navigation occurs on SelectionChanged, this isn't needed.
    // Because we use ItemInvoked to navigate, we need to call Navigate
    // here to load the home page.
    NavView_Navigate("home", new EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
    var goBack = new KeyboardAccelerator { Key = VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = VirtualKey.Left,
        Modifiers = VirtualKeyModifiers.Menu
    };
    altLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(altLeft);
}

private void NavView_ItemInvoked(muxc.NavigationView sender,
                                 muxc.NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.InvokedItemContainer != null)
    {
        var navItemTag = args.InvokedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

<!-- NavView_SelectionChanged is not used in this example, but is shown for completeness.
     You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
     but not both. -->
private void NavView_SelectionChanged(muxc.NavigationView sender,
                                      muxc.NavigationViewSelectionChangedEventArgs args)
{
    if (args.IsSettingsSelected == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.SelectedItemContainer != null)
    {
        var navItemTag = args.SelectedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

private void NavView_Navigate(string navItemTag, NavigationTransitionInfo transitionInfo)
{
    Type _page = null;
    if (navItemTag == "settings")
    {
        _page = typeof(SettingsPage);
    }
    else
    {
        var item = _pages.FirstOrDefault(p => p.Tag.Equals(navItemTag));
        _page = item.Page;
    }
    // Get the page type before navigation so you can prevent duplicate
    // entries in the backstack.
    var preNavPageType = ContentFrame.CurrentSourcePageType;

    // Only navigate if the selected page isn't currently loaded.
    if (!(_page is null) && !Type.Equals(preNavPageType, _page))
    {
        ContentFrame.Navigate(_page, null, transitionInfo);
    }
}

private void NavView_BackRequested(muxc.NavigationView sender,
                                   muxc.NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender,
                         KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed.
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == muxc.NavigationViewDisplayMode.Compact ||
         NavView.DisplayMode == muxc.NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
        NavView.SelectedItem = (muxc.NavigationViewItem)NavView.SettingsItem;
        NavView.Header = "Settings";
    }
    else if (ContentFrame.SourcePageType != null)
    {
        var item = _pages.FirstOrDefault(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<muxc.NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));

        NavView.Header =
            ((muxc.NavigationViewItem)NavView.SelectedItem)?.Content?.ToString();
    }
}
```

## <a name="navigation-view-customization"></a>Personnalisation de vue de navigation

### <a name="pane-backgrounds"></a>Arrière-plans de volet

Par défaut, le volet NavigationView utilise un arrière-plan différent selon le mode d’affichage:

- le volet est une couleur grise unie lorsqu’elle est développée sur la gauche, côte-à-côte avec le contenu (en mode de gauche).
- le volet utilise ACRYLIQUE dans l’application lorsqu’il est ouvert sous forme de superposition au-dessus de contenu (en mode Compact, un minimum ou supérieur).

Pour modifier l’arrière-plan du volet, vous pouvez remplacer les ressources de thème XAML utilisés pour restituer l’arrière-plan dans chaque mode. (Cette technique est utilisée au lieu d’une seule propriété PaneBackground pour prendre en charge des arrière-plans différents pour différents modes d’affichage).

Ce tableau indique quelle ressource de thème est utilisé dans chaque mode d’affichage.

| Mode d’affichage | Ressource de thème |
| ------------ | -------------- |
| Vers la gauche | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Top | NavigationViewTopPaneBackground |

Cet exemple montre comment substituer les ressources de thème dans App.xaml. Lorsque vous remplacez les ressources de thème, vous devez toujours fournir les dictionnaires de ressources «Default» et «HighContrast» au minimum et les dictionnaires pour «Clair» ou «Dark» ressources en fonction des besoins. Pour plus d’informations, voir [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Ce code montre comment utiliser la version de la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/) de AcrylicBrush. Si vous utilisez la version de la plateforme de AcrylicBrush au lieu de cela, la version minimale pour votre projet d’application doit être SDK 16299 ou une version ultérieure. Pour utiliser la version de la plateforme, supprimez toutes les références à `muxm:`.

```xaml
<Application
    <!-- ... -->
    xmlns:muxm="using:Microsoft.UI.Xaml.Media">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
                <ResourceDictionary>
                    <ResourceDictionary.ThemeDictionaries>
                        <ResourceDictionary x:Key="Default">
                            <!-- The "Default" theme dictionary is used unless a specific
                                 light, dark, or high contrast dictionary is provided. These
                                 resources should be tested with both the light and dark themes,
                                 and specific light or dark resources provided as needed. -->
                            <muxm:AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="LightSlateGray"
                                   TintOpacity=".6"/>
                            <muxm:AcrylicBrush x:Key="NavigationViewTopPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="{ThemeResource SystemAccentColor}"
                                   TintOpacity=".6"/>
                            <LinearGradientBrush x:Key="NavigationViewExpandedPaneBackground"
                                     StartPoint="0.5,0" EndPoint="0.5,1">
                                <GradientStop Color="LightSlateGray" Offset="0.0" />
                                <GradientStop Color="White" Offset="1.0" />
                            </LinearGradientBrush>
                        </ResourceDictionary>
                        <ResourceDictionary x:Key="HighContrast">
                            <!-- Always include a "HighContrast" dictionary when you override
                                 theme resources. This empty dictionary ensures that the 
                                 default high contrast resources are used when the user
                                 turns on high contrast mode. -->
                        </ResourceDictionary>
                    </ResourceDictionary.ThemeDictionaries>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

## <a name="related-topics"></a>Rubriques connexes

- [Classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maître/détails](master-details.md)
- [Notions de base sur la navigation](../basics/navigation-basics.md)
- [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md)
