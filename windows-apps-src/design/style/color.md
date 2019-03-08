---
description: Découvrez comment utiliser des couleurs d’accentuation et des thèmes dans vos applications UWP.
title: Couleur dans les applications UWP
ms.date: 04/7/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 49d891888e26b6ce4c9f94e92605eaf7d619b6f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654254"
---
# <a name="color"></a>Color

![image hero](images/header-color.svg)

La couleur fournit un moyen intuitif de communiquer des informations aux utilisateurs de votre application : elle peut être utilisée pour indiquer l’interactivité, fournir des commentaires sur les actions de l’utilisateur et procurer à votre interface une certaine continuité visuelle. 

Dans les applications UWP, les couleurs sont principalement déterminées par la couleur d’accentuation et le thème. Dans cet article, nous expliquerons comment utiliser la couleur dans votre application et comment utiliser les ressources de couleur d’accentuation et de thème pour rendre votre application UWP utilisable dans n’importe quel contexte de thème. 

## <a name="color-principles"></a>Principes sur les couleurs

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

**Remarque** : Dans Visual Studio, la valeur par défaut RequestedTheme étant light, vous devrez modifier le RequestedTheme pour tester les deux.

## <a name="theme-brushes"></a>Pinceaux de thème

Les contrôles communs utilisent automatiquement des [pinceaux de thème](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes) pour ajuster le contraste des thèmes clair et foncé.

Par exemple, voici une illustration de la façon dont [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) utilise les pinceaux de thème :

![exemple de contrôle pour les pinceaux de thème](images/color/theme-brushes.svg)

Les pinceaux de thème sont utilisés aux fins suivantes :

- **Base** est utilisé pour le texte.
- **Alt** est l’inverse de Base.
- **Chrome** est utilisé pour les éléments de niveau supérieur, comme les volets de navigation ou les barres de commandes.
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

Pour plus d’informations sur l’utilisation des pinceaux de thème dans votre application, voir [Ressources de thème](../controls-and-patterns/xaml-theme-resources.md).

## <a name="accent-color"></a>Couleur d’accentuation

Les contrôles communs utilisent une couleur d’accentuation pour transmettre les informations d’état. Par défaut, la couleur d’accentuation est la valeur `SystemAccentColor` sélectionnée par les utilisateurs dans leurs paramètres. Toutefois, vous pouvez également personnaliser la couleur d’accentuation de votre application pour refléter votre marque.

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

Ces nuances sont accessibles en tant que [ressources de thème](../controls-and-patterns/xaml-theme-resources.md) :

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true --> Vous pouvez également accéder à la palette de couleurs d’accentuation par programme avec le [ **UISettings.GetColorValue** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) (méthode) et [ **UIColorType** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType) enum.

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

![couleur sur couleur](images/color/color-on-color.png)

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

Les lettres « Arvb » signifient Alpha (opacité), Rouge, Vert et Bleu, qui sont les quatre composants d’une couleur. Chaque argument peut être compris entre 0 et 255. Vous pouvez choisir d’omettre la première valeur, ce qui vous donne une opacité par défaut de 255 ou 100 % opaque.

> [!Note]
> Si vous utilisez C++, vous devez créer des couleurs à l’aide de la classe [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper).

Un objet **Color** est le plus souvent utilisé comme argument d’un objet [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush), qui peut être utilisé pour peindre les éléments de l’interface utilisateur dans une seule couleur unie. Ces pinceaux sont généralement définis dans un objet [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary) : ils peuvent donc être réutilisés pour plusieurs éléments.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

Pour plus d’informations sur l’utilisation des pinceaux, voir [Pinceaux XAML](brushes.md).

## <a name="scoping-system-colors"></a>Portée des couleurs système

En plus de définir vos propres couleurs dans votre application, vous pouvez également étendre nos couleurs systématisées aux régions de votre choix tout au long de votre application à l’aide de la **ColorSchemeResources** balise. Cette API permet vous non seulement mettre en couleur et de grands groupes de thème des contrôles à la fois en définissant des quelques propriétés, mais également donne vous autre système de nombreux avantages que vous n’obtient normalement par la définition de vos propres couleurs personnalisées manuellement :

- N’importe quelle couleur à l’aide de **ColorSchemeResources** ne sera pas effectif de contraste élevé
  * Ce qui signifie que votre application sera accessible à d’autres personnes sans coût de développement ou conception supplémentaires
- Pouvez facilement définir les couleurs à clair, sombre ou répandue entre les deux thèmes en définissant une propriété sur l’API
- Jeu de couleurs sur **ColorSchemeResources** se produisent en cascade à tous les contrôles similaires qui utilisent également cette couleur système
  * Cela garantit que vous aura un récit cohérente des couleurs entre votre application tout en conservant l’apparence de votre marque
- Effets de tous les états visuels, des animations et des variations d’opacité sans avoir besoin de re-modèle

### <a name="how-to-use-colorschemeresources"></a>Comment utiliser ColorSchemeResources

ColorSchemeResources est une API qui indique le système de quelles ressources sont en cours étendue where. ColorSchemeResources doit prendre un [x : Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute), qui peut être une des trois options :
- Default
  * Affiche vos modifications de couleur dans les deux [Light](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) et [foncé](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme) thème
- Maigre
  * Affiche la couleur change uniquement dans [le thème clair](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) 
- Sombre
  * Affiche la couleur change uniquement dans [le thème sombre](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)

Définition de x : Key garantit que vos couleurs modifier de manière appropriée pour le thème système ou application, si vous souhaitez une apparence personnalisée différente dans un thème.

### <a name="how-to-apply-scoped-colors"></a>Comment appliquer des couleurs délimitées

Étendue des ressources via le **ColorSchemeResources** API dans XAML vous permet de prendre toute couleur système ou un pinceau qui se trouve dans notre [les ressources de thème](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources) bibliothèque et les redéfinir dans l’étendue d’une page ou conteneur.

Par exemple, si vous avez défini deux couleurs système - **SystemBaseLowColor** et **SystemBaseMediumLowColor** à l’intérieur d’une grille et puis placé deux boutons sur votre page : un à l’intérieur de cette grille et un extérieur :

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default" 
        SystemBaseLowColor="LightGreen" 
        SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

Vous obtiendriez **Button_A** avec les couleurs de nouveau appliquées, et **Button_B** resterait qualité comme notre bouton par défaut du système :

![couleurs système étendue sur le bouton](images/color/scopedcolors_cyan_button.png)

Toutefois, étant donné que toutes les couleurs de notre système en cascade trop à d’autres contrôles, l’affectation **SystemBaseLowColor** et **SystemBaseMediumLowColor** affecteront plus que des boutons. Dans ce cas, les contrôles tels que **ToggleButton**, **RadioButton** et **curseur** sont également concernés par ces modifications de couleur système, ces contrôles à ranger en ci-dessus ainsi étendue de la grille.
Si vous souhaitez étendre une modification de la couleur système *à un seul contrôle uniquement* vous pouvez le faire en définissant **ColorSchemeResources** au sein des ressources de ce contrôle :

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorSchemeResources x:Key="Default" 
                SystemBaseLowColor="LightGreen" 
                SystemBaseMediumLowColor="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
Vous avez essentiellement exactement la même chose en tant qu’avant, mais désormais tous les contrôles ajoutés à la grille ne récupéreront pas les modifications de couleur. Il s’agit, car ces couleurs système sont étendues à **Button_A** uniquement.

### <a name="nesting-scoped-resources"></a>Ressources de l’imbrication de la portée

Imbrication des couleurs système est également possible et est le fait en plaçant **ColorSchemeResources** dans les ressources des éléments imbriqués dans le balisage de la disposition de votre application :

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default"
            SystemBaseLowColor="LightGreen"
            SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorSchemeResources x:Key="Default"
                SystemBaseLowColor="Goldenrod"
                SystemBaseMediumLowColor="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

Dans cet exemple, **Button_A** est définir des couleurs qui hérite dans **Grid_A**de ressources, et **bouton imbriqué** hérite des couleurs à partir de **Grid_B**de ressources. Par extension, cela signifie que tous les contrôles placés dans **Grid_B** sera vérifier ou appliquer **Grid_B**de ressources tout d’abord, avant la vérification ou l’application **Grid_A**de les ressources et finalement à l’appliquer nos couleurs par défaut si rien n’est défini au niveau de la page ou une application.

Cela fonctionne pour n’importe quel nombre d’éléments imbriqués dont les ressources ont des définitions de couleur.

### <a name="scoping-with-a-resourcedictionary"></a>L’étendue avec un ResourceDictionary

Vous n’êtes pas limité à un conteneur ou les ressources de la page et que vous pouvez également définir ces couleurs système dans un ResourceDictionary qui peut être fusionné à n’importe quelle étendue la façon de vous serez normalement fusionner un dictionnaire.

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

Tout d’abord, vous devez créer un ResourceDictionary. Placez ensuite le **ColorPaletteResources** au sein de la ThemeDictionaries et remplacer les couleurs système souhaité :

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

Dans la page contenant votre disposition, fusionner ce dictionnaire dans l’étendue souhaitée :

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

À présent, toutes les ressources, les thèmes et les couleurs personnalisées peuvent être placés dans un seul **MyCustomTheme** dictionnaire de ressources et de portée lorsque cela est nécessaire sans avoir à vous soucier de l’encombrement supplémentaire dans votre balisage de disposition.

### <a name="other-ways-to-define-color-resources"></a>Autres façons de définir des ressources de couleur

ColorSchemeResources ainsi que des couleurs système à placer et la définition directement dans celui-ci comme un wrapper, plutôt que dans la ligne :

``` xaml
<ColorSchemeResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorSchemeResources>
```

## <a name="usability"></a>Facilité d'utilisation

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Articles connexes

- [Styles XAML](../controls-and-patterns/xaml-styles.md)
- [Ressources de thème XAML](../controls-and-patterns/xaml-theme-resources.md)
