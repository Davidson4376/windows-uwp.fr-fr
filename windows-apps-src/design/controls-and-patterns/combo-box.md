---
author: Jwmsft
Description: A text entry box that provides suggestions as the user types.
title: Zone de liste déroulante (zone de liste déroulante)
label: Combo box
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 0c34dda3039a9b6a66428266e37f81b41695fbc0
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6969916"
---
# <a name="combo-box"></a>Zone de liste modifiable

Utilisez une zone de liste déroulante (également appelé une liste déroulante) pour présenter une liste d’éléments à partir de l’utilisateur peut sélectionner. Une zone de liste déroulante démarre dans un état compact et se développe pour afficher une liste d’éléments sélectionnables.

Lorsque la zone de liste déroulante est fermée, elle affiche la sélection actuelle ou est vide si aucun élément sélectionné. Lorsque l’utilisateur développe la zone de liste modifiable, il affiche la liste d’éléments sélectionnables.

> **API importantes**: [classe ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [IsEditable propriété](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [propriété Text](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [TextSubmitted événement](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

Une zone de liste déroulante en état compact avec un en-tête.

![Exemple de liste déroulante à l’état compact](images/combo_box_collapsed.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

- Utilisez une liste déroulante pour permettre aux utilisateurs de sélectionner une valeur unique parmi un ensemble d’éléments qui peuvent être représentés correctement à l’aide de simples lignes de texte.
- Utilisez un affichage Liste ou Grille au lieu d’une zone de liste déroulante pour afficher des éléments contenant plusieurs lignes de texte ou images.
- En présence de moins de cinq éléments, utilisez des [cases d’option](radio-button.md) (si un seul élément peut être sélectionné) ou des [cases à cocher](checkbox.md) (si plusieurs éléments peuvent être sélectionnés).
- Utilisez cette zone de liste déroulante pour les éléments de sélection d’importance secondaire au sein de votre application. Si l’option par défaut est recommandée pour la plupart des utilisateurs dans la majorité des situations, l’affichage de tous les éléments à l’aide d’un affichage Liste risque d’attirer l’attention sur les options plus qu’il n’est nécessaire. Pour économiser de l’espace et éviter de distraire l’utilisateur, utilisez une zone de liste déroulante.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/ComboBox">Ouvrir l’application et voir zone de liste déroulante en action.</a></p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Une zone de liste déroulante en état compact peut afficher un en-tête.

![Exemple de liste déroulante à l’état compact](images/combo_box_collapsed.png)

Bien que les zones de listes déroulantes se développent pour prendre en charge des chaînes plus longues, évitez les chaînes excessivement longues qui rendent la lecture difficile.

![Exemple de liste déroulante avec une longue chaîne de texte](images/combo_box_listitemstate.png)

Si la collection figurant dans une zone de liste déroulante est suffisamment longue, une barre de défilement s’affiche. Regroupez les éléments logiquement dans la liste.

![Exemple de barre de défilement dans une liste déroulante](images/combo_box_scroll.png)

## <a name="create-a-combo-box"></a>Créer une zone de liste modifiable

Vous remplissez la zone de liste déroulante en ajoutant des objets directement à la collection [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) ou en liant la propriété [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) à une source de données. Les éléments ajoutés à la zone de liste déroulante sont encapsulées dans les conteneurs [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) .

Voici une zone de liste déroulante simple avec des éléments ajoutés en XAML.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue<x:String>
    <x:String>Green<x:String>
    <x:String>Red<x:String>
    <x:String>Yellow<x:String>
</ComboBox>
```

L’exemple suivant illustre une zone de liste déroulante de la liaison à une collection d’objets FontFamily.

```xaml
<ComboBox x:Name="FontsCombo" Header="Fonts" Height="44" Width="296"
          ItemsSource="{x:Bind fonts}" DisplayMemberPath="Source"/>
```

```csharp
ObservableCollection<FontFamily> fonts = new ObservableCollection<FontFamily>();

public MainPage()
{
    this.InitializeComponent();
    fonts.Add(new FontFamily("Arial"));
    fonts.Add(new FontFamily("Courier New"));
    fonts.Add(new FontFamily("Times New Roman"));
}
```

### <a name="item-selection"></a>Sélection d'élément

Comme les contrôles ListView et GridView, ComboBox est dérivé de [sélecteur](/uwp/api/windows.ui.xaml.controls.primitives.selector), afin d’obtenir la sélection de l’utilisateur de la même manière standard.

Vous pouvez obtenir ou définir la zone de liste modifiable sélectionné l’élément à l’aide de la propriété [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) et obtenir ou définir l’index de l’élément sélectionné à l’aide de la propriété [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) .

Pour obtenir la valeur d’une propriété spécifique sur l’élément de données sélectionné, vous pouvez utiliser la propriété [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) . Dans ce cas, définissez le [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) pour spécifier la propriété sur l’élément sélectionné permettant d’obtenir la valeur.

> [!TIP]
> Si vous définissez SelectedItem ou SelectedIndex pour indiquer la sélection par défaut, une exception se produit si la propriété est définie avant que la collection d’éléments de liste déroulante zone est remplie. Sauf si vous définissez vos éléments en XAML, il est préférable de gérer l’événement chargé de zone de liste déroulante, définissez SelectedItem ou SelectedIndex dans le Gestionnaire d’événements chargé.

Vous pouvez établir une liaison à ces propriétés en XAML, ou gérer l’événement [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) pour répondre aux modifications de sélection.

Dans l’événement code du gestionnaire, vous pouvez obtenir l’élément sélectionné à partir de la propriété [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) . Vous pouvez obtenir l’élément sélectionné précédemment (le cas échéant) à partir de la propriété [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) . Les collections AddedItems et RemovedItems contiennent uniquement 1 élément, car la zone de liste modifiable ne prend pas en charge la sélection multiple.

Cet exemple montre comment gérer l’événement SelectionChanged et également comment lier à l’élément sélectionné.

```xaml
<StackPanel>
    <ComboBox x:Name="colorComboBox" Width="200"
              Header="Colors" PlaceholderText="Pick a color"
              SelectionChanged="ColorComboBox_SelectionChanged">
        <x:String>Blue</x:String>
        <x:String>Green</x:String>
        <x:String>Red</x:String>
        <x:String>Yellow</x:String>
    </ComboBox>

    <Rectangle x:Name="colorRectangle" Height="30" Width="100"
               Margin="0,8,0,0" HorizontalAlignment="Left"/>

    <TextBlock Text="{x:Bind colorComboBox.SelectedIndex, Mode=OneWay}"/>
    <TextBlock Text="{x:Bind colorComboBox.SelectedItem, Mode=OneWay}"/>
</StackPanel>
```

```csharp
private void ColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Add "using Windows.UI;" for Color and Colors.
    string colorName = e.AddedItems[0].ToString();
    Color color;
    switch (colorName)
    {
        case "Yellow":
            color = Colors.Yellow;
            break;
        case "Green":
            color = Colors.Green;
            break;
        case "Blue":
            color = Colors.Blue;
            break;
        case "Red":
            color = Colors.Red;
            break;
    }
    colorRectangle.Fill = new SolidColorBrush(color);
}
```

#### <a name="selectionchanged-and-keyboard-navigation"></a>Navigation SelectionChanged et du clavier

Par défaut, l’événement SelectionChanged se produit lorsqu’un utilisateur clique sur, appuie ou appuie sur entrée sur un élément dans la liste pour valider leur sélection, et la zone de liste déroulante se ferme. Sélection ne change pas lorsque l’utilisateur parcourt la liste de zone de liste modifiable ouverte avec les touches de direction du clavier.

Pour créer une liste déroulante «à jour dynamique» pendant que l’utilisateur navigue la liste Ouvrir avec les touches de direction (comme une sélection déroulante Police), définissez [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) sur [toujours](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger). Cela déclenche l’événement SelectionChanged se produisent lorsque le focus change à un autre élément dans la liste Ouvrir.

#### <a name="selected-item-behavior-change"></a>Changement de comportement d’élément sélectionné

Dans Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou une version ultérieure, le comportement des éléments sélectionnés est mis à jour pour prendre en charge des zones de liste déroulante modifiable.

Avant le Kit de développement logiciel 17763, la valeur de la propriété SelectedItem (et par conséquent, SelectedValue et SelectedIndex) a été requis pour être dans la collection d’éléments de la zone de liste modifiable. À l’aide de l’exemple précédent, en définissant `colorComboBox.SelectedItem = "Pink"` entraîne:

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

Dans le Kit de développement logiciel 17763 et versions ultérieur, la valeur de la propriété SelectedItem (et par conséquent, SelectedValue et SelectedIndex) n’est pas nécessaire pour être dans la collection d’éléments de la zone de liste modifiable. À l’aide de l’exemple précédent, en définissant `colorComboBox.SelectedItem = "Pink"` entraîne:

- SelectedItem = rose
- SelectedValue = rose
- SelectedIndex = -1

### <a name="text-search"></a>Recherche en texte

Les zones de liste déroulante prennent automatiquement en charge la recherche au sein de leurs collections. Lorsque les utilisateurs tapent des caractères sur un clavier physique dans une zone de liste déroulante ouverte ou fermée, des candidats correspondant à la chaîne entrée apparaissent. Cette fonctionnalité est particulièrement utile lors de la navigation dans une liste longue. Par exemple, lors de l’interaction avec une liste déroulante contenant une liste des États, les utilisateurs peuvent appuyer sur la touche «w» à «Washington» afficher pour une sélection rapide. La recherche de texte n’est pas la casse.

Vous pouvez définir la propriété [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) sur **false** pour désactiver cette fonctionnalité.

## <a name="make-a-combo-box-editable"></a>Configurer une zone de liste déroulante modifiable

> [!IMPORTANT]
> Cette fonctionnalité nécessite Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou une version ultérieure.

Par défaut, une zone de liste déroulante permet à l’utilisateur une sélection parmi une liste prédéfinie d’options. Toutefois, il existe des cas où la liste contient uniquement un sous-ensemble de valeurs valides, et l’utilisateur doit être en mesure d’entrer d’autres valeurs qui ne sont pas répertoriés. Pour ce faire, vous pouvez rendre la zone de liste déroulante modifiable.

Pour rendre une zone de liste modifiable, définir la propriété [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) sur **true**. Ensuite, gérez l’événement [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) puisse fonctionner avec la valeur entrée par l’utilisateur.

Par défaut, la valeur SelectedItem est mis à jour lorsque l’utilisateur valide dans le texte personnalisé. Vous pouvez remplacer ce comportement en définissant la **propriété Handled** sur **true** dans les arguments d’événement TextSubmitted. Lorsque l’événement est marqué comme géré, la zone de liste déroulante n’aura aucune action supplémentaire après l’événement et reste dans l’état de modification. SelectedItem n’est pas mis à jour.

Cet exemple montre une zone de liste déroulante modifiable simple. La liste contient des chaînes simples et n’importe quelle valeur entrée par l’utilisateur est utilisée comme entrée.

Un sélecteur de «les noms utilisés récemment» permet à l’utilisateur de saisir des chaînes personnalisées. La liste 'RecentlyUsedNames' contienne des valeurs que l’utilisateur peut choisir, mais l’utilisateur peut également ajouter une nouvelle valeur personnalisée. La propriété «CurrentName» représente le nom actuellement saisi.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>Texte présenté

Vous pouvez gérer l’événement [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) puisse fonctionner avec la valeur entrée par l’utilisateur. Dans le Gestionnaire d’événements, vous allez généralement valider que la valeur entrée par l’utilisateur est valide, des événements ensuite utiliser la valeur dans votre application. Selon la situation, vous pouvez également ajouter la valeur à la liste de la zone de liste déroulante d’options pour une utilisation ultérieure.

L’événement TextSubmitted se produit lorsque ces conditions sont remplies:

- La propriété IsEditable est **true**
- L’utilisateur entre du texte qui ne correspond pas à une entrée existante dans la zone de liste modifiable
- L’utilisateur appuie sur entrée ou déplace le focus à partir de la zone de liste modifiable.

L’événement TextSubmitted ne se produit pas si l’utilisateur entre du texte, puis navigue vers le haut ou vers le bas par le biais de la liste.

### <a name="sample---validate-input-and-use-locally"></a>Exemple: valider l’entrée et utiliser localement

Dans cet exemple, un sélecteur de taille de police contient un ensemble de valeurs correspondant à la gamme de taille de police, mais l’utilisateur peut entrer des tailles qui ne figurent pas dans la liste.

Lorsque l’utilisateur ajoute une valeur qui n’est pas dans la liste, les mises à jour de taille de police, mais la valeur n’est pas ajouté à la liste des tailles de police.

Si la valeur qui vient d’être entrée n’est pas valide, ce qui que vous permet de revenir à la dernière de la propriété Text SelectedValue connu valeur correcte.

```xaml
<ComboBox x:Name="fontSizeComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfFontSizes}"
          TextSubmitted="FontSizeComboBox_TextSubmitted"/>
```

```csharp
private void FontSizeComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (byte.TryParse(e.Text, out double newValue))
    {
        // Update the app’s font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>Exemple: à valider l’entrée et à ajouter à la liste

Ici, un sélecteur de couleur préférée»» contient les couleurs favorites plus courants (rouge, bleu, vert, Orange), mais l’utilisateur peut entrer une couleur préférée qui n’est pas dans la liste. Lorsque l’utilisateur ajoute une couleur valide (comme rose), la couleur qui vient d’être entrée est ajoutée à la liste et définir en tant que l’actif «couleur préférée».

```xaml
<ComboBox x:Name="favoriteColorComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfColors}"
          TextSubmitted="FavoriteColorComboBox_TextSubmitted"/>
```

```csharp
private void FavoriteColorComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (IsValid(e.Text))
    {
        FavoriteColor newColor = new FavoriteColor()
        {
            ColorName = e.Text,
            Color = ColorFromStringConverter(e.Text)
        }
        ListOfColors.Add(newColor);
    }
    else
    {
        // If the item is invalid, reject it but do not revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        e.Handled = true;
    }
}

bool IsValid(string Text)
{
    // Validate that the string is: not empty; a color.
}
```

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Limitez le contenu texte des éléments de zone de liste déroulante à une seule ligne.
- Triez les éléments d’une zone de liste déroulante dans l’ordre le plus logique. Regroupez les options associées et placez les options les plus courantes en haut. Triez les noms par ordre alphabétique, les nombres par ordre numérique et les dates par ordre chronologique.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.
- [Exemple AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Articles associés

- [Contrôles de texte](text-controls.md)
- [Vérification de l’orthographe](text-controls.md)
- [Recherche](search.md)
- [Classe TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Classe PasswordBox Windows.UI.Xaml.Controls](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propriété String.Length](https://msdn.microsoft.com/library/system.string.length.aspx)
