---
description: Révéler que le Focus est un effet d’éclairage qui anime la bordure d’éléments pouvant prendre le focus lorsque l’utilisateur déplace le focus clavier ou gamepad leur.
title: Révéler le Focus
template: detail.hbs
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 7bcceb8d44b6d92cab05a9c077531b3fe1b05c79
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651754"
---
# <a name="reveal-focus"></a>Révéler le Focus

![image hero](images/header-reveal-focus.svg)

Révéler le Focus est un effet d’éclairage pour [10-foot expériences](/windows/uwp/design/devices/designing-for-tv), tels que Xbox One et écrans de télévision. Cet effet anime la bordure des éléments susceptibles d’être activés, comme les boutons, lorsque l’utilisateur déplace le focus du clavier ou du boîtier de commande sur ces derniers. Il est désactivé par défaut, mais il est facile de l’activer. 

(Pour l’effet de faire apparaître la mettre en surbrillance, un effet d’éclairage qui met en évidence les éléments interactifs, consultez le [révéler la mettre en surbrillance un article](/windows/uwp/design/style/reveal).)


> **API importantes**: [Propriété de Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [FocusVisualKind enum](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [Control.UseSystemFocusVisuals propriété](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Fonctionnement
Révéler les appels aux éléments ayant le focus attirer l’attention en ajoutant un éclat animée autour de bordure de l’élément :

![Visuel de l’effet Révéler](images/traveling-focus-fullscreen-light-rf.gif)

Ceci est particulièrement utile dans les scénarios de 10-foot où l’utilisateur ne peut pas être attention complète à l’écran entier. 

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/RevealFocus">ouvrez l’application et voir le Focus de révéler en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application de la galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Mode d’utilisation

Révéler que le Focus est désactivée par défaut. Pour l’activer :
1. Dans le constructeur de votre application, appelez la propriété [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) et vérifiez si la famille d’appareils en cours est `Windows.Xbox`.
2. Si la famille d’appareils est `Windows.Xbox`, définissez la propriété [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) sur `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Après avoir défini le **FocusVisualKind** propriété, le système applique automatiquement l’effet de faire apparaître le Focus à tous les contrôles dont la propriété [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) a la valeur de propriété **True** (la valeur par défaut pour la plupart des contrôles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Pourquoi n’est-elle pas révéler le Focus sur par défaut ? 
Comme vous pouvez le voir, il est assez facile à activer révéler le Focus lorsque l’application détecte qu’il s’exécute sur une console Xbox. Par conséquent, pourquoi le système ne l’active pas simplement pour vous ? Étant donné que le Focus de révéler augmente la taille de l’élément visuel de focus, ce qui peut provoquer des problèmes avec la disposition de votre interface utilisateur. Dans certains cas, vous souhaiterez sans doute personnaliser l’effet de faire apparaître le Focus pour optimiser pour votre application.

## <a name="customizing-reveal-focus"></a>Personnalisation de révéler le Focus

Vous pouvez personnaliser l’effet de faire apparaître le Focus en modifiant les propriétés visuelles du focus pour chaque contrôle : [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush), et [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Ces propriétés vous permettent de personnaliser la couleur et l’épaisseur du rectangle de focus. (Ils s’agit des mêmes propriétés que vous utilisez pour la création de [visuels de focus à haute visibilité](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals).) 

Mais avant de commencer la personnalisation, il est utile de connaître un peu plus sur les composants qui constituent de révéler le Focus.

Les visuels de révéler le Focus par défaut se compose de trois parties : la bordure principale, la bordure secondaire et la révélation brillent. La bordure principale a une épaisseur de **2 px** et apparaît autour de la bordure secondaire *extérieure*. La bordure secondaire a une épaisseur de **1 px** et apparaît autour de la bordure principale *intérieure*. L’éclat de révéler le Focus a une épaisseur proportionnelle à l’épaisseur de la bordure principale et s’exécute autour de la *en dehors de* de la bordure principale.

Outre les éléments statiques, visuels du Focus révéler fonctionnalité une lumière animée pulsates au repose et se déplace dans la direction de focus lors du déplacement du focus.

![Révéler les couches de Focus](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Personnaliser l’épaisseur des bordures

Pour modifier l’épaisseur des types de bordures d’un contrôle, utilisez ces propriétés :

| Type de bordure | Propriété |
| --- | --- |
| Principale, éclat   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (Le fait de modifier la bordure principale modifie de façon proportionnelle l’épaisseur de l’éclat.)   |
| Secondaire   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


Cet exemple modifie l’épaisseur de la bordure du visuel du focus d’un bouton :

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Personnaliser la marge

La marge est l’espace entre les limites de l’élément visuel du contrôle et le début de la bordure secondaire des visuels du focus. La marge par défaut est à 1 px des limites du contrôle. Vous pouvez modifier cette marge pour chaque contrôle, en modifiant la propriété [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin) :

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Une marge négative éloigne la bordure du centre du contrôle, tandis qu’une marge positive rapproche la bordure du centre du contrôle.

## <a name="customize-the-color"></a>Personnaliser la couleur

Pour modifier la couleur de l’élément visuel de Focus révéler, utilisez le [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) et [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) propriétés.

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

Pour modifier la couleur de tous les visuels du focus dans l’ensemble de votre application, vous devez remplacer les ressources **SystemControlRevealFocusVisualBrush** et **SystemControlFocusVisualSecondaryBrush** avec votre propre définition :

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

Une autre façon de personnaliser le Focus de révéler consiste à refuser les visuels du focus fournie par le système en dessinant vous-même à l’aide des états visuels. Pour plus d’informations, voir [Exemple de visuels de focus](https://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Révéler le Focus et le système de conception Fluent

Révéler que le Focus est un composant de Fluent Design System qui ajoute de la lumière à votre application. Pour en savoir plus sur Fluent Design System et ses autres composants, voir [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Articles connexes

- [Révéler la mise en surbrillance](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Conception pour Xbox et TV](/windows/uwp/design/devices/designing-for-tv)
- [Interactions GamePad et de contrôle à distance](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Exemple d’éléments visuels de focus](https://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Effets de composition](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Science dans le système : Profondeur et conception Fluent](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Science dans le système : Clair et conception Fluent](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
