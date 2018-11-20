---
author: QuinnRadich
Description: Radio buttons let users select one option from two or more choices.
title: Recommandations en matière de cases d’option
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 577e4ca0716427298344ac2eec5155c786d5530c
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7305191"
---
# <a name="radio-buttons"></a>Cases d’option

> **API importantes**: [classe RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [événement Checked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked), [propriété IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

Les cases d’option permettent aux utilisateurs de sélectionner une option dans un ensemble. Chaque option est représentée par une seule case d’option et les utilisateurs peuvent activer une seule case d’option dans un groupe de cases d’option.

(Si le nom vous intrigue, sachez que les cases d’option s’apparentent à des boutons de radio.)

![Cases d’option](images/controls/radio-button.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez les cases d’option pour présenter à l’utilisateur au moins deux options qui s’excluent mutuellement.

![Groupe de cases d’option](images/radiobutton_basic.png)

Utilisez les cases d’option lorsque les utilisateurs ont besoin de voir toutes les options pour effectuer une sélection. Comme les cases d’option mettent en avant les options de façon identique, elles peuvent donner plus d’importance à une option qu’elle n’en a vraiment. À moins que les options ne doivent attirer davantage l’attention de l’utilisateur, envisagez d’utiliser d’autres contrôles. Par exemple, si l’option par défaut est recommandée pour la plupart des utilisateurs et dans la plupart des situations, utilisez plutôt une [liste déroulante](lists.md).

![liste déroulante](images/combo_box_collapsed.png)

S’il existe uniquement deux options qui s’excluent mutuellement, combinez-les dans une [case à cocher](checkbox.md) ou un [bouton bascule](toggles.md) unique. Par exemple, utilisez une case à cocher pour « J’accepte », plutôt que deux cases d’option pour « J’accepte » et « Je n’accepte pas ».

![Deux façons de présenter un choix binaire](images/radiobutton_vs_checkbox.png)

Si l’utilisateur peut sélectionner plusieurs options, utilisez une [case à cocher](checkbox.md).

![Sélection de plusieurs options à l’aide de cases à cocher](images/checkbox2.png)

Lorsque les options sont des numéros incrémentés de manière fixe (10, 20, 30), utilisez un contrôle [curseur](slider.md).

![contrôle de curseur](images/controls/slider.png)

S’il existe plus de huit options, utilisez une [liste déroulante](lists.md) ou une [zone de liste](lists.md).

![zone de liste modifiable](images/combo_box_scroll.png)

Si les options disponibles dépendent du contexte actuel de l’application, ou sont amenées à changer de manière dynamique, utilisez une [zone de liste](lists.md) à sélection unique.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/RadioButton">ouvrir l’application et voir l'objet RadioButton en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Cases d’option dans les paramètres du navigateur MicrosoftEdge.

![Cases d’option dans les paramètres du navigateur MicrosoftEdge](images/control-examples/radio-buttons-edge.png)

## <a name="create-a-radio-button"></a>Créer une case d’option

Les cases d’option s’utilisent en groupes. Les 2méthodes permettant de grouper des contrôles de cases d’option sont les suivantes:
- Placez-les dans le même conteneur parent.
- Définissez la propriété [GroupName](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) de chaque case d’option sur la même valeur.

Dans cet exemple, le premier groupe de cases d’option est implicitement formé en raison de son appartenance au même panneau d’empilement. Le second groupe est divisé entre 2 panneaux d’empilement, si bien que les cases d’option sont explicitement regroupées par GroupName.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

La case d’option ressemble à ceci.

![Cases d’option en deux groupes](images/radio-button-groups.png)

Une case d’option a deux états: *sélectionnée* ou *désactivée*. Lorsqu’une case d’option est sélectionnée, sa propriété [IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) vaut **true**. Lorsqu’une case d’option est désactivée, sa propriété **IsChecked** vaut **false**. Une case d’option peut être désactivée en cliquant sur une autre case d’option dans le même groupe. Elle ne peut pas être désactivée en cliquant à nouveau dessus. Toutefois, vous pouvez désactiver une case d’option par programme en définissant sa propriété IsChecked sur **false**. Vous pouvez effectivement comparer la propriété **IsChecked** avec une valeur booléenne en obtenant la **Valeur** de la propriété **IsChecked**

## <a name="recommendations"></a>Recommandations

-   Assurez-vous que le but et l’état actuel d’un ensemble de cases d’option sont clairs.
-   Limitez le contenu du texte de la case d’option à une seule ligne.
-   Si le contenu du texte est dynamique, songez au redimensionnement du bouton et à ses conséquences sur les effets visuels environnants.
-   Utilisez la police par défaut à moins que vos instructions de personnalisation n’imposent d’en utiliser une autre.
-   Ne placez pas deux groupes de cases d’option côte à côte. Lorsque deux groupes de cases d’option sont adjacents, il est difficile de déterminer quelles cases appartiennent à quel groupe.

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires

Cette illustration montre la manière convenable de positionner et espacer les cases d’option.

![Ensemble de cases d’option](images/radiobutton-layout.png)

![recommandations d'espacement en matière de cases d’option](images/radiobutton-redlines.png)

## <a name="related-topics"></a>Rubriquesassociées

**Pour les concepteurs**
- [Boutons](buttons.md)
- [Boutons bascule](toggles.md)
- [Cases à cocher](checkbox.md)
- [Listes et zones de liste modifiable](lists.md)
- [Curseurs](slider.md)

**Pour les développeurs (XAML)**
- [Classe RadioButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
