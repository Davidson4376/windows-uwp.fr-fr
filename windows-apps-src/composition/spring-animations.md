---
author: jwmsft
title: Animations à effet ressort
description: Découvrez comment utiliser des animations de mouvement naturelles à effet ressort.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows10, uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 2b28653fc7746075c57f862b0c885beac6d4934f
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6272213"
---
# <a name="spring-animations"></a>Animations à effet ressort

L’article montre comment utiliser des NaturalMotionAnimations à effet ressort.

## <a name="prerequisites"></a>Éléments prérequis

À ce stade, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants:

- [Animations de mouvement naturelles](natural-animations.md)

## <a name="why-springs"></a>Pourquoi des effets ressort?

Les effets ressort sont une expérience de mouvement courante, que nous avons tous expérimentée à un moment de nos vies, que ce soit avec des jouets à ressort ou en classe de physique en faisant des expériences avec un bloc attaché à un ressort. Le mouvement d'oscillation d'un ressort provoque souvent une réaction émotionnelle ludique et enjouée chez l'observateur. Par conséquent, le mouvement d’un ressort se traduit bien en interface utilisateur d’application pour ceux qui souhaitent créer une expérience de mouvement plus vivante qui «parle» plus à l’utilisateur final qu'une courbe de Bézier cubique classique. Dans ce cas, le mouvement de ressort non seulement crée une expérience de mouvement plus vivante, mais peut également contribuer à attirer l’attention sur le contenu d'animation nouveau ou en cours. Selon la personnalisation de l'application ou le langage du mouvement, l’oscillation est plus prononcée et visible, mais dans d’autres cas, elle est plus subtile.

![Mouvement avec animation à effet ressort](images/animation/offset-spring.gif)
![Mouvement avec une animation de courbe de Bézier cubique](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>Utilisation d'effets ressort dans votre interface utilisateur

Comme mentionné précédemment, les effets ressort peuvent être un mouvement utile à intégrer dans votre application pour proposer une expérience d’interface utilisateur très familière et amusante. Exemples d'utilisation courante des effets ressort dans l’interface utilisateur:

| Description de l'utilisation des effets ressort | Exemple visuel |
| ------------------------ | -------------- |
| Mettre en valeur et rendre plus vivante une expérience de mouvement. (échelle d'animation) | ![Mouvement d'échelle avec animation à effet ressort](images/animation/scale-spring.gif) |
| Rendre une expérience de mouvement sensiblement plus dynamique (décalage d'animation) | ![Mouvement de décalage avec animation à effet ressort](images/animation/offset-spring.gif) |

Dans chacun de ces cas, le mouvement de ressort peut se déclencher soit en «sautant vers» et en oscillant autour d’une nouvelle valeur, soit en oscillant autour de sa valeur actuelle avec une certaine rapidité initiale.

![Oscillation d'animation à effet ressort](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>Définition de votre mouvement de ressort

Vous créez une expérience d'effet ressort à l’aide de l'API NaturalMotionAnimation. Plus précisément, vous créez une SpringNaturalMotionAnimation en utilisant les méthodes Create* à partir du compositeur. Vous pouvez ensuite définir les propriétés suivantes du mouvement:

- DampingRatio: exprime le niveau d’amortissement du mouvement de ressort utilisé dans l’animation.

| Valeur du rapport d’amortissement | Description |
| ------------------- | ----------- |
| DampingRatio = 0 | Undamped: le ressort va osciller pendant longtemps |
| 0 < DampingRatio < 1 | Underdamped: le ressort va osciller de légèrement à fortement. |
| DampingRatio = 1 | Criticallydamped: le ressort n’effectuera aucune oscillation. |
| DampingRatio > 1 | Overdamped: le ressort va rapidement atteindre sa destination avec une décélération brusque et aucune oscillation |

- Period: temps nécessaire au ressort pour effectuer une seule oscillation.
- Valeur de fin/début: positions définies de début et de fin du mouvement de ressort (si elles ne sont pas définies, la valeur de début et/ou la valeur de fin auront la valeur actuelle).
- Rapidité initiale: rapidité initiale par programmation du mouvement.

Vous pouvez également définir un ensemble de propriétés du mouvement identiques aux KeyFrameAnimations:

- DelayTime / Delay Behavior
- StopBehavior

Dans les cas courants de décalage et d'échelle/de taille d'animation, l’équipe de conception Windows recommande les valeurs suivantes de DampingRatio et Period pour différents types d'effets ressort:

| Propriété | Ressort normal | Ressort amorti | Ressort moins amorti |
| -------- | ------------- | --------------- | -------------------- |
| Offset | Damping Ratio = 0,8 <br/> Period = 50ms | Damping Ratio = 0,85 <br/> Period = 50ms | Damping Ratio = 0,65 <br/> Period = 60ms |
| Scale/Size | Damping Ratio = 0,7 <br/> Period = 50ms | Damping Ratio = 0,8 <br/> Period = 50ms | Damping Ratio = 0,6 <br/> Period = 60ms |

Une fois que vous avez défini les propriétés, vous pouvez ensuite transmettre votre NaturalMotionAnimation d'effet ressort à la méthode StartAnimation d’un CompositionObject ou à la propriété Motion d’un InertiaModifier d'InteractionTracker.

## <a name="example"></a>Exemple

Dans cet exemple, vous créez une expérience d’interface utilisateur de navigation et de canevas dans lequel, lorsque l’utilisateur clique sur un bouton de développement, un volet de navigation s'anime avec un mouvement d'oscillation élastique.

![Animation à effet ressort lancée par clic](images/animation/spring-animation-on-click.gif)

Commencez par définir l’animation à effet ressort dans l’événement de clic à l'apparition du volet de navigation. Ensuite, vous définissez les propriétés de l’animation, à l'aide de la fonctionnalité InitialValueExpression afin d'utiliser une Expression pour définir la FinalValue. Vous assurez également le suivi pour déterminer si le volet est ouvert ou non et, lorsque vous êtes prêt, vous démarrez l’animation.

```csharp
private void Button_Clicked(object sender, RoutedEventArgs e)
{
 _springAnimation = _compositor.CreateSpringScalarAnimation();
 _springAnimation.DampingRatio = 0.75f;
 _springAnimation.Period = TimeSpan.FromSeconds(0.5);

 if (!_expanded)
 {
 _expanded = true;
 _propSet.InsertBoolean("expanded", true);
 _springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue + 250”;
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue - 250”;
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

Maintenant que se passe-t-il si vous souhaitez lier ce mouvement à une entrée? Vous voulez faire en sorte que si l’utilisateur fait un balayage, les volets apparaissent avec un mouvement de ressort? Plus important encore, si l’utilisateur effectue un balayage plus fort ou plus rapide, le mouvement s’adapte à la vitesse de l’utilisateur final.

![Animation à effet ressort lancée par balayage](images/animation/spring-animation-on-swipe.gif)

Pour ce faire, vous pouvez utiliser la même animation à effet ressort et la transmettre à un InertiaModifier avec InteractionTracker. Pour plus d’informations sur les InputAnimations et l'InteractionTracker, voir [Expériences personnalisées de manipulation avec InteractionTracker](interaction-tracker-manipulations.md). Nous supposons que dans cet exemple de code, vous avez déjà configuré votre InteractionTracker et votre VisualInteractionSource. Nous allons nous concentrer sur la création des InertiaModifiers qui vont intégrer une NaturalMotionAnimation, dans ce cas, un effet ressort.

```csharp
// InteractionTracker and the VisualInteractionSource previously setup
// The open and close ScalarSpringAnimations defined earlier
private void SetupInput()
{
 // Define the InertiaModifier to manage the open motion
 var openMotionModifer = InteractionTrackerInertiaNaturalMotion.Create(compositor);

 // Condition defines to use open animation if panes in non-expanded view
 // Property set value to track if open or closed is managed in other part of code
 openMotionModifer.Condition = _compositor.CreateExpressionAnimation(
"propset.expanded == false");
 openMotionModifer.Condition.SetReferenceParameter("propSet", _propSet);
 openMotionModifer.NaturalMotion = _openSpringAnimation;

 // Define the InertiaModifer to manage the close motion
 var closeMotionModifier = InteractionTrackerInertiaNaturalMotion.Create(_compositor);

 // Condition defines to use close animation if panes in expanded view
 // Property set value to track if open or closed is managed in other part of code
 closeMotionModifier.Condition = 
_compositor.CreateExpressionAnimation("propSet.expanded == true");
 closeMotionModifier.Condition.SetReferenceParameter("propSet", _propSet);
 closeMotionModifier.NaturalMotion = _closeSpringAnimation;

 _tracker.ConfigurePositionXInertiaModifiers(new 
InteractionTrackerInertiaNaturalMotion[] { openMotionModifer, closeMotionModifier});

 // Take output of InteractionTracker and assign to the pane
 var exp = _compositor.CreateExpressionAnimation("-tracker.Position.X");
 exp.SetReferenceParameter("tracker", _tracker);
 ElementCompositionPreview.GetElementVisual(pageNavigation).
StartAnimation("Translation.X", exp);
}
```

Vous disposez désormais d'une animation à effet ressort à la fois pilotée par une entrée et par programmation dans votre interface utilisateur!

En résumé, voici les étapes nécessaires pour utiliser une animation à effet ressort dans votre application:

1. Créez votre SpringAnimation à partir de votre compositeur.
1. Définissez les propriétés de la SpringAnimation si vous souhaitez d'autres valeurs que celles par défaut:
    - DampingRatio
    - Période
    - Valeur de fin
    - Valeur initiale
    - Rapidité initiale
1. Affectez à la cible.
    - Si vous animez une propriété CompositionObject, transmettez SpringAnimation en tant que paramètre à StartAnimation.
    - Si vous souhaitez une utilisation avec une entrée, affectez la propriété NaturalMotion d'un InertiaModifier à SpringAnimation.

