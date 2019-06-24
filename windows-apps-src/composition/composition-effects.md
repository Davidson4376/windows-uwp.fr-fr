---
ms.assetid: 6e9b9ff2-234b-6f63-0975-1afb2d86ba1a
title: Effets de composition
description: Les API d’effet permettent aux développeurs de personnaliser l’affichage de leur interface utilisateur.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: afcb94ca0e6692d5dfede526f1368b71920ab771
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318205"
---
# <a name="composition-effects"></a>Effets de composition

L’API WinRT [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition) autorise l’application d’effets en temps réel à des images et à l’interface utilisateur avec des propriétés d’effet animables. Dans cette vue d’ensemble, nous allons parcourir les fonctionnalités disponibles, qui permettent d’appliquer des effets à un élément visuel de composition.

Pour prendre en charge la cohérence de [plateforme Windows universelle (UWP)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp) pour les développeurs décrivant des effets dans leurs applications, les effets de composition tirent parti de l’interface IGraphicsEffect de Win2D pour utiliser les descriptions d’effet via l’espace de noms [Microsoft.Graphics.Canvas.Effects](https://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm).

Les effets de pinceau permettent de peindre les zones d’une application en appliquant des effets à un ensemble d’images existantes. Les API d’effet de composition de Windows 10 sont axées sur SpriteVisual. SpriteVisual permet une flexibilité et une interconnexion de création de couleur, d’image et d’effet. SpriteVisual est un type d’élément visuel de composition qui permet de remplir un rectangle 2D avec un pinceau. L’élément visuel définit les limites du rectangle, et le pinceau définit les pixels utilisés pour peindre le rectangle.

Les pinceaux à effets sont utilisés sur les éléments visuels de l’arborescence de composition dont le contenu provient de la sortie d’un graphique d’effet. Les effets peuvent référencer des surfaces/textures existantes, mais pas la sortie des autres arborescences de composition.

Les effets peuvent également être appliqués à des éléments UIElements XAML à l’aide d’un pinceau à effets avec [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="effect-features"></a>Fonctionnalités d’effet

- [Bibliothèque d’effet](./composition-effects.md#effect-library)
- [Le chaînage des effets](./composition-effects.md#chaining-effects)
- [Prise en charge de l’animation](./composition-effects.md#animation-support)
- [Constante vs. Propriétés de l’effet d’animation](./composition-effects.md#constant-vs-animated-effect-properties)
- [Plusieurs Instances d’effet avec des propriétés indépendantes](./composition-effects.md#multiple-effect-instances-with-independent-properties)

### <a name="effect-library"></a>Bibliothèque d’effets

Actuellement, les compositions prennent en charge les effets suivants :

| Effet               | Description                                                                                                                                                                                                                |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Transformation affine 2D :  | applique une matrice de transformation affine 2D à une image. Nous avons utilisé cet effet pour animer un masque alpha dans nos [exemples](https://go.microsoft.com/fwlink/?LinkId=785341) d’effet.       |
| Composite arithmétique : | combine deux images à l’aide d’une équation flexible. Nous avons utilisé le composite arithmétique pour créer un effet de fondu enchaîné dans nos [exemples](https://go.microsoft.com/fwlink/?LinkId=785341). |
| Effet de fusion :         | crée un effet de fusion qui combine deux images. La composition fournit 21 des 26 [modes de fusion](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_Effects_BlendEffectMode.htm) pris en charge dans Win2D.        |
| Source de couleur :         | génère une image contenant une couleur unie.                                                                                                                                                                               |
| Composite :            | combine deux images. La composition fournit l’ensemble des 13 [modes composites](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasComposite.htm) pris en charge dans Win2D.                                              |
| Contraste             | augmente ou diminue le contraste d’une image.                                                                                                                                                                           |
| Exposition :             | augmente ou diminue l’exposition d’une image.                                                                                                                                                                           |
| Nuances de gris :            | convertit une image en gris monochrome.                                                                                                                                                                                   |
| Transfert gamma :       | modifie les couleurs d’une image en appliquant une fonction de transfert gamma par canal.                                                                                                                                           |
| Rotation des teintes :           | modifie la couleur d’une image en faisant tourner ses valeurs de teinte.                                                                                                                                                                   |
| Inverser :               | inverse les couleurs d’une image.                                                                                                                                                                                            |
| Saturer :             | modifie la saturation d’une image.                                                                                                                                                                                         |
| Sépia :                | convertit une image en tons sépia.                                                                                                                                                                                          |
| Température et teinte : | ajuste la température et/ou la teinte d’une image.                                                                                                                                                                           |

Pour plus d’informations, voir l’espace de noms [Microsoft.Graphics.Canvas.Effects](https://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm) de Win2D. Effets ne pas pris en charge dans une composition sont indiquées comme \[NoComposition\].

### <a name="chaining-effects"></a>Chaînage des effets

Les effets peuvent être chaînés, ce qui permet à une application d’utiliser simultanément plusieurs effets dans une image. Les graphiques d’effet peuvent prendre en charge plusieurs effets qui peuvent faire référence les uns aux autres. Lorsque vous décrivez votre effet, ajoutez simplement un effet comme entrée pour votre effet.

```cs
IGraphicsEffect graphicsEffect =
new Microsoft.Graphics.Canvas.Effects.ArithmeticCompositeEffect
{
  Source1 = new CompositionEffectSourceParameter("source1"),
  Source2 = new SaturationEffect
  {
    Saturation = 0,
    Source = new CompositionEffectSourceParameter("source2")
  },
  MultiplyAmount = 0,
  Source1Amount = 0.5f,
  Source2Amount = 0.5f,
  Offset = 0
}
```

L’exemple ci-dessus décrit un effet composite arithmétique possédant deux entrées. La deuxième entrée possède un effet de saturation avec une propriété de saturation de 0,5.

### <a name="animation-support"></a>Prise en charge de l’animation

Les propriétés d’effet prennent en charge l’animation ; lors de la compilation d’effet, vous pouvez spécifier les propriétés d’effet qui peuvent être animées et celles qui peuvent être « intégrées » sous forme de constantes. Les propriétés animables sont spécifiées par le biais de chaînes de la forme « nom d’effet.nom de propriété ». Ces propriétés peuvent être animées indépendamment sur plusieurs instanciations de l’effet.

### <a name="constant-vs-animated-effect-properties"></a>Propriétés des effets constant et animé

Lors de la compilation, vous pouvez spécifier des propriétés d’effet dynamiques ou des propriétés d’effet « intégrées » sous forme de constantes. Les propriétés dynamiques sont spécifiées par le biais de chaînes de la forme « <effect name>.<property name> ». Les propriétés dynamiques peuvent être définies sur une valeur spécifique ou être animées à l’aide du système d’animations de composition.

Lorsque vous compilez la description d’effet ci-dessus, vous avez la possibilité d’intégrer la saturation de sorte qu’elle soit égale à 0,5, ou de la rendre dynamique et de la définir de manière dynamique ou en l’animant.

Compilation d’un effet avec la saturation intégrée :

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
```

Compilation d’un effet avec la saturation dynamique :

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect, new[]{"SaturationEffect.Saturation"});
_catEffect = effectFactory.CreateBrush();
_catEffect.SetSourceParameter("mySource", surfaceBrush);
_catEffect.Properties.InsertScalar("saturationEffect.Saturation", 0f);
```

La propriété de saturation de l’effet ci-dessus peut alors être définie sur une valeur statique ou être animée à l’aide d’animations de type Expression ou ScalarKeyFrame.

Vous pouvez créer un élément ScalarKeyFrame qui sera utilisé pour animer la propriété de saturation d’un effet comme suit :

```cs
ScalarKeyFrameAnimation effectAnimation = _compositor.CreateScalarKeyFrameAnimation();
            effectAnimation.InsertKeyFrame(0f, 0f);
            effectAnimation.InsertKeyFrame(0.50f, 1f);
            effectAnimation.InsertKeyFrame(1.0f, 0f);
            effectAnimation.Duration = TimeSpan.FromMilliseconds(2500);
            effectAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
```

Démarrez l’animation sur la propriété de saturation de l’effet comme suit :

```cs
catEffect.Properties.StartAnimation("saturationEffect.Saturation", effectAnimation);
```

Voir l’[exemple Désaturation - Animation](https://go.microsoft.com/fwlink/?LinkId=785342) pour les propriétés d’effet animées avec des images clés et l’[exemple AlphaMask](https://go.microsoft.com/fwlink/?LinkId=785343) pour l’utilisation des effets et expressions.

### <a name="multiple-effect-instances-with-independent-properties"></a>Plusieurs instances d’effet avec des propriétés indépendantes

En indiquant qu’un paramètre doit être dynamique lors de la compilation d’effet, le paramètre peut ensuite être modifié sur la base d’une instance par effet. Cela permet à deux éléments visuels d’utiliser le même effet tout en étant affichés avec des propriétés d’effet différentes. Pour plus d’informations, voir l’[exemple](https://go.microsoft.com/fwlink/?LinkId=785344) de code utilisant les paramètres ColorSource et Blend.

## <a name="getting-started-with-composition-effects"></a>Prise en main des effets de composition

Ce didacticiel de démarrage rapide vous montre comment utiliser certaines fonctionnalités de base des effets.

- [Installation de Visual Studio](./composition-effects.md#installing-visual-studio)
- [Création d’un projet](./composition-effects.md#creating-a-new-project)
- [L’installation Win2D](./composition-effects.md#installing-win2d)
- [Définition de vos bases de Composition](./composition-effects.md#setting-your-composition-basics)
- [Création d’un pinceau CompositionSurface](./composition-effects.md#creating-a-compositionsurface-brush)
- [Création, la compilation et l’application d’effets](./composition-effects.md#creating-compiling-and-applying-effects)

### <a name="installing-visual-studio"></a>Installation de Visual Studio

- Si vous n’avez pas installé une version prise en charge de Visual Studio, accédez à la page de téléchargements de Visual Studio [ici](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs).

### <a name="creating-a-new-project"></a>Création d’un projet

- Accédez à Fichier-&gt;Nouveau -&gt;Projet...
- Sélectionnez Visual C#.
- Créez une application vide (Windows universelle) (Visual Studio 2015).
- Entrez le nom de projet de votre choix.
- Cliquez sur OK.

### <a name="installing-win2d"></a>Installation de Win2D

Win2D est publié en tant que package Nuget.org et doit être installé avant que vous ne puissiez utiliser des effets.

Il existe deux versions de package, l’une pour Windows 10 et l’autre pour Windows 8.1. Pour les effets de composition, vous allez utiliser la version de Windows 10.

- Lancez le Gestionnaire de package NuGet en accédant à Outils → Gestionnaire de package NuGet → Gérer les packages NuGet pour la solution.
- Recherchez Win2D et sélectionnez le package approprié pour votre version cible de Windows. Comme Windows.UI. Composition prend en charge Windows 10 (et non Windows 8.1), sélectionnez Win2D.uwp.
- Acceptez le contrat de licence.
- Cliquez sur Fermer.

Dans les étapes suivantes, nous allons utiliser les API de composition pour appliquer un effet de saturation à cette image de chat, qui supprimera toute la saturation. Dans ce modèle, l’effet est créé, puis appliqué à une image.

![Image source](images/composition-cat-source.png)
### <a name="setting-your-composition-basics"></a>Définition des bases de votre composition

Consultez l’[exemple d’arborescence d’éléments visuels de composition](https://go.microsoft.com/fwlink/?LinkId=785345) sur notre GitHub pour obtenir un exemple de configuration de Windows.UI.Composition Compositor, du paramètre ContainerVisual racine, et l’associer à la fenêtre principale.

```cs
_compositor = new Compositor();
_root = _compositor.CreateContainerVisual();
_target = _compositor.CreateTargetForCurrentView();
_target.Root = _root;
_imageFactory = new CompositionImageFactory(_compositor)
Desaturate();
```

### <a name="creating-a-compositionsurface-brush"></a>Création d’un pinceau CompositionSurface

```cs
CompositionSurfaceBrush surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(surfaceBrush);
```

### <a name="creating-compiling-and-applying-effects"></a>Création, compilation et application d’effets

1. Créer l’effet graphique

    ```cs
    var graphicsEffect = new SaturationEffect
    {
      Saturation = 0.0f,
      Source = new CompositionEffectSourceParameter("mySource")
    };
    ```

1. Compiler l’effet et créer le pinceau à effets

    ```cs
    var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);

    var catEffect = effectFactory.CreateBrush();
    catEffect.SetSourceParameter("mySource", surfaceBrush);
    ```

1. Créer un élément SpriteVisual dans l’arborescence de composition et appliquer l’effet

    ```cs
    var catVisual = _compositor.CreateSpriteVisual();
      catVisual.Brush = catEffect;
      catVisual.Size = new Vector2(219, 300);
      _root.Children.InsertAtBottom(catVisual);
    }
    ```

1. Créer votre source d’image à charger.

    ```cs
    CompositionImage imageSource = _imageFactory.CreateImageFromUri(new Uri("ms-appx:///Assets/cat.png"));
    CompositionImageLoadResult result = await imageSource.CompleteLoadAsync();
    if (result.Status == CompositionImageLoadStatus.Success)
    ```

1. Dimensionner et brosser la surface de l’élément SpriteVisual

    ```cs
    brush.Surface = imageSource.Surface;
    ```

1. Exécuter votre application : le résultat doit être une image de chat désaturée :

![Image désaturée](images/composition-cat-desaturated.png)

## <a name="more-information"></a>Plus d’informations

- [Microsoft – GitHub de Composition](https://github.com/microsoft/WindowsCompositionSamples)
- [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
- [Équipe de Composition de Windows sur Twitter](https://twitter.com/wincomposition)
- [Vue d’ensemble de la composition](https://blogs.windows.com/buildingapps/2015/12/08/awaken-your-creativity-with-the-new-windows-ui-composition/)
- [Principes fondamentaux d’arborescence d’éléments visuels](composition-visual-tree.md)
- [Pinceaux de composition](composition-brushes.md)
- [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)
- [Vue d’ensemble de l’animation](composition-animation.md)
- [COMPOSITION natif DirectX et Direct2D l’interopérabilité avec BeginDraw et EndDraw](composition-native-interop.md)
