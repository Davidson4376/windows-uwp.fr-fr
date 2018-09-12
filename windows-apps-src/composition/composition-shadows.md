---
author: daneuber
title: Ombres de composition
description: L’ombre API vous permettent d’ajouter des ombres personnalisables dynamiques vers le contenu de l’interface utilisateur.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 84e12d6c3e25a18902aaa55011949dd5b5ff97ca
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/12/2018
ms.locfileid: "3932080"
---
# <a name="shadows-in-windows-ui"></a>Ombres dans l’interface utilisateur de Windows

La classe [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) fournit un moyen de créer une ombre configurable qui peut être appliquée à un [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) ou un [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (sous-arbre d’éléments visuels). Comme c’est habituel pour les objets dans la couche visuelle, toutes les propriétés de la DropShadow peuvent être animées à l’aide de CompositionAnimations.

## <a name="basic-drop-shadow"></a>Ombre de base

Pour créer une ombre de base, simplement créer un nouveau DropShadow et l’associer à votre objet visuel. L’ombre est rectangulaire par défaut. Un ensemble standard de propriétés sont disponibles pour modifier l’aspect de l’ombre.

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

Il existe quelques façons de définir la forme de votre DropShadow:

- **Utilisez la valeur par défaut** - par défaut la forme DropShadow est définie par le mode «Par défaut» sur CompositionDropShadowSourcePolicy. Pour SpriteVisual, la valeur par défaut est rectangulaire, sauf si un masque est fourni. Pour LayerVisual, la valeur par défaut est hériter d’un masque à l’aide de la valeur alpha du pinceau de la de l’objet visuel.
- **Définir un masque** , vous pouvez définir la propriété [Mask](/uwp/api/windows.ui.composition.dropshadow.mask) pour définir un masque d’opacité de l’ombre.
- **Spécifier d’utiliser Inherited masque** – définir la propriété [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) à utiliser [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent d’utiliser le masque généré à partir de la valeur alpha du pinceau de la de l’objet visuel.

## <a name="masking-to-match-your-content"></a>Masquage pour faire correspondre votre contenu

Si vous souhaitez que votre ombre pour faire correspondre le contenu de l’objet visuel vous pouvez utiliser le pinceau des visuels de votre propriété de masque de clichés instantanés, ou la valeur de l’ombre pour hériter automatiquement masque le contenu. Si vous utilisez un LayerVisual, l’ombre héritent le masque par défaut.

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

Dans certains cas, vous souhaiterez peut-être l’ombre de la forme où il ne correspond pas à des contenus de votre visuel. Pour obtenir cet effet, vous devez définir explicitement la propriété masque à l’aide d’un pinceau avec alpha.

Dans le sous exemple, nous charger deux surfaces - un pour le contenu visuel et un pour le masque de l’ombre:

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

![Image web connectés avec ombre portée masquée de cercle](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>L’animation

Selon la norme dans la couche visuelle, DropShadow propriétés peuvent être animées à l’aide d’Animations de la Composition. Ci-après, nous modifions le code à partir de l’exemple pincée ci-dessus pour animer le rayon de flou de l’ombre.

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

Si vous souhaitez ajouter une ombre à des éléments plus complexes de l’infrastructure, il existe deux façons pour l’interopérabilité avec les ombres entre XAML et Composition:

1. Utilisez le [DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) disponible dans le Shared Computer Toolkit de communauté de Windows. Reportez-vous à la [documentation de DropShadowPanel](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) pour plus d’informations sur la façon de l’utiliser.
1. Créer un visuel pour l’utiliser comme hôte de clichés instantanés et la lier à un document XAML Visual.
1. Utilisez le contrôle de la galerie exemples de Composition [SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) personnalisé CompositionShadow. Consultez l’exemple ici pour afficher la syntaxe.

## <a name="performance"></a>Performances

Bien que la couche visuelle a plusieurs optimisations en place pour rendre les effets efficace et utilisable, la génération des ombres peut être une opération relativement coûteuse, selon les options que vous définissez. Voici le niveau «cher» pour différents types d’ombres. Notez que bien que certaines ombres peut s’avérer coûteux qu’ils peuvent toujours être appropriés d’utiliser avec parcimonie, dans certains scénarios.

Caractéristiques des clichés instantanés| Coût
------------- | -------------
Capture rectangulaire    | Faible
Shadow.Mask      | High
CompositionDropShadowSourcePolicy.InheritFromVisualContent | High
Rayon de flou statique | Faible
Animer le rayon de flou | High

## <a name="additional-resources"></a>Ressources supplémentaires

- [COMPOSITION DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [Mis en pension de WindowsUIDevLabs GitHub](https://github.com/Microsoft/WindowsUIDevLabs)