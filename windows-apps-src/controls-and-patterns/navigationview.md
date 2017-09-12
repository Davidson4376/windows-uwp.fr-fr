---
author: Jwmsft
Description: "Contrôle offrant une navigation de niveau supérieur tout en économisant l’espace de l’écran."
title: Affichage Navigation
ms.assetid: 
label: Navigation view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.openlocfilehash: 98ab8a95288e5a0225a2185f7ce037e16209d173
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2017
---
# <a name="navigation-view"></a>Affichage Navigation

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

> [!IMPORTANT]
> Cet article décrit les fonctionnalités qui n’ont pas encore été publiées et sont susceptibles d’être considérablement modifiées d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

L’affichage Navigation fournit une disposition verticale courante pour les zones supérieures de votre application via un menu de navigation réductible. Ce contrôle est conçu pour implémenter le modèle de volet de navigation ou de menu hamburger, et il adapte automatiquement sa disposition aux différentes tailles de fenêtre.

> **API importantes**: [classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview), [classe NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), [énumération NavigationViewDisplayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![Exemple de NavigationView](images/navview_wireframe.png)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

NavigationView fonctionne bien pour:

-  Les applications comportant de nombreux éléments de navigation de niveau supérieur ayant un type similaire. Par exemple, une application sportive avec des catégories comme Football américain, Baseball, Basketball, Football et ainsi de suite.
-  La fourniture d’une expérience de navigation cohérente sur l’ensemble des applications. Le volet de navigation doit inclure uniquement les éléments de navigation, pas les actions.
-  Un nombre moyen ou élevé de catégories de navigation de niveau supérieur (entre5 et10).
-  En conservant l’espace de l’écran des fenêtres de plus petite taille.

L’affichage Navigation fait partie des éléments de navigation que vous pouvez utiliser. Pour en savoir plus sur les modèles de navigation et sur les autres éléments de navigation, voir [Informations de base en matière de conception de la navigation pour les applications de plateforme Windows universelle (UWP)](../layout/navigation-basics.md).

Pour obtenir un exemple de code illustrant la manière de créer le modèle de volet de navigation à l’aide de SplitView et ListView, téléchargez la [solution de Navigation XAML](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/XamlNavigation) à partir de GitHub.

## <a name="navigationview-parts"></a>Composants NavigationView
Le contrôle est généralement subdivisé en trois sections: un volet de navigation sur la gauche, et les zones d’en-tête et de contenu sur la droite.

![Sections NavigationView](images/navview_sections.png)

### <a name="pane"></a>Volet

Le volet de navigation peut contenir:

- Des éléments de navigation, sous forme de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), pour accéder à des pages spécifiques
- Des séparateurs, sous forme de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), pour regrouper les éléments de navigation
- Des en-têtes, sous la forme de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), pour libeller des groupes d’éléments
- Un point d’entrée facultatif pour les [paramètres d’application](../app-settings/app-settings-and-data.md). Pour masquer l’élément relatif aux paramètres, utilisez la propriété [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsSettingsVisible).
- Contenu Forme libre dans le pied de page du volet lors de l’ajout de la propriété [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_PaneFooter)

Le bouton de navigation intégrée («hamburger») permet aux utilisateurs d’ouvrir et de fermer le volet. Sur les grandes fenêtres d’application lorsque le volet est ouvert, vous pouvez choisir de masquer ce bouton à l’aide de la propriété [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsPaneToggleButtonVisible).

### <a name="header"></a>En-tête

La zone d’en-tête est alignée verticalement avec le bouton de navigation et possède une hauteur fixe. Elle contient le titre de la page de la catégorie de navigation sélectionnée. L’en-tête est ancré sur le haut de la page et se comporte comme un point de découpage de défilement pour la zone de contenu.

L’en-tête doit être visible lorsque NavigationView est en mode Minimal. Vous pouvez choisir de masquer l’en-tête dans les autres modes, utilisés pour des fenêtres de largeur supérieure. Pour ce faire, définissez la propriété [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_AlwaysShowHeader) sur **false**.

### <a name="content"></a>Contenu

La zone de contenu présente la plupart des informations relatives à la catégorie de navigation sélectionnée. Elle peut contenir un ou plusieurs éléments et c’est une zone propice aux éléments de navigation de niveau inférieur supplémentaires comme [Pivot](tabs-pivot.md).

Nous recommandons des marges de 12px sur les côtés de votre contenu lorsque NavigationView est en mode Minimal, et 24px dans les autres cas.

## <a name="visual-style"></a>Style visuel

<div class="microsoft-internal-note">
Les traits rouges de ce contrôle sont disponibles sur le site [UNI](http://uni/DesignDepot.FrontEnd/#/ProductNav?t=Fluent%20Design%20System%7CControls%20%26%20Patterns%7CNavigationView).<br/><br/>
</div>

Les éléments de navigation prennent en charge les états visuels sélectionné, désactivé, pointé, appuyé et sélectionné.

![États des éléments NavigationView: désactivé, pointé, appuyé, sélectionné](images/navview_item-states.png)

Lorsque la configuration logicielle et matérielle requise est respectée, NavigationView utilise automatiquement la nouvelle [matière Acrylique](../style/acrylic.md) et [l’effet de révélation](../style/reveal.md) dans son volet.


## <a name="navigationview-modes"></a>Modes NavigationView
Le volet NavigationView peut être ouvert ou fermé, et offre trois options de mode d’affichage lorsqu’il est ouvert:
-  **Minimal**: seul le bouton hamburger reste fixe tandis que le volet est affiché ou masqué selon les besoins.
-  **Compact**: le volet est toujours affiché sous la forme d’une zone étroite qui peut être ouverte en pleine largeur.
-  **Étendu**: le volet reste ouvert en même temps que le contenu. Lorsqu’il est fermé par l’activation du bouton hamburger, le volet est affiché sous la forme d’une zone étroite.

Par défaut, le système sélectionne automatiquement le meilleur mode d’affichage en fonction de la quantité d’espace d’écran disponible pour le contrôle. (Vous pouvez remplacer ce paramètre, consultez la section suivante pour plus d’informations.)

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
-  Ce mode convient mieux aux écrans de taille moyenne comme ceux des tablettes et [expériences «10-foot»](../input-and-devices/designing-for-tv.md).
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

## <a name="overriding-the-default-adaptive-behavior"></a>Ignorer le comportement adaptatif par défaut

Le mode d’affichage de navigation change automatiquement selon la quantité d’espace d’écran à sa disposition.
[!NOTE] NavigationView doit servir de conteneur racine de votre application. Ce contrôle est conçu pour couvrir la pleine largeur et la pleine hauteur de la fenêtre de l’application.
Vous pouvez remplacer les largeurs auxquelles l’affichage de navigation modifie les modes d’affichage à l’aide des propriétés [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_CompactModeThresholdWidth) et ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ExpandedModeThresholdWidth). Examinez les scénarios suivants qui illustrent le moment où vous souhaiterez peut-être personnaliser le comportement du mode d’affichage.

-  **Navigation fréquente**: si vous pensez que les utilisateurs navigueront souvent entre les zones de l’application, envisagez de garder le volet visible lorsque la fenêtre est moins large. Une application musicale comportant des zones de navigation Morceaux/Albums/Artistes peut opter pour volet d’une largeur de 280px et rester en mode Étendu tant que la fenêtre d’application fait plus de 560px de large.
```xaml
<NavigationView OpenPaneLength=”280” CompactModeThresholdWidth="560" ExpandedModeThresholdWidth=”560”/>
```
-    **Navigation rare**: si vous pensez que les utilisateurs navigueront très rarement entre les zones de l’application, envisagez de garder le volet masqué lorsque la fenêtre est plus large. Une application Calculatrice avec plusieurs dispositions peut choisir de rester en mode Minimal, même lorsque l’application est agrandie sur un écran de 1080p.
```xaml
<NavigationView CompactModeThresholdWidth=”1920” ExpandedModeThresholdWidth=”1920”/>
```
-    **Levée d’ambiguïté concernant les icônes**: si les zones de navigation de votre application ne se prêtent pas à des icônes explicites, évitez d’utiliser le mode Compact. Une application de visualisation d’images comportant les zones de navigation Collections/Albums/Dossiers peuvent choisir d’afficher NavigationView en mode Minimal pour les fenêtres étroites et de largeur moyenne, et en mode Étendu pour les fenêtres de grande largeur.
```xaml
<NavigationView CompactModeThresholdWidth=”1008”/>
```

## <a name="interaction"></a>Interaction

Lorsque les utilisateurs appuient sur une catégorie de navigation dans le volet, NavigationView affichera cet élément comme étant sélectionné et déclenchera un événement [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ItemInvoked). Si l’appui entraîne la sélection d’un nouvel élément, NavigationView déclenche également un événement [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_SelectionChanged). Votre application est responsable de la mise à jour de l’en-tête et du contenu avec les informations adéquates en réponse à cette interaction utilisateur. En outre, nous recommandons d’effectuer un déplacement programmatique du focus, de l’élément de navigation vers le contenu. En définissant le focus initial sur la charge, vous rationalisez le flux d’utilisation et réduisez le nombre attendu de déplacements de focus clavier.

## <a name="how-to-use-navigationview"></a>Comment utiliser NavigationView

Voici un exemple simple de la façon dont vous pouvez incorporer NavigationView dans votre application.

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
                    Loaded="NavView_Loaded">
    <!-- Load NavigationViewItems in NavView_Loaded. -->

        <NavigationView.HeaderTemplate>
            <DataTemplate>
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Margin="12,0"
                           Text="Welcome"/>
                    <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
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

        <Frame x:Name="ContentFrame">
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
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Apps", Icon = new SymbolIcon(Symbol.AllApps), Tag = "apps" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Games", Icon = new SymbolIcon(Symbol.Video), Tag = "games" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Music", Icon = new SymbolIcon(Symbol.Audio), Tag = "music" });
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    foreach (NavigationViewItem item in NavView.MenuItems)
    {
        if (item.Tag.ToString() == "play")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        switch ((args.InvokedItem as NavigationViewItem).Tag)
        {
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
}
```

## <a name="navigation"></a>Navigation

NavigationView n’affiche pas automatiquement le bouton Précédent dans la barre de titre de votre application et n’ajoute pas automatiquement de contenu à la pile arrière. Le contrôle ne répond pas automatiquement aux appuis sur les boutons Précédent logiciel et matériel. Veuillez consulter la section [Navigation dans l’historique et navigation vers l’arrière](../layout/navigation-history-and-backwards-navigation.md) pour plus d’informations sur cette rubrique et sur la manière d’intégrer la prise en charge de la navigation à votre application.


## <a name="related-topics"></a>Rubriques connexes

* [Classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
* [Maître/Détails](master-details.md)
* [Contrôle Pivot](tabs-pivot.md)
* [Notions de base sur la navigation](../layout/navigation-basics.md)
 

 
