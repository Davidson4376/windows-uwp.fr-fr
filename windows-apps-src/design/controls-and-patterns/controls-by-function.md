---
Description: Fournit une liste par fonction de certains des contrôles que vous pouvez utiliser dans vos applications.
title: Contrôles par fonction
ms.assetid: 8DB4347B-91D6-4659-91F2-80ECF7BBB596
label: Controls by function
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6de5e9d8899a7f270d30438a0563b879ccdab898
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363140"
---
# <a name="controls-by-function"></a>Contrôles par fonction

L’infrastructure d’interface utilisateur XAML pour Windows fournit une bibliothèque complète de contrôles qui prennent en charge le développement d’une interface utilisateur. Certains de ces contrôles ont une représentation visuelle, tandis que d’autres font office de conteneurs d’autres contrôles ou d’autre contenu, par exemple des images ou du contenu multimédia. 

Vous pouvez voir de nombreux contrôles d’interface utilisateur Windows en action en téléchargeant l’[exemple d’éléments de base d’une interface utilisateur XAML](https://go.microsoft.com/fwlink/p/?LinkId=619992).

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/NavigationView">ouvrez l’application et consultez NavigationView en action</a> </p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>


Voici une liste par fonction des contrôles XAML courants que vous pouvez utiliser dans votre application.

## <a name="appbars-and-commands"></a>Barres d’applications et commandes

### <a name="app-bar"></a>Barre de l’application
Barre d’outils pour afficher les commandes spécifiques à l’application. Voir Barre de commandes.

Référence : [AppBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) 

### <a name="app-bar-button"></a>Bouton de barre de l’application
Bouton pour afficher des commandes avec les styles de la barre de l’application.

![Icônes des boutons de barre de l’application](images/controls/app-bar-buttons.png) 

Référence : [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton), [SymbolIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SymbolIcon), [BitmapIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon), [FontIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FontIcon), [PathIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon) 

Conception et procédures : [Guide de contrôle de barre de l’application et barre de commandes](app-bars.md) 

Exemple de code : [Exemples de commandes de XAML](https://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="app-bar-separator"></a>Séparateur de barre de l’application
Sépare visuellement des groupes de commandes dans une barre de commande.

Référence : [AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) 

Exemple de code : [Exemples de commandes de XAML](https://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="app-bar-toggle-button"></a>Bouton bascule de la barre de l’application
Bouton pour basculer les commandes dans une barre de commande.

Référence : [AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) 

Exemple de code : [Exemples de commandes de XAML](https://go.microsoft.com/fwlink/p/?LinkId=620019)

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
Référence : [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 

Conception et procédures : [Guide de contrôle de barre de l’application et barre de commandes](app-bars.md)

Exemple de code : [Exemples de commandes de XAML](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="buttons"></a>Boutons

### <a name="button"></a>Bouton
Contrôle qui répond à l’entrée utilisateur et déclenche un événement **Click**.

![Un bouton standard](images/controls/button.png)

```xaml
<Button x:Name="button1" Content="Button" 
        Click="Button_Click" />
```

Référence : [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) 

Conception et procédures : [Guide du contrôle de boutons](buttons.md) 

### <a name="hyperlink"></a>Hyperlink
Voir bouton Lien hypertexte.

### <a name="hyperlink-button"></a>Bouton Lien hypertexte
Un bouton qui apparaît sous la forme d’un texte balisé et ouvre l’URI spécifié dans un navigateur.

![Bouton Lien hypertexte](images/controls/hyperlink-button.png)

```xaml
<HyperlinkButton Content="www.microsoft.com" 
                 NavigateUri="https://www.microsoft.com"/>
```

Référence : [HyperlinkButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) 

Conception et procédures : [Guide du contrôle des liens hypertexte](hyperlinks.md)

### <a name="repeat-button"></a>Bouton de répétition
Bouton qui déclenche l’événement **Click** plusieurs fois à partir du moment où il est enfoncé jusqu’à ce qu’il soit relâché. 

![Contrôle de bouton de répétition](images/controls/repeat-button.png) 

```xaml
<RepeatButton x:Name="repeatButton1" Content="Repeat Button" 
              Click="RepeatButton_Click" />
```

Référence : [RepeatButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.RepeatButton) 

Conception et procédures : [Guide du contrôle de boutons](buttons.md) 

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

Référence : [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) 

Conception et procédures : [Retournement d’afficher le guide de contrôle](flipview.md) 

### <a name="grid-view"></a>Affichage Grille
Contrôle à défilement vertical qui présente une collection d’éléments en lignes et en colonnes.

```xaml
<GridView x:Name="gridView1" SelectionChanged="GridView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</GridView>
```

Référence : [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 

Conception et procédures : [Listes](lists.md) 

Exemple de code : [Exemple de ListView](https://go.microsoft.com/fwlink/p/?LinkId=619900)

### <a name="items-control"></a>Contrôle d’éléments
Contrôle qui présente une collection d’éléments dans une interface utilisateur spécifiée par un modèle de données. 

```xaml
<ItemsControl/>
```

Référence : [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 

### <a name="list-view"></a>Affichage Liste
Contrôle qui présente une collection d’éléments dans une liste à défilement vertical.

```xaml
<ListView x:Name="listView1" SelectionChanged="ListView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</ListView>
```

Référence : [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 

Conception et procédures : [Listes](lists.md) 

Exemple de code : [Exemple de ListView](https://go.microsoft.com/fwlink/p/?LinkId=619900)

## <a name="date-and-time-controls"></a>Contrôles de date et d’heure

### <a name="calendar-date-picker"></a>Sélecteur de dates du calendrier
Contrôle qui permet à un utilisateur de sélectionner une date à l’aide d’un affichage de calendrier déroulant.

![Sélecteur de dates du calendrier avec affichage Calendrier ouvert](images/controls/calendar-date-picker-open.png)

```xaml
<CalendarDatePicker/>
```

Référence : [CalendarDatePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker) 

Conception et procédures : [Contrôles de temps, de date et calendrier](date-and-time.md)
 
### <a name="calendar-view"></a>Affichage Calendrier
Affichage de calendrier configurable qui permet à un utilisateur de sélectionner une ou plusieurs dates.

```xaml
<CalendarView/>
```

Référence : [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 

Conception et procédures : [Contrôles de temps, de date et calendrier](date-and-time.md) 

### <a name="date-picker"></a>Sélecteur de dates
Contrôle qui permet à un utilisateur de sélectionner une date.

![Contrôle sélecteur de dates](images/controls/date-picker.png)

```xaml
<DatePicker Header="Arrival Date"/>
```

Référence : [DatePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) 

Conception et procédures : [Contrôles de temps, de date et calendrier](date-and-time.md)
 
### <a name="time-picker"></a>Sélecteur d’heure
Contrôle qui permet à un utilisateur de définir une heure.

![Contrôle de sélecteur d’heure](images/controls/time-picker.png) 

```xaml
<TimePicker Header="Arrival Time"/>
```

Référence : [Sélecteur d’heure](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) 

Conception et procédures : [Contrôles de temps, de date et calendrier](date-and-time.md)

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

Référence : [Menu volant](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout) 

Conception et procédures : [Menus volants](dialogs-and-flyouts/flyouts.md) 

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

Référence : [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout), [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutSeparator), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleMenuFlyoutItem) 

Conception et procédures : [Menus et menus contextuels](menus.md) 

Exemple de code : [Exemple de Menu contextuel de XAML](https://go.microsoft.com/fwlink/p/?LinkId=620021)

### <a name="popup-menu"></a>Menu contextuel
Commandes de présentation de menu personnalisé que vous spécifiez.

Référence : [PopupMenu](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) 

Conception et procédures : [Boîtes de dialogue](dialogs-and-flyouts/dialogs.md) 

### <a name="tooltip"></a>Info-bulle
Fenêtre contextuelle qui affiche des informations pour un élément. 
 
![Contrôle d’info-bulle](images/controls/tool-tip.png)

```xaml
<Button Content="Button" Click="Button_Click" 
        ToolTipService.ToolTip="Click to perform action" />
```

Référence : [ToolTip](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip), [ToolTipService](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTipService) 

Conception et procédures : Recommandations en matière d’info-bulles 

## <a name="images"></a>Images

### <a name="image"></a>Image
Contrôle qui présente une image.

```xaml
<Image Source="Assets/Logo.png" />
```

Référence : [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 

Conception et procédures : [Image et ImageBrush](images-imagebrushes.md) 

Exemple de code : [Exemples d’images XAML](https://go.microsoft.com/fwlink/p/?linkid=226867)

## <a name="graphics-and-ink"></a>Graphiques et entrée manuscrite

### <a name="inkcanvas"></a>InkCanvas
Contrôle qui reçoit et qui affiche des traits d’encre.

```xaml
<InkCanvas/>
```

Référence : [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 

### <a name="shapes"></a>Formes
Objets graphiques conservés dans différents modes pouvant être présentés comme des ellipses, rectangles, traits, tracés de Bézier, etc.

![Un polygone](images/controls/shapes-polygon.png) 
![un chemin d’accès](images/controls/shapes-path.png) 

```xaml
<Ellipse/>
<Path/>
<Rectangle/>
```

Référence : [Formes](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Shapes.Shape) 

Procédure : [Dessiner des formes](../../graphics/drawing-shapes.md) 

Exemple de code : [Exemple de dessin vectoriel XAML](https://go.microsoft.com/fwlink/p/?linkid=226866)

## <a name="layout-controls"></a>Contrôles de disposition

### <a name="border"></a>Bordure
Contrôle de conteneur qui dessine une bordure, un arrière-plan ou les deux, autour d’un autre objet.

![Bordure autour de 2 rectangles](images/controls/border.png) 

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

Référence : [Border](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)

### <a name="canvas"></a>Canevas
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

Référence : [Canvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas)
 
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

Référence : [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)
 
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

Référence : [RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel)

### <a name="scroll-bar"></a>Scroll bar
Voir Visionneuse à défilement. (ScrollBar est un élément de ScrollViewer. En règle générale, il n’est pas utilisé en tant que contrôle autonome.)

Référence : [ScrollBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar)
 
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

Référence : [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)

Conception et procédures : [Guide de contrôles de défilement et le panoramique](scroll-controls.md) 

Exemple de code : [XAML défilement, panoramique et zoom d’exemple](https://go.microsoft.com/fwlink/p/?linkid=238577)

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

Référence : [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)
 
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

Référence : [VariableSizedWrapGrid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid)

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

Référence : [Viewbox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Viewbox)
 
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

Référence : [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 

Conception et procédures : [Guide de contrôle d’élément multimédia](media-playback.md)

### <a name="mediatransportcontrols"></a>MediaTransportControls
Contrôle qui fournit les contrôles de lecture pour un MediaElement.

![Élément multimédia avec contrôles de transport](images/controls/media-transport-controls.png) 

```xaml
<MediaTransportControls MediaElement="myMediaElement"/>
```

Référence : [MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) 

Conception et procédures : [Guide de contrôle d’élément multimédia](media-playback.md) 

Exemple de code : [Exemple de contrôles de Transport de supports](https://go.microsoft.com/fwlink/p/?LinkId=620023)

### <a name="video"></a>Vidéo
Voir Élément multimédia.

## <a name="navigation"></a>Navigation

### <a name="navigationview"></a>NavigationView

Un conteneur adaptable et un modèle de navigation flexible qui implémente le volet de navigation gauche, la navigation supérieure et le motif d’onglets.

Référence : [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

Conception et procédures : [Guide du contrôle NavigationView](navigationview.md)

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

Référence : [SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView) 

Conception et procédures : [Guide du contrôle de mode fractionné](split-view.md)

### <a name="web-view"></a>Affichage web

Contrôle de conteneur qui héberge du contenu web.

```xaml
<WebView x:Name="webView1" Source="https://developer.microsoft.com" 
         Height="400" Width="800"/>
```

Référence : [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) 

Conception et procédures : Recommandations pour les affichages Web 

Exemple de code : [Exemple de contrôle XAML WebView](https://go.microsoft.com/fwlink/p/?linkid=238582)

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

Référence : [SemanticZoom](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 

Conception et procédures : [Guide du contrôle de zoom sémantique](semantic-zoom.md)

Exemple de code : [Exemple de SemanticZoom et de regroupement de XAML GridView](https://go.microsoft.com/fwlink/p/?linkid=226564)

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

Référence : [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) 

Conception et procédures : [Guide de contrôles de progression](progress-controls.md) 

### <a name="progress-ring"></a>Anneau de progression
Contrôle qui indique la progression indéterminée en affichant un cercle. 

![Contrôle d’anneau de progression](images/controls/progress-ring.png) 

```xaml
<ProgressRing x:Name="progressRing1" IsActive="True"/>
```

Référence : [ProgressRing](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 

Conception et procédures : [Guide de contrôles de progression](progress-controls.md) 

## <a name="text-controls"></a>Contrôles de texte

### <a name="auto-suggest-box"></a>Zone de suggestion automatique
Zone d’entrée de texte qui fournit une suggestion de texte à mesure que l’utilisateur tape au clavier.

![Zone de suggestion automatique pour la recherche](images/controls/auto-suggest-box.png) 

Référence : [AutoSuggestBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox)

Conception et procédures : [Contrôles de texte](text-controls.md), [guide du contrôle de zone de suggestion automatique](auto-suggest-box.md)

Exemple de code : [Exemple de migration AutoSuggestBox](https://go.microsoft.com/fwlink/p/?LinkId=619996)

### <a name="multi-line-text-box"></a>Zone de texte de plusieurs lignes
Voir Zone de texte.

### <a name="password-box"></a>Zone de mot de passe
Contrôle pour la saisie des mots de passe.

 ![Zone de mot de passe](images/controls/password-box.png)

```xaml
<PasswordBox x:Name="passwordBox1" 
             PasswordChanged="PasswordBox_PasswordChanged" />
```

Référence : [PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) 

Conception et procédures : [Contrôles de texte](text-controls.md), [guide de contrôle de zone de mot de passe](password-box.md) 

Exemple de code : [Exemple d’affichage XAML texte](https://go.microsoft.com/fwlink/p/?linkid=238579), [exemple de l’édition de texte XAML](https://go.microsoft.com/fwlink/p/?linkid=251417)

### <a name="rich-edit-box"></a>Zone d’édition enrichie
Contrôle qui permet à un utilisateur de modifier des documents en texte enrichi avec du contenu tel que du texte mis en forme, des liens hypertexte et des images.

```xaml
<RichEditBox />
```

Référence : [RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 

Conception et procédures : [Contrôles de texte](text-controls.md), [guide du contrôle de zone d’édition enrichie](rich-edit-box.md)

Exemple de code : [Exemple de texte XAML](https://go.microsoft.com/fwlink/p/?linkid=238578)

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

Référence : [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock), [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 

Conception et procédures : [Contrôles de texte](text-controls.md), [guide de contrôle de bloc de texte](text-block.md), [guide de contrôle de bloc de texte enrichi](rich-text-block.md)

Exemple de code : [Exemple de texte XAML](https://go.microsoft.com/fwlink/p/?linkid=238578)

### <a name="text-box"></a>Zone de texte
Champ de texte brut sur une ou plusieurs lignes.

![Contrôle de zone de texte](images/controls/text-box.png) 

```xaml
<TextBox x:Name="textBox1" Text="I am a TextBox" 
         TextChanged="TextBox_TextChanged"/>
```

Référence : [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 

Conception et procédures : [Contrôles de texte](text-controls.md), [guide de contrôle de zone de texte](text-box.md) 

Exemple de code : [Exemple de texte XAML](https://go.microsoft.com/fwlink/p/?linkid=238578)

## <a name="selection-controls"></a>Contrôles de sélection

### <a name="check-box"></a>Check box
Contrôle pouvant être activé ou désactivé.

![Les 3 états d’une case à cocher](images/templates-checkbox-states-default.png)

```xaml
<CheckBox x:Name="checkbox1" Content="CheckBox" 
          Checked="CheckBox_Checked"/>
```

Référence : [CheckBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 

Conception et procédures : [Guide du contrôle de case à cocher](checkbox.md) 

### <a name="combo-box"></a>Combo box
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

Référence : [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 

Conception et procédures : [Listes](lists.md) 

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

Référence : [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) 

Conception et procédures : [Listes](lists.md) 

### <a name="radio-button"></a>Radio button
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

Référence : [RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 

Conception et procédures : [Guide du contrôle de bouton radio](radio-button.md)
 
### <a name="slider"></a>Curseur
Contrôle qui permet à l’utilisateur d’effectuer une sélection parmi une plage de valeurs en déplaçant un contrôle Thumb le long d’une ligne.

![Contrôle de curseur](images/controls/slider.png)

```xaml
<Slider x:Name="slider1" Width="100" ValueChanged="Slider_ValueChanged" />
```

Référence : [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) 

Conception et procédures : [Guide du contrôle Slider](slider.md) 

### <a name="toggle-button"></a>Bouton bascule
Bouton pouvant être basculé entre deux états.

```xaml
<ToggleButton x:Name="toggleButton1" Content="Button" 
              Checked="ToggleButton_Checked"/>
```

Référence : [ToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton)

Conception et procédures : [Guide du contrôle de bouton bascule](toggles.md) 

### <a name="toggle-switch"></a>Commutateur bascule
Bouton pouvant basculer entre deux états.

![Contrôle de commutateur bascule](images/controls/toggle-switch.png) 

```xaml
<ToggleSwitch x:Name="toggleSwitch1" Header="ToggleSwitch" 
              OnContent="On" OffContent="Off" 
              Toggled="ToggleSwitch_Toggled"/>
```

Référence : [ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) 

Conception et procédures : [Guide du contrôle de bouton bascule](toggles.md) 
