---
author: jwmsft
title: Animations de mouvement naturelles
description: En savoir plus sur les animations de mouvement naturelles et leur utilisation dans l’interface utilisateur de votre application.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows10, uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 537e722917f00d590428dd2b5ee2d24e023e52b6
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6280562"
---
# <a name="natural-motion-animations"></a>Animations de mouvement naturelles

Cet article fait une brève présentation de l’espace NaturalMotionAnimation et indique comment envisager d'un point de vue conceptuel d'utiliser ces types d’animations dans votre interface utilisateur.

## <a name="making-motion-feel-familiar-and-natural"></a>Rendre le mouvement naturel et familier

Les applications les plus réussies créent des expériences qui captent et retiennent l’attention de l’utilisateur et les aident à accomplir des tâches. Le mouvement est le facteur de différenciation essentiel entre une Interface utilisateur et une Expérience utilisateur: qui suscite une connexion entre les utilisateurs et l’application avec laquelle ils interagissent. Plus la connexion est bonne, plus l'engagement et la satisfaction des utilisateurs finaux sont élevés.

Pour qu'un mouvement contribue à créer cette connexion, l'un des moyens consiste à créer des expériences qui paraissent familières aux utilisateurs. Les utilisateurs s'attendent inconsciemment à une certaine perception du mouvement basée sur les expériences de la vie réelle. Nous voyons comment les objets glissent sur le sol, tombent d'une table, rebondissent l'un contre l'autre et oscillent sous l'effet d'un ressort. Un mouvement qui tire parti de cette attente en se basant sur la physique réelle nous paraît plus naturel. Le mouvement devient plus naturel et interactif, mais plus important encore, l’expérience entière devient plus mémorable et agréable.

![Mouvement d'échelle sans animation](images/animation/scale-no-animation.gif)
![Mouvement d'échelle avec courbe de Bézier cubique](images/animation/scale-cubic-bezier.gif)
![Mouvement d'échelle avec animation à effet ressort](images/animation/scale-spring.gif)

Le résultat obtenu est un engagement et une rétention améliorés des utilisateurs avec l’application.

## <a name="balancing-control-and-dynamism"></a>Équilibrer contrôle et dynamisme

Dans une interface utilisateur classique, les objets [KeyFrameAnimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.keyframeanimation) constituent la méthode prédominante pour décrire un mouvement. Les images clés offrent aux concepteurs et aux développeurs un contrôle supérieur pour définir le début, la fin et l'interpolation. Bien qu'elles soient très utiles dans de nombreux cas, les animations par images clés ne sont pas très dynamiques: le mouvement n’est pas adaptatif et semble le même dans n’importe quelle condition.

À l’autre extrémité du spectre, il existe des simulations souvent utilisées dans les moteurs de jeux et de physique. Ces expériences sont souvent celles qui offrent aux utilisateurs les interactions les plus réalistes et dynamiques: elles créent le sentiment d'atmosphère et de caractère aléatoire que les utilisateurs voient tous les jours. Bien qu'elles fassent paraître le mouvement plus vivant et dynamique, elles donnent moins de contrôle aux concepteurs et aux développeurs, ce qui les rend plus difficiles à intégrer dans une interface utilisateur classique.

![Diagramme du spectre de contrôle](images/animation/natural-motion-diagram.png)

Les [NaturalMotionAnimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.naturalmotionanimation)s permettent de combler ce fossé: elles permettent d'équilibrer le contrôle des éléments importants d’une animation comme le début/la fin, tout en conservant un mouvement d'aspect naturel et dynamique.

> [!NOTE]
> Les NaturalMotionAnimations ne sont pas destinées à remplacer les animations par images clés: il reste des endroits dans le langage de conception Fluent où les images clés sont recommandées. Les NaturalMotionAnimations sont destinées à être utilisées dans les cas où un mouvement est nécessaire, mais les animations par images clés ne sont pas suffisamment dynamiques.

## <a name="using-naturalmotionanimations"></a>Utilisation de NaturalMotionAnimations

À partir de Fall Creators Update, vous avez accès à une nouvelle expérience de mouvement: **les animations à effet ressort**. Voir [Animations à effet ressort](spring-animations.md) pour un examen approfondi des effets ressort.

Ce type de mouvement s'obtient à l’aide de la nouvelle NaturalMotionAnimation: une nouveau type d'animation destiné à permettre aux développeurs de créer une sensation de mouvement plus naturelle et familière dans leur interface utilisateur, avec un équilibre entre contrôle et dynamisme. Elle permet les fonctionnalités suivantes:

- Définition de valeurs de début et de fin.
- Définition d'une InitialVelocity et liaison avec une entrée avec InteractionTracker.
- Définition de propriétés de mouvement spécifiques (par exemple, DampingRatio pour les effets ressort.)

Formule générale pour la prise en main:

1. Créez la NaturalMotionAnimation à partir du Compositeur à l’aide de l'une des méthodes **Créer**.
1. Définissez les propriétés de l’animation.
1. Transmettez la NaturalMotionAnimation en tant que paramètre à l’appel StartAnimation d'un CompositionObject.
    - Ou définissez la valeur à la propriété de Mouvement d’un InertiaModifier d'InteractionTracker.

Exemple de base utilisant une NaturalMotionAnimation à effet ressort pour faire «sauter» un élément visuel vers un nouvel emplacement de décalage X:

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```
