---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animations de composition
description: De nombreuses propriétés d’objet et d’effet de composition peuvent être animées à l’aide d’animations par images clés et expressions, ce qui permet aux propriétés d’un élément d’interface utilisateur de changer dans le temps ou en fonction d’un calcul.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18208986d7d07e4d437e52dce844deecc03cf1f6
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240027"
---
# <a name="composition-animations"></a>Animations de composition

Les API Windows.UI.Composition vous permettent de créer, d’animer, de transformer et de manipuler des objets compositeur dans une couche API unifiée. Les animations de composition offrent un moyen puissant et efficace d’exécuter des animations dans l’interface utilisateur de votre application. Elles ont été entièrement conçues pour garantir l’exécution de vos animations à 60 FPS, indépendamment du thread d’interface utilisateur, et pour vous offrir la possibilité de créer des expériences totalement inédites en usant de nombreuses propriétés et entrées pour produire les animations en question.

## <a name="motion-in-windows"></a>Mouvement dans Windows

Imaginez la conception du mouvement comme un film. Les transitions transparentes vous laissent concentrés sur l’histoire et donnent vie aux expériences. Nous pouvons inviter ce sentiment dans notre conception, pour guider les utilisateurs d’une tâche à l'autre avec une aisance cinématographique. Mouvement est souvent le facteur de différenciation entre une Interface utilisateur et une expérience utilisateur.

En tant que bloc de construction fondamental de la plateforme de l’interface utilisateur de Windows, CompositionAnimations fournissent un moyen puissant et efficace pour créer des expériences de mouvement dans l’interface utilisateur de votre application. Le moteur d’animation a été conçu dès le départ pour vous assurer que votre animation s’exécute à 60 FPS, indépendamment du thread d’interface utilisateur. Ces animations sont conçues pour fournir la flexibilité nécessaire pour créer des expériences innovantes mouvement basés sur le temps, d’entrée et d’autres propriétés.

### <a name="examples-of-motion"></a>Exemples de mouvement

Voici quelques exemples de mouvement dans une application.

Ici, une application utilise une animation connectée pour animer une image qui « perdure » pour faire partie de l’en-tête de la page suivante. L’effet aide à conserver le contexte de l’utilisateur pendant toute la transition.

![Un exemple d’Animation connecté](images/animation/connected-animation-example.gif)

Ici, un effet parallaxe visuel déplace les objets à différentes vitesses lorsque vous faites défiler ou que vous développez l'interface utilisateur, afin de créer un effet de profondeur, de perspective et de mouvement.

![Un exemple de parallaxe avec une liste et une image d’arrière-plan](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>Pour créer un mouvement à l’aide de CompositionAnimations

Pour générer le mouvement dans l’interface utilisateur, les développeurs peuvent accéder des animations en XAML ou la couche visuelle. Animations sur la couche visuelle offrent aux développeurs une série d’avantages :

- Performances – au lieu de l’animation lié aux threads d’interface utilisateur traditionnelle, animations sur la plateforme de l’interface utilisateur de Windows fonctionnent sur un thread indépendant à 60 FPS, l’activation des expériences de mouvement lisse.
- Modèle de création de modèles – animations dans la couche d’interface utilisateur de Windows sont des modèles, signification peut utiliser une seule animation sur plusieurs objets et modifier les propriétés ou paramètres sans se préoccuper d’obstruction précédente utilise.
- Personnalisation : la couche d’interface utilisateur Windows non seulement, il est facile de concevoir l’interface utilisateur de magnifiques, mais avec une gamme complète des types d’animation, Impossible de créer de nouvelles et étonnantes expériences avec un dégradé de personnalisations

En tant que développeur création d’expériences dans la couche d’interface utilisateur Windows, vous avez accès à un large éventail de concepts d’animation à donnez vie à vos concepts. Vous pouvez utiliser une de ces concepts pour animer une propriété ou de composant (le cas échéant) de n’importe quel CompositionObject de sous-chaîne.

> [!NOTE]
> Pas toutes les propriétés d’un CompositionObject peuvent être animées. Reportez-vous à la documentation de la CompositionObject individuel pour déterminer si une propriété est animée.

> [!NOTE]
> Le terme _sous-chaîne_ fait référence à un formulaire de composant d’une propriété. Par exemple, l’axe X, ou XY de la sous-chaîne d’une propriété de décalage Vector3.

| Concept d’animation | Description |
| ----------------- | ----------- |
| [Mouvement temporels KeyFrameAnimations](time-animations.md)  | KeyFrameAnimations permettent de contrôler directement l’intégralité d’une expérience de mouvement sur une période de temps. Développeurs décrivant d’un mouvement début, fin, une interpolation entre les deux et la durée de manière traditionnelle images clés. |
| [Mouvement relatif avec ExpressionAnimations](relation-animations.md)  | ExpressionAnimations sont utilisées pour décrire la façon dont un mouvement de la propriété d’un objet doit être motivé par rapport à la propriété d’un autre objet. Les développeurs définissent une équation mathématique qui définit la relation de référence. |
| ImplicitAnimations | Ces animations sont basés sur le déclencheur et sont définies séparément à partir de la logique d’application principale. ImplicitAnimations sont utilisées pour décrire comment et quand les animations se produisent en réponse aux modifications de propriété direct. |
| [Contrôlée par l’entrée de mouvement avec des Animations d’entrée](input-driven-animations.md)  | Les Animations d’entrée couvre un ensemble de scénarios qui permettent aux développeurs de décrire le mouvement par manipulation via tactile ou d’autres modalités d’entrée. Ces animations sont générées en fonction sur l’entrée d’utilisateur active ou de mouvements. |
| [En fonction du physique de mouvement avec NaturalMotionAnimations](natural-animations.md)  | NaturalMotionAnimations sont utilisées pour décrire le mouvement naturelle et familière forcer le mouvement piloté par les expériences basées sur le monde réel. Au lieu de définir le temps, les développeurs définissent les caractéristiques du mouvement (par exemple, d’amortissement ratio pour Springs) |