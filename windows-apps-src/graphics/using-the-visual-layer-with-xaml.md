---
author: jaster
ms.assetid: 
title: Utilisation de la couche visuelle avec le contenu XAML
description: "Découvrez les techniques d’utilisation des API de couche visuelle avec le contenu XAML existant pour créer des effets et des animations avancés."
translationtype: Human Translation
ms.sourcegitcommit: dfda33c70224f32d9c3e8877eabdfcd965521757
ms.openlocfilehash: 00d663b130202f4513cd1a9d82baed4068d909d3

---

# Utilisation de la couche visuelle avec le contenu XAML

## Introduction

La plupart des applications qui utilisent des fonctionnalités de couche visuelle font appel à l’infrastructure XAML pour définir le contenu d’interface utilisateur principal. Dans la mise à jour anniversaire Windows10, il existe de nouvelles fonctionnalités dans l’infrastructure XAML et la couche visuelle qui facilitent la combinaison de ces deux technologies pour créer des expériences utilisateur incroyables.
La fonctionnalité «d’interopérabilité» entre l’infrastructure XAML et la couche visuelle peut être utilisée pour créer des effets et des animations avancés qui ne sont pas disponibles à l’aide des API XAML seules. Cela comprend les éléments suivants:

-              Parallaxe et animations par défilement
-              Animations de disposition automatique
-              Ombres portées au pixel près
-              Effets de flou et verre givré

Ces effets et animations peuvent être appliqués au contenu XAML existant, vous n’avez donc pas besoin de restructurer énormément votre application XAML pour tirer parti des nouvelles fonctionnalités.
Les animations de disposition, les ombres et les effets de flou sont abordés dans la section Recettes ci-après. Pour obtenir un exemple de code implémentant la parallaxe, consultez [l’exemple ParallaxingListItems](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems). Le [référentiel WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) comprend également plusieurs exemples pour implémenter des animations, des ombres et des effets.

## La classe **ElementCompositionPreview**

[**ElementCompositionPreview**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.aspx) est une classe statique qui fournit une fonctionnalité d’interopérabilité entre l’infrastructure XAML et la couche visuelle. Pour une vue d’ensemble de la couche visuelle et de ses fonctionnalités, consultez [Couche visuelle](https://msdn.microsoft.com/en-us/windows/uwp/graphics/visual-layer). La classe **ElementCompositionPreview** propose les méthodes suivantes:

-   [**GetElementVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): obtenez un visuel de «document» qui est utilisé pour générer cet élément.
-   [**SetElementChildVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual.aspx): définissez un visuel d’élément comme dernier enfant de l’arborescence visuelle de cet élément. Ce visuel pourra dessiner par-dessus le reste de l’élément. 
-   [**GetElementChildVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): récupérez le visuel défini à l’aide de **SetElementChildVisual**.
-   [**GetScrollViewerManipulationPropertySet**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): obtenez un objet permettant de créer des animations de 60FPS en fonction du décalage de défilement dans un élément **ScrollViewer**.

## Remarques sur **ElementCompositionPreview.GetElementVisual**

**ElementCompositionPreview.GetElementVisual** renvoie un visuel de «document» qui est utilisé pour générer l’élément **UIElement** donné. Les propriétés telles que **Visual.Opacity**, **Visual.Offset** et **Visual.Size** sont définies par l’infrastructure XAML en fonction de l’état de l’élément UIElement. Cela permet d’activer des techniques telles que les animations de repositionnement implicite (consultez *Recettes*).

Notez que, dans la mesure où les éléments **Offset** et **Size** sont définis comme le résultat d’une disposition d’infrastructure XAML, les développeurs doivent être prudents lors de la modification ou de l’animation de ces propriétés. Les développeurs doivent uniquement modifier ou animer un élément Offset lorsque l’angle supérieur gauche de l’élément a la même position que celle de son parent dans la disposition. L’élément Size ne doit généralement pas être modifié, mais accéder à la propriété peut être utile. Par exemple, les exemples d’ombre portée et de verre givré ci-dessous utilisent la propriété Size d’un visuel de document comme entrée d’une animation.

Notez également que les propriétés mises à jour de visuel de document ne seront pas reflétées dans l’élément UIElement correspondant. Ainsi, quand vous définissez par exemple **UIElement.Opacity** sur 0,5, la propriété Opacity du visuel de document correspondant sera définie sur 0,5. Cependant, définir la propriété **Opacity** du visuel de document sur 0,5 entraînera l’affichage du contenu avec une opacité de 50%, mais ne modifiera pas la valeur de propriété Opacity de l’élément UIElement correspondant.

### Exemple d’animation **Offset**

#### Incorrect

```xml
<Border>
      <Image x:Name="MyImage" Margin="5" />
</Border>
```

```csharp
// Doesn’t work because Image has a margin!
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

#### Correct

```xml
<Border>
    <Canvas Margin="5">
        <Image x:Name="MyImage" />
    </Canvas>
</Border>
```

```csharp
// This works because the Canvas parent doesn’t generate a layout offset.
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

## La méthode **ElementCompositionPreview.SetElementChildVisual**

**ElementCompositionPreview.SetElementChildVisual** permet aux développeurs de fournir un visuel d’élément qui s’affichera comme composant de l’arborescence visuelle d’un élément. Cela permet aux développeurs de créer une «île de composition» où le contenu visuel peut s’afficher dans une interface utilisateur XAML. Les développeurs doivent être prudents avec l’utilisation de cette technique dans la mesure où le contenu visuel ne bénéficiera pas des mêmes accessibilité et garanties d’expérience utilisateur que le contenu XAML. Par conséquent, il est généralement recommandé d’utiliser cette technique uniquement lorsque cela est nécessaire pour implémenter des effets personnalisés tels que ceux disponibles dans la section Recettes ci-dessous.

## Méthodes **GetAlphaMask**

Les éléments [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752), [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) et [**Shape**](https://msdn.microsoft.com/library/windows/apps/br243377) implémentent chacun une méthode appelée **GetAlphaMask** qui renvoie un élément **CompositionBrush** représentant une image en nuances de gris avec la forme de l’élément. Cet élément **CompositionBrush** peut servir d’entrée pour une Composition **DropShadow**, de sorte que l’ombre puisse refléter la forme de l’élément plutôt qu’un rectangle. Cela permet d’obtenir des ombres de contour au pixel près pour le texte, les images avec masque alpha et les formes. Consultez la section *Ombre portée* ci-dessous pour obtenir un exemple de cette API.

## Recettes

### Animation de repositionnement

À l’aide des animations implicites de composition, un développeur peut automatiquement animer les changements d’une disposition d’élément par rapport à son parent. Par exemple, si vous modifiez la propriété **Margin** du bouton ci-dessous, il s’anime automatiquement pour rejoindre sa nouvelle position de disposition.

#### Vue d’ensemble de l’implémentation

1.            Obtenir le **visuel** de document pour l’élément cible
2.            Créer un élément **ImplicitAnimationCollection** qui anime automatiquement les modifications dans la propriété **Offset**
3.            Associer l’élément **ImplicitAnimationCollection** au visuel de stockage

#### XAML

```xml
<Button x:Name="RepositionTarget" Content="Click Me" />
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeRepositionAnimation(RepositionTarget);
}

private void InitializeRepositionAnimation(UIElement repositionTarget)
{
    var targetVisual = ElementCompositionPreview.GetElementVisual(repositionTarget);
    Compositor compositor = targetVisual.Compositor;

    // Create an animation to animate targetVisual's Offset property to its final value
    var repositionAnimation = compositor.CreateVector3KeyFrameAnimation();
    repositionAnimation.Duration = TimeSpan.FromSeconds(0.66);
    repositionAnimation.Target = "Offset";
    repositionAnimation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");

    // Run this animation when the Offset Property is changed
    var repositionAnimations = compositor.CreateImplicitAnimationCollection();
    repositionAnimations["Offset"] = repositionAnimation;

    targetVisual.ImplicitAnimations = repositionAnimations;
}
```

### Ombre portée

Appliquez une ombre portée au pixel près à un élément **UIElement**, par exemple un élément **Ellipse** contenant une image. Étant donné que l’ombre exige qu’un élément **SpriteVisual** soit créé par l’application, nous devons créer un élément «hôte» qui contient l’élément **SpriteVisual** à l’aide de **ElementCompositionPreview.SetElementChildVisual**.

#### Vue d’ensemble de l’implémentation

1.            Obtenir le **visuel** de document pour l’élément hôte
2.            Créer un élément Windows.UI.Composition **DropShadow**
3.            Configurer l’élément **DropShadow** pour obtenir sa forme à partir de l’élément cible via un masque
    - L’élément **DropShadow** est rectangulaire par défaut, cette étape n’est donc pas nécessaire si la cible est rectangulaire
4.            Attacher l’ombre à un nouvel élément **SpriteVisual** et définir l’élément **SpriteVisual** comme l’enfant de l’élément hôte
5.            Lier la taille de l’élément **SpriteVisual** à la taille de l’hôte en utilisant une **ExpressionAnimation**

#### XAML

```xml
<Grid Width="200" Height="200">
    <Canvas x:Name="ShadowHost" />
    <Ellipse x:Name="CircleImage">
        <Ellipse.Fill>
            <ImageBrush ImageSource="Assets/Images/2.jpg" Stretch="UniformToFill" />
        </Ellipse.Fill>
    </Ellipse>
</Grid>
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

private void InitializeDropShadow(UIElement shadowHost, Shape shadowTarget)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(shadowHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a drop shadow
    var dropShadow = compositor.CreateDropShadow();
    dropShadow.Color = Color.FromArgb(255, 75, 75, 80);
    dropShadow.BlurRadius = 15.0f;
    dropShadow.Offset = new Vector3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask = shadowTarget.GetAlphaMask();

    // Create a Visual to hold the shadow
    var shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
   ElementCompositionPreview.SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual.StartAnimation("Size", bindSizeAnimation);
}
```

### Verre givré

Créez un effet qui estompe et teint le contenu en arrière-plan. Notez que les développeurs doivent installer le package NuGet Win2D pour utiliser des effets. Consultez la [page d’accueil Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) pour obtenir les instructions d’installation.

#### Vue d’ensemble de l’implémentation

1.            Obtenir le **visuel** de document pour l’élément hôte
2.            Créer une arborescence d’effet de flou à l’aide de Win2D et **CompositionEffectSourceParameter**
3.            Créer un élément **CompositionEffectBrush** en fonction de l’arborescence d’effet
4.            Définir l’entrée de l’élément **CompositionEffectBrush** sur **CompositionBackdropBrush**, ce qui permet d’appliquer un effet au contenu se trouvant derrière un élément **SpriteVisual**
5.            Définir l’élément **CompositionEffectBrush** comme le contenu d’un nouvel élément **SpriteVisual** et définir l’élément **SpriteVisual** comme l’enfant de l’élément hôte
6.            Lier la taille de l’élément **SpriteVisual** à la taille de l’hôte en utilisant une **ExpressionAnimation**

#### XAML

```xml
<Grid Width="300" Height="300" Grid.Column="1">
    <Image
        Source="Assets/Images/2.jpg"
        Width="200"
        Height="200" />
    <Canvas
        x:Name="GlassHost"
        Width="150"
        Height="300"
        HorizontalAlignment="Right" />
</Grid>
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeFrostedGlass(GlassHost);
}

private void InitializedFrostedGlass(UIElement glassHost)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(glassHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a glass effect, requires Win2D NuGet package
    var glassEffect = new GaussianBlurEffect
    { 
        BlurAmount = 15.0f,
        BorderMode = EffectBorderMode.Hard,
        Source = new ArithmeticCompositeEffect
        {
            MultiplyAmount = 0,
            Source1Amount = 0.5f,
            Source2Amount = 0.5f,
            Source1 = new CompositionEffectSourceParameter("backdropBrush"),
            Source2 = new ColorSourceEffect
            {
                Color = Color.FromArgb(255, 245, 245, 245)
            }
        }
    };

    //  Create an instance of the effect and set its source to a CompositionBackdropBrush
    var effectFactory = compositor.CreateEffectFactory(glassEffect);
    var backdropBrush = compositor.CreateBackdropBrush();
    var effectBrush = effectFactory.CreateBrush();

    effectBrush.SetSourceParameter("backdropBrush", backdropBrush);

    // Create a Visual to contain the frosted glass effect
    var glassVisual = compositor.CreateSpriteVisual();
    glassVisual.Brush = effectBrush;

    // Add the blur as a child of the host in the visual tree
    ElementCompositionPreview.SetElementChildVisual(glassHost, glassVisual);

    // Make sure size of glass host and glass visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    glassVisual.StartAnimation("Size", bindSizeAnimation);
}
```

## Ressources supplémentaires:

-   [Vue d’ensemble de couche visuelle](https://msdn.microsoft.com/en-us/windows/uwp/graphics/visual-layer)
-   [Classe **ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/mt608976)
-   Exemples d’interface utilisateur et de composition avancés dans le [GitHub WindowsUIDevLabs](https://github.com/microsoft/windowsuidevlabs).
-   [Exemple de BasicXamlInterop](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/BasicXamlInterop)
-   [Exemple de ParallaxingListItems](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)



<!--HONumber=Aug16_HO3-->


