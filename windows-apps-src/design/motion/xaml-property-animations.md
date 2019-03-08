---
title: Animations de propriété XAML
description: Animer des éléments XAML avec des animations de composition.
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 81da1e769ab171e47a4f4046e8ec7e7c84ecf2d1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630354"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>Animer des éléments XAML avec des animations de composition

Cet article présente les nouvelles propriétés qui vous permettent d’animer un UIElement XAML avec les performances des animations de composition et la facilité de la définition des propriétés XAML.

Avant Windows 10, version 1809, vous aviez 2 choices afin de créer des animations dans vos applications UWP :

- Utilisez les constructions XAML, telles que [animations de storyboard](storyboarded-animations.md), ou le _* ThemeTransition_ et _* ThemeAnimation_ classes dans le [ Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation) espace de noms.
- utiliser des animations de composition comme décrit dans [à l’aide de la couche visuelle avec XAML](../../composition/using-the-visual-layer-with-xaml.md).

À l’aide de la couche visuelle offre de meilleures performances qu’à l’aide de le XAML construit. Mais l’utilisation [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) pour obtenir l’élément sous-jacent composition [Visual](/uwp/api/windows.ui.composition.visual) objet et puis animer l’élément visuel avec des animations de composition, est plus complexe à utiliser.

À compter de Windows 10, version 1809, vous pouvez animer des propriétés sur un UIElement directement à l’aide d’animations de composition sans la nécessité pour obtenir la composition sous-jacent Visual.

> [!NOTE]
> Pour utiliser ces propriétés sur UIElement, votre version cible du projet UWP doit être 1809 ou version ultérieure. Pour plus d’informations sur la version de votre projet de configuration, consultez [Version des applications flexibles](../../debug-test-perf/version-adaptive-apps.md).

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>Nouvelles propriétés de rendu remplacement les anciennes propriétés de rendu

Ce tableau présente les propriétés que vous pouvez utiliser pour modifier le rendu d’un UIElement, qui peut également être animé avec un [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation).

| Propriété | Type | Description |
| -- | -- | -- |
| [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | Le degré d’opacité de l’objet |
| [Translation](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Décaler la position X/Y/Z de l’élément |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | La matrice de transformation à appliquer à l’élément |
| [Mettre à l’échelle](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | Mettre à l’échelle de l’élément, centrée sur le point central |
| [Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | Flottante | Faire pivoter l’élément autour de le RotationAxis et le point central |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | L’axe de rotation |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | Le point central de la mise à l’échelle et rotation |

La valeur de propriété TransformMatrix est combinée avec les propriétés de mise à l’échelle, de Rotation et de traduction dans l’ordre suivant :  TransformMatrix, Scale, Rotation, Translation.

Ces propriétés n’affectent la disposition de l’élément, par conséquent, modification de ces propriétés n’entraîne pas un nouveau [mesure](/uwp/api/windows.ui.xaml.uielement.measure)/[organiser](/uwp/api/windows.ui.xaml.uielement.arrange) passer.

Ces propriétés ont le même objet et le même comportement que les propriétés de même nom sur la composition [Visual](/uwp/api/windows.ui.composition.visual) classe (à l’exception de la traduction, qui n’est pas sur visuel).

### <a name="example-setting-the-scale-property"></a>Exemple : Définition de la propriété de mise à l’échelle

Cet exemple montre comment définir la propriété de mise à l’échelle sur un bouton.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>Exclusivité mutuelle entre les anciennes et nouvelles propriétés

> [!NOTE]
> Le **opacité** propriété n’applique pas l’exclusivité mutuelle décrite dans cette section. Vous utilisez la même propriété d’opacité que vous utilisiez les animations XAML ou de composition.

Les propriétés qui peuvent être animées qu’avec une CompositionAnimation sont des remplacements pour plusieurs propriétés UIElement existantes :

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projection](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

Lorsque vous définissez (ou animez) les nouvelles propriétés, vous ne pouvez pas utiliser les anciennes propriétés. À l’inverse, si vous définissez (ou animez) les propriétés ancien, vous ne pouvez pas utiliser les nouvelles propriétés.

Vous ne pouvez pas utiliser les nouvelles propriétés si vous utilisez ElementCompositionPreview pour obtenir et de gérer l’élément visuel vous-même à l’aide de ces méthodes :

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> Vous essayez de combiner l’utilisation des deux jeux de propriétés entraîne l’appel d’API à échouer et produire un message d’erreur.

Il est possible de basculer d’un ensemble de propriétés en désactivant les, bien que, par souci de simplicité il n’est pas recommandé. Si la propriété est soutenue par un DependencyProperty (par exemple, UIElement.Projection est pris en charge par UIElement.ProjectionProperty), puis appelez ClearValue afin de rétablir son état « inutilisé ». Sinon (par exemple, la propriété de mise à l’échelle), définissez la propriété à sa valeur par défaut.

## <a name="animating-uielement-properties-with-compositionanimation"></a>Animer des propriétés de UIElement avec CompositionAnimation

Vous pouvez animer les propriétés de rendu répertoriées dans la table avec une CompositionAnimation. Ces propriétés peuvent également être référencées par un [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation).

Utilisez le [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) et [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) méthodes sur un UIElement pour animer les propriétés de UIElement.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>Exemple : Anime la propriété à l’échelle avec un Vector3KeyFrameAnimation

Cet exemple montre comment animer la mise à l’échelle d’un bouton.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>Exemple : Anime la propriété à l’échelle avec une ExpressionAnimation

Une page comporte deux boutons. Le deuxième bouton anime pour être deux fois (via la mise à l’échelle) en tant que le premier bouton.

```xaml
<Button x:Name="sourceButton" Content="Source"/>
<Button x:Name="destinationButton" Content="Destination"/>
```

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateExpressionAnimation("sourceButton.Scale*2");
animation.SetExpressionReferenceParameter("sourceButton", sourceButton);
animation.Target = "Scale";
destinationButton.StartAnimation(animation);
```

## <a name="related-topics"></a>Rubriques connexes

- [Animations de storyboard](storyboarded-animations.md)
- [À l’aide de la couche visuelle avec XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [Vue d’ensemble des transformations](../layout/transforms.md)
