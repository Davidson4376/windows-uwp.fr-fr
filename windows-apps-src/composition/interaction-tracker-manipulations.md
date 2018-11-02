---
author: jwmsft
title: Manipulations personnalisées avec InteractionTracker
description: Utilisez les API InteractionTracker pour créer des expériences personnalisées de manipulation.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: Windows10, uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 0a991d692b4ba4c7a221932218a7d25e48fe16ca
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5969443"
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>Expériences personnalisées de manipulation avec InteractionTracker

Dans cet article, nous vous montrons comment utiliser InteractionTracker pour créer des expériences personnalisées de manipulation.

## <a name="prerequisites"></a>Éléments prérequis

À ce stade, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants:

- [Animations pilotées par une entrée](input-driven-animations.md)
- [Animations basées sur une relation](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>Pourquoi créer des expériences personnalisées de manipulation?

Dans la plupart des cas, l'utilisation de contrôles de manipulation prédéfinis suffit pour créer des expériences d’interface utilisateur. Mais si vous vous démarquiez des contrôles courants? Que se passe-t-il si vous souhaitez créer une expérience spécifique pilotée par une entrée ou disposer d’une interface utilisateur dans laquelle un mouvement de manipulation classique n’est pas suffisant? C'est là qu'intervient la création d'expériences personnalisées. Elles permettent aux développeurs et aux concepteurs d’application de faire preuve de davantage de créativité, de donner vie à des expériences de mouvement qui illustrent mieux leur image de marque et leur langage de conception personnalisée. Vous avez accès à un ensemble approprié de blocs de construction pour personnaliser complètement une expérience de manipulation, depuis la réponse du mouvement au contact du doigt sur l'écran, jusqu'aux points d’ancrage et au chaînage d’entrée.

Voici quelques exemples courants de cas où la création d'une expérience personnalisée de manipulation est appropriée:

- Ajout d’un balayage personnalisé, comportement visant à supprimer/ignorer
- Entrée pilotée par des effets (un mouvement panoramique rend le contenu flou)
- Contrôles personnalisés avec des mouvements de manipulation personnalisés (ListView personnalisé, ScrollViewer, etc.).

![Exemple de barre de défilement de balayage](images/animation/swipe-scroller.gif)

![Exemple tirer pour animer](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>Pourquoi utiliser InteractionTracker?

InteractionTracker a été introduit dans l'espace de noms Windows.UI.Composition.Interactions dans la version du SDK10586. Avantages d'InteractionTracker:

- **Flexibilité totale**: nous voulons vous permettre de personnaliser et d'adapter tous les aspects d’une expérience de manipulation, plus précisément, les mouvements exacts qui se produisent pendant ou en réponse à une entrée. Lorsque vous créez une expérience de manipulation personnalisée avec InteractionTracker, tous les boutons dont vous avez besoin sont à votre disposition.
- **Performances efficaces**: une des difficultés des expériences de manipulation est que leurs performances dépendent du thread d’interface utilisateur. Cela peut nuire à l'expérience de manipulation si l’interface utilisateur est occupée. InteractionTracker a été conçu pour utiliser le nouveau moteur Animation qui ne fonctionne que sur un thread indépendant à 60FPS, ce qui permet d'obtenir un mouvement fluide.

## <a name="overview-interactiontracker"></a>Vue d'ensemble: InteractionTracker

Lorsque vous créez des expériences personnalisées de manipulation, vous interagissez avec deux composants principaux. Nous allons aborder ceux-ci en premier:

- [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker): objet principal qui maintient une machine à états dont les propriétés sont pilotées par une entrée utilisateur active ou par des mises à jour directes et des animations. Il est destiné à se lier ensuite à une CompositionAnimation pour créer le mouvement personnalisé de manipulation.
- [VisualInteractionSource](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.visualinteractionsource): objet de complément qui définit quand et dans quelles conditions l’entrée est envoyée à InteractionTracker. Il définit à la fois le CompositionVisual utilisé pour le test de positionnement et les autres propriétés de configuration d’entrée.

En tant que machine à états, les propriétés d'InteractionTracker peuvent être déterminées par les éléments suivants:

- Interaction utilisateur directe: l’utilisateur final manipule directement dans la zone de test de positionnement VisualInteractionSource
- Inertie: à partir d'une vitesse par programmation ou d'un mouvement de l’utilisateur, les propriétés d'InteractionTracker s'animent selon une courbe d'inertie
- CustomAnimation: animation personnalisée ciblant directement une propriété d'InteractionTracker

### <a name="interactiontracker-state-machine"></a>Machine à états InteractionTracker

Comme mentionné précédemment, InteractionTracker est une machine à états avec gère 4 états – chacun d'entre eux peut passer à un de l’autre fourstates. (Pour plus d’informations sur le passage d'InteractionTracker d'un état à l'autre, voir la documentation de la classe [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker).)

| État | Description |
|-------|-------------|
| Inactif | Non actif, pilotant des entrées ou des animations |
| Interaction | Entrée utilisateur active détectée |
| Inertie | Mouvement actif résultant d’une entrée active ou d'une vitesse de programmation |
| CustomAnimation | Mouvement actif résultant d’une animation personnalisée |

Dans chacun des cas où l’état d'InteractionTracker change, un événement (ou un rappel) est généré que vous pouvez détecter. Pour détecter ces événements, vous devez implémenter l'interface [IInteractionTrackerOwner](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) et créer son objet InteractionTracker avec la méthode CreateWithOwner. Le diagramme suivant présente également les différents événements déclenchés.

![Machine à états InteractionTracker](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>Utilisation de VisualInteractionSource

Pour qu'InteractionTracker soit piloté par une entrée, vous devez lui connecter une VisualInteractionSource (VIS). La VIS est créée en tant qu’objet de complément à l’aide d’un CompositionVisual pour définir:

1. la zone de test de positionnement que l'entrée va suivre et l’espace de coordonnées dans lequel les mouvements sont détectés;
1. les configurations d’entrée qui seront détectées et routées, notamment les suivantes:
    - Mouvements détectables: Position X et Y (panoramique horizontal et vertical), Échelle (pincement)
    - Inertie
    - Rails et chaînage
    - Modes de redirection: les données d’entrée qui sont redirigées automatiquement vers InteractionTracker

> [!NOTE]
> Dans la mesure où le VisualInteractionSource est créé en fonction de la position de test de positionnement et de l’espace de coordonnées d’un élément visuel, il recommandé de ne pas utiliser un élément visuel qui sera en mouvement ou dans une position changeante.

> [!NOTE]
> Vous pouvez utiliser plusieurs instances VisualInteractionSource avec le même InteractionTracker s’il existe plusieurs zones de test de positionnement. Toutefois, le cas le plus courant consiste à n'utiliser qu’une seule VIS.

La VisualInteractionSource est également responsable de la gestion du moment où les données d’entrée de différentes modalités (tactile, PTP, stylet) sont acheminées vers InteractionTracker. Ce comportement est défini par la propriété ManipulationRedirectionMode. Par défaut, toutes les entrées de pointeur sont envoyées au thread d’interface utilisateur et les entrées du pavé tactile de précision sont envoyées à VisualInteractionSource et InteractionTracker.

Par conséquent, si vous souhaitez disposer de l'interface tactile et d'un stylet (Creators Update) pour piloter une manipulation via une VisualInteractionSource et InteractionTracker, vous devez appeler la méthode VisualInteractionSource.TryRedirectForManipulation. Dans le court extrait ci-dessous d’une application XAML, la méthode est appelée lorsqu’un événement de touche enfoncée se produit dans la Grille UIElement située tout en haut:

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>Raccordement avec ExpressionAnimations

Lorsque vous utilisez InteractionTracker pour piloter une expérience de manipulation, vous interagissez principalement avec les propriétés Échelle et Position. Comme les autres propriétés CompositionObject, ces propriétés peuvent à la fois être la cible et être référencées dans une CompositionAnimation, plus communément ExpressionAnimations.

Pour utiliser InteractionTracker au sein d’une Expression, faites référence à la propriété Position (ou Échelle) du suivi comme dans l’exemple ci-dessous. Comme la propriété d'InteractionTracker est modifiée en raison d’une des conditions décrites précédemment, la sortie de l’Expression change également.

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> Lorsque vous référencez la position de d'InteractionTracker dans une Expression, vous devez rendre négative la valeur de l’Expression obtenue pour vous déplacer dans la bonne direction. Cela est dû à la progression de l'InteractionTracker à partir de l'origine sur un graphique et vous permet d’envisager la progression de l'InteractionTracker en coordonnées «réalistes», par exemple, la distance à partir de son origine.

## <a name="get-started"></a>Prise en main

Pour apprendre à utiliser InteractionTracker pour créer des expériences personnalisées de manipulation:

1. Créez votre objet InteractionTracker à l’aide d'InteractionTracker.Create ou d'InteractionTracker.CreateWithOwner.
    - (Si vous utilisez CreateWithOwner, assurez-vous d'implémenter l’interface IInteractionTrackerOwner.)
1. Définissez la position Minimum et Maximum de votre InteractionTracker nouvellement créé.
1. Créez votre VisualInteractionSource avec un CompositionVisual.
    - Assurez-vous que l’élément visuel que vous transmettez a une taille différente de zéro. Sinon, il ne passera pas correctement le test de positionnement.
1. Définissez les propriétés de la VisualInteractionSource.
    - VisualInteractionSourceRedirectionMode
    - PositionXSourceMode, PositionYSourceMode, ScaleSourceMode
    - Rails et chaînage
1. Ajoutez la VisualInteractionSource à InteractionTracker à l’aide d'InteractionTracker.InteractionSources.Add.
1. Configurez TryRedirectForManipulation pour le moment de la détection d'une entrée tactile et du stylet.
    - Pour XAML, cela s'effectue généralement sur l'événement PointerPressed de l’élément UIElement.
1. Créez une ExpressionAnimation qui fait référence à la position de l'InteractionTracker et cible une propriété de CompositionObject.

Voici un court extrait de code qui montre les n°1 à 5 en action:

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSourc
    var source = VisualInteractionSource.Create(hitTestRoot);

    // #4 Set the properties for the VisualInteractionSource
    source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    source.PositionXSourceMode = InteractionSourceMode.EnabledWithInertia;
    source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;

    // #5 Add the VisualInteractionSource to InteractionTracker
    tracker.InteractionSources.Add(source);
}
```

Pour des utilisations plus avancées d'InteractionTracker, consultez les articles suivants:

- [Créer des points d’ancrage à l'aide d'InertiaModifiers](inertia-modifiers.md)
- [Tirer pour actualiser avec des SourceModifiers](source-modifiers.md)
