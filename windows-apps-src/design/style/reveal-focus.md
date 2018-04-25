---
author: cphilippona
description: Révéler focus est un effet visuel qui anime la bordure des éléments susceptibles d’être activés lorsque l’utilisateur déplace le focus du clavier ou du boîtier de commande sur ces derniers.
title: Effet Révéler focus
template: detail.hbs
ms.author: mijacobs
ms.date: 03/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: high
ms.openlocfilehash: f545cf38897e44dc2b3da9fac139f37bf10fc50a
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="reveal-focus"></a>Effet Révéler focus

L’effet Révéler focus est un effet visuel pour les [expériences «10-foot»](/windows/uwp/design/devices/designing-for-tv), telles que XboxOne et les écrans de télévision. Cet effet anime la bordure des éléments susceptibles d’être activés, comme les boutons, lorsque l’utilisateur déplace le focus du clavier ou du boîtier de commande sur ces derniers. Il est désactivé par défaut, mais il est facile de l’activer. 

(Pour plus d’informations sur l’effet Révéler, un effet visuel qui met en évidence les éléments interactifs, voir l’[article sur l'effet Révéler](/windows/uwp/design/style/reveal).)


> **API importantes**: [propriété Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [FocusVisualKind enum](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [propriété Control.UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Principe de fonctionnement
L’effet Révéler focus attire l’attention sur les éléments sur lesquels se trouve le focus en ajoutant un éclat animé autour de la bordure de l’élément:

![Visuel de l’effet Révéler](images/traveling-focus-fullscreen-light-rf.gif)

Ceci est particulièrement utile dans les scénarios «10-foot» où l’utilisateur peut ne pas être totalement concentré sur la totalité de l’écran de télévision. 

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/RevealFocus">ouvrir l’application et voir Révéler focus en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Mode d’utilisation

Par défaut, Révéler focus est désactivé. Pour l’activer:
1. Dans le constructeur de votre application, appelez la propriété [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) et vérifiez si la famille d’appareils en cours est `Windows.Xbox`.
2. Si la famille d’appareils est `Windows.Xbox`, définissez la propriété [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) sur `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Après avoir défini la propriété **FocusVisualKind**, le système applique automatiquement l’effet Révéler focus à tous les contrôles dont la propriété [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) est définie sur **True** (valeur par défaut pour la plupart des contrôles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Pourquoi Révéler focus n’est-il pas activé par défaut? 
Comme vous pouvez le constater, il est relativement facile d’activer Révéler focus lorsque l’application détecte qu’elle est exécutée sur une Xbox. Par conséquent, pourquoi le système ne l’active pas simplement pour vous? Parce que Révéler focus augmente la taille du visuel du focus, ce qui peut entraîner des problèmes avec la disposition de votre interface utilisateur. Dans certains cas, vous voudrez personnaliser l’effet Révéler focus afin de l’optimiser pour votre application.

## <a name="customizing-reveal-focus"></a>Personnaliser Révéler focus

Vous pouvez personnaliser l’effet Révéler focus en modifiant les propriétés du visuel du focus pour chaque contrôle: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) et [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Ces propriétés vous permettent de personnaliser la couleur et l’épaisseur du rectangle de focus. (Ils s’agit des mêmes propriétés que vous utilisez pour la création de [visuels de focus à haute visibilité](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals).) 

Mais avant de commencer la personnalisation, il est utile de connaître un peu plus les composants de l’effet Révéler focus.

Les visuels de Révéler focus par défaut sont composés de trois éléments: la bordure principale, la bordure secondaire et l’éclat. La bordure principale a une épaisseur de **2px** et apparaît autour de la bordure secondaire *extérieure*. La bordure secondaire a une épaisseur de **1px** et apparaît autour de la bordure principale *intérieure*. L’éclat de Révéler focus a une épaisseur proportionnelle à l’épaisseur de la bordure principale et apparaît autour de la bordure principale *extérieure*.

En plus des éléments statiques, les visuels de Révéler focus présentent un éclat animé qui clignote au repos et se déplace dans la direction du focus lors du déplacement de ce dernier.

![Couches de l’effet Révéler focus](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Personnaliser l’épaisseur des bordures

Pour modifier l’épaisseur des types de bordures d’un contrôle, utilisez ces propriétés:

| Type de bordure | Propriété |
| --- | --- |
| Principale, éclat   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (Le fait de modifier la bordure principale modifie de façon proportionnelle l’épaisseur de l’éclat.)   |
| Secondaire   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


Cet exemple modifie l’épaisseur de la bordure du visuel du focus d’un bouton:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Personnaliser la marge

La marge est l’espace entre les limites de l’élément visuel du contrôle et le début de la bordure secondaire des visuels du focus. La marge par défaut est à 1px des limites du contrôle. Vous pouvez modifier cette marge pour chaque contrôle, en modifiant la propriété [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin):

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Une marge négative éloigne la bordure du centre du contrôle, tandis qu’une marge positive rapproche la bordure du centre du contrôle.

## <a name="customize-the-color"></a>Personnaliser la couleur

Pour modifier la couleur du visuel Révéler focus, utilisez les propriétés [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) et [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush).

| Propriété | Ressource par défaut | Valeur de la ressource par défaut |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(La propriété FocusPrimaryBrush a uniquement pour valeur par défaut les ressources **SystemControlRevealFocusVisualBrush** lorsque **FocusVisualKind** est défini sur **Reveal**. Dans le cas contraire, elle utilise **SystemControlFocusVisualPrimaryBrush**.)


Pour modifier la couleur du visuel du focus d’un contrôle individuel, définissez les propriétés directement sur le contrôle. Cet exemple remplace les couleurs du visuel du focus d’un bouton.

```xaml

<!-- Specifying a color directly -->
<Button
    FocusVisualPrimaryBrush="DarkRed"
    FocusVisualSecondaryBrush="Pink"/>

<!-- Using theme resources -->
<Button
    FocusVisualPrimaryBrush="{ThemeResource SystemBaseHighColor}"
    FocusVisualSecondaryBrush="{ThemeResource SystemAccentColor}"/>    
```

Pour modifier la couleur de tous les visuels du focus dans l’ensemble de votre application, vous devez remplacer les ressources **SystemControlRevealFocusVisualBrush** et **SystemControlFocusVisualSecondaryBrush** avec votre propre définition:

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

Pour plus d’informations sur la modification de la couleur du visuel du focus, voir [Personnalisation des couleurs et des visuels du focus](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing).


## <a name="show-just-the-glow"></a>Afficher uniquement l’éclat

Si vous souhaitez utiliser uniquement l’éclat sans le visuel du focus principal ou secondaire, définissez simplement la propriété [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) du contrôle sur `Transparent` et [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) sur `0`. Dans ce cas, l’éclat adopte la couleur de l’arrière-plan du contrôle pour fournir un aspect sans bordure. Vous pouvez modifier l’épaisseur de l’éclat à l’aide de [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness).

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>Utiliser vos propres visuels de focus

Une autre façon de personnaliser Révéler focus consiste à ne pas utiliser les visuels de focus fournis par le système en dessinant vos propres états visuels. Pour plus d’informations, voir [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Révéler focus et Fluent Design System

Révéler focus est un composant de Fluent Design System qui ajoute des effets lumineux à votre application. Pour en savoir plus sur Fluent Design System et ses autres composants, voir [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Articles associés

- [Principales fonctionnalités de Révéler](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Conception pour Xbox et TV](/windows/uwp/design/devices/designing-for-tv)
- [Interactions entre le boîtier de commande et la télécommande](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Effets de composition](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [De la science dans le système: Fluent Design et la Profondeur](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [De la science dans le système: Fluent Design et la Lumière](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
