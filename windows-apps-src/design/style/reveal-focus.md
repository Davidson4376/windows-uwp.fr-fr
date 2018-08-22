---
author: cphilippona
description: Afficher que le Focus est un effet d’éclairage qui anime la bordure des éléments peut recevoir le focus lorsque l’utilisateur déplace le focus clavier ou boîtier leur.
title: Afficher le Focus
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
ms.localizationpriority: medium
ms.openlocfilehash: 7b5fa84efbe20368be55a50ce20c8e6e5d1fe439
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2787278"
---
# <a name="reveal-focus"></a>Afficher le Focus

![image hero](images/header-reveal-focus.svg)

Afficher que le Focus est un effet d’éclairage pour [10-pied expériences](/windows/uwp/design/devices/designing-for-tv), tels que des écrans une Xbox et télévision. Cet effet anime la bordure des éléments susceptibles d’être activés, comme les boutons, lorsque l’utilisateur déplace le focus du clavier ou du boîtier de commande sur ces derniers. Il est désactivé par défaut, mais il est facile de l’activer. 

(Pour l’effet révéler mettre en surbrillance, un effet d’éclairage qui met en évidence les éléments interactifs, consultez l' [article révéler mettre en surbrillance](/windows/uwp/design/style/reveal).)


> **API importantes**: [propriété Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [FocusVisualKind enum](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [propriété Control.UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Principe de fonctionnement
Afficher les appels aux éléments ayant le focus attirer l’attention en ajoutant une lumière animée autour des bordures de l’élément:

![Visuel de l’effet de révélation](images/traveling-focus-fullscreen-light-rf.gif)

Ceci est particulièrement utile dans les scénarios de 10-pied où l’utilisateur ne peut pas être observant complète la totalité de l’écran TV. 

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application de la <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/RevealFocus">Ouvrir l’application et consulter le Focus de révéler à le œuvre.</a></p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Mode d’utilisation

Afficher que le Focus est désactivée par défaut. Pour l’activer:
1. Dans le constructeur de votre application, appelez la propriété [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) et vérifiez si la famille d’appareils en cours est `Windows.Xbox`.
2. Si la famille d’appareils est `Windows.Xbox`, définissez la propriété [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) sur `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Une fois que vous définissez la propriété **FocusVisualKind** , le système applique automatiquement l’effet révéler le Focus à tous les contrôles dont la propriété [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) est définie sur **True** (valeur par défaut pour la plupart des contrôles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Pourquoi n’est-il pas révéler le Focus sur par défaut? 
Comme vous pouvez le constater, il est assez facile à activer révéler le Focus lorsque l’application détecte qu’il s’exécute sur un Xbox. Par conséquent, pourquoi le système ne l’active pas simplement pour vous? Étant donné que le Focus révéler augmente la taille du focus visuel, qui peut entraîner des problèmes avec votre mise en page de l’interface utilisateur. Dans certains cas, vous souhaiterez personnaliser l’effet révéler le Focus pour optimiser pour votre application.

## <a name="customizing-reveal-focus"></a>Personnalisation de révéler Focus

Vous pouvez personnaliser l’effet de Focus révéler en modifiant les propriétés visuelles du focus pour chaque contrôle: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)et [ FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Ces propriétés vous permettent de personnaliser la couleur et l’épaisseur du rectangle de focus. (Ils s’agit des mêmes propriétés que vous utilisez pour la création de [visuels de focus à haute visibilité](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals).) 

Mais avant de commencer la personnalisation, il est utile de connaître un peu plus sur les composants qui constituent le Focus révéler.

Il existe trois parties visuels révéler le Focus par défaut: la bordure principale, la bordure secondaire et le révéler éclat. La bordure principale a une épaisseur de **2px** et apparaît autour de la bordure secondaire *extérieure*. La bordure secondaire a une épaisseur de **1px** et apparaît autour de la bordure principale *intérieure*. La lumière révéler le Focus a une épaisseur proportionnelle à l’épaisseur de la bordure principale et exécute autour de l' *en dehors* de la bordure du principale.

Outre les éléments statiques, des fonctionnalités visuels Focus révéler une lumière animée pulsates au repose et se déplace dans le sens de focus lors du déplacement du focus.

![Afficher les couches de Focus](images/reveal-breakdown.svg)

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

Pour modifier la couleur du Focus révéler visual, utilisez les propriétés [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) et [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) .

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

Une autre manière de personnaliser le Focus révéler consiste à exclure visuels focus fourni par le système à dessiner vos propres à l’aide des états visuels. Pour plus d’informations, voir [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Afficher le Focus et le système de conception Fluent

Afficher que le Focus est un composant de Fluent concevoir un système qui ajoute de la lumière à votre application. Pour en savoir plus sur Fluent Design System et ses autres composants, voir [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Articles associés

- [Révéler la mise en surbrillance](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Conception pour Xbox et TV](/windows/uwp/design/devices/designing-for-tv)
- [Interactions entre le boîtier de commande et la télécommande](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Effets de composition](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [De la science dans le système: Fluent Design et la Profondeur](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [De la science dans le système: Fluent Design et la Lumière](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
