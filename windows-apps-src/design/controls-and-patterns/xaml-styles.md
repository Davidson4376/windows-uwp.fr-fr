---
description: Les styles permettent de définir les propriétés des contrôles et de réutiliser ces paramètres pour uniformiser l’apparence de plusieurs contrôles.
MS-HAID: dev\_ctrl\_layout\_txt.styling\_controls
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
ms.date: 01/03/2019
title: Styles XAML
ms.assetid: AB469A46-FAF5-42D0-9340-948D0EDF4150
label: XAML styles
template: detail.hbs
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cd11427ed1b53641a25c32742ca114b121efcfe8
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66363960"
---
# <a name="xaml-styles"></a>Styles XAML





Vous pouvez personnaliser l’apparence de vos applications de nombreuses manières à l’aide de l’infrastructure XAML. Les styles permettent de définir les propriétés des contrôles et de réutiliser ces paramètres pour uniformiser l’apparence de plusieurs contrôles.

## <a name="style-basics"></a>Bases des styles

Les styles permettent d’extraire des paramètres de propriété visuels afin de disposer de ressources réutilisables. Voici un exemple représentant 3 boutons avec un style définissant les propriétés [BorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.borderbrush), [BorderThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.borderthickness) et [Foreground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.foreground). Lorsque vous appliquez un style, vous pouvez faire en sorte que les contrôles aient la même apparence sans définir ces propriétés pour chaque contrôle individuellement.

![Boutons stylés](images/styles-rainbow-buttons.png)

Vous pouvez définir un style inline en langage XAML pour un contrôle ou sous la forme de ressource réutilisable. Vous définissez les ressources dans le fichier XAML d’une page individuelle, dans le fichier App.xaml ou dans le fichier XAML d’un dictionnaire de ressources distinct. Le fichier XAML d’un dictionnaire de ressources peut être partagé entre plusieurs applications et plusieurs dictionnaires de ressources peuvent être fusionnés dans une seule application. L’endroit où la ressource est définie détermine l’étendue (champ d’application) de son utilisation. Les ressources au niveau page sont uniquement disponibles dans la page où elles sont définies. Si des ressources sont définies avec la même clé à la fois dans App.xaml et dans une page, la ressource de la page remplace la ressource de App.xaml. Si une ressource est définie dans un fichier de dictionnaire de ressources distinct, son étendue est déterminée par l’endroit où le dictionnaire de ressources est référencé.

Dans la définition de [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style), vous avez besoin d’un attribut [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style.targettype) et d’une collection d’un ou de plusieurs éléments [Setter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter). L’attribut **TargetType** est une chaîne qui spécifie le type [FrameworkElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) auquel appliquer le style. La valeur **TargetType** doit spécifier un type dérivé de **FrameworkElement** qui est défini par Windows Runtime ou un type personnalisé disponible dans un assembly référencé. Si vous essayez d’appliquer un style à un contrôle et que le type de ce contrôle ne correspond pas à l’attribut **TargetType** du style que vous essayez d’appliquer, une exception se produit.

Chaque élément [Setter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) nécessite un élément [Property](https://docs.microsoft.com/uwp/api/windows.ui.xaml.setter.property) et [Value](https://docs.microsoft.com/uwp/api/windows.ui.xaml.setter.value). Ces paramètres de propriété indiquent quelle est la propriété de contrôle à laquelle ce paramètre s’applique, ainsi que la valeur à définir pour cette propriété. Vous pouvez définir la valeur **Setter.Value** avec une syntaxe d’attribut ou une syntaxe d’élément de propriété. Le code XAML ci-dessous montre le style appliqué aux boutons illustrés précédemment. Dans ce code XAML, les deux premiers éléments **Setter** utilisent une syntaxe d’attribut, tandis que le dernier élément **Setter**, relatif à la propriété [BorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.borderbrush), utilise une syntaxe d’élément de propriété. L’[attribut x:Key](../../xaml-platform/x-key-attribute.md) n’étant pas utilisé, le style est appliqué implicitement aux boutons. La section suivante explique la notion d’application implicite et explicite de styles.

```XAML
<Page.Resources>
    <Style TargetType="Button">
        <Setter Property="BorderThickness" Value="5" />
        <Setter Property="Foreground" Value="Black" />
        <Setter Property="BorderBrush" >
            <Setter.Value>
                <LinearGradientBrush StartPoint="0.5,0" EndPoint="0.5,1">
                    <GradientStop Color="Yellow" Offset="0.0" />
                    <GradientStop Color="Red" Offset="0.25" />
                    <GradientStop Color="Blue" Offset="0.75" />
                    <GradientStop Color="LimeGreen" Offset="1.0" />
                </LinearGradientBrush>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<StackPanel Orientation="Horizontal">
    <Button Content="Button"/>
    <Button Content="Button"/>
    <Button Content="Button"/>
</StackPanel>
```

## <a name="apply-an-implicit-or-explicit-style"></a>Appliquer un style de manière implicite ou explicite

Si vous définissez un style en tant que ressource, vous pouvez l’appliquer à vos contrôles de deux façons :

-   Implicitement, en définissant uniquement un élément [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style.targettype) pour [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style).
-   Explicitement, en spécifiant un élément [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style.targettype) et un attribut [x:Key](../../xaml-platform/x-key-attribute.md) pour [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style), puis en définissant la propriété [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.style) du contrôle cible avec une référence [Extension de balisage {StaticResource}](https://docs.microsoft.com/windows/uwp/xaml-platform/staticresource-markup-extension) qui utilise la clé explicite.

Si un style contient l’[attribut x:Key](../../xaml-platform/x-key-attribute.md), vous ne pouvez l’appliquer à un contrôle qu’en définissant la propriété [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.style) du contrôle sur le style associé à la clé. En revanche, un style sans attribut x:Key est automatiquement appliqué à chaque contrôle de son type cible qui, sinon, n’a pas de paramètre de style explicite.

Voici deux boutons illustrant les styles implicite et explicite.

![Boutons aux styles implicite et explicite](images/styles-buttons-implicit-explicit.png)

Dans cet exemple, l’[attribut x:Key](../../xaml-platform/x-key-attribute.md) est associé au premier style et le type cible de ce dernier est [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button). La propriété [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.style) du premier bouton est définie sur cette clé : le style est donc appliqué explicitement. Le type cible du deuxième style est **Button** et aucun attribut x:Key n’est associé à ce dernier : le style est donc appliqué implicitement au deuxième bouton.

```XAML
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Segoe UI"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="Purple"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="FontFamily" Value="Segoe UI"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="RenderTransform">
            <Setter.Value>
                <RotateTransform Angle="25"/>
            </Setter.Value>
        </Setter>
        <Setter Property="BorderBrush" Value="Green"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Foreground" Value="Green"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button"/>
</Grid>
```

## <a name="use-based-on-styles"></a>Utiliser des styles basés sur d’autres styles

Pour faciliter la gestion des styles et optimiser leur réutilisation, vous pouvez créer des styles héritant d’autres styles. Pour créer des styles hérités, vous devez utiliser la propriété [BasedOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style.basedon). Les styles héritant d’autres styles doivent cibler le même type de contrôle ou un contrôle dérivé du type ciblé par le style de référence. Par exemple, si le contrôle cible du style de référence est [ContentControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl), les styles basés sur ce style peuvent cibler également **ContentControl** ou des types dérivés de **ContentControl** , tels que [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) et [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer). Si vous ne définissez aucune valeur dans le style qui hérite, celle du style de référence est utilisée. Pour modifier une valeur issue du style de référence, le style qui hérite écrase cette valeur. L’exemple suivant présente un type **Button** et un type [CheckBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) avec des styles basés sur le même style de référence.

![Boutons stylés à l’aide de styles basés sur d’autres styles](images/styles-buttons-based-on.png)

Le type cible du style de référence est [ContentControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl), et les propriétés [Height](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) et [Width](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) sont définies pour ce style. Les styles basés sur ce style ciblent les types [CheckBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) et [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) dérivés de **ContentControl**. Des couleurs différentes sont définies pour les propriétés [BorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.borderbrush) et [Foreground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.foreground) de ces styles. (En règle générale, on ne met pas de bordure autour d’un contrôle **CheckBox**. Nous le faisons ici pour illustrer les effets du style.)

```XAML
<Page.Resources>
    <Style x:Key="BasicStyle" TargetType="ContentControl">
        <Setter Property="Width" Value="130" />
        <Setter Property="Height" Value="30" />
    </Style>

    <Style x:Key="ButtonStyle" TargetType="Button"
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Orange" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Red" />
    </Style>

    <Style x:Key="CheckBoxStyle" TargetType="CheckBox"
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Blue" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Green" />
    </Style>
</Page.Resources>

<StackPanel>
    <Button Content="Button" Style="{StaticResource ButtonStyle}" Margin="0,10"/>
    <CheckBox Content="CheckBox" Style="{StaticResource CheckBoxStyle}"/>
</StackPanel>
```

## <a name="use-tools-to-work-with-styles-easily"></a>Utilisation de styles en toute simplicité à l’aide d’outils

Pour appliquer rapidement des styles à vos contrôles, cliquez avec le bouton droit sur un contrôle dans l’aire de conception XAML de Microsoft Visual Studio et sélectionnez **Modifier le style** ou **Modifier le modèle** (selon le contrôle concerné). Vous pouvez ensuite appliquer un style existant en sélectionnant **Appliquer la ressource** ou en définir un nouveau en sélectionnant **Créer vide**. Si vous créez un style vide, vous avez l’option de le définir dans la page, dans le fichier App.xaml ou dans un dictionnaire de ressources distinct.

## <a name="lightweight-styling"></a>Création d’un style léger

Le remplacement des pinceaux système est généralement effectué au niveau de l’application ou de la page. Dans les deux cas, la substitution de la couleur s’appliquera à tous les contrôles qui font référence à ce pinceau (dans du code XAML, de nombreux contrôles peuvent référencer le même pinceau système).

![Boutons stylés](images/LightweightStyling_ButtonStatesExample.png)

```XAML
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                 <SolidColorBrush x:Key="ButtonBackground" Color="Transparent"/>
                 <SolidColorBrush x:Key="ButtonForeground" Color="MediumSlateBlue"/>
                 <SolidColorBrush x:Key="ButtonBorderBrush" Color="MediumSlateBlue"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>
```

Pour les états du type PointerOver (souris déplacée sur le bouton), **PointerPressed** (activation du bouton), ou Disabled (impossible d’interagir avec le bouton). Ces terminaisons sont ajoutées aux noms de style léger d’origine : **ButtonBackgroundPointerOver**, **ButtonForegroundPointerPressed**, **ButtonBorderBrushDisabled**, etc. En modifiant ces pinceaux, vous avez la garantie que vos contrôles seront colorés de manière cohérente, suivant le thème de votre application.

Le fait de placer ces remplacements de pinceau au niveau **App.Resources** a pour effet d’affecter tous les boutons de l’ensemble de votre application, et non une seule page.

### <a name="per-control-styling"></a>Création d’un style par contrôle

Dans d’autres cas, il peut être souhaitable de modifier un seul contrôle sur une même page afin de lui affecter un style donné, sans altérer les autres versions de ce contrôle :

![Boutons stylés](images/LightweightStyling_CheckboxExample.png)

```XAML
<CheckBox Content="Normal CheckBox" Margin="5"/>
    <CheckBox Content="Special CheckBox" Margin="5">
        <CheckBox.Resources>
            <ResourceDictionary>
                <ResourceDictionary.ThemeDictionaries>
                    <ResourceDictionary x:Key="Light">
                        <SolidColorBrush x:Key="CheckBoxForegroundUnchecked"
                            Color="Purple"/>
                        <SolidColorBrush x:Key="CheckBoxForegroundChecked"
                            Color="Purple"/>
                        <SolidColorBrush x:Key="CheckBoxCheckGlyphForegroundChecked"
                            Color="White"/>
                        <SolidColorBrush x:Key="CheckBoxCheckBackgroundStrokeChecked"  
                            Color="Purple"/>
                        <SolidColorBrush x:Key="CheckBoxCheckBackgroundFillChecked"
                            Color="Purple"/>
                    </ResourceDictionary>
                </ResourceDictionary.ThemeDictionaries>
            </ResourceDictionary>
        </CheckBox.Resources>
    </CheckBox>
<CheckBox Content="Normal CheckBox" Margin="5"/>
```

Cela affecterait uniquement cet élément « Special CheckBox » sur la page qui contenait ce contrôle.

## <a name="modify-the-default-system-styles"></a>Modifier les styles système par défaut

Lorsque vous le pouvez, vous devez utiliser les styles provenant des ressources XAML Windows Runtime par défaut. Quand vous devez définir vos propres styles, essayez de les baser sur les styles par défaut lorsque cela est possible (en utilisant des styles basés sur d’autres styles, comme expliqué précédemment, ou en commençant par modifier une copie du style par défaut d’origine).

## <a name="the-template-property"></a>Propriété Template

Il est possible d’utiliser un setter de style pour la propriété [Template](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template) d’un élément [Control](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control). En réalité, cela constitue la majorité d’un style XAML classique et des ressources XAML d’une application . Ce sujet est abordé de manière plus détaillée dans la rubrique [Modèles de contrôles](control-templates.md).
