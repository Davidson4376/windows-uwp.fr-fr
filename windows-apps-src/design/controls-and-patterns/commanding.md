---
title: Exécution de commandes dans Universal Windows Platform (UWP) des applications
description: Comment utiliser les classes XamlUICommand et StandardUICommand (ainsi que l’interface ICommand) pour partager et gérer les commandes entre les différents types de contrôle, quel que soit l’appareil et type d’entrée qui est utilisé.
author: Karl-Bridge-Microsoft
ms.service: ''
ms.topic: overview
ms.date: 03/11/2019
ms.openlocfilehash: a85a101cd529bf487cbc97b93bb3905f28213c19
ms.sourcegitcommit: 99271798fe53d9768fc52b21366de05268cadcb0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58221415"
---
# <a name="commanding-in-universal-windows-platform-uwp-apps-using-standarduicommand-xamluicommand-and-icommand"></a>Exécution de commandes dans les applications de plateforme universelle Windows (UWP) à l’aide de StandardUICommand, XamlUICommand et ICommand

Dans cette rubrique, nous décrivons des commandes dans les applications Universal Windows Platform (UWP). Plus spécifiquement, nous avons abordé comment vous pouvez utiliser la [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) et [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) classes (ainsi que l’interface ICommand) pour partager et gérer des commandes sur différents types de contrôle, quel que soit de l’appareil et le type d’entrée qui est utilisé.

![Un diagramme représentant une utilisation courante pour une commande partagée : met en évidence plusieurs interface utilisateur avec une commande « favorie »](images/commanding/generic-commanding.png)

*Commandes de partage pour les différents contrôles, quel que soit l’appareil et le type d’entrée*

## <a name="important-apis"></a>API importantes

- [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) et [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>Vue d'ensemble

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

Commandes peuvent être appelées directement par le biais d’interactions de l’interface utilisateur comme un clic sur un bouton ou en sélectionnant un élément dans un menu contextuel. Ils peuvent également être appelés indirectement via un périphérique d’entrée comme un accélérateur de clavier, de mouvement, la reconnaissance vocale ou un outil d’automatisation / d’accessibilité. Une fois appelé, la commande peut ensuite être traitée par un contrôle (texte de navigation dans un contrôle d’édition), une fenêtre (navigation arrière) ou l’application (Quitter).

Commandes ne peuvent fonctionner sur un contexte spécifique au sein de votre application, telles que la suppression de texte ou l’annulation d’une action, ou ils peuvent être sans contexte, telles que le mode silencieux ou réglage de la luminosité.

L’illustration suivante montre deux interfaces de commande (un [CommandBar](app-bars.md) et flottante contextuelles [CommandBarFlyout](command-bar-flyout.md)) qui partagent un grand nombre de commandes.

![Exemple d’interface de commande](images/commanding/command-interface-example.png)

## <a name="command-interactions"></a>Interactions de commande

Étant donné la diversité des périphériques, des types d’entrée et des surfaces d’interface utilisateur qui peuvent affecter la façon dont une commande est appelée, nous vous recommandons d’exposer vos commandes par le biais des surfaces de commandes autant que possible. Celles-ci peuvent inclure une combinaison de [balayez](swipe.md), [barre de menus](menus.md), [CommandBar](app-bars.md), [CommandBarFlyout](command-bar-flyout.md)et traditionnel [ menu contextuel](menus.md).

**Pour les commandes critiques, utiliser des accélérateurs de spécifique à l’entrée.** Accélérateurs d’entrée permettent un utilisateur d’effectuer des actions plus rapidement en fonction de l’appareil d’entrée qu’ils utilisent.

Voici certains accélérateurs d’entrée communs pour différents types d’entrée :

- **Pointeur** -souris & uvrir pointez les boutons
- **Clavier** -raccourcis (clés d’accès et touches accélérateur)
- **Touch** -effectuez un balayage
- **Touch** -Pull pour actualiser les données

Vous devez prendre en compte les expériences utilisateur et le type d’entrée pour rendre les fonctionnalités de votre application universellement accessibles. Par exemple, les collections (en particulier ceux qui sont modifiables utilisateur) incluent généralement des diverses commandes spécifiques qui sont effectuées tout à fait différemment en fonction du périphérique d’entrée.

Le tableau suivant présente certaines commandes de la collection classique et les façons d’exposer ces commandes.

| Command          | Indépendamment de l’entrée | Accélérateur souris | Accélérateur clavier | Accélérateur tactile |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Supprimer l’élément      | Menu contextuel   | Pointage sur le bouton      | Touche Suppr              | Balayage pour supprimer   |
| Marquer l’élément        | Menu contextuel   | Pointage sur le bouton      | Ctrl + Maj + G         | Balayage pour marquer     |
| Actualiser les données     | Menu contextuel   | N/A               | Touche F5               | Tirer pour actualiser   |
| Mettre un élément en favori | Menu contextuel   | Pointage sur le bouton      | F, Ctrl + S            | Balayage pour mettre en favori |

**Toujours fournir un menu contextuel** nous recommandons, y compris toutes les commandes contextuelles pertinentes dans le menu de contexte traditionnel ou CommandBarFlyout, comme les deux sont pris en charge pour tous les types d’entrée. Par exemple, si une commande est exposée uniquement pendant un événement de pointage de pointeur, il ne peut pas être utilisé sur un appareil tactile uniquement.

## <a name="commands-in-uwp-applications"></a>Commandes dans les applications UWP

Il existe plusieurs façons, vous pouvez partager et gérer des expériences de l’exécution de commandes dans une application UWP. Vous pouvez définir des gestionnaires d’événements pour des interactions standard, tels que Click, dans le code-behind (qui peut être très inefficace, selon la complexité de votre interface utilisateur), vous pouvez lier l’écouteur d’événements pour les interactions standards à un gestionnaire partagé, ou vous pouvez lier le contrôle Propriété de la commande à une implémentation ICommand qui décrit la logique de commande.

Pour fournir des expériences utilisateur riches et complète sur les surfaces de commande efficacement et avec la duplication d’un minimum de code, nous vous recommandons d’utiliser la fonctionnalités décrites dans cette rubrique de liaison de commande (pour la gestion des événements standard, consultez les rubriques d’événement individuel).

Pour lier un contrôle à une ressource partagée de commande, vous pouvez implémenter les interfaces ICommand vous-même, ou vous pouvez générer votre commande à partir du [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) classe de base ou une des commandes de plateforme définies par le [ StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) classe dérivée.

- L’interface ICommand ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) ou [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)) vous permet de créer entièrement personnalisée, réutilisable commandes sur votre application.
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) également offre cette possibilité mais simplifie le développement en exposant un ensemble de propriétés de commande intégrée, le comportement de la commande raccourcis clavier (touche d’accès rapide et de la touche d’accès rapide), icône, étiquette et description.
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) simplifie les choses en vous permettant de choisir parmi un ensemble de commandes de plate-forme standard avec les propriétés prédéfinies.

> [!Important]
> Dans les applications UWP, commandes sont des implémentations d’un le [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) (C++) ou le [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) (C#) interface, selon votre choisi infrastructure de langage.

## <a name="command-experiences-using-the-standarduicommand-class"></a>Expériences de commande à l’aide de la classe StandardUICommand

Dérivé de [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) (dérivée de [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) pour C++ ou [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) pour C#), le [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) classe expose un ensemble de commandes de plate-forme standard avec des propriétés prédéfinies telles que l’icône de touche d’accès rapide et description.

Un [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) offre un moyen rapide et cohérent pour définir des commandes courantes telles que `Save` ou `Delete`. Il vous suffit est de fournir les fonctions execute et canExecute.

### <a name="example"></a>Exemple

![Exemple de StandardUICommand](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

| Télécharger le code de cet exemple |
| -------------------- |
| [Exemple d’exécution de commandes UWP (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip) |

Dans cet exemple, nous montrons comment améliorer un basic [ListView](listview-and-gridview.md) commande implémentée par le biais d’élément avec une suppression le [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) (classe), tout en optimisant l’expérience utilisateur pour une variété de types d’entrée à l’aide un [barre de menus](menus.md), [balayez](swipe.md) contrôle, des boutons de pointage, et [menu contextuel](menus.md).

> [!NOTE]
> Cet exemple requiert le package NuGet de Microsoft.UI.Xaml.Controls, une partie de la [bibliothèque d’interface utilisateur Microsoft Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

**XAML :**

L’exemple d’interface utilisateur inclut un [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de cinq éléments. La suppression [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) est lié à un [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), un [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), un [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), et [ Menu de ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

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

1. Tout d’abord, nous définissons un `ListItemData` classe qui contient une chaîne de texte et l’ICommand pour chaque ListViewItem dans notre ListView.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. Dans la classe MainPage, nous définissons une collection de `ListItemData` des objets pour le [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) de la [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate). Nous puis de le remplir avec une collection initiale de cinq éléments (avec texte et associé [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) supprimer).

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

4. Enfin, nous définissons les gestionnaires pour divers événements de ListView, y compris [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited), et [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) événements. Les gestionnaires d’événements de pointeur sont utilisés pour afficher ou masquer le bouton Supprimer pour chaque élément.

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

Si vous avez besoin créer une commande qui n’est pas définie par le [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) classe, ou si vous souhaitez mieux contrôler l’apparence de la commande, le [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) classe dérive de la [ ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) interface, ajout de diverses propriétés (par exemple, une icône d’étiquette, description et raccourcis clavier), méthodes et événements pour définir rapidement l’interface utilisateur et le comportement d’une commande personnalisée de l’interface utilisateur.

[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) vous permet de spécifier l’interface utilisateur via le contrôle de liaison, par exemple, une icône d’étiquette, description et des raccourcis (à la fois une clé d’accès et une touche d’accès rapide), de clavier sans avoir à définir les propriétés individuelles.

### <a name="example"></a>Exemple

![Exemple de XamlUICommand](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

| Télécharger le code de cet exemple |
| -------------------- |
| [Exemple d’exécution de commandes UWP (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip) |

Cet exemple partage les fonctionnalités de suppression de la précédente [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) exemple, mais montre comment la [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) classe vous permet de définir une commande de suppression personnalisées avec votre propre icône de police, étiquette, touche d’accès rapide et la description. Comme le [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) exemple, nous améliorer un basic [ListView](listview-and-gridview.md) de suppression d’élément commande implémentée par le biais le [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) (classe), tout en optimisant le expérience utilisateur pour différents types d’entrée à l’aide d’un [barre de menus](menus.md), [balayez](swipe.md) contrôle, des boutons de pointage, et [menu contextuel](menus.md).

De nombreux contrôles de plateforme utilisent les propriétés XamlUICommand en coulisses, tout comme notre exemple StandardUICommand dans la section précédente. 

> [!NOTE]
> Cet exemple requiert le package NuGet de Microsoft.UI.Xaml.Controls, une partie de la [bibliothèque d’interface utilisateur Microsoft Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

**XAML :**

L’exemple d’interface utilisateur inclut un [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de cinq éléments. Personnalisé [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) Delete est lié à un [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), un [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), un [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), et [ Menu de ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

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

1. Tout d’abord, nous définissons un `ListItemData` classe qui contient une chaîne de texte et l’ICommand pour chaque ListViewItem dans notre ListView.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. Dans la classe MainPage, nous définissons une collection de `ListItemData` des objets pour le [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) de la [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate). Nous puis de le remplir avec une collection initiale de cinq éléments (avec texte et associé [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)).

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

4. Enfin, nous définissons les gestionnaires pour divers événements de ListView, y compris [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited), et [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) événements. Les gestionnaires d’événements de pointeur sont utilisés pour afficher ou masquer le bouton Supprimer pour chaque élément.

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

Les contrôles UWP standards (bouton, liste, sélection, calendrier, texte prédictive) fournissent la base des nombreuses expériences de commande communes. Pour obtenir une liste complète des types de contrôle, consultez [contrôles et modèles pour les applications UWP](index.md).

La façon la plus simple pour prendre en charge une expérience d’exécution de commandes structurée consiste à définir une implémentation de l’interface ICommand ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) pour C++ ou [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)pour C#).  Cette instance ICommand peut ensuite être liée à des contrôles tels que des boutons.

> [!NOTE]
> Dans certains cas, il peut être aussi efficace pour lier une méthode à l’événement Click et une propriété à la propriété IsEnabled.

#### <a name="example"></a>Exemple

![Exemple d’interface de commande](images/commanding/icommand.gif)

*Exemple de ICommand*

| Télécharger le code de cet exemple |
| -------------------- |
| [Exemple d’exécution de commandes UWP (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip) |

Dans cet exemple de base, nous allons montrer comment une seule commande peut être appelée avec un bouton Cliquez, un accélérateur de clavier et faire pivoter une roulette de souris.

Nous utilisons deux [ListViews](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), un rempli avec cinq éléments et l’autres vide et deux boutons, une pour le déplacement d’éléments à partir de la ListView sur la gauche pour le ListView à droite, et l’autre pour le déplacement des éléments de droite à gauche. Chaque bouton est lié à une commande correspondante (ViewModel.MoveRightCommand et ViewModel.MoveLeftCommand, respectivement) et sont activées et désactivées automatiquement en fonction du nombre d’éléments dans leur ListView associée.

**Le code XAML suivant définit l’interface utilisateur pour notre exemple.**

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

Dans le code-behind, nous connecter à notre modèle de vue qui contient notre code de commande. En outre, nous définissons un gestionnaire pour l’entrée à partir de la roulette de souris qui se connecte également notre code de commande.

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

Notre modèle de vue est où nous définir les détails de l’exécution pour les deux commandes dans notre application, remplir un ListView et fournir un convertisseur de valeurs d’opacité de masquage ou affichage partie d’interface utilisateur supplémentaire selon le nombre d’éléments de chaque ListView.

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

Ici, nous définissons une commande qui implémente le [ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) interface et relaie simplement ses fonctionnalités à d’autres objets.

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

## <a name="summary"></a>Récapitulatif

La plateforme Windows universelle fournit un système de commande robuste et flexible qui vous permet de créer des applications qui partagent et gérer les commandes entre les types de contrôle, les appareils et les types d’entrée.

Utiliser les méthodes suivantes lors de la création de commandes pour vos applications UWP :

- Écouter et gérer des événements dans XAML/code-behind
- Lier à un événement tel que cliquez sur la méthode de gestion
- Définir votre propre implémentation ICommand
- Créer des objets de XamlUICommand avec vos propres valeurs pour un ensemble prédéfini de propriétés
- Créer des objets de StandardUICommand avec un ensemble de propriétés de la plateforme prédéfinis et des valeurs

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir un exemple complet qui illustre un [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) et [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) implémentation, consultez le [galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) exemple.

## <a name="see-also"></a>Voir aussi

[Contrôles et modèles pour les applications UWP](index.md)

### <a name="samples"></a>Exemples

#### <a name="topic-samples"></a>Exemples de la rubrique

- [Exemple d’exécution de commandes UWP (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip)
- [Exemple d’exécution de commandes UWP (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip)
- [Exemple d’exécution de commandes UWP (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip)

#### <a name="other-samples"></a>Autres exemples

- [Exemples de plate-forme de Windows universels (C# et C++)](https://go.microsoft.com/fwlink/?linkid=832713)
- [Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery)

<!---Some context for the following links goes here
- [link to next logical step for the customer](global-quickstart-template.md)--->

<!--- Required:
In Overview articles, provide at least one next step and no more than three.
Next steps in overview articles will often link to a quickstart.
Use regular links; do not use a blue box link. What you link to will depend on what is really a next step for the customer.
Do not use a "More info section" or a "Resources section" or a "See also section".
--->
