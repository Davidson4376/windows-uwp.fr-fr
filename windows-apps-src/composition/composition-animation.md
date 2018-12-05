---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animations de composition
description: De nombreuses propriétés d’objet et d’effet de composition peuvent être animées à l’aide d’animations par images clés et expressions, ce qui permet aux propriétés d’un élément d’interface utilisateur de changer dans le temps ou en fonction d’un calcul.
ms.date: 10/10/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b94f14b32c5dd74e0aefb9b9a99f64bbd905a05d
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8710185"
---
# <a name="composition-animations"></a>Animations de composition

Les API Windows.UI.Composition vous permettent de créer, d’animer, de transformer et de manipuler des objets compositeur dans une couche API unifiée. Les animations de composition offrent un moyen puissant et efficace d’exécuter des animations dans l’interface utilisateur de votre application. Elles ont été entièrement conçues pour garantir l’exécution de vos animations à 60FPS, indépendamment du thread d’interface utilisateur, et pour vous offrir la possibilité de créer des expériences totalement inédites en usant de nombreuses propriétés et entrées pour produire les animations en question.

## <a name="motion-in-windows"></a>Mouvement dans Windows

Imaginez la conception du mouvement comme un film. Les transitions transparentes vous laissent concentrés sur l’histoire et donnent vie aux expériences. Nous pouvons inviter cette sensation dans nos conceptions, en guidant les utilisateurs à partir d’une tâche à l’autre avec une aisance cinématographique. Le mouvement est souvent le facteur de différenciation entre une Interface utilisateur et une expérience utilisateur.

En tant qu’un bloc de construction fondamental de la plateforme d’interface utilisateur de Windows, des CompositionAnimations offrent un moyen puissant et efficace pour créer des expériences de mouvement dans l’interface utilisateur de votre application. Le moteur d’animation a été conçu dès le départ pour vous assurer que votre animation s’exécute à 60 FPS, indépendamment du thread d’interface utilisateur. Ces animations sont conçues pour fournir la flexibilité nécessaire pour créer des expériences de mouvement innovante basées sur le temps, l’entrée et d’autres propriétés.

### <a name="examples-of-motion"></a>Exemples de mouvement

Voici quelques exemples de mouvement dans une application.

Ici, une application utilise une animation connectée pour animer une image qui «perdure» pour faire partie de l’en-tête de la page suivante. L’effet aide à conserver le contexte de l’utilisateur pendant toute la transition.

![Un exemple d’Animation connectée](images/animation/connected-animation-example.gif)

Ici, un effet parallaxe visuel déplace les objets à différentes vitesses lorsque vous faites défiler ou que vous développez l'interface utilisateur, afin de créer un effet de profondeur, de perspective et de mouvement.

![Exemple de parallaxe avec une liste et une image d’arrière-plan](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>À l’aide des CompositionAnimations pour créer un mouvement

Pour générer le mouvement dans l’interface utilisateur, les développeurs peuvent accéder à des animations en XAML (lien vers les tables de montage séquentiel ici) ou la couche visuelle. Animations sur la couche visuelle fournissent aux développeurs une série d’avantages:

- Performances – au lieu de l’animation liée au Thread d’interface utilisateur classique, animations sur la plateforme d’interface utilisateur Windows fonctionnent sur un thread indépendant à 60 FPS, l’activation des expériences de mouvement fluide.
- Modèle de création de modèles – animations de la couche interface utilisateur Windows sont des modèles, signification peut utiliser une animation unique sur plusieurs objets et modifier les propriétés ou paramètres sans avoir besoin de gêner précédent utilise.
- Personnalisation: la couche interface utilisateur Windows non seulement facilite rendre l’interface utilisateur magnifique, mais à toute une gamme de types d’animation, possible afin de créer de nouveaux et des expériences avec un dégradé de personnalisations

En tant que développeur création d’expériences au niveau de la couche interface utilisateur Windows, vous avez accès à une variété de concepts d’animation pour donner vie vos conceptions. Vous pouvez utiliser une de ces concepts pour animer une propriété ou de la sous-chaîne composant (le cas échéant) de n’importe quel CompositionObject.

> [!NOTE]
> Pas toutes les propriétés d’un CompositionObject peuvent être animées. Reportez-vous à la documentation du CompositionObject individuel pour déterminer si une propriété est animée.

> [!NOTE]
> Le terme _sous-canal_ fait référence à une forme de composant d’une propriété. Par exemple, l’axe X, ou en mode XY sous-chaîne d’une propriété de décalage Vector3.

| Concept d’animation | Description |
| ----------------- | ----------- |
| [Basées sur le temps de mouvement avec des KeyFrameAnimations](time-animations.md)  | KeyFrameAnimations permettent de contrôler directement la totalité d’une expérience de mouvement sur une période de temps. Développeurs décrivant d’un mouvement début, fin, interpolation entre les deux et durée de manière images clés traditionnelles. |
| [Mouvement relatif avec ExpressionAnimations](relation-animations.md)  | Les ExpressionAnimations sont utilisées pour décrire la façon dont un mouvement de propriété d’un objet doit soit piloté par rapport à la propriété d’un autre objet. Les développeurs définissent une équation mathématique qui définit la relation basée sur la référence. |
| ImplicitAnimations | Ces animations sont basés sur le déclencheur et sont définies séparément à partir de la logique d’application. ImplicitAnimations sont utilisées pour décrire quand et comment les animations de se produisent en réponse aux changements de propriété direct. |
| [Mouvement pilotées par l’entrée avec des Animations d’entrée](input-driven-animations.md)  | Animations d’entrée décrit un ensemble de scénarios qui permettent aux développeurs décrire un mouvement de basée sur la manipulation via l’interaction tactile ou d’autres modalités d’entrée. Ces animations sont pilotées en fonction sur l’entrée utilisateur active ou des mouvements. |
| [Basée sur la physique de mouvement avec NaturalMotionAnimations](natural-animations.md)  | Les NaturalMotionAnimations sont utilisées pour décrire un mouvement naturel et familier expériences basées sur le monde réel forcer le mouvement piloté. Au lieu de la définition de temps, les développeurs définissent les caractéristiques du mouvement (par exemple, damping ratio pour les effets ressort) |