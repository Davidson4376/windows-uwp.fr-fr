---
title: Utilisation des commandes dans les applications de plateforme Windows universelle (UWP)
description: Comment utiliser les classes XamlUICommand et StandardUICommand (ainsi que l’interface ICommand) pour partager et gérer les commandes entre les différents types de contrôles, quels que soient l’appareil et le type d’entrée utilisés.
author: Karl-Bridge-Microsoft
ms.service: ''
ms.topic: overview
ms.date: 07/23/2019
ms.openlocfilehash: 338cae7b6238c3c773f409322600c8bee8c193f5
ms.sourcegitcommit: 401c8ecaf74eee247f1ed0093028cc6558b4a605
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68446379"
---
# <a name="commanding-in-universal-windows-platform-uwp-apps-using-standarduicommand-xamluicommand-and-icommand"></a>Utilisation des commandes dans les applications de plateforme Windows universelle (UWP) à l’aide de StandardUICommand, XamlUICommand et ICommand

Dans cette rubrique, nous décrivons l’utilisation des commandes dans les applications de plateforme Windows universelle (UWP). Nous expliquons particulièrement comment utiliser les classes [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) et [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) (ainsi que l’interface ICommand) pour partager et gérer les commandes entre les différents types de contrôles, quels que soient l’appareil et le type d’entrée utilisés.

![Diagramme illustrant l’utilisation courante d’une commande partagée : plusieurs surfaces d’interface utilisateur avec une commande « favori »](images/commanding/generic-commanding.png)

*Partager des commandes entre différents contrôles, quels que soient l’appareil et le type d’entrée*

## <a name="important-apis"></a>API importantes

- [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) et [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>Vue d’ensemble

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

Les commandes peuvent être appelées directement par le biais d’interactions de l’interface utilisateur telles qu’un clic sur un bouton ou la sélection d’un élément dans un menu contextuel. Elles peuvent également être appelées indirectement par le biais d’un périphérique d’entrée comme un accélérateur clavier, d’un mouvement, de la reconnaissance vocale ou d’un outil d’automatisation d’accessibilité. Une fois appelée, la commande peut être traitée par un contrôle (navigation de texte dans un contrôle d’édition), une fenêtre (navigation vers l’arrière) ou l’application (quitter).

Les commandes peuvent fonctionner sur un contexte spécifique au sein de votre application, telles que la suppression de texte ou l’annulation d’une action, ou elles peuvent être dépourvues de contexte, telles que la désactivation du son ou le réglage de la luminosité.

L’illustration suivante montre deux interfaces de commande (un [CommandBar](app-bars.md) et un [CommandBarFlyout](command-bar-flyout.md) contextuel flottant) qui partagent quelques commandes.

![Barre de commandes dans Photos Microsoft](images/control-examples/command-bar-photos.png)<br>*Barre de commandes dans Photos Microsoft*

![Menu contextuel dans la galerie Microsoft Photos](images/ContextMenu_example.png)<br>*Menu contextuel dans la galerie Microsoft Photos*

## <a name="command-interactions"></a>Interactions de commande

Étant donné la diversité des appareils, des types d’entrée et des surfaces d’interface utilisateur qui peuvent affecter la façon dont une commande est appelée, nous vous recommandons d’exposer vos commandes par le biais d’autant de surfaces de commande que possible. Celles-ci peuvent combiner [Swipe](swipe.md), [MenuBar](menus.md), [CommandBar](app-bars.md), [CommandBarFlyout](command-bar-flyout.md) et un [menu contextuel](menus.md) traditionnel.

**Pour les commandes critiques, utilisez des accélérateurs spécifiques de l’entrée.** Les accélérateurs d’entrée permettent à un utilisateur d’effectuer des actions plus rapidement en fonction du périphérique d’entrée utilisé.

Voici certains accélérateurs d’entrée courants pour différents types d’entrée :

- **Pointeur** : boutons sensitifs de stylet & souris
- **Clavier** : raccourcis (touches d’accès rapide et touches accélérateur)
- **Interaction tactile** : balayage
- **Interaction tactile** : tirage pour actualiser les données

Vous devez prendre en compte le type d’entrée et les expériences utilisateur pour que les fonctionnalités de votre application soient universellement accessibles. Par exemple, les collections (en particulier celles qui sont modifiables par l’utilisateur) incluent généralement de nombreuses commandes spécifiques qui sont effectuées tout à fait différemment d’un périphérique d’entrée à l’autre.

Le tableau suivant présente certaines commandes et certains modes de collection standard permettant d’exposer ces commandes.

| Commande          | Indépendant de l’entrée | Accélérateur souris | Accélérateur clavier | Accélérateur tactile |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Supprimer l’élément      | Menu contextuel   | Bouton sensitif      | Touche Suppr              | Balayer pour supprimer   |
| Marquer l’élément        | Menu contextuel   | Bouton sensitif      | Ctrl + Maj + G         | Balayer pour marquer     |
| Actualiser les données     | Menu contextuel   | N/A               | Touche F5               | Tirer pour actualiser   |
| Mettre un élément en favori | Menu contextuel   | Bouton sensitif      | F, Ctrl + S            | Balayer pour mettre en favori |

**Fournissez toujours un menu contextuel** Nous vous recommandons d’inclure toutes les commandes contextuelles pertinentes dans un menu contextuel traditionnel ou un CommandBarFlyout, les deux étant pris en charge pour tous les types d’entrée. Par exemple, si une commande est exposée uniquement pendant un événement de pointage, elle ne peut pas être utilisée sur un appareil uniquement tactile.

## <a name="commands-in-uwp-applications"></a>Commandes dans les applications UWP

Il existe plusieurs façons de partager et gérer des expériences d’utilisation de commandes dans une application UWP. Vous pouvez définir des gestionnaires d’événements pour les interactions standard, comme Click, dans le code-behind (cela peut être très inefficace, selon la complexité de votre interface utilisateur), vous pouvez lier le détecteur d’événements pour les interactions standard à un gestionnaire partagé ou vous pouvez lier la propriété Command du contrôle à une implémentation d’ICommand qui décrit la logique de la commande.

Pour fournir des expériences utilisateur riches et complètes sur les surfaces de commande efficacement et avec une duplication de code minimale, utilisez de préférence les fonctionnalités de liaison de commande décrites dans cette rubrique (pour la gestion des événements standard, consultez la rubrique consacrée à l’événement concerné).

Pour lier un contrôle à une ressource de commande partagée, vous pouvez implémenter les interfaces ICommand vous-même, ou vous pouvez générer votre commande à partir de la classe de base [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) ou d’une des commandes de plateforme définies par la classe dérivée [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand).

- L’interface ICommand ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) ou [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)) vous permet de créer des commandes entièrement personnalisées et réutilisables dans votre application.
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) offre également cette possibilité, mais simplifie le développement en exposant un ensemble de propriétés de commande intégrées, comme le comportement de la commande, les raccourcis clavier (touche d’accès rapide et touche accélérateur), l’icône, l’étiquette et la description.
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) simplifie davantage les choses en vous permettant de choisir parmi un ensemble de commandes de plateforme standard dotées de propriétés prédéfinies.

> [!Important]
> Dans les applications UWP, les commandes sont des implémentations de l’interface [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) (C++) ou [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) (C#), selon le framework de langage que vous avez choisi.

## <a name="command-experiences-using-the-standarduicommand-class"></a>Expériences de commande à l’aide de la classe StandardUICommand

Dérivée de [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) (de [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) pour C++ ou de [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) pour C#), la classe [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) expose un ensemble de commandes de plateforme standard dotées de propriétés prédéfinies telles que l’icône, la touche d’accès rapide et la description.

Une classe [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) offre un moyen rapide et cohérent pour définir des commandes courantes telles que `Save` ou `Delete`. Il vous suffit de fournir les fonctions execute et canExecute.

### <a name="example"></a>Exemple

![Exemple StandardUICommand](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

| Télécharger le code pour cet exemple |
| -------------------- |
| [Exemple d’utilisation de commandes UWP (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip) |

Dans cet exemple, nous montrons comment améliorer un [ListView](listview-and-gridview.md) de base avec une commande de suppression d’élément implémentée par le biais de la classe [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand), tout en optimisant l’expérience utilisateur pour une variété de types d’entrée à l’aide d’un [MenuBar](menus.md), d’un contrôle [Swipe](swipe.md), de boutons de pointage et d’un [menu contextuel](menus.md).

> [!NOTE]
> Cet exemple nécessite le package NuGet Microsoft.UI.Xaml.Controls, qui fait partie de la [bibliothèque d’interface utilisateur Microsoft Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

**Xaml**

L’exemple d’interface utilisateur présente un [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de cinq éléments. La [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) de suppression est liée à un [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), un [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), un [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) et un [menu ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

``` xaml
<Page
    x:Class="StandardUICommandSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:StandardUICommandSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="60"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                StandardUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the StandardUICommand class to 
                share a platform command and consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a standard delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1" Padding="10">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True" 
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered" 
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer" >
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem" 
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left" 
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Code-behind**

1. Tout d’abord, nous définissons une classe `ListItemData` qui contient une chaîne de texte et l’ICommand pour chaque ListViewItem dans notre ListView.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. Dans la classe MainPage, nous définissons une collection d’objets `ListItemData` pour le [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) de l’[ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) du [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview). Ensuite, nous la remplissons avec une collection initiale de cinq éléments (avec du texte et la commande de suppression [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) associée).

```csharp
/// <summary>
/// ListView item collection.
/// </summary>
ObservableCollection<ListItemData> collection = 
    new ObservableCollection<ListItemData>();

/// <summary>
/// Handler for the layout Grid control load event.
/// </summary>
/// <param name="sender">Source of the control loaded event</param>
/// <param name="e">Event args for the loaded event</param>
private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    // Create the standard Delete command.
    var deleteCommand = new StandardUICommand(StandardUICommandKind.Delete);
    deleteCommand.ExecuteRequested += DeleteCommand_ExecuteRequested;

    DeleteFlyoutItem.Command = deleteCommand;

    for (var i = 0; i < 5; i++)
    {
        collection.Add(
            new ListItemData {
                Text = "List item " + i.ToString(),
                Command = deleteCommand });
    }
}

/// <summary>
/// Handler for the ListView control load event.
/// </summary>
/// <param name="sender">Source of the control loaded event</param>
/// <param name="e">Event args for the loaded event</param>
private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    // Populate the ListView with the item collection.
    listView.ItemsSource = collection;
}
```

3. Ensuite, nous définissons le gestionnaire ICommand ExecuteRequested dans lequel nous implémentons la commande de suppression d’élément.

``` csharp
/// <summary>
/// Handler for the Delete command.
/// </summary>
/// <param name="sender">Source of the command event</param>
/// <param name="e">Event args for the command event</param>
private void DeleteCommand_ExecuteRequested(
    XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    // If possible, remove specfied item from collection.
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. Enfin, nous définissons des gestionnaires pour divers événements ListView, notamment les événements [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited) et [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Les gestionnaires d’événements de pointeur sont utilisés pour afficher ou masquer le bouton de suppression pour chaque élément.

```csharp
/// <summary>
/// Handler for the ListView selection changed event.
/// </summary>
/// <param name="sender">Source of the selection changed event</param>
/// <param name="e">Event args for the selection changed event</param>
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

/// <summary>
/// Handler for the pointer entered event.
/// Displays the delete item "hover" buttons.
/// </summary>
/// <param name="sender">Source of the pointer entered event</param>
/// <param name="e">Event args for the pointer entered event</param>
private void ListViewSwipeContainer_PointerEntered(
    object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Mouse || 
        e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(
            sender as Control, "HoverButtonsShown", true);
    }
}

/// <summary>
/// Handler for the pointer exited event.
/// Hides the delete item "hover" buttons.
/// </summary>
/// <param name="sender">Source of the pointer exited event</param>
/// <param name="e">Event args for the pointer exited event</param>

private void ListViewSwipeContainer_PointerExited(
    object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(
        sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-xamluicommand-class"></a>Expériences de commande à l’aide de la classe XamlUICommand

Si vous avez besoin de créer une commande qui n’est pas définie par la classe [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) ou que vous souhaitez mieux contrôler l’apparence des commandes, la classe [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) dérive de l’interface [ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand), ajoutant diverses propriétés d’interface utilisateur (par exemple, une icône, une étiquette, une description et des raccourcis clavier), des méthodes et des événements qui permettent de définir rapidement l’interface utilisateur et le comportement d’une commande personnalisée.

Avec [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand), vous pouvez spécifier l’interface utilisateur par le biais de la liaison de contrôle, par exemple, une icône, une étiquette, une description et des raccourcis clavier (à la fois une touche d’accès rapide et un accélérateur clavier), sans avoir à définir les différentes propriétés.

### <a name="example"></a>Exemple

![Exemple XamlUICommand](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

| Télécharger le code pour cet exemple |
| -------------------- |
| [Exemple d’utilisation de commandes UWP (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip) |

Cet exemple partage les fonctionnalités de suppression de l’exemple [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) précédent, mais montre comment la classe [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) vous permet de définir une commande de suppression personnalisée avec vos propres icône de police, étiquette, accélérateur clavier et description. Comme l’exemple [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand), nous améliorons un [ListView](listview-and-gridview.md) de base avec une commande de suppression d’élément implémentée par le biais de la classe [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand), tout en optimisant l’expérience utilisateur pour une variété de types d’entrée à l’aide d’un [MenuBar](menus.md), d’un contrôle [Swipe](swipe.md), de boutons de pointage et d’un [menu contextuel](menus.md).

De nombreux contrôles de plateforme utilisent les propriétés XamlUICommand en coulisses, tout comme notre exemple StandardUICommand dans la section précédente. 

> [!NOTE]
> Cet exemple nécessite le package NuGet Microsoft.UI.Xaml.Controls, qui fait partie de la [bibliothèque d’interface utilisateur Microsoft Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

**Xaml**

L’exemple d’interface utilisateur présente un [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de cinq éléments. La commande de suppression [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) personnalisée est liée à un [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), un [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), un [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) et un [menu ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

``` xaml
<Page
    x:Class="XamlUICommand_Sample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:XamlUICommand_Sample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <XamlUICommand x:Name="CustomXamlUICommand" ExecuteRequested="DeleteCommand_ExecuteRequested" Description="Custom XamlUICommand" Label="Custom XamlUICommand">
            <XamlUICommand.IconSource>
                <FontIconSource FontFamily="Wingdings" Glyph="&#x4D;"/>
            </XamlUICommand.IconSource>
            <XamlUICommand.KeyboardAccelerators>
                <KeyboardAccelerator Key="D" Modifiers="Control"/>
            </XamlUICommand.KeyboardAccelerators>
        </XamlUICommand>

        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="70"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
        
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded" Name="MainGrid">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                XamlUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the XamlUICommand class to 
                share a custom command with consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a custom delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem" Command="{StaticResource CustomXamlUICommand}"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True"
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered"
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer">
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem"
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left"       
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Code-behind**

1. Tout d’abord, nous définissons une classe `ListItemData` qui contient une chaîne de texte et l’ICommand pour chaque ListViewItem dans notre ListView.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. Dans la classe MainPage, nous définissons une collection d’objets `ListItemData` pour le [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) de l’[ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) du [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview). Ensuite, nous la remplissons avec une collection initiale de cinq éléments (avec du texte et la commande [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) associée).

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    for (var i = 0; i < 5; i++)
    {
        collection.Add(new ListItemData { Text = "List item " + i.ToString(), Command = CustomXamlUICommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. Ensuite, nous définissons le gestionnaire ICommand ExecuteRequested dans lequel nous implémentons la commande de suppression d’élément.

``` csharp
private void DeleteCommand_ExecuteRequested(XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. Enfin, nous définissons des gestionnaires pour divers événements ListView, notamment les événements [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited) et [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Les gestionnaires d’événements de pointeur sont utilisés pour afficher ou masquer le bouton de suppression pour chaque élément.

```csharp
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

private void ListViewSwipeContainer_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-icommand-interface"></a>Expériences de commande à l’aide de l’interface ICommand

Les contrôles UWP standard (bouton, liste, sélection, calendrier, texte prédictif) fournissent la base de nombreuses expériences de commande courantes. Pour obtenir la liste complète des types de contrôle, consultez [Contrôles et modèles pour applications UWP](index.md).

La façon la plus simple de prendre en charge une expérience d’utilisation de commandes structurée consiste à définir une implémentation de l’interface ICommand ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) pour C++ ou [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) pour C#).  Cette instance ICommand peut ensuite être liée à des contrôles tels que des boutons.

> [!NOTE]
> Dans certains cas, il peut suffire de lier une méthode à l’événement Click et une propriété à la propriété IsEnabled.

#### <a name="example"></a>Exemple

![Exemple d’interface de commande](images/commanding/icommand.gif)

*Exemple ICommand*

| Télécharger le code pour cet exemple |
| -------------------- |
| [Exemple d’utilisation de commandes UWP (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip) |

Dans cet exemple de base, nous allons montrer comment une seule commande peut être appelée avec un clic de bouton, un accélérateur clavier et en actionnant la roulette de la souris.

Nous utilisons deux [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), un rempli avec cinq éléments et l’autre vide, et deux boutons, un pour le déplacement des éléments depuis le ListView de gauche vers le ListView de droite, et l’autre pour le déplacement des éléments de droite à gauche. Chaque bouton est lié à une commande correspondante (ViewModel.MoveRightCommand et ViewModel.MoveLeftCommand, respectivement) et est activé et désactivé automatiquement en fonction du nombre d’éléments dans son ListView associée.

**Le code XAML suivant définit l’interface utilisateur de notre exemple.**

```xaml
<Page
    x:Class="UICommand1.View.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:vm="using:UICommand1.ViewModel"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <vm:OpacityConverter x:Key="opaque" />
    </Page.Resources>

    <Grid Name="ItemGrid"
          Background="AliceBlue"
          PointerWheelChanged="Page_PointerWheelChanged">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <ListView Grid.Column="0" VerticalAlignment="Center"
                  x:Name="CommandListView" 
                  ItemsSource="{x:Bind Path=ViewModel.ListItemLeft}" 
                  SelectionMode="None" IsItemClickEnabled="False" 
                  HorizontalAlignment="Right">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <Grid Grid.Column="1" Margin="0,0,0,0"
              HorizontalAlignment="Center" 
              VerticalAlignment="Center">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel Grid.Row="1">
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE893;" 
                          Opacity="{x:Bind Path=ViewModel.ListItemLeft.Count, 
                                        Mode=OneWay, Converter={StaticResource opaque}}"/>
                <Button Name="MoveItemRightButton"
                        Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                        Command="{x:Bind Path=ViewModel.MoveRightCommand}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Add" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Next"/>
                        <TextBlock>Move item right</TextBlock>
                    </StackPanel>
                </Button>
                <Button Name="MoveItemLeftButton" 
                            Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                            Command="{x:Bind Path=ViewModel.MoveLeftCommand}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Subtract" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Previous"/>
                        <TextBlock>Move item left</TextBlock>
                    </StackPanel>
                </Button>
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE892;"
                          Opacity="{x:Bind Path=ViewModel.ListItemRight.Count, 
                                        Mode=OneWay, Converter={StaticResource opaque}}"/>
            </StackPanel>
        </Grid>
        <ListView Grid.Column="2" 
                  x:Name="CommandListViewRight" 
                  VerticalAlignment="Center" 
                  IsItemClickEnabled="False" 
                  SelectionMode="None"
                  ItemsSource="{x:Bind Path=ViewModel.ListItemRight}" 
                  HorizontalAlignment="Left">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Voici le code-behind pour l’interface utilisateur précédente.**

Dans le code-behind, nous nous connectons à notre modèle de vue qui contient notre code de commande. De plus, nous définissons un gestionnaire pour l’entrée à partir de la roulette de la souris, qui connecte également notre code de commande.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Controls;
using UICommand1.ViewModel;
using Windows.System;
using Windows.UI.Core;

namespace UICommand1.View
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Reference to our view model.
        public UICommand1ViewModel ViewModel { get; set; }

        // Initialize our view and view model.
        public MainPage()
        {
            this.InitializeComponent();
            ViewModel = new UICommand1ViewModel();
        }

        /// <summary>
        /// Handle mouse wheel input and assign our
        /// commands to appropriate direction of rotation.
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void Page_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            var props = e.GetCurrentPoint(sender as UIElement).Properties;

            // Require CTRL key and accept only vertical mouse wheel movement 
            // to eliminate accidental wheel input.
            if ((Window.Current.CoreWindow.GetKeyState(VirtualKey.Control) != 
                CoreVirtualKeyStates.None) && !props.IsHorizontalMouseWheel)
            {
                bool delta = props.MouseWheelDelta < 0 ? true : false;

                switch (delta)
                {
                    case true:
                        ViewModel.MoveRight();
                        break;
                    case false:
                        ViewModel.MoveLeft();
                        break;
                    default:
                        break;
                }
            }
        }
    }
}
```

**Voici le code à partir de notre modèle de vue**

Notre modèle de vue est l’endroit où nous définissons les détails de l’exécution des deux commandes dans notre application, remplissons un ListView et fournissons un convertisseur de valeur d’opacité pour le masquage ou l’affichage d’éléments d’interface utilisateur supplémentaires selon le nombre d’éléments de chaque ListView.

```csharp
using System;
using System.Collections.ObjectModel;
using System.ComponentModel;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Data;

namespace UICommand1.ViewModel
{
    /// <summary>
    /// UI properties for our list items.
    /// </summary>
    public class ListItemData
    {
        /// <summary>
        /// Gets and sets the list item content string.
        /// </summary>
        public string ListItemText { get; set; }
        /// <summary>
        /// Gets and sets the list item icon.
        /// </summary>
        public Symbol ListItemIcon { get; set; }
    }

    /// <summary>
    /// View Model that sets up a command to handle invoking the move item buttons.
    /// </summary>
    public class UICommand1ViewModel
    {
        /// <summary>
        /// The command to invoke when the Move item left button is pressed.
        /// </summary>
        public RelayCommand MoveLeftCommand { get; private set; }

        /// <summary>
        /// The command to invoke when the Move item right button is pressed.
        /// </summary>
        public RelayCommand MoveRightCommand { get; private set; }

        // Item collections
        public ObservableCollection<ListItemData> ListItemLeft { get; } = new ObservableCollection<ListItemData>();
        public ObservableCollection<ListItemData> ListItemRight { get; } = new ObservableCollection<ListItemData>();

        public ListItemData listItem;

        /// <summary>
        /// Sets up a command to handle invoking the move item buttons.
        /// </summary>
        public UICommand1ViewModel()
        {
            MoveLeftCommand = new RelayCommand(new Action(MoveLeft), CanExecuteMoveLeftCommand);
            MoveRightCommand = new RelayCommand(new Action(MoveRight), CanExecuteMoveRightCommand);

            LoadItems();
        }

        /// <summary>
        ///  Populate our list of items.
        /// </summary>
        public void LoadItems()
        {
            for (var x = 0; x <= 4; x++)
            {
                listItem = new ListItemData();
                listItem.ListItemText = "Item " + (ListItemLeft.Count + 1).ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemLeft.Add(listItem);
            }
        }

        /// <summary>
        /// Move left command valid when items present in the list on right.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveLeftCommand()
        {
            return ListItemRight.Count > 0;
        }

        /// <summary>
        /// Move right command valid when items present in the list on left.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveRightCommand()
        {
            return ListItemLeft.Count > 0;
        }

        /// <summary>
        /// The command implementation to execute when the Move item right button is pressed.
        /// </summary>
        public void MoveRight()
        {
            if (ListItemLeft.Count > 0)
            {
                listItem = new ListItemData();
                ListItemRight.Add(listItem);
                listItem.ListItemText = "Item " + ListItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemLeft.RemoveAt(ListItemLeft.Count - 1);
                MoveRightCommand.RaiseCanExecuteChanged();
                MoveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// The command implementation to execute when the Move item left button is pressed.
        /// </summary>
        public void MoveLeft()
        {
            if (ListItemRight.Count > 0)
            {
                listItem = new ListItemData();
                ListItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + ListItemLeft.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemRight.RemoveAt(ListItemRight.Count - 1);
                MoveRightCommand.RaiseCanExecuteChanged();
                MoveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// Views subscribe to this event to get notified of property updates.
        /// </summary>
        public event PropertyChangedEventHandler PropertyChanged;

        /// <summary>
        /// Notify subscribers of updates to the named property
        /// </summary>
        /// <param name="propertyName">The full, case-sensitive, name of a property.</param>
        protected void NotifyPropertyChanged(string propertyName)
        {
            PropertyChangedEventHandler handler = this.PropertyChanged;
            if (handler != null)
            {
                PropertyChangedEventArgs args = new PropertyChangedEventArgs(propertyName);
                handler(this, args);
            }
        }
    }

    /// <summary>
    /// Convert a collection count to an opacity value of 0.0 or 1.0.
    /// </summary>
    public class OpacityConverter : IValueConverter
    {
        /// <summary>
        /// Converts a collection count to an opacity value of 0.0 or 1.0.
        /// </summary>
        /// <param name="value">The count passed in</param>
        /// <param name="targetType">Ignored.</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns>1.0 if count > 0, otherwise returns 0.0</returns>
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            return ((int)value > 0 ? 1.0 : 0.0);
        }

        /// <summary>
        /// Not used, converter is not intended for two-way binding. 
        /// </summary>
        /// <param name="value">Ignored</param>
        /// <param name="targetType">Ignored</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns></returns>
        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

**Pour finir, voici notre implémentation de l’interface ICommand**

Ici, nous définissons une commande qui implémente l’interface [ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) et relaie simplement ses fonctionnalités à d’autres objets.

```csharp
using System;
using System.Windows.Input;

namespace UICommand1
{
    /// <summary>
    /// A command whose sole purpose is to relay its functionality 
    /// to other objects by invoking delegates. 
    /// The default return value for the CanExecute method is 'true'.
    /// <see cref="RaiseCanExecuteChanged"/> needs to be called whenever
    /// <see cref="CanExecute"/> is expected to return a different value.
    /// </summary>
    public class RelayCommand : ICommand
    {
        private readonly Action _execute;
        private readonly Func<bool> _canExecute;

        /// <summary>
        /// Raised when RaiseCanExecuteChanged is called.
        /// </summary>
        public event EventHandler CanExecuteChanged;

        /// <summary>
        /// Creates a new command that can always execute.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        public RelayCommand(Action execute)
            : this(execute, null)
        {
        }

        /// <summary>
        /// Creates a new command.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        /// <param name="canExecute">The execution status logic.</param>
        public RelayCommand(Action execute, Func<bool> canExecute)
        {
            if (execute == null)
                throw new ArgumentNullException("execute");
            _execute = execute;
            _canExecute = canExecute;
        }

        /// <summary>
        /// Determines whether this <see cref="RelayCommand"/> can execute in its current state.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
        /// </param>
        /// <returns>true if this command can be executed; otherwise, false.</returns>
        public bool CanExecute(object parameter)
        {
            return _canExecute == null ? true : _canExecute();
        }

        /// <summary>
        /// Executes the <see cref="RelayCommand"/> on the current command target.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
        /// </param>
        public void Execute(object parameter)
        {
            _execute();
        }

        /// <summary>
        /// Method used to raise the <see cref="CanExecuteChanged"/> event
        /// to indicate that the return value of the <see cref="CanExecute"/>
        /// method has changed.
        /// </summary>
        public void RaiseCanExecuteChanged()
        {
            var handler = CanExecuteChanged;
            if (handler != null)
            {
                handler(this, EventArgs.Empty);
            }
        }
    }
}
```

## <a name="summary"></a>Résumé

La plateforme Windows universelle fournit un système d’utilisation de commandes robuste et flexible avec lequel vous pouvez générer des applications qui partagent et gèrent des commandes entre des types de contrôle, des appareils et des types d’entrée.

Utilisez les approches suivantes quand vous générez des commandes pour vos applications UWP :

- Écouter et gérer les événements dans XAML/code-behind
- Lier à une méthode de gestion d’événement comme Click
- Définir votre propre implémentation ICommand
- Créer des objets XamlUICommand avec vos propres valeurs pour un ensemble prédéfini de propriétés
- Créer des objets StandardUICommand avec un ensemble de propriétés et de valeurs de plateforme prédéfinies

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir une illustration complète d’une implémentation de [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) et [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand), consultez l’exemple [Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics).

## <a name="see-also"></a>Voir également

[Contrôles et modèles pour applications UWP](index.md)

### <a name="samples"></a>Exemples

#### <a name="topic-samples"></a>Exemples de la rubrique

- [Exemple d’utilisation de commandes UWP (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip)
- [Exemple d’utilisation de commandes UWP (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip)
- [Exemple d’utilisation de commandes UWP (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip)

#### <a name="other-samples"></a>Autres exemples

- [Exemples de la plateforme Windows universelle (C# et C++)](https://go.microsoft.com/fwlink/?linkid=832713)
- [Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery)

<!---Some context for the following links goes here
- [link to next logical step for the customer](global-quickstart-template.md)--->

<!--- Required:
In Overview articles, provide at least one next step and no more than three.
Next steps in overview articles will often link to a quickstart.
Use regular links; do not use a blue box link. What you link to will depend on what is really a next step for the customer.
Do not use a "More info section" or a "Resources section" or a "See also section".
--->
