---
title: Modernisez votre application de bureau à l’aide de la couche visuelle
description: Utiliser la couche visuelle pour améliorer l’interface utilisateur de votre application de bureau .NET ou Win32.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 042e34b8d09c2bae5cefb4227ef2a104a43861a6
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984969"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>À l’aide de la couche visuelle dans les applications de bureau

Vous pouvez maintenant utiliser des API UWP dans les applications de bureau non UWP pour améliorer l’apparence et les fonctionnalités de votre WPF, Windows Forms, et C++ applications Win32 et tirer parti des dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via UWP.

Pour de nombreux scénarios, vous pouvez utiliser [(îles) XAML](xaml-islands.md) pour ajouter des contrôles XAML modernes à votre application. Toutefois, lorsque vous avez besoin créer des expériences personnalisées qui dépassent les contrôles intégrés, vous pouvez accéder à la couche visuelle API.

La couche visuelle fournit un haute performance, les API en mode mémorisé pour les graphiques, les effets et les animations. Il est à la base de l’interface utilisateur sur des appareils Windows 10. Les contrôles UWP XAML reposent sur la couche visuelle, et elle permet de nombreux aspects de la [Fluent Design System](/windows/uwp/design/fluent-design-system/index), telles que la lumière, profondeur, Motion, matériel et mise à l’échelle.

![Interface utilisateur créée avec la couche visuelle](images/visual-layer-interop/pull-to-animate.gif)

> _Interface utilisateur créée avec la couche visuelle_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>Créer une interface utilisateur visuellement attrayante dans n’importe quelle application Windows

La couche visuelle vous permet de créer des expériences attrayantes à l’aide de composition léger de contenu dessiné personnalisé (visuels) et appliquer des animations puissantes, des effets et des manipulations sur ces objets dans votre application. La couche visuelle ne remplace pas n’importe quel framework de l’interface utilisateur existante ; au lieu de cela, il est un complément précieux ces infrastructures.

Vous pouvez utiliser la couche visuelle pour donner à votre application un aspect unique et établir une identité qui le différencie des autres applications. Il permet également les principes de conception Fluent, lesquels sont conçus pour rendre vos applications plus facile à utiliser, de dessin plus engagement des utilisateurs. Par exemple, vous pouvez l’utiliser pour créer des signaux visuels et des transitions d’écran animées qui illustrent les relations entre éléments sur l’écran.

## <a name="visual-layer-features"></a>Fonctionnalités de la couche visuelle

### <a name="brushes"></a>Pinceaux

[Pinceaux de composition](/windows/uwp/composition/composition-brushes) let vous peindre des objets d’interface utilisateur avec des couleurs unies, des dégradés, images, vidéos, des effets complexes et bien plus encore.

![Un egg créé avec un créateur de matériau](images/visual-layer-interop/egg.gif)

> _Un egg créé avec le [application de démonstration de créateur de matériau](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)._

### <a name="effects"></a>Effets

[Effets de composition](/windows/uwp/composition/composition-effects) incluent light, clichés instantanés et une liste des effets de filtre. Ils peuvent être animées, personnalisés et chaînées, puis appliquées directement aux éléments visuels. Le SceneLightingEffect peut être combiné avec éclairage de composition pour créer une atmosphère, profondeur et supports.

![Matériel et signalisation](images/visual-layer-interop/light-interop.gif)

> _Lumières et matériau illustrés dans le [Galerie d’exemples de Composition de l’interface utilisateur Windows](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

### <a name="animations"></a>Animations

[Animations de composition](/windows/uwp/composition/composition-animation) s’exécutent directement dans le processus de compositeur, indépendamment du thread d’interface utilisateur. Cela garantit le lissage et la mise à l’échelle, donc vous pouvez exécuter un grand nombre d’animations simultanées, explicites. En plus familiers animations de trame clé pour piloter les changements de propriété au fil du temps, vous pouvez utiliser des expressions pour définir des relations mathématiques entre les différentes propriétés, y compris l’entrée d’utilisateur. Animations piloté par les d’entrée vous permettent de créer l’interface utilisateur avec fluidité et dynamiquement répond à l’entrée utilisateur, ce qui peut entraîner l’intérêt des utilisateurs plus élevée.

![Interface utilisateur créée avec la couche visuelle](images/visual-layer-interop/swipe-scroller.gif)

> _Mouvement illustré dans le [Galerie d’exemples de Composition de l’interface utilisateur Windows](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>Conserver votre base de code existante et adopter de façon incrémentielle

Le code dans vos applications existantes représente un investissement important que vous ne souhaitez pas perdre. Vous pouvez migrer _(îles)_ du contenu à utiliser la couche visuelle et de conserver le reste de l’interface utilisateur dans son infrastructure existante. Cela signifie que vous pouvez rendre mises à jour importantes et améliorations à l’interface utilisateur de votre application sans avoir à apporter des modifications importantes à votre code existant base.

## <a name="samples-and-tutorials"></a>Exemples et didacticiels

Découvrez comment utiliser la couche visuelle dans vos applications en appuyant sur nos exemples. Ces exemples et didacticiels vous aident à commencez à l’aide de la couche visuelle et illustrer le fonctionnement des fonctionnalités.

### <a name="win32"></a>Win32

- [À l’aide de la couche visuelle avec Win32](using-the-visual-layer-with-win32.md) didacticiel
  - [Exemple de Composition Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Exemple de vecteurs Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [Exemple de Surfaces virtuel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [Exemple de Capture d’écran](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- [À l’aide de la couche visuelle avec Windows Forms](using-the-visual-layer-with-windows-forms.md) didacticiel
  - [Exemple de Composition Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [Exemple d’intégration de la couche Visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [À l’aide de la couche visuelle avec WPF](using-the-visual-layer-with-wpf.md) didacticiel
  - [Exemple de Composition Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [Exemple d’intégration de la couche Visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [Exemple de Capture d’écran](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>Limitations

Bien que de nombreuses fonctionnalités de la couche visuelle fonctionnent de la même quand ils sont hébergés dans une application de bureau comme ils le font dans une application UWP, certaines fonctionnalités certaines limitations s’appliquent. Voici quelques-unes des limitations à connaître :

- Chaînes d’effet s’appuient sur [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) pour les descriptions d’effet. Le [package Win2D NuGet](https://www.nuget.org/packages/Win2D.uwp) ne prend pas en charge les applications de bureau, donc vous devez recompiler à partir de la [code source](https://github.com/Microsoft/Win2D).
- Pour le test de positionnement, vous devez effectuer des calculs de limites en parcourant l’arborescence visuelle vous-même. Cela est identique à la couche visuelle dans UWP, mais dans ce cas il n’existe aucun élément XAML que vous pouvez facilement lier à pour le test de positionnement.
- La couche visuelle n’a pas une primitive de rendu de texte.
- Lorsque deux technologies d’interface utilisateur différentes sont utilisées ensemble, telles que WPF et la couche visuelle, chacune d’elles est responsable du dessin de leurs propres pixels sur l’écran, et ils ne peuvent pas partager des pixels. Par conséquent, le contenu de la couche visuelle est toujours rendue sur tout autre contenu de l’interface utilisateur. (Il s’agit du _espace aérien_ problème.) Vous devrez peut-être générer du code supplémentaire et la tester pour s’assurer de votre contenu de la couche visuelle est redimensionné avec l’interface utilisateur de l’hôte et n’occlude tout autre contenu.
- Contenu hébergé dans une application de bureau n’automatiquement redimensionner ou mettre à l’échelle dpi. Étapes supplémentaires peuvent nécessaire pour garantir votre contenu gère les modifications de PPP. (Consultez les didacticiels spécifiques de la plateforme pour plus d’informations.)

## <a name="additional-resources"></a>Ressources complémentaires

- [Couche visuelle](/windows/uwp/composition/visual-layer)
- [COMPOSITION visual](/windows/uwp/composition/composition-visual-tree)
- [Pinceaux de composition](/windows/uwp/composition/composition-brushes)
- [Effets de composition](/windows/uwp/composition/composition-effects)
- [Animations de composition](/windows/uwp/composition/composition-animation)

Informations de référence sur les API

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)