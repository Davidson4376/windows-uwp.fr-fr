---
author: QuinnRadich
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: Affichage de navigation
template: detail.hbs
ms.author: quradic
ms.date: 06/25/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 99982e54bd9eebd6d6c34fa08c9f1c480b626a15
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4501533"
---
# <a name="navigation-view-preview-version"></a>Mode de navigation (version d’évaluation)

> **Il s’agit d’une version d’évaluation**: cet article décrit une nouvelle version du contrôle NavigationView qui est toujours en développement. Pour l’utiliser, vous devez maintenant [dernière build Insider de Windows et Kit de développement](https://insider.windows.com/for-developers/) ou la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Le contrôle NavigationView fournit la navigation de niveau supérieur pour votre application. Il s’adapte à différentes prend en charge des tailles d’écran plusieurs styles de navigation.

> **API de bibliothèque de l’interface utilisateur de Windows**: [Microsoft.UI.Xaml.Controls.NavigationView classe](/uwp/api/microsoft.ui.xaml.controls.navigationview)

> **API de la plateforme**: [Windows.UI.Xaml.Controls.NavigationView classe](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

## <a name="get-the-windows-ui-library"></a>Obtenir la bibliothèque d’interface utilisateur Windows

Ce contrôle est inclus dans le cadre de la bibliothèque de l’interface utilisateur de Windows, un package NuGet qui contient les nouveaux contrôles et les fonctionnalités de l’interface utilisateur pour les applications UWP. Pour plus d’informations, y compris les instructions d’installation, consultez la [vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). 

## <a name="navigation-styles"></a>Styles de navigation

NavigationView prend en charge:

**Volet de navigation de gauche ou de menu**

![volet de navigation développé](images/displaymode-left.png)

**Volet de navigation supérieur ou le menu**

![navigation en haut](images/displaymode-top.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

NavigationView est un contrôle de navigation adaptatif qui fonctionne bien pour:

- La fourniture d’une expérience de navigation cohérente dans toute votre application.
- Conservation de l’espace écran sur les fenêtres plus petites.
- Organisation d’accès à de nombreuses catégories de navigation.

Pour les autres contrôles de navigation, voir [les bases de conception de Navigation](../basics/navigation-basics.md).

Si votre navigation requiert un comportement plus complexe qui n’est pas pris en charge par NavigationView, vous devez envisager le modèle [Maître/Détails](master-details.md) à la place.

:::row:::
    :::column:::
        ![Some image](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
        **XAML Controls Gallery**<br>
        If you have the XAML Controls Gallery app installed, click <a href="xamlcontrolsgallery:/item/NavigationView">here</a> to open the app and see NavigationView in action.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="display-modes"></a>Modes d’affichage

NavigationView peut être défini sur différents modes d’affichage, via le `PaneDisplayMode` propriété:

:::row:::
    :::column:::
    ### <a name="left"></a>Vers la gauche
    Affiche un volet de gauche positionné développé.
    :::column-end:::
    :::column span="2":::
    ![volet de navigation gauche développé](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Nous vous recommandons de navigation de gauche lorsque:

- Vous avez un nombre élevé moyenne (entre 5 et 10) des catégories de navigation de niveau supérieur tout aussi importants.
- Votre choix des catégories de navigation très important avec moins d’espace pour d’autres contenus d’application.

:::row:::
    :::column:::
    ### <a name="top"></a>Top
    Affiche un haut positionné volet.
    :::column-end:::
    :::column span="2":::
    ![navigation en haut](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

Nous vous recommandons de navigation supérieur lorsque:

- Vous avez 5 ou moins de catégories de navigation de niveau supérieur tout aussi importants, tel que toutes les catégories de navigation de niveau supérieur supplémentaires qui se retrouver dans la liste déroulante de débordement menu sont considérés comme moins importants.
- Vous devez afficher toutes les options de navigation à l’écran.
- Vous souhaitez davantage d’espace pour le contenu de votre application.
- Les icônes ne peuvent pas décrivent clairement les catégories de navigation de votre application.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    Affiche une zone étroite mince avec des icônes sur la gauche.
    :::column-end:::
    :::column span="2":::
    ![volet de navigation compact](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Affiche uniquement le bouton de menu.
    :::column-end:::
    :::column span="2":::
    ![volet de navigation minimal](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automatique

![GIF leftnav un comportement adaptatif par défaut](images/displaymode-auto.png)

Adapte entre LeftMinimal sur les petits écrans, LeftCompact sur les écrans de taille moyenne et gauche sur grand écran. Consultez la section de [comportement ADAPTATIF](#adaptive-behavior) pour plus d’informations.

## <a name="anatomy"></a>Anatomie

<b>Navigation de gauche</b><br>

![sections NavigationView gauche](images/leftnav-anatomy.png)

<b>Navigation supérieure</b><br>

![Top des sections NavigationView](images/topnav-anatomy.png)

## <a name="pane"></a>Volet

Le volet peut être positionné sur haut ou sur la gauche, via le `PanePosition` propriété.

Voici l’anatomie volet détaillées pour les postes de volet de gauche et de haut:

<b>Navigation de gauche</b><br>

![Anatomie de NavigationView](images/navview-pane-anatomy-vertical.png)

1. Bouton Menu
1. Éléments de navigation
1. Séparateurs
1. En-têtes
1. AutoSuggestBox (facultatif)
1. Bouton de paramètres (facultatif)

<b>Navigation supérieure</b><br>

![Anatomie de NavigationView](images/navview-pane-anatomy-horizontal.png)

1. En-têtes
1. Éléments de navigation
1. Séparateurs
1. AutoSuggestBox (facultatif)
1. Bouton de paramètres (facultatif)

Le bouton précédent s’affiche dans le coin supérieur gauche du volet, mais NavigationView n’ajoute pas automatiquement de contenu à la pile back. Pour activer la navigation vers l’arrière, voir la [vers l’arrière navigation](#backwards-navigation) section.

Le volet NavigationView peut également contenir:

1. Éléments de navigation, sous la forme de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), pour accéder à des pages spécifiques.
2. Séparateurs, sous la forme de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), pour regrouper les éléments de navigation. Définissez la propriété [Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity) sur 0 pour restituer le séparateur en tant qu’espace.
3. En-têtes, sous la forme de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), pour libeller des groupes d’éléments.
4. Facultatif [AutoSuggestBox](auto-suggest-box.md) pour permettre des recherches au niveau de l’application.
5. Un point d’entrée facultatif pour les [paramètres d’application](../app-settings/app-settings-and-data.md). Pour masquer l’élément paramètres, utilisez la propriété [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) .

Le volet gauche contient:

6. Bouton de menu pour activer/désactiver le volet ouvrir et fermer. Sur les grandes fenêtres d’application lorsque le volet est ouvert, vous pouvez choisir de masquer ce bouton à l’aide de la propriété [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

### <a name="pane-footer"></a>Pied de page volet

Contenu Forme libre dans le pied de page du volet lors de l’ajout de la propriété [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)

:::row:::
    :::column:::
    <b>Navigation de gauche</b><br>
    ![Volet de navigation de gauche pied de page](images/navview-freeform-footer-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navigation supérieure</b><br>
    ![Navigation supérieure de volet en-tête](images/navview-freeform-footer-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-header"></a>En-tête du volet

Contenu forme libre dans l’en-tête du volet, lors de l’ajout à la propriété [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)

:::row:::
    :::column:::
    <b>Navigation de gauche</b><br>
    ![Volet de navigation de gauche en-tête](images/navview-freeform-header-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navigation supérieure</b><br>
    ![Navigation supérieure de volet en-tête](images/navview-freeform-header-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-content"></a>Volet de contenu

Contenu forme libre dans le volet, lors de l’ajout à la propriété [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)

:::row:::
    :::column:::
    <b>Navigation de gauche</b><br>
    ![Volet personnalisé contentleft nav](images/navview-freeform-pane-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navigation supérieure</b><br>
    ![Volet navigation supérieure contenue personnalisée](images/navview-freeform-pane-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="visual-style"></a>Style visuel

Lors de la configuration matérielle et logicielle requise est remplies, NavigationView utilise automatiquement le [support ACRYLIQUE](../style/acrylic.md) dans son volet et les [principales fonctionnalités de révéler](../style/reveal.md) uniquement dans le volet gauche.

## <a name="header"></a>En-tête

![image de générique navview de la zone d’en-tête](images/nav-header.png)

La zone d’en-tête est alignée verticalement avec le bouton de navigation à la position du volet de gauche et se trouve sous le volet à la position du volet supérieur. Il possède une hauteur fixe de 52 px. Elle contient le titre de la page de la catégorie de navigation sélectionnée. L’en-tête est ancré sur le haut de la page et se comporte comme un point de découpage de défilement pour la zone de contenu.

L’en-tête doit être visible lorsque NavigationView est en mode d’affichage Minimal. Vous pouvez choisir de masquer l’en-tête dans les autres modes, utilisés pour des fenêtres de largeur supérieure. Pour ce faire, définissez la propriété [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) sur **false**.

## <a name="content"></a>Contenu

![image de générique navview de zone de contenu](images/nav-content.png)

La zone de contenu présente la plupart des informations relatives à la catégorie de navigation sélectionnée.

Nous recommandons des marges de 12px pour votre zone de contenu lorsque NavigationView est en mode Minimal, et des marges de 24px dans les autres cas.

## <a name="adaptive-behavior"></a>Comportement adaptatif

NavigationView change automatiquement son mode d'affichage selon la quantité d’espace d’écran à sa disposition. Toutefois, vous souhaiterez peut-être personnaliser le comportement de mode d’affichage adaptatif.

### <a name="default"></a>Par défaut

Le comportement adaptatif par défaut de NavigationView consiste à afficher un volet de gauche développé des largeurs de grande fenêtre, un volet de navigation d’icône seule gauche des largeurs de la fenêtre de taille moyenne et un bouton de menu hamburger sur les fenêtres étroites. Pour plus d’informations sur les tailles de fenêtre pour un comportement ADAPTATIF, voir les [tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![GIF leftnav un comportement adaptatif par défaut](images/displaymode-auto.png)

```xaml
<NavigationView />
```

### <a name="minimal"></a>Minimal

Courant ADAPTATIF deuxième consiste à utiliser un volet de gauche développé sur les grandes fenêtres étroites et un menu hamburger sur les deux largeurs de la fenêtre de petites et moyennes.

![GIF leftnav comportement ADAPTATIF 2](images/adaptive-behavior-minimal.png)

```xaml
<NavigationView CompactModeThresholdWidth="1008" ExpandedModeThresholdWidth="1007" />
```

Nous recommandons cette quand:

- Davantage d’espace pour le contenu de l’application des largeurs de plus petite fenêtre votre choix.
- Vos catégories de navigation ne peut pas être représentés clairement avec des icônes.

### <a name="compact"></a>Compact

Courant ADAPTATIF tiers consiste à utiliser un volet de gauche développé sur les grandes fenêtres étroites et un volet de navigation d’icône seule gauche sur les deux largeurs de la fenêtre de petites et moyennes. Un bon exemple de ce est l’application de messagerie.

![GIF leftnav comportement ADAPTATIF 3](images/adaptive-behavior-compact.png)

```xaml
<NavigationView CompactModeThresholdWidth="0" ExpandedModeThresholdWidth="1007" />
```

Nous recommandons cette quand:

- Il est important de toujours afficher toutes les options de navigation à l’écran.
- vos catégories de navigation peuvent être représentés clairement avec des icônes.

### <a name="no-adaptive-behavior"></a>Aucun comportement ADAPTATIF

Parfois, ne peut-être pas votre choix tout comportement adaptatif du tout. Vous pouvez définir le volet afin d’être toujours développé, toujours minimal ou compact toujours.

![GIF leftnav comportement ADAPTATIF 4](images/adaptive-behavior-none.png)

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

### <a name="top-to-left-navigation"></a>En haut de la navigation de gauche

Nous recommandons l’utilisation de navigation supérieur sur les grandes tailles de fenêtres et de navigation de gauche sur les petites tailles de fenêtre lorsque:

- Vous disposez d’un ensemble de tout aussi les catégories de navigation de niveau supérieur important à afficher ensemble, telles que si une catégorie dans ce jeu ne tient pas à l’écran, vous réduisez de navigation de gauche afin de donner leur importance équivalente.
- Vous souhaitez conserver en tant que contenu beaucoup d’espace que possible dans les tailles de petite fenêtre.

Voici un exemple:

![comportement adaptatif de navigation supérieur ou gauche GIF 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>

    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>
</Grid>

```

Parfois, les applications doivent lier des données différentes pour le volet supérieur et le volet gauche. Souvent, le volet gauche comprend plusieurs éléments de navigation.

Voici un exemple:

![comportement adaptatif de navigation supérieur ou gauche GIF 2](images/navigation-top-to-left2.png)

```xaml
<Page >
    <Page.Resources>
        <DataTemplate x:name="navItem_top_temp" x:DataType="models:Item">
            <NavigationViewItem Background= Icon={x:Bind TopIcon}, Content={x:Bind TopContent}, Visibility={x:Bind TopVisibility} />
        </DataTemplate>

        <DataTemplate x:name="navItem_temp" x:DataType="models:Item">
            <NavigationViewItem Icon={x:Bind Icon}, Content={x:Bind Content}, Visibility={x:Bind Visibility} />
        </DataTemplate>
        
        <services:NavViewDataTemplateSelector x:Key="navview_selector" 
              NavItemTemplate="{StaticResource navItem_temp}" 
              NavItemTopTemplate="{StaticResource navItem_top_temp}" 
              NavPaneDisplayMode="{x:Bind NavigationViewControl.PaneDisplayMode}">
        </services:NavViewDataTemplateSelector>
    </Page.Resources>

    <Grid >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavView x:Name='NavigationViewControl' MenuItemsSource={x:Bind items}   
                 PanePosition = "Top" MenuItemTemplateSelector="navview_selector" />
    </Grid>
</Page>

```

```csharp
ObservableCollection<Item> items = new ObservableCollection<Item>();
items.Add(new Item() {
    Content = "Aa",
    TopContent ="A",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Bb",
    TopContent = "B",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Cc",
    TopContent = "C",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});

public class NavViewDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate NavItemTemplate { get; set; }

    public DataTemplate NavItemTopTemplate { get; set; }    

    public NavigationViewPaneDisplayMode NavPaneDisplayMode { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        Item currItem = item as Item;
        if (NavPaneDisplayMode == NavigationViewPanePosition.Top)
            return NavItemTopTemplate;
        else 
            return NavItemTemplate;
    }   

}

```

## <a name="interaction"></a>Interaction

Lorsque les utilisateurs appuient sur un élément de navigation dans le volet, NavigationView affichera cet élément comme étant sélectionné et déclenchera un événement [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked). Si l’appui entraîne la sélection d’un nouvel élément, NavigationView déclenche également un événement [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged).

Votre application est responsable de la mise à jour de l’en-tête et du contenu avec les informations adéquates en réponse à cette interaction utilisateur. En outre, nous recommandons d’effectuer un déplacement programmatique du [focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.FocusState) de l’élément de navigation vers le contenu. En définissant le focus initial sur la charge, vous rationalisez le flux d’utilisation et réduisez le nombre attendu de déplacements de focus clavier.

### <a name="tabs"></a>Onglets

Dans le modèle onglets, la sélection et le focus sont liées. Une action que normalement équipes focus est décalé également sélection. Dans l’exemple indiqué ci-dessous, aide droit se déplace l’indicateur de sélection de l’affichage à la Loupe. Vous pouvez y parvenir en définissant la propriété [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.selectionfollowsfocus) sur activé.

![capture d’écran de texte uniquement navview supérieur](images/nav-tabs.png)

Voici l’exemple de code XAML pour que:

```xaml
<NavigationView PanePosition="Top" SelectionFollowsFocus="Enabled" >
   <NavigationView.MenuItems>
        <NavigationViewItem Content="Display" />
        <NavigationViewItem Content="Magnifier"  />
        <NavigationViewItem Content="Keyboard" />
    </NavigationView.MenuItems>
</NavigationView>

```

Pour remplacer le contenu lorsque vous modifiez la sélection de l’onglet, vous pouvez utiliser la méthode de [NavigateWithOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.NavigateToType) de l’image avec FrameNavigationOptions.IsNavigationStackEnabled est défini sur False et NavigateOptions.TransitionInfoOverride définie sur l’approprié à-côte animation de glissement. Pour obtenir un exemple, consultez l' [exemple de code](#code-example) ci-dessous.

Si vous souhaitez modifier le Style par défaut, vous pouvez remplacer la propriété de [MenuItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemcontainerstyle) de NavigationView. Vous pouvez également définir la propriété [MenuItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemtemplate) pour spécifier un modèle de données différents.

## <a name="backwards-navigation"></a>Navigation vers l’arrière

NavigationView a un bouton Précédent intégré, qui peut être activé avec les propriétés suivantes:

- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) est une énumération NavigationViewBackButtonVisible et «Auto» par défaut. Il est utilisé pour afficher/masquer le bouton Précédent. Lorsque le bouton n’est pas visible, l’espace pour dessiner le bouton précédent sera réduit.
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) est false par défaut et peut être utilisé pour activer les états du bouton précédent.
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) est déclenché lorsqu’un utilisateur clique sur le bouton précédent.
    - En mode Minimal/Compact, lorsque le NavigationView.Pane est ouvert sous forme de menu volant, le fait de cliquer sur le bouton précédent ferme le volet et déclenche l'événement **PaneClosing** à la place.
    - Non déclenché si IsBackEnabled a la valeur false.

:::row:::
    :::column:::
    <b>Navigation de gauche</b><br>
    ![Bouton précédent de NavigationView de navigation à gauche](images/leftnav-back.png)
    :::column-end:::
    :::column:::
     <b>Navigation supérieure</b><br>
    ![Bouton précédent de NavigationView de navigation supérieure](images/topnav-back.png)
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Exemple de code

> [!NOTE]
> NavigationView doit servir de conteneur racine de votre application, car ce contrôle est conçu pour couvrir la pleine largeur et la pleine hauteur de la fenêtre de l’application.
Vous pouvez remplacer les largeurs qui déclenchent un changement du mode d’affichage de navigation à l'aide des propriétés [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) et [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth).

Voici un exemple de bout en bout de la façon dont vous pouvez incorporer NavigationView avec un volet de navigation supérieur de grandes tailles de fenêtres et un volet de navigation de gauche sur les tailles de petite fenêtre.

Dans cet exemple, nous pensons que les utilisateurs finaux pour sélectionner fréquemment des catégories de navigation et donc que nous:

- Définissez la propriété [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) sur activé
- Utilisez des navigations de trame qui n’ajoutez pas à la pile de navigation.
- Conserver la valeur par défaut sur la propriété [ShoulderNavigationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) , qui est utilisée pour indiquer si les gâchettes hautes de gauche/droite sur un boîtier de commande naviguer dans les catégories de navigation de niveau supérieur de votre application. La valeur par défaut est «WhenSelectionFollowsFocus». Les autres valeurs possibles sont «Toujours» et «Jamais».

Nous montrons également comment implémenter la navigation avec le bouton précédent de NavigationView vers l’arrière.

Voici un enregistrement de ce que l’exemple illustre:

![Exemple de NavigationView de bout en bout](images/nav-code-example.gif)

Voici l’exemple de code:

> [!NOTE]
> Si vous utilisez la [Bibliothèque de l’interface utilisateur de Windows](https://docs.microsoft.com/uwp/toolkits/winui/), vous devez ajouter une référence au Kit de ressources: `xmlns:controls="using:Microsoft.UI.Xaml.Controls"`.

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavigationView x:Name="NavView"
                    SelectionFollowsFocus="Enabled"
                    ItemInvoked="NavView_ItemInvoked"
                    IsSettingsVisible="True"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested"
                    Header="Welcome">

            <NavigationView.MenuItems>
                <NavigationViewItem Content="Home" x:Name="home" Tag="home">
                    <NavigationViewItem.Icon>
                        <FontIcon Glyph="&#xE10F;"/>
                    </NavigationViewItem.Icon>
                </NavigationViewItem>
                <NavigationViewItemSeparator/>
                <NavigationViewItemHeader Content="Main pages"/>
                <NavigationViewItem Icon="AllApps" Content="Apps" x:Name="apps" Tag="apps"/>
                <NavigationViewItem Icon="Video" Content="Games" x:Name="games" Tag="games"/>
                <NavigationViewItem Icon="Audio" Content="Music" x:Name="music" Tag="music"/>
            </NavigationView.MenuItems>

            <NavigationView.AutoSuggestBox>
                <AutoSuggestBox x:Name="ASB" QueryIcon="Find"/>
            </NavigationView.AutoSuggestBox>

            <NavigationView.HeaderTemplate>
                <DataTemplate>
                    <Grid Margin="24,10,0,0">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Text="Welcome"/>
                        <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
                            VerticalAlignment="Top"
                            DefaultLabelPosition="Right"
                            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
                            <AppBarButton Label="Refresh" Icon="Refresh"/>
                            <AppBarButton Label="Import" Icon="Import"/>
                        </CommandBar>
                    </Grid>
                </DataTemplate>
            </NavigationView.HeaderTemplate>

            <Frame x:Name="ContentFrame" Margin="24"/>

        </NavigationView>
    </Grid>
</Page>
```

> [!NOTE]
> Si vous utilisez la [Bibliothèque de l’interface utilisateur de Windows](https://docs.microsoft.com/uwp/toolkits/winui/), vous devez ajouter une référence au Kit de ressources: `using MUXC = Microsoft.UI.Xaml.Controls;`.

```csharp
private Type currentPage;

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page 
private readonly IList<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon(Symbol.Folder),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default: you need to specify it
    NavView_Navigate("home");

    // Add keyboard accelerators for backwards navigation
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

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    if (args.InvokedItem == null)
        return;

    if (args.IsSettingsInvoked)
        ContentFrame.Navigate(typeof(SettingsPage));
    else
    {
        // Getting the Tag from Content (args.InvokedItem is the content of NavigationViewItem)
        var navItemTag = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(i => args.InvokedItem.Equals(i.Content))
            .Tag.ToString();

        NavView_Navigate(navItemTag);
    }
}

private void NavView_Navigate(string navItemTag)
{
    var item = _pages.First(p => p.Tag.Equals(navItemTag));
    if (currentPage == item.Page)
          return;
    ContentFrame.Navigate(item.Page);

    currentPage = item.Page;
}

private void NavView_BackRequested(NavigationView sender, NavigationViewBackRequestedEventArgs args) => On_BackRequested();

private void BackInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == NavigationViewDisplayMode.Compact ||
        NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag
        NavView.SelectedItem = (NavigationViewItem)NavView.SettingsItem;
    }
    else
    {
        var item = _pages.First(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));
    }
}
```

## <a name="customizing-backgrounds"></a>Personnalisation des arrière-plans

Pour modifier l’arrière-plan de la zone principale de NavigationView, réglez sa propriété `Background` sur votre pinceau préféré.

L’arrière-plan du volet affiche l’ACRYLIQUE dans l’application lorsque NavigationView est en haut, un minimum, ou en mode Compact. Pour mettre à jour ce comportement ou personnaliser l’apparence de l'acrylique de votre volet, modifiez les ressources des deux thèmes en les remplaçant dans votre App.xaml.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewTopPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="scroll-content-under-top-pane"></a>Faire défiler le contenu dans le volet supérieur

Pour un aspect épuré + la sensation, si votre application comporte des pages qui utilisent un élément ScrollViewer et que votre volet de navigation est haut positionné, nous vous conseillons le défilement de contenu situé sous le volet de navigation supérieure. Cela donne un type d’en-tête rémanent du comportement à l’application.

Cela est possible en définissant la propriété [CanContentRenderOutsideBounds](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.cancontentrenderoutsidebounds) sur l’élément ScrollViewer pertinent sur true.

![navview défilement VoletNav](images/nav-scroll-content.png)

Si votre application possède le contenu de défilement très longs, vous souhaiterez pensez à incorporer des en-têtes rémanents joindre dans le volet de navigation supérieure qui forment une surface lisse. 

![en-tête rémanent de défilement navview](images/nav-scroll-stickyheader.png)

Vous pouvez y parvenir en définissant la propriété [ContentOverlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) sur NavigationView. 

Parfois, si l’utilisateur est défilement vers le bas, vous voudrez masquer le volet de navigation, obtenu en définissant la propriété [IsPaneVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) sur NavigationView sur false.

![navview défilement masquer nav](images/nav-scroll-hidepane.png)

## <a name="related-topics"></a>Rubriques connexes

- [Classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maître/Détails](master-details.md)
- [Contrôle Pivot](tabs-pivot.md)
- [Notions de base sur la navigation](../basics/navigation-basics.md)
- [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md)
