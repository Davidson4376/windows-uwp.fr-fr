---
author: Jwmsft
Description: "Les styles permettent de définir les propriétés des contrôles et de réutiliser ces paramètres pour uniformiser l’apparence de plusieurs contrôles."
MS-HAID: dev\_ctrl\_layout\_txt.styling\_controls
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Application de styles aux contrôles"
ms.assetid: AB469A46-FAF5-42D0-9340-948D0EDF4150
label: Styling controls
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 2386ccbc89cad5514c0b1a4879af6d0df328263e

---

# Application de styles aux contrôles



Vous pouvez personnaliser l’apparence de vos applications de nombreuses manières à l’aide de l’infrastructure XAML. Les styles permettent de définir les propriétés des contrôles et de réutiliser ces paramètres pour uniformiser l’apparence de plusieurs contrôles.

## Bases des styles


Les styles permettent d’extraire des paramètres de propriété visuels afin de disposer de ressources réutilisables. Voici un exemple représentant 3boutons avec un style définissant les propriétés [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397), [**BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209399) et [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414). Lorsque vous appliquez un style, vous pouvez faire en sorte que les contrôles aient la même apparence sans définir ces propriétés pour chaque contrôle individuellement.

![Boutons stylés](images/styles-rainbow-buttons.png)

Vous pouvez définir un style inline en langage XAML pour un contrôle ou sous la forme de ressource réutilisable. Vous définissez les ressources dans le fichier XAML d’une page individuelle, dans le fichier App.xaml ou dans le fichier XAML d’un dictionnaire de ressources distinct. Le fichier XAML d’un dictionnaire de ressources peut être partagé entre plusieurs applications et plusieurs dictionnaires de ressources peuvent être fusionnés dans une seule application. L’endroit où la ressource est définie détermine l’étendue (champ d’application) de son utilisation. Les ressources au niveau page sont uniquement disponibles dans la page où elles sont définies. Si des ressources sont définies avec la même clé à la fois dans App.xaml et dans une page, la ressource de la page remplace la ressource de App.xaml. Si une ressource est définie dans un fichier de dictionnaire de ressources distinct, son étendue est déterminée par l’endroit où le dictionnaire de ressources est référencé.

Dans la définition de [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849), vous avez besoin d’un attribut [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857) et d’une collection d’un ou de plusieurs éléments [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817). L’attribut **TargetType** est une chaîne spécifiant le type [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) auquel appliquer le style. La valeur **TargetType** doit spécifier un type dérivé de **FrameworkElement** qui est défini par Windows Runtime ou un type personnalisé disponible dans un assembly référencé. Si vous essayez d’appliquer un style à un contrôle et que le type de ce contrôle ne correspond pas à l’attribut **TargetType** du style que vous essayez d’appliquer, une exception se produit.

Chaque élément [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) nécessite un élément [**Property**](https://msdn.microsoft.com/library/windows/apps/br208836) et [**Value**](https://msdn.microsoft.com/library/windows/apps/br208838). Ces paramètres de propriété indiquent quelle est la propriété de contrôle à laquelle ce paramètre s’applique, ainsi que la valeur à définir pour cette propriété. Vous pouvez définir la valeur **Setter.Value** avec une syntaxe d’attribut ou une syntaxe d’élément de propriété. Le code XAML ci-dessous montre le style appliqué aux boutons illustrés précédemment. Dans ce code XAML, les deux premiers éléments **Setter** utilisent une syntaxe d’attribut, tandis que le dernier élément **Setter**, relatif à la propriété [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397), utilise une syntaxe d’élément de propriété. L’[attribut x:Key](../xaml-platform/x-key-attribute.md) n’étant pas utilisé, le style est appliqué implicitement aux boutons. La section suivante explique la notion d’application implicite et explicite de styles.

```XAML
<Page.Resources>
    <Style TargetType="Button">
        <Setter Property="BorderThickness" Value="5" />
        <Setter Property="Foreground" Value="Blue" />
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

## Appliquer un style de manière implicite ou explicite

Si vous définissez un style en tant que ressource, vous pouvez l’appliquer à vos contrôles de deux façons :

-   Implicitement, en définissant uniquement un élément [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857) pour [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849).
-   Explicitement, en spécifiant un élément [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857) et un attribut [x:Key attribute](../xaml-platform/x-key-attribute.md) pour [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849), puis en définissant la propriété [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) du contrôle cible avec une référence [Extension de balisage {StaticResource}](https://msdn.microsoft.com/library/windows/apps/mt185588) qui utilise la clé explicite.

Si un style contient l’[attribut x:Key](../xaml-platform/x-key-attribute.md), vous ne pouvez l’appliquer à un contrôle qu’en définissant la propriété [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) du contrôle sur le style associé à la clé. En revanche, un style sans attribut x:Key est automatiquement appliqué à chaque contrôle de son type cible qui, sinon, n’a pas de paramètre de style explicite.

Voici deux boutons illustrant les styles implicite et explicite.

![Boutons aux styles implicite et explicite](images/styles-buttons-implicit-explicit.png)

Dans cet exemple, l’[attribut x:Key](../xaml-platform/x-key-attribute.md) est associé au premier style et le type cible de ce dernier est [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265). La propriété [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) du premier bouton est définie sur cette clé: le style est donc appliqué explicitement. Le type cible du deuxième style est **Button** et aucun attribut x:Key n’est associé à ce dernier: le style est donc appliqué implicitement au deuxième bouton.

```XAML
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="RenderTransform">
            <Setter.Value>
                <RotateTransform Angle="25"/>
            </Setter.Value>
        </Setter>
        <Setter Property="BorderBrush" Value="Orange"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

## Utiliser des styles basés sur d’autres styles

Pour faciliter la gestion des styles et optimiser leur réutilisation, vous pouvez créer des styles héritant d’autres styles. Pour créer des styles hérités, vous devez utiliser la propriété [**BasedOn**](https://msdn.microsoft.com/library/windows/apps/br208852). Les styles héritant d’autres styles doivent cibler le même type de contrôle ou un contrôle dérivé du type ciblé par le style de référence. Par exemple, si le contrôle cible du style de référence est [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/br209365), les styles basés sur ce style peuvent cibler également **ContentControl** ou des types dérivés de **ContentControl**, tels que [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) et [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527). Si vous ne définissez aucune valeur dans le style qui hérite, celle du style de référence est utilisée. Pour modifier une valeur issue du style de référence, le style qui hérite écrase cette valeur. L’exemple suivant présente un type **Button** et un type [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) avec des styles basés sur le même style de référence.

![Boutons stylés à l’aide de styles basés sur d’autres styles](images/styles-buttons-based-on.png)

Le type cible du style de référence est [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/br209365), et les propriétés [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) et [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) sont définies pour ce style. Les styles basés sur ce style ciblent les types [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) et [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) dérivés de **ContentControl**. Des couleurs différentes sont définies pour les propriétés [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397) et [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) de ces styles. (En règle générale, on ne met pas de bordure autour d’un contrôle **CheckBox**. Nous le faisons ici pour illustrer les effets du style.)

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

## Utilisation de styles en toute simplicité à l’aide d’outils

Pour appliquer rapidement des styles à vos contrôles, cliquez avec le bouton droit sur un contrôle dans l’aire de conception XAML de Microsoft Visual Studio et sélectionnez **Modifier le style** ou **Modifier le modèle** (selon le contrôle concerné). Vous pouvez ensuite appliquer un style existant en sélectionnant **Appliquer la ressource** ou en définir un nouveau en sélectionnant **Créer vide**. Si vous créez un style vide, vous avez l’option de le définir dans la page, dans le fichier App.xaml ou dans un dictionnaire de ressources distinct.

## Modifier les styles système par défaut

Lorsque vous le pouvez, vous devez utiliser les styles provenant des ressources XAML Windows Runtime par défaut. Quand vous devez définir vos propres styles, essayez de les baser sur les styles par défaut lorsque cela est possible (en utilisant des styles basés sur d’autres styles, comme expliqué précédemment, ou en commençant par modifier une copie du style par défaut d’origine).

## Propriété Template

Il est possible d’utiliser un setter de style pour la propriété [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465) d’un élément [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390). En réalité, cela constitue la majorité d’un style XAML classique et des ressources XAML d’une application . Ce sujet est abordé de manière plus détaillée dans la rubrique [Modèles de contrôles](control-templates.md).




<!--HONumber=Aug16_HO3-->


