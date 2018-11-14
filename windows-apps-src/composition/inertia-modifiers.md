---
author: jwmsft
title: Utiliser des modificateurs d'inertie pour créer des points d’ancrage
description: Découvrez comment utiliser la fonctionnalité InertiaModifier d’un InteractionTracker pour créer des expériences de mouvement ancrées sur un point donné.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows10, uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 20c10b1cc621da834a8a7c411e75eb92b1944b5a
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6465007"
---
# <a name="create-snap-points-with-inertia-modifiers"></a>Créer des points d’ancrage à l'aide de modificateurs d'inertie

Dans cet article, nous expliquons plus en détail comment utiliser la fonctionnalité InertiaModifier d’un InteractionTracker pour créer des expériences de mouvement ancrées sur un point donné.

## <a name="prerequisites"></a>Éléments prérequis

À ce stade, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants:

- [Animations pilotées par une entrée](input-driven-animations.md)
- [Expériences personnalisées de manipulation avec InteractionTracker](interaction-tracker-manipulations.md)
- [Animations basées sur une relation](relation-animations.md)

## <a name="what-are-snap-points-and-why-are-they-useful"></a>Quels sont les points d’ancrage et pourquoi sont-ils utiles?

Lorsque vous créez des expériences personnalisées de manipulation, il est parfois utile de créer des _points de position_ spécialisés dans le canevas avec zoom/défilement, auxquels l'InteractionTracker s'arrêtera toujours. Ils sont souvent appelés _points d’ancrage_.

Dans l’exemple suivant, vous remarquez que le défilement peut laisser l’interface utilisateur dans une position incorrecte entre les différentes images:

![Défilement sans points d’ancrage](images/animation/snap-points-none.gif)

Si vous ajoutez des points d’ancrage, lorsque vous arrêtez le défilement entre les images, elles «s'ancrent» à une position spécifiée. Avec des points d’ancrage, l’expérience de défilement des images est plus propre et plus réactive.

![Défilement avec un point d’ancrage unique](images/animation/snap-points-single.gif)

## <a name="interactiontracker-and-inertiamodifiers"></a>InteractionTracker et InertiaModifiers

Lors de la création d’expériences personnalisées de manipulation avec InteractionTracker, vous pouvez créer des expériences de mouvement avec points d'ancrage à l'aide d'InertiaModifiers. Les InertiaModifiers sont essentiellement un moyen de définir où et comment un InteractionTracker atteint sa destination lorsqu'il entre en état d'inertie. Vous pouvez appliquer des InertiaModifiers pour affecter la position X ou Y ou les propriétés d'échelle d’InteractionTracker.

Il existe 3 types d'InertiaModifiers:

- InteractionTrackerInertiaRestingValue: moyen de modifier la position d'arrêt finale après une interaction ou une vitesse par programmation. Un mouvement prédéfini amènera InteractionTracker à cette position.
- InteractionTrackerInertiaMotion: moyen de définir un mouvement spécifique qu'InteractionTracker effectuera après une interaction ou une vitesse par programmation. La position finale est dérivée de ce mouvement.
- InteractionTrackerInertiaNaturalMotion: moyen de définir la position d'arrêt finale après une interaction ou une vitesse par programmation, mais avec une animation basée sur la physique (NaturalMotionAnimation).

Lorsque vous entrez en inertie, InteractionTracker évalue chacun des InertiaModifiers qui lui est affecté et détermine si l'un d’eux s’applique. Cela signifie que vous pouvez créer et affecter plusieurs InertiaModifiers à un InteractionTracker, mais, lorsque vous définissez chacun, vous devez effectuer les opérations suivantes:

1. Définir la condition: Expression qui définit l’instruction conditionnelle du moment auquel cet InertiaModifier spécifique doit avoir lieu. Cela nécessite souvent de rechercher la NaturalRestingPosition de l'InteractionTracker (inertie par défaut donnée de destination).
1. Définir le RestingValue/Motion/NaturalMotion: définir l’Expression de valeur d'arrêt réelle, l’Expression de mouvement ou NaturalMotionAnimation qui a lieu lorsque la condition est remplie.

> [!NOTE]
> Les aspects de condition des InertiaModifiers sont évalués une seule fois lorsqu’InteractionTracker entre en inertie. Toutefois, seulement pour InertiaMotion, l'Expression de mouvement est évaluée à chaque image pour le modificateur dont la condition est définie sur true.

## <a name="example"></a>Exemple

Maintenant examinons comment vous pouvez utiliser des InertiaModifiers pour créer quelques expériences de point d'ancrage afin de recréer le canevas de défilement d’images. Dans cet exemple, chaque manipulation est censée potentiellement se déplacer dans une image unique: cela est souvent appelé un point d’ancrage obligatoire unique.

Commençons par configurer InteractionTracker, la VisualInteractionSource et l’Expression qui va tirer profit de la position de l'InteractionTracker.

```csharp
private void SetupInput()
{
    _tracker = InteractionTracker.Create(_compositor);
    _tracker.MinPosition = new Vector3(0f);
    _tracker.MaxPosition = new Vector3(3000f);

    _source = VisualInteractionSource.Create(_root);
    _source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    _source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
    _tracker.InteractionSources.Add(_source);

    var scrollExp = _compositor.CreateExpressionAnimation("-tracker.Position.Y");
    scrollExp.SetReferenceParameter("tracker", _tracker);
    ElementCompositionPreview.GetElementVisual(scrollPanel).StartAnimation("Offset.Y", scrollExp);
}
```

Ensuite, étant donné que le comportement d'un point d’ancrage obligatoire unique déplace le contenu vers le haut ou vers le bas, vous avez besoin de deux modificateurs d'inertie différents: un qui déplace le contenu de défilement vers le haut et un qui le déplace vers le bas.

```csharp
// Snap-Point to move the content up
var snapUpModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
// Snap-Point to move the content down
var snapDownModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
```

L'ancrage vers le haut ou vers le bas est déterminé par l'endroit où l'InteractionTracker atterrirait naturellement par rapport à la distance d'ancrage, la distance entre les emplacements d'ancrage. S'il dépasse le point situé à mi-course, ancrez vers le bas, sinon ancrez vers le haut. (Dans cet exemple, vous enregistrez la distance d'ancrage dans un PropertySet)

```csharp
// Is NaturalRestingPosition less than the halfway point between Snap Points?
snapUpModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y < (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapUpModifier.Condition.SetReferenceParameter("prop", _propSet);
// Is NaturalRestingPosition greater than the halfway point between Snap Points?
snapDownModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y >= (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapDownModifier.Condition.SetReferenceParameter("prop", _propSet);
```

Ce schéma donne une description visuelle de la logique utilisée:

![Schéma de modificateur d'inertie](images/animation/inertia-modifier-diagram.png)

Maintenant, il ne vous reste plus qu'à définir les Valeurs d'arrêt de chaque InertiaModifier: déplacez la position de l'InteractionTracker vers la position d'ancrage précédente ou vers la suivante.

```csharp
snapUpModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue - mod(this.StartingValue, prop.snapDistance)");
snapUpModifier.RestingValue.SetReferenceParameter("prop", _propSet);
snapForwardModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue + prop.snapDistance - mod(this.StartingValue, ” + “prop.snapDistance)");
snapForwardModifier.RestingValue.SetReferenceParameter("prop", _propSet);
```

Enfin, ajoutez les InertiaModifiers à l'InteractionTracker. Désormais, quand InteractionTracker entre en InertiaState, il vérifie les conditions de vos InertiaModifiers pour voir si sa position doit être modifiée.

```csharp
var modifiers = new InteractionTrackerInertiaRestingValue[] { 
snapUpModifier, snapDownModifier };
_tracker.ConfigurePositionYInertiaModifiers(modifiers);
```