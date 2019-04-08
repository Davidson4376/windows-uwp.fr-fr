---
Description: Menus volants de barre de commande accédant les utilisateurs inline aux tâches les plus courantes de votre application.
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
ms.openlocfilehash: cb87bea001492e39a0f60b96f884db70b5bd28ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592524"
---
# <a name="command-bar-flyout"></a>Menu volant de barre de commandes

Le menu volant barre de commande vous permet de fournir aux utilisateurs un accès facile aux tâches courantes en indiquant les commandes dans une barre d’outils flottante lié à un élément sur le canevas de l’interface utilisateur.

![Un lanceur de barre de commande de texte développé](images/command-bar-flyout-header.png)

> CommandBarFlyout nécessite Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou le [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

> - **API de plateforme**: [Classe de CommandBarFlyout](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout classe](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [AppBarButton classe](/uwp/api/windows.ui.xaml.controls.appbarbutton), [AppBarToggleButton classe](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [ Classe de AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **API de bibliothèque de l’interface utilisateur de Windows**: [Classe de CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout classe](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

Comme [CommandBar](app-bars.md), CommandBarFlyout a **PrimaryCommands** et **SecondaryCommands** propriétés que vous pouvez utiliser pour ajouter des commandes. Vous pouvez placer des commandes dans la collection, ou les deux. Quand et comment les commandes principales et secondaires sont affichés varient selon le mode d’affichage.

La barre de commande menu volant a deux modes d’affichage : *réduit* et *développé*.

- Dans le mode réduit, uniquement les commandes principales sont affichés. Si votre barre de commande menu volant a principaux et secondaires des commandes, un bouton « voir plus d’informations », qui est représenté par des points de suspension \[•••\], s’affiche. Cela permet à l’utilisateur d’accéder aux commandes secondaire lors du passage en mode étendu.
- Dans le mode étendu, les commandes principales et secondaires sont affichés. (Si le contrôle a uniquement les éléments secondaires, elles sont affichées de manière similaire au contrôle MenuFlyout.)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez le contrôle de CommandBarFlyout pour afficher une collection de commandes à l’utilisateur, tels que des boutons et les éléments de menu, dans le contexte d’un élément sur le canevas de l’application.

Le TextCommandBarFlyout affiche les commandes de texte dans les contrôles TextBox, TextBlock, RichEditBox, RichTextBlock et PasswordBox. Les commandes sont automatiquement configurés de manière appropriée à la sélection de texte actuelle. Utiliser un CommandBarFlyout pour remplacer les commandes de texte par défaut sur les contrôles de texte.

Pour afficher contextuelles commandes sur les éléments de liste suivent les instructions de [commandes contextuelles pour les listes et les collections](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>Vs CommandBarFlyout MenuFlyout

Pour afficher les commandes dans un menu contextuel, vous pouvez utiliser CommandBarFlyout ou MenuFlyout. Nous vous recommandons CommandBarFlyout, car elle fournit davantage de fonctionnalités que MenuFlyout. Vous pouvez utiliser CommandBarFlyout avec uniquement les commandes secondaires pour obtenir le comportement et recherche d’un MenuFlyout ou utiliser le menu volant barre de commande complet avec les commandes principales et secondaires.

> Pour des informations connexes, consultez [menus volants](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [Menus et des menus contextuels](menus.md), et [barres de commandes](app-bars.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/CommandBarFlyout">ouvrez l’application et consultez le CommandBarFlyout en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Proactive et réactive appel

Il existe généralement deux façons d’appeler un menu volant ou un menu qui est associé à un élément sur le canevas de l’interface utilisateur : _invocation proactive_ et _invocation réactive_.

Dans l’appel proactive, les commandes apparaissent automatiquement lorsque l’utilisateur interagit avec l’élément qui les commandes associées. Par exemple, commandes de mise en forme de texte peuvent apparaître lorsque l’utilisateur sélectionne le texte dans une zone de texte. Dans ce cas, le Lanceur de barre de commande ne prend pas le focus. Au lieu de cela, elle présente des commandes proche de l’élément d’avec que l’utilisateur interagit. Si l’utilisateur n’interagit pas avec les commandes, ils sont ignorés.

Dans l’appel réactive, les commandes sont affichées en réponse à une action explicite de l’utilisateur pour demander les commandes ; par exemple, un clic droit. Cela correspond au concept traditionnel d’un [menu contextuel](menus.md).

Vous pouvez utiliser le CommandBarFlyout dans moyen, ou même un mélange des deux.

## <a name="create-a-command-bar-flyout"></a>Créer un lanceur de barre de commande

Cet exemple montre comment créer un lanceur de barre de commande et l’utiliser à la fois de façon proactive et réactive. Lorsque l’utilisateur appuie sur l’image, le menu volant est indiqué dans son mode réduit. Lors de son affichage en tant qu’un menu contextuel, le menu volant est indiqué dans son mode étendu. Dans les deux cas, l’utilisateur peut développer ou réduire le menu volant qu’il est ouvert.

![Exemple d’un lanceur de barre de commandes réduite](images/command-bar-flyout-img-collapsed.png)

> _Un lanceur de barre de commandes réduite_

![Exemple d’un menu volant barre de commande étendue](images/command-bar-flyout-img-expanded.png)

> _Un lanceur de barre de commande étendue_

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

### <a name="show-commands-proactively"></a>Afficher les commandes proactive

Lorsque vous affichez les commandes contextuelles de façon proactive, uniquement les commandes principales doivent être affichés par défaut (le Lanceur de barre de commande doit être réduit). Placez les commandes plus importantes de la collection de commandes principal et des commandes supplémentaires qui iraient traditionnellement dans un menu contextuel dans la collection de commandes secondaires.

Pour afficher les commandes de manière proactive, vous gérez en général le [cliquez sur](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) ou [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) événement pour afficher le menu volant barre de commande. Définir le Lanceur [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) à **temporaires** ou **TransientWithDismissOnPointerMoveAway** pour ouvrir le menu volant dans son mode réduit sans prendre le focus.

À compter de Windows 10 Insider Preview, les contrôles de texte ont une **SelectionFlyout** propriété. Lorsque vous affectez un menu volant à cette propriété, il s’affiche automatiquement quand le texte est sélectionné.

### <a name="show-commands-reactively"></a>Afficher les commandes de manière réactive

Lorsque vous affichez les commandes contextuelles réactive, comme un menu contextuel, les commandes secondaires sont affichées par défaut (le Lanceur de barre de commande doit être développé). Dans ce cas, le Lanceur de barre de commandes principales et secondaires de commandes, ou peut-être uniquement les commandes secondaire.

Pour afficher les commandes dans un menu contextuel, vous attribuez généralement le menu volant à la [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) propriété d’un élément d’interface utilisateur. De cette façon, ouvrant le menu volant est gérée par l’élément, et vous n’avez rien à faire plus.

Si vous gérez montrant le menu volant vous-même (par exemple, sur un [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) événement), définissez le Lanceur [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) à **Standard** pour ouvrir le menu volant dans son mode étendu et lui donner le focus.

> [!TIP]
> Pour plus d’informations sur les options lors de l’affichage d’un menu volant et comment contrôler l’emplacement du Lanceur, consultez [menus volants](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Commandes et contenu

Le contrôle CommandBarFlyout a 2 propriétés que vous pouvez utiliser pour ajouter des commandes et contenu : [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) et [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

Par défaut, les éléments de la barre de commandes sont ajoutés à la collection **PrimaryCommands**. Ces commandes sont affichent dans la barre de commandes et sont visibles dans les deux modes réduits et développés. Contrairement à CommandBar, les commandes principales ne le faites pas automatiquement un dépassement de capacité pour les commandes secondaire et risquent d’être tronqués.

Vous pouvez également ajouter des commandes à la **SecondaryCommands** collection. Commandes secondaires sont affichés dans la partie de menu du contrôle et sont visibles uniquement dans le mode étendu.

### <a name="app-bar-buttons"></a>Boutons de la barre de l’application

Vous pouvez remplir le PrimaryCommands et SecondaryCommands directement avec [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx), et [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) contrôles.

Les contrôles des boutons de la barre de l’application sont caractérisés par une icône et une étiquette avec un libellé. Ces contrôles sont optimisés pour une utilisation dans une barre de commandes, et leur apparence change selon que le contrôle est affiché dans la barre de commandes ou dans le menu de dépassement de capacité.

- Boutons de barre d’application utilisés en tant que commandes principales sont affichés dans la barre de commandes avec leur icône uniquement ; l’étiquette de texte n’est pas affiché. Nous vous recommandons d’utiliser une info-bulle à afficher une description de la commande, comme indiqué ici.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Boutons de barre de l’application utilisés comme commandes secondaires sont affichés dans le menu, avec l’icône visible et l’étiquette.

### <a name="other-content"></a>Autre contenu

Vous pouvez ajouter d’autres contrôles à un lanceur de barre de commandes en les encapsulant dans un AppBarElementContainer. Cela vous permet d’ajouter des contrôles tels que [DropDownButton](buttons.md) ou [SplitButton](buttons.md), ou ajouter des conteneurs comme [StackPanel](buttons.md) pour créer une interface utilisateur plus complexe.

Pour pouvoir être ajoutée aux collections de commande principal ou secondaire d’un lanceur de barre de commande, un élément doit implémenter le [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) interface. AppBarElementContainer est un wrapper qui implémente cette interface, vous pouvez ajouter un élément à une barre de commandes même si elle n’implémente pas l’interface elle-même.

Ici, un AppBarElementContainer est utilisé pour ajouter des éléments supplémentaires à un lanceur de barre de commandes. Un bouton partagé est ajouté aux commandes principales pour permettre la sélection de couleurs. Un StackPanel est ajouté aux commandes secondaire pour permettre une mise en page plus complexe pour les contrôles de zoom.

> [!TIP]
> Par défaut, les éléments conçus pour la zone de l’application ne peuvent pas s’affichent pas correctement dans une barre de commandes. Lorsque vous ajoutez un élément à l’aide de AppBarElementContainer, il existe quelques étapes à suivre pour rendre l’élément correspond à d’autres éléments de barre de commande :
>
> - Remplacer les pinceaux par défaut avec [style léger](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling) pour rendre l’élément en arrière-plan et bordure correspondent à des boutons de la barre d’application.
> - Ajuster la taille et la position de l’élément.
> - Encapsuler les icônes dans une fenêtre avec une largeur et une hauteur de 16px.

> [!NOTE]
> Cet exemple montre uniquement la commande barre menu volant l’interface utilisateur, qu'il n’implémente pas une des commandes qui sont affichés. Pour plus d’informations sur l’implémentation des commandes, consultez [boutons](buttons.md) et [principes fondamentaux de conception de commande](../basics/commanding-basics.md).

![Un lanceur de barre de commande avec un bouton partagé](images/command-bar-flyout-split-button.png)

> _Un lanceur de barre de commandes réduite avec un SplitButton open_

![Un lanceur de barre de commandes avec une interface utilisateur complexe](images/command-bar-flyout-custom-ui.png)

> _Un menu volant à la barre commande étendue avec le zoom personnalisé l’interface utilisateur dans le menu_


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Créer un menu contextuel avec uniquement les commandes secondaires

Vous pouvez utiliser un CommandBarFlyout avec uniquement les commandes secondaires en tant qu’un [menu contextuel](menus.md), à la place d’un MenuFlyout.

![Un lanceur de barre de commandes avec uniquement les commandes secondaires](images/command-bar-flyout-context-menu.png)

> _Menu volant de barre de commandes en tant qu’un menu contextuel_

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

![Un lanceur de barre de commandes avec comme un bouton menu déroulant](images/command-bar-flyout-dropdown.png)

> _Liste déroulante du bouton dans un menu volant barre de commande_

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

## <a name="command-bar-flyouts-for-text-controls"></a>Menus volants de barre de commande pour les contrôles de texte

Le [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) est un lanceur de barre de commande spécialisé qui contient des commandes de modification de texte. Chaque contrôle de texte affiche automatiquement le TextCommandBarFlyout en tant qu’un menu contextuel (clic droit), ou lorsque le texte est sélectionné. Le Lanceur de barre de commande de texte s’adapte à la sélection de texte pour afficher uniquement les commandes appropriées.

![Un lanceur de barre de commande de texte réduit](images/command-bar-flyout-text-selection.png)

> _Un lanceur de barre de commande de texte sur la sélection de texte_

![Un lanceur de barre de commande de texte développé](images/command-bar-flyout-text-full.png)

> _Un lanceur de barre de commande de texte développé_


### <a name="available-commands"></a>Commandes disponibles

Ce tableau montre les commandes qui sont incluses dans un TextCommandBarFlyout, et quand ils sont affichés.

| Commande | Indiqué... |
| ------- | -------- |
| Gras | Lorsque le contrôle de texte n’est pas en lecture seule (RichEditBox uniquement). |
| Italic | Lorsque le contrôle de texte n’est pas en lecture seule (RichEditBox uniquement). |
| Underline | Lorsque le contrôle de texte n’est pas en lecture seule (RichEditBox uniquement). |
| Vérification linguistique | Lorsque IsSpellCheckEnabled est **true** et le mot mal orthographié est sélectionné. |
| Cut | Lorsque le contrôle de texte n’est pas en lecture seule et texte est sélectionné. |
| Copier | Lorsque le texte est sélectionné. |
| Coller | Lorsque le contrôle de texte n’est pas en lecture seule et le Presse-papiers contient du contenu. |
| Undo | Lorsqu’il existe une action qui peut être annulée. |
| Tout sélectionner | Lorsque le texte peut être sélectionné. |

### <a name="custom-text-command-bar-flyouts"></a>Lanceurs de barre de commande de texte personnalisé

TextCommandBarFlyout ne peuvent pas être personnalisé et est géré automatiquement par chaque contrôle de texte. Toutefois, vous pouvez remplacer la valeur par défaut TextCommandBarFlyout grâce aux commandes personnalisées.

- Pour remplacer la valeur par défaut TextCommandBarFlyout qui s’affiche sur la sélection de texte, vous pouvez créer un CommandBarFlyout personnalisé (ou autre type de menu volant) et assignez-la à la **SelectionFlyout** propriété. Si vous définissez SelectionFlyout sur **null**, aucune commande ne figurent sur la sélection.
- Pour remplacer la valeur par défaut TextCommandBarFlyout est affiché comme le menu contextuel, affecter un CommandBarFlyout personnalisé (ou un autre type de menu volant) à la **ContextFlyout** propriété sur un contrôle de texte. Si vous définissez ContextFlyout sur **null**, le menu volant indiqué dans les versions précédentes du texte contrôle est affiché à la place le TextCommandBarFlyout.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) - Affichez tous les contrôles XAML dans un format interactif.
- [Exemples de commandes de XAML](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Articles connexes

- [Principes fondamentaux de conception de commande pour les applications UWP](../basics/commanding-basics.md)
- [Classe de CommandBar](https://msdn.microsoft.com/library/windows/apps/dn279427)
