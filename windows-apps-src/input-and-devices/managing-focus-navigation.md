---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 8389d1f089d507a087693879a793b95572d7860b
ms.sourcegitcommit: 5ece992c31870df4c089360ef47501bd4ce14fa9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2017
---
# <a name="managing-focus-navigation"></a>Gestion de la navigation en mode focus

Nombre de dispositifs de saisie, notamment le clavier, les outils d’accessibilité tels que Windows Narrator, le boîtier de commande et la télécommande, partagent un mécanisme sous-jacent commun pour déplacer le visuel du focus dans l’interface utilisateur de vos applications. Pour en savoir plus sur le visuel du focus et la navigation, lisez le document [Interactions avec le clavier](keyboard-interactions.md), ainsi que le document [Conception pour Xbox et téléviseur](designing-for-tv.md#xy-focus-navigation-and-interaction).

Ensuite, fournissez un moyen indépendant d’entrée pour que le focus se déplace dans l’interface utilisateur de l’application, ce qui vous permet d’écrire un code unique permettant un excellent fonctionnement avec plusieurs types d’entrées.

## <a name="navigation-strategies-properties-to-fine-tune-focus-movements"></a>Propriétés des stratégies de navigation permettant d’affiner les changements de focus

Utilisez les propriétés de stratégie de navigation XYFocus pour spécifier le contrôle qui doit recevoir le focus en fonction de la touche flèche enfoncée. Ces propriétés sont:

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

Ces propriétés ont une valeur de type **XYFocusNavigationStrategy**. Si la valeur est **automatique**, le comportement de l’élément est basé sur ses ancêtres. Si tous les éléments sont réglés sur **Automatique**, **Projection** est utilisé.

Ces stratégies de navigation sont applicables au clavier, au boîtier de commande et à la télécommande.

### <a name="projection"></a>Projection

La stratégie de projection indique que le focus se déplace vers le premier élément rencontré lors de la projection du bord de l’élément ayant actuellement le focus dans la direction de navigation.

> [!NOTE]
> Il existe d’autres facteurs qui influencent le résultat, tels que l’élément ciblé précédemment et la proximité de l’axe du sens de la navigation.

![projection](images/keyboard/projection.png)

*Le focus se déplace de A vers D sur la navigation vers le bas basée sur la projection du bord inférieur de A*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

La stratégie NavigationDirectionDistance déplace le focus vers l’élément le plus proche de l’axe principal du sens de la navigation.

Le bord du rectangle englobant correspondant à la direction de navigation est étendu et projeté pour identifier les cibles candidates. Le premier élément rencontré est identifié comme étant la cible. Dans le cas de plusieurs candidats, l’élément le plus proche est identifié comme étant la cible. S’il existe encore plusieurs candidats, l’élément plus haut/le plus à gauche est identifié comme candidat.

![distance de sens de navigation](images/keyboard/navigation-direction-distance.png)

*Le focus se déplace de A vers C, puis de C vers B sur la navigation vers le bas*


### <a name="rectilineardistance"></a>RectilinearDistance

La stratégie RectilinearDistance déplace le focus vers l’élément le plus proche en fonction de la distance2D la plus courte (métrique Manhattan). Cette distance est calculée par ajout de la distance principale et de la distance secondaire de chaque candidat potentiel. En cas d’égalité, le premier élément vers la gauche est sélectionné si la direction va vers le haut ou le bas, ou le premier élément vers le haut est sélectionné si la direction va vers la gauche ou la droite.

Dans l’image suivante, nous montrons que lorsque le focus est sur A, il se déplace vers B, car:

-   Distance (A, B, bas) = 10 + 0 = 10
-   Distance (A, C, bas) = 0 + 30 = 30
-   Distance (A, D, bas) 30 + 0 = 30

![Distance rectiligne](images/keyboard/rectilinear-distance.png)

*Lorsque le focus est sur A, il se déplace vers B lors de l’utilisation de la stratégie RectilinearDistance*

L’image suivante montre comment la stratégie de navigation est appliquée à l’élément proprement dit, mais pas aux éléments enfants (contrairement à XYFocusKeyboardNavigation).

Par exemple, lorsque le focus est sur E, un appui vers la droite le déplace vers F grâce à la stratégie RectilinearDistance, mais lorsqu’il est sur D, un appui à droite le déplace vers H en cas d’utilisation de la stratégie de Projection.

Notez que la stratégie du conteneur principal (Navigation Direction Distance) n’est pas appliquée. Elle n’est utilisée que lorsque le focus est sur H, F ou G.

![stratégies de navigation différentes](images/keyboard/different-navigation-strategies.png)

*Différents comportements du focus en fonction de la stratégie de navigation appliquée*

L’exemple qui suit simule le comportement du panneau de vignettes à partir du menu Démarrer de Windows10. Dans ce cas, l’appui sur les flèches haut et bas fait appel à la stratégie NavigationDirectionDistance et l’appui sur les flèches gauche et droite utilise la stratégie de Projection.

```XAML
<start:TileGridView
        XYFocusKeyboardNavigation ="Default"
        XYFocusUpNavigationStrategy=" NavigationDirectionDistance "
        XYFocusDownNavigationStrategy=" NavigationDirectionDistance "
        XYFocusLeftNavigationStrategy="Projection"
        XYFocusRightNavigationStrategy="Projection"
/>
```

## <a name="move-focus-programmatically"></a>Déplacement programmé du focus

Utilisez l’une des méthodes [FocusManager.TryMoveFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.TryMoveFocus) ou [FocusManager.FindNextElement](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.FindNextFocusableElement) pour programmer le déplacement du focus.

TryMoveFocus définit le focus, comme si une touche de navigation avait été actionnée.
FindNextElement retourne l’élément (de type DependencyObject) vers lequel TryMoveFocus se déplacerait.

**REMARQUE** Nous vous recommandons d’utiliser la méthode **FindNextElement** au lieu de **FindNextFocusableElement**. FindNextFocusableElement tente de retourner un élément UIElement, toutefois, si le prochain élément susceptible de recevoir le focus est un lien hypertexte, il retourne la valeur null, car un lien hypertexte n’est pas un élément UIElement. En lieu et place, la méthode FindNextElement retourne un DependencyObject.

### <a name="find-a-focus-candidate-in-a-scope"></a>Détecter un candidat au focus dans une étendue

FocusManager.FindNextElement et FocusManager.TryMoveFocus vous permettent tous deux de personnaliser le comportement de recherche du prochain élément éligible. Vous pouvez effectuer une recherche dans une arborescence spécifique de l’interface utilisateur ou exclure les éléments d’une zone de navigation spécifique.

Cet exemple est tiré d’une implémentation d’un jeu TicTacToe et montre comment utiliser les méthodes FindNextElement et TryMoveFocus:

```XAML
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
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="0" x:Name="Cell00" />
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="0" x:Name="Cell10"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="0" x:Name="Cell20"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="1" x:Name="Cell01"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="1" x:Name="Cell11"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="1" x:Name="Cell21"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="2" x:Name="Cell02"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="2" x:Name="Cell22"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="2" x:Name="Cell32"/>
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
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Up, options);
                    break;
                case Windows.System.VirtualKey.Down:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Down, options);
                    break;
                case Windows.System.VirtualKey.Left:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Left, options);
                    break;
                case Windows.System.VirtualKey.Right:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Right, options);
                    break;
            }
         //Note you should also consider whether is a Hyperlink, WebView, or TextBlock.
            if (candidate != null && candidate is Control)
            {
                (candidate as Control).Focus(FocusState.Keyboard);
            }
        }
```

**REMARQUE**: FocusNavigationDirection.Left et FocusNavigationDirection.Right sont logiques (proche et éloigné) dans le cadre de la prise en charge des scénarios de la droite vers la gauche. (Ceci correspond au reste du code Xaml.)

La recherche de candidats au focus peut être réglée à l’aide de la classe FindNextElementOptions. Les propriétés de cet objet sont les suivantes:

-   **SearchRoot** DependencyObject: pour étendre la recherche de candidats aux enfants de cet objet DependencyObject. La valeur null indique toute l’arborescence visuelle de l’application.
-   **ExclusionRect** Rect: pour exclure de la recherche les éléments qui, lorsqu’ils sont restitués, se chevauchent d’au moins un pixel à partir de ce rectangle.
-   **HintRect** Rect: les candidats potentiels sont calculés en prenant l’élément sélectionné comme référence. Ce rectangle permet aux développeurs de spécifier une référence autre que l’élément ciblé. Le rectangle est purement «fictif» et ne sert que pour les calculs. Il n’est jamais converti en véritable rectangle ni ajouté à l’arborescence visuelle.
-   **XYFocusNavigationStrategyOverride** XYFocusNavigationStrategyOverride: stratégie de navigation utilisée pour déplacer le focus. Le type est une énumération qui peut avoir les valeurs None (qui correspond à zéro), Auto, RectilinearDistance, NavigationDirectionDistance et Projection.
    **Important**: **SearchRoot** n’est pas utilisé pour calculer la zone rendue (géométrique) ni pour détecter les candidats à l’intérieur de la zone. En conséquence, si les transformations sont appliquées aux descendants du DependencyObject qui les placent en dehors de la zone directionnelle, ces éléments sont toujours considérés comme candidats.

La surcharge d’options de FindNextElement ne peut pas être utilisée pour la navigation par onglets, elle ne prend en charge que la navigation directionnelle.

L’image suivante illustre ces options.

Par exemple, lorsque le focus est sur B, l’appel FindNextElement (avec les différentes options indiquant que le focus se déplace vers la droite) place le focus sur I. Les raisons sont:

1.  La référence n’est pas B, mais A en raison de HintRect
2.  C est exclu, car le candidat potentiel doit se trouver sur MyPanel (la SearchRoot)
3.  F est exclu car il chevauche le rectangle d’exclusions

![astuces de navigation](images/keyboard/navigation-hints.png)

*Comportement du focus en fonction des indications de navigation*

### <a name="no-focus-candidate-available"></a>Aucun candidat au focus disponible

L’événement UIElement.NoFocusCandidateFound est déclenché lorsque l’utilisateur tente de déplacer le focus avec les onglets ou les touches flèches alors qu’il n’existe aucun candidat possible dans la direction spécifiée. Cet événement n’est pas déclenché pour FocusManager.TryMoveFocus().

Dans la mesure où il s’agit d’un événement routé, il se propage à partir de l’élément sur lequel se trouve le focus par le biais d’objets parents successifs vers la racine de l’arborescence des objets. Ainsi, vous pouvez gérer l’événement, le cas échéant.

<a name="split-view-code-sample"></a>L’exemple qui suit montre une grille qui s’ouvre en mode fractionné lorsque l’utilisateur tente de déplacer le focus vers le contrôle le mieux placé à gauche, comme décrit dans [Conception pour une documentation TV](designing-for-tv.md#navigation-pane).

```XAML
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">  ...</Grid>
```

```csharp
private void OnNoFocusCandidateFound (UIElement sender, NoFocusCandidateFoundEventArgs args)
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

## <a name="focus-events"></a>Événements de focus

Les événements [UIElement.GotFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.GotFocus) et UIElement.LostFocus sont déclenchés lorsqu’un contrôle reçoit ou perd le focus, respectivement. Cet événement n’est pas déclenché pour FocusManager.TryMoveFocus().

Dans la mesure où il s’agit d’un événement routé, il se propage à partir de l’élément sur lequel se trouve le focus par le biais d’objets parents successifs vers la racine de l’arborescence des objets. Ainsi, vous pouvez gérer l’événement, le cas échéant.

Les événements **UIElement.GettingFocus** et **UIElement.LosingFocus** se propagent également à partir du contrôle, mais *avant* que le changement de focus se produise. Ainsi, l’application dispose d’une opportunité de rediriger ou annuler le changement de focus.

Notez que les événements GettingFocus et LosingFocus sont synchrones, tandis que les événements GotFocus et LostFocus sont asynchrones. Par exemple, si le focus se déplace lorsque l’application appelle la méthode Control.Focus(), GettingFocus est déclenché lors de l’appel, tandis que GotFocus est déclenché après l’appel.

Les éléments UIElements peuvent s’accrocher aux événements GettingFocus et LosingFocus et modifier le focus (à l’aide de la propriété NewFocusedElement) avant qu’il se déplace. Même si le focus a changé, l’événement se propage toujours et un autre parent peut le modifier une nouvelle fois.

Pour éviter les problèmes de réentrance, des exceptions sont générées si vous tentez de déplacer le focus (à l’aide de FocusManager.TryMoveFocus ou de Control.Focus) alors que ces événements sont en cours de propagation.

Ces événements sont déclenchés quelle que soit la raison du déplacement du focus (par exemple, navigation par onglets, de type XYFocus ou programmée).

L’image suivante montre comment, lors du déplacement de A vers la droite, XYFocus choisit LVI4 comme candidat. Puis, LVI4 déclenche l’événement GettingFocus dans le cadre duquel ListView dispose de la possibilité de replacer le focus sur LVI3.

![événements de focus](images/keyboard/focus-events.png)

*Événements de focus*

Cet exemple de code montre comment gérer l’événement OnGettingFocus et rediriger le focus en fonction de l’emplacement où il était précédemment défini.

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
            if (MyListView.SelectedIndex != -1 && IsNotAChildOf(MyListView, args.OldFocusedElement))
            {
                var selectedContainer = MyListView.ContainerFromItem(MyListView.SelectedItem);
                if (args.FocusState == FocusState.Keyboard && args.NewFocusedElement != selectedContainer)
                {
                    args.NewFocusedElement = MyListView.ContainerFromItem(MyListView.SelectedItem);
                    args.Handled = true;
                }
            }        
  }
```

Cet exemple de code montre comment gérer l’événement OnLosingFocus d’un objet CommandBar et attendre que le menu déroulant soit fermé pour placer le focus.

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
          if (MyCommandBar.IsOpen == true && IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
          {
              args.Cancel = true;
              args.Handled = true;
          }
}
```

### <a name="losingfocus-and-gettingfocus-event-order"></a>Ordre des événements LosingFocus et GettingFocus

LosingFocus et GettingFocus sont des événements synchrones, c’est pourquoi le focus ne doit pas être déplacé pendant que ces événements sont cours de propagation. Cependant, comme LostFocus et GotFocus sont des événements asynchrones, rien ne garantit que le focus ne se déplacera plus avant l’exécution du gestionnaire. La seule garantie est que le Gestionnaire de LostFocus s’exécutera avant celui de GotFocus.

Voici l’ordre d’exécution:

1.  Synchronisation LosingFocus: si LosingFocus réinitialise le focus sur l’élément qui le perdait ou si le paramètre Cancel prend la valeur «vrai» (Cancel=true), aucun autre événement ne suit
2.  Synchronisation GettingFocus: si GettingFocus réinitialise le focus sur l’élément qui le perdait ou si le paramètre Cancel prend la valeur «vrai» (Cancel=true), aucun autre événement ne suit
3.  LostFocus asynchrone
4.  GotFocus asynchrone

## <a name="find-the-first-and-last-focusable-element-a-namefindfirstfocusableelement"></a>Recherchez les premier et dernier éléments <a name="findfirstfocusableelement"> susceptibles d’être activés

Les méthodes **FocusManager.FindFirstFocusableElement** et **FocusManager.FindLastFocusableElement** vous permettent de déplacer le focus vers le premier ou le dernier élément susceptible de le recevoir dans l’étendue d’un objet (l’arborescence d’un élément UIElement ou l’arborescence texte d’un objet TextElement). L’étendue peut être spécifiée lors de l’appel de ces méthodes. Si l’argument est null, l’étendue sera la fenêtre active.

**Remarque**: ces méthodes peuvent retourner la valeur null si aucun élément de l’étendue ne peut recevoir le focus.

<a name="popup-ui-code-sample"></a>L’exemple de code ci-dessous montre comment spécifier que les boutons d’un CommandBar présentent un comportement directionnel enveloppant décrit dans l’article [Interactions avec le clavier](keyboard-interactions.md#popup-ui).

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
