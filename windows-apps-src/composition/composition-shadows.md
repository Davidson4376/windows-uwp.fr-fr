---
title: Ombres de composition
description: L’API de l’ombre vous permettre d’ajouter des ombres personnalisables dynamiques à du contenu de l’interface utilisateur.
ms.date: 07/16/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9541ea1c00d473bc4881a80d8597625592e278f9
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7703173"
---
# <a name="shadows-in-windows-ui"></a>Ombres dans l’interface utilisateur de Windows

La classe [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) fournit un moyen de créer une ombre configurable qui peut être appliquée à un [élément SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) ou un [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (sous-arborescence d’éléments visuels). Comme est d’usage pour les objets de la couche visuelle, toutes les propriétés de la DropShadow peuvent être animées à l’aide des CompositionAnimations.

## <a name="basic-drop-shadow"></a>Ombre de base

Pour créer une ombre de base, simplement créer un nouveau DropShadow et l’associer à votre élément visuel. L’ombre est rectangulaire par défaut. Un ensemble de propriétés standard sont disponibles pour modifier l’apparence de l’ombre.

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

![Visual rectangulaire avec DropShadow de base](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>La mise en forme de l’ombre

Il existe plusieurs façons de définir la forme pour votre DropShadow:

- **Utilisez la valeur par défaut** - par défaut la forme DropShadow est définie par le mode «Default» sur CompositionDropShadowSourcePolicy. Pour SpriteVisual, la valeur par défaut est rectangulaire, sauf si un masque est fourni. Pour LayerVisual, la valeur par défaut est hériter un masque à l’aide de la valeur alpha du pinceau de l’élément visuel.
- **Définir un masque** : vous pouvez définir la propriété [masque](/uwp/api/windows.ui.composition.dropshadow.mask) pour définir un masque d’opacité de l’ombre.
- **Spécifiez à utiliser les héritées masque** – définir la propriété de [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) à utiliser [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent à utiliser le masque généré à partir de la valeur alpha du pinceau de l’élément visuel.

## <a name="masking-to-match-your-content"></a>Masquage pour faire correspondre votre contenu

Si vous souhaitez que votre ombre pour faire correspondre le contenu de l’élément visuel vous pouvez utiliser un pinceau de l’élément visuel pour que votre propriété de masque d’ombre, ou définir l’ombre automatiquement hériter masque le contenu. Si vous utilisez un LayerVisual, l’ombre héritera le masque par défaut.

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

![Image web connectés avec ombre portée masquée](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>À l’aide d’un masque de remplacement

Dans certains cas, vous souhaiterez l’ombre de forme tels qu’il ne correspond pas au contenu de votre élément visuel. Pour obtenir cet effet, vous devez définir explicitement la propriété masque à l’aide d’un pinceau avec alpha.

Dans l’exemple indiqué ci-dessous, nous chargeons deux surfaces - un pour le contenu visuel et un pour le masque de clichés instantanés:

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

![Image web connectés avec un cercle masquée ombre portée](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Animation

Selon la norme de la couche visuelle, DropShadow propriétés peuvent être animées à l’aide d’Animations de Composition. Ci-dessous, nous modifions le code à partir de l’exemple de faible pluie ci-dessus pour animer le rayon de flou de l’ombre.

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>Ombres en XAML

Si vous souhaitez ajouter une ombre à des éléments de l’infrastructure plus complexes, il existe deux façons pour permettre l’interopérabilité avec les ombres entre XAML et de Composition:

1. Utilisez le [DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) disponible dans le Kit de ressources de la Communauté Windows. Consultez la [documentation de DropShadowPanel](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) pour plus d’informations sur son utilisation.
1. Créer un élément visuel pour utiliser en tant que l’hôte de l’ombre et la lier au visuel de document XAML.
1. Utiliser le contrôle de CompositionShadow personnalisé de la galerie d’exemples Composition [SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) . Voir l’exemple ci-dessous pour afficher la syntaxe.

## <a name="performance"></a>Analyse des performances

Bien que la couche visuelle possède de nombreuses optimisations en place pour rendre les effets efficace et utilisable, la génération d’ombres peut être une opération relativement coûteuse selon les options que vous définissez. Vous trouverez ci-dessous des niveau «coûts élevés» pour différents types d’ombres. Notez que bien que certaines ombres peut s’avérer coûteux qu’ils peuvent toujours être appropriés à utiliser avec parcimonie, dans certains scénarios.

Caractéristiques de l’ombre| Coût
------------- | -------------
Capture rectangulaire    | Faible
Shadow.Mask      | High
CompositionDropShadowSourcePolicy.InheritFromVisualContent | High
Rayon de flou statique | Faible
Animation de flou Radius | High

## <a name="additional-resources"></a>Ressources complémentaires

- [COMPOSITION DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [Référentiel de GitHub WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs)