---
Description: Un bouton permet à l’utilisateur de déclencher une action immédiate.
title: Boutons
label: Buttons
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 210431928c5dd7c5d5dfb99855322f1560e91dd7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363230"
---
# <a name="buttons"></a>Boutons

Un bouton permet à l’utilisateur de déclencher une action immédiate. Certains boutons sont spécialisées pour des tâches particulières, telles que la navigation, d’actions répétées ou présentant des menus.

![Exemple de boutons](images/controls/button.png)

L’infrastructure XAML fournit un contrôle de bouton standard, ainsi que plusieurs contrôles bouton spécialisé.

Commande | Description
------- | -----------
[Button](/uwp/api/windows.ui.xaml.controls.button) | Lance une action immédiate. Peut être utilisé avec un événement de clic ou de la liaison de commande.
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | Un bouton qui déclenche un événement Click en continu pendant enfoncé.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | Un bouton qui a une mise en forme comme un lien hypertexte, utilisé pour la navigation. Voir [Liens hypertexte](hyperlinks.md) pour plus d’informations.
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | Bouton avec un chevron pour ouvrir un menu volant attaché.
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | Bouton avec les deux côtés. Côté « un » lance une action, et l’autre côté ouvre un menu.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | Un bouton bascule avec deux côtés. Active ou désactive du côté « un » activé/désactivé, et l’autre côté ouvre un menu.

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| DropDownButton SplitButton et ToggleSplitButton sont inclus dans le cadre de la bibliothèque d’interface utilisateur de Windows, un package NuGet qui contient les nouveaux contrôles et fonctionnalités de l’interface utilisateur pour les applications UWP. Pour plus d’informations, y compris les instructions d’installation, consultez le [vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de plateforme** | **API de bibliothèque de l’interface utilisateur de Windows** |
| - | - |
| [Cliquez sur l’événement](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), [Command, propriété](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [Classe DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton), [SplitButton classe](/uwp/api/microsoft.ui.xaml.controls.splitbutton), [ToggleSplitButton classe](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un **bouton** pour permettre à l’utilisateur de lancer une action immédiate, par exemple, envoyez un formulaire.

N’utilisez pas un bouton lors de l’action consiste à naviguer vers une autre page ; utiliser un [HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) à la place. Voir [Liens hypertexte](hyperlinks.md) pour plus d’informations.
> Exception : Pour la navigation de l’Assistant, utilisez les boutons nommés « Back » et « Suivant ». Pour les autres types de descendante navigation ou la navigation vers un niveau supérieur, utilisez un [bouton Précédent](../basics/navigation-history-and-backwards-navigation.md).

Utilisez un **RepeatButton** lorsque l’utilisateur peut souhaiter déclencher une action à plusieurs reprises. Par exemple, utiliser un RepeatButton pour incrémenter ou décrémenter une valeur dans un compteur.

Utilisez un **DropDownButton** lorsque le bouton a un lanceur qui contient davantage d’options. Le chevron situé par défaut fournit une indication visuelle que le bouton inclut un menu volant.

Utilisez un **SplitButton** lorsque vous souhaitez que l’utilisateur pour pouvoir lancer une action immédiate, ou choisissez indépendamment des options supplémentaires.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Button">ouvrir l’application et voir le bouton en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
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

Lorsque vous appuyez sur un bouton avec un doigt ou un stylet, ou lorsque vous cliquez dessus avec le bouton de la souris, le bouton déclenche l’événement [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Si un bouton est sélectionné au clavier, une pression sur la touche Entrée ou sur la barre d’espace déclenche également l’événement Click.

Généralement, vous ne pouvez pas gérer les événements [PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) de bas niveau sur un bouton, car un comportement Click lui est affecté à la place. Pour plus d’informations, voir [Vue d’ensemble des événements et des événements routés](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

Vous pouvez modifier la façon dont un bouton déclenche l’événement Click en modifiant la propriété [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode). La valeur ClickMode par défaut est **Release**, mais vous pouvez également définir la propriété ClickMode d'un bouton sur **Hover** ou **Press**. Si ClickMode est défini sur **Hover**, l’événement Click ne peut pas être déclenché avec le clavier ou le mode tactile.


### <a name="button-content"></a>Contenu du bouton

Le bouton est un [ContentControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl). Sa propriété de contenu XAML est [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content), ce qui permet une syntaxe comme celle-ci pour XAML : `<Button>A button's content</Button>`. Vous pouvez définir n’importe quel objet comme contenu du bouton. Si le contenu est un objet [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), il est affiché dans le bouton. Si le contenu est un autre type d’objet, sa représentation sous forme de chaîne est affichée dans le bouton.

Le contenu d’un bouton correspond généralement à du texte. Voici des recommandations de conception pour les boutons contenant du texte :
-   Utilisez un texte concis, précis et suffisamment explicite qui décrit clairement l’action effectuée par le bouton. Généralement le texte d’un bouton est représenté par un seul mot, un verbe.
-   Utilisez la police par défaut à moins que vos instructions de personnalisation imposent d’en utiliser une autre.
-   Pour les textes courts, évitez les boutons de commande trop étroits en utilisant une largeur de bouton d'au moins 120 px.
- Pour les textes longs, évitez les boutons de commande trop larges en limitant le texte à une longueur maximale de 26 caractères.
-   Si le contenu texte du bouton est dynamique (par exemple, s’il est [localisé](../globalizing/globalizing-portal.md)), songez au redimensionnement du bouton et à ses conséquences sur les contrôles environnants.

<table>
<tr>
<td> <b>Devoir résoudre :</b><br> corriger les boutons dont le texte déborde. </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>Option 1 :</b><br> Augmentez la largeur du bouton, empilez les boutons et faites un renvoi à la ligne si la longueur du texte dépasse 26 caractères. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>Option 2 :</b><br> Augmentez la hauteur du bouton et ajustez le texte avec un renvoi à la ligne. </td>
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

Un [RepeatButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) est un bouton qui déclenche les événements [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) plusieurs fois à partir du moment où il est enfoncé jusqu’à ce qu’il soit relâché. Définissez la propriété [Delay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.delay) pour spécifier la durée pendant laquelle RepeatButton attend entre le moment où il est actionné et le moment où il commence à répéter l’action de clic. Définissez la propriété [Interval](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.interval) pour spécifier la durée entre les répétitions de l’action de clic. Les durées pour les deux propriétés sont spécifiées en millisecondes.

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

> DropDownButton nécessite Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou le [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) est un bouton qui affiche un chevron comme un indicateur visuel qu’il possède un menu volant attaché qui contient davantage d’options. Il a le même comportement comme un bouton standard avec un menu volant ; seule l’apparence est différente.

La liste déroulante bouton hérite de l’événement Click, mais vous ne l’utilisez généralement. Au lieu de cela, vous utilisez la propriété de menu volant pour attacher un menu volant et appeler des actions à l’aide des options de menu dans le menu volant. Le menu volant s’ouvre automatiquement lorsque le bouton est activé. Veillez à spécifier le [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) propriété de votre menu volant pour vous assurer de l’emplacement souhaité en relation avec le bouton. L’algorithme de sélection élective par défaut peuvent ne pas produire l’emplacement prévu dans toutes les situations.

> [!TIP]
> Pour plus d’informations sur les menus volants, consultez [Menus et des menus contextuels](menus.md).

### <a name="example---drop-down-button"></a>Exemple - bouton déroulant

Cet exemple montre comment créer un bouton déroulant avec un menu volant qui contient des commandes pour l’alignement de paragraphe dans un RichEditBox. (Pour plus d’informations et de code, consultez [zone d’édition enrichie](rich-edit-box.md)).

![Une liste déroulante bouton avec les commandes d’alignement](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8E4;"/>
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

> SplitButton nécessite Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou le [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) comporte deux parties qui peuvent être appelés séparément. Un composant se comporte comme un bouton standard et appelle une action immédiate. L’autre composant appelle un menu volant qui propose des options supplémentaires à l’utilisateur.

> [!NOTE]
> Lorsqu’elle est appelée avec touch, le bouton partagé se comporte comme un bouton déroulant ; les deux moitiés du bouton appeler le menu volant. Avec d’autres méthodes d’entrée, un utilisateur peut appeler moitié du bouton séparément.

Le comportement par défaut pour un bouton partagé est :

- Lorsque l’utilisateur clique sur la partie de bouton, gérez l’événement Click pour appeler l’option qui est actuellement sélectionnée dans la liste déroulante.
- Lorsque la liste déroulante est ouverte, appel de handle des éléments dans la liste déroulante pour les deux modifications option est sélectionnée, puis l’appeler. Il est important de l’appel de l’élément de menu volant car l’événement Click ne se produit lors de l’utilisation tactile de bouton.

> [!TIP]
> Il existe de nombreuses façons de placer des éléments dans la liste déroulante vers le bas et de gérer leur appel. Si vous utilisez un ListView ou le GridView, une façon consiste à gérer l’événement SelectionChanged. Si vous procédez ainsi, la valeur [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) à **false**. Cela permet aux utilisateurs de parcourir les options à l’aide d’un clavier sans appel de l’élément à chaque changement.

### <a name="example---split-button"></a>Exemple - bouton partagé

Cet exemple montre comment créer un bouton partagé qui est utilisé pour modifier la couleur de premier plan du texte sélectionné dans un RichEditBox. (Pour plus d’informations et de code, consultez [zone d’édition enrichie](rich-edit-box.md)).
Fractionner les utilisations du menu volant du bouton [BottomEdgeAlignedLeft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) comme valeur par défaut pour son [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) propriété. Vous ne pouvez pas remplacer cette valeur.

![Un bouton partagé pour la sélection de couleur de premier plan](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout">
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

## <a name="create-a-toggle-split-button"></a>Créer un bouton de fractionnement bascule

> ToggleSplitButton nécessite Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou le [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) comporte deux parties qui peuvent être appelés séparément. Un composant se comporte comme un bouton bascule qui peut être activé ou désactivé. L’autre composant appelle un menu volant qui propose des options supplémentaires à l’utilisateur.

Un bouton de fractionnement bascule sert généralement à activer ou désactiver une fonctionnalité lorsque la fonctionnalité a plusieurs options que l’utilisateur peut choisir. Par exemple, dans un éditeur de document, il peut servir à activer ou désactiver, les listes tandis que la liste déroulante permet de choisir le style de la liste.

> [!NOTE]
> Lorsqu’elle est appelée avec touch, le bouton de fractionnement bascule se comporte comme un bouton déroulant. Avec d’autres méthodes d’entrée, un utilisateur peut activer/désactiver et appeler les deux moitiés du bouton séparément. Avec touch, les deux moitiés du bouton appeler le menu volant. Par conséquent, vous devez inclure une option dans le contenu de votre menu volant pour activer / désactiver le bouton.

### <a name="differences-with-togglebutton"></a>Différences avec ToggleButton

Contrairement aux [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton), ToggleSplitButton n’a pas un état indéterminé. Par conséquent, vous gardez à l’esprit ces différences :

- ToggleSplitButton n’a pas un **IsThreeState** propriété ou **indéterminé** événement.
- Le [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) propriété est simplement un **bool**, et non un **nullable bool**.
- ToggleSplitButton a uniquement les [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged) événement ; il n’a pas distinct **Checked** et **Unchecked** événements.

### <a name="example---toggle-split-button"></a>Exemple - bascule de bouton partagé

L’exemple suivant montre comment une bascule de bouton partagé peut être utilisée pour activer ou désactiver la mise en forme de liste et modifier le style de la liste, dans un RichEditBox. (Pour plus d’informations et de code, consultez [zone d’édition enrichie](rich-edit-box.md)).
Activer/désactiver fractionner utilise du menu volant du bouton [BottomEdgeAlignedLeft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) comme valeur par défaut pour son [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) propriété. Vous ne pouvez pas remplacer cette valeur.

![Un bouton de fractionnement de bascule pour sélectionner des styles de liste](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout>
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
- Quand plusieurs boutons permettent de faire le même choix (comme dans une boîte de dialogue de confirmation), présentez les boutons de validation dans cet ordre, où [Faire l’action] et [Ne pas faire l’action] sont des réponses spécifiques à l’instruction principale :
    - OK/[Faire l’action]/Oui
    - [Ne pas faire l’action]/Non
    - Cancel
- Exposez seulement un ou deux boutons à la fois pour l’utilisateur, par exemple Accepter et Annuler. Si vous devez inviter l’utilisateur à effectuer plusieurs actions, pensez à utiliser des [cases à cocher](checkbox.md) ou des [cases d’option](radio-button.md) à partir desquelles l’utilisateur peut sélectionner des actions, avec un seul bouton de commande pour les déclencher.
- Pour une action qui doit être disponible sur plusieurs pages dans votre application, pensez à utiliser une [barre d’application inférieure](app-bars.md), au lieu de dupliquer un bouton sur plusieurs pages.

### <a name="recommended-single-button-layout"></a>Disposition recommandée pour un bouton unique

Si votre disposition ne requiert qu’un seul bouton, vous devez l’aligner à gauche ou à droite en fonction du contexte de son conteneur.

- Les boîtes de dialogue avec un seul bouton doivent **aligner à droite** le bouton. Si votre boîte de dialogue contient un seul bouton, assurez-vous que ce bouton effectue l’action sans échec, non destructrice. Si vous utilisez la classe [ContentDialog](dialogs.md) et spécifiez un seul bouton, celui-ci sera automatiquement aligné à droite.

![Un bouton dans une boîte de dialogue](images/pushbutton_doc_dialog.png)

- Si votre bouton s’affiche dans un élément d’interface utilisateur conteneur (par exemple, au sein d’une notification toast, un menu volant ou un élément de l’affichage Liste), vous devez **aligner à droite** le bouton dans le conteneur.

![Un bouton dans un conteneur](images/pushbutton_doc_container.png)

- Dans les pages qui contiennent un bouton unique (par exemple, un bouton « Appliquer » en bas d’une page de paramètres), vous devez **aligner à gauche** le bouton. Cela garantit que le bouton s’aligne avec le reste du contenu de la page.

![Un bouton sur une page](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>Boutons Précédent

Le bouton Précédent est un élément de l’interface utilisateur fournie par le système qui permet de revenir en arrière dans la pile Back ou dans l’historique de navigation de l’utilisateur. Vous n’avez pas besoin de créer votre propre bouton précédent, mais vous devez peut-être effectuer certaines opérations pour obtenir une bonne expérience de navigation vers l’arrière. Pour plus d’informations, voir [Navigation dans l’historique et navigation vers l’arrière](../basics/navigation-history-and-backwards-navigation.md)

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Classe Button](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button)
- [Cases d’option](radio-button.md)
- [Cases à cocher](checkbox.md)
- [Activer/désactiver commutateurs](toggles.md)
