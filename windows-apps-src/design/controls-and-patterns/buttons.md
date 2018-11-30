---
Description: A button gives the user a way to trigger an immediate action.
title: Boutons
label: Buttons
template: detail.hbs
ms.date: 10/2/2018
ms.topic: article
keywords: windows10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5b755533cd4f0720477988781e6cc955b532f1a9
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8214555"
---
# <a name="buttons"></a>Boutons

Un bouton permet à l’utilisateur de déclencher une action immédiate. Certains boutons spécialisées pour les tâches particulières, par exemple, navigation, les actions répétées ou présentant des menus.

![Exemple de boutons](images/controls/button.png)

L’infrastructure XAML fournit un contrôle button standard ainsi que plusieurs contrôles de bouton spécialisés.

Contrôle | Description
------- | -----------
[Bouton](/uwp/api/windows.ui.xaml.controls.button) | Lance une action immédiate. Peut être utilisé avec un événement Click ou de la liaison de commande.
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | Un bouton qui déclenche un événement Click en continu tant que l’état appuyé.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | Un bouton qui a stylisé comme un lien hypertexte, utilisé pour la navigation. Voir [Liens hypertexte](hyperlinks.md) pour plus d’informations.
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | Un bouton avec un chevron pour ouvrir un menu volant joint.
[Bouton partagé](/uwp/api/windows.ui.xaml.controls.splitbutton) | Un bouton avec deux des côtés. Un côté génère une action, et l’autre côté ouvre un menu.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | Un bouton bascule avec deux côtés. Active ou désactive un côté marche/arrêt, et l’autre côté ouvre un menu.

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| DropDownButton, bouton partagé et ToggleSplitButton sont inclus dans le cadre de la bibliothèque de l’interface utilisateur de Windows, un package NuGet qui contient les nouveaux contrôles et fonctionnalités de l’interface utilisateur pour les applications UWP. Pour plus d’informations, y compris les instructions d’installation, consultez la [vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de la plateforme** | **API de bibliothèque de l’interface utilisateur Windows** |
| - | - |
| [Événement click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), [propriété de commande](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [Classe DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton), [classe de bouton partagé](/uwp/api/microsoft.ui.xaml.controls.splitbutton), [classe ToggleSplitButton](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez un **bouton** pour permettre à l’utilisateur de lancer une action immédiate, telle que l’envoi d’un formulaire.

N’utilisez pas un bouton quand l’action consiste à naviguer vers une autre page; Utilisez plutôt un [contrôle HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) . Voir [Liens hypertexte](hyperlinks.md) pour plus d’informations.
> Exception: pour la navigation dans un assistant, utilisez les boutons Précédent et Suivant. Pour les autres types de vers l’arrière navigation ou à un niveau supérieur, utilisez un [bouton précédent](../basics/navigation-history-and-backwards-navigation.md).

Utilisez un **RepeatButton** lorsque l’utilisateur peut souhaiter déclencher une action à plusieurs reprises. Par exemple, utilisez un RepeatButton pour incrémenter ou décrémenter une valeur dans un compteur.

Utilisez un **DropDownButton** lorsque le bouton a un menu volant qui contient plus d’options. Le chevron par défaut fournit une indication visuelle que le bouton inclut un menu volant.

Utilisez un **bouton partagé** lorsque vous souhaitez que l’utilisateur puisse lancer une action immédiate ou choisir parmi des options supplémentaires de manière indépendante.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Button">ouvrir l’application et voir le bouton en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Cet exemple utilise deux boutons, Autoriser et Bloquer, dans une boîte de dialogue demandant l’accès à la géolocalisation.

![Exemple de boutons utilisés dans une boîte de dialogue](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>Créer un bouton

Cet exemple illustre un bouton qui répond à un clic.

Créez le bouton en XAML.

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

Ou créez le bouton dans le code.

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

Gérez l’événement Click.

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="button-interaction"></a>Interaction de bouton

Lorsque vous appuyez sur un bouton avec un doigt ou un stylet, ou lorsque vous cliquez dessus avec le bouton de la souris, le bouton déclenche l’événement [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Si un bouton est sélectionné au clavier, une pression sur la touche Entrée ou sur la barre d’espace déclenche également l’événement Click.

Généralement, vous ne pouvez pas gérer les événements [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) de bas niveau sur un bouton, car un comportement Click lui est affecté à la place. Pour plus d’informations, voir [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx).

Vous pouvez modifier la façon dont un bouton déclenche l’événement Click en modifiant la propriété [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode). La valeur ClickMode par défaut est **Release**, mais vous pouvez également définir la propriété ClickMode d'un bouton sur **Hover** ou **Press**. Si ClickMode est défini sur **Hover**, l’événement Click ne peut pas être déclenché avec le clavier ou le mode tactile.


### <a name="button-content"></a>Contenu du bouton

Le bouton est un [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx). Sa propriété de contenu XAML est [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx), ce qui permet une syntaxe comme celle-ci pour XAML : `<Button>A button's content</Button>`. Vous pouvez définir n’importe quel objet comme contenu du bouton. Si le contenu est un objet [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), il est affiché dans le bouton. Si le contenu est un autre type d’objet, sa représentation sous forme de chaîne est affichée dans le bouton.

Le contenu d’un bouton correspond généralement à du texte. Voici des recommandations de conception pour les boutons contenant du texte:
-   Utilisez un texte concis, précis et suffisamment explicite qui décrit clairement l’action effectuée par le bouton. Généralement, le texte d’un bouton est représenté par un seul mot, un verbe.
-   Utilisez la police par défaut à moins que vos instructions de personnalisation imposent d’en utiliser une autre.
-   Pour les textes courts, évitez les boutons de commande trop étroits en utilisant une largeur de bouton d'au moins 120px.
- Pour les textes longs, évitez les boutons de commande trop larges en limitant le texte à une longueur maximale de 26caractères.
-   Si le contenu texte du bouton est dynamique (par exemple, s’il est [localisé](../globalizing/globalizing-portal.md)), songez au redimensionnement du bouton et à ses conséquences sur les contrôles environnants.

<table>
<tr>
<td> <b>Vous devez:</b><br> corriger les boutons dont le texte déborde. </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>Option1:</b><br> Augmentez la largeur du bouton, empilez les boutons et faites un renvoi à la ligne si la longueur du texte dépasse 26caractères. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>Option2:</b><br> Augmentez la hauteur du bouton et ajustez le texte avec un renvoi à la ligne. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

Vous pouvez également personnaliser les effets visuels qui constituent l’apparence du bouton. Par exemple, vous pourriez remplacer le texte par une icône, ou utiliser une icône plus du texte.

Ici, un objet **StackPanel** qui contient une image et du texte est défini comme le contenu d’un bouton.

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

Le bouton ressemble à ceci.

![Bouton avec contenu d’image et de texte](images/button-orange.png)

## <a name="create-a-repeat-button"></a>Créer un bouton de répétition

Un [RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) est un bouton qui déclenche les événements [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) plusieurs fois à partir du moment où il est enfoncé jusqu’à ce qu’il soit relâché. Définissez la propriété [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) pour spécifier la durée pendant laquelle RepeatButton attend entre le moment où il est actionné et le moment où il commence à répéter l’action de clic. Définissez la propriété [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) pour spécifier la durée entre les répétitions de l’action de clic. Les durées pour les deux propriétés sont spécifiées en millisecondes.

L’exemple suivant montre deux contrôles RepeatButton dont les événements Click respectifs sont utilisés pour augmenter ou réduire la valeur affichée dans un bloc de texte.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## <a name="create-a-drop-down-button"></a>Créer un bouton déroulant

> DropDownButton requiert Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou de la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) est un bouton qui affiche un chevron sous la forme d’un indicateur visuel qu’il dispose d’un menu volant joint qui contient davantage d’options. Il a le même comportement comme un bouton standard avec un menu volant; l’apparence est différente.

La liste déroulante bouton hérite de l’événement Click, mais vous ne l’utilisez généralement. Au lieu de cela, vous utilisez la propriété de menu volant pour associer un menu volant et appeler des actions à l’aide des options de menu dans le menu volant. Le menu volant s’ouvre automatiquement lorsque le bouton est cliqué.

> [!TIP]
> Pour plus d’informations sur les menus volants, voir les [Menus et menus contextuels](menus.md).

### <a name="example---drop-down-button"></a>Exemple - bouton déroulant

Cet exemple montre comment créer une liste déroulante bouton avec un menu volant qui contient des commandes pour l’alignement de paragraphe dans un contrôle RichEditBox. (Pour plus d’informations et de code, voir [zone d’édition enrichie](rich-edit-box.md)).

![Une liste déroulante bouton avec les commandes d’alignement](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="16" Text="&#xE8E4;"/>
    <DropDownButton.Flyout>
        <MenuFlyout Placement="BottomEdgeAlignedLeft">
            <MenuFlyoutItem Text="Left" Icon="AlignLeft" Tag="left"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Center" Icon="AlignCenter" Tag="center"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Right" Icon="AlignRight" Tag="right"
                            Click="AlignmentMenuFlyoutItem_Click"/>
        </MenuFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

```csharp
private void AlignmentMenuFlyoutItem_Click(object sender, RoutedEventArgs e)
{
    var option = ((MenuFlyoutItem)sender).Tag.ToString();

    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the alignment to the selected paragraphs.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (option == "left")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Left;
        }
        else if (option == "center")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Center;
        }
        else if (option == "right")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Right;
        }
    }
}
```

## <a name="create-a-split-button"></a>Créer un bouton partagé

> Bouton partagé requiert Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou de la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [bouton partagé](/uwp/api/windows.ui.xaml.controls.splitbutton) comporte deux parties qui peuvent être appelés séparément. Une part se comporte comme un bouton standard et appelle une action immédiate. L’autre partie appelle un menu volant contenant des options supplémentaires que l’utilisateur peut choisir.

> [!NOTE]
> Lorsqu’elle est appelée avec une interface tactile, le bouton partagé se comporte comme un bouton déroulant; les deux moitiés du bouton appellent le menu volant. Avec d’autres méthodes d’entrée, un utilisateur peut appeler soit moitié du bouton séparément.

Le comportement classique pour un bouton Fractionné est le suivant:

- Lorsque l’utilisateur clique sur la partie de bouton, gérez l’événement Click pour appeler l’option qui est actuellement sélectionnée dans la liste déroulante.
- Lorsque la liste déroulante est ouverte, appel handle des éléments dans la liste déroulante pour les deux modifications qui option est sélectionnée, puis appelez-le. Il est important d’appeler l’élément de menu volant dans la mesure où l’événement Click ne se produit lors de l’utilisation tactile du bouton.

> [!TIP]
> Il existe de nombreuses façons de placer les éléments dans la liste déroulante vers le bas et de gérer leur invocation. Si vous utilisez un contrôle ListView ou GridView, une consiste à gérer l’événement SelectionChanged. Si vous procédez ainsi, définissez [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) sur **false**. Cela permet aux utilisateurs de naviguer dans les options à l’aide d’un clavier sans appeler l’élément sur chaque modification.

### <a name="example---split-button"></a>Exemple - bouton partagé

Cet exemple montre comment créer un bouton partagé qui est utilisé pour modifier la couleur de premier plan du texte sélectionné dans un contrôle RichEditBox. (Pour plus d’informations et de code, voir [zone d’édition enrichie](rich-edit-box.md)).

![Un bouton partagé pour la sélection de couleur de premier plan](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout" Placement="BottomEdgeAlignedLeft">
            <!-- Set SingleSelectionFollowsFocus="False"
                 so that keyboard navigation works correctly. -->
            <GridView ItemsSource="{x:Bind ColorOptions}" 
                      SelectionChanged="BrushSelectionChanged"
                      SingleSelectionFollowsFocus="False"
                      SelectedIndex="0" Padding="0">
                <GridView.ItemTemplate>
                    <DataTemplate>
                        <Rectangle Fill="{Binding}" Width="20" Height="20"/>
                    </DataTemplate>
                </GridView.ItemTemplate>
                <GridView.ItemContainerStyle>
                    <Style TargetType="GridViewItem">
                        <Setter Property="Margin" Value="2"/>
                        <Setter Property="MinWidth" Value="0"/>
                        <Setter Property="MinHeight" Value="0"/>
                    </Style>
                </GridView.ItemContainerStyle>
            </GridView>
        </Flyout>
    </SplitButton.Flyout>
</SplitButton>
```

```csharp
public sealed partial class MainPage : Page
{
    // Color options that are bound to the grid in the split button flyout.
    private List<SolidColorBrush> ColorOptions = new List<SolidColorBrush>();
    private SolidColorBrush CurrentColorBrush = null;

    public MainPage()
    {
        this.InitializeComponent();

        // Add color brushes to the collection.
        ColorOptions.Add(new SolidColorBrush(Colors.Black));
        ColorOptions.Add(new SolidColorBrush(Colors.Red));
        ColorOptions.Add(new SolidColorBrush(Colors.Orange));
        ColorOptions.Add(new SolidColorBrush(Colors.Yellow));
        ColorOptions.Add(new SolidColorBrush(Colors.Green));
        ColorOptions.Add(new SolidColorBrush(Colors.Blue));
        ColorOptions.Add(new SolidColorBrush(Colors.Indigo));
        ColorOptions.Add(new SolidColorBrush(Colors.Violet));
        ColorOptions.Add(new SolidColorBrush(Colors.White));
    }

    private void BrushButtonClick(object sender, object e)
    {
        // When the button part of the split button is clicked,
        // apply the selected color.
        ChangeColor();
    }

    private void BrushSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        // When the flyout part of the split button is opened and the user selects
        // an option, set their choice as the current color, apply it, then close the flyout.
        CurrentColorBrush = (SolidColorBrush)e.AddedItems[0];
        SelectedColorBorder.Background = CurrentColorBrush;
        ChangeColor();
        BrushFlyout.Hide();
    }

    private void ChangeColor()
    {
        // Apply the color to the selected text in a RichEditBox.
        Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
        if (selectedText != null)
        {
            Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
            charFormatting.ForegroundColor = CurrentColorBrush.Color;
            selectedText.CharacterFormat = charFormatting;
        }
    }
}
```

## <a name="create-a-toggle-split-button"></a>Créer un bouton de fractionnement de bascule

> ToggleSplitButton requiert Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou de la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) comporte deux parties qui peuvent être appelés séparément. Une partie se comporte comme un bouton bascule qui peut être activé ou désactivé. L’autre partie appelle un menu volant contenant des options supplémentaires que l’utilisateur peut choisir.

Un bouton de fractionnement de bascule est généralement utilisé pour activer ou désactiver une fonctionnalité lorsque la fonctionnalité possède plusieurs options que l’utilisateur peut choisir. Par exemple, dans un éditeur de document, elle pourrait servir à activer des listes ou désactiver, tandis que la liste déroulante permet de choisir le style de la liste.

> [!NOTE]
> Lorsqu’elle est appelée avec une interface tactile, le bouton partagé se comporte comme un bouton déroulant. Avec d’autres méthodes d’entrée, un utilisateur peut appeler soit moitié du bouton séparément. Avec une interface tactile, les deux moitiés du bouton appellent le menu volant. Par conséquent, vous devez inclure une option dans le contenu de votre menu volant pour activer/désactiver le bouton activé ou désactivé.

### <a name="differences-with-togglebutton"></a>Différences avec ToggleButton

Contrairement aux [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton), ToggleSplitButton n’a pas d’un état indéterminé. Par conséquent, vous devez garder à l’esprit ces différences:

- ToggleSplitButton n’a pas une propriété **IsThreeState** ou un événement de **indéterminé** .
- La propriété [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) est simplement un **bool**et non une **valeur booléenne nullable**.
- ToggleSplitButton a uniquement l’événement [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged) ; Il n’a pas d’événements **Checked** et **Unchecked** distincts.

### <a name="example---toggle-split-button"></a>Exemple - bascule de bouton partagé

L’exemple suivant montre comment une bascule bouton partagé peut être utilisé pour activer ou désactiver la mise en forme de liste et modifier le style de la liste, dans un contrôle RichEditBox. (Pour plus d’informations et de code, voir [zone d’édition enrichie](rich-edit-box.md)).

![Un bouton bascule fractionner le bouton de sélection des styles de liste](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="16" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout Placement="BottomEdgeAlignedLeft">
            <ListView x:Name="ListStylesListView"
                      SelectionChanged="ListStylesListView_SelectionChanged" 
                      SingleSelectionFollowsFocus="False">
                <StackPanel Tag="bullet" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7C8;"/>
                    <TextBlock Text="Bullet" Margin="8,0"/>
                </StackPanel>
                <StackPanel Tag="alpha" Orientation="Horizontal">
                    <TextBlock Text="A" FontSize="24" Margin="2,0"/>
                    <TextBlock Text="Alpha" Margin="8"/>
                </StackPanel>
                <StackPanel Tag="numeric" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xF146;"/>
                    <TextBlock Text="Numeric" Margin="8,0"/>
                </StackPanel>
                <TextBlock Tag="none" Text="None" Margin="28,0"/>
            </ListView>
        </Flyout>
    </ToggleSplitButton.Flyout>
</ToggleSplitButton>
```

```csharp
private void ListStyleButton_IsCheckedChanged(ToggleSplitButton sender, ToggleSplitButtonIsCheckedChangedEventArgs args)
{
    // Use the toggle button to turn the selected list style on or off.
    if (((ToggleSplitButton)sender).IsChecked == true)
    {
        // On. Apply the list style selected in the drop down to the selected text.
        var listStyle = ((FrameworkElement)(ListStylesListView.SelectedItem)).Tag.ToString();
        ApplyListStyle(listStyle);
    }
    else
    {
        // Off. Make the selected text not a list,
        // but don't change the list style selected in the drop down.
        ApplyListStyle("none");
    }
}

private void ListStylesListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var listStyle = ((FrameworkElement)(e.AddedItems[0])).Tag.ToString();

    if (ListButton.IsChecked == true)
    {
        // Toggle button is on. Turn it off...
        if (listStyle == "none")
        {
            ListButton.IsChecked = false;
        }
        else
        {
            // or apply the new selection.
            ApplyListStyle(listStyle);
        }
    }
    else
    {
        // Toggle button is off. Turn it on, which will apply the selection
        // in the IsCheckedChanged event handler.
        ListButton.IsChecked = true;
    }
}

private void ApplyListStyle(string listStyle)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the list style to the selected text.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (listStyle == "none")
        {  
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.None;
        }
        else if (listStyle == "bullet")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Bullet;
        }
        else if (listStyle == "numeric")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Arabic;
        }
        else if (listStyle == "alpha")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.UppercaseEnglishLetter;
        }
        selectedText.ParagraphFormat = paragraphFormatting;
    }
}
```

## <a name="recommendations"></a>Recommandations

- Assurez-vous que le but et l’état actuel d’un bouton sont clairs pour l’utilisateur.
- Quand plusieurs boutons permettent de faire le même choix (comme dans une boîte de dialogue de confirmation), présentez les boutons de validation dans cet ordre, où [Faire l’action] et [Ne pas faire l’action] sont des réponses spécifiques à l’instruction principale:
    - OK/[Faire l’action]/Oui
    - [Ne pas faire l’action]/Non
    - Annuler
- Exposez seulement un ou deux boutons à la fois pour l’utilisateur, par exemple Accepter et Annuler. Si vous devez inviter l’utilisateur à effectuer plusieurs actions, pensez à utiliser des [cases à cocher](checkbox.md) ou des [cases d’option](radio-button.md) à partir desquelles l’utilisateur peut sélectionner des actions, avec un seul bouton de commande pour les déclencher.
- Pour une action qui doit être disponible sur plusieurs pages dans votre application, pensez à utiliser une [barre d’application inférieure](app-bars.md), au lieu de dupliquer un bouton sur plusieurs pages.

### <a name="recommended-single-button-layout"></a>Disposition recommandée pour un bouton unique

Si votre disposition ne requiert qu’un seul bouton, vous devez l’aligner à gauche ou à droite en fonction du contexte de son conteneur.

- Les boîtes de dialogue avec un seul bouton doivent **aligner à droite** le bouton. Si votre boîte de dialogue contient un seul bouton, assurez-vous que ce bouton effectue l’action sans échec, non destructrice. Si vous utilisez la classe [ContentDialog](dialogs.md) et spécifiez un seul bouton, celui-ci sera automatiquement aligné à droite.

![Un bouton dans une boîte de dialogue](images/pushbutton_doc_dialog.png)

- Si votre bouton s’affiche dans un élément d’interface utilisateur conteneur (par exemple, au sein d’une notification toast, un menu volant ou un élément de l’affichage Liste), vous devez **aligner à droite** le bouton dans le conteneur.

![Un bouton dans un conteneur](images/pushbutton_doc_container.png)

- Dans les pages qui contiennent un bouton unique (par exemple, un bouton «Appliquer» en bas d’une page de paramètres), vous devez **aligner à gauche** le bouton. Cela garantit que le bouton s’aligne avec le reste du contenu de la page.

![Un bouton sur une page](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>Boutons Précédent

Le bouton Précédent est un élément de l’interface utilisateur fournie par le système qui permet de revenir en arrière dans la pile Back ou dans l’historique de navigation de l’utilisateur. Vous n’avez pas besoin de créer votre propre bouton précédent, mais vous devez peut-être effectuer certaines opérations pour obtenir une bonne expérience de navigation vers l’arrière. Pour plus d’informations, voir [Navigation dans l’historique et navigation vers l’arrière](../basics/navigation-history-and-backwards-navigation.md)

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Classe Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [Cases d’option](radio-button.md)
- [Cases à cocher](checkbox.md)
- [Boutons bascule](toggles.md)
