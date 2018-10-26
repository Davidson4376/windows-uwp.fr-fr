---
author: jwmsft
title: Animations basées sur le pointeur
description: Découvrez comment utiliser la position du pointeur pour créer des expériences dynamiques «qui collent au curseur».
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: b69899761e1c4a139fd2b15d6810440df5192487
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5598339"
---
# <a name="pointer-based-animations"></a>Animations basées sur le pointeur

Cet article montre comment utiliser la position du pointeur pour créer des expériences dynamiques «qui collent au curseur».

## <a name="prerequisites"></a>Éléments prérequis

À ce stade, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants:

- [Animations pilotées par une entrée](input-driven-animations.md)
- [Animations basées sur une relation](relation-animations.md)

## <a name="why-create-pointer-position-driven-experiences"></a>Pourquoi créer des expériences basées sur la position du pointeur?

Dans le langage de conception Fluent, l'interaction tactile n’est pas la seule façon d’interagir avec l’interface utilisateur. Comme UWP couvre plusieurs facteurs de forme de périphérique, les utilisateurs finaux interagissent avec les applications avec d’autres modalités d’entrée, comme la souris et le stylet. L'utilisation des données de position de ces autres modalités d’entrée fournit une opportunité pour que les utilisateurs finaux se sentent encore plus connectés avec votre application.

Les expériences basées sur la position du pointeur permettent de tirer parti de la position sur l'écran d’une modalité d’entrée de pointeur pour créer des mouvements et des expériences d’interface utilisateur supplémentaires pour votre application. Ces expériences peuvent souvent fournir un contexte et un retour d'information supplémentaire aux utilisateurs finaux sur le comportement et la structure de l’interface utilisateur. L’expérience n’est plus un flux à sens unique, mais devient plutôt un flux bidirectionnel où l’utilisateur final fournit une entrée avec sa modalité d’entrée et l’interface utilisateur de l’application peut y réagir.

Voici quelques exemples:

- Animation de la position d’un projecteur pour suivre le curseur

    ![Exemple de projecteur de pointeur](images/animation/spotlight-reveal.gif)

- Rotation d’une image basée sur la position du pointeur

    ![Exemple de rotation de pointeur](images/animation/pointer-rotate.gif)

## <a name="using-pointerpositionpropertyset"></a>Utilisation de PointerPositionPropertySet

Vous pouvez créer ces expériences à l’aide du PointerPositionPropertySet. Ce PropertySet est créé pour un élément UIElement pour maintenir la position du pointeur pendant que l’élément UIElement est soumis à un test de positionnement positif. La valeur de la position est relative à l’espace de coordonnées de l’élément UIElement (une position <0,0> correspond au coin supérieur gauche de l’élément UIElement). Vous pouvez exploiter cette propriété définie dans une Animation pour piloter le mouvement d’une autre propriété.

Pour chacune des modalités d’entrée de pointeur différentes, il existe un certain nombre d'états possibles de l’entrée lorsque la position change: Pointage, Appuyé, Appuyé et déplacé. Le PointerPositionPropertySet ne conserve la position du pointeur que dans les états Pointage, Appuyé et Appuyé et déplacé de la souris et du stylet.

Procédure générale de prise en main:

1. Identifiez l’élément UIElement dans lequel vous souhaitez que la position du pointeur soit suivie.
1. Accédez au PointerPositionPropertySet via ElementCompositionPreview.
    - Transmettez l'élément UIElement dans la méthode ElementCompositionPreview.GetPointerPositionPropertySet.
1. Créez une ExpressionAnimation qui fait référence à la propriété Position du PropertySet.
    - N’oubliez pas de définir votre paramètre de référence!
1. Ciblez une propriété de CompositionObject avec l'ExpressionAnimation.

> [!NOTE]
> Il est recommandé d’attribuer le PropertySet renvoyé à partir de la méthode GetPointerPositionPropertySet à une variable de classe. Cela garantit que le jeu de propriétés ne soit pas nettoyé par le nettoyage de la mémoire et n'a donc aucun effet sur l'ExpressionAnimation dans lequel il est référencé. Les ExpressionAnimations ne conservent pas une référence forte à l’un des objets utilisés dans l’équation.

## <a name="example"></a>Exemple

Examinons un exemple où nous tirons parti de la position de pointage d'une modalité d’entrée de souris et stylet pour faire pivoter dynamiquement une image.

![Exemple de rotation de pointeur](images/animation/pointer-rotate.gif)

L’image est un élément UIElement, donc nous allons tout d’abord obtenir une référence au PointerPositionPropertySet

```csharp
_pointerPositionPropSet = ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element);
```

Dans cet exemple, deux Expressions sont en œuvre:

- Une Expression où l’image pivote en fonction de la distance du pointeur par rapport au centre de l’image. Plus la distance est grande, plus la rotation est forte.
- Une Expression où l’axe de rotation change en fonction de la position du pointeur. Vous voulez que l'axe de rotation soit perpendiculaire au vecteur de la position.

Nous allons définir les deux Expressions, une qui cible la propriété RotationAngle et l’autre, la propriété RotationAxis. Vous référencez le PointerPositionPropertySet comme n’importe quel autre PropertySet.

Dans cet exemple, vous créez des Expressions à l'aide des classes ExpressionBuilder.

```csharp
// || DEFINE THE EXPRESSION FOR THE ROTATION ANGLE ||
var hoverPosition = _pointerPositionPropSet.GetSpecializedReference
<PointerPositionPropertySetReferenceNode>().Position;
var angleExpressionNode =
EF.Conditional(
 hoverPosition == new Vector3(0, 0, 0),
 ExpressionValues.CurrentValue.CreateScalarCurrentValue(),
 35 * ((EF.Clamp(EF.Distance(center, hoverPosition), 0, distanceToCenter) % distanceToCenter) / distanceToCenter));
_tiltVisual.StartAnimation("RotationAngleInDegrees", angleExpressionNode);

// || DEFINE THE EXPRESSION FOR THE ROTATION AXIS ||
var axisAngleExpressionNode = EF.Vector3(
-(hoverPosition.Y - center.Y) * EF.Conditional(hoverPosition.Y == center.Y, 0, 1),
 (hoverPosition.X - center.X) * EF.Conditional(hoverPosition.X == center.X, 0, 1),
 0);
_tiltVisual.StartAnimation("RotationAxis", axisAngleExpressionNode);
```