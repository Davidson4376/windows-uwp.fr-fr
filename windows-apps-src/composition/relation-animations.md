---
author: jwmsft
title: Animations basées sur une relation
description: Créer un mouvement basé sur une propriété sur un autre objet.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: Windows10, uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: cde3868d1a554396bfda7c13ea0c71bd037416bc
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7307419"
---
# <a name="relation-based-animations"></a>Animations basées sur une relation

Cet article fournit une vue d’ensemble sur la réalisation d'animations basées sur une relation à l’aide de Composition ExpressionAnimations.

## <a name="dynamic-relation-based-experiences"></a>Expériences dynamiques basées sur une relation

Lorsque vous créez des expériences de mouvement dans une application, il arrive parfois que le mouvement ne soit pas basé sur le temps, mais plutôt dépendant d'une propriété sur un autre objet. Les KeyFrameAnimations ne parviennent pas à exprimer très facilement ces types d’expériences de mouvement. Dans ces cas spécifiques, le mouvement n’a plus besoin d’être discret et prédéfini. Au lieu de cela, le mouvement peut s’adapter dynamiquement en fonction de sa relation avec d’autres propriétés de l’objet. Par exemple, vous pouvez animer l’opacité d’un objet en fonction de sa position horizontale. Autres exemples d'expériences de mouvement: les en-têtes rémanents et l’effet parallaxe.

Ces types d’expériences de mouvement vous permettent de créer une interface utilisateur qui semble plus connectée, au lieu de paraître singulière et indépendante. Pour l'utilisateur, cela donne l’impression d’une expérience d’interface utilisateur dynamique.

![Cercle en orbite](images/animation/orbit.gif)

![Affichage liste avec parallaxe](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>Utilisation d'ExpressionAnimations

Pour créer des expériences de mouvement basées sur la relation, vous utilisez le type ExpressionAnimation. Les ExpressionAnimations (ou Expressions pour faire court), sont un nouveau type d’animation qui vous permet d’exprimer une relation mathématique: une relation que le système utilise pour calculer la valeur d’une propriété d’animation à chaque image. Autrement dit, les Expressions sont simplement une équation mathématique qui définit la valeur souhaitée d’une propriété d’animation par trame. Les Expressions sont un composant très polyvalent qui peut être utilisé sur un large éventail de scénarios, notamment les exemples suivants:

- Taille relative, animations de décalage.
- En-têtes rémanents, effet de parallaxe avec ScrollViewer. (Voir [Améliorer les expériences ScrollViewer existantes](scroll-input-animations.md).)
- Points d'ancrage avec InertiaModifiers et InteractionTracker. (Voir [Créer des points d’ancrage à l'aide de modificateurs d'inertie](inertia-modifiers.md).)

Lorsque vous travaillez avec des ExpressionAnimations, il est important de connaître à l'avance ces quelques points:

- Sans fin: contrairement à leur équivalent KeyFrameAnimation, les Expressions n’ont pas de durée limitée. Comme les Expressions sont des relations mathématiques, ce sont des animations qui sont constamment «en cours d'exécution». Vous avez la possibilité d’arrêter ces animations si vous le souhaitez.
- En exécution, mais pas toujours en évaluation: les performances sont toujours un problème avec des animations qui s’exécutent en permanence. Pas d'inquiétude, cependant. Le système est suffisamment intelligent pour que l’Expression se réévalue uniquement en cas de changement d'une de ses entrées ou d'un de ces paramètres.
- Résolution dans le bon type d’objet: comme les Expressions sont des relations mathématiques, il est important de s’assurer que l’équation qui définit l’Expression se résout dans le même type que la propriété ciblée par l’animation. Par exemple, si vous animez Offset, votre Expression doit se résoudre en un type de valeur Vector3.

### <a name="components-of-an-expression"></a>Composants d'une Expression

Lorsque vous créez la relation mathématique d’une Expression, il existe plusieurs composants principaux:

- Paramètres: valeurs représentant des valeurs de constantes ou des références à d’autres objets de Composition.
- Opérateurs mathématiques: opérateurs mathématiques standard plus (+), moins (-), multiplier (*), diviser (/) qui lient des paramètres ensemble pour former une équation. Vous trouverez également des opérateurs conditionnels comme supérieur à (>), égal (==), opérateur ternaire (condition? ifTrue: ifFalse), etc.
- Fonctions mathématiques: fonctions/raccourcis mathématiques basés sur System.Numerics. Pour obtenir une liste complète des fonctions prises en charge, voir [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation).

Les Expressions prennent également en charge un ensemble de mots clés: expressions spéciales qui ont une signification distincte uniquement dans le système ExpressionAnimation. Ils sont répertoriés (ainsi que la liste complète des fonctions mathématiques) dans la documentation [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation).

### <a name="creating-expressions-with-expressionbuilder"></a>Création d’Expressions avec ExpressionBuilder

Il existe deux options de création d’Expressions dans les applications UWP:

1. Création de l’équation en tant que chaîne via l’API officielle, publique.
1. Création de l’équation dans un modèle d’objet de type sécurisé via l’outil open source ExpressionBuilder. Consultez [Source et documentation Github](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/ExpressionBuilder).

Dans le cadre de ce document, nous allons définir nos Expressions à l’aide d'ExpressionBuilder.

### <a name="parameters"></a>Paramètres

Les paramètres constituent le cœur d’une Expression. Il existe deux types de paramètres:

- Constantes: il s’agit de paramètres qui représentent les variables System.Numeric typées. Des valeurs sont attribuées à ces paramètres une fois au démarrage de l’animation.
- Références: il s’agit des paramètres qui représentent des références aux CompositionObjects: les valeurs de ces paramètres sont mises à jour en permanence après le démarrage d'une animation.

En général, les Références sont l’élément essentiel pour déterminer le changement dynamique de la sortie d’une Expression. Comme ces références changent, la sortie de l’Expression change en conséquence. Si vous créez votre Expression avec des chaînes ou les utilisez dans un scénario de création de modèles (en utilisant votre Expression pour cibler plusieurs CompositionObjects), vous devez nommer et définir les valeurs de vos paramètres. Voir la section Exemple pour plus d'informations.

### <a name="working-with-keyframeanimations"></a>Utilisation de KeyFrameAnimations

Les Expressions peuvent également être utilisées avec des KeyFrameAnimations. Dans ce cas, vous pouvez utiliser une Expression pour définir la valeur d’une image clé à un point temporel: ces types d'images clés sont appelées ExpressionKeyFrames.

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

Toutefois, contrairement aux ExpressionAnimations, les ExpressionKeyFrames sont évaluées une seule fois au démarrage de la KeyFrameAnimation. Gardez à l’esprit que vous ne transmettez pas un élément ExpressionAnimation comme valeur de l'image clé, plutôt une chaîne (ou un ExpressionNode, si vous utilisez ExpressionBuilder).

## <a name="example"></a>Exemple

Examinons maintenant un exemple d’utilisation d’Expressions, plus précisément l’exemple PropertySet de la galerie d’exemples de l’interface utilisateur Windows. Nous allons étudier l’Expression qui gère le comportement de mouvement en orbite de la bille bleue.

![Cercle en orbite](images/animation/orbit.gif)

Trois composants sont en œuvre dans l’expérience totale:

1. Une KeyFrameAnimation qui anime le décalage Y de la bille rouge.
1. Une classe PropertySet avec une propriété **Rotation** qui permet de piloter l’orbite, animée par une autre KeyFrameAnimation.
1. Un élément ExpressionAnimation qui pilote le décalage de la bille bleue en référençant le décalage de la bille rouge et la propriété de Rotation pour conserver un orbite parfait.

Nous allons nous concentrer sur l'ExpressionAnimation définie dans le 3e point. Nous avons également utiliser les classes ExpressionBuilder pour construire cette Expression. Une copie du code utilisé pour créer cette expérience via des chaînes est répertorié à la fin.

Dans cette équation, il existe deux propriétés, que vous devez référencer à partir de la classe PropertySet: l'une est un décalage de point central et l’autre est la rotation.

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

Ensuite, vous devez définir le composant Vector3 qui tient compte de la rotation en orbite réelle.

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` est une notation «using» abrégée pour définir ExpressionBuilder.ExpressionFunctions.

Enfin, combinez ces composants et référencez la position de la bille rouge pour définir la relation mathématique.

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

Dans une situation hypothétique, que se passe-t-il si vous souhaitez utiliser cette même Expression, mais avec deux autres objets visuels, c'est-a-dire 2ensembles de cercles en orbite. Avec des CompositionAnimations, vous pouvez réutiliser l’animation et cibler plusieurs CompositionObjects. La seule chose que vous devez modifier lorsque vous utilisez cette Expression dans le cas d'une orbite supplémentaire est la référence à l’élément visuel. Nous appelons cela une création de modèles.

Dans ce cas, vous modifiez l’Expression que vous avez créée précédemment. Au lieu d’«obtenir» une référence au CompositionObject, vous créez une référence avec un nom et ensuite, vous affectez des valeurs différentes:

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

Voici le code si vous avez défini l’Expression avec des chaînes au moyen de l’API publique.

```
ExpressionAnimation expressionAnimation =
compositor.CreateExpressionAnimation("visual.Offset + " +
"propertySet.CenterPointOffset + " +
"Vector3(cos(ToRadians(propertySet.Rotation)) * 150," + "sin(ToRadians(propertySet.Rotation)) * 75, 0)");
 var propSetCenterPoint = _propertySet.GetReference().GetVector3Property("CenterPointOffset");
 var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
expressionAnimation.SetReferenceParameter("propertySet", _propertySet);
expressionAnimation.SetReferenceParameter("visual", redSprite);
```
