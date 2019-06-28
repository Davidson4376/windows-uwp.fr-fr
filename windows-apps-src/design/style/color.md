---
description: Découvrez comment utiliser des couleurs d’accentuation et des thèmes dans vos applications UWP.
title: Couleur dans les applications UWP
ms.date: 04/7/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3177deb74085737531366f63e9f2e8bbecac06e6
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67028473"
---
# <a name="color"></a>Color

![Image Hero](images/header-color.svg)

La couleur fournit un moyen intuitif de communiquer des informations aux utilisateurs de votre application : elle peut être utilisée pour indiquer une interactivité, donner un feedback aux actions de l’utilisateur et procurer à votre interface une certaine continuité visuelle.

Dans les applications UWP, les couleurs sont principalement déterminées par la couleur d’accentuation et le thème. Dans cet article, nous expliquons comment utiliser la couleur dans votre application, et comment utiliser les ressources de couleur d’accentuation et de thème pour rendre votre application UWP utilisable dans n’importe quel contexte de thème.

## <a name="color-principles"></a>Principes des couleurs

:::row:::
    :::column:::
        **Use color meaningfully.**
        When color is used sparingly to highlight important elements, it can help create a user interface that is fluid and intuitive.
    :::column-end:::
    :::column:::
        **Use color to indicate interactivity.**
        It's a good idea to choose one color to indicate elements of your application that are interactive. For example, many web pages use blue text to denote a hyperlink.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Color is personal.**
        In Windows, users can choose an accent color and a light or dark theme, which are reflected throughout their experience. You can choose how to incorporate the user's accent color and theme into your application, personalizing their experience.
    :::column-end:::
    :::column:::
        **Color is cultural.**
        Consider how the colors you use will be interpreted by people from different cultures. For example, in some cultures the color blue is associated with virtue and protection, while in others it represents mourning.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>Thèmes

Les applications UWP peuvent utiliser un thème d’application clair ou foncé. Le thème affecte les couleurs de l’arrière-plan, du texte, des icônes et des [contrôles courants](../controls-and-patterns/index.md) de l’application.

### <a name="light-theme"></a>Thème clair

![thème clair](images/color/light-theme.svg)

### <a name="dark-theme"></a>Thème foncé

![thème foncé](images/color/dark-theme.svg)

Par défaut, le thème de votre application UWP est la préférence de thème de l’utilisateur définie dans les paramètres Windows, ou le thème par défaut de l’appareil (c’est-à-dire foncé sur XBox). Vous pouvez cependant définir le thème de votre application UWP.

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

Les utilisateurs peuvent également sélectionner le thème à contraste élevé, qui utilise une petite palette de couleurs contrastées pour faciliter la visualisation de l’interface. Dans ce cas, le système remplace votre propriété RequestedTheme.

### <a name="testing-themes"></a>Test des thèmes

Si vous ne demandez pas un thème pour votre application, veillez à tester votre application dans les thèmes clair et foncé pour vérifier qu’elle est lisible dans toutes les conditions.

**Remarque** : Dans Visual Studio, la valeur par défaut de RequestedTheme est Light (Clair) : vous devrez donc modifier la valeur de RequestedTheme pour tester les deux thèmes.

## <a name="theme-brushes"></a>Pinceaux de thème

Les contrôles courants utilisent automatiquement des [pinceaux de thème](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes) pour ajuster le contraste des thèmes clair et foncé.

Par exemple, voici une illustration de la façon dont [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) utilise les pinceaux de thème :

![exemple de contrôle avec des pinceaux de thème](images/color/theme-brushes.svg)

Les pinceaux de thème sont utilisés aux fins suivantes :

- **Base** est utilisé pour le texte.
- **Alt** est l’inverse de Base.
- **Chrome** est utilisé pour les éléments du plus haut niveau, comme les volets de navigation ou les barres de commandes.
- **List** est utilisé pour les contrôles de liste.

**Low**/**Medium**/**High** font référence à l’intensité de la couleur.

### <a name="using-theme-brushes"></a>Utilisation des pinceaux de thème

:::row:::
    :::column:::
        When creating templates for custom controls, use theme brushes rather than hard code color values. This way, your app can easily adapt to any theme.

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

Pour plus d’informations sur l’utilisation des pinceaux de thème dans votre application, consultez [Ressources de thème](../controls-and-patterns/xaml-theme-resources.md).

## <a name="accent-color"></a>Couleur d’accentuation

Les contrôles courants utilisent une couleur d’accentuation pour donner des informations d’état. Par défaut, la couleur d’accentuation est la valeur de `SystemAccentColor` sélectionnée par les utilisateurs dans leurs paramètres. Cependant, vous pouvez aussi personnaliser la couleur d’accentuation de votre application pour refléter votre marque.

![contrôles Windows](images/color/windows-controls.svg)

:::row:::
    :::column:::
        ![user-selected accent header](images/color/user-accent.svg)
        ![user-selected accent color](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![custom accent header](images/color/custom-accent.svg)
        ![custom brand accent color](images/color/brand-color.svg)
    :::column-end:::
:::row-end:::

### <a name="overriding-the-accent-color"></a>Remplacement de la couleur d’accentuation

Pour changer la couleur d’accentuation de votre application, placez le code suivant dans `app.xaml`.

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

Ces nuances sont accessibles en tant que [ressources de thème](../controls-and-patterns/xaml-theme-resources.md) :

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true -->
Vous pouvez également accéder à la palette de couleurs d’accentuation par programmation avec la méthode [**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) et l’énumération [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType).

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

Quand vous utilisez du texte en couleur sur un arrière-plan en couleur, vérifiez que le contraste est suffisant entre le texte et l’arrière-plan. Par défaut, le lien hypertexte utilise la couleur d’accentuation. Si vous appliquez des variantes de la couleur d’accentuation à l’arrière-plan, vous devez utiliser une variante de la couleur d’accentuation d’origine pour optimiser le contraste du texte en couleur sur un arrière-plan en couleur.

Le graphique ci-dessous illustre un exemple des différentes nuances claires et foncées de la couleur d’accentuation. Il décrit également comment le type peut être appliqué sur une surface en couleur.

![couleur sur couleur](images/color/color-on-color.png)

Pour plus d’informations sur l’application de styles aux contrôles, voir [Styles XAML](../controls-and-patterns/xaml-styles.md).

## <a name="color-api"></a>API de couleur

Vous pouvez utiliser plusieurs API pour ajouter de la couleur à votre application. Il y a d’abord la classe [**Colors**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors), qui implémente une longue liste de couleurs prédéfinies. Celles-ci sont accessibles automatiquement avec les propriétés XAML. Dans l’exemple ci-dessous, nous créons un bouton et nous définissons les propriétés de couleur d’arrière-plan et de premier plan pour les membres de la classe **Colors**.

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

Les lettres « Arvb » signifient Alpha (opacité), Rouge, Vert et Bleu, qui sont les quatre composants d’une couleur. Chaque argument peut être compris entre 0 et 255. Vous pouvez choisir d’omettre la première valeur, ce qui vous donne une opacité par défaut de 255, c’est-à-dire 100 % opaque.

> [!Note]
> Si vous utilisez C++, vous devez créer des couleurs à l’aide de la classe [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper).

Un objet **Color** est le plus souvent utilisé comme argument d’un objet [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush), qui peut être utilisé pour peindre les éléments de l’interface utilisateur dans une seule couleur unie. Ces pinceaux sont généralement définis dans un objet [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary) : ils peuvent donc être réutilisés pour plusieurs éléments.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

Pour plus d’informations sur l’utilisation des pinceaux, consultez [Pinceaux XAML](brushes.md).

## <a name="scoping-system-colors"></a>Étendue des couleurs système

En plus de définir vos propres couleurs dans votre application, vous pouvez également délimiter nos couleurs systématisées à des régions souhaitées dans toute votre application en utilisant la balise **ColorPaletteResources**. Cette API vous permet non seulement de mettre en couleur et d’appliquer des thèmes pour des contrôles en même temps en définissant quelques propriétés, mais vous apporte aussi de nombreux avantages du système que vous n’obtenez normalement pas en définissant vos propres couleurs personnalisées manuellement :

- Une couleur définie avec **ColorPaletteResources** ne va pas apporter un contraste élevé.
  * Cela signifie que votre application sera accessible à plus de personnes sans coût de développement ou de conception supplémentaires.
- Vous pouvez facilement définir des couleurs sur Light, sur Dark ou pour qu’elle soit omniprésente sur les deux thèmes en définissant une seule propriété sur l’API.
- Les couleurs définies sur **ColorPaletteResources** sont appliquées en cascade à tous les contrôles similaires qui utilisent également cette couleur système.
  * Ceci vous permet d’avoir un schéma de couleurs cohérent dans votre application tout en conservant l’aspect de votre marque
- Traite tous les états visuels, les animations et les variations d’opacité sans qu’il soit nécessaire de recréer un modèle

### <a name="how-to-use-colorpaletteresources"></a>Comment utiliser ColorPaletteResources

ColorPaletteResources est une API qui indique au système quelles ressources sont dans l’étendue. ColorPaletteResources doit prendre un [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute), qui peut être un des trois choix suivants :
- Default
  * Affiche vos modifications de couleur dans les thèmes [Light](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) et [Dark](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)
- Maigre
  * Affiche vos modifications de couleur seulement dans le thème [Light](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)
- Sombre
  * Affiche vos modifications de couleur seulement dans le thème [Dark](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)

La définition de ce x:Key garantit que vos couleurs changent de façon appropriée pour le thème système ou le thème de l’application dans le cas où vous voulez une apparence personnalisée différente dans l’un ou l’autre thème.

### <a name="how-to-apply-scoped-colors"></a>Comment appliquer des couleurs délimitées

La délimitation des ressources via l’API **ColorPaletteResources** dans XAML vous permet de prendre n’importe quelle couleur ou pinceau système qui se trouve dans notre bibliothèque de [ressources de thème](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources), et de les redéfinir dans l’étendue d’une page ou d’un conteneur.

Par exemple, si vous avez défini deux couleurs système - **BaseLow** et **BaseMediumLow** dans une grille, puis que vous avez placé deux boutons sur votre page, un à l’intérieur de cette grille et un à l’extérieur :

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
        BaseLow="LightGreen"
        BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

Vous obtenez **Button_A**, auquel les nouvelles couleurs sont appliquées, et **Button_B**, dont l’apparence reste celle du bouton système par défaut :

![couleurs système délimitées à un bouton](images/color/scopedcolors_cyan_button.png)

Cependant, comme toutes nos couleurs système sont également appliquées en cascade à d’autres contrôles, la définition de **BaseLow** et de **BaseMediumLow** va affecter d’autres éléments que simplement les boutons. Dans ce cas, des contrôles comme **ToggleButton**, **RadioButton** et **Slider** sont également affectés par ces changements de couleur système dès lors qu’ils se trouvent dans l’étendue de la grille de notre exemple ci-dessus.
Si vous voulez délimiter un changement de la couleur système *à un seul contrôle* vous pouvez le faire en définissant **ColorPaletteResources** dans les ressources de ce contrôle :

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="LightGreen"
                BaseMediumLow="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
Vous avez exactement la même chose qu’auparavant, mais maintenant, tous les contrôles ajoutés à la grille ne vont pas être soumis aux changements de couleur. La raison en est que ces couleurs système sont délimitées au seul élément **Button_A**.

### <a name="nesting-scoped-resources"></a>Imbrication de ressources délimitées

L’imbrication de couleurs système est également possible et se fait en plaçant **ColorPaletteResources** dans les ressources des éléments imbriqués, dans le balisage de la disposition de votre application :

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
            BaseLow="LightGreen"
            BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="Goldenrod"
                BaseMediumLow="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

Dans cet exemple, **Button_A** hérite des couleurs définies dans les ressources de **Grid_A**, et **Nested Button** hérite des couleurs provenant des ressources de **Grid_B**. Par extension, cela signifie que tous les autres contrôles placés dans **Grid_B** vont d’abord consulter ou appliquer les ressources de **Grid_B**, avant de consulter ou d’appliquer les ressources de **Grid_A**, et qu’ils vont au final appliquer nos couleurs par défaut si rien n’est défini au niveau de la page ou de l’application.

Ceci fonctionne pour n’importe quel nombre d’éléments imbriqués dont les ressources ont des définitions de couleur.

### <a name="scoping-with-a-resourcedictionary"></a>Délimitation avec un ResourceDictionary

Vous n’êtes pas limité aux ressources d’un conteneur ou d’une page, et vous pouvez aussi définir ces couleurs système dans un ResourceDictionary, qui peut ensuite être fusionné à n’importe quelle étendue, de la façon dont vous fusionnez normalement un dictionnaire.

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

Vous créez d’abord un ResourceDictionary. Vous placez ensuite le **ColorPaletteResources** dans les ThemeDictionaries et vous remplacez les couleurs système souhaitées :

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TestApp">

    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
            <ResourceDictionary.MergedDictionaries>

                <ColorPaletteResources x:Key="Default"
                    Accent="#FF0073CF"
                    AltHigh="#FF000000"
                    AltLow="#FF000000"/>

            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>        
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

#### <a name="mainpagexaml"></a>MainPage.xaml

Dans la page contenant votre disposition, fusionnez simplement ce dictionnaire à l’étendue souhaitée :

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <ResourceDictionary Source="MyCustomTheme.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
    </Grid.Resources>

    <Button Content="Button_A"/>
</Grid>
```

À présent, toutes les ressources, les thèmes et les couleurs personnalisées peuvent être placés dans un même dictionnaire de ressources **MyCustomTheme** et délimitées où c’est nécessaire, sans avoir à se soucier de l’encombrement supplémentaire dans le balisage de votre disposition.

### <a name="other-ways-to-define-color-resources"></a>Autres moyens de définir des ressources de couleur

ColorPaletteResources permet aussi le placement de couleurs système et leur définition directe dans celui-ci en tant que wrapper, au lieu d’une définition « in line » :

``` xaml
<ColorPaletteResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorPaletteResources>
```

## <a name="usability"></a>Facilité d'utilisation

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessibility should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the usability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the usability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Articles connexes

- [Styles XAML](../controls-and-patterns/xaml-styles.md)
- [Ressources de thème XAML](../controls-and-patterns/xaml-theme-resources.md)
