---
author: Jwmsft
Description: Theme resources in XAML are a set of resources that apply different values depending on which system theme is active.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_theme\_resources
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Ressources de thème XAML
ms.assetid: 41B87DBF-E7A2-44E9-BEBA-AF6EEBABB81B
label: XAML theme resources
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e576814617204749a37963ac5f2724f290520349
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6276959"
---
# <a name="xaml-theme-resources"></a>Ressources de thème XAML

Les ressources de thème de XAML sont un ensemble de ressources qui s’appliquent à différentes valeurs en fonction desquelles le thème du système est actif. Trois thèmes sont pris en charge par l’infrastructure XAML: «Light» (clair), «Dark» (sombre) et «HighContrast» (contraste élevé).

**Prérequis**: cette rubrique suppose que vous avez lu [Références aux ressources ResourceDictionary et XAML](resourcedictionary-and-xaml-resource-references.md).

## <a name="theme-resources-v-static-resources"></a>Ressources de thème par rapport aux ressources statiques

Deux extensions de balisage XAML peuvent référencer une ressource XAML d’un dictionnaire de ressources XAML existant: [extension de balisage {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) et [extension de balisage {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md).

L’évaluation d’une [extension de balisage {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) se produit lors du chargement de l’application et à chaque changement de thème pendant l’exécution. Cela est notamment le cas lorsque l’utilisateur modifie les paramètres de son appareil ou lorsqu’un programme apporte à l’application un changement qui modifie son thème actuel.

En revanche, pour une [extension de balisage {StaticResource}](../../xaml-platform/staticresource-markup-extension.md), une évaluation se produit uniquement lors du premier chargement du code XAML par l’application. Il n’y a aucune mise à jour. Le processus est similaire à une opération de type rechercher-remplacer dans votre code XAML avec la valeur d’exécution réelle au démarrage de l’application.

## <a name="theme-resources-in-the-resource-dictionary-structure"></a>Ressources de thème dans la structure du dictionnaire de ressources

Chaque ressource de thème fait partie du fichier XAML themeresources.xaml. À des fins de conception, themeresources.xaml est disponible dans le dossier \\(Program Files)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;SDK version&gt;\\Generic d’une installation du Kit de développement logiciel (SDK) Windows. Les dictionnaires de ressources dans themeresources.xaml sont également reproduits dans generic.xaml dans le même répertoire.

Windows Runtime n’utilise pas ces fichiers physiques pour la correspondance du runtime. C’est pourquoi ils se trouvent spécifiquement dans un dossier DesignTime et ne sont pas copiés dans les applications par défaut. À la place, ces dictionnaires de ressources existent en mémoire dans Windows Runtime proprement dit, et les références des ressources XAML de votre application aux ressources de thème (ou ressources système) sont résolues ici lors de l’exécution.

## <a name="guidelines-for-custom-theme-resources"></a>Recommandations pour les ressources de thème personnalisées

Respectez ces recommandations lorsque vous définissez et consommez vos propres ressources de thème personnalisées:

- Spécifiez des dictionnaires pour les thèmes «Light» et «Dark», en plus de votre dictionnaire «HighContrast». Bien que vous puissiez créer un élément [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) avec la clé «Default», nous vous recommandons d’être explicite et d’utiliser «Light», «Dark» et «HighContrast».

- Utilisez l’[extension de balisage {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) dans les éléments suivants: styles, méthodes setter, modèles de contrôle, méthodes setter de propriété et animations.

- N’utilisez pas l’[extension de balisage {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) dans les définitions de ressources de votre [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807). Utilisez plutôt l’[extension de balisage {StaticResource}](../../xaml-platform/staticresource-markup-extension.md).

    EXCEPTION : vous pouvez utiliser l’extension de balisage [{ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) pour référencer les ressources indépendantes du thème de l’application dans votre [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807). Il peut s’agir par exemple des ressources de couleur d’accentuation comme `SystemAccentColor`, ou des ressources de couleur système, qui présentent généralement le préfixe SystemColor, comme `SystemColorButtonFaceColor`.

> [!CAUTION]
> Si vous ne suivez pas ces recommandations, vous constaterez peut-être un comportement inattendu lié aux thèmes dans votre application. Pour plus d’informations, voir la section [Résolution des problèmes de ressources de thème](#troubleshooting-theme-resources).

## <a name="the-xaml-color-ramp-and-theme-dependent-brushes"></a>Gamme de couleurs de code XAML et pinceaux dépendants du thème

L’ensemble des couleurs pour les thèmes «Light», «Dark» et «HighContrast» constitue la *gamme de couleurs Windows* du code XAML. Que vous souhaitiez modifier les thèmes système ou appliquer un thème système à vos propres éléments XAML, vous devez maîtriser la structure des ressources de couleur.

Pour plus d’informations sur la façon d’appliquer la couleur dans votre application UWP, voir [Couleur dans les applications UWP](../style/color.md).

### <a name="light-and-dark-theme-colors"></a>Couleurs des thèmes Light et Dark

L’infrastructure XAML fournit un ensemble de ressources [Color](/uwp/api/Windows.UI.Color) nommées, avec des valeurs adaptées aux thèmes «Light» et «Dark». Les clés que vous utilisez pour référencer ces ressources respectent le format d’attribution de noms : `System[Simple Light/Dark Name]Color`.

Le tableau ci-dessous répertorie la clé, le nom simple et la représentation sous forme de chaîne de la couleur (au format #aarrggbb) pour les ressources «Light» et «Dark» fournies par l’infrastructure XAML. La clé sert à référencer la ressource dans une application. Le «nom simple Light/Dark» est utilisé dans le cadre de la convention d’affectation de noms aux pinceaux, que nous expliquons plus tard.

| Clé                             | Nom Simple light/dark | Light      | Dark       |
|---------------------------------|------------------------|------------|------------|
| SystemAltHighColor              | AltHigh                | \#FFFFFFFF | \#FF000000 |
| SystemAltLowColor               | AltLow                 | \#33FFFFFF | \#33000000 |
| SystemAltMediumColor            | AltMedium              | \#99FFFFFF | \#99000000 |
| SystemAltMediumHighColor        | AltMediumHigh          | \#CCFFFFFF | #CC000000 |
| SystemAltMediumLowColor         | AltMediumLow           | \#66FFFFFF | \#66000000 |
| SystemBaseHighColor             | BaseHigh               | \#FF000000 | \#FFFFFFFF |
| SystemBaseLowColor              | BaseLow                | \#33000000 | \#33FFFFFF |
| SystemBaseMediumColor           | BaseMedium             | \#99000000 | \#99FFFFFF |
| SystemBaseMediumHighColor       | BaseMediumHigh         | #CC000000 | \#CCFFFFFF |
| SystemBaseMediumLowColor        | BaseMediumLow          | \#66000000 | \#66FFFFFF |
| SystemChromeAltLowColor         | ChromeAltLow           | \#FF171717 | \#FFF2F2F2 |
| SystemChromeBlackHighColor      | ChromeBlackHigh        | \#FF000000 | \#FF000000 |
| SystemChromeBlackLowColor       | ChromeBlackLow         | \#33000000 | \#33000000 |
| SystemChromeBlackMediumLowColor | ChromeBlackMediumLow   | \#66000000 | \#66000000 |
| SystemChromeBlackMediumColor    | ChromeBlackMedium      | #CC000000 | #CC000000 |
| SystemChromeDisabledHighColor   | ChromeDisabledHigh     | \#FFCCCCCC | \#FF333333 |
| SystemChromeDisabledLowColor    | ChromeDisabledLow      | \#FF7A7A7A | \#FF858585 |
| SystemChromeHighColor           | ChromeHigh             | \#FFCCCCCC | \#FF767676 |
| SystemChromeLowColor            | ChromeLow              | \#FFF2F2F2 | \#FF171717 |
| SystemChromeMediumColor         | ChromeMedium           | \#FFE6E6E6 | \#FF1F1F1F |
| SystemChromeMediumLowColor      | ChromeMediumLow        | \#FFF2F2F2 | \#FF2B2B2B |
| SystemChromeWhiteColor          | ChromeWhite            | \#FFFFFFFF | \#FFFFFFFF |
| SystemListLowColor              | ListLow                | \#19000000 | \#19FFFFFF |
| SystemListMediumColor           | ListMedium             | \#33000000 | \#33FFFFFF |

:::row:::
    :::column:::
        #### Light theme
    :::column-end:::
    :::column:::
        #### Dark theme
    :::column-end:::
:::row-end:::

#### <a name="base"></a>Base

:::row:::
    :::column:::
        ![The base light theme](images/themes/light-base.png)
    :::column-end:::
    :::column:::
        ![The base dark theme](images/themes/dark-base.png)
    :::column-end:::
:::row-end:::

#### <a name="alt"></a>Alt

:::row:::
    :::column:::
        ![The alt light theme](images/themes/light-alt.png)
    :::column-end:::
    :::column:::
        ![The alt dark theme](images/themes/dark-alt.png)
    :::column-end:::
:::row-end:::

#### <a name="list"></a>List

:::row:::
    :::column:::
        ![The list light theme](images/themes/light-list.png)
    :::column-end:::
    :::column:::
        ![The list dark theme](images/themes/dark-list.png)
    :::column-end:::
:::row-end:::

#### <a name="chrome"></a>Chrome

:::row:::
    :::column:::
        ![The chrome light theme](images/themes/light-chrome.png)
    :::column-end:::
    :::column:::
        ![The chrome dark theme](images/themes/dark-chrome.png)
    :::column-end:::
:::row-end:::

### <a name="windows-system-high-contrast-colors"></a>Couleurs à contraste élevé du système Windows

Outre l’ensemble des ressources fournies par l’infrastructure XAML, il existe un ensemble de valeurs de couleur dérivé de la palette du système Windows. Ces couleurs ne sont pas spécifiques aux applications Windows Runtime ou aux applications de plateforme Windows universelle (UWP). Toutefois, la majeure partie des ressources [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) XAML utilisent ces couleurs lorsque le système fonctionne (et lorsque l’application est en cours d’exécution) avec le thème «HighContrast». L’infrastructure XAML fournit ces couleurs système en tant que ressources à clé. Les clés respectent le format d’attribution de noms : `SystemColor[name]Color`.

Le tableau suivant répertorie les couleurs système fournies par l’infrastructure XAML en tant qu’objets de ressources dérivés de la palette du système Windows. La colonne «Nom d’options d’ergonomie» indique le nom porté par la couleur dans les paramètres Windows. La colonne «Nom simple HighContrast» fournit une description en un mot de la façon dont la couleur est appliquée dans les contrôles XAML courants. Ce nom est utilisé dans le cadre de la convention d’affectation de noms aux pinceaux, que nous expliquons plus tard. La colonne «Valeur initiale par défaut» indique les valeurs que vous obtenez si le système ne s’exécute pas du tout en mode de contraste élevé.

| Clé                           | Nom d’options d’ergonomie            | Nom simple ContrasteÉlevé | Valeur initiale par défaut |
|-------------------------------|--------------------------------|--------------------------|-----------------|
| SystemColorButtonFaceColor    | **Texte du bouton** (arrière-plan)   | Arrière-plan               | \#FFF0F0F0      |
| SystemColorButtonTextColor    | **Texte du bouton** (premier plan)   | Premier plan               | \#FF000000      |
| SystemColorGrayTextColor      | **Texte désactivé**              | Désactivée                 | \#FF6D6D6D      |
| SystemColorHighlightColor     | **Texte sélectionné** (arrière-plan) | Highlight                | \#FF3399FF      |
| SystemColorHighlightTextColor | **Texte sélectionné** (premier plan) | HighlightAlt             | \#FFFFFFFF      |
| SystemColorHotlightColor      | **Liens hypertexte**                 | Hyperlink                | \#FF0066CC      |
| SystemColorWindowColor        | **Arrière-plan**                 | PageBackground           | \#FFFFFFFF      |
| SystemColorWindowTextColor    | **Texte**                       | PageText                 | \#FF000000      |

Windows propose différents thèmes à contraste élevé, et permet à l’utilisateur de définir les couleurs spécifiques pour les paramètres de contraste élevé par l’intermédiaire des Options d’ergonomie, comme indiqué ici. Par conséquent, il n’est pas possible de fournir une liste exhaustive des valeurs de couleur à contraste élevé.

![Interface utilisateur Windows des paramètres de contraste élevé](images/high-contrast-settings.png)

Pour plus d’informations sur la prise en charge des thèmes à contraste élevé, consultez [Thèmes à contraste élevé](https://msdn.microsoft.com/library/windows/apps/mt244346).

### <a name="system-accent-color"></a>Couleur d’accentuation système

En plus des couleurs de thème à contraste élevé du système, la couleur d’accentuation système est fournie comme ressource spéciale de couleur à l’aide de la clé `SystemAccentColor`. Au moment de l’exécution, cette ressource récupère la couleur que l’utilisateur a indiquée comme couleur d’accentuation dans les paramètres de personnalisation de Windows.

> [!NOTE]
> Bien qu'il soit possible de remplacer les ressources de couleur système, il est cependant recommandé de respecter les couleurs choisies par l’utilisateur, en particulier pour les paramètres de contraste élevé.

### <a name="theme-dependent-brushes"></a>Pinceaux dépendants du thème

Les ressources de couleur indiquées dans les sections précédentes sont utilisées pour définir la propriété [Color](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) des ressources [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) dans les dictionnaires de ressources de thème du système. Les ressources de pinceau permettent d’appliquer la couleur à des éléments XAML. Les clés pour les ressources de pinceau respectent le format d’attribution de noms : `SystemControl[Simple HighContrast name][Simple light/dark name]Brush`. Exemple : `SystemControlBackroundAltHighBrush`.

Examinons à présent comment la valeur de couleur de ce pinceau est déterminée au moment de l’exécution. Dans les dictionnaires de ressources «Light» et «Dark», ce pinceau est défini comme ceci:

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{StaticResource SystemAltHighColor}"/>`

Dans le dictionnaire de ressources «HighContrast», ce pinceau est défini comme ceci:

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>`

Lorsque ce pinceau est appliqué à un élément XAML, sa couleur est déterminée au moment de l’exécution par le thème actuel, comme indiqué dans le tableau suivant.

| Thème        | Nom simple de couleur | Ressource de couleur             | Valeur d’exécution                                              |
|--------------|-------------------|----------------------------|------------------------------------------------------------|
| Light        | AltHigh           | SystemAltHighColor         | \#FFFFFFFF                                                 |
| Dark         | AltHigh           | SystemAltHighColor         | \#FF000000                                                 |
| HighContrast | Arrière-plan        | SystemColorButtonFaceColor | La couleur spécifiée dans les paramètres pour l’arrière-plan du bouton. |

Vous pouvez utiliser le schéma de nommage `SystemControl[Simple HighContrast name][Simple light/dark name]Brush` pour déterminer le pinceau à appliquer à vos propres éléments XAML.

<!--
For many examples of how the brushes are used in the XAML control templates, see the [Default control styles and templates](default-control-styles-and-templates.md).
-->

> [!NOTE]
> Les combinaisons de \[*Simple HighContrast name*\]\[*Simple light/dark name*\] ne sont pas toutes fournies en tant que ressource de pinceau.

## <a name="the-xaml-type-ramp"></a>Gamme de type XAML

Le fichier themeresources.xaml définit plusieurs ressources, qui définissent elles-mêmes un [Style](https://msdn.microsoft.com/library/windows/apps/br208849) que vous pouvez appliquer à des conteneurs de texte dans votre interface utilisateur, plus spécifiquement pour [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) ou [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565). Il ne s’agit pas des styles implicites par défaut. Ils sont fournis pour faciliter la création de définitions XAML d’interface utilisateur qui correspondent à la *gamme de type Windows* explicitée dans [Recommandations en matière de polices](../style/typography.md).

Ces styles concernent les attributs de texte à appliquer à l’ensemble du conteneur de texte. Si vous souhaitez appliquer des styles uniquement à certaines parties du texte, définissez des attributs sur les éléments de texte dans le conteneur, par exemple sur [Run](https://msdn.microsoft.com/library/windows/apps/br209959) dans [TextBlock.Inlines](https://msdn.microsoft.com/library/windows/apps/br209668) ou sur [Paragraph](https://msdn.microsoft.com/library/windows/apps/br244503) dans [RichTextBlock.Blocks](https://msdn.microsoft.com/library/windows/apps/br244347).

Les styles ressemblent à ceci quand ils sont appliqués à un élément [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652):

![styles de bloc de texte](../style/images/type/text-block-type-ramp.svg)

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

Pour savoir comment utiliser la gamme de caractères UWP dans votre application, voir [Typographie des applications UWP](../style/typography.md).

### <a name="basetextblockstyle"></a>BaseTextBlockStyle

**TargetType**: [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

Fournit les propriétés communes à tous les autres styles de conteneur [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652).

```XAML
<!-- Usage -->
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="15"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
</Style>
```

### <a name="headertextblockstyle"></a>HeaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="HeaderTextBlockStyle" TargetType="TextBlock"
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="46"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subheadertextblockstyle"></a>SubheaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubheaderTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="34"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="titletextblockstyle"></a>TitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="TitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="SemiLight"/>
    <Setter Property="FontSize" Value="24"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subtitletextblockstyle"></a>SubtitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubtitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="20"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodytextblockstyle"></a>BodyTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BodyTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="15"/>
</Style>
```

### <a name="captiontextblockstyle"></a>CaptionTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="CaptionTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="12"/>
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

### <a name="baserichtextblockstyle"></a>BaseRichTextBlockStyle

**TargetType**: [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565)

Fournit les propriétés communes à tous les autres styles de conteneur [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565).

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BaseRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BaseRichTextBlockStyle" TargetType="RichTextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="15"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodyrichtextblockstyle"></a>BodyRichTextBlockStyle

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BodyRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BodyRichTextBlockStyle" TargetType="RichTextBlock" BasedOn="{StaticResource BaseRichTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

**Remarque**: les styles [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565) n’ont pas tous les styles de gamme de texte [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) est le cas, principalement car le modèle d’objet de document basé sur un bloc pour **RichTextBlock** rend plus facile de définir des attributs sur le texte individuels éléments. Par ailleurs, si vous définissez [TextBlock.Text](https://msdn.microsoft.com/library/windows/apps/br209676) à l’aide de la propriété de contenu XAML, vous ne pouvez pas appliquer de style à un élément de texte, ce qui vous oblige à appliquer un style au conteneur. Ceci ne constitue pas un problème pour **RichTextBlock**, car son contenu de texte doit toujours figurer dans des éléments de texte spécifiques comme [Paragraph](https://msdn.microsoft.com/library/windows/apps/br244503), c’est-à-dire l’emplacement à partir duquel vous pouvez définir des styles XAML pour un en-tête de page, un sous-en-tête de page et des définitions de gamme de texte semblables.

## <a name="miscellaneous-named-styles"></a>Divers styles nommés

Il existe un ensemble supplémentaire de définitions [Style](https://msdn.microsoft.com/library/windows/apps/br208849) à clé que vous pouvez appliquer pour donner à un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) un style autre que le style implicite par défaut.

### <a name="textblockbuttonstyle"></a>TextBlockButtonStyle

**TargetType**: [ButtonBase](https://msdn.microsoft.com/library/windows/apps/br227736)

Appliquez ce style à un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) lorsque vous avez besoin d’afficher du texte sur lequel l’utilisateur peut cliquer pour déclencher une action. Le style du texte est défini en utilisant la couleur d’accentuation actuelle pour l’identifier comme interactif. Des rectangles de focus, bien adaptés au texte, sont également ajoutés. Contrairement au style implicite d’un [HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/br242739), le **TextBlockButtonStyle** ne souligne pas le texte.

Le modèle applique également des styles au texte présenté de manière à utiliser **SystemControlHyperlinkBaseMediumBrush** (pour l’état «PointerOver»), **SystemControlHighlightBaseMediumLowBrush** (pour l’état «Pressed») et **SystemControlDisabledBaseLowBrush** (pour l’état «Disabled»).

Voici un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) auquel la ressource **TextBlockButtonStyle** est appliquée.

```XAML
<Button Content="Clickable text" Style="{StaticResource TextBlockButtonStyle}"
        Click="Button_Click"/>
```

Il se présente ainsi:

![Bouton stylisé pour ressembler à du texte](images/styles-textblock-button-style.png)

### <a name="navigationbackbuttonnormalstyle"></a>NavigationBackButtonNormalStyle

**TargetType**: [Button](https://msdn.microsoft.com/library/windows/apps/br209265)

Ce [Style](https://msdn.microsoft.com/library/windows/apps/br208849) fournit un modèle complet pour un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) qui peut servir de bouton de navigation Précédent pour une application de navigation. Les dimensions par défaut sont de 40×40pixels. Pour adapter le style, vous pouvez définir explicitement [Height](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height), [Width](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [FontSize](https://msdn.microsoft.com/library/windows/apps/br209406) et d’autres propriétés sur votre **Button**, ou créer un style dérivé à l’aide de [BasedOn](https://msdn.microsoft.com/library/windows/apps/br208852).

Voici un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) auquel la ressource **NavigationBackButtonNormalStyle** est appliquée.

```XAML
<Button Style="{StaticResource NavigationBackButtonNormalStyle}" />
```

Il se présente ainsi:

![Bouton stylisé comme bouton Précédent](images/styles-back-button-normal.png)

### <a name="navigationbackbuttonsmallstyle"></a>NavigationBackButtonSmallStyle

**TargetType**: [Button](https://msdn.microsoft.com/library/windows/apps/br209265)

Ce [Style](https://msdn.microsoft.com/library/windows/apps/br208849) fournit un modèle complet pour un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) qui peut servir de bouton de navigation Précédent pour une application de navigation. Il est semblable à **NavigationBackButtonNormalStyle**, mais les dimensions sont de 30x30 pixels.

Voici un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) auquel la ressource **NavigationBackButtonSmallStyle** est appliquée.

```XAML
<Button Style="{StaticResource NavigationBackButtonSmallStyle}" />
```

## <a name="troubleshooting-theme-resources"></a>Résolution des problèmes liés aux ressources de thème

Si vous ne suivez pas les [recommandations pour l’utilisation de ressources de thème](#guidelines-for-using-theme-resources), vous constaterez peut-être un comportement inattendu lié aux thèmes dans votre application.

Par exemple, lorsque vous ouvrez un menu volant sur le thème clair, certaines parties de votre application sur le thème foncé changent également, comme si elles faisaient partie du thème clair. Ou, si vous naviguez vers une page sur le thème clair puis revenez en arrière, la page d’origine sur le thème foncé (ou certaines parties de cette page) se présente comme si elle était soumise au thème clair.

En règle générale, ces types de problèmes se produisent lorsque vous indiquez un thème «Default» et un thème «HighContrast» pour prendre en charge les scénarios à contraste élevé, puis utilisez les thèmes «Light» et «Dark» dans différentes parties de votre application.

Prenons l’exemple de cette définition de dictionnaire de thème:

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Au premier abord, elle semble correcte. Vous souhaitez modifier la couleur vers laquelle pointe `myBrush` lorsque vous utilisez le contraste élevé, mais lorsque vous n’êtes pas en contraste élevé, vous vous appuyez sur l’extension de balisage [{ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) pour vous assurer que `myBrush` pointe vers la bonne couleur pour votre thème. Si votre application n’a jamais d’ensemble [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/dn298515) sur les éléments au sein de son arborescence visuelle, elle fonctionne généralement comme prévu. Toutefois, vous rencontrez des problèmes avec votre application dès que vous commencez à modifier le thème de différentes parties de votre arborescence visuelle.

Ce problème est dû au fait que les pinceaux sont des ressources partagées, contrairement à la plupart des autres types XAML. Si vous avez 2 éléments dans des sous-arborescences XAML avec différents thèmes qui font référence à la même ressource de pinceau, lorsque l’infrastructure parcourt chaque sous-arborescence pour mettre à jour les expressions d’[extension de balisage {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md), les modifications apportées à la ressource de pinceau partagée sont reflétées dans l’autre sous-arborescence, ce qui n’est pas le résultat souhaité.

Pour résoudre ce problème, remplacez le dictionnaire «Default» par des dictionnaires de thème distincts pour les thèmes «Light» et «Dark», en plus du thème «HighContrast»:

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Toutefois, vous rencontrerez toujours des problèmes si l’une de ces ressources est référencée dans les propriétés héritées comme [Foreground](https://msdn.microsoft.com/library/windows/apps/br209414). Votre modèle de contrôle personnalisé peut spécifier la couleur de premier plan d’un élément à l’aide de l’[extension de balisage {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md), mais lorsque l’infrastructure propage la valeur héritée aux éléments enfants, elle fournit une référence directe à la ressource qui a été résolue par l’expression d’extension de balisage {ThemeResource}. Cela crée des problèmes lorsque l’infrastructure traite les modifications du thème au fur et à mesure qu’elle parcourt l’arborescence visuelle de votre contrôle. Elle évalue de nouveau l’expression d’extension de balisage {ThemeResource} afin d’obtenir une nouvelle ressource de pinceau, mais ne propage pas encore cette référence vers les enfants de votre contrôle; cela se produit plus tard, par exemple pendant la passe de mesure suivante.

Par conséquent, après avoir parcouru l’arborescence visuelle du contrôle suite à un changement de thème, l’infrastructure parcourt les enfants et met à jour les expressions d’[extension de balisage {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) définies sur chacun d’eux, ou sur les objets définis à partir de leurs propriétés. C’est à ce moment que le problème survient; l’infrastructure parcourt la ressource de pinceau et, dans la mesure où elle spécifie sa couleur à l’aide d’une extension de balisage {ThemeResource}, une réévaluation est effectuée.

À ce stade, l’infrastructure semble avoir pollué votre dictionnaire de thèmes: il inclut désormais une ressource d’un dictionnaire dont la couleur est définie à partir d’un autre dictionnaire.

Pour résoudre ce problème, utilisez l’extension de balisage [{StaticResource}](../../xaml-platform/staticresource-markup-extension.md) à la place de [{ThemeResource}](../../xaml-platform/themeresource-markup-extension.md). Si vous appliquez correctement les recommandations, les dictionnaires de thèmes se présentent ainsi:

```XAML
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Notez que l’[extension de balisage {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) est toujours utilisée à la place de l’[extension de balisage {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) dans le dictionnaire «HighContrast». Cette situation correspond à l’exception citée précédemment dans les recommandations. La plupart des valeurs de pinceau utilisées pour le thème «HighContrast» utilisent des choix de couleur qui sont contrôlés globalement par le système, mais exposés au code XAML sous forme d’une ressource spécialement nommée (ceux dont le nom comporte le préfixe SystemColor). Le système permet à l’utilisateur de définir les couleurs spécifiques qui doivent être utilisées pour les paramètres de contraste élevé par l’intermédiaire des Options d’ergonomie. Ces choix de couleur sont appliqués aux ressources spécialement nommées. L’infrastructure XAML utilise le même événement de modification de thème pour mettre également à jour ces pinceaux lorsqu’elle détecte une modification au niveau du système. C’est pourquoi l’extension de balisage {ThemeResource} est utilisée ici.