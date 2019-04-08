---
Description: Zone de texte qui fournit une suggestion à mesure que l’utilisateur tape.
title: Zone de liste modifiable (zone de liste déroulante)
label: Combo box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 21a6c698fa0e07587e2c25ae827dc6654a8ced9d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618624"
---
# <a name="combo-box"></a>Combo box

Utiliser une zone de liste déroulante (également appelé une liste déroulante) pour présenter une liste d’éléments à partir de l’utilisateur peut sélectionner. Une zone de liste déroulante démarre dans un état compact et se développe pour afficher une liste d’éléments sélectionnables.

Lorsque la zone de liste déroulante est fermée, il affiche la sélection actuelle, ou est vide s’il n’existe aucun élément sélectionné. Lorsque l’utilisateur développe la zone de liste modifiable, il affiche la liste d’éléments sélectionnables.

> **API importantes** : [Classe de zone de liste déroulante](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [IsEditable propriété](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [propriété Text](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [TextSubmitted événement](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

Une zone de liste déroulante dans son état compact avec un en-tête.

![Exemple de liste déroulante à l’état compact](images/combo_box_collapsed.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

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
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/ComboBox">ouvrez l’application et consultez la zone de liste déroulante en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
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

## <a name="create-a-combo-box"></a>Créer une zone de liste déroulante

Vous remplissez la zone de liste déroulante en ajoutant des objets directement à la [éléments](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) collection ou en liant le [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) propriété à une source de données. Éléments ajoutés à la zone de liste déroulante sont encapsulées dans [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) conteneurs.

Voici une zone de liste déroulante simple avec éléments ajoutés dans XAML.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

L’exemple suivant illustre la liaison d’une zone de liste déroulante à une collection d’objets FontFamily.

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

Tout comme ListView et GridView, zone de liste déroulante est dérivée de [sélecteur](/uwp/api/windows.ui.xaml.controls.primitives.selector), de sorte que vous pouvez obtenir la sélection de l’utilisateur de la même façon standard.

Vous pouvez obtenir ou définir la zone de liste déroulante élément sélectionné à l’aide de la [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) propriété et get ou set l’index de l’élément sélectionné à l’aide de la [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) propriété.

Pour obtenir la valeur d’une propriété particulière sur l’élément de données sélectionnée, vous pouvez utiliser la [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) propriété. Dans ce cas, définissez le [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) pour spécifier quelle propriété sur l’élément sélectionné pour obtenir la valeur à partir de.

> [!TIP]
> Si vous définissez la propriété SelectedItem ou SelectedIndex pour indiquer la sélection par défaut, une exception se produit si la propriété est définie avant que la collection d’éléments de liste déroulante boîte est remplie. Sauf si vous définissez vos éléments dans XAML, il est préférable à l’événement Loaded de zone de liste déroulante et définir SelectedItem ou SelectedIndex dans le Gestionnaire d’événements chargé.

Vous pouvez lier à ces propriétés dans XAML, ou gérer le [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) événements pour répondre aux modifications de sélection.

Dans l’événement code gestionnaire, vous pouvez obtenir l’élément sélectionné à partir de la [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) propriété. Vous pouvez obtenir l’élément précédemment sélectionné (le cas échéant) à partir de la [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) propriété. Les collections AddedItems et RemovedItems contiennent uniquement 1 élément, car la zone de liste déroulante ne prend pas en charge la sélection multiple.

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

Par défaut, l’événement SelectionChanged se produit lorsqu’un utilisateur clique sur, appuie ou appuie sur entrée sur un élément dans la liste pour valider leur sélection, et la zone de liste déroulante se ferme. Sélection ne change pas lorsque l’utilisateur navigue de la liste déroulante ouverte avec les touches de direction du clavier.

Pour accorder à une liste déroulante de zone que « mises à jour automatiques » alors que l’utilisateur navigue ouvrir la liste avec les touches de direction (par exemple, une police sélection déroulante), définissez [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) à [toujours](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger). Cela entraîne l’événement SelectionChanged se produit lorsque le focus est déplacé vers un autre élément dans la liste ouverte.

#### <a name="selected-item-behavior-change"></a>Changement de comportement d’élément sélectionné

Dans Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, le comportement des éléments sélectionnés est mis à jour pour prendre en charge les zones de liste déroulante modifiable.

Avant le Kit de développement logiciel 17763, la valeur de la propriété SelectedItem (et par conséquent, SelectedValue et SelectedIndex) était nécessaire pour se trouver dans la collection d’éléments de la zone de liste modifiable. À l’aide de l’exemple précédent, paramètre `colorComboBox.SelectedItem = "Pink"` entraîne :

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

Dans le Kit de développement logiciel 17763 et versions ultérieur, la valeur de la propriété SelectedItem (et par conséquent, SelectedValue et SelectedIndex) n’est pas requis pour se trouver dans la collection d’éléments de la zone de liste modifiable. À l’aide de l’exemple précédent, paramètre `colorComboBox.SelectedItem = "Pink"` entraîne :

- SelectedItem = rose
- SelectedValue = rose
- SelectedIndex = -1

### <a name="text-search"></a>Recherche en texte

Les zones de liste déroulante prennent automatiquement en charge la recherche au sein de leurs collections. Lorsque les utilisateurs tapent des caractères sur un clavier physique dans une zone de liste déroulante ouverte ou fermée, des candidats correspondant à la chaîne entrée apparaissent. Cette fonctionnalité est particulièrement utile lors de la navigation dans une liste longue. Par exemple, lors de l’interaction avec une liste déroulante contenant une liste d’états, les utilisateurs peuvent appuyer sur la clé « w » à « Washington » afficher pour la sélection rapide. La recherche de texte ne respecte pas la casse.

Vous pouvez définir le [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) propriété **false** pour désactiver cette fonctionnalité.

## <a name="make-a-combo-box-editable"></a>Rendre une zone de liste déroulante modifiable

> [!IMPORTANT]
> Cette fonctionnalité nécessite Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure.

Par défaut, une zone de liste déroulante permet à l’utilisateur sélectionner dans une liste prédéfinie d’options. Toutefois, il existe des cas où la liste contient uniquement un sous-ensemble des valeurs valides, et l’utilisateur doit être en mesure d’entrer d’autres valeurs qui ne sont pas répertoriés. Pour ce faire, vous pouvez rendre la zone de liste déroulante modifiable.

Pour accorder à une zone de liste déroulante modifiable, définissez la [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) propriété **true**. Gérez ensuite le [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) événement fonctionne avec la valeur entrée par l’utilisateur.

Par défaut, la valeur de la propriété SelectedItem est mis à jour lorsque l’utilisateur est validée texte personnalisé. Vous pouvez substituer ce comportement en définissant **Handled** à **true** dans les arguments d’événement TextSubmitted. Lorsque l’événement est marqué comme géré, la zone de liste déroulante ne prendra aucune action supplémentaire après l’événement et est conservée dans l’état de modification. SelectedItem ne sera pas mis à jour.

Cet exemple montre une zone de liste déroulante modifiable simple. La liste contient des chaînes simples, et toute valeur entrée par l’utilisateur est utilisé comme entrée.

Un sélecteur « les noms utilisés récemment » autorise l’utilisateur à entrer des chaînes personnalisées. La liste 'RecentlyUsedNames' contienne des valeurs que l’utilisateur peut choisir à partir de, mais l’utilisateur peut également ajouter une nouvelle valeur personnalisée. La propriété « CurrentName » représente le nom saisi actuellement.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>Texte envoyé

Vous pouvez gérer le [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) événement fonctionne avec la valeur entrée par l’utilisateur. En cas de gestionnaire, vous allez généralement valider que la valeur entrée par l’utilisateur est valide, puis utiliser la valeur dans votre application. Selon la situation, vous pouvez également ajouter la valeur à la liste de la zone de liste déroulante des options pour une utilisation ultérieure.

L’événement TextSubmitted se produit lorsque ces conditions sont remplies :

- La propriété IsEditable est **true**
- L’utilisateur entre du texte qui ne correspond pas à une entrée existante dans la zone de liste modifiable
- L’utilisateur appuie sur entrée, ou déplace le focus à partir de la zone de liste déroulante.

L’événement TextSubmitted n’a pas lieu si l’utilisateur entre du texte, puis navigue vers le haut ou vers le bas dans la liste.

### <a name="sample---validate-input-and-use-locally"></a>Exemple : valider l’entrée et d’utiliser localement

Dans cet exemple, un sélecteur de taille de police contient un ensemble de valeurs correspondant à la base de taille de police, mais l’utilisateur peut entrer des tailles de police qui ne sont pas dans la liste.

Lorsque l’utilisateur ajoute une valeur qui n’est pas dans la liste, les mises à jour de taille de police, mais la valeur n’est pas ajouté à la liste des tailles de police.

Si la valeur récemment entrée n’est pas valide, ce qui que vous permet de revenir à la dernière de la propriété Text SelectedValue connu bonne valeur.

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

### <a name="sample---validate-input-and-add-to-list"></a>Exemple : valider les entrées et ajouter à la liste

Ici, un sélecteur de couleur favorite » » contient les courants favoris des couleurs (rouge, bleu, vert, Orange), mais l’utilisateur peut entrer une couleur préférée n’est pas dans la liste. Lorsque l’utilisateur ajoute une couleur valide (par exemple, rose), la couleur récemment entrée est ajoutée à la liste et définir comme actif « couleur préférée ».

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

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) - Affichez tous les contrôles XAML dans un format interactif.
- [Exemple de AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Articles connexes

- [Contrôles de texte](text-controls.md)
- [Vérification orthographique](text-controls.md)
- [Recherche](search.md)
- [Classe de zone de texte](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Classe de Windows.UI.Xaml.Controls PasswordBox](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length, propriété](https://msdn.microsoft.com/library/system.string.length.aspx)
