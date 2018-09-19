---
author: QuinnRadich
description: Découvrez comment utiliser des couleurs d’accentuation et des thèmes dans vos applications UWP.
title: Couleur dans les applications UWP
ms.author: quradic
ms.date: 4/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.openlocfilehash: 19f4d9cde6ee2bc9615f044f18bc5e8828ca1985
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4052781"
---
# <a name="color"></a>Couleur

![image hero](images/header-color.svg)

La couleur fournit un moyen intuitif de communiquer des informations aux utilisateurs de votre application: elle peut être utilisée pour indiquer l’interactivité, fournir des commentaires sur les actions de l’utilisateur et procurer à votre interface une certaine continuité visuelle. 

Dans les applications UWP, les couleurs sont principalement déterminées par la couleur d’accentuation et le thème. Dans cet article, nous expliquerons comment utiliser la couleur dans votre application et comment utiliser les ressources de couleur d’accentuation et de thème pour rendre votre application UWP utilisable dans n’importe quel contexte de thème. 

## <a name="color-principles"></a>Principes sur les couleurs

:::row:::
    :::column:::
        **Utilisez couleur de manière intelligente.**
Lorsque la couleur est utilisée avec parcimonie pour mettre en évidence des éléments importants, elle permet de créer une interface utilisateur fluide et intuitive.
    :::column-end:::
    :::column:::
        **Utilisez la couleur pour indiquer l’interactivité.**
Il est recommandé de choisir une couleur pour indiquer les éléments interactifs de votre application. Par exemple, la plupart des pages web utilisent du texte en bleu pour représenter un lien hypertexte.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **La couleur est personnalisable.**
Dans Windows, les utilisateurs peuvent choisir une couleur d’accentuation et un thème clair ou foncé, qui sont conservés tout au long de leur expérience. Vous pouvez choisir comment incorporer la couleur d’accentuation et le thème de l’utilisateur dans votre application pour personnaliser son expérience.
    :::column-end:::
    :::column:::
        **Couleur est culturelle.**
Prenez en compte la façon dont les couleurs utilisées seront interprétées par des personnes de différentes cultures. Par exemple, dans certaines cultures, la couleur bleue est associée à la vertu et la protection, tandis que dans d’autres cultures, elle représente le deuil.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>Thèmes

Les applications UWP peuvent utiliser un thème d’application clair ou foncé. Le thème affecte les couleurs de l’arrière-plan, du texte, des icônes et des [contrôles communs](../controls-and-patterns/index.md) de l’application.

### <a name="light-theme"></a>Thème clair

![thème clair](images/color/light-theme.svg)

### <a name="dark-theme"></a>Thème foncé

![thème foncé](images/color/dark-theme.svg)

Par défaut, le thème de votre application UWP est la préférence de thème de l’utilisateur définie dans les paramètres Windows, ou le thème par défaut de l’appareil (c'est-à-dire, foncé sur XBox). Toutefois, vous pouvez définir le thème de votre application UWP. 

### <a name="changing-the-theme"></a>Modification du thème

Vous pouvez changer de thèmes en modifiant la propriété **RequestedTheme** dans votre fichier `App.xaml`.

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

La suppression de la propriété **RequestedTheme** signifie que votre application utilise les paramètres système de l’utilisateur.

Les utilisateurs peuvent également sélectionner le thème à contraste élevé, qui utilise une petite palette de couleurs contrastées pour faciliter l’affichage de l’interface. Dans ce cas, le système remplace votre propriété RequestedTheme.

### <a name="testing-themes"></a>Test des thèmes

Si vous ne demandez pas un thème pour votre application, veillez à tester votre application dans les thèmes clair et foncé pour vérifier qu'elle est lisible dans toutes les conditions.

**Remarque**: dans Visual Studio, la valeur RequestedTheme par défaut est Light, vous devrez donc modifier la valeur RequestedTheme pour tester les deux thèmes.

## <a name="theme-brushes"></a>Pinceaux de thème

Les contrôles communs utilisent automatiquement des [pinceaux de thème](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes) pour ajuster le contraste des thèmes clair et foncé.

Par exemple, voici une illustration de la façon dont [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) utilise les pinceaux de thème:

![exemple de contrôle pour les pinceaux de thème](images/color/theme-brushes.svg)

Les pinceaux de thème sont utilisés aux fins suivantes:

- **Base** est utilisé pour le texte.
- **Alt** est l’inverse de Base.
- **Chrome** est utilisé pour les éléments de niveau supérieur, comme les volets de navigation ou les barres de commandes.
- **List** est utilisé pour les contrôles de liste.

**Low**/**Medium**/**High** font référence à l’intensité de la couleur.

### <a name="using-theme-brushes"></a>Utilisation des pinceaux de thème

:::row:::
    :::column:::
        Lors de la création de modèles de contrôles personnalisés, utilisez les pinceaux de thème au lieu des valeurs de couleur de coder en dur. De cette façon, votre application peut facilement s’adapter à n’importe quel thème.

        For example, these [item templates for ListView](../controls-and-patterns/item-templates-listview.md) demonstrate how to use theme brushes in a custom template.
    :::column-end:::
    :::column:::
         ![double line list item with icon example](images/color/list-view.svg)
    :::column-end:::
:::row-end:::

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Pour plus d’informations sur l’utilisation des pinceaux de thème dans votre application, voir [Ressources de thème](../controls-and-patterns/xaml-theme-resources.md).

## <a name="accent-color"></a>Couleur d’accentuation

Les contrôles communs utilisent une couleur d’accentuation pour transmettre les informations d’état. Par défaut, la couleur d’accentuation est la valeur `SystemAccentColor` sélectionnée par les utilisateurs dans leurs paramètres. Toutefois, vous pouvez également personnaliser la couleur d’accentuation de votre application pour refléter votre marque.

![contrôles Windows](images/color/windows-controls.svg)

:::row:::
    :::column:::
        ![en-tête d’accentuation sélectionné par l’utilisateur](images/color/user-accent.svg) ![couleur d’accentuation sélectionné par l’utilisateur](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![en-tête d’accentuation personnalisé](images/color/custom-accent.svg) ![couleur d’accentuation personnalisée](images/color/brand-color.svg)
    :::column-end:::
:::row-end:::

### <a name="overriding-the-accent-color"></a>Remplacement de la couleur d’accentuation

Pour modifier la couleur d’accentuation de votre application, placez le code suivant dans `app.xaml`.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <Color x:Key="SystemAccentColor">#107C10</Color>
    </ResourceDictionary>
</Application.Resources>
```

### <a name="choosing-an-accent-color"></a>Choix d’une couleur d’accentuation

Si vous sélectionnez une couleur d’accentuation personnalisée pour votre application, vérifiez que le texte et les arrière-plans qui utilisent la couleur d’accentuation ont un contraste suffisant pour assurer une lisibilité optimale. Pour tester le contraste, vous pouvez utiliser l’outil Sélecteur de couleurs dans les paramètres Windows, ou vous pouvez utiliser ces [outils de contraste en ligne](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

![Sélecteur de couleurs d’accentuation personnalisées dans les paramètres Windows](images/color/color-picker.svg)

## <a name="accent-color-palette"></a>Palette de couleurs d’accentuation

Un algorithme de couleur d’accentuation dans le shell Windows génère des nuances claires et foncées de la couleur d’accentuation.

![palette de couleurs d’accentuation](images/color/accent-color-palette.svg)

Ces nuances sont accessibles en tant que [ressources de thème](../controls-and-patterns/xaml-theme-resources.md):

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true -->
Vous pouvez également accéder à la palette de couleurs d’accentuation par programme avec la méthode [**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) et l’énumération [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType).

Vous pouvez utiliser la palette de couleurs d’accentuation pour les thèmes de couleur de votre application. Voici un exemple de l’utilisation de la palette de couleurs d’accentuation sur un bouton.

![Palette de couleurs d’accentuation appliquée à un bouton](images/color/color-theme-button.svg)

```xaml
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ButtonBackground" Color="{ThemeResource SystemAccentColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver" Color="{ThemeResource SystemAccentColorLight1}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed" Color="{ThemeResource SystemAccentColorDark1}"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>

<Button Content="Button"></Button>
```

Lorsque vous utilisez du texte en couleur sur un arrière-plan en couleur, vérifiez que le contraste est suffisant entre le texte et l’arrière-plan. Par défaut, le lien hypertexte utilise la couleur d’accentuation. Si vous appliquez des variantes de la couleur d’accentuation à l’arrière-plan, vous devez utiliser une variante de la couleur d’accentuation d’origine pour optimiser le contraste du texte en couleur sur un arrière-plan en couleur.

Le graphique ci-dessous illustre un exemple des différentes nuances claires et foncées de la couleur d’accentuation. Il décrit également comment le type peut être appliqué sur une surface en couleur.

![couleur sur couleur](images/color/color-on-color.svg)

Pour plus d’informations sur l’application de styles aux contrôles, voir [Styles XAML](../controls-and-patterns/xaml-styles.md).

## <a name="color-api"></a>API de couleur

Plusieurs API peuvent être utilisées pour ajouter de la couleur à votre application. Tout d’abord, la classe [**Colors**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors), qui implémente une longue liste de couleurs prédéfinies. Celles-ci sont accessibles automatiquement avec les propriétés XAML. Dans l’exemple ci-dessous, nous créons un bouton et définissons les propriétés de couleur en arrière-plan et au premier plan pour les membres de la classe **Colors**.

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

Vous pouvez créer vos propres couleurs à partir des valeurs RVB ou hexadécimales en utilisant la structure [**Color**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.color) en XAML.

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

Vous pouvez également créer la même couleur dans le code à l’aide de la méthode **FromArgb**.

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

Les lettres «Arvb» signifient Alpha (opacité), Rouge, Vert et Bleu, qui sont les quatre composants d’une couleur. Chaque argument peut être compris entre 0 et 255. Vous pouvez choisir d’omettre la première valeur, ce qui vous donne une opacité par défaut de 255 ou 100% opaque.

> [!Note]
> Si vous utilisez C++, vous devez créer des couleurs à l’aide de la classe [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper).

Un objet **Color** est le plus souvent utilisé comme argument d’un objet [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush), qui peut être utilisé pour peindre les éléments de l’interface utilisateur dans une seule couleur unie. Ces pinceaux sont généralement définis dans un objet [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary): ils peuvent donc être réutilisés pour plusieurs éléments.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

Pour plus d’informations sur l’utilisation des pinceaux, voir [Pinceaux XAML](brushes.md).

## <a name="usability"></a>Facilité d’utilisation

:::row:::
    :::column:::
        ![illustration du contraste élevé](images/color/illo-contrast.svg)
    :::column-end:::
    ::: column span = «2»::: **à contraste élevé**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![illustration du contraste élevé](images/color/illo-lighting.svg)
    :::column-end:::
    ::: column span = «2»::: **éclairage**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![illustration du contraste élevé](images/color/illo-colorblindness.svg)
    :::column-end:::
    ::: column span = «2»::: **daltonisme**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Articles associés

- [Styles XAML](../controls-and-patterns/xaml-styles.md)
- [Ressources de thème XAML](../controls-and-patterns/xaml-theme-resources.md)