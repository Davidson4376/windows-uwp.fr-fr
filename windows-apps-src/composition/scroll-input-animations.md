---
title: Améliorer les expériences ScrollViewer existantes
description: Découvrez comment utiliser un ScrollViewer XAML et des ExpressionAnimations pour créer des expériences dynamiques de mouvement piloté par une entrée.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 25b0732b7c29653d18f0e018698ab4b6398d402a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318074"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>Améliorer les expériences ScrollViewer existantes

Cet article explique comment utiliser un ScrollViewer XAML et des ExpressionAnimations pour créer des expériences dynamiques de mouvement piloté par une entrée.

## <a name="prerequisites"></a>Prérequis

À ce stade, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants :

- [Animations contrôlée par l’entrée](input-driven-animations.md)
- [Animations en fonction de relation](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>Pourquoi se baser sur ScrollViewer ?

En règle générale, vous utilisez le ScrollViewer XAML existant pour créer une surface de zoom et de défilement pour le contenu de votre application. Avec l’introduction du langage Fluent Design, vous devez désormais réfléchir également à la manière d'utiliser une action de défilement ou de zoom sur une surface pour piloter d'autres expériences de mouvement. Par exemple, utiliser le défilement pour entraîner une animation de flou d’un arrière-plan ou pour déterminer la position d’un « en-tête rémanent ».

Dans ces scénarios, vous exploitez des expériences de comportement ou de manipulation comme le défilement et le zoom pour rendre d'autres parties de votre application plus dynamiques. Celles-ci rendent l’application plus harmonieuse et les expériences plus faciles à retenir pour les utilisateurs finaux. En rendant l’interface utilisateur de l'application plus mémorable, vous incitez les utilisateurs finaux à utiliser l’application plus souvent et pour de plus longues périodes.

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>Que pouvez-vous réaliser en vous basant sur ScrollViewer ?

Vous pouvez exploiter la position d’un élément ScrollViewer pour générer un certain nombre d’expériences dynamiques :

- Parallaxe : utilisez la position d’un élément ScrollViewer pour déplacer le contenu à l'arrière-plan ou au premier plan à une distance relative à la position de défilement.
- StickyHeaders : utilisez la position d’un élément ScrollViewer pour animer et « fixer » un en-tête à une position
- Effets pilotés par une entrée : utilisez la position d’un élément Scrollviewer pour animer un effet de composition, par exemple un flou.

En règle générale, en faisant référence à la position d’un élément ScrollViewer avec un élément ExpressionAnimation, vous pouvez créer une animation qui change de façon dynamique relativement à la quantité de défilement.

![Affichage liste avec parallaxe](images/animation/parallax.gif)

![En-tête discret](images/animation/shy-header.gif)

## <a name="using-scrollmanipulationpropertyset"></a>Utilisation de ScrollManipulationPropertySet

Pour créer ces expériences dynamiques à l’aide d’un élément ScrollViewer XAML, vous devez être en mesure de référencer la position de défilement dans une animation. Pour ce faire, vous devez accéder à un CompositionPropertySet à partir d'un ScrollViewer XAML appelé le ScrollManipulationPropertySet.
Le ScrollManipulationPropertySet contient une seule propriété Vector3 appelée Traduction qui fournit l’accès à la position de défilement de l’élément ScrollViewer. Vous pouvez ensuite le référencer comme n’importe quel autre CompositionPropertySet dans votre ExpressionAnimation.

Procédure générale de prise en main :

1. Accédez au ScrollManipulationPropertySet via ElementCompositionPreview.
    - `ElementCompositionPreview.GetScrollManipulationPropertySet(ScrollViewer scroller)`
1. Créez une ExpressionAnimation qui fait référence à la propriété Traduction du PropertySet.
    - N’oubliez pas de définir le paramètre Référence !
1. Ciblez une propriété de CompositionObject avec l'ExpressionAnimation.

> [!NOTE]
> Il est recommandé d’attribuer le PropertySet renvoyé à partir de la méthode GetScrollManipulationPropertySet à une variable de classe. Cela garantit que le jeu de propriétés ne soit pas nettoyé par le nettoyage de la mémoire et n'a donc aucun effet sur l'ExpressionAnimation dans lequel il est référencé. Les ExpressionAnimations ne conservent pas une référence forte à l’un des objets utilisés dans l’équation.

## <a name="example"></a>Exemple

Examinons comment l’exemple de parallaxe ci-dessus est élaboré. Pour référence, tout le code source de l’application se trouve dans le [référentiel Windows UI Dev Labs sur GitHub](https://github.com/microsoft/WindowsCompositionSamples).

La première étape consiste à obtenir une référence au ScrollManipulationPropertySet.

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

L’étape suivante consiste à créer l'ExpressionAnimation qui définit une équation qui utilise la Position de défilement de l’élément ScrollViewer.

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> Vous pouvez également utiliser des classes d’assistance ExpressionBuilder pour créer la même Expression sans avoir besoin de chaînes :

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

Enfin, vous prenez cette ExpressionAnimation et vous ciblez l’élément visuel pour lequel vous souhaitez un effet de parallaxe. Dans ce cas, il s'agit de l’image de chacun des éléments de la liste.

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```