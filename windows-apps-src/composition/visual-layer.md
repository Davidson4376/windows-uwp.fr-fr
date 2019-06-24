---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Couche visuelle
description: L’API Windows.UI.Composition vous donne accès à la couche de composition comprise entre la couche d’infrastructure (XAML) et la couche graphique (DirectX).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bf9cc4f97cdfcb02eb725b81163f215b22b259e4
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318056"
---
# <a name="visual-layer"></a>Couche visuelle

La couche visuelle fournit une performance élevée, une API en mode retenu pour les graphiques, les effets et les animations. Elle constitue la base de l’ensemble de l’interface utilisateur sur les appareils Windows. Vous définissez votre interface utilisateur de façon déclarative, et la couche visuelle s’appuie sur l’accélération matérielle graphique pour assurer le rendu de vos contenus, effets et animations de manière fluide et sans défauts, indépendamment du thread d’interface utilisateur de l’application.

Éléments essentiels :

* API WinRT familières
* Conçue pour des interactions et une interface utilisateur plus dynamiques
* Concepts harmonisés avec les outils de conception
* Évolutivité linéaire sans chute brutale des performances

Vos applications UWP Windows utilisent déjà la couche visuelle via l’une des infrastructures d’interface utilisateur. Vous pouvez également tirer parti directement de la couche visuelle pour personnaliser très simplement vos rendus, effets et animations.

![Couche d’infrastructure d’interface utilisateur : la couche d’infrastructure (Windows.UI.XAML) s’appuie sur la couche visuelle (Windows.UI.Composition) qui repose sur la couche graphique (DirectX).](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>Nouveautés de la couche visuelle

Principales fonctions de la couche visuelle :

1. **Contenu** : COMPOSITION léger de contenu dessiné personnalisé
1. **Effets**: L’interface utilisateur en temps réel des effets système dont les effets peuvent être animées, chaînées et personnalisées
1. **Animations**: Animations expressives, indépendant du framework en cours d’exécution indépendamment du thread d’interface utilisateur

### <a name="content"></a>Contenu

Le contenu est hébergé, transformé et peut être utilisé par le système d’effets et d’animations à l’aide d’éléments visuels. La classe [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual) figure à la base de la hiérarchie de classes. Il s’agit d’un proxy léger, agile de thread dans le processus d’application pour l’état visuel du compositeur. Incluent les sous-classes de visuel  [**ContainerVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) pour permettre aux enfants créer des arborescences d’éléments visuels et [ **SpriteVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) qui contient le contenu et peut être peint avec deux couleurs unies, dessinés contenus ou élément visuel des effets personnalisés. Ensemble, ces types Visual constituent la structure de l’arborescence des éléments visuels de l’interface utilisateur 2D et soutiennent les éléments FrameworkElements XAML les plus visibles.

Pour plus d’informations, consultez la vue d’ensemble [Éléments visuels de composition](composition-visual-tree.md).

### <a name="effects"></a>Effets

Le système d’effets de la couche visuelle vous permet d’appliquer une chaîne d’effets de filtre et de transparence à un élément visuel ou à une arborescence d’éléments visuels. Il s’agit d’un système d’effets d’interface utilisateur, à ne pas confondre avec les effets d’image et multimédias. Les effets fonctionnent conjointement avec le système d’animations, ce qui permet aux utilisateurs d’obtenir des animations fluides et dynamiques des propriétés d’effet, dont le rendu est indépendant du thread d’interface utilisateur. Les effets de la couche visuelle fournissent les blocs de construction créative qui peuvent être combinés et animés pour créer des expériences personnalisées et interactives.

Outre les chaînes à effet animable, la couche visuelle prend en charge un modèle d’éclairage qui permet aux éléments visuels de reproduire des propriétés matérielles en répondant à des éclairages animables. Les éléments visuels peuvent également projeter des ombres. L’éclairage et les ombres peuvent être combinés pour créer une perception de profondeur et de réalisme.

Pour plus d’informations, consultez la vue d’ensemble [Effets de composition](composition-effects.md).

### <a name="animations"></a>Animations

Le système d’animations de la couche visuelle vous permet de déplacer des éléments visuels, d’animer des effets, et d’actionner des transformations, des clips et d’autres propriétés.  Ce système indépendant de l’infrastructure a été entièrement conçu en ayant les performances à l’esprit.  Il s’exécute indépendamment du thread d’interface utilisateur pour garantir fluidité et évolutivité.  Si ce système vous permet d’utiliser des animations par images clés familières pour animer des modifications de propriétés au fil du temps, il vous permet aussi de définir des relations mathématiques entre les différentes propriétés, notamment les entrées utilisateur, ce qui vous permet de concevoir directement des expériences chorégraphiées harmonieuses.

Pour plus d’informations, consultez la vue d’ensemble [Animations de composition](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Utilisation de votre application UWP XAML

Vous pouvez accéder à un élément visuel créé par l’infrastructure XAML, et soutenant un élément FrameworkElement visible, à l’aide de la classe [**ElementCompositionPreview**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) de [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting). Notez que les éléments visuels créés pour vous par l’infrastructure présentent certaines limites de personnalisation. Cela tient au fait que l’infrastructure gère les décalages, les transformations et les durées de vie. Vous pouvez toutefois créer vos propres éléments visuels et les associer à un élément XAML existant via ElementCompositionPreview, ou en l’ajoutant à un élément ContainerVisual existant quelque part dans la structure de l’arborescence des éléments visuels.

Pour plus d’informations, consultez la vue d’ensemble [Utilisation de la couche visuelle avec XAML](using-the-visual-layer-with-xaml.md).

### <a name="working-with-your-desktop-app"></a>Utilisation de votre application de bureau

Vous pouvez utiliser la couche visuelle pour améliorer l’apparence et les fonctionnalités de votre WPF, Windows Forms, et C++ des applications de bureau Win32. Vous pouvez migrer des îlots de contenu à utiliser la couche visuelle et de conserver le reste de votre interface utilisateur dans son infrastructure existante. Cela signifie que vous pouvez rendre mises à jour importantes et améliorations à l’interface utilisateur de votre application sans avoir à apporter des modifications importantes à votre code existant base.

Pour plus d’informations, consultez [Moderniser votre application de bureau à l’aide de la couche Visuel](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps).

## <a name="additional-resources"></a>Ressources supplémentaires

* [**Documentation de référence complète pour l’API**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
* Exemples d’interface utilisateur et de composition avancés dans le [GitHub WindowsUIDevLabs](https://github.com/microsoft/WindowsCompositionSamples).
* [Galerie d’exemples Windows.UI.Composition](https://aka.ms/winuiapp)
* [@windowsui Flux Twitter ](https://twitter.com/windowsui)
* Lisez l’Article MSDN de Kenny Kerr sur cette API : [Graphisme et Animation - API de Composition Windows 10](https://msdn.microsoft.com/magazine/mt590968)
