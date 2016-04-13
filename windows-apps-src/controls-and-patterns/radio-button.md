---
Les cases d’option permettent aux utilisateurs de faire un choix parmi au moins deux possibilités.
Recommandations en matière de cases d’option
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
Cases d’option
template: detail.hbs
---
# Cases d’option
Les cases d’option permettent aux utilisateurs de faire un choix parmi au moins deux possibilités. Chaque option est représentée par une seule case d’option ; un utilisateur peut activer une seule case d’option dans un groupe de cases d’option.

(Si le nom vous intrigue, sachez que les cases d’option s’apparentent à des boutons de radio.)

![Cases d’option](images/controls/radio-button.png)

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Classe RadioButton**](https://msdn.microsoft.com/library/windows/apps/br227544)
-   [**Événement Checked**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx)
-   [**Propriété IsChecked**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx)

## Est-ce le contrôle approprié ?

Utilisez les cases d’option pour présenter à l’utilisateur au moins deux options qui s’excluent mutuellement, comme ci-après.

![Groupe de cases d’option](images/radiobutton_basic.png)

Les cases d’option confèrent plus de clarté et de visibilité aux options les plus importantes de votre application. Utilisez les cases d’option lorsque les options présentées sont suffisamment importantes pour justifier une plus grande occupation de l’espace à l’écran et quand la clarté du choix exige des options très explicites.

Les cases d’option mettent en avant les options de façon identique, ce qui peut donner plus d’importance à une option qu’elle n’en a vraiment. Envisagez d’utiliser d’autres contrôles, à moins que les options ne doivent attirer davantage l’attention de l’utilisateur. Par exemple, si l’option par défaut est recommandée pour la plupart des utilisateurs et dans la plupart des situations, utilisez plutôt une [liste déroulante](lists.md).

S’il existe uniquement deux options qui s’excluent mutuellement, combinez-les dans une [case à cocher](checkbox.md) ou un [bouton bascule](toggles.md) unique. Par exemple, utilisez une case à cocher pour « J’accepte », plutôt que deux cases d’option pour « J’accepte » et « Je n’accepte pas ».

![Deux façons de présenter un choix binaire](images/radiobutton_vs_checkbox.png)

Si l’utilisateur peut sélectionner plusieurs options, utilisez plutôt une [case à cocher](checkbox.md) ou une [zone de liste](lists.md).

![Sélection de plusieurs options à l’aide de cases à cocher](images/checkbox2.png)

N’utilisez pas de cases d’option lorsque les options sont des nombres incrémentés de manière fixe, comme 10, 20, 30. Dans ce cas, utilisez un contrôle de type [curseur](slider.md).

S’il existe plus de huit options, utilisez une [liste déroulante](lists.md), une [zone de liste](lists.md) à sélection unique, ou une [zone de liste](lists.md).

Si les options disponibles dépendent du contexte actuel de l’application, ou sont amenées à changer de manière dynamique, utilisez une [zone de liste](lists.md) à sélection unique.

## Exemple
Cases d’option dans les paramètres du navigateur Microsoft Edge.

![Cases d’option dans les paramètres du navigateur Microsoft Edge](images/control-examples/radio-buttons-edge.png)

## Créer une case d’option

Les cases d’option s’utilisent en groupes. Les 2 méthodes permettant de grouper des contrôles de cases d’option sont les suivantes :
- Placez-les dans le même conteneur parent.
- Définissez la propriété [**GroupName**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.radiobutton.groupname.aspx) de chaque case d’option sur la même valeur.

> **Remarque**&nbsp;&nbsp;Un groupe de cases d’option se comporte comme un seul contrôle quand l’utilisateur y accède via le clavier. Seule l’option sélectionnée est accessible à l’aide de la touche Tabulation, mais les utilisateurs peuvent passer d’une option à l’autre dans le groupe à l’aide des touches de direction.

Dans cet exemple, le premier groupe de cases d’option est implicitement formé en raison de son appartenance au même panneau d’empilement. Le second groupe est divisé entre 2 panneaux d’empilement, si bien que les cases d’option sont explicitement groupées par GroupName.

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

Les groupes de cases d’option ressemblent à ceci.

![Cases d’option en deux groupes](images/radio-button-groups.png)

Une case d’option a deux états : *sélectionnée* ou *désactivée*. Lorsqu’une case d’option est sélectionnée, sa propriété [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx) vaut **true**. Lorsqu’une case d’option est désactivée, sa propriété **IsChecked** vaut **false**. Une case d’option peut être désactivée en cliquant sur une autre case d’option dans le même groupe. Elle ne peut pas être désactivée en cliquant à nouveau dessus. Toutefois, vous pouvez désactiver une case d’option par programme en définissant sa propriété IsChecked sur **false**.

## Recommandations

-   Assurez-vous que le but et l’état actuel d’un ensemble de cases d’option sont clairs.
-   Fournissez toujours un retour visuel quand l’utilisateur appuie sur une case d’option.
-   Fournissez toujours un retour visuel quand l’utilisateur interagit avec une case d’option. Les exemples d’états d’une case d’option sont : normal, enfoncé, activé ou désactivé. L’utilisateur appuie sur une case d’option pour activer l’option correspondante. Le fait d’appuyer sur une option activée ne la désactive pas, mais le fait d’appuyer sur une autre option active celle-ci.
-   Réservez les animations et les effets visuels au retour tactile et à l’état activé. Dans l’état non activé, les contrôles de cases d’option non activés doivent apparaître inutilisés ou inactifs (mais pas désactivés).
-   Limitez le contenu du texte de la case d’option à une seule ligne. Vous pouvez personnaliser les effets visuels d’une case d’option pour afficher une description de l’option dans une taille inférieure de police apparaissant sous la ligne principale de texte.
-   Si le contenu du texte est dynamique, songez au redimensionnement du bouton et à ses conséquences sur les effets visuels environnant.
-   Utilisez la police par défaut à moins que vos instructions de personnalisation imposent d’en utiliser une autre.
-   Entourez la case d’option d’un élément d’étiquette de sorte que l’appui sur l’étiquette entraîne la sélection de la case d’option.
-   Placez le texte de l’étiquette après le contrôle de case d’option, pas avant ni au-dessus de celle-ci.
-   Personnalisez vos cases d’option. Par défaut, une case d’option se compose de deux cercles concentriques (le cercle intérieur est rempli et affiché quand la case d’option est activée, le cercle extérieur est un trait) et de contenu textuel. Mais, nous vous encourageons à faire preuve de créativité. Les utilisateurs aiment bien interagir directement avec le contenu d’une application. Vous pouvez donc choisir d’afficher le contenu réel à l’aide de graphismes ou sous la forme de subtils boutons bascule textuels.
-   Ne placez pas plus de huit options dans un groupe de cases d’option. Si vous voulez en présenter davantage, utilisez plutôt une [liste déroulante](lists.md), une [zone de liste](lists.md) ou un [affichage sous forme de liste](lists.md).
-   Ne placez pas deux groupes de cases d’option en regard l’un de l’autre. Lorsque deux groupes de cases d’option sont adjacents, il est difficile de déterminer quelles cases appartiennent à quel groupe. Utilisez des étiquettes de groupe pour les distinguer.

## Indications d’utilisation supplémentaires

Cette illustration montre la manière convenable de positionner et espacer les cases d’option.

![Ensemble de cases d’option](images/radiobutton_layout1.png)
## Rubriques connexes

**Pour les concepteurs**
- [Recommandations en matière de boutons](buttons.md)
- [Recommandations en matière de boutons bascule](toggles.md)
- [Recommandations en matière de cases à cocher](checkbox.md)
- [Recommandations en matière de listes déroulantes](lists.md)
- [Recommandations en matière de contrôles d’affichages de liste et d’affichages de grille](lists.md)
- [Recommandations en matière de curseurs](slider.md)
- [Recommandations en matière de contrôle de sélection](lists.md)


**Pour les développeurs (XAML)**
- [**Classe Windows.UI.Xaml.Controls RadioButton**](https://msdn.microsoft.com/library/windows/apps/br227544)


<!--HONumber=Mar16_HO1-->


