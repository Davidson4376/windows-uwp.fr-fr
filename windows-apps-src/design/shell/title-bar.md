---
description: Personnaliser la barre de titre d’une application de bureau pour correspondre à la personnalité de l’application.
title: Personnalisation de la barre de titre
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows10, uwp, barre de titre
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 88c613456525648883735850fe831cb3b67f145c
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8829679"
---
# <a name="title-bar-customization"></a>Personnalisation de la barre de titre



Lorsque votre application s’exécute dans une fenêtre de bureau, vous pouvez personnaliser les barres de titre pour correspondre à la personnalité de votre application. Les API de personnalisation de barre de titre vous permettent de spécifier les couleurs des éléments de barre de titre ou d’étendre le contenu de votre application dans la zone de barre de titre et d’en prendre le contrôle total.

> **API importantes**: [propriété ApplicationView.TitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview), [classe ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), [classe CoreApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>Degrés de personnalisation de la barre de titre

Il existe deux niveaux de personnalisation que vous pouvez appliquer à la barre de titre.

Pour une personnalisation simple des couleurs, vous pouvez définir les propriétés [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) pour spécifier les couleurs que vous souhaitez utiliser pour les éléments de barre de titre. Dans ce cas, le système conserve la responsabilité de tous les autres aspects de la barre de titre, notamment le tracé du titre de l’application et la définition des zones pouvant être glissées.

L’autre option consiste à masquer la barre de titre par défaut et à la remplacer par votre propre contenu XAML. Par exemple, vous pouvez placer du texte, des boutons ou des menus personnalisés dans la zone de barre de titre. Vous devrez également utiliser cette option pour étendre un arrière-plan [acrylique](../style/acrylic.md) dans la zone de barre de titre.

Lorsque vous optez pour une personnalisation complète, vous êtes responsable de placer le contenu dans la zone de barre de titre et vous pouvez définir votre propre zone pouvant être glissée. Les boutons système Retour, Fermer, Réduire et Agrandir sont toujours disponibles et gérés par le système, contrairement aux éléments tels que le titre de l’application. Vous devrez créer ces éléments par vous-même, selon les besoins de votre application.

> [!NOTE]
> La personnalisation simple des couleurs est disponible pour les applicationsUWP utilisant du code XAML, DirectX et HTML. La personnalisation complète est disponible uniquement pour les applicationsUWP en XAML.

## <a name="simple-color-customization"></a>Personnalisation simple des couleurs

Si vous souhaitez uniquement personnaliser les couleurs de la barre de titre et ne rien faire de trop fantaisiste (par exemple, placer des onglets dans votre barre de titre), vous pouvez définir les propriétés de couleur sur [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) pour votre fenêtre d’application.

Cet exemple montre comment obtenir une instance de ApplicationViewTitleBar et définir ses propriétés de couleur.

```csharp
// using Windows.UI.ViewManagement;

var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
titleBar.ButtonForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonHoverForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonHoverBackgroundColor = Windows.UI.Colors.DarkSeaGreen;
titleBar.ButtonPressedForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonPressedBackgroundColor = Windows.UI.Colors.LightGreen;

// Set inactive window colors
titleBar.InactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.InactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonInactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonInactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
```

> [!NOTE]
> Ce code peut être placé dans la méthode [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) de votre application (_App.xaml.cs_), après l’appel à [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate), ou dans la première page de votre application.

> [!TIP]
> Le Kit de ressources de la Communauté Windows fournit des extensions qui vous permettent de définir ces propriétés de couleur en XAML. Pour plus d’informations, voir la [documentation du Kit de ressources de la Communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/extensions/viewextensions).

Il y a plusieurs choses à savoir lors de la définition des couleurs de la barre de titre:

- La couleur d’arrière-plan des boutons n’est pas appliquée aux états de survol et d’activation du bouton Fermer. Le bouton Fermer utilise toujours la couleur définie par le système pour ces états.
- Les propriétés de couleur des boutons sont appliquées au bouton système Retour lorsqu’il est utilisé. ([Voir Historique de navigation et navigation vers l’arrière](../basics/navigation-history-and-backwards-navigation.md).)
- La définition d’une propriété de couleur sur **null** la réinitialise sur la couleur système par défaut.
- Vous ne pouvez pas définir des couleurs transparentes. Le canal alpha de couleur est ignoré.

Windows donne à un utilisateur la possibilité d’appliquer la [couleur d’accentuation](https://docs.microsoft.com/windows/uwp/style/color#accent-color) sélectionnée à la barre de titre. Si vous définissez une couleur de barre de titre, nous vous recommandons de définir explicitement toutes les couleurs. Cela garantit qu’aucune combinaison de couleurs non souhaitée ne se produit en raison des paramètres de couleur définis par l’utilisateur.

## <a name="full-customization"></a>Personnalisation complète

Lorsque vous choisissez une personnalisation complète de la barre de titre, la zone cliente de votre application est étendue pour couvrir la fenêtre entière, y compris la zone de barre de titre. Vous êtes responsable de tracer et de gérer les entrées de la fenêtre entière, sauf les boutons de légende, qui sont placés sur le canevas de l’application.

Pour masquer la barre de titre par défaut et étendre votre contenu dans la zone de barre de titre, définissez la propriété [CoreApplicationViewTitleBar.ExtendViewIntoTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar) sur **true**.

Cet exemple montre comment obtenir CoreApplicationViewTitleBar et définir la propriété ExtendViewIntoTitleBar sur **true**. Cela est possible dans la méthode [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) de votre application (_App.xaml.cs_) ou dans la première page de votre application.

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> Ce paramètre est conservé lorsque votre application est fermée et redémarrée. Dans VisualStudio, si vous définissez ExtendViewIntoTitleBar sur **true**et que vous voulez revenir à la valeur par défaut, vous devez la définir explicitement sur **false** et exécuter votre application pour remplacer le paramètre persistant.

### <a name="draggable-regions"></a>Zones pouvant être glissées

La zone de barre de titre pouvant être glissée définit où l’utilisateur peut cliquer et faire glisser pour déplacer la fenêtre (par opposition à faire simplement glisser un contenu dans le canevas de l’application). Vous spécifiez la zone pouvant être glissée en appelant la méthode [Window.SetTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.settitlebar) et en transmettant un élément UIElement qui définit la zone pouvant être glissée. (L’élément UIElement est souvent un panneau qui contient d’autres éléments.)

Voici comment définir une grille de contenu comme la zone de barre de titre pouvant être glissée. Ce code va dans le code XAML et le code-behind pour la première page de votre application. Voir la section [Exemple de personnalisation complète](./title-bar.md#full-customization-example) pour obtenir le code complet.

```xaml
<Grid x:Name="AppTitleBar" Background="Transparent">
    <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
    <!-- Using padding columns instead of Margin ensures that the background
         paints the area under the caption control buttons (for transparent buttons). -->
    <Grid.ColumnDefinitions>
        <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
        <ColumnDefinition/>
        <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
    </Grid.ColumnDefinitions>
    <Image Source="Assets/Square44x44Logo.png" 
           Grid.Column="1" HorizontalAlignment="Left" 
           Width="20" Height="20" Margin="12,0"/>
    <TextBlock Text="Custom Title Bar" 
               Grid.Column="1" 
               Style="{StaticResource CaptionTextBlockStyle}" 
               Margin="44,8,0,0"/>
</Grid>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;

    // Set XAML element as a draggable region.
    AppTitleBar.Height = coreTitleBar.Height;
    Window.Current.SetTitleBar(AppTitleBar);
}
```

L’élément UIElement (`AppTitleBar`) fait partie du code XAML pour votre application. Vous pouvez soit déclarer et définir la barre de titre dans une page racine qui ne change pas, soit déclarer et définir une zone de barre de titre dans chaque page à laquelle votre application peut accéder. Si vous la définissez dans chaque page, vous devez vous assurer que la zone pouvant être glissée reste cohérente lorsqu’un utilisateur navigue dans votre application.

Vous pouvez appeler SetTitleBar pour basculer vers un nouvel élément de barre de titre lorsque votre application est en cours d’exécution. Vous pouvez également transmettre **null** comme paramètre à SetTitleBar afin de rétablir le comportement de glissement par défaut. (Voir «Zone pouvant être glissée par défaut» pour plus d’informations.)

> [!IMPORTANT]
> La zone pouvant être glissée que vous spécifiez doit pouvoir faire l’objet d'un test de positionnement, ce qui signifie que, pour certains éléments, vous devrez peut-être définir un pinceau d’arrière-plan transparent. Consultez les remarques sur [VisualTreeHelper.FindElementsInHostCoordinates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) pour plus d’informations.
>
>Par exemple, si vous définissez une grille comme votre zone pouvant être glissée, définissez `Background="Transparent"` pour qu’elle puisse être glissée.
>
>Cette grille ne peut pas être glissée (mais les éléments visibles au sein de celle-ci le sont): `<Grid x:Name="AppTitleBar">`.
>
>Cette grille a le même aspect, mais la grille entière peut être glissée: `<Grid x:Name="AppTitleBar" Background="Transparent">`.

#### <a name="default-draggable-region"></a>Zone pouvant être glissée par défaut

Si vous ne spécifiez pas une zone pouvant être glissée, un rectangle qui correspond à la largeur de la fenêtre et à la hauteur des boutons de légende, et qui est positionné sur le bord supérieur de la fenêtre est défini comme la zone pouvant être glissée par défaut.

Si vous définissez une zone pouvant être glissée, le système réduit la zone pouvant être glissée par défaut à une petite zone de la taille d’un bouton de légende, positionné à gauche des boutons de légende (ou à droite si les boutons de légende sont situés à gauche de la fenêtre). Cela garantit qu’il existe toujours une zone cohérente que l’utilisateur peut faire glisser.

### <a name="system-caption-buttons"></a>Boutons de légende système

Le système réserve le coin supérieur gauche ou droit de la fenêtre d’application pour les boutons de légende système (Retour, Réduire, Agrandir, Fermer). Le système conserve le contrôle de la zone de contrôle de légende afin de garantir que les fonctionnalités minimales sont fournies pour le glissement, la réduction, l'agrandissement et la fermeture de la fenêtre. Le système trace le bouton Fermer dans l’angle supérieur droit pour les langues qui s’écrivent de gauche à droite, et dans l’angle supérieur gauche pour les langues qui s’écrivent de droite à gauche.

Les dimensions et la position de la zone de contrôle de légende est communiquée par la classe CoreApplicationViewTitleBar afin que vous puissiez en tenir compte dans la disposition de votre interface utilisateur de barre de titre. La largeur de la zone réservée de chaque côté est fournie par les propriétés [SystemOverlayLeftInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) ou [SystemOverlayRightInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset), et sa hauteur est fournie par la propriété [Height](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height).

Vous pouvez tracer le contenu en dessous de la zone de contrôle de légende définie par ces propriétés, comme l’arrière-plan de votre application, mais vous ne devez pas placer d’interface utilisateur avec laquelle vous souhaitez que l’utilisateur interagisse. Celle-ci ne reçoit aucune entrée dans la mesure où les entrées pour les contrôles de légende sont gérées par le système.

Vous pouvez gérer l’événement [LayoutMetricsChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged) pour répondre aux modifications de la taille des boutons de légende. Par exemple, cela peut se produire lorsque le bouton système Retour est affiché ou masqué. Gérez cet événement pour vérifier et mettre à jour le positionnement des éléments d'interface utilisateur qui dépendent de la taille de la barre de titre.

Cet exemple montre comment ajuster la disposition de votre barre de titre pour prendre en compte les modifications comme l’affichage ou le masquage du bouton système Retour. `AppTitleBar`, `LeftPaddingColumn` et `RightPaddingColumn` sont déclarés dans le code XAML indiqué précédemment.

```csharp
private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}
```

### <a name="interactive-content"></a>Contenu interactif

Vous pouvez placer des contrôles interactifs, tels que des boutons, des menus ou une zone de recherche, dans la partie supérieure de l’application afin qu’ils apparaissent dans la barre de titre. Toutefois, il existe quelques règles que vous devez suivre afin de vous assurer que vos éléments interactifs puissent recevoir des entrées utilisateur.
- Vous devez appeler SetTitleBar pour définir une zone comme la zone de barre de titre pouvant être glissée. Dans le cas contraire, le système définit la zone pouvant être glissée par défaut en haut de la page. Le système gérera ensuite toutes les entrées utilisateur dans cette zone et empêchera les entrées d’atteindre vos contrôles.
- Placez vos contrôles interactifs au-dessus de la zone pouvant être glissée définie par l’appel à SetTitleBar (avec un ordre de plan supérieur). Ne définissez pas vos contrôles interactifs comme les enfants de l’élément UIElement transmis à SetTitleBar. Une fois que vous transmettez un élément à SetTitleBar, le système le traite comme la barre de titre système et gère toutes les entrées de pointeur sur cet élément.

Ici, l’élément `TitleBarButton` a un ordre de plan supérieur à `AppTitleBar`, donc il reçoit les entrées utilisateur.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition />
    </Grid.RowDefinitions>

    <Grid x:Name="AppTitleBar" Background="Transparent">
        <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
        <!-- Using padding columns instead of Margin ensures that the background
             paints the area under the caption control buttons (for transparent buttons). -->
        <Grid.ColumnDefinitions>
            <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
            <ColumnDefinition/>
            <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/Square44x44Logo.png"
               Grid.Column="1" HorizontalAlignment="Left"
               Width="20" Height="20" Margin="12,0"/>
        <TextBlock Text="Custom Title Bar"
                   Grid.Column="1"
                   Style="{StaticResource CaptionTextBlockStyle}"
                   Margin="44,8,0,0"/>
    </Grid>

    <!-- This Button has a higher z-order than AppTitleBar, 
         so it receives user input. -->
    <Button x:Name="TitleBarButton" Content="Button in the title bar"
        HorizontalAlignment="Right"/>
</Grid>
```

### <a name="transparency-in-caption-buttons"></a>Transparence dans les boutons de légende

Lorsque vous définissez ExtendViewIntoTitleBar sur **true**, vous pouvez rendre l’arrière-plan des boutons de légende transparent afin de laisser apparaître à travers l’arrière-plan de votre application. Vous définissez généralement l’arrière-plan sur [Colors.Transparent](https://docs.microsoft.com/uwp/api/windows.ui.colors.Transparent) pour une transparence totale. Pour une transparence partielle, définissez le canal alpha pour la valeur [Color](https://docs.microsoft.com/uwp/api/windows.ui.color) sur laquelle vous avez défini la propriété.

Ces propriétés ApplicationViewTitleBar peuvent être transparentes:

- ButtonBackgroundColor
- ButtonHoverBackgroundColor
- ButtonPressedBackgroundColor
- ButtonInactiveBackgroundColor

La couleur d’arrière-plan des boutons n’est pas appliquée aux états de survol et d’activation du bouton Fermer. Le bouton Fermer utilise toujours la couleur définie par le système pour ces états.

Toutes les autres propriétés de couleur continuent d’ignorer le canal alpha. Si ExtendViewIntoTitleBar est défini sur **false**, le canal alpha est toujours ignoré pour toutes les propriétés de couleur ApplicationViewTitleBar.

### <a name="full-screen-and-tablet-mode"></a>Plein écran et mode tablette

Lorsque votre application s’exécute en _plein écran_ ou _mode tablette_, le système masque la barre de titre et les boutons de contrôle de légende. Toutefois, l’utilisateur peut appeler la barre de titre afin qu’elle s’affiche sous forme de superposition sur l’interface utilisateur de l’application.
Vous pouvez gérer l’événement [CoreApplicationViewTitleBar.IsVisibleChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged) pour être averti lorsque la barre de titre est masquée ou appelée, et afficher ou masquer le contenu de votre barre de titre personnalisée en fonction des besoins.

Cet exemple montre comment gérer IsVisibleChanged pour afficher et masquer les éléments `AppTitleBar` indiqués précédemment.

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

>[!NOTE]
>Le mode _plein écran_ peut être activé uniquement s’il est pris en charge par votre application. Voir [ApplicationView.IsFullScreenMode](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode) pour plus d’informations. Le [_mode tablette_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) est une option utilisateur sur le matériel pris en charge, de sorte qu’un utilisateur peut choisir d’exécuter n’importe quelle application en mode tablette.

## <a name="full-customization-example"></a>Exemple de personnalisation complète

```xaml
<Page
    x:Class="CustomTitleBar.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:CustomTitleBar"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition />
        </Grid.RowDefinitions>

        <Grid x:Name="AppTitleBar" Background="Transparent">
            <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
            <!-- Using padding columns instead of Margin ensures that the background
                 paints the area under the caption control buttons (for transparent buttons). -->
            <Grid.ColumnDefinitions>
                <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
                <ColumnDefinition/>
                <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
            </Grid.ColumnDefinitions>
            <Image Source="Assets/Square44x44Logo.png" 
                   Grid.Column="1" HorizontalAlignment="Left" 
                   Width="20" Height="20" Margin="12,0"/>
            <TextBlock Text="Custom Title Bar" 
                       Grid.Column="1" 
                       Style="{StaticResource CaptionTextBlockStyle}" 
                       Margin="44,8,0,0"/>
        </Grid>

        <!-- This Button has a higher z-order than MyTitleBar, 
             so it receives user input. -->
        <Button x:Name="TitleBarButton" Content="Button in the title bar"
                HorizontalAlignment="Right"/>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Hide default title bar.
    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    UpdateTitleBarLayout(coreTitleBar);

    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);

    // Register a handler for when the size of the overlaid caption control changes.
    // For example, when the app moves to a screen with a different DPI.
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Faites en sorte qu'on distingue clairement lorsque votre fenêtre est active ou inactive. Au minimum, modifiez la couleur du texte, des icônes et des boutons dans votre barre de titre.
- Définissez une zone pouvant être glissée le long du bord supérieur du canevas de l’application. La mise en correspondance avec la position des barres de titre système permet aux utilisateurs d'y accéder plus facilement.
- Définissez une zone pouvant être glissée qui corresponde à la barre de titre visuelle (le cas échéant) sur le canevas de l’application.

## <a name="related-articles"></a>Articles associés

- [Acrylique](../style/acrylic.md)
- [Couleur](../style/color.md)
