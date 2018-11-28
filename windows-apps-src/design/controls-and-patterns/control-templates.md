---
Description: You can customize a control's visual structure and visual behavior by creating a control template in the XAML framework.
MS-HAID: dev\_ctrl\_layout\_txt.control\_templates
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Modèles de contrôles
ms.assetid: 6E642626-A1D6-482F-9F7E-DBBA7A071DAD
label: Control templates
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9ad66d8797234d01673518256d6f5376ce93245f
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7851424"
---
# <a name="control-templates"></a>Modèles de contrôles

Vous pouvez personnaliser la structure et le comportement visuels d’un contrôle en créant un modèle de contrôle dans l’infrastructure XAML. Les contrôles sont dotés de plusieurs propriétés, telles que [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395), [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) et [**FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209404) que vous pouvez définir en spécifiant les différents aspects de l’apparence du contrôle. Cependant, les modifications que vous apportez en définissant ces propriétés sont limitées. Vous pouvez spécifier des personnalisations supplémentaires en créant un modèle à l’aide de la classe [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). Voici comment créer une classe **ControlTemplate** pour personnaliser l’apparence d’un contrôle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316).

> **API importantes**: [**classe ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391), [**propriété Control.Template**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.template.aspx)

## <a name="custom-control-template-example"></a>Exemple de modèle de contrôle personnalisé

Par défaut, le contenu (chaîne ou objet situé à côté de [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)) d’un contrôle **CheckBox** figure à droite de la case de sélection, et une coche indique qu’un utilisateur a sélectionné le **CheckBox**. Ces caractéristiques représentent la structure et le comportement visuels de la case **CheckBox**.

Vous trouverez ci-dessous l’exemple d’une case [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) avec le modèle [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) par défaut aux états `Unchecked`, `Checked` et `Indeterminate`.

![modèle checkbox par défaut](images/templates-checkbox-states-default.png)

Vous pouvez modifier ces caractéristiques en créant un modèle [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) pour le [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316). Par exemple, si vous souhaitez placer le contenu d’une case à cocher en dessous de la case de sélection et utiliser un **X** pour indiquer qu’un utilisateur a sélectionné la case. Vous spécifiez alors ces caractéristiques dans le modèle **ControlTemplate** de **CheckBox**.

Pour utiliser un modèle personnalisé avec un contrôle, affectez le modèle [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) à la propriété [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465) du contrôle. Vous trouverez ci-dessous l’exemple d’un contrôle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) avec un modèle **ControlTemplate** appelé `CheckBoxTemplate1`. Le code XAML (Extensible Application Markup Language) relatif au modèle **ControlTemplate** est présenté à la section suivante.

```XAML
<CheckBox Content="CheckBox" Template="{StaticResource CheckBoxTemplate1}" IsThreeState="True" Margin="20"/>
```

Voici ce à quoi ressemble ce contrôle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) aux états `Unchecked`, `Checked` et `Indeterminate` une fois le modèle appliqué.

![modèle checkbox personnalisé](images/templates-checkbox-states.png)

## <a name="specify-the-visual-structure-of-a-control"></a>Spécifier la structure visuelle d’un contrôle

Lorsque vous créez un modèle [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391), vous combinez des objets [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) afin d’obtenir un contrôle unique. Un modèle **ControlTemplate** doit uniquement disposer d’un objet **FrameworkElement** comme élément racine. L’élément racine contient généralement d’autres objets **FrameworkElement**. La combinaison des objets forme la structure visuelle du contrôle.

Le code XAML suivant permet de créer un modèle [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) pour un contrôle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) spécifiant que le contenu du contrôle doit être placé en dessous la case de sélection. L’élément racine est un élément [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250). L’exemple fourni inclut la mention [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) pour créer un **X** indiquant que l’utilisateur a sélectionné la case **CheckBox** et la mention [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) indiquant un état indéterminé. Notez qu’[**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) a la valeur 0 dans les sections **Path** et **Ellipse**. Ainsi, par défaut, aucune opacité ne s’affiche dans les deux cas.

Un [TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) est un lien spécial qui lie la valeur d’une propriété dans un modèle de contrôle à la valeur d’une autre propriété exposée sur le contrôle basé sur un modèle. TemplateBinding peut uniquement être utilisé dans une définition ControlTemplate en XAML. Voir [Extension de balisage TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) pour plus d’informations.

> [!NOTE]
> À partir de Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)), vous pouvez utiliser des extensions de balisage [**x: Bind**](https://msdn.microsoft.com/library/windows/apps/Mt204783) dans des lieux vous utilisez [TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md). Voir [Extension de balisage TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) pour plus d’informations.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}"
            BorderThickness="{TemplateBinding BorderThickness}"
            Background="{TemplateBinding Background}">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20"
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}"
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}"
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph"
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z"
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                  FlowDirection="LeftToRight"
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph"
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter"
                              ContentTemplate="{TemplateBinding ContentTemplate}"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}" Grid.Row="1"
                              HorizontalAlignment="Center"
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

## <a name="specify-the-visual-behavior-of-a-control"></a>Spécifier le comportement visuel d’un contrôle

Le comportement visuel indique l’apparence d’un contrôle lorsqu’il se trouve dans un état spécifique. 3 états de sélection sont associés au contrôle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) : `Checked`, `Unchecked` et `Indeterminate`. La valeur de la propriété [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/br209798) détermine l’état du contrôle **CheckBox**, lequel détermine ce qui s’affiche dans la case.

Le tableau suivant comporte les valeurs [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/br209798) possibles, les états correspondants du contrôle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) et l’apparence de **CheckBox**.

|                     |                    |                         |
|---------------------|--------------------|-------------------------|
| Valeur de **IsChecked** | État de **CheckBox** | Apparence de **CheckBox** |
| **true**            | `Checked`          | Contient un «X».        |
| **false**           | `Unchecked`        | Vide.                  |
| **null**            | `Indeterminate`    | Contient un cercle.      |


Vous spécifiez l’apparence d’un contrôle lorsqu’il se trouve dans un état spécifique à l’aide d’objets [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007). Un objet **VisualState** contient un élément [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) ou [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br243053) changeant l’apparence des éléments dans le modèle [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). Lorsque l’état du contrôle devient celui spécifié par la propriété [**VisualState.Name**](https://msdn.microsoft.com/library/windows/apps/br209031), les modifications de propriété de l’élément **Setter** ou [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) sont appliquées. Lorsque le contrôle quitte cet état, les modifications sont supprimées. Vous ajoutez des objets **VisualState** à des objets [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014). Vous ajoutez des objets **VisualStateGroup** à la propriété [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505) associée, que vous définissez sur l’élément [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) racine du modèle **ControlTemplate**.

Le code XAML suivant montre les objets [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) correspondant aux états `Checked`, `Unchecked` et `Indeterminate`. Dans cet exemple, la propriété associée [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505) est définie sur [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250), à savoir l’élément racine du modèle [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). L’état **VisualState** `Checked` indique que la valeur [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de l’élément [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) nommé `CheckGlyph` (présenté dans l’exemple précédent) est 1. L’état `Indeterminate` **VisualState** indique que la valeur **Opacity** de l’élément [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) nommé `IndeterminateGlyph` est 1. L’état **VisualState** `Unchecked` ne dispose d’aucun élément [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) ou [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490). Ainsi, le contrôle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) reprend son apparence par défaut.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}"
            BorderThickness="{TemplateBinding BorderThickness}"
            Background="{TemplateBinding Background}">

        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CheckStates">
                <VisualState x:Name="Checked">
                    <VisualState.Setters>
                        <Setter Target="CheckGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="CheckGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
                <VisualState x:Name="Unchecked"/>
                <VisualState x:Name="Indeterminate">
                    <VisualState.Setters>
                        <Setter Target="IndeterminateGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="IndeterminateGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20"
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}"
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}"
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph"
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z"
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                  FlowDirection="LeftToRight"
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph"
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter"
                              ContentTemplate="{TemplateBinding ContentTemplate}"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}" Grid.Row="1"
                              HorizontalAlignment="Center"
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

Pour mieux comprendre le fonctionnement des objets [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), observez ce qui se produit lorsque le contrôle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) passe de l’état `Unchecked` à l’état `Checked`, puis à l’état `Indeterminate`, pour revenir ensuite à l’état `Unchecked`. Voici les transitions entre les états :

|                                      |                                                                                                                                                                                                                                                                                                                                                |                                                   |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| Transition                     | Action                                                                                                                                                                                                                                                                                                                                   | Apparence du contrôle CheckBox en fin de transition |
| De `Unchecked` à `Checked`       | La valeur [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) de l’état [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) `Checked` est appliquée. Par conséquent, la valeur [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de l’élément `CheckGlyph` est 1.                                                                                                                                                         | Un X s’affiche.                                |
| De `Checked` à `Indeterminate`   | La valeur [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) de l’état [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) `Indeterminate` est appliquée. Par conséquent, la valeur [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de l’élément `IndeterminateGlyph` est 1. La valeur **Setter** de l’état **VisualState** `Checked` est supprimée. Par conséquent, la valeur [**Opacity**](https://msdn.microsoft.com/library/windows/apps/br228078) de l’élément `CheckGlyph` est 0. | Un cercle s’affiche.                            |
| De `Indeterminate` à `Unchecked` | La valeur [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) de l’état [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) `Indeterminate` est supprimée. Par conséquent, la valeur [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de l’élément `IndeterminateGlyph` est 0.                                                                                                                                           | Rien ne s’affiche.                             |

 
Pour plus d’informations sur la façon de créer des états visuels pour des contrôles, notamment la façon d’utiliser la classe [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) et les types d’animations, voir [Animations dans une table de montage séquentiel pour les états visuels](https://msdn.microsoft.com/library/windows/apps/xaml/jj819808).

## <a name="use-tools-to-work-with-themes-easily"></a>Utilisation de thèmes en toute simplicité à l’aide d’outils

Pour appliquer rapidement des thèmes à vos contrôles, cliquez avec le bouton droit sur le volet **Structure du document** de Microsoft Visual Studio et sélectionnez **Modifier le thème** ou **Modifier le style** (selon le contrôle concerné). Vous pouvez ensuite appliquer un thème existant en sélectionnant **Appliquer la ressource** ou en définir un nouveau en sélectionnant **Créer vide**.

## <a name="controls-and-accessibility"></a>Contrôles et accessibilité

Lorsque vous créez un modèle pour un contrôle, vous pouvez modifier non seulement le comportement et l’aspect visuel du contrôle, mais aussi la façon dont il se présente aux infrastructures d’accessibilité. La plateforme Windows universelle (UWP) prend en charge l’infrastructure Microsoft UI Automation pour l’accessibilité. Tous les contrôles par défaut et leurs modèles prennent en charge des types et modèles de contrôle UI Automation courants qui sont adaptés à l’objet et à la fonction du contrôle. Ces types et modèles de contrôle sont interprétés par les clients UI Automation, notamment les technologies d’assistance. Un contrôle peut ainsi être accessible au sein d’une interface utilisateur d’application accessible plus importante.

Pour séparer la logique du contrôle de base et satisfaire à une partie des exigences architecturales d’UI Automation, la prise en charge de l’accessibilité des classes de contrôle se trouve dans une classe séparée, à savoir un homologue d’automation. Les homologues d’automation interagissent parfois avec les modèles de contrôle, et ce car les homologues s’attendent à ce que certaines parties nommées soient présentes dans les modèles. Il est donc possible d’implémenter des fonctionnalités telles que des technologies d’assistance pour appeler des actions de boutons.

Lorsque vous créez un contrôle personnalisé de toutes pièces, vous êtes parfois amené à créer un homologue d’automation pour l’accompagner. Pour plus d’informations, voir [Homologues d’automation personnalisés](../accessibility/custom-automation-peers.md).

## <a name="learn-more-about-a-controls-default-template"></a>En savoir plus sur le modèle par défaut d’un contrôle

Les rubriques qui documentent les styles et les modèles des contrôles XAML vous montrent des extraits du même code XAML de départ que vous pouvez voir si vous avez utilisé les techniques **Modifier le thème** ou **Modifier le style** décrites précédemment. Chaque rubrique répertorie les noms des états visuels, les ressources de thème utilisées et le code XAML complet du style qui contient le modèle. Les rubriques peuvent vous être utiles si vous avez déjà commencé à modifier un modèle et si vous voulez voir à quoi ressemblait le modèle d’origine, ou pour vérifier que votre nouveau modèle dispose de tous les états visuels nommés nécessaires.

## <a name="theme-resources-in-control-templates"></a>Ressources de thème dans les modèles de contrôle

Pour certains des attributs des exemples XAML, vous avez peut-être remarqué des références de ressources qui utilisent l’[extension de balisage {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md). Il s’agit d’une technique qui permet à un modèle de contrôle unique d’utiliser des ressources dont les valeurs peuvent être différentes selon le thème actif. Cela est particulièrement important pour les pinceaux et les couleurs, car le but principal des thèmes est de permettre aux utilisateurs de choisir s’ils veulent appliquer un thème foncé, clair ou à contraste élevé à l’ensemble du système. Les applications qui se servent du système de ressources XAML peuvent utiliser un ensemble de ressources approprié à ce thème, afin que les choix de thème dans l’interface utilisateur d’une application reflètent le choix de thème à l’échelle du système de l’utilisateur.

 ## Obtenir l’exemple de code

* [Exemple de galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [Exemple de contrôle d’édition de texte personnalisé](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomEditControl)
