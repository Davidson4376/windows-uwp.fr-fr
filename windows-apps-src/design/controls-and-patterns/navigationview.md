---
Description: NavigationView est un contrôle adaptatif qui implémente les modèles de navigation de niveau supérieur pour votre application.
title: Affichage de navigation
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4ba3a45701d82ad0b43591469bf390190ec18db0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642224"
---
# <a name="navigation-view"></a>Affichage de navigation

Le contrôle NavigationView fournit la navigation de niveau supérieur pour votre application. Il s’adapte à un large éventail de tailles d’écran et prend en charge les _haut_ et _gauche_ styles de navigation.

![navigation en haut](images/nav-view-header.png)<br/>
_Vue de la navigation prend en charge à la fois haut et le volet de navigation gauche ou le menu_

> **API de plateforme**: [Classe de Windows.UI.Xaml.Controls.NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **API de bibliothèque de l’interface utilisateur de Windows**: [Classe de Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Certaines fonctionnalités de NavigationView, tel que _haut_ navigation, nécessitent Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou le [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

NavigationView est un contrôle de navigation adaptatif qui fonctionne bien pour :

- En fournissant une expérience de navigation cohérente tout au long de votre application.
- Préservation écran sur windows plus petits.
- Organiser l’accès à de nombreuses catégories de navigation.

Pour les autres modèles de navigation, consultez [principes fondamentaux de conception de Navigation](../basics/navigation-basics.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/NavigationView">ouvrir l’application et voir l'objet NavigationView en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Modes d’affichage

> La propriété PaneDisplayMode nécessite Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou le [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Vous pouvez utiliser la propriété PaneDisplayMode pour configurer les styles de navigation différent ou modes d’affichage, de NavigationView.

:::row:::
    :::column:::
    ### <a name="top"></a>Haut
    Le volet est positionné au-dessus du contenu.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de navigation supérieure](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

Nous vous recommandons de _haut_ navigation lorsque :

- Vous avez 5 ou moins de catégories de navigation de niveau supérieur sont tout aussi importants et aucune navigation de niveau supérieur supplémentaire catégories qui se retrouvent dans le menu déroulant de dépassement de capacité sont considérés comme moins importants.
- Vous devez afficher toutes les options de navigation à l’écran.
- Vous souhaitez davantage d’espace pour le contenu de votre application.
- Icônes ne peut pas décrire clairement les catégories de navigation de votre application.

:::row:::
    :::column:::
    ### <a name="left"></a>Left (Gauche)
    Le volet est développé et positionné à gauche du contenu.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de volet de navigation gauche développé](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Nous vous recommandons de _gauche_ navigation lorsque :

- Vous avez des catégories de navigation de niveau supérieur tout aussi important de 5 à 10.
- Vous souhaitez que les catégories de navigation pour être très important, avec moins d’espace pour un autre contenu de l’application.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    Le volet affiche uniquement les icônes jusqu'à ce qu’ouvert et est positionnée à gauche du contenu.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de volet de navigation de gauche compact](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Seul le bouton de menu est affiché jusqu'à ce que le volet s’ouvre. Ouvert, il est placé à gauche du contenu.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de volet de navigation de gauche minimal](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Auto

Par défaut, PaneDisplayMode est définie sur Auto. En mode Auto, le mode de navigation s’adapte entre LeftMinimal lorsque la fenêtre est étroite, pour LeftCompact, puis de gauche à mesure que la fenêtre augmente plus large. Pour plus d’informations, consultez le [comportement adaptive](#adaptive-behavior) section.

![Comportement de navigation de gauche par défaut ADAPTATIF](images/displaymode-auto.png)<br/>
_Navigation vue adaptatif par défaut_

## <a name="anatomy"></a>Anatomie

Ces images correspondent à la disposition du volet, en-tête, les zones de contenu du contrôle lorsque configuré pour _haut_ ou _gauche_ navigation.

![Disposition de vue de navigation supérieure](images/topnav-anatomy.png)<br/>
_Mise en page de navigation supérieure_

![Disposition de vue de navigation gauche](images/leftnav-anatomy.png)<br/>
_Interface de navigation gauche_

### <a name="pane"></a>Volet

Vous pouvez utiliser la propriété PaneDisplayMode pour positionner le volet au-dessus du contenu ou à gauche du contenu.

Le volet de NavigationView peut contenir :

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) objets. Éléments de navigation pour parcourir les pages spécifiques.
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) objets. Séparateurs pour regrouper des éléments de navigation. Définir le [opacité](/uwp/api/windows.ui.xaml.uielement.opacity) 0 pour restituer le séparateur d’espace à la propriété.
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) objets. En-têtes pour l’étiquetage des groupes d’éléments.
- Facultatif [AutoSuggestBox](auto-suggest-box.md) contrôle pour permettre la recherche au niveau de l’application. Affecter le contrôle à la [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) propriété.
- Un point d’entrée facultatif pour les [paramètres d’application](../app-settings/app-settings-and-data.md). Pour masquer l’élément de paramètres, définissez la [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) propriété **false**.

Le volet gauche contient également :

- Un bouton de menu pour activer/désactiver le volet ouvert et fermé. Sur les grandes fenêtres d’application lorsque le volet est ouvert, vous pouvez choisir de masquer ce bouton à l’aide de la propriété [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

La vue de la navigation a un bouton précédent est placé dans le coin supérieur gauche du volet. Toutefois, il ne sont pas automatiquement gérer la navigation vers l’arrière et ajouter du contenu à la pile de retour. Pour activer la navigation vers l’arrière, consultez le [descendante navigation](#backwards-navigation) section.

Voici l’anatomie du volet détaillé pour les positions du volet supérieur et gauche.

#### <a name="top-navigation-pane"></a>Volet de navigation supérieure

![Anatomie de volet supérieur de mode de navigation](images/navview-pane-anatomy-horizontal.png)

1. En-têtes
1. Éléments de navigation
1. Séparateurs
1. AutoSuggestBox (facultatif)
1. Bouton Paramètres (facultatif)

#### <a name="left-navigation-pane"></a>Volet de navigation gauche

![Mode de navigation Anatomie du volet de gauche](images/navview-pane-anatomy-vertical.png)

1. Bouton Menu
1. Éléments de navigation
1. Séparateurs
1. En-têtes
1. AutoSuggestBox (facultatif)
1. Bouton Paramètres (facultatif)

#### <a name="pane-footer"></a>Pied de page de volet

Vous pouvez placer libre contenu dans le pied de page du volet en l’ajoutant à la [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) propriété.

:::row:::
    :::column:::
    ![Navigation supérieure du pied de page du volet](images/navview-freeform-footer-top.png)<br>
     _Pied de page du volet supérieur_<br>
    :::column-end:::
    :::column:::
    ![Volet de navigation de gauche du pied de page](images/navview-freeform-footer-left.png)<br>
    _Pied de page du volet gauche_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>En-tête et le titre du volet

Vous pouvez placer le contenu de texte dans la zone d’en-tête volet en définissant le [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) propriété. Il prend une chaîne et affiche le texte en regard du bouton de menu.

Pour ajouter du contenu non textuel, tel qu’une image ou un logo, vous pouvez placer n’importe quel élément dans l’en-tête du volet en l’ajoutant à la [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) propriété.

Si PaneTitle et PaneHeader sont définis, le contenu est empilé horizontalement en regard du bouton de menu, avec le PaneTitle le plus proche pour le bouton de menu.

:::row:::
    :::column:::
    ![Volet en-tête supérieur nav](images/navview-freeform-header-top.png)<br>
     _En-tête du volet supérieur_<br>
    :::column-end:::
    :::column:::
    ![Volet en-tête de navigation de gauche](images/navview-freeform-header-left.png)<br>
    _En-tête du volet de gauche_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Volet de contenu

Vous pouvez placer libre contenu dans le volet en l’ajoutant à la [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) propriété.

:::row:::
    :::column:::
    ![Volet navigation supérieure contenue personnalisée](images/navview-freeform-pane-top.png)<br>
     _Contenu personnalisé du volet supérieur_<br>
    :::column-end:::
    :::column:::
    ![Contenu personnalisé de volet de navigation de gauche](images/navview-freeform-pane-left.png)<br>
    _Contenu personnalisé du volet gauche_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>En-tête

Vous pouvez ajouter un titre de page en définissant le [en-tête](/uwp/api/windows.ui.xaml.controls.navigationview.header) propriété.

![Exemple de zone d’en-tête de mode de navigation](images/nav-header.png)<br/>
_En-tête de la vue navigation_

L’en-tête est aligné verticalement avec le bouton de navigation dans la position du volet gauche et se trouve sous le volet de la position du volet supérieur. Il a une hauteur fixe de 52 px. Elle contient le titre de la page de la catégorie de navigation sélectionnée. L’en-tête est ancré sur le haut de la page et se comporte comme un point de découpage de défilement pour la zone de contenu.

L’en-tête est visible à tout moment que navigationview est en mode d’affichage Minimal. Vous pouvez choisir de masquer l’en-tête dans les autres modes, utilisés pour des fenêtres de largeur supérieure. Pour masquer l’en-tête, définissez le [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) propriété **false**.

### <a name="content"></a>Contenu

![Exemple de zone de contenu de vue de navigation](images/nav-content.png)<br/>
_Contenu de la vue navigation_

La zone de contenu présente la plupart des informations relatives à la catégorie de navigation sélectionnée.

Nous vous recommandons de marges 12px pour votre zone de contenu lorsque NavigationView est en **minimale** marges mode et 24 px sinon.

## <a name="adaptive-behavior"></a>Comportement adaptatif

Par défaut, la vue navigation modifie automatiquement son mode d’affichage selon la quantité d’espace d’écran à sa disposition. Le [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) et [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) propriétés spécifient les points d’arrêt, le mode d’affichage des modifications apportées. Vous pouvez modifier ces valeurs pour personnaliser le comportement de mode d’affichage adaptatif.

### <a name="default"></a>Default

Quand PaneDisplayMode est définie sur sa valeur par défaut **automatique**, le comportement ADAPTATIF consiste à afficher :

- Un volet de gauche développé des largeurs de grandes fenêtres (1008px ou supérieur).
- A laissé, icône uniquement, le volet de navigation (LeftCompact) des largeurs de fenêtre de taille moyenne (641px à 1007px).
- Uniquement un bouton de menu (LeftMinimal) des largeurs de petite fenêtre (640 px ou moins).

Pour plus d’informations sur les tailles de fenêtre pour le comportement adaptative, consultez [écran tailles et des points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Comportement de navigation de gauche par défaut ADAPTATIF](images/displaymode-auto.png)<br/>
_Navigation vue adaptatif par défaut_

### <a name="minimal"></a>Minimal

Un second modèle ADAPTATIF courantes consiste à utiliser un volet de gauche développé sur les largeurs de grandes fenêtres et un bouton de menu à la fois des largeurs de taille moyenne et petite fenêtre.

Nous vous recommandons cette quand :

- Vous souhaitez davantage d’espace pour le contenu de l’application des largeurs de fenêtre plus petite.
- Vos catégories de navigation ne peut pas être clairement représentées avec des icônes.

![Comportement ADAPTATIF minimal de navigation gauche](images/adaptive-behavior-minimal.png)<br/>
_Comportement d’ADAPTATIF « minimal » de la vue navigation_

Pour configurer ce comportement, la valeur CompactModeThresholdWidth la largeur souhaitée pour le volet à réduire. Ici, il est modifié à partir de la valeur par défaut de 640 à 1007. Vous devez également définir ExpandedModeThresholdWidth pour garantir que les valeurs ne sont pas en conflit.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Compact

Un troisième modèle ADAPTATIF courant consiste à utiliser un volet de gauche développé sur les largeurs de grandes fenêtres et un LeftCompact, icône seule, volet de navigation sur les deux largeurs de taille moyenne et petite fenêtre.

Nous vous recommandons cette quand :

- Il est important de toujours afficher toutes les options de navigation à l’écran.
- Vos catégories de navigation peuvent être clairement représentées avec des icônes.

![Comportement ADAPTATIF compact de navigation gauche](images/adaptive-behavior-compact.png)<br/>
_Navigation vue « compact » comportement d’adaptative_

Pour configurer ce comportement, définissez CompactModeThresholdWidth sur 0.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Aucun comportement ADAPTATIF

Pour désactiver le comportement automatique ADAPTATIF, valeur PaneDisplayMode est une valeur autre qu’automatique. Ici, il est défini à LeftMinimal, ainsi que le bouton de menu s’affiche, quel que soit la largeur de la fenêtre.

![Aucun comportement adaptatif de barre de navigation gauche](images/adaptive-behavior-none.png)<br/>
_Mode de navigation avec PaneDisplayMode défini sur LeftMinimal_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Comme décrit précédemment dans le _modes d’affichage_ section, vous pouvez définir le volet à être toujours visible, toujours développé, toujours compact ou toujours minimal. Vous pouvez également gérer les modes d’affichage vous-même dans le code de votre application. Un exemple de ceci est illustré dans la section suivante.

### <a name="top-to-left-navigation"></a>En haut à gauche

Lorsque vous utilisez la navigation supérieure dans votre application, les éléments de navigation réduire dans un menu de dépassement de capacité en tant que la diminution de la largeur de fenêtre. Lorsque votre fenêtre de l’application est étroite, il peut fournir une meilleure expérience utilisateur pour basculer le PaneDisplayMode à partir du haut pour la navigation de LeftMinimal, plutôt que de laisser tous les éléments réduire dans le menu de dépassement de capacité.

Nous recommandons l’utilisation de navigation supérieure sur les grandes tailles de fenêtres et de navigation de gauche sur la petite taille des fenêtres lorsque :

- Vous avez un ensemble de tout aussi important de navigation de premier niveau catégories s’affichent ensemble, telles que si une catégorie dans ce jeu ne tient pas sur l’écran, vous réduisez à la navigation gauche afin de leur donner une importance égale.
- Vous souhaitez conserver en tant que contenu beaucoup d’espace que possible dans les tailles petite fenêtre.

Cet exemple montre comment utiliser un [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) et [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) propriété pour basculer entre le haut et LeftMinimal la navigation.

![Exemple de comportement ADAPTATIF 1 supérieur ou gauche](images/navigation-top-to-left.png)

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
> Lorsque vous utilisez AdaptiveTrigger.MinWindowWidth, l’état visuel est déclenchée lorsque la fenêtre est plus large que la largeur minimale spécifiée. Cela signifie que la valeur par défaut XAML définit la fenêtre étroite et VisualState définit les modifications sont appliquées lorsque la fenêtre obtient plus large. La valeur par défaut PaneDisplayMode pour le mode de navigation est automatique, par conséquent, lorsque la largeur de la fenêtre est inférieure ou égale à CompactModeThresholdWidth, LeftMinimal la navigation est utilisée. Quand la fenêtre obtient plus large, VisualState remplace la valeur par défaut, et sert de navigation supérieure.

## <a name="navigation"></a>Navigation

Le mode de navigation n’effectue pas automatiquement les tâches de navigation. Lorsque l’utilisateur appuie sur un élément de navigation, le mode de navigation montre cet élément comme étant sélectionnée et déclenche une [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) événement. Si le tap aboutit à un nouvel élément sélectionné, un [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) événement est également déclenché.

Vous pouvez gérer l’événement pour effectuer des tâches liées à la navigation demandée. Celui que vous devez gérer dépend du comportement souhaité pour votre application. En règle générale, vous accédez à la page demandée et mettre à jour de l’en-tête de la vue navigation en réponse à ces événements.

**ItemInvoked** est déclenché chaque fois que l’utilisateur appuie sur un élément de navigation, même si elle est déjà sélectionnée. (L’élément peut également être appelé avec une action équivalente à l’aide de la souris, clavier ou autre entrée. Pour plus d’informations, consultez [entrées et interactions](../input/index.md).) Si vous naviguez dans le Gestionnaire de ItemInvoked, par défaut, la page sera rechargée, et une entrée dupliquée est ajoutée à la pile de navigation. Si vous accédez quand un élément est appelé, vous devez interdire recharger la page, ou vous assurer qu’une entrée dupliquée n’est pas créée dans la navigation backstack lors du rechargement de la page. (Voir les exemples de code).

**SelectionChanged** peut être déclenché par un utilisateur qui l’appelle un élément qui n’est pas actuellement sélectionné, ou en modifiant par programmation de l’élément sélectionné. Si le changement de sélection se produit, car un utilisateur appelé un élément, l’événement ItemInvoked se produit en premier. Si la modification de la sélection par programmation, ItemInvoked n’est pas déclenché.

### <a name="backwards-navigation"></a>Navigation vers l’arrière

NavigationView a un bouton précédent intégré ; Toutefois, comme avec la navigation vers l’avant, il n’effectue pas descendante navigation automatiquement. Lorsque l’utilisateur actionne le bouton précédent, le [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) événement est déclenché. Vous gérez cet événement pour effectuer la navigation vers l’arrière. Pour plus d’exemples de code et d’informations, consultez [l’historique de Navigation et vers l’arrière navigation](../basics/navigation-history-and-backwards-navigation.md).

En mode Minimal ou Compact, la vue navigation volet est ouverte en tant qu’un menu volant. En cliquant sur le bouton précédent dans ce cas, ferme le volet et déclencher le **PaneClosing** événement à la place.

Vous pouvez masquer ou désactiver le bouton précédent en définissant ces propriétés :

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): utiliser pour afficher et masquer le bouton précédent. Cette propriété accepte une valeur de la [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) énumération et est défini sur **automatique** par défaut. Lorsque le bouton est réduit, aucun espace n’est réservé pour celui-ci dans la disposition.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): utiliser pour activer ou désactiver le bouton précédent. Vous pouvez lier les données de cette propriété sur le [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) propriété de votre cadre de navigation. **BackRequested** n’est pas déclenché si **IsBackEnabled** est **false**.

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

Cet exemple montre comment vous pouvez utiliser NavigationView avec un volet de navigation supérieure sur les grandes tailles de fenêtres et un volet de navigation de gauche sur la taille de la petite fenêtre. Il peut être adapté à la navigation de gauche uniquement en supprimant le _haut_ les paramètres de navigation dans le VisualStateManager.

L’exemple montre une méthode recommandée pour définir les données de navigation qui fonctionneront pour de nombreux scénarios. Il montre également comment implémenter la navigation avec la navigation de clavier et le bouton de retour de NavigationView vers l’arrière.

Ce code suppose que votre application contienne des pages avec les noms suivants pour accéder à : _Page d’accueil_, _AppsPage_, _GamesPage_, _MusicPage_, _MyContentPage_, et _SettingsPage_ . Code de ces pages n’est pas affiché.

> [!IMPORTANT]
> Informations sur les pages de l’application sont stockées dans un [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple). Ce struct nécessite que la version minimale pour votre projet d’application doit être SDK 17763 ou supérieure. Si vous utilisez la version WinUI de NavigationView pour cibler des versions antérieures de Windows 10, vous pouvez utiliser la [package NuGet de System.ValueTuple](https://www.nuget.org/packages/System.ValueTuple/) à la place.

> [!IMPORTANT]
> Ce code montre comment utiliser le [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/) version de NavigationView. Si vous utilisez la version de plateforme de NavigationView au lieu de cela, la version minimale pour votre projet d’application doit être SDK 17763 ou supérieure. Pour utiliser la version de la plateforme, supprimez toutes les références à `muxc:`.

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
> Ce code montre comment utiliser le [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/) version de NavigationView. Si vous utilisez la version de plateforme de NavigationView au lieu de cela, la version minimale pour votre projet d’application doit être SDK 17763 ou supérieure. Pour utiliser la version de la plateforme, supprimez toutes les références à `muxc`.

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

Voici un [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/index) version de la **NavView_ItemInvoked** gestionnaire à partir de la C# exemple de code ci-dessus. La technique en C / c++ / WinRT Gestionnaire implique que vous consiste à stocker (dans la balise de la [ **NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)) le nom de type complet de la page à laquelle vous souhaitez accéder. Dans le gestionnaire, vous effectuer une conversion unboxing à cette valeur, convertir en un [ **Windows::UI::Xaml::Interop::TypeName** ](/uwp/api/windows.ui.xaml.interop.typename) de l’objet et l’utiliser pour accéder à la page de destination. Il n’est pas nécessaire pour la variable de mappage nommée `_pages` que vous voyez dans le C# exemple ; et vous serez en mesure de créer des tests unitaires confirmant que les valeurs à l’intérieur de vos balises sont d’un type valide. Consultez également [Boxing et unboxing des valeurs scalaires à IInspectable avec C / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

```cppwinrt
void MainPage::NavView_ItemInvoked(Windows::Foundation::IInspectable const & /* sender */, Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
{
    if (args.IsSettingsInvoked())
    {
        // Navigate to Settings.
    }
    else if (args.InvokedItemContainer())
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        pageTypeName.Name = unbox_value<hstring>(args.InvokedItemContainer().Tag());
        pageTypeName.Kind = Windows::UI::Xaml::Interop::TypeKind::Primitive;
        ContentFrame().Navigate(pageTypeName, nullptr);
    }
}
```

## <a name="navigation-view-customization"></a>Personnalisation des affichages de navigation

### <a name="pane-backgrounds"></a>Arrière-plans de volet

Par défaut, le volet de NavigationView utilise un arrière-plan différent selon le mode d’affichage :

- le volet est une couleur grise unie développée sur la gauche, côte à côte avec le contenu (en mode de gauche).
- le volet utilise ACRYLIQUE dans l’application lors de l’ouverture chevauchera contenu (en mode Compact, minimale ou supérieur).

Pour modifier l’arrière-plan du volet, vous pouvez remplacer les ressources de thème XAML utilisés pour restituer l’arrière-plan dans chaque mode. (Cette technique est utilisée au lieu d’une seule propriété PaneBackground pour prendre en charge des arrière-plans différents pour différents modes d’affichage).

Ce tableau montre les ressources de thème est utilisé dans chaque mode d’affichage.

| Mode d’affichage | Ressources de thème |
| ------------ | -------------- |
| Left (Gauche) | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Haut | NavigationViewTopPaneBackground |

Cet exemple montre comment remplacer les ressources de thème dans App.xaml. Lorsque vous substituez les ressources de thème, vous devez toujours fournir au minimum les dictionnaires de ressources « Default » et « Contraste élevé » et les dictionnaires pour « Clair » ou « Foncé » ressources en fonction des besoins. Pour plus d’informations, consultez [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Ce code montre comment utiliser le [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/) version de AcrylicBrush. Si vous utilisez la version de plateforme de AcrylicBrush au lieu de cela, la version minimale pour votre projet d’application doit être SDK 16299 ou supérieure. Pour utiliser la version de la plateforme, supprimez toutes les références à `muxm:`.

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

- [Classe de NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maître/détails](master-details.md)
- [Notions de base sur la navigation](../basics/navigation-basics.md)
- [Fluent Design pour une vue d’ensemble UWP](../fluent-design-system/index.md)
