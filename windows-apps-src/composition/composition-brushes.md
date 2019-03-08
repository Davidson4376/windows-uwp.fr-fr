---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Pinceaux de composition
description: Un pinceau peint la zone d’un Visual avec sa sortie. Des pinceaux différents ont différents types de sortie.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eb0d48cee4fe6698ec371c882c913affa5af7729
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644884"
---
# <a name="composition-brushes"></a>Pinceaux de composition
Tous les éléments visibles sur votre écran à partir d’une application UWP sont visibles parce qu'ils ont été peints par un pinceau. Les pinceaux vous permettent de peindre les objets de l’interface utilisateur avec du contenu allant de couleurs simples et unies pour les images ou les dessins jusqu'à des chaînes d’effets complexes. Cette rubrique présente les concepts de peinture avec CompositionBrush.

Remarquez que lorsque vous travaillez avec une application XAML UWP, vous pouvez choisir de peindre un élément UIElement avec un [pinceau XAML](/windows/uwp/design/style/brushes) ou un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush). En règle générale, il est plus facile et recommandé de choisir un pinceau XAML si votre scénario est pris en charge par ce type de pinceau. Par exemple, animez la couleur d’un bouton, modifiez le remplissage d’un texte ou d'une forme avec une image. En revanche, si vous essayez de faire quelque chose qui n’est pas pris en charge par un pinceau XAML de peinture avec un masque animé ou un étirement de neuf grilles animé ou une chaîne d’effet, vous pouvez utiliser un CompositionBrush pour peindre un UIElement à l’aide de [ XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

Lorsque vous travaillez avec la couche visuelle, un CompositionBrush doit être utilisé pour peindre la zone d’un [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual).

-   [Conditions préalables](./composition-brushes.md#prerequisites)
-   [Peinture avec CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Peindre avec une couleur unie](./composition-brushes.md#paint-with-a-solid-color)
    -   [Peindre avec un dégradé linéaire](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [Peindre avec une image](./composition-brushes.md#paint-with-an-image)
    -   [Peindre avec un dessin personnalisé](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Peindre avec une vidéo](./composition-brushes.md#paint-with-a-video)
    -   [Peindre avec un effet de filtre](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Peindre avec un CompositionBrush avec un masque d’opacité](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Peindre avec un CompositionBrush à l’aide de NineGrid stretch](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Peindre à l’aide de Pixels de l’arrière-plan](./composition-brushes.md#paint-using-background-pixels)
-   [Combinaison de CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [À l’aide d’un vs pinceau de XAML. CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Rubriques connexes](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Conditions préalables
Cette vue d’ensemble suppose que vous êtes familiarisé avec la structure d’une application Composition de base, comme décrit dans [la présentation de la couche visuelle](visual-layer.md).

## <a name="paint-with-a-compositionbrush"></a>Peindre avec un CompositionBrush

Un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) « peint » une zone avec sa sortie. Des pinceaux différents ont différents types de sortie. Certains peignent une zone avec une couleur unie, d’autres avec un dégradé, une image, un dessin personnalisé ou un effet. Il existe également des pinceaux spécialisées qui modifient le comportement des autres pinceaux. Par exemple, un masque d’opacité peut être utilisé pour contrôler la zone à peindre avec un CompositionBrush ou un élément de neuf grilles peut être utilisé pour contrôler l’étirement appliqué à un CompositionBrush quand vous peignez une zone. CompositionBrush peut être de l’un des types suivants :

|Classe                                   |Détails                                         |Introduit dans|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Peint une zone d'une couleur unie                        |Mise à jour du 10 novembre de Windows (Kit de développement logiciel 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Peint une zone avec le contenu d’un [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)|Mise à jour du 10 novembre de Windows (Kit de développement logiciel 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Peint une zone avec le contenu d’un effet de composition |Mise à jour du 10 novembre de Windows (Kit de développement logiciel 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Peint un visuel avec un CompositionBrush et un masque d’opacité |Mise à jour anniversaire Windows 10 (14393 SDK)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Peint une zone avec un CompositionBrush utilisant un étirement NineGrid |Mise à jour anniversaire Windows 10 SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Peint une zone avec un dégradé linéaire                    |Windows 10 Fall Creators Update (SDK Insider Preview)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Peint une zone en échantillonnant les pixels en arrière-plan provenant de l’application ou les pixels situés directement derrière la fenêtre de l’application sur le bureau. Utilisé comme entrée d'un autre CompositionBrush, comme un CompositionEffectBrush | Mise à jour anniversaire Windows 10 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>Peindre avec une couleur unie

Un [CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) peint une zone avec une couleur unie. Il existe plusieurs façons de spécifier la couleur d'un SolidColorBrush. Par exemple, vous pouvez spécifier ses canaux alpha, rouge, bleu et vert (ARGB) ou utiliser l'une des couleurs prédéfinies fournies par la classe [Couleurs](https://docs.microsoft.com/uwp/api/windows.ui.colors).

L’illustration et le code suivants montrent une petite arborescence d’éléments visuels, et créent un rectangle tracé avec un pinceau de couleur noire et peint à l’aide d’un pinceau de couleur unie, dont la valeur de couleur est 0x9ACD32.

![CompositionColorBrush](images/composition-compositioncolorbrush.png)

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual _colorVisual1, _colorVisual2;
CompositionColorBrush _blackBrush, _greenBrush;

_compositor = Window.Current.Compositor;
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
_colorVisual1= _compositor.CreateSpriteVisual();
_colorVisual1.Brush = _blackBrush;
_colorVisual1.Size = new Vector2(156, 156);
_colorVisual1.Offset = new Vector3(0, 0, 0);
_container.Children.InsertAtBottom(_colorVisual1);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
_colorVisual2 = _compositor.CreateSpriteVisual();
_colorVisual2.Brush = _greenBrush;
_colorVisual2.Size = new Vector2(150, 150);
_colorVisual2.Offset = new Vector3(3, 3, 0);
_container.Children.InsertAtBottom(_colorVisual2);
```

### <a name="paint-with-a-linear-gradient"></a>Peindre avec un dégradé linéaire

Un [CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) peint une zone avec un dégradé linéaire. Un dégradé linéaire mélange deux ou plusieurs couleurs sur une ligne, l’axe de dégradé. Les objets GradientStop vous permettent de spécifier les couleurs du dégradé et leurs positions.

L’illustration et le code suivants montrent un SpriteVisual peint avec un LinearGradientBrush avec 2 points à l’aide d’une couleur rouge et jaune.

![CompositionLinearGradientBrush](images/composition-compositionlineargradientbrush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionLinearGradientBrush _redyellowBrush;

_compositor = Window.Current.Compositor;

_redyellowBrush = _compositor.CreateLinearGradientBrush();
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Red));
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.Yellow));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = _redyellowBrush;
_gradientVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-an-image"></a>Peindre avec une image

Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) peint une zone de pixels affichée sur une ICompositionSurface. Par exemple, un CompositionSurfaceBrush peut être utilisé pour peindre une zone avec une image affichée sur une surface ICompositionSurface à l’aide de l'API [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface).

L’illustration et le code suivants présentent un SpriteVisual peint avec une image bitmap de réglisse affichée sur une ICompositionSurface à l'aide de LoadedImageSurface. Les propriétés de CompositionSurfaceBrush peuvent servir à étirer et aligner l’image bitmap dans les limites de l’élément visuel.

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png)

```cs
Compositor _compositor;
SpriteVisual _imageVisual;
CompositionSurfaceBrush _imageBrush;

_compositor = Window.Current.Compositor;

_imageBrush = _compositor.CreateSurfaceBrush();

// The loadedSurface has a size of 0x0 till the image has been been downloaded, decoded and loaded to the surface. We can assign the surface to the CompositionSurfaceBrush and it will show up once the image is loaded to the surface.
LoadedImageSurface _loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_imageBrush.Surface = _loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _imageBrush;
_imageVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-custom-drawing"></a>Peindre avec un dessin personnalisé
Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) peut également servir à peindre une zone avec des pixels issus d’un ICompositionSurface rendu à l’aide de [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm) (ou D2D).

Le code suivant montre un SpriteVisual peint avec une exécution de texte rendue sur un ICompositionSurface à l’aide de Win2D. Remarquez qu'afin de pouvoir utiliser Win2D, vous devez inclure le package [NuGet Win2D](https://www.nuget.org/packages/Win2D.uwp) dans votre projet.

```cs
Compositor _compositor;
CanvasDevice _device;
CompositionGraphicsDevice _compositionGraphicsDevice;
SpriteVisual _drawingVisual;
CompositionSurfaceBrush _drawingBrush;

_device = CanvasDevice.GetSharedDevice();
_compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(_compositor, _device);

_drawingBrush = _compositor.CreateSurfaceBrush();
CompositionDrawingSurface _drawingSurface = _compositionGraphicsDevice.CreateDrawingSurface(new Size(256, 256), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied);

using (var ds = CanvasComposition.CreateDrawingSession(_drawingSurface))
{
     ds.Clear(Colors.Transparent);
     var rect = new Rect(new Point(2, 2), (_drawingSurface.Size.ToVector2() - new Vector2(4, 4)).ToSize());
     ds.FillRoundedRectangle(rect, 15, 15, Colors.LightBlue);
     ds.DrawRoundedRectangle(rect, 15, 15, Colors.Gray, 2);
     ds.DrawText("This is a composition drawing surface", rect, Colors.Black, new CanvasTextFormat()
     {
          FontFamily = "Comic Sans MS",
          FontSize = 32,
          WordWrapping = CanvasWordWrapping.WholeWord,
          VerticalAlignment = CanvasVerticalAlignment.Center,
          HorizontalAlignment = CanvasHorizontalAlignment.Center
     }
);

_drawingBrush.Surface = _drawingSurface;

_drawingVisual = _compositor.CreateSpriteVisual();
_drawingVisual.Brush = _drawingBrush;
_drawingVisual.Size = new Vector2(156, 156);
```

De même, le CompositionSurfaceBrush peut également servir à peindre un SpriteVisual avec une SwapChain à l’aide de l’interopérabilité Win2D. [Cet exemple](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) fournit un exemple d’utilisation de Win2D pour peindre un SpriteVisual avec un SwapChain.

### <a name="paint-with-a-video"></a>Peindre avec une vidéo
Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) peut également servir à peindre une zone avec des pixels issus d’un ICompositionSurface rendu à l’aide d'une vidéo chargée via la classe [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer).

Le code suivant montre un SpriteVisual peint avec une vidéo chargée sur un ICompositionSurface.

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("https://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
var item = new MediaPlaybackItem(source);
_mediaPlayer.Source = item;
_mediaPlayer.IsLoopingEnabled = true;

// Get the surface from MediaPlayer and put it on a brush
_videoSurface = _mediaPlayer.GetSurface(_compositor);
_videoBrush = _compositor.CreateSurfaceBrush(_videoSurface.CompositionSurface);

_videoVisual = _compositor.CreateSpriteVisual();
_videoVisual.Brush = _videoBrush;
_videoVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-filter-effect"></a>Peindre avec un effet de filtre

Un [CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) peint une zone avec le résultat d’un CompositionEffect. O,n peut voir les effets de la couche visuelle comme des effets de filtre animables appliqués à une collection de contenu source tel que des couleurs, des dégradés, des images, des vidéos, des chaînes de permutation, des régions de votre interface utilisateur ou des arborescences d’éléments visuels. Le contenu source est généralement spécifié à l’aide d’un autre CompositionBrush.

L’illustration et le code suivants montrent un SpriteVisual peint avec une image de chat à laquelle un effet de filtre de désaturation est appliqué.

![CompositionEffectBrush](images/composition-cat-desaturated.png)

```cs
Compositor _compositor;
SpriteVisual _effectVisual;
CompositionEffectBrush _effectBrush;

_compositor = Window.Current.Compositor;

var graphicsEffect = new SaturationEffect {
                              Saturation = 0.0f,
                              Source = new CompositionEffectSourceParameter("mySource")
                         };

var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
_effectBrush = effectFactory.CreateBrush();

CompositionSurfaceBrush surfaceBrush =_compositor.CreateSurfaceBrush();
LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/cat.jpg"));
SurfaceBrush.surface = loadedSurface;

_effectBrush.SetSourceParameter("mySource", surfaceBrush);

_effectVisual = _compositor.CreateSpriteVisual();
_effectVisual.Brush = _effectBrush;
_effectVisual.Size = new Vector2(156, 156);
```

Pour plus d’informations sur la création d’un effet à l’aide de CompositionBrushes, voir [effets dans la couche visuelle](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Peindre avec un CompositionBrush et un masque d’opacité appliqué

Un [CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) peint une zone avec un CompositionBrush auquel un masque d’opacité est appliqué. La source du masque d’opacité peut être n’importe quel CompositionBrush de type CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush ou CompositionNineGridBrush. Le masque d’opacité doit être spécifié comme un CompositionSurfaceBrush.

L’illustration et le code suivants montrent un SpriteVisual peints avec un CompositionMaskBrush. La source du masque est un CompositionLinearGradientBrush masquée pour ressembler à un cercle à l’aide d’une image de cercle utilisée comme masque.

![CompositionMaskBrush](images/composition-compositionmaskbrush.png)

```cs
Compositor _compositor;
SpriteVisual _maskVisual;
CompositionMaskBrush _maskBrush;

_compositor = Window.Current.Compositor;

_maskBrush = _compositor.CreateMaskBrush();

CompositionLinearGradientBrush _sourceGradient = _compositor.CreateLinearGradientBrush();
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(0,Colors.Red));
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(1,Colors.Yellow));
_maskBrush.Source = _sourceGradient;

LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/circle.png"), new Size(156.0, 156.0));
_maskBrush.Mask = _compositor.CreateSurfaceBrush(loadedSurface);

_maskVisual = _compositor.CreateSpriteVisual();
_maskVisual.Brush = _maskBrush;
_maskVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Peindre avec un CompositionBrush et un étirement NineGrid

Un [CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) peint une zone avec un CompositionBrush étirée à l’aide de la métaphore à neuf grilles. La métaphore à neuf grilles vous permet d'étirer les bords et les angles d'un CompositionBrush différemment de son centre. La source de l'étirement à neuf grilles peut être n’importe quel CompositionBrush de type CompositionColorBrush, CompositionSurfaceBrush ou CompositionEffectBrush.

L’illustration et le code suivants montrent un SpriteVisual peints avec un CompositionNineGridBrush. La source du masque est un CompositionSurfaceBrush étiré à l’aide d’un effet Nine-Grid.

```cs
Compositor _compositor;
SpriteVisual _nineGridVisual;
CompositionNineGridBrush _nineGridBrush;

_compositor = Window.Current.Compositor;

_ninegridBrush = _compositor.CreateNineGridBrush();

// nineGridImage.png is 50x50 pixels; nine-grid insets, as measured relative to the actual size of the image, are: left = 1, top = 5, right = 10, bottom = 20 (in pixels)

LoadedImageSurface _imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/nineGridImage.png"));
_nineGridBrush.Source = _compositor.CreateSurfaceBrush(_imageSurface);

// set Nine-Grid Insets

_ninegridBrush.SetInsets(1, 5, 10, 20);

// set appropriate Stretch on SurfaceBrush for Center of Nine-Grid

sourceBrush.Stretch = CompositionStretch.Fill;

_nineGridVisual = _compositor.CreateSpriteVisual();
_nineGridVisual.Brush = _ninegridBrush;
_nineGridVisual.Size = new Vector2(100, 75);
```

### <a name="paint-using-background-pixels"></a>Peindre à l’aide de pixels en arrière-plan

Un [CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) peint une zone avec le contenu placé derrière elle. Un CompositionBackdropBrush n’est jamais utilisé seul, mais plutôt comme entrée d'un autre CompositionBrush, comme un EffectBrush. Par exemple, en utilisant un CompositionBackdropBrush comme entrée d'un effet de flou, vous pouvez obtenir un effet de verre givré.

Le code suivant montre une petite arborescence visuelle pour créer une image à l’aide de CompositionSurfaceBrush et une superposition de verre givré au-dessus de l’image. La superposition de verre givré est créée en plaçant un SpriteVisual rempli avec un EffectBrush au-dessus de l’image. L'EffectBrush utilise un CompositionBackdropBrush comme entrée de l’effet de flou.

```cs
Compositor _compositor;
SpriteVisual _containerVisual;
SpriteVisual _imageVisual;
SpriteVisual _backdropVisual;

_compositor = Window.Current.Compositor;

// Create container visual to host the visual tree
_containerVisual = _compositor.CreateContainerVisual();

// Create _imageVisual and add it to the bottom of the container visual.
// Paint the visual with an image.

CompositionSurfaceBrush _licoriceBrush = _compositor.CreateSurfaceBrush();

LoadedImageSurface loadedSurface = 
    LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_licoriceBrush.Surface = loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _licoriceBrush;
_imageVisual.Size = new Vector2(156, 156);
_imageVisual.Offset = new Vector3(0, 0, 0);
_containerVisual.Children.InsertAtBottom(_imageVisual)

// Create a SpriteVisual and add it to the top of the containerVisual.
// Paint the visual with an EffectBrush that applies blur to the content
// underneath the Visual to create a frosted glass effect.

GaussianBlurEffect blurEffect = new GaussianBlurEffect(){
                                    Name = "Blur",
                                    BlurAmount = 1.0f,
                                    BorderMode = EffectBorderMode.Hard,
                                    Source = new CompositionEffectSourceParameter("source");
                                    };

CompositionEffectFactory blurEffectFactory = _compositor.CreateEffectFactory(blurEffect);
CompositionEffectBrush _backdropBrush = blurEffectFactory.CreateBrush();

// Create a BackdropBrush and bind it to the EffectSourceParameter source

_backdropBrush.SetSourceParameter("source", _compositor.CreateBackdropBrush());
_backdropVisual = _compositor.CreateSpriteVisual();
_backdropVisual.Brush = _licoriceBrush;
_backdropVisual.Size = new Vector2(78, 78);
_backdropVisual.Offset = new Vector3(39, 39, 0);
_containerVisual.Children.InsertAtTop(_backdropVisual);
```

## <a name="combining-compositionbrushes"></a>Combinaison de CompositionBrushes
Un certain nombre de CompositionBrushes utilisent d'autres CompositionBrushes comme entrées. Par exemple, la méthode SetSourceParameter peut être utilisée pour définir un autre CompositionBrush comme entrée d'un CompositionEffectBrush. Le tableau ci-dessous décrit les combinaisons de CompositionBrushes pris en charge. Notez que l’utilisation d’une combinaison non prise en charge lève une exception.

<table>
<tbody>
<tr>
<th>Brush</th>
<th>EffectBrush.SetSourceParameter()</th>
<th>MaskBrush.Mask</th>
<th>MaskBrush.Source</th>
<th>NineGridBrush.Source</th>
</tr>
<tr>
<td>CompositionColorBrush</td>
<td>OUI</td>
<td>OUI</td>
<td>OUI</td>
<td>OUI</td>
</tr>
<tr>
<td>CompositionLinear<br />GradientBrush</td>
<td>OUI</td>
<td>OUI</td>
<td>OUI</td>
<td>NON</td>
</tr>
<tr>
<td>CompositionSurfaceBrush</td>
<td>OUI</td>
<td>OUI</td>
<td>OUI</td>
<td>OUI</td>
</tr>
<tr>
<td>CompositionEffectBrush</td>
<td>NON</td>
<td>NON</td>
<td>OUI</td>
<td>NON</td>
</tr>
<tr>
<td>CompositionMaskBrush</td>
<td>NON</td>
<td>NON</td>
<td>NON</td>
<td>NON</td>
</tr>
<tr>
<td>CompositionNineGridBrush</td>
<td>OUI</td>
<td>OUI</td>
<td>OUI</td>
<td>NON</td>
</tr>
<tr>
<td>CompositionBackdropBrush</td>
<td>OUI</td>
<td>NON</td>
<td>NON</td>
<td>NON</td>
</tr>
</tbody>
</table>


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>À l’aide d’un vs pinceau de XAML. CompositionBrush

Le tableau suivant fournit une liste des scénarios et indique si l'utilisation d'un pinceau XAML ou Composition est prescrite pour peindre un élément UIElement ou un SpriteVisual dans votre application. 

> [!NOTE]
> Si un CompositionBrush est suggéré pour un élément UIElement XAML, on suppose que le CompositionBrush est mis en package à l’aide d’un XamlCompositionBrushBase.

|Scénario                                                                   | UIElement XAML                                                                                                |Composition SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Peindre une zone d'une couleur unie                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Peindre une zone d'une couleur animée                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Peindre une zone avec un dégradé statique                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Peindre une zone avec des points de dégradé animés                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Peindre une zone avec une image                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|Peindre une zone avec une page Web                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |Non applicable
|Peindre une zone avec une image à l'aide d'un étirement NineGrid                         |[Contrôle d’image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Peindre une zone à l'aide d'un étirement NineGrid animé                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Peindre une zone avec une chaîne d’échange                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) avec interopérabilité de chaîne d’échange
|Peindre une zone avec une vidéo                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) avec interopérabilité média
|Peindre une zone avec un dessin 2D personnalisé                                       |[CanvasControl](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) de Win2D                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) avec interopérabilité Win2D
|Peindre une zone avec un masque non animé                                       |Utiliser des [formes](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes) XAML pour définir un masque   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Peindre une zone avec un masque animé                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Peindre une zone avec un effet de filtre animé                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Peindre une zone avec un effet appliqué aux pixels en arrière-plan        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Rubriques connexes

[COMPOSITION DirectX et Direct2D une interopérabilité native avec BeginDraw et EndDraw](composition-native-interop.md)

[Interopérabilité de pinceau XAML avec XamlCompositionBrushBase](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
