---
Description: Vous pouvez répondre aux actions d’un clavier matériel ou logiciel dans vos applications à l’aide de gestionnaires d’événements de classe et de clavier.
title: Événements de clavier
ms.assetid: ac500772-d6ed-4a3a-825b-210a9c3c8f59
label: Keyboard events
template: detail.hbs
keywords: clavier, boîtier de commande, accès à distance, accessibilité, navigation, focus, texte, saisie, interactions utilisateur, boîtier de commande, touche haut, touche bas
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 0aac8f82f4dd6a40f5672af8c000905cfa9c747e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365638"
---
# <a name="keyboard-events"></a>Événements de clavier

## <a name="keyboard-events-and-focus"></a>Événements de clavier et focus

Les événements de clavier suivants peuvent se produire pour les claviers physiques et tactiles.

| Événement                                      | Description                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) | Se produit lorsqu’une touche est enfoncée.  |
| [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)     | Se produit lorsqu’une touche est relâchée. |

> [!IMPORTANT]
> Certains contrôles Windows Runtime gèrent les événements d’entrée en interne. Dans ces cas-là, il peut sembler qu’un événement d’entrée n’a pas lieu car votre écouteur d’événements n’appelle pas le gestionnaire associé. En général, ces touches sont gérées par le gestionnaire de classe pour fournir une prise en charge intégrée de l’accessibilité de base du clavier. Par exemple, la classe [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) substitue les événements [**OnKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown) pour la touche Espace et la touche Entrée (de même que [**OnPointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onpointerpressed)) et les achemine vers l’événement [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) du contrôle. Quand un appui sur une touche est géré par la classe de contrôle, les événements [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) et [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) ne sont pas déclenchés.  
> Cela fournit un équivalent de clavier intégré pour l’appel du bouton, comme en cas d’appui avec un doigt ou de clic avec une souris. Les touches autres que Espace ou Entrée déclenchent quand même les événements [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) et [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup). Pour plus d’informations sur le fonctionnement de cette gestion des événements basée sur la classe (en particulier la section Gestionnaires d’événements d’entrée dans les contrôles), voir [Vue d’ensemble des événements et des événements routés](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).


Les contrôles de votre interface utilisateur génèrent des événements de clavier uniquement lorsqu’ils ont le focus d’entrée. Un élément individuel reçoit le focus lorsque l’utilisateur clique ou appuie directement sur cet élément dans l’interface ou qu’il utilise la touche Tab dans la zone de contenu.

Vous pouvez également appeler la méthode [**Focus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.focus) d’un contrôle pour forcer le focus. Cette action est nécessaire lorsque vous implémentez des touches de raccourci, car le focus du clavier n’est pas défini par défaut lors du chargement de votre interface utilisateur. Pour plus d’informations, voir **Exemple de touches de raccourci**, plus loin dans cette rubrique.

Pour qu’un contrôle puisse recevoir le focus d’entrée, il doit être activé, visible et avoir les propriétés [**IsTabStop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istabstop) et [**HitTestVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.ishittestvisible) associées à la valeur **true**. Il s’agit de l’état par défaut de la plupart des contrôles. Lorsqu’un contrôle a le focus d’entrée, il peut être déclenché et répondre aux événements d’entrée de clavier, tel que décrit plus loin dans cette rubrique. Vous pouvez également répondre à un contrôle qui reçoit ou perd le focus en gérant les événements [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) et [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus).

Par défaut, l’ordre de tabulation des contrôles est celui dans lequel les contrôles apparaissent dans le code XAML (Extensible Application Markup Language). Vous pouvez cependant changer cet ordre à l’aide de la propriété [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex). Pour plus d’informations, voir [Implémentation de l’accessibilité du clavier](https://docs.microsoft.com/previous-versions/windows/apps/hh868161(v=win.10)).

## <a name="keyboard-event-handlers"></a>Gestionnaires d’événements de clavier


Un gestionnaire d’événements d’entrée implémente un délégué qui fournit les informations suivantes :

-   L’expéditeur de l’événement. L’expéditeur signale l’objet auquel le gestionnaire d’événements est attaché.
-   Les données d’événement. Dans le cas des événements de clavier, ces données seront une instance de [**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs). Le délégué des gestionnaires est [**KeyEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyeventhandler). Les propriétés les plus significatives de **KeyRoutedEventArgs** pour la plupart des scénarios de gestionnaires sont [**Key**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key) et éventuellement [**KeyStatus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.keystatus).
-   [**OriginalSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource). Les événements de clavier étant des événements routés, les données d’événements fournissent **OriginalSource**. Si vous autorisez délibérément des événements à être proposés par le biais d’un arbre d’objets, **OriginalSource** est parfois l’objet de la question plutôt que l’expéditeur, bien que cela dépende de la conception de votre application. Pour plus d’informations concernant l’utilisation de **OriginalSource** à la place de l’expéditeur, voir la section Événements routés de clavier de cette rubrique ou [Vue d’ensemble des événements et des événements routés](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

### <a name="attaching-a-keyboard-event-handler"></a>Attachement d’un gestionnaire d’événements de clavier

Vous pouvez attacher les fonctions des gestionnaires d’événements de clavier pour n’importe quel objet qui inclut l’événement en tant que membre. Cela inclut toute classe dérivée [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement). L’exemple de code XAML suivant montre comment attacher des gestionnaires pour l’événement [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) d’un [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid).

```xaml
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

Vous pouvez également associer un gestionnaire d’événements à du code. Pour plus d’informations, voir [Vue d’ensemble des événements et des événements routés](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

### <a name="defining-a-keyboard-event-handler"></a>Définition d’un gestionnaire d’événements de clavier

L’exemple suivant montre la définition incomplète du gestionnaire d’événements [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) attaché dans l’exemple précédent.

```csharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```vb
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    ' handling code here
End Sub
```

```c++
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
  {
      //handling code here
  }
```

### <a name="using-keyroutedeventargs"></a>Utilisation de KeyRoutedEventArgs

Tous les événements de clavier utilisent [**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) pour les données d’événements, et **KeyRoutedEventArgs** contient les propriétés suivantes :

-   [**Clé**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key)
-   [**KeyStatus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.keystatus)
-   [**Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled)
-   [**OriginalSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource) (héritée de [ **RoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEventArgs))

### <a name="key"></a>Touche

L’événement [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) est déclenché si une touche est enfoncée. De même, l’événement [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) est déclenché si une touche est relâchée. Vous êtes généralement à l’écoute des événements en vue de traiter une valeur de touche spécifique. Afin de déterminer quelle touche est enfoncée ou relâchée, vérifiez la valeur [**Key**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key) dans les données d’événements. **Key** renvoie une valeur [**VirtualKey**](https://docs.microsoft.com/uwp/api/Windows.System.VirtualKey). L’énumération **VirtualKey** inclut toutes les touches prises en charge.

### <a name="modifier-keys"></a>Touches de modification

Les touches de modification sont des touches, telles que Ctrl ou Maj, sur lesquelles les utilisateurs appuient généralement en même temps que d’autres touches. Votre application peut utiliser ces combinaisons de touches en tant que raccourcis clavier pour appeler des commandes d’application.

Vous pouvez détecter des combinaisons de touches de raccourci à l’aide de code dans vos gestionnaires d’événements [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) et [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup). Vous pouvez ensuite effectuer le suivi de l’état d’appui des touches de modification qui vous intéressent. Lorsqu’un événement de clavier se produit pour une touche qui n’est pas une touche de modification, vous pouvez vérifier en même temps si une touche de modification est à l’état appuyé.

> [!NOTE]
> La touche Alt est représentée par la valeur **VirtualKey.Menu**.

 

### <a name="shortcut-keys-example"></a>Exemple de touches de raccourci


L’exemple suivant explique comment implémenter des touches de raccourci. Dans cet exemple, les utilisateurs peuvent contrôler la lecture du contenu multimédia à l’aide des boutons Lecture, Pause et Arrêt ou des raccourcis clavier Ctrl+P, Ctrl+A et Ctrl+S. Le bouton XAML affiche les touches de raccourci à l’aide des info-bulles et des propriétés [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) des étiquettes de boutons. Cette documentation d’auto-apprentissage est importante afin d’augmenter la facilité d’utilisation et l’accessibilité de votre application. Pour plus d’informations, voir [Accessibilité du clavier](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility).

Notez également que la page définit le focus d’entrée sur elle-même lors de son chargement. Sans cette étape, aucun contrôle n’a le focus d’entrée initial et l’application ne déclenche pas les événements d’entrée tant qu’un utilisateur n’a pas défini le focus d’entrée manuellement (par exemple en utilisant la touche de tabulation ou en cliquant sur un contrôle).

```xaml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>

  </StackPanel>

</Grid>
```

```c++
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) {
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


bool KeyboardSupport::MainPage::IsCtrlKeyPressed()
{
    auto ctrlState = CoreWindow::GetForCurrentThread()->GetKeyState(VirtualKey::Control);
    return (ctrlState & CoreVirtualKeyStates::Down) == CoreVirtualKeyStates::Down;
}

void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (IsCtrlKeyPressed()) {
        if (e->Key==VirtualKey::P) { DemoMovie->Play(); }
        if (e->Key==VirtualKey::A) { DemoMovie->Pause(); }
        if (e->Key==VirtualKey::S) { DemoMovie->Stop(); }
    }
}
```

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}

private static bool IsCtrlKeyPressed()
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (IsCtrlKeyPressed())
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Function IsCtrlKeyPressed As Boolean
    Dim ctrlState As CoreVirtualKeyStates = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    Return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
End Function

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If IsCtrlKeyPressed() Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

> [!NOTE]
> La définition [**d’AutomationProperties.AcceleratorKey**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.acceleratorkey?view=netframework-4.8) ou [**d’AutomationProperties.AccessKey**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.accesskey?view=netframework-4.8) en XAML fournit des informations de chaîne qui documentent la touche de raccourci d’appel de cette action particulière. Les informations sont capturées par des clients Microsoft UI Automation, tels que le Narrateur, et généralement fournis directement à l’utilisateur.
>
> La définition d’**AutomationProperties.AcceleratorKey** ou d’**AutomationProperties.AccessKey** ne constitue pas une action proprement dite. Vous devrez quand même associer des gestionnaires pour les événements [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) ou [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) afin de mettre réellement en œuvre le comportement de la touche de raccourci dans l’application. De même, l’ornement de soulignement du texte n’est pas fourni automatiquement pour une touche d’accès. Si vous voulez afficher du texte souligné dans l’interface utilisateur, vous devez souligner le texte de manière explicite pour la touche en question dans votre mnémonique en insérant une mise en forme [**Underline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Underline).

 

## <a name="keyboard-routed-events"></a>Événements routés de clavier


Certains événements sont des événements routés, comme [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) et [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup). Les événements routés utilisent la stratégie de routage de propagation. La stratégie de routage de propagation signifie qu’un événement provenant d’un objet enfant est ensuite routé jusqu’aux objets parents successifs dans l’arbre d’objets, ce qui offre ainsi une autre opportunité de gérer le même événement et d’interagir avec les mêmes données d’événements.

Considérez l’exemple de code XAML suivant, dans lequel des événements [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) sont définis pour un objet [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) et deux objets [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button). Dans ce cas, si vous relâchez une touche tandis que le focus est détenu par l’un des objets **Button**, l’événement **KeyUp** est déclenché. L’événement est ensuite propagé à l’objet **Canvas** parent.

```xaml
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

L’exemple suivant montre comment implémenter le gestionnaire d’événements [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) pour le contenu XAML correspondant dans l’exemple précédent.

```csharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

Notez l’utilisation de la propriété [**OriginalSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource) dans le gestionnaire précédent. Dans ce cas, **OriginalSource** signale l’objet qui a déclenché l’événement. L’objet ne pouvait pas être l’objet [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel), car **StackPanel** n’est pas un contrôle et il ne peut pas avoir le focus. Seul un des deux boutons au sein de l’objet **StackPanel** a pu déclencher l’événement, mais lequel ? Vous utilisez **OriginalSource** pour identifier l’objet source de l’événement réel si vous gérez l’événement sur un objet parent.

### <a name="the-handled-property-in-event-data"></a>Propriété Handled dans des données d’événements

En fonction de votre stratégie de gestion des événements, il se peut que vous vouliez qu’un seul gestionnaire d’événements réagisse à un événement de propagation. Ainsi, tout gestionnaire [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) spécifique attaché à l’un des contrôles [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) a en premier lieu la possibilité de gérer cet événement. Dans ce cas, il se peut que vous ne vouliez pas que le panneau parent gère également l’événement. Pour ce scénario, utilisez la propriété [**Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) dans les données d’événements.

Le rôle de la propriété [**Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) dans une classe de données d’événements routés est de signaler qu’un autre gestionnaire que vous aviez enregistré précédemment sur l’itinéraire des événements a déjà agi. Cela influence le comportement du système d’événements routés. Lorsque vous définissez la valeur de la propriété **Handled** sur **true** dans un gestionnaire d’événements, cet événement arrête le routage et n’est pas envoyé aux éléments parents successifs.

### <a name="addhandler-and-already-handled-keyboard-events"></a>AddHandler et événements de clavier déjà gérés

Vous pouvez utiliser une technique spéciale pour associer des gestionnaires pouvant agir sur des événements déjà marqués comme étant gérés. Cette technique utilise le [ **AddHandler** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) méthode pour inscrire un gestionnaire, plutôt que les attributs XAML ou syntaxe spécifique au langage pour l’ajout de gestionnaires, tels que += dans C\#.

L’une des limitations générales de cette technique est le fait que l’API **AddHandler** utilise un paramètre de type [**RoutedEvent**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEvent) qui identifie l’événement routé en question. Tous les événements routés ne fournissent pas un identificateur **RoutedEvent** et cette considération affecte par conséquent les événements routés qui peuvent encore être gérés dans le cas [**Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled). Les événements [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) et [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) possèdent des identificateurs d’événements routés ([**KeyDownEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydownevent) et [**KeyUpEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyupevent)) sur [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement). Toutefois, les autres événements tels que [**TextBox.TextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) ne possèdent pas d’identificateurs d’événements routés. Par conséquent, ils ne peuvent pas être utilisés pour la technique **AddHandler**.

### <a name="overriding-keyboard-events-and-behavior"></a>Remplacement des événements et du comportement du clavier

Vous pouvez remplacer les événements de touche pour certains contrôles spécifiques (tels que [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)) pour garantir une navigation cohérente pour différents périphériques de saisie, notamment le clavier et le boîtier de commande.

Dans l’exemple suivant, nous sous-classe le contrôle et que vous remplacez le comportement de l’événement KeyDown pour déplacer le focus sur le contrôle GridView de contenu lorsqu’une touche de direction est enfoncée.

```csharp
public class CustomGridView : GridView
  {
    protected override void OnKeyDown(KeyRoutedEventArgs e)
    {
      // Override arrow key behaviors.
      if (e.Key != Windows.System.VirtualKey.Left && e.Key !=
        Windows.System.VirtualKey.Right && e.Key !=
          Windows.System.VirtualKey.Down && e.Key !=
            Windows.System.VirtualKey.Up)
              base.OnKeyDown(e);
      else
        FocusManager.TryMoveFocus(FocusNavigationDirection.Down);
    }
  }
```

> [!NOTE]
> Si vous utilisez un contrôle GridView uniquement pour la mise en page, pensez à utiliser d’autres contrôles tels que [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) avec [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid).

## <a name="commanding"></a>Commandes

Un petit nombre d’éléments d’interface utilisateur fournit la prise en charge intégrée pour les commandes. Les commandes utilisent les événements routés associés à une entrée dans leur implémentation sous-jacente et permettent de traiter l’entrée d’interface utilisateur associée (une certaine action du pointeur, une touche d’accès rapide spécifique), en invoquant un seul gestionnaire de commandes.

Si les commandes sont disponibles pour un élément d’interface utilisateur, envisagez d’utiliser ses API de commandes plutôt que les événements d’entrée discrets. Pour plus d’informations, voir [**ButtonBase.Command**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).

Vous pouvez également implémenter [**ICommand**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand) afin d’encapsuler les fonctionnalités de commande que vous appelez à partir de gestionnaires d’événements ordinaires, ce qui vous permet d’utiliser les commandes même lorsqu’aucune propriété **Command** n’est disponible.

## <a name="text-input-and-controls"></a>Entrée de texte et contrôles

Certains contrôles réagissent aux événements de clavier avec leur propre gestion. Par exemple, [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) est un contrôle conçu pour capturer puis représenter visuellement du texte entré à l’aide du clavier. Il utilise [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) et [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) dans sa propre logique en vue de capturer les frappes de touches, puis il déclenche également son propre événement [**TextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) si le texte a changé.

D’une manière générale, vous pouvez toujours ajouter des gestionnaires pour [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) et [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) à un élément [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox), ou tout contrôle associé destiné à traiter l’entrée de texte. Toutefois, comme prévu de par sa conception, il se peut qu’un contrôle ne réponde pas à l’ensemble des valeurs de touches qui lui sont envoyées par le biais des événements de touches. Le comportement est spécifique à chaque contrôle.

En guise d’exemple, [**ButtonBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) (la classe de base pour [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)) traite l’élément [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) de sorte qu’il puisse surveiller la barre d’espace ou la touche Entrée. **ButtonBase** considère que **KeyUp** équivaut à un clic de bouton gauche de souris pour les besoins du déclenchement d’événement [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Ce traitement de l’événement s’effectue lorsque **ButtonBase** remplace la méthode virtuelle [**OnKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeyup). Dans son implémentation, la valeur **true** est affectée à [**Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled). Le résultat est que tout parent d’un bouton à l’écoute d’un événement de touche, dans le cas de la barre d’espace, ne recevra pas l’événement déjà géré pour ses propres gestionnaires.

Un autre exemple est [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Certaines touches, telles que les touches de direction, ne sont pas considérées comme étant du texte par **TextBox** mais sont au contraire considérées comme étant spécifiques au comportement d’interface utilisateur du contrôle. L’élément **TextBox** marque ces cas d’événements comme gérés.

Les contrôles personnalisés peuvent implémenter leur propre comportement similaire de remplacement pour les événements de touches en remplaçant [**OnKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown) / [**OnKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeyup). Si votre contrôle personnalisé traite des touches accélérateur spécifiques ou présente un comportement de contrôle ou de focus qui est similaire au scénario décrit pour [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox), vous devez placer cette logique dans vos propres remplacements **OnKeyDown** / **OnKeyUp**.

## <a name="the-touch-keyboard"></a>Le clavier tactile

Les contrôles de saisie de texte fournissent le support automatique du clavier tactile. Lorsque l’utilisateur définit le focus d’entrée sur un contrôle de texte à l’aide de l’entrée tactile, le clavier tactile apparaît automatiquement. Lorsque le focus d’entrée n’est pas placé sur un contrôle de texte, le clavier tactile est masqué.

Lorsque le clavier tactile apparaît, votre interface utilisateur est automatiquement repositionnée afin que l’élément ciblé reste visible. Des zones importantes de l’interface utilisateur peuvent alors sortir de l’écran. Vous pouvez désactiver ce comportement par défaut et effectuer vos propres ajustements de l’interface utilisateur lorsque le clavier tactile apparaît. Pour plus d’informations, voir [Exemple de clavier tactile](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard).

Si vous créez un contrôle personnalisé qui nécessite une saisie de texte mais ne découle pas d’un contrôle de saisie de texte standard, vous pouvez ajouter la prise en charge du clavier tactile en implémentant les modèles de contrôle UI Automation appropriés. Pour plus d’informations, voir [Exemple de clavier tactile](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard).

Les touches enfoncées sur le clavier tactile déclenchent les événements [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) et [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup), exactement comme si vous aviez appuyé sur les touches d’un clavier physique. Cependant, le clavier tactile ne déclenche pas d’événements de saisie pour les touches Ctrl+A, Ctrl+Z, Ctrl+X, Ctrl+C et Ctrl+V, qui sont réservées à la manipulation de texte dans le contrôle de saisie.

Vous pouvez nettement faciliter et accélérer la saisie de données par les utilisateurs dans votre application, en définissant l’étendue des entrées du contrôle de texte afin qu’elle corresponde au type de données attendu de la part de l’utilisateur. L’étendue des entrées fournit une indication sur le type d’entrée de texte attendu par le contrôle, afin que le système puisse fournir une disposition de clavier tactile spécialisée pour le type d’entrée. Par exemple, si une zone de texte est utilisée uniquement pour saisir un code confidentiel à 4 chiffres, définissez la propriété [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) sur [**Number**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue). Cela indique au système qu’il doit afficher la disposition du pavé numérique, ce qui facilite l’entrée d’un PIN par l’utilisateur. Pour plus d’informations, voir [Comment utiliser l’étendue des entrées pour modifier le clavier tactile](https://docs.microsoft.com/windows/uwp/design/input/use-input-scope-to-change-the-touch-keyboard).

## <a name="related-articles"></a>Articles connexes

**Développeurs**
* [Interactions de clavier](keyboard-interactions.md)
* [Identification des périphériques d’entrée](identify-input-devices.md)
* [Répondre à la présence du clavier tactile](respond-to-the-presence-of-the-touch-keyboard.md)

**Concepteurs**
* [Instructions de conception de clavier](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)

**Exemples**
* [Exemple de clavier tactile](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [Exemple d’entrée de base](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple d’éléments visuels de focus](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemples d’archive**
* [Exemple d’entrée](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrée : Exemples de fonctionnalités d’appareil](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrée : Exemple de clavier tactile](https://go.microsoft.com/fwlink/p/?linkid=246019)
* [Répondre à l’apparence de l’exemple le clavier visuel](https://go.microsoft.com/fwlink/p/?linkid=231633)
* [Exemple de modification de texte XAML](https://go.microsoft.com/fwlink/p/?LinkID=251417)
 

 
