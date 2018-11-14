---
author: jwmsft
title: Animations pilotées par une entrée
description: Découvrez comment utiliser des animations d’entrée dans l’interface utilisateur de votre application.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows10, uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 04eabb4c70143a08f5b850e6444f7f3d21a9dd4a
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6462865"
---
# <a name="input-driven-animations"></a>Animations pilotées par une entrée

Cet article constitue une présentation de l’API InputAnimation et recommande l’utilisation de ces types d’animations dans votre interface utilisateur.

## <a name="prerequisites"></a>Éléments prérequis

À ce stade, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants:

- [Animations basées sur une relation](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>Mouvement fluide piloté par des interactions utilisateur

Dans le langage de la conception Fluent, l’interaction entre les utilisateurs finaux et les applications est d’une importance capitale. Les applications doivent non seulement avoir de l'allure, mais également répondre naturellement et dynamiquement aux utilisateurs qui interagissent avec elles. Cela signifie que lorsqu’un doigt se place sur l’écran, l’interface utilisateur doit réagir impeccablement aux variations de degrés d’entrée. Le défilement doit être fluide et réagir instantanément au mouvement panoramique du doigt sur l’écran.

La création d’une interface utilisateur qui répond de manière dynamique et fluide à l’entrée utilisateur augmente l'engagement des utilisateurs. Le mouvement n'est plus seulement fidèle, mais agréable et naturel quand les utilisateurs interagissent avec vos différentes expériences d’interface utilisateur. Cela permet aux utilisateurs finaux de se connecter plus facilement avec votre application, ce qui rend l’expérience plus mémorable et agréable.

## <a name="expanding-past-just-touch"></a>Au-delà de la simple interaction tactile

Bien que l’interaction tactile soit l'une des interfaces les plus couramment utilisées pour manipuler le contenu de l’interface utilisateur, d'autres modalités d’entrée diverses sont également utilisées, comme la souris et le stylet. Dans ces cas, il est important que les utilisateurs finaux perçoivent que votre interface utilisateur répond dynamiquement à leur entrée, quelles que soient les modalités d’entrée choisies. Vous devez être conscient des différentes modalités d’entrée lors de la conception d'expériences de mouvement pilotées par l’entrée.

## <a name="different-input-driven-motion-experiences"></a>Expériences de mouvement pilotées par l’entrée diverses

L’espace InputAnimation fournit plusieurs expériences différentes qui vous permettent de créer un mouvement réagissant de manière dynamique. Comme le reste du système d’animation de l’interface utilisateur de Windows, ces animations pilotées par l’entrée fonctionnent sur un thread indépendant, ce qui contribue à l’expérience de mouvement dynamique. Toutefois, dans certains cas où l’expérience tire parti de contrôles et de composants XAML existants, des parties de ces expériences restent liées au thread d’interface utilisateur.

Trois expériences principales sont possibles lorsque vous créez des animations de mouvement dynamiques pilotées par l’entrée:

1. Amélioration des expériences ScrollView existantes: activez la position d’un élément ScrollViewer XAML pour piloter des expériences d’animation dynamiques.
1. Expériences basées sur la position du pointeur: utilisez la position d’un curseur sur un UIElement qui a subi un test de positionnement pour piloter des expériences d’animation dynamiques.
1. Expériences personnalisées de manipulation avec InteractionTracker: créer des expériences de manipulation hors thread entièrement personnalisées avec InteractionTracker (par exemple, un canevas de défilement/zoom).

## <a name="enhancing-existing-scrollviewer-experiences"></a>Amélioration des expériences ScrollViewer existantes

Une des méthodes courantes pour créer des expériences plus dynamiques consiste à utiliser un contrôle XAML ScrollViewer existant. Dans ces situations, vous tirez parti de la position de défilement d’un élément ScrollViewer pour créer des composants d’interface utilisateur supplémentaires permettant une expérience de défilement simple plus dynamique. Certains exemples comprennent des en-têtes rémanents/discrets et un effet parallaxe.

![Affichage liste avec parallaxe](images/animation/parallax.gif)

![En-tête discret](images/animation/shy-header.gif)

Lorsque vous créez ces types d’expériences, il faut respecter une formule générale:

1. Accédez au ScrollManipulationPropertySet sur le XAML ScrollViewer pour lequel vous souhaitez piloter une animation.
    - Effectué via l’API ElementCompositionPreview.GetScrollViewerManipulationPropertySet(UIElement element)
    - Renvoie un CompositionPropertySet contenant une propriété appelée **Traduction**
1. Créez un objet Composition ExpressionAnimation avec une équation qui fait référence à la propriété Traduction.
1. Démarrez l’animation sur la propriété d’un CompositionObject.

Pour plus d’informations sur la création de ces expériences, voir [Améliorer les expériences ScrollViewer existantes](scroll-input-animations.md).

## <a name="pointer-position-driven-experiences"></a>Expériences basées sur la position du pointeur

Une autre expérience dynamique courante impliquant l’entrée consiste à piloter une animation en fonction de la position d’un pointeur, par exemple, le curseur de la souris. Dans ce cas, les développeurs tirent profit de l’emplacement d’un curseur lors du test de positionnement dans un élément UIElement XAML qui permet de créer des expériences telles que Révéler avec un projecteur.

![Exemple de projecteur de pointeur](images/animation/spotlight-reveal.gif)

![Exemple de rotation de pointeur](images/animation/pointer-rotate.gif)

Lorsque vous créez ces types d’expériences, il existe une formule générale à suivre:

1. Accédez au PointerPositionPropertySet sur un élément UIElement XAML dont vous souhaitez connaître la position du curseur lors du test de positionnement.
    - Effectué via l’API ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element)
    - Renvoie un CompositionPropertySet contenant une propriété appelée **Position**
1. Créez un objet CompositionExpressionAnimation avec une équation qui fait référence à la propriété Position.
1. Démarrez l’animation sur la propriété d’un CompositionObject.

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>Expériences personnalisées de manipulation avec InteractionTracker

L'une des difficultés liées à l’utilisation d'un XAML ScrollViewer est qu’il est lié au thread d’interface utilisateur. Par conséquent, l’expérience de défilement et de zoom est souvent décalée et saccadée si le thread d’interface utilisateur est occupé. Cela produit une expérience peu attrayante. En outre, il n’est pas possible de personnaliser beaucoup d'aspects de l’expérience ScrollViewer. InteractionTracker a été créé pour résoudre ces deux problèmes en fournissant un ensemble de blocs de construction pour créer des expériences personnalisées de manipulation qui sont exécutées sur un thread indépendant.

![Exemple d’interactions 3D](images/animation/interactions-3d.gif)

![Exemple tirer pour animer](images/animation/pull-to-animate.gif)

Lorsque vous créez des expériences avec InteractionTracker, il existe une formule générale à suivre:

1. Créez votre objet InteractionTracker et définissez ses propriétés.
1. Créez des VisualInteractionSources pour tout objet CompositionVisual qui doit capturer une entrée à utiliser par InteractionTracker.
1. Créez un objet Composition ExpressionAnimation avec une équation qui fait référence à la propriété Position d'InteractionTracker.
1. Démarrez l’animation sur la propriété d'une composition visuelle que vous souhaitez piloter par InteractionTracker.
