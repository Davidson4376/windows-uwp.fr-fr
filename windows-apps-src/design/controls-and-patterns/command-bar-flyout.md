---
Description: Les menus volants de barre de commandes procurent aux utilisateurs un accès inline aux tâches les plus courantes de votre application.
title: Menu volant de barre de commandes
label: Command bar flyout
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9793679dcd036415ba6a1c238c1986392beb5761
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319144"
---
# <a name="command-bar-flyout"></a>Menu volant de barre de commandes

Le menu volant de barre de commandes vous permet de fournir aux utilisateurs un accès facile aux tâches courantes en affichant les commandes dans une barre d’outils flottante liée à un élément sur le canevas de l’interface utilisateur.

![Menu volant de barre de commandes de texte développé](images/command-bar-flyout-header.png)

> CommandBarFlyout nécessite Windows 10 version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou ultérieure, ou la [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

> - **API de plateforme** : [classe CommandBarFlyout](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [classe TextCommandBarFlyout ](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [classe AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton), [classe AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [classe AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **API de la bibliothèque d’interface utilisateur Windows** : [classe CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [classe TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

Comme [CommandBar](app-bars.md), CommandBarFlyout a des propriétés **PrimaryCommands** et **SecondaryCommands** que vous pouvez utiliser pour ajouter des commandes. Vous pouvez placer des commandes dans une des collections ou dans les deux. Le mode d’affichage détermine à quel moment et comment les commandes principales et secondaires sont affichées.

Le menu volant de barre de commandes a deux modes d’affichage : *réduit* et *développé*.

- En mode réduit, seules les commandes principales sont affichées. Si votre menu volant de barre de commandes a des commandes principales et secondaires, un bouton « Plus d’informations », représenté par des points de suspension \[•••\], s’affiche. Il permet à l’utilisateur d’accéder aux commandes secondaires en passant au mode développé.
- En mode développé, les commandes principales et secondaires sont affichées. (Si le contrôle a uniquement des éléments secondaires, ils sont affichés de manière similaire au contrôle MenuFlyout.)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez le contrôle CommandBarFlyout pour afficher une collection de commandes à l’utilisateur, telles que des boutons et des éléments de menu, dans le contexte d’un élément sur le canevas de l’application.

Le TextCommandBarFlyout affiche les commandes de texte dans des contrôles TextBox, TextBlock, RichEditBox, RichTextBlock et PasswordBox. Les commandes sont automatiquement configurées en fonction de la sélection de texte actuelle. Utilisez un CommandBarFlyout pour remplacer les commandes de texte par défaut sur les contrôles de texte.

Pour afficher des commandes contextuelles sur des éléments de liste, suivez les instructions indiquées dans [Commandes contextuelles pour les collections et les listes](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout ou MenuFlyout

Pour afficher des commandes dans un menu contextuel, vous pouvez utiliser CommandBarFlyout ou MenuFlyout. Nous vous recommandons CommandBarFlyout, qui fournit davantage de fonctionnalités que MenuFlyout. Vous pouvez utiliser CommandBarFlyout avec uniquement des commandes secondaires pour obtenir le comportement et l’aspect d’un MenuFlyout ou utiliser le menu volant de barre de commandes complet avec des commandes principales et secondaires.

> Pour des informations connexes, consultez [Menus volants](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [Menus et menus contextuels](menus.md) et [Barres de commandes](app-bars.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/CommandBarFlyout">ouvrir l’application et voir le contrôle CommandBarFlyout en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Appel proactif ou réactif

Il existe généralement deux façons d’appeler un menu volant ou un menu associé à un élément sur le canevas de l’interface utilisateur : _appel proactif_ et _appel réactif_.

Dans le cas d’un appel proactif, les commandes apparaissent automatiquement quand l’utilisateur interagit avec l’élément avec lequel elles sont associées. Par exemple, les commandes de mise en forme de texte peuvent apparaître quand l’utilisateur sélectionne du texte dans une zone de texte. Dans ce cas, le menu volant de barre de commandes ne prend pas le focus. Au lieu de cela, il présente des commandes pertinentes proches de l’élément avec lequel l’utilisateur interagit. Si l’utilisateur n’interagit pas avec les commandes, elles sont ignorées.

Dans le cas d’un appel réactif, les commandes sont affichées en réponse à une action explicite de l’utilisateur pour demander les commandes ; par exemple, un clic droit. Cela correspond au concept traditionnel de [menu contextuel](menus.md).

Vous pouvez utiliser le CommandBarFlyout dans un cas de figure, comme dans l’autre.

## <a name="create-a-command-bar-flyout"></a>Créer un menu volant de barre de commandes

Cet exemple montre comment créer un menu volant de barre de commandes et l’utiliser à la fois de façon proactive et réactive. Quand l’utilisateur appuie sur l’image, le menu volant est affiché en mode réduit. Quand il se présente sous la forme d’un menu contextuel, le menu volant est affiché en mode développé. Dans les deux cas, l’utilisateur peut développer ou réduire le menu volant une fois celui-ci ouvert.

![Exemple de menu volant de barre de commandes réduit](images/command-bar-flyout-img-collapsed.png)

> _Menu volant de barre de commandes réduit_

![Exemple de menu volant de barre de commandes développé](images/command-bar-flyout-img-expanded.png)

> _Menu volant de barre de commandes développé_

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Select all"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           Tapped="Image_Tapped" FlyoutBase.AttachedFlyout="{x:Bind ImageCommandsFlyout}"
           ContextFlyout="{x:Bind ImageCommandsFlyout}"/>
</Grid>
```

```csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    var flyout = FlyoutBase.GetAttachedFlyout((FrameworkElement)sender);
    var options = new FlyoutShowOptions()
    {
        // Position shows the flyout next to the pointer.
        // "Transient" ShowMode makes the flyout open in its collapsed state.
        Position = e.GetPosition((FrameworkElement)sender),
        ShowMode = FlyoutShowMode.Transient
    };
    flyout?.ShowAt((FrameworkElement)sender, options);
}
```

### <a name="show-commands-proactively"></a>Afficher les commandes de façon proactive

Quand vous affichez les commandes contextuelles de façon proactive, seules les commandes principales doivent être affichées par défaut (le menu volant de barre de commandes doit être réduit). Placez les commandes les plus importantes dans la collection de commandes principales et les commandes supplémentaires qui iraient d’ordinaire dans un menu contextuel dans la collection de commandes secondaires.

Pour afficher les commandes de manière proactive, vous gérez en général l’événement [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) ou [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) pour afficher le menu volant de barre de commandes. Définissez le [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) du menu volant sur **Transient** ou **TransientWithDismissOnPointerMoveAway** pour ouvrir le menu volant en mode réduit sans prise du focus.

À compter de Windows 10 Insider Preview, les contrôles de texte ont une propriété **SelectionFlyout**. Quand vous affectez un menu volant à cette propriété, il s’affiche automatiquement quand du texte est sélectionné.

### <a name="show-commands-reactively"></a>Afficher les commandes de façon réactive

Quand vous affichez les commandes contextuelles de façon réactive, sous la forme d’un menu contextuel, les commandes secondaires sont affichées par défaut (le menu volant de barre de commandes doit être développé). Dans ce cas, le menu volant de barre de commandes peut avoir des commandes principales et secondaires, ou uniquement des commandes secondaires.

Pour afficher des commandes dans un menu contextuel, vous attribuez généralement le menu volant à la propriété [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) d’un élément d’interface utilisateur. De cette façon, l’ouverture du menu volant est gérée par l’élément, et vous n’avez rien à faire de plus.

Si vous gérez l’affichage du menu volant vous-même (par exemple, sur un événement [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped)), définissez le [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) du menu volant sur **Standard** pour ouvrir le menu volant en mode développé et lui donner le focus.

> [!TIP]
> Pour plus d’informations sur les options d’affichage d’un menu volant et sur la façon de contrôler son emplacement, consultez [Menus volants](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Commandes et contenu

Le contrôle CommandBarFlyout a 2 propriétés que vous pouvez utiliser pour ajouter des commandes et du contenu : [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) et [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

Par défaut, les éléments de la barre de commandes sont ajoutés à la collection **PrimaryCommands**. Ces commandes sont affichées dans la barre de commandes et sont visibles dans les deux modes réduit et développé. Contrairement à CommandBar, les commandes principales ne débordent pas automatiquement sur les commandes secondaires et risquent d’être tronquées.

Vous pouvez également ajouter des commandes à la collection **SecondaryCommands**. Les commandes secondaires sont affichées dans la partie de menu du contrôle et sont visibles uniquement en mode développé.

### <a name="app-bar-buttons"></a>Boutons de la barre de l’application

Vous pouvez remplir les propriétés PrimaryCommands et SecondaryCommands directement avec les contrôles [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton), [AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) et [AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator).

Les contrôles des boutons de la barre de l’application sont caractérisés par une icône et une étiquette avec un libellé. Ces contrôles sont optimisés pour une utilisation dans une barre de commandes, et leur apparence change selon qu’ils sont affichés dans la barre de commandes ou dans le menu de dépassement.

- Les boutons de barre d’application utilisés en tant que commandes principales sont affichés dans la barre de commandes avec leur icône uniquement ; l’étiquette de texte n’est pas affichée. Nous vous recommandons d’utiliser une info-bulle pour afficher une description de la commande, comme illustré ici.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Les boutons de barre d’application utilisés en tant que commandes secondaires sont affichés dans le menu, avec l’icône et l’étiquette visibles.

### <a name="other-content"></a>Autre contenu

Vous pouvez ajouter d’autres contrôles à un menu volant de barre de commandes en les wrappant dans un AppBarElementContainer. Cela vous permet d’ajouter des contrôles tels que [DropDownButton](buttons.md) ou [SplitButton](buttons.md), ou d’ajouter des conteneurs comme [StackPanel](buttons.md) pour créer une interface utilisateur plus complexe.

Pour pouvoir être ajouté aux collections de commandes principales ou secondaires d’un menu volant de barre de commandes, un élément doit implémenter l’interface [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement). AppBarElementContainer est un wrapper qui implémente cette interface ; ainsi, vous pouvez ajouter un élément à une barre de commandes même s’il n’implémente pas l’interface lui-même.

Ici, un AppBarElementContainer est utilisé pour ajouter des éléments à un menu volant de barre de commandes. Un SplitButton est ajouté aux commandes principales pour permettre la sélection de couleurs. Un StackPanel est ajouté aux commandes secondaires pour permettre une mise en page plus complexe des contrôles de zoom.

> [!TIP]
> Par défaut, les éléments conçus pour le canevas de l’application peuvent ne pas avoir l’apparence appropriée dans une barre de commandes. Quand vous ajoutez un élément à l’aide d’AppBarElementContainer, vous devez suivre quelques étapes pour que l’élément corresponde à d’autres éléments de la barre de commandes :
>
> - Remplacez les propriétés de pinceau par défaut en adoptant un [style léger](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling) afin que l’arrière-plan et la bordure de l’élément correspondent aux boutons de la barre d’application.
> - Ajustez la taille et la position de l’élément.
> - Wrappez les icônes dans un Viewbox avec une largeur et une hauteur de 16px.

> [!NOTE]
> Cet exemple montre uniquement l’interface utilisateur du menu volant de la barre de commandes ; il n’implémente aucune des commandes affichées. Pour plus d’informations sur l’implémentation des commandes, consultez [Boutons](buttons.md) et [Informations de base sur la conception des commandes](../basics/commanding-basics.md).

![Menu volant de barre de commandes avec un bouton partagé](images/command-bar-flyout-split-button.png)

> _Menu volant de barre de commandes réduit avec un bouton partagé ouvert_

![Menu volant de barre de commandes avec une interface utilisateur complexe](images/command-bar-flyout-custom-ui.png)

> _Menu volant de barre de commandes développé avec une interface utilisateur de zoom personnalisée dans le menu_


```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Alignment controls -->
    <AppBarElementContainer>
        <SplitButton ToolTipService.ToolTip="Alignment">
            <SplitButton.Resources>
                <!-- Override default brushes to make the SplitButton 
                     match other command bar elements. -->
                <Style TargetType="SplitButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="SplitButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrush" Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </SplitButton.Resources>
            <SplitButton.Content>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="AlignLeft"/>
                </Viewbox>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Icon="AlignLeft" Text="Align left"/>
                    <MenuFlyoutItem Icon="AlignCenter" Text="Center"/>
                    <MenuFlyoutItem Icon="AlignRight" Text="Align right"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Alignment controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <!-- Override default brushes to make the Buttons 
                     match other command bar elements. -->
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
                <Style TargetType="Button">
                    <Setter Property="Height" Value="40"/>
                    <Setter Property="Width" Value="40"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,-4">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="76"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="Zoom"/>
                </Viewbox>
                <TextBlock Text="Zoom" Margin="10,0,0,0" Grid.Column="1"/>
                <StackPanel Orientation="Horizontal" Grid.Column="2">
                    <Button ToolTipService.ToolTip="Zoom out">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomOut"/>
                        </Viewbox>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button ToolTipService.ToolTip="Zoom in">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomIn"/>
                        </Viewbox>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all" Icon="SelectAll"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Créer un menu contextuel avec uniquement des commandes secondaires

Vous pouvez utiliser un CommandBarFlyout avec uniquement des commandes secondaires en tant que [menu contextuel](menus.md), à la place d’un MenuFlyout.

![Menu volant de barre de commandes avec uniquement des commandes secondaires](images/command-bar-flyout-context-menu.png)

> _Menu volant de barre de commandes sous la forme d’un menu contextuel_

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarButton Label="Save" Icon="Save"/>
                <AppBarButton Label="Print" Icon="Print"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

Vous pouvez également utiliser un CommandBarFlyout avec un DropDownButton pour créer un menu standard.

![Menu volant de barre de commandes avec un bouton déroulant](images/command-bar-flyout-dropdown.png)

> _Bouton déroulant dans un menu volant de barre de commande_

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Placeholder"/>
    <AppBarElementContainer>
        <DropDownButton Content="Mail">
            <DropDownButton.Resources>
                <!-- Override default brushes to make the DropDownButton 
                     match other command bar elements. -->
                <Style TargetType="DropDownButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>

                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </DropDownButton.Resources>
            <DropDownButton.Flyout>
                <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
                    <CommandBarFlyout.SecondaryCommands>
                        <AppBarButton Icon="MailReply" Label="Reply"/>
                        <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
                        <AppBarButton Icon="MailForward" Label="Forward"/>
                    </CommandBarFlyout.SecondaryCommands>
                </CommandBarFlyout>
            </DropDownButton.Flyout>
        </DropDownButton>
    </AppBarElementContainer>
    <AppBarButton Icon="Placeholder"/>
    <AppBarButton Icon="Placeholder"/>
</CommandBarFlyout>
```

## <a name="command-bar-flyouts-for-text-controls"></a>Menus volants de barre de commandes pour les contrôles de texte

Le [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) est un menu volant de barre de commandes spécialisé qui contient des commandes de modification de texte. Chaque contrôle de texte affiche automatiquement le TextCommandBarFlyout sous la forme d’un menu contextuel (clic droit) ou quand du texte est sélectionné. Le menu volant de barre de commandes de texte s’adapte à la sélection du texte pour n’afficher que les commandes appropriées.

![Menu volant de barre de commandes de texte réduit](images/command-bar-flyout-text-selection.png)

> _Menu volant de barre de commandes de texte associé à une sélection de texte_

![Menu volant de barre de commandes de texte développé](images/command-bar-flyout-text-full.png)

> _Menu volant de barre de commandes de texte développé_


### <a name="available-commands"></a>Commandes disponibles

Le tableau ci-après montre les commandes qui sont incluses dans un TextCommandBarFlyout et indique quand elles sont affichées.

| Commande | Affichée... |
| ------- | -------- |
| Gras | quand le contrôle de texte n’est pas en lecture seule (RichEditBox uniquement). |
| Italic | quand le contrôle de texte n’est pas en lecture seule (RichEditBox uniquement). |
| Underline | quand le contrôle de texte n’est pas en lecture seule (RichEditBox uniquement). |
| Vérification linguistique | quand IsSpellCheckEnabled est **true** et que du texte mal orthographié est sélectionné. |
| Cut | quand le contrôle de texte n’est pas en lecture seule et que du texte est sélectionné. |
| Copier | quand du texte est sélectionné. |
| Coller | quand le contrôle de texte n’est pas en lecture seule et que le Presse-papiers a du contenu. |
| Undo | quand une action peut être annulée. |
| Tout sélectionner | quand du texte peut être sélectionné. |

### <a name="custom-text-command-bar-flyouts"></a>Menus volants de barre de commandes de texte personnalisés

TextCommandBarFlyout ne peut pas être personnalisé et est géré automatiquement par chaque contrôle de texte. Toutefois, vous pouvez remplacer le TextCommandBarFlyout par défaut par des commandes personnalisées.

- Pour remplacer le TextCommandBarFlyout par défaut qui s’affiche quand du texte est sélectionné, vous pouvez créer un CommandBarFlyout personnalisé (ou un autre type de menu volant) et l’assigner à la propriété **SelectionFlyout**. Si vous définissez SelectionFlyout sur **null**, aucune commande n’apparaît au moment de la sélection.
- Pour remplacer le TextCommandBarFlyout par défaut qui s’affiche sous la forme d’un menu contextuel, affectez un CommandBarFlyout personnalisé (ou un autre type de menu volant) à la propriété **ContextFlyout** sur un contrôle de texte. Si vous définissez ContextFlyout sur **null**, le menu volant affiché dans les versions précédentes du contrôle de texte apparaît à la place du TextCommandBarFlyout.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.
- [Exemple de commandes XAML](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Articles connexes

- [Notions de base de la conception des commandes pour les applications UWP](../basics/commanding-basics.md)
- [Classe CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
