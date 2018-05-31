---
author: serenaz
Description: Control that provides top-level app navigation with an automatically adapting, collapsible left navigation menu
title: Affichage de navigation
ms.assetid: ''
label: Navigation view
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: vasriram
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7fc365a7dbc69819ce88a22db2490b327412c8b4
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675366"
---
# <a name="navigation-view"></a>Affichage de navigation

Le contrôle d’affichage de la navigation fournit un menu de navigation réductible pour la navigation dans les zones supérieures de votre application. Ce contrôle implémente le modèle de volet de navigation, ou menu de type Hamburger, et adapte automatiquement le mode d’affichage du volet aux différentes tailles de fenêtres.

> **API importantes**: [classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview), [classe NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), [énumération NavigationViewDisplayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![Exemple de NavigationView](images/navview_wireframe.png)

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev010/player]

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

NavigationView fonctionne bien pour:

-  De nombreux éléments de navigation de niveau supérieur de type similaire. (Par exemple, une application sportive avec des catégories comme Football américain, Baseball, Basketball, Football et ainsi de suite.)
-  Un nombre moyen ou élevé de catégories de navigation de niveau supérieur (entre5 et10).
-  La fourniture d’une expérience de navigation cohérente. Le volet de navigation doit inclure uniquement les éléments de navigation, pas les actions.
-  En conservant l’espace de l’écran des fenêtres de plus petite taille.

NavigationView fait partie des éléments de navigation que vous pouvez utiliser. Pour en savoir plus sur les modèles de navigation et sur les autres éléments de navigation, voir [Informations de base en matière de conception de la navigation](../basics/navigation-basics.md).

Le contrôle NavigationView dispose de nombreux comportements intégrés qui implémentent le modèle de volet de navigation simple. Si votre navigation requiert un comportement plus complexe qui n’est pas pris en charge par NavigationView, vous devez envisager le modèle [Maître/Détails](master-details.md) à la place.

## <a name="examples"></a>Exemples
<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/NavigationView">ouvrir l’application et voir l'objet NavigationView en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="navigationview-sections"></a>Sections NavigationView

![Sections NavigationView](images/navview_sections.png)

### <a name="pane"></a>Volet

Le bouton de navigation intégrée («hamburger») permet aux utilisateurs d’ouvrir et de fermer le volet. Sur les grandes fenêtres d’application lorsque le volet est ouvert, vous pouvez choisir de masquer ce bouton à l’aide de la propriété [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible). L’étiquette de texte située en regard du hamburger est la propriété [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle).

Le bouton Précédent intégré s’affiche dans le coin supérieur gauche dans le volet. Le contrôle NavigationView n'ajoute pas automatiquement du contenu à la pile Back, mais pour activer la navigation vers l’arrière, voir la section [navigation vers l’arrière](#backwards-navigation).

Le volet NavigationView peut également contenir:

- Des éléments de navigation, sous forme de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), pour accéder à des pages spécifiques
- Des séparateurs, sous forme de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), pour regrouper les éléments de navigation
- Des en-têtes, sous la forme de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), pour libeller des groupes d’éléments
- Un [AutoSuggestBox](auto-suggest-box.md) facultatif permettant d'effectuer des recherches au niveau de l’application
- Un point d’entrée facultatif pour les [paramètres d’application](../app-settings/app-settings-and-data.md). Pour masquer l’élément relatif aux paramètres, utilisez la propriété [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible).
- Contenu Forme libre dans le pied de page du volet lors de l’ajout de la propriété [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)

#### <a name="visual-style"></a>Style visuel

Les éléments NavigationView prennent en charge les états visuels sélectionné, désactivé, pointé, appuyé et actif.

![États des éléments NavigationView: désactivé, pointé, appuyé, sélectionné](images/navview_item-states.png)

Lorsque la configuration logicielle et matérielle requise est respectée, NavigationView utilise automatiquement la nouvelle [matière Acrylique](../style/acrylic.md) et [l’effet de révélation](../style/reveal.md) dans son volet.

### <a name="header"></a>En-tête

La zone d’en-tête est alignée verticalement avec le bouton de navigation et possède une hauteur fixe de 52px. Elle contient le titre de la page de la catégorie de navigation sélectionnée. L’en-tête est ancré sur le haut de la page et se comporte comme un point de découpage de défilement pour la zone de contenu.

L’en-tête doit être visible lorsque NavigationView est en mode Minimal. Vous pouvez choisir de masquer l’en-tête dans les autres modes, utilisés pour des fenêtres de largeur supérieure. Pour ce faire, définissez la propriété [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) sur **false**.

### <a name="content"></a>Contenu

La zone de contenu présente la plupart des informations relatives à la catégorie de navigation sélectionnée. 

Nous recommandons des marges de 12px pour votre zone de contenu lorsque NavigationView est en mode Minimal, et des marges de 24px dans les autres cas.

## <a name="navigationview-display-modes"></a>Modes d'affichage NavigationView
Le volet NavigationView peut être ouvert ou fermé, et offre trois options de mode d’affichage lorsqu’il est ouvert:
-  **Minimal**: seul le bouton hamburger reste fixe tandis que le volet est affiché ou masqué selon les besoins.
-  **Compact**: le volet est toujours affiché sous la forme d’une zone étroite qui peut être ouverte en pleine largeur.
-  **Étendu**: le volet reste ouvert en même temps que le contenu. Lorsqu’il est fermé par l’activation du bouton hamburger, le volet est affiché sous la forme d’une zone étroite.

Par défaut, le système sélectionne automatiquement le meilleur mode d’affichage en fonction de la quantité d’espace d’écran disponible pour le contrôle. (Vous pouvez [remplacer](#overriding-the-default-adaptive-behavior) ce paramètre.)

### <a name="minimal"></a>Minimal

![NavigationView en mode Minimal, avec affichage ouvert et fermé du volet](images/navview_minimal.png)

-  Lorsqu’il est fermé, le volet est masqué par défaut, avec uniquement le bouton de navigation visible.
-  Permet une navigation à la demande qui économise l’espace de l’écran. Il est idéal pour les applications sur les téléphones et phablettes.
-  L’appui sur le bouton de navigation ouvre et ferme le volet, qui se dessine sous forme de superposition au-dessus de l’en-tête et du contenu. Le contenu n’est pas réorganisé.
-  Lorsqu’il est ouvert, le volet est temporaire et peut être fermé avec un mouvement d’abandon interactif, notamment en effectuant une sélection, en appuyant sur le bouton Précédent ou en appuyant en dehors du volet.
-  L’élément sélectionné devient visible lorsque la superposition du volet s’ouvre.
-  Lorsque les conditions sont remplies, l’arrière-plan du volet ouvert est [acrylique dans l’application](../style/acrylic.md#acrylic-blend-types).
-  Par défaut, NavigationView est en mode Minimal lorsque sa largeur globale est inférieure ou égale à 640px.

### <a name="compact"></a>Compact

![NavigationView en mode Compact, avec affichage ouvert et fermé du volet](images/navview_compact.png)

-  Lorsqu’il est fermé, une zone verticale étroite du volet affichant uniquement les icônes et le bouton de navigation est visible.
-  Fournit des indications sur l’emplacement sélectionné tout en utilisant une petite quantité d’espace écran.
-  Ce mode convient mieux aux écrans de taille moyenne comme ceux des tablettes et [expériences «10-foot»](../devices/designing-for-tv.md).
-  L’appui sur le bouton de navigation ouvre et ferme le volet, qui se dessine sous forme de superposition au-dessus de l’en-tête et du contenu. Le contenu n’est pas réorganisé.
-  L’en-tête n’est pas indispensable et peut être masqué afin de donner davantage d’espace vertical au contenu.
-  L’élément sélectionné affiche un indicateur visuel pour mettre en évidence l’endroit où se trouve l’utilisateur dans l’arborescence de navigation.
-  Lorsque les conditions sont remplies, l’arrière-plan du volet est [acrylique dans l’application](../style/acrylic.md#acrylic-blend-types).
-  Par défaut, NavigationView est en mode Compact lorsque sa largeur globale se situe entre 641px et 1007px.

### <a name="expanded"></a>Étendu

![NavigationView en mode Étendu, avec volet ouvert](images/navview_expanded.png)

-  Le volet reste ouvert par défaut. Ce mode convient mieux aux écrans plus grands.
-  Le volet se dessine à côté de l’en-tête et du contenu, qui est réorganisé au sein de son espace disponible.
-  Lorsque le volet est fermé à l’aide du bouton de navigation, il est affiché sous forme de zone étroite à côté de l’en-tête et du contenu.
-  L’en-tête n’est pas indispensable et peut être masqué afin de donner davantage d’espace vertical au contenu.
-  L’élément sélectionné affiche un indicateur visuel pour mettre en évidence l’endroit où se trouve l’utilisateur dans l’arborescence de navigation.
-  Lorsque les conditions sont remplies, l’arrière-plan du volet est peint à l’aide de l’[acrylique en arrière-plan](../style/acrylic.md#acrylic-blend-types).
-  Par défaut, NavigationView est en mode Étendu lorsque sa largeur globale est supérieure à 1007px.

### <a name="overriding-the-default-adaptive-behavior"></a>Ignorer le comportement adaptatif par défaut

NavigationView change automatiquement son mode d'affichage selon la quantité d’espace d’écran à sa disposition.

> [!NOTE] 
NavigationView doit servir de conteneur racine de votre application, car ce contrôle est conçu pour couvrir la pleine largeur et la pleine hauteur de la fenêtre de l’application.
Vous pouvez remplacer les largeurs qui déclenchent un changement du mode d’affichage de navigation à l'aide des propriétés [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) et [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth). 

Examinez les scénarios suivants qui illustrent le moment où vous souhaiterez peut-être personnaliser le comportement du mode d’affichage.

- **Navigation fréquente**: si vous pensez que les utilisateurs navigueront souvent entre les zones de l’application, envisagez de garder le volet visible lorsque la fenêtre est moins large. Une application musicale comportant des zones de navigation Morceaux/Albums/Artistes peut opter pour volet d’une largeur de 280px et rester en mode Étendu tant que la fenêtre d’application fait plus de 560px de large.
```xaml
<NavigationView OpenPaneLength="280" CompactModeThresholdWidth="560" ExpandedModeThresholdWidth="560"/>
```

- **Navigation rare**: si vous pensez que les utilisateurs navigueront très rarement entre les zones de l’application, envisagez de garder le volet masqué lorsque la fenêtre est plus large. Une application Calculatrice avec plusieurs dispositions peut choisir de rester en mode Minimal, même lorsque l’application est agrandie sur un écran de 1080p.
```xaml
<NavigationView CompactModeThresholdWidth="1920" ExpandedModeThresholdWidth="1920"/>
```

- **Levée d’ambiguïté concernant les icônes**: si les zones de navigation de votre application ne se prêtent pas à des icônes explicites, évitez d’utiliser le mode Compact. Une application de visualisation d’images comportant les zones de navigation Collections/Albums/Dossiers peuvent choisir d’afficher NavigationView en mode Minimal pour les fenêtres étroites et de largeur moyenne, et en mode Étendu pour les fenêtres de grande largeur.
```xaml
<NavigationView CompactModeThresholdWidth="1008"/>
```

## <a name="interaction"></a>Interaction

Lorsque les utilisateurs appuient sur un élément de navigation dans le volet, NavigationView affichera cet élément comme étant sélectionné et déclenchera un événement [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked). Si l’appui entraîne la sélection d’un nouvel élément, NavigationView déclenche également un événement [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged). 

Votre application est responsable de la mise à jour de l’en-tête et du contenu avec les informations adéquates en réponse à cette interaction utilisateur. En outre, nous recommandons d’effectuer un déplacement programmatique du [focus](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.FocusState) de l’élément de navigation vers le contenu. En définissant le focus initial sur la charge, vous rationalisez le flux d’utilisation et réduisez le nombre attendu de déplacements de focus clavier.

## <a name="backwards-navigation"></a>Navigation vers l’arrière
NavigationView a un bouton Précédent intégré, qui peut être activé avec les propriétés suivantes:
- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) est une énumération NavigationViewBackButtonVisible et «Auto» par défaut. Il est utilisé pour afficher/masquer le bouton Précédent. Lorsque le bouton n’est pas visible, l’espace pour dessiner le bouton précédent sera réduit.
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) est false par défaut et peut être utilisé pour activer les états du bouton précédent.
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) est déclenché lorsqu’un utilisateur clique sur le bouton précédent.
    - En mode Minimal/Compact, lorsque le NavigationView.Pane est ouvert sous forme de menu volant, le fait de cliquer sur le bouton précédent ferme le volet et déclenche l'événement **PaneClosing** à la place.
    - Non déclenché si IsBackEnabled a la valeur false.

![Bouton Précédent de NavigationView](../basics/images/back-nav/NavView.png)

## <a name="code-example"></a>Exemple de code

Voici un exemple simple de la façon dont vous pouvez incorporer NavigationView dans votre application. 

Nous montrons comment implémenter la navigation vers l’arrière avec le bouton Précédent de NavigationView. Notez que pour utiliser les propriétés de navigation vers l’arrière de NavigationView, vous aurez besoin de [Windows10InsiderPreview (version10.0.17110.0 introduite)](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK).

Nous montrons également la localisation de chaînes de contenu d'élément de navigation avec `x:Uid`. Pour plus d’informations sur la localisation, voir [Localiser des chaînes dans votre interface utilisateur](../../app-resources/localize-strings-ui-manifest.md).

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <NavigationView x:Name="NavView"
                    ItemInvoked="NavView_ItemInvoked"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested">

        <NavigationView.MenuItems>
            <NavigationViewItem x:Uid="HomeNavItem" Content="Home" Tag="home">
                <NavigationViewItem.Icon>
                    <FontIcon Glyph="&#xE10F;"/>
                </NavigationViewItem.Icon>
            </NavigationViewItem>
            <NavigationViewItemSeparator/>
            <NavigationViewItemHeader Content="Main pages"/>
            <NavigationViewItem x:Uid="AppsNavItem" Icon="AllApps" Content="Apps" Tag="apps"/>
            <NavigationViewItem x:Uid="GamesNavItem" Icon="Video" Content="Games" Tag="games"/>
            <NavigationViewItem x:Uid="MusicNavItem" Icon="Audio" Content="Music" Tag="music"/>
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

        <NavigationView.PaneFooter>
            <HyperlinkButton x:Name="MoreInfoBtn"
                             Content="More info"
                             Click="More_Click"
                             Margin="12,0"/>
        </NavigationView.PaneFooter>

        <Frame x:Name="ContentFrame" Margin="24">
            <Frame.ContentTransitions>
                <TransitionCollection>
                    <NavigationThemeTransition/>
                </TransitionCollection>
            </Frame.ContentTransitions>
        </Frame>

    </NavigationView>
</Page>
```

```csharp
private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // you can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
    { Content = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    // set the initial SelectedItem 
    foreach (NavigationViewItemBase item in NavView.MenuItems)
    {
        if (item is NavigationViewItem && item.Tag.ToString() == "home")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
            
    ContentFrame.Navigated += On_Navigated;

    // add keyboard accelerators for backwards navigation
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
    
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{  
    if (args.IsSettingsInvoked)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        // find NavigationViewItem with Content that equals InvokedItem
        var item = sender.MenuItems.OfType<NavigationViewItem>().First(x => (string)x.Content == (string)args.InvokedItem);
        NavView_Navigate(item as NavigationViewItem);
    }
}

private void NavView_Navigate(NavigationViewItem item)
{
    switch (item.Tag)
    {
        case "home":
            ContentFrame.Navigate(typeof(HomePage));
            break;

        case "apps":
            ContentFrame.Navigate(typeof(AppsPage));
            break;

        case "games":
            ContentFrame.Navigate(typeof(GamesPage));
            break;

        case "music":
            ContentFrame.Navigate(typeof(MusicPage));
            break;

        case "content":
            ContentFrame.Navigate(typeof(MyContentPage));
            break;
    }           
}

private void NavView_BackRequested(NavigationView sender, NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    bool navigated = false;

    // don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen && (NavView.DisplayMode == NavigationViewDisplayMode.Compact || NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
    {
        return false;
    }
    else
    {
        if (ContentFrame.CanGoBack)
        {
            ContentFrame.GoBack();
            navigated = true;
        }
    }
    return navigated;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        NavView.SelectedItem = NavView.SettingsItem as NavigationViewItem;
    }
    else 
    {
        Dictionary<Type, string> lookup = new Dictionary<Type, string>()
        {
            {typeof(HomePage), "home"},
            {typeof(AppsPage), "apps"},
            {typeof(GamesPage), "games"},
            {typeof(MusicPage), "music"},
            {typeof(MyContentPage), "content"}    
        };

        String stringTag = lookup[ContentFrame.SourcePageType];

        // set the new SelectedItem  
        foreach (NavigationViewItemBase item in NavView.MenuItems)
        {
            if (item is NavigationViewItem && item.Tag.Equals(stringTag))
            {
                item.IsSelected = true;
                break;
            }
        }        
    }
}
```

## <a name="customizing-backgrounds"></a>Personnalisation des arrière-plans

Pour modifier l’arrière-plan de la zone principale de NavigationView, réglez sa propriété `Background` sur votre pinceau préféré.

L'arrière-plan du volet affiche l’acrylique in-app lorsque NavigationView est en mode Minimal ou Compact et acrylique d'arrière-plan en mode étendu. Pour mettre à jour ce comportement ou personnaliser l’apparence de l'acrylique de votre volet, modifiez les ressources des deux thèmes en les remplaçant dans votre App.xaml.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="extending-your-app-into-the-title-bar"></a>Étendre votre application dans la barre de titre

Pour un aspect transparent et fluide dans la fenêtre de votre application, nous vous recommandons d'étendre NavigationView et son volet acrylique dans la zone de la barre de titre de votre application. Cela évite la forme visuellement peu attrayante créée par la barre de titre, le contenu de NavigationView de couleur unie et le volet acrylique de NavigationView.

Pour ce faire, ajoutez le code suivant à votre App.xaml.cs.

```csharp
//draw into the title bar
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;

//remove the solid-colored backgrounds behind the caption controls and system back button
var viewTitleBar = ApplicationView.GetForCurrentView().TitleBar;
viewTitleBar.ButtonBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonForegroundColor = (Color)Resources["SystemBaseHighColor"];
```

Dessiner dans la barre de titre a pour effet secondaire de masquer le titre de votre application. Pour aider les utilisateurs, restaurez le titre en ajoutant votre propre TextBlock. Ajoutez le balisage suivant à la page racine contenant votre NavigationView.

```xaml
<Grid>

    <TextBlock x:Name="AppTitle" 
        xmlns:appmodel="using:Windows.ApplicationModel"
        Text="{x:Bind appmodel:Package.Current.DisplayName}" 
        Style="{StaticResource CaptionTextBlockStyle}" 
        IsHitTestVisible="False" 
        Canvas.ZIndex="1"/>

    <NavigationView Canvas.ZIndex="0" ... />

</Grid>
```

Vous devez également ajuster les marges de l'AppTitle en fonction de la visibilité du bouton précédent. Et, lorsque l’application est en FullScreenMode, vous devez supprimer l’espacement pour la flèche Précédent, même si la barre de titre réserve de l'espace pour celui-ci.

```csharp
void UpdateAppTitle()
{
    var full = (ApplicationView.GetForCurrentView().IsFullScreenMode);
    var left = 12 + (full ? 0 : CoreApplication.GetCurrentView().TitleBar.SystemOverlayLeftInset);
    AppTitle.Margin = new Thickness(left, 8, 0, 0);
}

Window.Current.CoreWindow.SizeChanged += (s, e) => UpdateAppTitle();
coreTitleBar.LayoutMetricsChanged += (s, e) => UpdateAppTitle();
```

Pour plus d’informations sur la personnalisation des barres de titre, voir [personnalisation de la barre de titre](../shell/title-bar.md).

## <a name="related-topics"></a>Rubriquesassociées

- [Classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maître/Détails](master-details.md)
- [Contrôle Pivot](tabs-pivot.md)
- [Notions de base sur la navigation](../basics/navigation-basics.md)
- [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md)

