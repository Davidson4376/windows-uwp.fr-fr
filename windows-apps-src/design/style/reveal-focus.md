---
description: Révéler que focus est un effet visuel qui anime la bordure des éléments susceptibles d’être activés lorsque l’utilisateur déplace le focus clavier ou du boîtier de commande sur ces derniers.
title: Révéler Focus
template: detail.hbs
ms.date: 03/1/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 311e5714c5428fac6509564fd00784299a02f630
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8748052"
---
# <a name="reveal-focus"></a>Révéler Focus

![image hero](images/header-reveal-focus.svg)

Révéler que focus est un effet visuel pour [des expériences «10-foot»](/windows/uwp/design/devices/designing-for-tv), par exemple, les écrans de télévision et Xbox One. Cet effet anime la bordure des éléments susceptibles d’être activés, comme les boutons, lorsque l’utilisateur déplace le focus du clavier ou du boîtier de commande sur ces derniers. Il est désactivé par défaut, mais il est facile de l’activer. 

(Pour l’effet révéler mettre en surbrillance, un effet d’éclairage qui met en évidence les éléments interactifs, consultez l' [article révéler mettre en surbrillance](/windows/uwp/design/style/reveal).)


> **API importantes**: [propriété Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [FocusVisualKind enum](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [propriété Control.UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Principe de fonctionnement
L’effet révéler Focus attire l’attention sur les éléments actifs en ajoutant un éclat animé autour de bordure de l’élément:

![Visuel de l’effet de révélation](images/traveling-focus-fullscreen-light-rf.gif)

Cela est particulièrement utile dans les scénarios «10-foot» où l’utilisateur ne peut pas être totalement concentré sur l’écran de TV entier. 

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> est installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/RevealFocus">Ouvrir l’application et voir révéler Focus en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Mode d’utilisation

Révéler que focus est désactivé par défaut. Pour l’activer:
1. Dans le constructeur de votre application, appelez la propriété [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) et vérifiez si la famille d’appareils en cours est `Windows.Xbox`.
2. Si la famille d’appareils est `Windows.Xbox`, définissez la propriété [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) sur `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Une fois que vous définissez la propriété **FocusVisualKind** , le système applique automatiquement l’effet révéler le Focus à tous les contrôles dont la propriété [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) est définie sur **True** (valeur par défaut pour la plupart des contrôles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Pourquoi Focus n’est-il pas révéler sur par défaut? 
Comme vous pouvez le constater, il est relativement facile à activer révéler le Focus lorsque l’application détecte qu’il s’exécute sur une Xbox. Par conséquent, pourquoi le système ne l’active pas simplement pour vous? Étant donné que le Focus de révéler augmente la taille du visuel du focus, ce qui peut entraîner des problèmes avec la disposition de votre interface utilisateur. Dans certains cas, vous voudrez personnaliser l’effet révéler la mise au point pour optimiser pour votre application.

## <a name="customizing-reveal-focus"></a>Personnaliser révéler Focus

Vous pouvez personnaliser l’effet révéler le Focus en modifiant les propriétés visuelles du focus pour chaque contrôle: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)et [ FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Ces propriétés vous permettent de personnaliser la couleur et l’épaisseur du rectangle de focus. (Ils s’agit des mêmes propriétés que vous utilisez pour la création de [visuels de focus à haute visibilité](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals).) 

Toutefois, avant de commencer la personnalisation, il est utile de connaître un peu plus les composants qui composent le Focus de révéler.

Il existe trois parties, les visuels de Focus révéler par défaut: la bordure principale, la bordure secondaire et l’éclat. La bordure principale a une épaisseur de **2px** et apparaît autour de la bordure secondaire *extérieure*. La bordure secondaire a une épaisseur de **1px** et apparaît autour de la bordure principale *intérieure*. L’éclat de révéler le Focus a une épaisseur proportionnelle à l’épaisseur de la bordure principale et apparaît autour de l' *extérieur* de la bordure principale.

En plus des éléments statiques, visuels de Focus révéler dotés d’une lumière animée qui clignote au repos et se déplace dans la direction du focus lors du déplacement du focus.

![Révéler les couches de Focus](images/reveal-breakdown.svg)

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

Pour modifier la couleur de l’effet révéler visuel du Focus, utilisez les propriétés [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) et [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) .

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

    <!-- Override Reveal Focus default resources. -->
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

Une autre façon de personnaliser le Focus de révéler consiste à désactiver les visuels de focus fournis par le système en dessinant vos propres à l’aide des états visuels. Pour plus d’informations, voir [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Révéler Focus et Fluent Design System

Révéler que focus est un composant de Fluent Design System qui ajoute de la lumière à votre application. Pour en savoir plus sur Fluent Design System et ses autres composants, voir [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Articles associés

- [Principales fonctionnalités de révéler](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Conception pour Xbox et TV](/windows/uwp/design/devices/designing-for-tv)
- [Interactions entre le boîtier de commande et la télécommande](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Effets de composition](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [De la science dans le système: Fluent Design et la Profondeur](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [De la science dans le système: Fluent Design et la Lumière](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
