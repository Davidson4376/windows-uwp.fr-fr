---
Description: Découvrez comment Fluent utilise de mouvement de temporisation et de fonctions d’accélération.
title: Minutage et accélération - animation dans les applications UWP
label: Timing and easing
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b736a10a7284e3cc9aa193e082dc654e908afe40
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444172"
---
# <a name="timing-and-easing"></a>Minutage et accélération

Bien que le mouvement soit basé sur le monde réel, nous sommes également un support numérique, ce qui entraîne des attentes de vitesse et de performances.

## <a name="examples"></a>Exemples

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/EasingFunction">ouvrez l’application et voir les fonctions d’accélération en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>Comment le mouvement Fluent utilise le temps

Le minutage est un élément important pour que le mouvement des objets entrant, sortant ou bougeant dans l’interface utilisateur paraisse naturel.

1. Les objets ou les scènes entrant dans le champ de vision sont rapides, mais mis en valeur. Ces animations sont en général plus longues que les sorties pour permettre la construction hiérarchique d’une scène.
1. Les objets ou les scènes sortant du champ de vision sont très rapides. L’utilisateur doit être en mesure de comprendre où l’interface utilisateur est allée. Toutefois, une fois que l’interface utilisateur est masquée, elle ne doit pas gêner.
1. Les objets en transit dans une scène doivent avoir une durée appropriée à la distance dont ils se déplacent.

## <a name="timing-in-fluent-motion"></a>Minutage en mouvement Fluent

Le minutage du mouvement dans Fluent prend en principe 500 ms (soit une demi-seconde), car c’est la durée maximale perçue par un utilisateur comme instantanée.

![image hero](images/time.gif)

### <a name="150ms-exit"></a>**150 ms** (sortie)

:::row:::
    :::column:::
À utiliser pour les objets ou des pages qui sont de quitter la scène ou de fermeture.
Permet une rétroaction directionnelle très rapide de l’interface utilisateur sortante lorsque le minutage ne gêne pas la fréquence d’images pour obtenir une animation fluide.
    :::column-end:::
    :::column:::
        ![150ms motion](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300 ms** (Entrée)

:::row:::
    :::column:::
À utiliser pour les objets ou des pages qui sont la saisie de la scène ou l’ouverture.
Laisse une durée raisonnable de temps pour mettre en valeur le contenu qui entre dans la scène.
    :::column-end:::
    :::column:::
        ![300ms motion](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤500 ms** (déplacement)

:::row:::
    :::column:::
À utiliser pour les objets qui sont traduire dans une scène unique ou plusieurs scènes. 
    :::column-end:::
    :::column:::
        ![500ms motion](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>Accélération en mouvement Fluent

L’accélération est un moyen de manipuler la vitesse d’un objet lors de son transit. C'est la colle qui lie toutes les sensations de mouvement Fluent. Bien qu'extrême, l’accélération utilisée dans le système aide à unifier l’apparence physique des objets en déplacement dans l’ensemble du système. C'est une façon d'imiter le monde réel et de faire en sorte que les objets en mouvement paraissent appartenir à leur environnement.

![image hero](images/easing.gif)

## <a name="apply-easing-to-motion"></a>Appliquer l’accélération au mouvement

Ces accélérations vous aideront à atteindre un aspect plus naturel et sont la base que nous utilisons pour le mouvement Fluent.

Les exemples de code vous montrent comment appliquer des valeurs d’accélération recommandées aux animations de Storyboard (XAML) ou de Composition (C#).

### <a name="accelerate-exit"></a>**Accélérer** (sortie)

:::row:::
    :::column:::
Utilisation des objets qui sont de quitter la scène ou de l’interface utilisateur.

Objets deviennent alimentées et gagner du terrain jusqu'à ce qu’ils atteignent la rapidité d’échappement.
L’impression qui en résulte est que l’objet essaie de ses plus difficile à tirer parti de l’utilisateur et de libérer de l’espace pour le nouveau contenu à venir.
    :::column-end:::
    :::column:::
        ![accelerate easing](images/accelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.7 , 0 , 1 , 0.5)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.15">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="4.5" EasingMode="EaseIn" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction accelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.7f, 0.0f), new Vector2(1.0f, 0.5f));
_exitAnimation = _compositor.CreateScalarKeyFrameAnimation();
_exitAnimation.InsertKeyFrame(0.0f, _startValue);
_exitAnimation.InsertKeyFrame(1.0f, _endValue, accelerate);
_exitAnimation.Duration = TimeSpan.FromMilliseconds(150);
```

### <a name="decelerate-enter"></a>**Décélérer** (entrée)

:::row:::
    :::column:::
À utiliser pour les objets ou de l’interface utilisateur entrant de la scène, navigation ou lors de la génération.

Une fois sur scène, l’objet est remplie avec friction extrême, ce qui ralentit l’objet et rest.
L’impression qui en résulte est que l’objet parcourus à partir d’une longue distance et entrée à partir d’une rapidité extrême ou est de retourner rapidement à un état de repos.

Même s’il est précédé d’un moment de l’absence de réponse, la rapidité de l’objet entrant a pour effet de vous vous sentez rapide et réactif.
    :::column-end:::
    :::column:::
        ![decelerate easing](images/decelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.1 , 0.9 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.3">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="7" EasingMode="EaseOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction decelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.1f, 0.9f), new Vector2(0.2f, 1.0f));
_enterAnimation = _compositor.CreateScalarKeyFrameAnimation();
_enterAnimation.InsertKeyFrame(0.0f, _startValue);
_enterAnimation.InsertKeyFrame(1.0f, _endValue, decelerate);
_enterAnimation.Duration = TimeSpan.FromMilliseconds(300);
```

### <a name="standard-easing-move"></a>**Accélération standard** (déplacer)

:::row:::
    :::column:::
Il s’agit de la ligne de base d’accélération pour toute modification de paramètre animé à l’intérieur du système.
Utilisez l’accélération standard pour des objets qui changent d'état à l’écran, par exemple, pour des changements de position simples. Utilisez-le aussi pour des objets qui se transforment dans la scène, par exemple un objet qui grandit.

L’impression qui en résulte est que la résolution des objets de changement d’état de A à B, ce qui impose de naturel prises par.
    :::column-end:::
    :::column:::
        ![standard easing](images/standardEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.8 , 0 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.5">
        <DoubleAnimation.EasingFunction>
            <CircleEase EasingMode="EaseInOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction standard =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.8f, 0.0f), new Vector2(0.2f, 1.0f));
 _moveAnimation = _compositor.CreateScalarKeyFrameAnimation();
 _moveAnimation.InsertKeyFrame(0.0f, _startValue);
 _moveAnimation.InsertKeyFrame(1.0f, _endValue, standard);
 _moveAnimation.Duration = TimeSpan.FromMilliseconds(500);
```

## <a name="related-articles"></a>Articles connexes

- [Présentation de Motion](index.md)
- [Une direction et la gravité](directionality-and-gravity.md)
