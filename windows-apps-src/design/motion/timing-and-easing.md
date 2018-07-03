---
author: jwmsft
Description: Learn how Fluent motion uses timing and easing functions.
title: Minutage et accélération - animation dans les applications UWP
label: Timing and easing
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 412ba7e36c2bb36562ceee13bb1e204ff402a882
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843791"
---
# <a name="timing-and-easing"></a>Minutage et accélération

Bien que le mouvement soit basé sur le monde réel, nous sommes également un support numérique, ce qui entraîne des attentes de vitesse et de performances. 

## <a name="how-fluent-motion-uses-time"></a>Comment le mouvement Fluent utilise le temps

Le minutage est un élément important pour que le mouvement des objets entrant, sortant ou bougeant dans l’interface utilisateur paraisse naturel.

1. Les objets ou les scènes entrant dans le champ de vision sont rapides, mais mis en valeur. Ces animations sont en général plus longues que les sorties pour permettre la construction hiérarchique d’une scène.
1. Les objets ou les scènes sortant du champ de vision sont très rapides. L’utilisateur doit être en mesure de comprendre où l’interface utilisateur est allée. Toutefois, une fois que l’interface utilisateur est masquée, elle ne doit pas gêner.
1. Les objets en transit dans une scène doivent avoir une durée appropriée à la distance dont ils se déplacent.

## <a name="timing-in-fluent-motion"></a>Minutage en mouvement Fluent

Le minutage du mouvement dans Fluent prend en principe 500ms (soit une demi-seconde), car c’est la durée maximale perçue par un utilisateur comme instantanée.

![image hero](images/time.gif)

### <a name="150ms-exit"></a>**150ms** (sortie)

:::row::: :::column::: Utiliser pour des objets ou des pages sortant de la scène ou se fermant.
Permet une rétroaction directionnelle très rapide de l’interface utilisateur sortante lorsque le minutage ne gêne pas la fréquence d’images pour obtenir une animation fluide.
:::column-end::: :::column::: ![mouvement de 150ms](images/150msAlt.gif) :::column-end::: :::row-end:::

### <a name="300ms-enter"></a>**300ms** (Entrée)

:::row::: :::column::: Utiliser pour des objets ou des pages entrant dans la scène ou s'ouvrant.
Laisse une durée raisonnable de temps pour mettre en valeur le contenu qui entre dans la scène.
:::column-end::: :::column::: ![mouvement de 300ms](images/300ms.gif) :::column-end::: :::row-end:::

### <a name="500ms-move"></a>**≤500ms** (déplacement)

:::row::: :::column::: Utiliser pour les objets en transit dans une scène unique ou plusieurs scènes. :::column-end::: :::column::: ![mouvement de 500ms](images/500ms.gif) :::column-end::: :::row-end:::

## <a name="easing-in-fluent-motion"></a>Accélération en mouvement Fluent

L’accélération est un moyen de manipuler la vitesse d’un objet lors de son transit. C'est la colle qui lie toutes les sensations de mouvement Fluent. Bien qu'extrême, l’accélération utilisée dans le système aide à unifier l’apparence physique des objets en déplacement dans l’ensemble du système. C'est une façon d'imiter le monde réel et de faire en sorte que les objets en mouvement paraissent appartenir à leur environnement.

![image hero](images/easing.gif)

## <a name="apply-easing-to-motion"></a>Appliquer l’accélération au mouvement

Ces accélérations vous aideront à atteindre un aspect plus naturel et sont la base que nous utilisons pour le mouvement Fluent.

Les exemples de code vous montrent comment appliquer des valeurs d’accélération recommandées aux animations de Storyboard (XAML) ou de Composition (C#).

### <a name="accelerate-exit"></a>**Accélérer** (sortie)

:::row::: :::column::: Utiliser pour des interfaces utilisateur des objets sortant de la scène.

        Objects become powered and gain momentum until they reach escape velocity.
        The resulting feel is that the object is trying its hardest to get out of the user's way and make room for new content to come in.
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

:::row::: :::column::: Utiliser pour des objets ou des interfaces utilisateur entrant dans la scène, soit par navigation, soit par génération.

        Once on-scene, the object is met with extreme friction, which slows the object to rest.
        The resulting feel is that the object traveled from a long distance away and entered at an extreme velocity, or is quickly returning to a rest state.

        Even if it's preceded by a moment of unresponsiveness, the velocity of the incoming object has the effect of feeling fast and responsive.
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

:::row::: :::column::: Il s’agit de l’accélération de base pour toute modification de paramètre animé à l’intérieur du système.
Utilisez l’accélération standard pour des objets qui changent d'état à l’écran, par exemple, pour des changements de position simples. Utilisez-le aussi pour des objets qui se transforment dans la scène, par exemple un objet qui grandit.

        The resulting feel is that objects changing state from A to B are overcoming, and taken over by, natural forces.
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

## <a name="related-articles"></a>Articles associés

- [Présentation du mouvement](index.md)
- [Direction et gravité](directionality-and-gravity.md)