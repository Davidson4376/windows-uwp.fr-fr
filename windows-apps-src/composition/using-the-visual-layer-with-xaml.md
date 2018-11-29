---
ms.assetid: b7a4ac8a-d91e-461b-a060-cc6fcea8e778
title: Utilisation de la couche visuelle avec le contenu XAML
description: Découvrez les techniques d’utilisation des API de couche visuelle avec le contenu XAML existant pour créer des effets et des animations avancés.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ae9bc0f6d53181a88b02ecda19b3aed745febe40
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7984244"
---
# <a name="using-the-visual-layer-with-xaml"></a>Utilisation de la couche visuelle avec le contenu XAML

La plupart des applications qui utilisent des fonctionnalités de couche visuelle font appel à l’infrastructure XAML pour définir le contenu d’interface utilisateur principal. Dans la mise à jour anniversaire Windows10, il existe de nouvelles fonctionnalités dans l’infrastructure XAML et la couche visuelle qui facilitent la combinaison de ces deux technologies pour créer des expériences utilisateur incroyables.
La fonctionnalité d’interopérabilité entre l’infrastructure XAML et la couche visuelle permet de créer des effets et des animations avancés qui ne sont pas disponibles à l’aide des APIXAML seules. Cela comprend les éléments suivants:

- Effets de pinceau comme le flou et le verre givré
- Effets d’éclairage dynamique
- Parallaxe et animations par défilement
- Animations de disposition automatique
- Ombres portées au pixel près

Comme ces effets et animations peuvent être appliqués au contenu XAML existant, vous n’avez pas besoin de restructurer énormément votre application XAML pour tirer parti des nouvelles fonctionnalités.
Les animations de disposition, les ombres et les effets de flou sont abordés dans la section Recettes ci-après. Pour obtenir un exemple de code implémentant la parallaxe, consultez [l’exemple ParallaxingListItems](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems). Le [référentiel WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) comprend également plusieurs exemples pour implémenter des animations, des ombres et des effets.

## <a name="the-xamlcompositionbrushbase-class"></a>Classe XamlCompositionBrushBase

**XamlCompositionBrush** fournit une classe de base pour les pinceaux XAML qui peignent une zone avec un élément **CompositionBrush**. Cela permet d’appliquer facilement des effets de composition, comme le flou ou le verre givré, aux éléments de l’interface utilisateur XAML.

Consultez la section [**Pinceaux**](/windows/uwp/design/style/brushes#xamlcompositionbrushbase) pour en savoir plus sur l’utilisation des pinceaux avec l’interface utilisateur XAML.

Pour obtenir des exemples de code, voir la page de référence de la classe [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="the-xamllight-class"></a>Classe XamlLight

**XamlLight** fournit une classe de base pour les effets d’éclairage XAML qui illuminent de manière dynamique une zone avec un élément **CompositionLight**.

Consultez la section [**Éclairage**](xaml-lighting.md) pour en savoir plus sur l’utilisation des éclairages, notamment les éléments d’éclairage de l’interface utilisateur XAML.

Pour obtenir des exemples de code, voir la page de référence de la classe [**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight).

## <a name="the-elementcompositionpreview-class"></a>Classe ElementCompositionPreview

[**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.aspx) est une classe statique qui fournit une fonctionnalité d’interopérabilité entre l’infrastructure XAML et la couche visuelle. Pour une vue d’ensemble de la couche visuelle et de ses fonctionnalités, consultez [Couche visuelle](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer). La classe **ElementCompositionPreview** propose les méthodes suivantes:

-   [**GetElementVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): obtenez un visuel de «document» qui est utilisé pour générer cet élément.
-   [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual.aspx): définissez un visuel d’élément comme dernier enfant de l’arborescence visuelle de cet élément. Ce visuel pourra dessiner par-dessus le reste de l’élément. 
-   [**GetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): récupérez le visuel défini à l’aide de **SetElementChildVisual**.
-   [**GetScrollViewerManipulationPropertySet**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): obtenez un objet permettant de créer des animations de 60FPS en fonction du décalage de défilement dans un élément **ScrollViewer**.

## <a name="remarks-on-elementcompositionpreviewgetelementvisual"></a>Remarques sur ElementCompositionPreview.GetElementVisual

**ElementCompositionPreview.GetElementVisual** renvoie un visuel de «document» qui est utilisé pour générer l’élément **UIElement** donné. Les propriétés telles que **Visual.Opacity**, **Visual.Offset** et **Visual.Size** sont définies par l’infrastructure XAML en fonction de l’état de l’élément UIElement. Cela permet d’activer des techniques telles que les animations de repositionnement implicite (consultez *Recettes*).

Notez que, dans la mesure où les éléments **Offset** et **Size** sont définis comme le résultat d’une disposition d’infrastructure XAML, les développeurs doivent être prudents lors de la modification ou de l’animation de ces propriétés. Les développeurs doivent uniquement modifier ou animer un élément Offset lorsque l’angle supérieur gauche de l’élément a la même position que celle de son parent dans la disposition. L’élément Size ne doit généralement pas être modifié, mais accéder à la propriété peut être utile. Par exemple, les exemples d’ombre portée et de verre givré ci-dessous utilisent la propriété Size d’un visuel de document comme entrée d’une animation.

Notez également que les propriétés mises à jour de visuel de document ne seront pas reflétées dans l’élément UIElement correspondant. Ainsi, quand vous définissez par exemple **UIElement.Opacity** sur 0,5, la propriété Opacity du visuel de document correspondant sera définie sur 0,5. Cependant, définir la propriété **Opacity** du visuel de document sur 0,5 entraînera l’affichage du contenu avec une opacité de 50%, mais ne modifiera pas la valeur de propriété Opacity de l’élément UIElement correspondant.

### <a name="example-of-offset-animation"></a>Exemple d’animation **Offset**

#### <a name="incorrect"></a>Incorrect

```xaml
<Border>
      <Image x:Name="MyImage" Margin="5" />
</Border>
```

```csharp
// Doesn’t work because Image has a margin!
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

#### <a name="correct"></a>Correct

```xaml
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

## <a name="the-elementcompositionpreviewsetelementchildvisual-method"></a>La méthode **ElementCompositionPreview.SetElementChildVisual**

**ElementCompositionPreview.SetElementChildVisual** permet aux développeurs de fournir un visuel d’élément qui s’affichera comme composant de l’arborescence visuelle d’un élément. Cela permet aux développeurs de créer une «île de composition» où le contenu visuel peut s’afficher dans une interface utilisateur XAML. Les développeurs doivent être prudents avec l’utilisation de cette technique dans la mesure où le contenu visuel ne bénéficiera pas des mêmes accessibilité et garanties d’expérience utilisateur que le contenu XAML. Par conséquent, il est généralement recommandé d’utiliser cette technique uniquement lorsque cela est nécessaire pour implémenter des effets personnalisés tels que ceux disponibles dans la section Recettes ci-dessous.

## <a name="getalphamask-methods"></a>Méthodes **GetAlphaMask**

Les éléments [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752), [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) et [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) implémentent chacun une méthode appelée **GetAlphaMask** qui renvoie un élément **CompositionBrush** représentant une image en nuances de gris avec la forme de l’élément. Cet élément **CompositionBrush** peut servir d’entrée pour une Composition **DropShadow**, de sorte que l’ombre puisse refléter la forme de l’élément plutôt qu’un rectangle. Cela permet d’obtenir des ombres de contour au pixel près pour le texte, les images avec masque alpha et les formes. Consultez la section *Ombre portée* ci-dessous pour obtenir un exemple de cette API.

## <a name="recipes"></a>Recettes

### <a name="reposition-animation"></a>Animation de repositionnement

À l’aide des animations implicites de composition, un développeur peut automatiquement animer les changements d’une disposition d’élément par rapport à son parent. Par exemple, si vous modifiez la propriété **Margin** du bouton ci-dessous, il s’anime automatiquement pour rejoindre sa nouvelle position de disposition.

#### <a name="implementation-overview"></a>Vue d’ensemble de l’implémentation

1. Obtenir le **visuel** de document pour l’élément cible
1. Créer un élément **ImplicitAnimationCollection** qui anime automatiquement les modifications dans la propriété **Offset**
1. Associer l’élément **ImplicitAnimationCollection** au visuel de stockage

```xaml
<Button x:Name="RepositionTarget" Content="Click Me" />
```

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

### <a name="drop-shadow"></a>Ombre portée

Appliquez une ombre portée au pixel près à un élément **UIElement**, par exemple un élément **Ellipse** contenant une image. Étant donné que l’ombre exige qu’un élément **SpriteVisual** soit créé par l’application, nous devons créer un élément «hôte» qui contient l’élément **SpriteVisual** à l’aide de **ElementCompositionPreview.SetElementChildVisual**.

#### <a name="implementation-overview"></a>Vue d’ensemble de l’implémentation

1. Obtenir le **visuel** de document pour l’élément hôte
2. Créer un élément Windows.UI.Composition **DropShadow**
3. Configurer l’élément **DropShadow** pour obtenir sa forme à partir de l’élément cible via un masque
    - L’élément **DropShadow** est rectangulaire par défaut, cette étape n’est donc pas nécessaire si la cible est rectangulaire
4. Attacher l’ombre à un nouvel élément **SpriteVisual** et définir l’élément **SpriteVisual** comme l’enfant de l’élément hôte
5. Lier la taille de l’élément **SpriteVisual** à la taille de l’hôte en utilisant une **ExpressionAnimation**.

```xaml
<Grid Width="200" Height="200">
    <Canvas x:Name="ShadowHost" />
    <Ellipse x:Name="CircleImage">
        <Ellipse.Fill>
            <ImageBrush ImageSource="Assets/Images/2.jpg" Stretch="UniformToFill" />
        </Ellipse.Fill>
    </Ellipse>
</Grid>
```

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

Les deux listes suivantes montrent les équivalents [C++/WinRT](https://aka.ms/cppwinrt) et [C++/CX](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx) du code C&#35; précédent à l’aide de la même structure XAML.

```cppwinrt
#include <winrt/Windows.UI.Composition.h>
#include <winrt/Windows.UI.Xaml.h>
#include <winrt/Windows.UI.Xaml.Hosting.h>
#include <winrt/Windows.UI.Xaml.Shapes.h>
...
MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost(), CircleImage());
}

int32_t MyProperty();
void MyProperty(int32_t value);

void InitializeDropShadow(Windows::UI::Xaml::UIElement const& shadowHost, Windows::UI::Xaml::Shapes::Shape const& shadowTarget)
{
    auto hostVisual{ Windows::UI::Xaml::Hosting::ElementCompositionPreview::GetElementVisual(shadowHost) };
    auto compositor{ hostVisual.Compositor() };

    // Create a drop shadow
    auto dropShadow{ compositor.CreateDropShadow() };
    dropShadow.Color(Windows::UI::ColorHelper::FromArgb(255, 75, 75, 80));
    dropShadow.BlurRadius(15.0f);
    dropShadow.Offset(Windows::Foundation::Numerics::float3{ 2.5f, 2.5f, 0.0f });
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask(shadowTarget.GetAlphaMask());

    // Create a Visual to hold the shadow
    auto shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow(dropShadow);

    // Add the shadow as a child of the host in the visual tree
    Windows::UI::Xaml::Hosting::ElementCompositionPreview::SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    auto bindSizeAnimation{ compositor.CreateExpressionAnimation(L"hostVisual.Size") };
    bindSizeAnimation.SetReferenceParameter(L"hostVisual", hostVisual);

    shadowVisual.StartAnimation(L"Size", bindSizeAnimation);
}
```

```cpp
#include "WindowsNumerics.h"

MainPage::MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

void MainPage::InitializeDropShadow(Windows::UI::Xaml::UIElement^ shadowHost, Windows::UI::Xaml::Shapes::Shape^ shadowTarget)
{
    auto hostVisual = Windows::UI::Xaml::Hosting::ElementCompositionPreview::GetElementVisual(shadowHost);
    auto compositor = hostVisual->Compositor;

    // Create a drop shadow
    auto dropShadow = compositor->CreateDropShadow();
    dropShadow->Color = Windows::UI::ColorHelper::FromArgb(255, 75, 75, 80);
    dropShadow->BlurRadius = 15.0f;
    dropShadow->Offset = Windows::Foundation::Numerics::float3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow->Mask = shadowTarget->GetAlphaMask();

    // Create a Visual to hold the shadow
    auto shadowVisual = compositor->CreateSpriteVisual();
    shadowVisual->Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
    Windows::UI::Xaml::Hosting::ElementCompositionPreview::SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    auto bindSizeAnimation = compositor->CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation->SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual->StartAnimation("Size", bindSizeAnimation);
}
```

### <a name="frosted-glass"></a>Verre givré

Créez un effet qui estompe et teint le contenu en arrière-plan. Notez que les développeurs doivent installer le package NuGet Win2D pour utiliser des effets. Consultez la [page d’accueil Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) pour obtenir les instructions d’installation.

#### <a name="implementation-overview"></a>Vue d’ensemble de l’implémentation

1.  Obtenir le **visuel** de document pour l’élément hôte
2.  Créer une arborescence d’effet de flou à l’aide de Win2D et **CompositionEffectSourceParameter**
3.  Créer un élément **CompositionEffectBrush** en fonction de l’arborescence d’effet
4.  Définir l’entrée de l’élément **CompositionEffectBrush** sur **CompositionBackdropBrush**, ce qui permet d’appliquer un effet au contenu se trouvant derrière un élément **SpriteVisual**
5.  Définir l’élément **CompositionEffectBrush** comme le contenu d’un nouvel élément **SpriteVisual** et définir l’élément **SpriteVisual** comme l’enfant de l’élément hôte. Vous pouvez également utiliser une classe XamlCompositionBrushBase.
6.  Lier la taille de l’élément **SpriteVisual** à la taille de l’hôte en utilisant une **ExpressionAnimation**.

```xaml
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

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeFrostedGlass(GlassHost);
}

private void InitializeFrostedGlass(UIElement glassHost)
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

## <a name="additional-resources"></a>Ressources supplémentaires

- [Vue d’ensemble de couche visuelle](https://msdn.microsoft.com/windows/uwp/composition/visual-layer)
- [Classe **ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/mt608976)
- Exemples d’interface utilisateur et de composition avancés dans le [GitHub WindowsUIDevLabs](https://github.com/microsoft/windowsuidevlabs).
- [Exemple de BasicXamlInterop](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/BasicXamlInterop)
- [Exemple de ParallaxingListItems](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)
