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
ms.openlocfilehash: e454bed6dc1a9429fe313e305dc9ba818d86e765
ms.sourcegitcommit: 802699ce8d21e7fa4639f0b19b1c5b6c46c2c727
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68835642"
---
# <a name="buttons"></a>Boutons

Un bouton permet à l’utilisateur de déclencher une action immédiate. Certains boutons conviennent à des tâches spéciales, telles que la navigation, les actions répétées ou l’affichage des menus.

![Exemple de boutons](images/controls/button.png)

Le framework [XAML (Extensible Application Markup Language)](../../xaml-platform/xaml-overview.md) fournit un contrôle de bouton standard, ainsi que plusieurs contrôles de boutons spécialisés.

Commande | Description
------- | -----------
[Button](/uwp/api/windows.ui.xaml.controls.button) | Bouton qui lance une action immédiate. Peut être utilisé avec un événement [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) ou une liaison de [commande](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | Bouton qui déclenche un événement [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) en continu quand l’utilisateur appuie dessus.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | Bouton qui ressemble à un lien hypertexte et qui est utilisé pour la navigation. Pour plus d’informations sur les liens hypertexte, consultez [Liens hypertexte](hyperlinks.md).
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | Bouton comprenant un chevron pour ouvrir un menu volant attaché.
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | Bouton à deux côtés. Un côté lance une action et l’autre ouvre un menu.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | Bouton bascule à deux côtés. Un côté active ou désactive, et l’autre ouvre un menu.
[ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton) | Un bouton que l’on peut activer ou désactiver.

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| **DropDownButton**, **SplitButton** et **ToggleSplitButton** sont inclus dans la bibliothèque d’interface utilisateur Windows. Cette bibliothèque est un package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur des applications UWP. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de plateforme** | **API de la bibliothèque d’interface utilisateur Windows** |
| - | - |
| [Click, événement](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)<br/> [Command, propriété](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [DropDownButton, classe](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)<br/> [SplitButton, classe](/uwp/api/microsoft.ui.xaml.controls.splitbutton)<br/> [ToggleSplitButton, classe](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un contrôle **Button** pour permettre à l’utilisateur de lancer une action immédiate, comme l’envoi d’un formulaire.

N’utilisez pas de contrôle **Button** quand l’action consiste à naviguer vers une autre page. Utilisez plutôt un contrôle [HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton). Pour plus d’informations sur les liens hypertexte, consultez [Liens hypertexte](hyperlinks.md).

> [!IMPORTANT]
> Pour la navigation dans un Assistant, utilisez les boutons *Précédent* et *Suivant*. Pour les autres types de navigation vers l’arrière ou vers un niveau supérieur, utilisez un [bouton Précédent](../basics/navigation-history-and-backwards-navigation.md).

Utilisez un contrôle **RepeatButton** quand l’utilisateur est susceptible de déclencher une action à plusieurs reprises. Par exemple, utilisez un contrôle **RepeatButton** pour incrémenter ou décrémenter une valeur dans un compteur.

Utilisez un contrôle **DropDownButton** quand le bouton comprend un menu volant qui contient d’autres options. Le chevron par défaut indique que le bouton comprend un menu volant.

Utilisez un contrôle **SplitButton** quand vous souhaitez permettre à l’utilisateur de lancer une action immédiate ou de choisir parmi des options supplémentaires de façon indépendante.

Utilisez un contrôle **ToggleButton** lorsque vous souhaitez que l’utilisateur puisse basculer immédiatement entre deux états mutuellement exclusifs et qu’un bouton est le mieux adapté à vos besoins en matière d’interface utilisateur. Si votre interface utilisateur ne dispose pas d’un bouton, il peut être préférable d'utiliser un contrôle [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [CheckBox](checkbox.md), [RadioButton](radio-button.md), ou [ToggleSwitch](toggles.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous avez installé <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Button">ouvrir l’application et voir le bouton en contexte</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Cet exemple utilise deux boutons, **Autoriser** et **Bloquer**, dans une boîte de dialogue demandant l’accès à la géolocalisation.

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

Gérez l’événement [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click).

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

Quand vous appuyez sur un contrôle **Button** avec un doigt ou un stylet, ou quand vous cliquez dessus avec le bouton gauche de la souris, le bouton déclenche l’événement [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Si un bouton est sélectionné au clavier, une pression sur la touche Entrée ou sur la barre d’espace déclenche également l’événement **Click**.

Généralement, vous ne pouvez pas gérer les événements [PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) de bas niveau sur un objet **Button**, car un comportement **Click** lui est affecté à la place. Pour plus d’informations, voir [Vue d’ensemble des événements et des événements routés](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

Vous pouvez modifier la façon dont un bouton déclenche l’événement **Click** en modifiant la propriété [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode). La valeur par défaut de **ClickMode** est **Release**, mais vous pouvez également affecter **Hover** ou **Press** à la valeur **ClickMode** d’un bouton. Si **ClickMode** a la valeur **Hover**, l’événement **Click** ne peut pas être déclenché à l’aide du clavier ou du mode tactile.


### <a name="button-content"></a>Contenu du bouton

**Button** est un contrôle de contenu de la classe [ContentControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl). Sa propriété de contenu XAML est [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content), ce qui permet une syntaxe comme celle-ci pour XAML : `<Button>A button's content</Button>`. Vous pouvez définir n’importe quel objet comme contenu du bouton. Si le contenu est un objet [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), il est affiché dans le bouton. Si le contenu est un autre type d’objet, sa représentation sous forme de chaîne est affichée dans le bouton.

Un bouton contient généralement du texte. Quand vous concevez ce texte, utilisez les recommandations suivantes :

-  Utilisez un texte concis, précis et suffisamment explicite qui décrit clairement l’action effectuée par le bouton. Le texte d’un bouton est généralement représenté par un seul mot, un verbe.

-  Utilisez la police par défaut à moins que vos instructions de personnalisation imposent d’en utiliser une autre.

-  Pour les textes courts, évitez les boutons de commande trop étroits en utilisant une largeur de bouton d’au moins 120 px.

- Pour les textes longs, évitez les boutons de commande trop larges en limitant le texte à 26 caractères.

-  Si le texte du bouton est dynamique (par exemple, s’il est [localisé](../globalizing/globalizing-portal.md)), songez au redimensionnement du bouton et à ses conséquences sur les contrôles environnants.

<table>
<tr>
<td> <b>Vous devez :</b><br> Corriger les boutons dont le texte déborde. </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>Option 1 :</b><br> Augmenter la largeur du bouton, empiler les boutons et faire un renvoi à la ligne si la longueur du texte dépasse 26 caractères. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>Option 2 :</b><br> Augmenter la hauteur du bouton et renvoyer le texte à la ligne. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

Vous pouvez également personnaliser les visuels qui constituent l’apparence du bouton. Par exemple, vous pouvez remplacer le texte par une icône ou utiliser une icône en plus du texte.

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

Un contrôle [RepeatButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) est un bouton qui déclenche des événements [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) plusieurs fois à partir du moment où il est enfoncé jusqu’à ce qu’il soit relâché. Définissez la propriété [Delay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.delay) pour spécifier la durée pendant laquelle le contrôle **RepeatButton** attend entre le moment où il est actionné et le moment où il commence à répéter l’action de clic. Définissez la propriété [Interval](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.interval) pour spécifier la durée entre les répétitions de l’action de clic. Les durées pour les deux propriétés sont spécifiées en millisecondes.

L’exemple suivant montre deux contrôles **RepeatButton** dont les événements **Click** respectifs sont utilisés pour augmenter ou réduire la valeur affichée dans un bloc de texte.

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

> L’utilisation de **DropDownButton** nécessite la [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/) ou Windows 10 version 1809 (SDK 17763) ou ultérieure. Pour télécharger la dernière version du SDK, consultez [SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) ; pour télécharger un SDK antérieur, consultez [Archive du SDK Windows et de l’émulateur](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive).

Un bouton déroulant ([DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton)) est un bouton qui comprend un chevron, ce qui indique qu’il contient un menu volant attaché comprenant des options supplémentaires. Son comportement est le même que celui d’un contrôle **Button** standard avec un menu volant. Seule l’apparence est différente.

Le bouton déroulant hérite de l’événement **Click**, mais vous ne l’utilisez généralement pas. En effet, vous utilisez la propriété **Flyout** pour attacher un menu volant et appeler des actions à l’aide des options du menu volant. Le menu volant s’ouvre automatiquement lorsque l’utilisateur clique sur le bouton.
Spécifiez la propriété [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) de votre menu volant pour choisir l’emplacement souhaité par rapport au bouton. L’algorithme de placement par défaut peut ne pas toujours fournir l’emplacement prévu.

> [!TIP]
> Pour plus d’informations sur les menus volants, consultez [Menus et menus contextuels](menus.md).

### <a name="example---drop-down-button"></a>Exemple - Bouton déroulant

Cet exemple montre comment créer un bouton déroulant avec un menu volant comprenant des commandes pour l’alignement des paragraphes d’un **RichEditBox**. (pour plus d’informations et de code, consultez [Zone d’édition enrichie](rich-edit-box.md)).

![Bouton déroulant avec commandes d’alignement](images/drop-down-button-align.png)

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

 > [!IMPORTANT]
 > L’utilisation de **SplitButton** nécessite la [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/) ou Windows 10 version 1809 (SDK 17763) ou ultérieure. Pour télécharger la dernière version du SDK, consultez [SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) ; pour télécharger un SDK antérieur, consultez [Archive du SDK Windows et de l’émulateur](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive).

Un contrôle [SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) se compose de deux parties qui peuvent être appelées séparément. Un composant se comporte comme un bouton standard et appelle une action immédiate. L’autre composant appelle un menu volant qui propose des options supplémentaires à l’utilisateur.

> [!NOTE]
> Lorsqu’il est appelé à l’aide d’une interaction tactile, le bouton partagé se comporte comme un bouton déroulant. Les deux parties du bouton appellent le menu volant. Avec d’autres méthodes d’entrée, l’utilisateur peut appeler séparément l’une ou l’autre de ces parties.

Le comportement par défaut d’un bouton partagé est le suivant :

- Quand l’utilisateur clique sur la partie bouton, l’événement **Click** est géré de manière à appeler l’option actuellement sélectionnée dans la liste déroulante.

- Lorsque la liste est déroulée, l’appel des éléments de la liste déroulante est géré de manière à changer l’option sélectionnée, puis à l’appeler. Il est important d’appeler l’élément de menu volant, car l’événement **Click** ne se produit pas lors d’une interaction tactile.

> [!TIP]
> Il existe de nombreuses façons de positionner les éléments d’une liste déroulante et de gérer leur appel. Si vous utilisez **ListView** ou **GridView**, vous pouvez gérer l’événement **SelectionChanged**. Si vous procédez ainsi, définissez [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) sur **false**. Cela permet aux utilisateurs de parcourir les options à l’aide du clavier sans appeler l’élément à chaque changement.

### <a name="example---split-button"></a>Exemple - Bouton partagé

Cet exemple montre comment créer un bouton partagé permettant de changer la couleur de premier plan du texte sélectionné dans un contrôle **RichEditBox**. (pour plus d’informations et de code, consultez [Zone d’édition enrichie](rich-edit-box.md)).
Le menu volant du bouton partagé utilise [BottomEdgeAlignedLeft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) comme valeur par défaut pour sa propriété [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement). Vous ne pouvez pas remplacer cette valeur.

![Un bouton partagé pour sélectionner la couleur de premier plan](images/split-button-rtb.png)

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

## <a name="create-a-toggle-split-button"></a>Créer un bouton partagé de basculement

> [!NOTE]
> L’utilisation de **ToggleSplitButton** nécessite la [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/) ou Windows 10 version 1809 (SDK 17763) ou ultérieure. Pour télécharger la dernière version du SDK, consultez [SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) ; pour télécharger un SDK antérieur, consultez [Archive du SDK Windows et de l’émulateur](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive).

Un contrôle [ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) se compose de deux parties qui peuvent être appelées séparément. Un composant se comporte comme un bouton bascule qui peut être activé ou désactivé. L’autre composant appelle un menu volant qui propose des options supplémentaires à l’utilisateur.

Un bouton partagé de basculement sert généralement à activer et à désactiver une fonctionnalité, lorsque celle-ci propose plusieurs options. Par exemple, dans un éditeur de document, il peut servir à activer ou à désactiver des listes, tandis que la liste déroulante est utilisée pour choisir le style de la liste.

> [!NOTE]
> Lorsqu’il est appelé par une interaction tactile, le bouton partagé de basculement se comporte comme un bouton déroulant. Avec d’autres méthodes d’entrée, l’utilisateur peut basculer d’une partie à l’autre, et les appeler séparément. Lors d’une interaction tactile, les deux parties du bouton appellent le menu volant. Par conséquent, vous devez inclure dans le menu volant une option permettant d’activer et de désactiver le bouton.


### <a name="differences-with-togglebutton"></a>Différences entre ToggleSplitButton et ToggleButton

Contrairement à [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton), **ToggleSplitButton** n’a pas d’état indéterminé. Vous devez donc garder à l’esprit les différences suivantes :

- **ToggleSplitButton** n’a pas de propriété **IsThreeState** ni d’événement **Indeterminate**.
- La propriété [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) est un booléen, et non un **Nullable<bool>** .
- **ToggleSplitButton** comprend uniquement l’événement [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged). Il n’a pas d’événements **Checked** et **Unchecked** distincts.


### <a name="example---toggle-split-button"></a>Exemple - Bouton partagé de basculement

L’exemple suivant montre comment un bouton partagé de basculement peut être utilisé pour activer ou désactiver la mise en forme des listes, et pour modifier le style de la liste dans un contrôle **RichEditBox**. (pour plus d’informations et de code, consultez [Zone d’édition enrichie](rich-edit-box.md)).
Le menu volant du bouton partagé de basculement utilise [BottomEdgeAlignedLeft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) comme valeur par défaut pour sa propriété [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement). Vous ne pouvez pas remplacer cette valeur.

![Bouton partagé de basculement pour sélectionner des styles de liste](images/toggle-split-button-open.png)

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

- Exposez seulement un ou deux boutons à la fois pour l’utilisateur, par exemple **Accepter** et **Annuler**. Si vous devez inviter l’utilisateur à effectuer plusieurs actions, pensez à utiliser des [cases à cocher](checkbox.md) ou des [cases d’option](radio-button.md) à partir desquelles l’utilisateur peut sélectionner des actions, avec un seul bouton de commande pour les déclencher.

- Pour une action qui doit être disponible sur plusieurs pages dans votre application, pensez à utiliser une [barre d’application inférieure](app-bars.md), au lieu de dupliquer un bouton sur plusieurs pages.


### <a name="recommended-single-button-layout"></a>Disposition recommandée pour un bouton seul

Si votre disposition ne nécessite qu’un seul bouton, celui-ci doit être aligné à gauche ou à droite en fonction du contexte de son conteneur.

  - Dans les boîtes de dialogue ne comprenant qu’un seul bouton, celui-ci doit être **aligné à droite**. Si votre boîte de dialogue contient un seul bouton, vérifiez que celui-ci effectue une action non destructrice. Si vous utilisez [ContentDialog](dialogs.md) et spécifiez un seul bouton, celui-ci sera automatiquement aligné à droite.

    ![Un bouton dans une boîte de dialogue](images/pushbutton_doc_dialog.png)

  - Si votre bouton s’affiche dans l’interface utilisateur d’un conteneur (par exemple, dans une notification toast, un menu volant ou un élément de liste), il doit être **aligné à droite** dans le conteneur.

    ![Un bouton dans un conteneur](images/pushbutton_doc_container.png)

  - Dans les pages qui contiennent un seul bouton (par exemple, un bouton **Appliquer** au bas d’une page de paramètres), le bouton doit être **aligné à gauche**. Vous avez ainsi la garantie que le bouton est aligné sur le reste du contenu de la page.

    ![Un bouton dans une page](images/pushbutton_doc_page.png)


## <a name="back-buttons"></a>Boutons Précédent

Le bouton Précédent est un élément de l’interface utilisateur fournie par le système qui permet de revenir en arrière dans la pile Back ou dans l’historique de navigation de l’utilisateur. Vous n’avez pas besoin de créer votre propre bouton précédent, mais vous devez peut-être effectuer certaines opérations pour obtenir une bonne expérience de navigation vers l’arrière. Pour plus d’informations, consultez [Historique de navigation et navigation vers l’arrière pour les applications UWP](../basics/navigation-history-and-backwards-navigation.md).


## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : Cet exemple montre tous les contrôles XAML dans un format interactif.


## <a name="related-articles"></a>Articles connexes

- [Button, classe](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button)
- [Cases d’option](radio-button.md)
- [Cases à cocher](checkbox.md)
- [Boutons bascule](toggles.md)
