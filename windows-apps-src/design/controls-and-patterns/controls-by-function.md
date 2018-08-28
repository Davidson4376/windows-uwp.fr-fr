---
author: Jwmsft
Description: Provides a list by function of some of the controls that you can use in your apps.
title: Contrôles par fonction
ms.assetid: 8DB4347B-91D6-4659-91F2-80ECF7BBB596
label: Controls by function
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0840bab2e039ec55ea4070f8dad39c0ae4e74bbc
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2887908"
---
# <a name="controls-by-function"></a>Contrôles par fonction

L’infrastructure d’interface utilisateur XAML pour Windows fournit une bibliothèque complète de contrôles qui prennent en charge le développement d’une interface utilisateur. Certains de ces contrôles ont une représentation visuelle, tandis que d’autres font office de conteneurs d’autres contrôles ou d’autre contenu, par exemple des images ou du contenu multimédia. 

Vous pouvez voir de nombreux contrôles d’interface utilisateur Windows en action en téléchargeant l’[exemple d’éléments de base d’une interface utilisateur XAML](http://go.microsoft.com/fwlink/p/?LinkId=619992).

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application de la <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/NavigationView">Ouvrir l’application et de voir le NavigationView en action</a> </p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>


Voici une liste par fonction des contrôles XAML courants que vous pouvez utiliser dans votre application.

## <a name="appbars-and-commands"></a>Barres d’applications et commandes

### <a name="app-bar"></a>Barre de l’application
Barre d’outils pour afficher les commandes spécifiques à l’application. Voir Barre de commandes.

Référence : [AppBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.aspx) 

### <a name="app-bar-button"></a>Bouton de barre de l’application
Bouton pour afficher des commandes avec les styles de la barre de l’application.

![Icônes des boutons de barre de l’application](images/controls/app-bar-buttons.png) 

Référence : [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [SymbolIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.symbolicon.aspx), [BitmapIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.bitmapicon.aspx), [FontIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.fonticon.aspx), [PathIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pathicon.aspx) 

Conception et procédure : [Guide de contrôle Barres d’application et barres de commande](app-bars.md) 

Exemple de code : [Exemple de commandes XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="app-bar-separator"></a>Séparateur de barre de l’application
Sépare visuellement des groupes de commandes dans une barre de commande.

Référence : [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 

Exemple de code : [Exemple de commandes XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="app-bar-toggle-button"></a>Bouton bascule de la barre de l’application
Bouton pour basculer les commandes dans une barre de commande.

Référence : [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) 

Exemple de code : [Exemple de commandes XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="command-bar"></a>Barre de commandes
Barre de l’application spécialisée qui gère le redimensionnement des éléments de boutons de la barre de l’application.

![Contrôle de barre de commandes](images/command-bar-compact.png)

```xaml
<CommandBar>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
</CommandBar>
```
Référence : [CommandBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx) 

Conception et procédure : [Guide de contrôle Barres d’application et barres de commande](app-bars.md)

Exemple de code : [Exemple de commandes XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="buttons"></a>Boutons

### <a name="button"></a>Button
Contrôle qui répond à l’entrée utilisateur et déclenche un événement **Click**.

![Un bouton standard](images/controls/button.png)

```xaml
<Button x:Name="button1" Content="Button" 
        Click="Button_Click" />
```

Référence : [Button](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.button.aspx) 

Conception et procédure : [Guide de contrôle Boutons](buttons.md) 

### <a name="hyperlink"></a>Lien hypertexte
Voir bouton Lien hypertexte.

### <a name="hyperlink-button"></a>Bouton Lien hypertexte
Un bouton qui apparaît sous la forme d’un texte balisé et ouvre l’URI spécifié dans un navigateur.

![Bouton Lien hypertexte](images/controls/hyperlink-button.png)

```xaml
<HyperlinkButton Content="www.microsoft.com" 
                 NavigateUri="http://www.microsoft.com"/>
```

Référence : [HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hyperlinkbutton.aspx) 

Conception et procédure : [Guide de contrôle Liens hypertexte](hyperlinks.md)

### <a name="repeat-button"></a>Bouton de répétition
Bouton qui déclenche l’événement **Click** plusieurs fois à partir du moment où il est enfoncé jusqu’à ce qu’il soit relâché. 

![Contrôle de bouton de répétition](images/controls/repeat-button.png) 

```xaml
<RepeatButton x:Name="repeatButton1" Content="Repeat Button" 
              Click="RepeatButton_Click" />
```

Référence : [RepeatButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 

Conception et procédure : [Guide de contrôle Boutons](buttons.md) 

## <a name="collectiondata-controls"></a>Contrôles de collection/données

### <a name="flip-view"></a>Vue symétrique
Contrôle qui présente une collection d’éléments que l’utilisateur peut parcourir un par un.

```xaml
<FlipView x:Name="flipView1" SelectionChanged="FlipView_SelectionChanged">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

Référence : [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview.aspx) 

Conception et procédure : [Guide de contrôle Vue symétrique](flipview.md) 

### <a name="grid-view"></a>Affichage Grille
Contrôle à défilement vertical qui présente une collection d’éléments en lignes et en colonnes.

```xaml
<GridView x:Name="gridView1" SelectionChanged="GridView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</GridView>
```

Référence : [GridView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) 

Conception et procédure : [Listes](lists.md) 

Exemple de code : [Exemple ListView](http://go.microsoft.com/fwlink/p/?LinkId=619900)

### <a name="items-control"></a>Contrôle d’éléments
Contrôle qui présente une collection d’éléments dans une interface utilisateur spécifiée par un modèle de données. 

```xaml
<ItemsControl/>
```

Référence : [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx) 

### <a name="list-view"></a>Affichage de liste
Contrôle qui présente une collection d’éléments dans une liste à défilement vertical.

```xaml
<ListView x:Name="listView1" SelectionChanged="ListView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</ListView>
```

Référence : [ListView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) 

Conception et procédure : [Listes](lists.md) 

Exemple de code : [Exemple ListView](http://go.microsoft.com/fwlink/p/?LinkId=619900)

## <a name="date-and-time-controls"></a>Contrôles de date et d’heure

### <a name="calendar-date-picker"></a>Sélecteur de dates du calendrier
Contrôle qui permet à un utilisateur de sélectionner une date à l’aide d’un affichage de calendrier déroulant.

![Sélecteur de dates du calendrier avec affichage Calendrier ouvert](images/controls/calendar-date-picker-open.png)

```xaml
<CalendarDatePicker/>
```

Référence : [CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx) 

Conception et procédure : [Contrôles de calendrier, de date et d’heure](date-and-time.md)
 
### <a name="calendar-view"></a>Affichage Calendrier
Affichage de calendrier configurable qui permet à un utilisateur de sélectionner une ou plusieurs dates.

```xaml
<CalendarView/>
```

Référence : [CalendarView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx) 

Conception et procédure : [Contrôles de calendrier, de date et d’heure](date-and-time.md) 

### <a name="date-picker"></a>Sélecteur de dates
Contrôle qui permet à un utilisateur de sélectionner une date.

![Contrôle sélecteur de dates](images/controls/date-picker.png)

```xaml
<DatePicker Header="Arrival Date"/>
```

Référence : [DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx) 

Conception et procédure : [Contrôles de calendrier, de date et d’heure](date-and-time.md)
 
### <a name="time-picker"></a>Sélecteur d’heure
Contrôle qui permet à un utilisateur de définir une heure.

![Contrôle de sélecteur d’heure](images/controls/time-picker.png) 

```xaml
<TimePicker Header="Arrival Time"/>
```

Référence : [TimePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx) 

Conception et procédure : [Contrôles de calendrier, de date et d’heure](date-and-time.md)

## <a name="flyouts"></a>Menus volants

### <a name="context-menu"></a>Menu contextuel
Voir Menu volant et Menu contextuel.

### <a name="flyout"></a>Menu volant
Affiche un message nécessitant une action de la part de l’utilisateur. (Contrairement à une boîte de dialogue, un menu volant ne crée pas de fenêtre distincte et ne bloque pas l’action des autres utilisateurs.)

![Contrôle de menu volant](images/controls/flyout.png)

```xaml
<Flyout>
    <StackPanel>
        <TextBlock Text="All items will be removed. Do you want to continue?"/>
        <Button Click="DeleteConfirmation_Click" Content="Yes, empty my cart"/>
    </StackPanel>
</Flyout>
```

Référence : [Flyout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flyout.aspx) 

Conception et procédures: [volants](dialogs-and-flyouts/flyouts.md) 

### <a name="menu-flyout"></a>Menu volant
Affiche temporairement une liste de commandes ou d’options liées à l’action en cours de l’utilisateur.

![Contrôle de menu volant](images/controls/menu-flyout.png) 

```xaml
<MenuFlyout>
    <MenuFlyoutItem Text="Reset" Click="Reset_Click"/>
    <MenuFlyoutSeparator/>
    <ToggleMenuFlyoutItem Text="Shuffle" 
                          IsChecked="{Binding IsShuffleEnabled, Mode=TwoWay}"/>
    <ToggleMenuFlyoutItem Text="Repeat" 
                          IsChecked="{Binding IsRepeatEnabled, Mode=TwoWay}"/>
</MenuFlyout>
```

Référence : [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyout.aspx), [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 

Conception et procédures: [Menus et menus contextuels](menus.md) 

Exemple de code : [Exemple de menu contextuel XAML](http://go.microsoft.com/fwlink/p/?LinkId=620021)

### <a name="popup-menu"></a>Menu contextuel
Commandes de présentation de menu personnalisé que vous spécifiez.

Référence : [PopupMenu](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.popups.popupmenu.aspx) 

Conception et procédures: [boîtes de dialogue](dialogs-and-flyouts/dialogs.md) 

### <a name="tooltip"></a>Info-bulle
Fenêtre contextuelle qui affiche des informations pour un élément. 
 
![Contrôle d’info-bulle](images/controls/tool-tip.png)

```xaml
<Button Content="Button" Click="Button_Click" 
        ToolTipService.ToolTip="Click to perform action" />
```

Référence : [ToolTip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.tooltip.aspx), [ToolTipService](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.tooltipservice.aspx) 

Conception et procédure: Recommandations en matière d’info-bulles 

## <a name="images"></a>Images

### <a name="image"></a>Image
Contrôle qui présente une image.

```xaml
<Image Source="Assets/Logo.png" />
```

Référence : [Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx) 

Conception et procédure : [Image et ImageBrush](images-imagebrushes.md) 

Exemple de code : [Exemple d’images XAML](http://go.microsoft.com/fwlink/p/?linkid=226867)

## <a name="graphics-and-ink"></a>Graphiques et entrée manuscrite

### <a name="inkcanvas"></a>InkCanvas
Contrôle qui reçoit et qui affiche des traits d’encre.

```xaml
<InkCanvas/>
```

Référence : [InkCanvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.inkcanvas.aspx) 

### <a name="shapes"></a>Formes
Objets graphiques conservés dans différents modes pouvant être présentés comme des ellipses, rectangles, traits, tracés de Bézier, etc.

![Un polygone](images/controls/shapes-polygon.png) 
![un chemin d’accès](images/controls/shapes-path.png) 

```xaml
<Ellipse/>
<Path/>
<Rectangle/>
```

Référence: [Shapes](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.aspx) 

Procédure : [Dessiner des formes](../../graphics/drawing-shapes.md) 

Exemple de code : [Exemple de dessin vectoriel XAML](http://go.microsoft.com/fwlink/p/?linkid=226866)

## <a name="layout-controls"></a>Contrôles de disposition

### <a name="border"></a>Bordure
Contrôle de conteneur qui dessine une bordure, un arrière-plan ou les deux, autour d’un autre objet.

![Bordure autour de 2rectangles](images/controls/border.png) 

```xaml
<Border BorderBrush="Blue" BorderThickness="4" 
        Height="108" Width="64" 
        Padding="8" CornerRadius="4">
    <Canvas>
        <Rectangle Fill="Orange"/>
        <Rectangle Fill="Green" Margin="0,44"/>
    </Canvas>
</Border>
```

Référence : [Border](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.border.aspx)

### <a name="canvas"></a>Zone de dessin
Panneau de disposition qui prend en charge le positionnement absolu des éléments enfants par rapport au coin supérieur gauche de la zone de dessin.
 
![Panneau de disposition de la zone de dessin](images/controls/canvas.png) 

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

Référence : [Canvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)
 
### <a name="grid"></a>Grille
Panneau de disposition qui prend en charge l’organisation des éléments enfants en lignes et colonnes.

![Panneau de disposition de la grille](images/controls/grid.png) 

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="50"/>
        <ColumnDefinition Width="50"/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```

Référence : [Grid](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)
 
### <a name="panning-scroll-viewer"></a>Visionneuse à mouvement panoramique
Voir Visionneuse à défilement.

### <a name="relativepanel"></a>RelativePanel
Panneau qui vous permet de positionner et d’aligner des objets enfants les uns par rapport aux autres ou par rapport au panneau parent.

![Panneau à disposition relative](images/controls/relative-panel.png) 

```xaml
<RelativePanel>
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

Référence : [RelativePanel](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx)

### <a name="scroll-bar"></a>Barre de défilement
Voir Visionneuse à défilement. (ScrollBar est un élément de ScrollViewer. En règle générale, il n’est pas utilisé en tant que contrôle autonome.)

Référence : [ScrollBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.scrollbar.aspx)
 
### <a name="scroll-viewer"></a>Visionneuse à défilement
Contrôle de conteneur qui permet à l’utilisateur d’appliquer une vue panoramique ou un zoom à son contenu.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" 
              HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

Référence : [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx)

Conception et procédure : [Guide de contrôle panoramique et défilement](scroll-controls.md) 

Exemple de code : [Exemple de zoom, de panoramique et de défilement XAML](http://go.microsoft.com/fwlink/p/?linkid=238577)

### <a name="stack-panel"></a>Panneau d’empilement
Panneau de disposition qui organise les éléments enfants sur une seule ligne orientable horizontalement ou verticalement.

![Contrôle de disposition de panneau d’empilement](images/controls/stack-panel.png) 

```xaml
<StackPanel>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue"/>
    <Rectangle Fill="Green"/>
    <Rectangle Fill="Orange"/>
</StackPanel>
```

Référence : [StackPanel](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx)
 
### <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid
Panneau de disposition qui prend en charge l’organisation des éléments enfants en lignes et colonnes. Chaque élément enfant peut occuper plusieurs lignes et colonnes.

![Panneau de disposition de la grille avec renvoi à la ligne de taille variable](images/controls/variable-sized-wrap-grid.png) 

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Height="80" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" Width="80" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" Height="80" Width="80" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```

Référence : [VariableSizedWrapGrid](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx)

### <a name="viewbox"></a>Viewbox
Contrôle de conteneur qui applique une taille spécifique à son contenu.

![Contrôle de viewbox](images/controls/view-box.png) 

```xaml
<Viewbox MaxWidth="25" MaxHeight="25">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="75" MaxHeight="75">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="150" MaxHeight="150">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
```

Référence : [Viewbox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.viewbox.aspx)
 
### <a name="zooming-scroll-viewer"></a>Visionneuse à défilement avec zoom
Voir Visionneuse à défilement.

## <a name="media-controls"></a>Contrôles multimédias

### <a name="audio"></a>Audio
Voir Élément multimédia.

### <a name="media-element"></a>Élément multimédia
Contrôle qui lit du contenu audio et vidéo.

```xaml
<MediaElement x:Name="myMediaElement"/>
```

Référence : [MediaElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediaelement.aspx) 

Conception et procédure : [Guide de contrôle Élément multimédia](media-playback.md)

### <a name="mediatransportcontrols"></a>MediaTransportControls
Contrôle qui fournit les contrôles de lecture pour un MediaElement.

![Élément multimédia avec contrôles de transport](images/controls/media-transport-controls.png) 

```xaml
<MediaTransportControls MediaElement="myMediaElement"/>
```

Référence : [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 

Conception et procédure : [Guide de contrôle Élément multimédia](media-playback.md) 

Exemple de code : [Exemple de contrôles de transport multimédias système](http://go.microsoft.com/fwlink/p/?LinkId=620023)

### <a name="video"></a>Vidéo
Voir Élément multimédia.

## <a name="navigation"></a>Navigation

### <a name="navigationview"></a>NavigationView

Un modèle de navigation flexible qui implémente le volet de navigation de gauche, la partie supérieure et le modèle d’onglets et de conteneur adaptable.

Référence: [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

Conception et procédures: [guide du contrôle NavigationView](navigationview.md)

### <a name="splitview"></a>SplitView

Contrôle de conteneur avec deux vues : un affichage pour le contenu principal et un affichage généralement utilisé pour un menu de navigation.

![Contrôle de mode fractionné](images/controls/split-view.png) 

```xaml
<SplitView>
    <SplitView.Pane>
        <!-- Menu content -->
    </SplitView.Pane>
    <SplitView.Content>
        <!-- Main content -->
    </SplitView.Content>
</SplitView>
```

Référence : [SplitView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) 

Conception et procédure : [Guide de contrôle Mode fractionné](split-view.md)

### <a name="web-view"></a>Affichage web

Contrôle de conteneur qui héberge du contenu web.

```xaml
<WebView x:Name="webView1" Source="http://dev.windows.com" 
         Height="400" Width="800"/>
```

Référence : [WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) 

Conception et procédure: Recommandations pour l’affichage web 

Exemple de code : [Exemple de contrôle d’affichage web XAML](http://go.microsoft.com/fwlink/p/?linkid=238582)

### <a name="semantic-zoom"></a>Zoom sémantique

Contrôle de conteneur qui permet à l’utilisateur d’effectuer un zoom entre deux vues d’une collection d’éléments.

```xaml
<SemanticZoom>
    <ZoomedInView>
        <GridView></GridView>
    </ZoomedInView>
    <ZoomedOutView>
        <GridView></GridView>
    </ZoomedOutView>
</SemanticZoom>
```

Référence : [SemanticZoom](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.aspx) 

Conception et procédure : [Guide de contrôle Zoom sémantique](semantic-zoom.md)

Exemple de code : [Exemple de groupement de GridView et SemanticZoom XAML](http://go.microsoft.com/fwlink/p/?linkid=226564)

## <a name="progress-controls"></a>Contrôles de progression

### <a name="progress-bar"></a>Barre de progression
Contrôle qui indique la progression en affichant une barre.

![Contrôle de barre de progression](images/controls/progress-bar-determinate.png)

Barre de progression qui affiche une valeur spécifique.

```xaml
<ProgressBar x:Name="progressBar1" Value="50" Width="100"/>
```

![Contrôle de barre de progression indéterminée](images/controls/progress-bar-indeterminate.png)

Barre de progression qui affiche une progression indéterminée.

```xaml
<ProgressBar x:Name="indeterminateProgressBar1" IsIndeterminate="True" Width="100"/>
```

Référence : [ProgressBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx) 

Conception et procédure : [Guide Contrôles de progression](progress-controls.md) 

### <a name="progress-ring"></a>Anneau de progression
Contrôle qui indique la progression indéterminée en affichant un cercle. 

![Contrôle d’anneau de progression](images/controls/progress-ring.png) 

```xaml
<ProgressRing x:Name="progressRing1" IsActive="True"/>
```

Référence : [ProgressRing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx) 

Conception et procédure : [Guide Contrôles de progression](progress-controls.md) 

## <a name="text-controls"></a>Contrôles de texte

### <a name="auto-suggest-box"></a>Zone de suggestion automatique
Zone d’entrée de texte qui fournit une suggestion de texte à mesure que l’utilisateur tape au clavier.

![Zone de suggestion automatique pour la recherche](images/controls/auto-suggest-box.png) 

Référence : [AutoSuggestBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)

Conception et procédure : [Contrôles de texte](text-controls.md), [Guide de contrôle Zone de suggestion automatique](auto-suggest-box.md)

Exemple de code : [Exemple de migration AutoSuggestBox](http://go.microsoft.com/fwlink/p/?LinkId=619996)

### <a name="multi-line-text-box"></a>Zone de texte de plusieurs lignes
Voir Zone de texte.

### <a name="password-box"></a>Zone de mot de passe
Contrôle pour la saisie des mots de passe.

 ![Zone de mot de passe](images/controls/password-box.png)

```xaml
<PasswordBox x:Name="passwordBox1" 
             PasswordChanged="PasswordBox_PasswordChanged" />
```

Référence : [PasswordBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx) 

Conception et procédure : [Contrôles de texte](text-controls.md), [Guide de contrôle Zone de mot de passe](password-box.md) 

Exemple de code : [Exemple d’affichage de texte XAML](http://go.microsoft.com/fwlink/p/?linkid=238579), [Exemple de modification de texte XAML](http://go.microsoft.com/fwlink/p/?linkid=251417)

### <a name="rich-edit-box"></a>Zone d’édition enrichie
Contrôle qui permet à un utilisateur de modifier des documents en texte enrichi avec du contenu tel que du texte mis en forme, des liens hypertexte et des images.

```xaml
<RichEditBox />
```

Référence : [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) 

Conception et procédure : [Contrôles de texte](text-controls.md), [Guide de contrôle Zone d’édition enrichie](rich-edit-box.md)

Exemple de code : [Exemple de texte XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)

### <a name="search-box"></a>Zone de recherche
Voir Zone de suggestion automatique.

### <a name="single-line-text-box"></a>Zone de texte d’une ligne
Voir Zone de texte.

### <a name="static-textparagraph"></a>Paragraphe/texte statique
Voir Bloc de texte.

### <a name="text-block"></a>Bloc de texte
Contrôle qui affiche du texte.

![Contrôle de bloc de texte](images/controls/text-block.png) 

```xaml
<TextBlock x:Name="textBlock1" Text="I am a TextBlock"/>
```

Référence : [TextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx), [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx) 

Conception et procédure : [Contrôles de texte](text-controls.md), [Guide de contrôle Bloc de texte](text-block.md), [Guide de contrôle Bloc de texte enrichi](rich-text-block.md)

Exemple de code : [Exemple de texte XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)

### <a name="text-box"></a>Zone de texte
Champ de texte brut sur une ou plusieurs lignes.

![Contrôle de zone de texte](images/controls/text-box.png) 

```xaml
<TextBox x:Name="textBox1" Text="I am a TextBox" 
         TextChanged="TextBox_TextChanged"/>
```

Référence : [TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) 

Conception et procédure : [Contrôles de texte](text-controls.md), [Guide de contrôle Zone de texte](text-box.md) 

Exemple de code : [Exemple de texte XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)

## <a name="selection-controls"></a>Contrôles de sélection

### <a name="check-box"></a>Case à cocher
Contrôle pouvant être activé ou désactivé.

![Les 3états d’une case à cocher](images/templates-checkbox-states-default.png)

```xaml
<CheckBox x:Name="checkbox1" Content="CheckBox" 
          Checked="CheckBox_Checked"/>
```

Référence : [CheckBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.checkbox.aspx) 

Conception et procédure : [Guide de contrôle Case à cocher](checkbox.md) 

### <a name="combo-box"></a>Zone de liste modifiable
Liste déroulante dans laquelle un utilisateur peut sélectionner des éléments.

![Zone de liste modifiable ouverte](images/controls/combo-box-open.png) 

```xaml
<ComboBox x:Name="comboBox1" Width="100"
          SelectionChanged="ComboBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ComboBox>
```

Référence : [ComboBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.combobox.aspx) 

Conception et procédure : [Listes](lists.md) 

### <a name="list-box"></a>Zone de liste
Contrôle qui présente une liste inline dans laquelle un utilisateur peut sélectionner des éléments. 

![Contrôle de zone de liste](images/controls/list-box.png)

```xaml
<ListBox x:Name="listBox1" Width="100"
         SelectionChanged="ListBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ListBox>
```

Référence : [ListBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listbox.aspx) 

Conception et procédure : [Listes](lists.md) 

### <a name="radio-button"></a>Case d’option
Contrôle qui autorise un utilisateur à sélectionner une seule option dans un groupe d’options. Lorsque des cases d’option sont regroupées, elles sont mutuellement exclusives.

![Contrôles de case d’option](images/controls/radio-button.png)

```xaml
<RadioButton x:Name="radioButton1" Content="RadioButton 1" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
<RadioButton x:Name="radioButton2" Content="RadioButton 2" GroupName="Group1" 
             Checked="RadioButton_Checked" IsChecked="True"/>
<RadioButton x:Name="radioButton3" Content="RadioButton 3" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
```

Référence : [RadioButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.radiobutton.aspx) 

Conception et procédure : [Guide de contrôle Case d’option](radio-button.md)
 
### <a name="slider"></a>Curseur
Contrôle qui permet à l’utilisateur d’effectuer une sélection parmi une plage de valeurs en déplaçant un contrôle Thumb le long d’une ligne.

![Contrôle de curseur](images/controls/slider.png)

```xaml
<Slider x:Name="slider1" Width="100" ValueChanged="Slider_ValueChanged" />
```

Référence : [Slider](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx) 

Conception et procédure : [Guide de contrôle Curseur](slider.md) 

### <a name="toggle-button"></a>Bouton bascule
Bouton pouvant être basculé entre deux états.

```xaml
<ToggleButton x:Name="toggleButton1" Content="Button" 
              Checked="ToggleButton_Checked"/>
```

Référence : [ToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.togglebutton.aspx)

Conception et procédure : [Guide de contrôle Bascule](toggles.md) 

### <a name="toggle-switch"></a>Commutateur bascule
Bouton pouvant basculer entre deux états.

![Contrôle de commutateur bascule](images/controls/toggle-switch.png) 

```xaml
<ToggleSwitch x:Name="toggleSwitch1" Header="ToggleSwitch" 
              OnContent="On" OffContent="Off" 
              Toggled="ToggleSwitch_Toggled"/>
```

Référence : [ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.toggleswitch.aspx) 

Conception et procédure : [Guide de contrôle Bascule](toggles.md) 
