---
author: jwmsft
title: Animations basées sur le temps
description: Utilisez des classes KeyFrameAnimation pour modifier votre interface utilisateur au fil du temps.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: Windows10, uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: bf6d3f16c7b240ca370c01a787fef09862f35863
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5568816"
---
# <a name="time-based-animations"></a>Animations basées sur le temps

Quand un composant ou l'intégralité de l'interface utilisateur change, les utilisateurs finaux l'observent souvent de deux manières: au fil du temps ou instantanément. Sur la plateforme Windows, le premier est préféré à ce dernier: les expériences utilisateur qui changent instantanément souvent et surprennent les utilisateurs finaux, car elles ne sont pas en mesure de suivre ce qui se passe. L’utilisateur final perçoit alors l’expérience comme saccadée et anormale.

Vous pouvez plutôt modifier votre interface utilisateur au fil du temps pour guider l’utilisateur final ou l'informer des modifications apportées à l’expérience. Sur la plateforme Windows, cela s'effectue à l'aide d’animations basées sur le temps, également appelées KeyFrameAnimations. Les KeyFrameAnimations vous permettent de modifier une interface utilisateur au fil du temps et de contrôler chaque aspect de l’animation, notamment quand et comment elle démarre, et comment elle atteint son état de fin. Par exemple, l’animation d’un objet pour le déplacer vers une nouvelle position pendant 300millisecondes est plus agréable qu’une «téléportation» instantanée. Lorsque vous utilisez des animations au lieu de modifications instantanées, vous obtenez une expérience plus agréable et plus attrayante.

## <a name="types-of-time-based-animations"></a>Types d’animations basées sur le temps

Il existe deux catégories d’animations basées sur le temps que vous pouvez utiliser pour créer de magnifiques expériences utilisateur sur Windows:

**Animations explicites**: comme le nom l'indique, vous démarrez explicitement l’animation pour effectuer des mises à jour.
**Animations implicites**: ces animations sont déclenchées par le système en votre nom lorsqu’une condition est remplie.

Dans cet article, nous allons expliquer comment créer et utiliser des animations _explicit_ basées sur le temps à l'aide de KeyFrameAnimations.

Que ce soit pour des animations basées sur le temps explicites ou implicites, il existe différents types correspondant aux divers types de propriétés de CompositionObjects que vous pouvez animer.

- ColorKeyFrameAnimation
- QuaternionKeyFrameAnimation
- ScalarKeyFrameAnimation
- Vector2KeyFrameAnimation
- Vector3KeyFrameAnimation
- Vector4KeyFrameAnimation

## <a name="create-time-based-animations-with-keyframeanimations"></a>Créer des animations basées sur le temps à l'aide de KeyFrameAnimations

Avant d'expliquer comment créer des animations explicites basées sur le temps à l'aide de KeyFrameAnimations, nous allons présenter quelques concepts.

- Images clés: ce sont des «instantanés» individuels par lesquels une animation s'anime.
  - Définies sous forme de paires clé-valeur. La clé représente la progression entre 0 et 1, c'est-à-dire le moment dans la durée de vie de l’animation où se produit cet «instantané». L’autre paramètre représente la valeur de propriété à cet instant.
- Propriétés KeyFrameAnimation: options de personnalisation que vous pouvez appliquer pour répondre aux besoins de l’interface utilisateur.
  - DelayTime: durée avant le démarrage d’une animation après l’appel de StartAnimation.
  - Duration: durée de l’animation.
  - IterationBehavior: nombre ou comportement de répétition à l’infini d’une animation.
  - IterationCount: nombre de fois qu’une animation d’image clé se répète.
  - KeyFrameCount: indique le nombre d’images clés que contient une animation par images clés.
  - StopBehavior: spécifie le comportement d’une valeur de propriété d’animation quand StopAnimation est appelé.
  - Direction: indique le sens de lecture de l’animation.
- Animation Group: démarrage de plusieurs animations en même temps.
  - Souvent utilisé si vous voulez animer plusieurs propriétés en même temps.

Pour plus d’informations, voir [CompositionAnimationGroup](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionanimationgroup).

En gardant ces concepts à l’esprit, examinons la formule générale qui permet de construire une KeyFrameAnimation:

1. Identifiez le CompositionObject et sa propriété respective que vous devez animer.
1. Créez un modèle de type KeyFrameAnimation du compositeur correspondant au type de propriété que vous souhaitez animer.
1. À l’aide du modèle d’animation, commencez à ajouter des images clés et à définir des propriétés de l’animation.
    - Une image clé au minimum est obligatoire (image clé 100% ou 1f).
    - Il est recommandé de définir une durée également.
1. Une fois que vous êtes prêt à exécuter cette animation, appelez StartAnimation(...) sur le CompositionObject, en ciblant la propriété à animer. Plus précisément:
    - `Visual.StartAnimation("targetProperty", CompositionAnimation animation);`
    - `Visual.StartAnimationGroup(AnimationGroup animationGroup);`
1. Si vous disposez d’une animation en cours d’exécution et que vous souhaitez arrêter l’animation ou le groupe d’animations, vous pouvez utiliser ces API:
    - `Visual.StopAnimation("targetProperty");`
    - `Visual.StopAnimationGroup(AnimationGroup AnimationGroup);`

Examinons un exemple pour voir cette formule en action.

## <a name="example"></a>Exemple

Dans cet exemple, vous voulez animer le décalage d’un élément visuel de <0,0,0> à <20,20,20> pendant 1seconde. En outre, vous voulez voir les éléments visuels s'animer entre ces positions 10fois.

![Animation par images clés](images/animation/animated-rectangle.gif)

Tout d’abord, vous démarrez en identifiant le CompositionObject et la propriété à animer. Dans ce cas, le carré rouge est représenté par un élément visuel de composition nommé `redSquare`. Vous démarrez votre animation à partir de cet objet.

Ensuite, comme vous voulez animer la propriété Offset, vous devez créer un Vector3KeyFrameAnimation (Offset est de type Vector3). Vous définissez également les images clés correspondantes pour la KeyFrameAnimation.

```csharp
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
```

Ensuite, nous allons définir les propriétés de la KeyFrameAnimation pour décrire sa durée, ainsi que le comportement à animer 10fois entre les deux positions (actuelle et <200,0,0>).

```csharp
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
```

Enfin, pour exécuter une animation, vous devez la démarrer sur une propriété d’un CompositionObject.

```csharp
redVisual.StartAnimation("Offset.X", animation);
```

Voici le code complet:

```csharp
private void AnimateSquare(Compositor compositor, SpriteVisual redSquare)
{ 
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
    visual.StartAnimation("Offset.X", animation);
} 
```
