---
title: Ombres de composition
description: L’API de l’ombre vous permettre d’ajouter des ombres personnalisables dynamiques au contenu de l’interface utilisateur.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9541ea1c00d473bc4881a80d8597625592e278f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630834"
---
# <a name="shadows-in-windows-ui"></a>Ombres dans l’interface utilisateur de Windows

Le [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) classe fournit les moyens de la création d’une ombre configurable qui peut être appliquée à un [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) ou [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (sous-arborescence d’éléments visuels). Comme est courant pour les objets dans la couche visuelle, toutes les propriétés de la DropShadow peuvent être animées à l’aide de CompositionAnimations.

## <a name="basic-drop-shadow"></a>Ombre de base

Pour créer un cliché instantané de base, simplement créer un nouveau DropShadow et associez-le à votre élément visuel. L’ombre est rectangulaire par défaut. Un ensemble standard de propriétés sont disponibles pour modifier l’apparence de l’ombre.

```cs
var basicRectVisual = _compositor.CreateSpriteVisual();
basicRectVisual.Brush = _compositor.CreateColorBrush(Colors.Blue);
basicRectVisual.Offset = new Vector3(100, 100, 20);
basicRectVisual.Size = new Vector2(300, 300);

var basicShadow = _compositor.CreateDropShadow();
basicShadow.BlurRadius = 25f;
basicShadow.Offset = new Vector3(20, 20, 20);

basicRectVisual.Shadow = basicShadow;
```

![Visual rectangulaire avec base DropShadow](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>Mise en forme de l’ombre

Il existe quelques façons de définir la forme pour votre DropShadow :

- **Utilisez la valeur par défaut** - par défaut, la forme DropShadow est définie par le mode 'Default' sur CompositionDropShadowSourcePolicy. Pour SpriteVisual, la valeur par défaut est rectangulaires, sauf si un masque est fourni. Pour LayerVisual, la valeur par défaut est d’hériter d’un masque à l’aide de la valeur alpha de pinceau de l’élément du visuel.
- **Définir un masque** – vous pouvez définir le [masque](/uwp/api/windows.ui.composition.dropshadow.mask) propriété pour définir un masque d’opacité de l’ombre.
- **Spécifiez l’utilisation de l’héritage masque** : définissez le [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) propriété à utiliser [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent à utiliser le masque généré à partir de la valeur alpha de pinceau de l’élément du visuel.

## <a name="masking-to-match-your-content"></a>Masquage pour correspondre à votre contenu

Si vous souhaitez que votre ombre à faire correspondre le contenu de l’élément visuel vous pouvez soit utiliser les pinceau de l’élément visuel pour votre propriété de masque de clichés instantanés, ou définir l’ombre d’hériter automatiquement masque le contenu. Si vous utilisez un LayerVisual, l’ombre hériteront le masque par défaut.

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = imageBrush;
// or use shadow.SourcePolicy = CompositionDropShadowSourcePolicy.InheritFromVisualContent;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![Image web connectés avec ombre portée sont masquées](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>À l’aide d’un autre masque

Dans certains cas, vous souhaiterez peut-être mettre en forme l’ombre de sorte qu’il ne correspond pas aux contenus de votre élément visuel. Pour obtenir cet effet, vous devez définir explicitement la propriété de masque à l’aide d’un pinceau avec alpha.

Dans l’exemple ci-dessous, nous chargeons deux surfaces - un pour le contenu visuel et un pour le masque de clichés instantanés :

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var circleSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myCircleImage.png"));
var customMask = _compositor.CreateSurfaceBrush(circleSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = customMask;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![Image web connectés avec ombre portée cercle masquée](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Animation

Comme c’est le standard dans la couche visuelle, des propriétés de DropShadow peuvent être animées pour l’utilisation des Animations de Composition. Ci-dessous, nous modifions le code de l’exemple d’une pincée ci-dessus pour animer le rayon de flou de l’ombre.

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>Shadows dans XAML

Si vous souhaitez ajouter une ombre à des éléments de framework plus complexes, il existe plusieurs façons pour assurer l’interopérabilité avec les ombres entre XAML et la Composition :

1. Utilisez le [DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) disponibles dans le Kit de ressources de communauté de Windows. Consultez le [DropShadowPanel documentation](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) pour plus d’informations sur comment l’utiliser.
1. Créer un visuel pour les utiliser en tant que l’hôte de clichés instantanés & Lier à la documentation XAML visuel.
1. Utilisation de la galerie d’exemples Composition [SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) contrôle CompositionShadow personnalisé. Consultez l’exemple présenté ici pour l’utilisation.

## <a name="performance"></a>Performances

Bien que la couche visuelle a de nombreuses optimisations en place pour rendre les effets efficace et utilisable, la génération des ombres peut être une opération relativement coûteuse selon les options que vous définissez. Voici le niveau « coûts élevés » pour différents types d’ombres. Notez que bien que certaines shadows peuvent coûter cher elles peuvent toujours être approprié d’utiliser avec parcimonie dans certains scénarios.

Caractéristiques des clichés instantanés| Coût
------------- | -------------
Capture rectangulaire    | Faible
Shadow.Mask      | Élevée
CompositionDropShadowSourcePolicy.InheritFromVisualContent | Élevée
Rayon de flou statique | Faible
Animer le rayon de flou | Élevée

## <a name="additional-resources"></a>Ressources complémentaires

- [COMPOSITION DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [Référentiel GitHub de WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs)