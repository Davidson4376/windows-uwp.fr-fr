---
Description: A color picker lets a user browse through and select colors.
title: Sélecteur de couleurs
label: Color Picker
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ea3cfc10eb221ae0de8429cf299a4ca5b4d53716
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8209414"
---
# <a name="color-picker"></a>Sélecteur de couleurs

Un sélecteur de couleur permet de parcourir et sélectionner des couleurs. Par défaut, il permet à un utilisateur de parcourir les couleurs sur une gamme de couleurs, ou de spécifier une couleur dans les zones de saisie RVB (Rouge, Vert, Bleu), HSV (Valeur de saturation de la teinte) ou Hexadécimal.

> **API importantes**: [classe ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker), [propriété Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color), [événement ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged)

![Un sélecteur de couleur par défaut](images/color-picker-default.png)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez le sélecteur de couleurs pour permettre à un utilisateur de sélectionner des couleurs dans votre application. Par exemple, utilisez-le pour modifier les paramètres de couleur, telles que les couleurs de police, d’arrière-plan ou de thème de l’application.

Si votre application est destinée au dessin ou à des tâches similaires à l’aide du stylet, envisagez d’utiliser les [contrôles pour l’entrée manuscrite](inking-controls.md) en plus du sélecteur de couleurs.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/ColorPicker">ouvrir l’application et voir l'objet ColorPicker en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-color-picker"></a>Créer un sélecteur de couleurs

Cet exemple montre comment créer un sélecteur de couleurs par défaut en XAML.

```xaml
<ColorPicker x:Name="myColorPicker"/>
```

Par défaut, le sélecteur de couleurs affiche un aperçu de la couleur choisie dans la barre rectangulaire située en regard de la gamme de couleurs. Vous pouvez utiliser l’événement [ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged) ou la propriété [Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color) pour accéder à la couleur sélectionnée et l’utiliser dans votre application. Consultez les exemples de code détaillés suivants.

### <a name="bind-to-the-chosen-color"></a>Lier à la couleur choisie

Lorsque la sélection de couleur doit prendre effet immédiatement, vous pouvez utiliser la liaison de données pour lier la propriété Color, ou gérer l’événement ColorChanged pour accéder à la couleur sélectionnée dans votre code.

Dans cet exemple, vous liez la propriété Color de l’objet SolidColorBrush, utilisée comme propriété Fill d’objet Rectangle, directement à la couleur sélectionnée dans le sélecteur de couleurs. Toute modification apportée au sélecteur de couleurs entraîne un changement dynamique de la propriété liée.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape=”Ring”
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>

<Rectangle Height="50" Width="50">
    <Rectangle.Fill>
        <SolidColorBrush Color="{x:Bind myColorPicker.Color, Mode=OneWay}"/>
    </Rectangle.Fill>
</Rectangle>
```

Cet exemple utilise un sélecteur de couleurs simplifié, composé uniquement du cercle et du curseur, offrant une expérience de sélection de couleur «informelle» courante. Lorsque la modification de couleur se produit et apparaît en temps réel sur l’objet affecté, vous n’avez pas besoin d’afficher la barre d’aperçu de couleur. Consultez la section *Personnaliser le sélecteur de couleurs* pour en savoir plus.

### <a name="save-the-chosen-color"></a>Enregistrer la couleur choisie

Il peut arriver que vous ne souhaitiez pas appliquer immédiatement la modification de couleur. Par exemple, lorsque vous hébergez un sélecteur de couleur dans un menu volant, nous vous recommandons d’appliquer la couleur sélectionnée uniquement après que l’utilisateur a confirmé la sélection ou fermé le menu volant. Vous pouvez également enregistrer la valeur de couleur sélectionnée pour une utilisation ultérieure.

Dans cet exemple, vous hébergez un sélecteur de couleur dans un menu volant, avec boutons Confirmer et Annuler. Lorsque l’utilisateur confirme son choix de couleur, vous enregistrez la couleur sélectionnée pour une utilisation ultérieure dans votre application.

```xaml
<Page.Resources>
    <Flyout x:Key="myColorPickerFlyout">
        <RelativePanel>
            <ColorPicker x:Name="myColorPicker"
                         IsColorChannelTextInputVisible="False"
                         IsHexInputVisible="False"/>

            <Grid RelativePanel.Below="myColorPicker"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Content="OK" Click="confirmColor_Click"
                        Margin="0,12,2,0" HorizontalAlignment="Stretch"/>
                <Button Content="Cancel" Click="cancelColor_Click"
                        Margin="2,12,0,0" HorizontalAlignment="Stretch"
                        Grid.Column="1"/>
            </Grid>
        </RelativePanel>
    </Flyout>
</Page.Resources>

<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Button x:Name="colorPickerButton"
            Content="Pick a color"
            Flyout="{StaticResource myColorPickerFlyout}"/>
</Grid>
```

```csharp
private Color mycolor;

private void confirmColor_Click(object sender, RoutedEventArgs e)
{
    // Assign the selected color to a variable to use outside the popup.
    myColor = myColorPicker.Color;

    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}

private void cancelColor_Click(object sender, RoutedEventArgs e)
{
    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}
```

### <a name="configure-the-color-picker"></a>Configurer le sélecteur de couleurs

Le sélecteur de couleurs est flexible car tous les champs ne sont pas indispensables pour permettre à un utilisateur de choisir une couleur. Il offre différentes options qui vous permettent de configurer le contrôle pour répondre à vos besoins.

Par exemple, lorsque l’utilisateur n’a pas besoin d’un contrôle précis, notamment pour sélectionner une couleur de mise en surbrillance dans une application de prise de notes, vous pouvez utiliser une interface utilisateur simplifiée. Vous pouvez masquer les champs de saisie de texte et afficher la gamme des couleurs sous forme de cercle.

Lorsque l’utilisateur a besoin d’un contrôle précis, notamment dans une application de conception graphique, vous pouvez afficher à la fois les curseurs et les champs de saisie de texte pour chaque aspect de la couleur.

#### <a name="show-the-circle-spectrum"></a>Afficher la gamme sous forme de cercle

Cet exemple montre comment utiliser la propriété [ColorSpectrumShape](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorSpectrumShape) pour configurer le sélecteur de couleurs afin qu’il affiche la gamme de couleurs sous forme de cercle, en remplacement du carré par défaut.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"/>
```

![Un sélecteur de couleurs sous forme de cercle](images/color-picker-ring.png)

Lorsque vous devez choisir entre le carré et le cercle pour afficher la gamme de couleurs, la principale considération concerne la précision. Un utilisateur a plus de contrôle lorsqu’il sélectionne une couleur spécifique à l’aide d’un carré car celui-ci affiche une plus large gamme de couleurs. Vous devez considérer l’affichage sous forme de cercle comme une expérience de sélection de couleur plus «informelle».

#### <a name="show-the-alpha-channel"></a>Afficher le canal alpha

Dans cet exemple, vous intégrez un curseur d’opacité et une zone de texte sur le sélecteur de couleurs.

```xaml
<ColorPicker x:Name="myColorPicker"
             IsAlphaEnabled="True"/>
```

![Un sélecteur de couleur avec propriété IsAlphaEnabled définie sur true](images/color-picker-alpha.png)

#### <a name="show-a-simple-picker"></a>Afficher un sélecteur simple

Cet exemple montre comment configurer le sélecteur de couleurs avec une interface utilisateur simple pour une utilisation «informelle». Vous affichez la gamme de couleur sous forme de cercle et masquez les zones de saisie de texte par défaut. Lorsque la modification de couleur se produit et apparaît en temps réel sur l’objet affecté, vous n’avez pas besoin d’afficher la barre d’aperçu de couleur. Sinon, vous devez laisser l’aperçu de couleur visible.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>
```

![Un sélecteur de couleurs simple](images/color-picker-casual.png)

#### <a name="show-or-hide-additional-features"></a>Afficher ou masquer des fonctionnalités supplémentaires

Ce tableau montre toutes les options à votre disposition pour configurer le contrôle ColorPicker.

Fonctionnalité | Propriétés
--------|-----------
Gamme de couleurs | IsColorSpectrumVisible, ColorSpectrumShape, ColorSpectrumComponents
Aperçu de couleur | IsColorPreviewVisible
Valeurs de couleur| IsColorSliderVisible, IsColorChannelTextInputVisible
Valeurs d’opacité | IsAlphaEnabled, IsAlphaSliderVisible, IsAlphaTextInputVisible
Valeurs hexadécimales | IsHexInputVisible

> [!NOTE]
> IsAlphaEnabled doit être défini sur **true** pour afficher la zone de saisie et le curseur d’opacité. Il est ensuite possible de modifier la visibilité des contrôles d’entrée à l’aide des propriétés IsAlphaTextInputVisible et IsAlphaSliderVisible. Consultez la documentation sur les API pour en savoir plus.

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Pensez au type d’expérience de sélection de couleur le mieux adapté à votre application. Certains scénarios peuvent ne pas nécessiter une sélection de couleur granulaire et bénéficieraient d’un sélecteur simplifié
- Pour offrir l’expérience de sélection de couleur la plus précise, utilisez la gamme de forme carrée et assurez-vous que sa taille est d’au moins 256x256px, ou intégrez des champs de saisie de texte pour permettre aux utilisateurs d’affiner leur couleur sélectionnée.
- Dans le cas d’un menu volant, le fait d’appuyer sur la gamme de couleurs ou d’ajuster le curseur ne suffit pas pour valider la sélection de couleur. Pour valider la sélection:
  - Fournissez les boutons de validation et d’annulation pour appliquer ou annuler la sélection. Le fait d’appuyer sur le bouton Précédent ou d’appuyer en dehors du menu volant a pour effet de le fermer et de ne pas enregistrer la sélection de l’utilisateur.
  - Sinon, validez la sélection lors de la fermeture du menu volant, en appuyant en dehors du menu volant ou en appuyant sur le bouton Précédent.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Interactions avec le stylo et le stylet dans les applicationsUWP](../input/pen-and-stylus-interactions.md)
- [Entrée manuscrite](inking-controls.md)

<!--
<div class=”microsoft-internal-note”>
<p>
<p>
Note: For more info, see the [color picker redlines](http://uni/DesignDepot.FrontEnd/#/ProductNav/3666/15/dv/?t=Windows%7CControls&f=RS2) on UNI.
</div>
-->