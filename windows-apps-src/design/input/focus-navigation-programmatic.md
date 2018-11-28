---
Description: Learn how to programmatically manage focus navigation with keyboard, gamepad, and accessibility tools in a UWP app.
title: Navigation en mode focus programmé avec un clavier, un boîtier de commande, et des outils d’accessibilité
label: Programmatic focus navigation
keywords: clavier, contrôleur de jeu, télécommande, navigation, stratégie de navigation, entrée, interaction utilisateur, accessibilité, facilité d’utilisation
ms.date: 03/19/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 64a15526b32be52b0fc667dc6f8b549d6faae8f1
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7827945"
---
# <a name="programmatic-focus-navigation"></a>Navigation en mode focus programmé

![Clavier, télécommande et bouton multidirectionnel](images/dpad-remote/dpad-remote-keyboard.png)

Pour déplacer le focus par programmation dans votre application UWP, vous pouvez utiliser la méthode [FocusManager.TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) ou la méthode [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_).

[TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) tente de déplacer le focus à partir de l’élément ayant le focus vers le prochain élément susceptible de recevoir le focus dans la direction spécifiée, tandis que [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) récupère l’élément (comme un [DependencyObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject)) qui recevra le focus en fonction de la direction de navigation spécifiée (navigation directionnelle uniquement, ne peut pas être utilisé pour émuler la navigation par tabulation).

> [!NOTE]
> Nous vous recommandons d’utiliser la méthode [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) au lieu de [FindNextFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextFocusableElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) car FindNextFocusableElement extrait un UIElement, qui retourne la valeur null si le prochain élément pouvant recevoir le focus n’est pas un UIElement (comme un objet Hyperlink). 

## <a name="find-a-focus-candidate-within-a-scope"></a>Détecter un candidat au focus dans une étendue

Vous pouvez personnaliser le comportement de navigation en mode focus pour [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) et [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_), y compris la recherche du candidat au focus suivant dans une arborescence de l’interface utilisateur spécifique ou en excluant certains éléments spécifiques.

Cet exemple utilise un jeu TicTacToe pour illustrer les méthodes [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) et [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_).

```xaml
<StackPanel Orientation="Horizontal"
                VerticalAlignment="Center"
                HorizontalAlignment="Center" >
    <Button Content="Start Game" />
    <Button Content="Undo Movement" />
    <Grid x:Name="TicTacToeGrid" KeyDown="OnKeyDown">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="0" 
            x:Name="Cell00" />
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="0" 
            x:Name="Cell10"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="0" 
            x:Name="Cell20"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="1" 
            x:Name="Cell01"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="1" 
            x:Name="Cell11"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="1" 
            x:Name="Cell21"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="2" 
            x:Name="Cell02"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="2" 
            x:Name="Cell22"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="2" 
            x:Name="Cell32"/>
    </Grid>
</StackPanel>
```

```csharp
private void OnKeyDown(object sender, KeyRoutedEventArgs e)
{
    DependencyObject candidate = null;

    var options = new FindNextElementOptions ()
    {
        SearchRoot = TicTacToeGrid,
        NavigationStrategy = NavigationStrategyMode.Heuristic
    };

    switch (e.Key)
    {
        case Windows.System.VirtualKey.Up:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Up, options);
            break;
        case Windows.System.VirtualKey.Down:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Down, options);
            break;
        case Windows.System.VirtualKey.Left:
            candidate = FocusManager.FindNextElement(
                FocusNavigationDirection.Left, options);
            break;
        case Windows.System.VirtualKey.Right:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Right, options);
            break;
    }
    // Also consider whether candidate is a Hyperlink, WebView, or TextBlock.
    if (candidate != null && candidate is Control)
    {
        (candidate as Control).Focus(FocusState.Keyboard);
    }
}
```

Utilisez [FindNextElementOptions](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.findnextelementoptions) pour personnaliser davantage la façon dont les candidats au focus sont identifiés. Cet objet fournit les propriétés suivantes:

- [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot)-Étend la recherche des candidats de la navigation en mode focus aux enfants de ce DependencyObject. La valeur null indique de lancer la recherche à partir de la racine de l’arborescence visuelle.

> [!Important] 
> Si une ou plusieurs transformations sont appliquées aux descendants du **SearchRoot** qui les placent en dehors de la zone directionnelle, ces éléments sont toujours considérés comme candidats.

- [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect)-Les candidats à la navigation en mode focus sont identifiés à l’aide d’un rectangle englobant «fictif», où tous les objets qui se chevauchent sont exclus du focus de navigation. Ce rectangle est utilisé uniquement pour les calculs et n’est jamais ajouté à l’arborescence visuelle.
- [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect)-Les candidats à la navigation en mode focus sont identifiés à l’aide d’un rectangle englobant «fictif» qui identifie les éléments les plus susceptibles de recevoir le focus. Ce rectangle est utilisé uniquement pour les calculs et n’est jamais ajouté à l’arborescence visuelle.
- [XYFocusNavigationStrategyOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_XYFocusNavigationStrategyOverride)-Stratégie de navigation en mode focus utilisée pour identifier le meilleur élément candidat pour recevoir le focus.

L’image suivante illustre certains de ces concepts. 

Lorsque l’élément B a le focus, FindNextElement identifie I comme le candidat au focus lors de la navigation vers la droite. Cela pour les raisons suivantes:
- Comme le [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) est sur A, la référence de départ est A, et non B
- C n’est pas un candidat car MyPanel a été spécifié comme le [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot)
- F n’est pas un candidat car [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) le chevauche

![Comportement de navigation en mode focus personnalisé à l’aide des astuces de navigation](images/keyboard/navigation-hints.png)

*Comportement de navigation en mode focus personnalisé à l’aide des astuces de navigation*

## <a name="navigation-focus-events"></a>Événements de focus de navigation

### <a name="nofocuscandidatefound-event"></a>Événement NoFocusCandidateFound

L’événement [UIElement.NoFocusCandidateFound](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_NoFocusCandidateFound) est déclenché lorsque la touche Tab ou les touches de direction sont actionnées et qu’il n’existe aucun candidat au focus dans la direction spécifiée. Cet événement n’est pas déclenché pour [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_).

Dans la mesure où il s’agit d’un événement routé, il se propage à partir de l’élément sur lequel se trouve le focus par le biais d’objets parents successifs vers la racine de l’arborescence des objets. Cela vous permet de gérer l’événement, le cas échéant.

<a name="split-view-code-sample"></a>

L’exemple qui suit montre un élément Grid qui ouvre un [SplitView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview) lorsque l’utilisateur tente de déplacer le focus vers la gauche du contrôle susceptible de recevoir le focus placé le plus gauche (voir [Conception pour Xbox et TV](../devices/designing-for-tv.md#navigation-pane)).

```xaml
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">
...
</Grid>
```

```csharp
private void OnNoFocusCandidateFound (
    UIElement sender, NoFocusCandidateFoundEventArgs args)
{
    if(args.NavigationDirection == FocusNavigationDirection.Left)
    {
        if(args.InputDevice == FocusInputDeviceKind.Keyboard ||
        args.InputDevice == FocusInputDeviceKind.GameController )
            {
                OpenSplitPaneView();
            }
        args.Handled = true;
    }
}
```

### <a name="gotfocus-and-lostfocus-events"></a>Événements GotFocus et LostFocus
Les événements [UIElement.GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) et [UIElement.LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) sont déclenchés lorsqu’un élément reçoit ou perd le focus, respectivement. Cet événement n’est pas déclenché pour [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_).

Dans la mesure où il s’agit d’événements routés, ils se propagent à partir de l’élément sur lequel se trouve le focus par le biais d’objets parents successifs vers la racine de l’arborescence des objets. Cela vous permet de gérer l’événement, le cas échéant.

### <a name="gettingfocus-and-losingfocus-events"></a>Événements GettingFocus et LosingFocus

Les événements [UIElement.GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) et [UIElement.LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) sont déclenchés avant les événements [UIElement.GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) et [UIElement.LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) respectifs. 

Dans la mesure où il s’agit d’événements routés, ils se propagent à partir de l’élément sur lequel se trouve le focus par le biais d’objets parents successifs vers la racine de l’arborescence des objets. Comme cela se produit avant un changement de focus, vous pouvez rediriger ou annuler le changement de focus.

[GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) et [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) sont des événements synchrones, c’est pourquoi le focus ne doit pas être déplacé pendant que ces événements sont cours de propagation. Cependant, comme [GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) et [LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) sont des événements asynchrones, rien ne garantit que le focus ne se déplacera plus avant l’exécution du gestionnaire.

si le focus se déplace par le biais d’un appel à [Control.Focus](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_), [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) est déclenché lors de l’appel, tandis que [GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) est déclenché après l’appel.

La cible de la navigation en mode focus peut être modifiée pendant les événements [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) et [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) (avant que le focus ne se déplace) via la propriété [GettingFocusEventArgs.NewFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_NewFocusedElement). Même si la cible change, l’événement se propage toujours et la cible peut être modifiée une nouvelle fois.

Pour éviter les problèmes de réentrance, une exception est générée si vous tentez de déplacer le focus (à l’aide de [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) ou de [Control.Focus](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_)) alors que ces événements sont en cours de propagation.

Ces événements sont déclenchés quelle que soit la raison du déplacement du focus (y compris la navigation par tabulation, la navigation directionnelle et la navigation programmée).

Voici l’ordre d’exécution des événements de focus:

1.  [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) Si le focus est réinitialisé sur l’élément perdant le focus ou [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.losingfocuseventargs#Windows_UI_Xaml_Input_LosingFocusEventArgs_TryCancel) a réussi, aucun événement supplémentaire n’est déclenché.
2.  [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) Si le focus est réinitialisé sur l’élément perdant le focus ou [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_TryCancel) a réussi, aucun événement supplémentaire n’est déclenché.
3.  [LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)
4.  [GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)

L’image suivante montre comment, lors du déplacement vers la droite à partir de A, XYFocus choisit B4 comme candidat. Puis, B4 déclenche l’événement GettingFocus dans le cadre duquel ListView dispose de la possibilité de replacer le focus sur B3.

![Modification de la cible de navigation en mode focus sur l’événement GettingFocus](images/keyboard/focus-events.png)

*Modification de la cible de navigation en mode focus sur l’événement GettingFocus*

Voici comment gérer l’événement [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) et rediriger le focus.

```XAML
<StackPanel Orientation="Horizontal">
    <Button Content="A" />
    <ListView x:Name="MyListView" SelectedIndex="2" GettingFocus="OnGettingFocus">
        <ListViewItem>LV1</ListViewItem>
        <ListViewItem>LV2</ListViewItem>
        <ListViewItem>LV3</ListViewItem>
        <ListViewItem>LV4</ListViewItem>
        <ListViewItem>LV5</ListViewItem>
    </ListView>
</StackPanel>
```

```csharp
private void OnGettingFocus(UIElement sender, GettingFocusEventArgs args)
{
    //Redirect the focus only when the focus comes from outside of the ListView.
    // move the focus to the selected item
    if (MyListView.SelectedIndex != -1 && 
        IsNotAChildOf(MyListView, args.OldFocusedElement))
    {
        var selectedContainer = 
            MyListView.ContainerFromItem(MyListView.SelectedItem);
        if (args.FocusState == 
            FocusState.Keyboard && 
            args.NewFocusedElement != selectedContainer)
        {
            args.TryRedirect(
                MyListView.ContainerFromItem(MyListView.SelectedItem));
            args.Handled = true;
        }
    }
}
```

Voici comment gérer l’événement [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) pour un [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) et définir le focus lorsque le menu est fermé.

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
     <AppBarButton Icon="Back" Label="Back" />
     <AppBarButton Icon="Stop" Label="Stop" />
     <AppBarButton Icon="Play" Label="Play" />
     <AppBarButton Icon="Forward" Label="Forward" />

     <CommandBar.SecondaryCommands>
         <AppBarButton Icon="Like" Label="Like" />
         <AppBarButton Icon="Share" Label="Share" />
     </CommandBar.SecondaryCommands>
 </CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (MyCommandBar.IsOpen == true && 
        IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
    {
        if (args.TryCancel())
        {
            args.Handled = true;
        }
    }
}
```

## <a name="find-the-first-and-last-focusable-element"></a>Détecter le premier et le dernier élément susceptible de recevoir le focus

Les méthodes [FocusManager.FindFirstFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindFirstFocusableElement_Windows_UI_Xaml_DependencyObject_) et [FocusManager.FindLastFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindLastFocusableElement_Windows_UI_Xaml_DependencyObject_) vous permettent de déplacer le focus vers le premier ou le dernier élément susceptible de le recevoir dans l’étendue d’un objet (l’arborescence d’un élément [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) ou l’arborescence texte d’un objet [TextElement](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.documents.textelement)). L’étendue est spécifiée dans l’appel (si l’argument est null, l’étendue est la fenêtre active).

Si aucun candidat au focus ne peut être identifié dans l’étendue, la valeur null est renvoyé.

Voici comment spécifier que les boutons d’un CommandBar présentent un comportement directionnel enveloppant (voir [Interactions avec le clavier](keyboard-interactions.md#popup-ui)).

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
    <AppBarButton Icon="Back" Label="Back" />
    <AppBarButton Icon="Stop" Label="Stop" />
    <AppBarButton Icon="Play" Label="Play" />
    <AppBarButton Icon="Forward" Label="Forward" />

    <CommandBar.SecondaryCommands>
        <AppBarButton Icon="Like" Label="Like" />
        <AppBarButton Icon="ReShare" Label="Share" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (IsNotAChildOf(MyCommandBar, args.NewFocussedElement))
    {
        DependencyObject candidate = null;
        if (args.Direction == FocusNavigationDirection.Left)
        {
            candidate = FocusManager.FindLastFocusableElement(MyCommandBar);
        }
        else if (args.Direction == FocusNavigationDirection.Right)
        {
            candidate = FocusManager.FindFirstFocusableElement(MyCommandBar);
        }
        if (candidate != null)
        {
            args.NewFocusedElement = candidate;
            args.Handled = true;
        }
    }
}
```

## <a name="related-articles"></a>Articles associés

- [Navigation en mode focus pour clavier, boîtier de commande, télécommande et outils d’accessibilité](focus-navigation.md)
- [Interactions avec le clavier](keyboard-interactions.md)
- [Accessibilité du clavier](../accessibility/keyboard-accessibility.md)