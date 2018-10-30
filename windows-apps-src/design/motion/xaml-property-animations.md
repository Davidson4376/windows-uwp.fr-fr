---
author: Jwmsft
title: Animations de propriété XAML
description: Animation d’éléments XAML avec des animations de composition.
ms.author: jimwalk
ms.date: 09/13/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.openlocfilehash: 9372ba818805446948a444632e809ec06691c5e5
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5766174"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>Animation d’éléments XAML avec des animations de composition

Cet article présente les nouvelles propriétés qui vous permettent d’animer un élément UIElement XAML avec les performances des animations de composition et la facilité de définition des propriétés XAML.

Avant Windows 10, version 1809, vous deviez 2 choix pour créer des animations dans vos applications UWP:

- utiliser des constructions de XAML comme [des animations Storyboarded](storyboarded-animations.md), ou le _* ThemeTransition_ et _* ThemeAnimation_ classes dans l’espace de noms [Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation) .
- Utilisez les animations de composition comme décrit à l’aide de [La couche visuelle avec XAML](../../composition/using-the-visual-layer-with-xaml.md).

À l’aide de la couche visuelle offre de meilleures performances que construit à l’aide du code XAML. Toutefois, l’utilisation de [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) pour obtenir [visuel](/uwp/api/windows.ui.composition.visual) objet de composition sous-jacent l’élément et puis animer l’élément visuel avec des animations de composition, sont plus complexe à utiliser.

À compter de Windows 10, version 1809, vous pouvez animer des propriétés sur un élément UIElement directement à l’aide des animations de composition sans la configuration requise pour obtenir la composition sous-jacent Visual.

> [!NOTE]
> Pour utiliser ces propriétés sur UIElement, votre version cible du projet UWP doit être 1809 ou une version ultérieure. Pour plus d’informations sur la configuration de la version de votre projet, reportez-vous à la section [applications adaptatives de Version](../../debug-test-perf/version-adaptive-apps.md).

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>Nouvelles propriétés de rendu remplacement anciennes propriétés de rendu

Ce tableau indique les propriétés que vous pouvez utiliser pour modifier le rendu d’un élément UIElement, qui peut également être animé avec une [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation).

| Propriété | Type | Description |
| -- | -- | -- |
| [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | Le degré d’opacité de l’objet |
| [Translation](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Décaler la position X/Y/Z de l’élément |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | La matrice de transformation à appliquer à l’élément |
| [Scale](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | Mettre à l’échelle de l’élément, centrée sur le point central |
| [Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | Flottant | Faire pivoter l’élément autour de la RotationAxis et le point central |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | L’axe de rotation |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | Le point central de mise à l’échelle et la rotation |

La valeur de propriété TransformMatrix est combinée avec les propriétés de mise à l’échelle, la Rotation et Translation dans l’ordre suivant: TransformMatrix, l’échelle, Rotation, Translation.

Ces propriétés n’affectent pas la disposition de l’élément, par conséquent, modifiant ces propriétés n’entraîne pas une nouvelle [mesure](/uwp/api/windows.ui.xaml.uielement.measure)/passe[d’organisation](/uwp/api/windows.ui.xaml.uielement.arrange) .

Ces propriétés ont le même objectif et le même comportement que les propriétés de même nom sur la composition [visuelle](/uwp/api/windows.ui.composition.visual) classe (à l’exception de traduction, qui n’est pas sur visuel).

### <a name="example-setting-the-scale-property"></a>Exemple: Définition de la propriété à l’échelle

Cet exemple montre comment définir la propriété à l’échelle sur un bouton.

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
> La propriété **Opacity** n’applique pas l’exclusivité mutuelle décrite dans cette section. Vous utilisez la propriété Opacity même si vous utilisez les animations de composition ou XAML.

Les propriétés qui peuvent être animées avec une CompositionAnimation sont les remplacements pour plusieurs propriétés UIElement existantes:

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projection](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

Lorsque vous définissez (ou animez) une des nouvelles propriétés, vous ne pouvez pas utiliser les propriétés anciennes. À l’inverse, si vous définissez (ou animez) toutes les propriétés anciennes, vous ne pouvez pas utiliser les nouvelles propriétés.

Vous ne pouvez pas utiliser les nouvelles propriétés si vous utilisez ElementCompositionPreview pour obtenir et gérer l’élément visuel vous-même à l’aide de ces méthodes:

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> Toute tentative de combiner l’utilisation des deux jeux de propriétés provoquera l’appel d’API échouer et produire un message d’erreur.

Il est possible de passer d’un ensemble de propriétés en désactivant les, mais pour simplifier, il est déconseillé. Si la propriété est renforcée par un DependencyProperty (par exemple, UIElement.Projection est renforcé par UIElement.ProjectionProperty), puis appelez ClearValue pour la restaurer à son état «inutilisé». Dans le cas contraire (par exemple, la propriété échelle), définissez la propriété à sa valeur par défaut.

## <a name="animating-uielement-properties-with-compositionanimation"></a>Animer des propriétés UIElement avec CompositionAnimation

Vous pouvez animer les propriétés de rendu répertoriées dans le tableau avec une CompositionAnimation. Ces propriétés peuvent également être référencées par un [élément ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation).

Utilisez les méthodes [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) et [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) sur UIElement d’animer les propriétés de l’élément UIElement.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>Exemple: Anime la propriété à l’échelle avec un Vector3KeyFrameAnimation

Cet exemple montre comment animer l’échelle d’un bouton.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>Exemple: Anime la propriété à l’échelle avec un élément ExpressionAnimation

Une page possède deux boutons. Le deuxième bouton s’anime à deux fois plus large (via l’échelle) en tant que le premier bouton.

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

## <a name="related-topics"></a>Rubriquesconnexes

- [Animations dans une table de montage](storyboarded-animations.md)
- [Utilisation de la couche visuelle avec le contenu XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [Vue d’ensemble des transformations](../layout/transforms.md)