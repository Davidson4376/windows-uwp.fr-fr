---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Pinceaux de composition
description: Un pinceau peint la zone d’un Visual avec sa sortie. Des pinceaux différents ont différents types de sortie.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e8c995c5a9513bea44664bcb395cd604ba2668c3
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/01/2018
ms.locfileid: "8351418"
---
# <a name="composition-brushes"></a>Pinceaux de composition
Tous les éléments visibles à l’écran à partir d’une application UWP sont visible dans la mesure où il a été peint par un pinceau. Pinceaux activer vous permettent de peindre des objets d’interface (interface utilisateur) utilisateur avec le contenu allant des couleurs unies, simples à des images ou des dessins à la chaîne d’effets complexes. Cette rubrique présente les concepts de peinture avec CompositionBrush.

Notez que lorsque vous travaillez avec une application UWP XAML, vous pouvez choisir de peindre un élément UIElement avec un [Pinceau XAML](/windows/uwp/design/style/brushes) ou un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush). En règle générale, il est recommandé de choisir un pinceau XAML si votre scénario est pris en charge par un pinceau XAML et plus facile. Par exemple, animez la couleur d’un bouton, en modifiant le remplissage d’un texte ou une forme avec une image. En revanche, si vous essayez de faire quelque chose qui n’est pas pris en charge par un pinceau XAML comme peinture avec un masque animé ou un étirement incrustations animé ou une chaîne d’effets, vous pouvez utiliser un CompositionBrush pour peindre un élément UIElement par le biais de [ XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

Lorsque vous travaillez avec la couche visuelle, un CompositionBrush doit être utilisé pour peindre la zone d’un [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual).

-   [Conditions préalables](./composition-brushes.md#prerequisites)
-   [Paint avec CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Peindre avec une couleur unie](./composition-brushes.md#paint-with-a-solid-color)
    -   [Peindre avec un dégradé linéaire](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [Peindre avec une image](./composition-brushes.md#paint-with-an-image)
    -   [Peindre avec un dessin personnalisé](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Peindre avec une vidéo](./composition-brushes.md#paint-with-a-video)
    -   [Peindre avec un effet de filtre](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Peindre avec un CompositionBrush avec un masque d’opacité](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Peindre avec un CompositionBrush à l’aide de NineGrid stretch](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Peindre à l’aide de Pixels en arrière-plan](./composition-brushes.md#paint-using-background-pixels)
-   [Combinaison de CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [À l’aide d’un pinceau XAML VS CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Rubriques connexes](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Conditions préalables
Cette vue d’ensemble suppose que vous êtes familiarisé avec la structure d’une application de Composition de base, comme décrit dans [vue d’ensemble de la couche visuelle](visual-layer.md).

## <a name="paint-with-a-compositionbrush"></a>Paint avec un CompositionBrush

Un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) «peint» une zone avec sa sortie. Des pinceaux différents ont différents types de sortie. Certains peignent une zone avec une couleur unie, d’autres avec un dégradé, image, dessin personnalisé ou effet. Il existe également des pinceaux spécialisé qui modifient le comportement d’autres pinceaux. Par exemple, masque d’opacité peut être utilisé pour le contrôle quelle zone est peint par un CompositionBrush ou une incrustations peuvent être utilisée pour contrôler l’étirement appliquée à un CompositionBrush lorsque vous peignez une zone. CompositionBrush peut être l’un des types suivants:

|Classe                                   |Détails                                         |Introduite dans|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Peint une zone avec une couleur unie                        |Mise à jour de novembre Windows 10 (kit SDK 10586)|
|[Compositionsurfacebrush.](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Peint une zone avec le contenu d’un [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)|Mise à jour de novembre Windows 10 (kit SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Peint une zone avec le contenu d’un effet de composition |Mise à jour de novembre Windows 10 (kit SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Peint un élément visuel avec une CompositionBrush avec un masque d’opacité |Mise à jour anniversaire de Windows 10 (14393 du SDK)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Peint une zone avec un CompositionBrush à l’aide d’un étirement NineGrid |Mise à jour anniversaire de Windows 10 SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Peint une zone avec un dégradé linéaire                    |Windows 10 Fall Creators Update (SDK Insider Preview)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Peint une zone en échantillonnant les pixels en arrière-plan à partir de l’application ou pixels directement derrière la fenêtre de l’application sur le bureau. Utilisée comme entrée pour une autre CompositionBrush comme un CompositionEffectBrush | Mise à jour anniversaire de Windows 10 (14393 du SDK)

### <a name="paint-with-a-solid-color"></a>Peindre avec une couleur unie

Un [CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) peint une zone avec une couleur unie. Il existe plusieurs façons de spécifier la couleur de l’objet SolidColorBrush. Par exemple, vous pouvez spécifier ses canaux de (ARVB) alpha, rouge, bleus et verts ou utiliser l’une des couleurs prédéfinies fournies par la classe de [couleurs](https://docs.microsoft.com/uwp/api/windows.ui.colors) .

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

Un [CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) peint une zone avec un dégradé linéaire. Un dégradé linéaire mélange deux ou plusieurs couleurs sur une ligne, l’axe de dégradé. Objets GradientStop vous permettent de spécifier les couleurs du dégradé et leurs positions.

L’illustration et le code suivant montre un élément SpriteVisual peint avec un LinearGradientBrush avec des taquets de 2 à l’aide d’une couleur rouge et jaune.

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

Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) peint une zone avec des pixels affichées sur un ICompositionSurface. Par exemple, un CompositionSurfaceBrush peut être utilisé pour peindre une zone avec une image affichée sur une surface de ICompositionSurface à l’aide de [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API.

L’illustration et le code suivant montre un élément SpriteVisual peint avec une image bitmap d’un réglisse affichée sur un ICompositionSurface à l’aide de LoadedImageSurface. Les propriétés de CompositionSurfaceBrush peuvent être utilisées pour étirer et d’aligner l’image bitmap dans les limites de l’élément visuel.

![Compositionsurfacebrush.](images/composition-compositionsurfacebrush.png)

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
Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) peut également être utilisé pour peindre une zone avec des pixels à partir d’un ICompositionSurface rendus à l’aide de [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) (ou D2D).

Le code suivant montre qu'un élément SpriteVisual peint avec une exécution rendue sur une ICompositionSurface de texte à l’aide de Win2D. Notez que pour pouvoir utiliser Win2D, vous devez inclure le package [NuGet Win2D](http://www.nuget.org/packages/Win2D.uwp) dans votre projet.

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

De même, le CompositionSurfaceBrush peut également être utilisé pour peindre un SpriteVisual avec une chaîne d’échange à l’aide de l’interopérabilité Win2D. [Cet exemple](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) fournit un exemple d’utilisation de Win2D pour peindre un SpriteVisual avec une chaîne d’échange.

### <a name="paint-with-a-video"></a>Peindre avec une vidéo
Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) peut également être utilisé pour peindre une zone avec des pixels à partir d’un ICompositionSurface rendus à l’aide d’une vidéo chargée par le biais de la classe de [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) .

Le code suivant montre qu'un élément SpriteVisual peint avec une vidéo chargée sur un ICompositionSurface.

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("http://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
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

Un [CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) peint une zone avec la sortie d’une CompositionEffect. Effets de la couche visuelle peuvent être considérés comme les effets de filtre animables appliquées à une collection de contenu source telles que les couleurs, les dégradés, images, vidéos, des chaînes de permutation, des régions de votre interface utilisateur ou des arborescences d’éléments visuels. Le contenu de la source est généralement spécifié à l’aide d’un autre CompositionBrush.

L’illustration et le code suivant montre un élément SpriteVisual peint avec une image d’une image de Chat qui Désaturation filtre effet est appliqué.

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

Pour plus d’informations sur la création d’un effet à l’aide de CompositionBrushes voir les [effets dans la couche visuelle](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Peindre avec un CompositionBrush avec masque d’opacité appliqué

Un [CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) peint une zone avec un CompositionBrush avec un masque d’opacité appliqué. La source du masque d’opacité peut être n’importe quel CompositionBrush de type CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush ou CompositionNineGridBrush. Le masque d’opacité doit être spécifié comme un CompositionSurfaceBrush.

L’illustration et le code suivant montre un élément SpriteVisual peint avec un CompositionMaskBrush. La source du masque est un CompositionLinearGradientBrush qui est masquée comme un cercle à l’aide d’une image de cercle en tant que masque.

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Peindre avec un CompositionBrush à l’aide de NineGrid stretch

Un [CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) peint une zone avec un CompositionBrush qui est étirée à l’aide de la métaphore incrustations. La métaphore incrustations vous permet d’étirer des bords et les coins d’un CompositionBrush différemment que son centre. La source de l’étirement incrustations peut par n’importe quel CompositionBrush de type CompositionColorBrush, CompositionSurfaceBrush ou CompositionEffectBrush.

Le code suivant montre qu'un élément SpriteVisual peint avec un CompositionNineGridBrush. La source du masque est un CompositionSurfaceBrush qui est étirée à l’aide d’une incrustations.

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

### <a name="paint-using-background-pixels"></a>Peindre à l’aide de Pixels en arrière-plan

Un [CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) peint une zone avec le contenu derrière la zone. Un CompositionBackdropBrush n’est pas utilisée sur son propre, mais au lieu de cela est utilisé en tant qu’entrée pour une autre CompositionBrush comme un EffectBrush. Par exemple, en utilisant un CompositionBackdropBrush en tant qu’entrée pour un effet de flou, vous pouvez obtenir un effet de verre givré.

Le code suivant montre une petite arborescence d’éléments visuel pour créer une image à l’aide de CompositionSurfaceBrush et une superposition de verre givré au-dessus de l’image. La superposition de verre givré est créée en plaçant un élément SpriteVisual rempli avec une EffectBrush au-dessus de l’image. L’EffectBrush utilise un CompositionBackdropBrush en tant qu’entrée pour l’effet de flou.

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
Un certain nombre de CompositionBrushes utiliser autres CompositionBrushes en tant qu’entrées. Par exemple, à l’aide de la méthode SetSourceParameter être permet de définir une autre CompositionBrush en tant qu’entrée pour un CompositionEffectBrush. Le tableau ci-dessous décrit les combinaisons de CompositionBrushes pris en charge. Notez qu’en utilisant une combinaison non pris en charge lève une exception.

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
<td>NO</td>
</tr>
<tr>
<td>Compositionsurfacebrush.</td>
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
<td>NO</td>
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
<td>NO</td>
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


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>À l’aide d’un pinceau XAML VS CompositionBrush

Le tableau suivant fournit une liste des scénarios et si XAML ou Composition, utilisez pinceau est prescrite lorsque vous peignez un élément UIElement ou un élément SpriteVisual dans votre application. 

> [!NOTE]
> Si un CompositionBrush est suggérée pour un élément UIElement XAML, il est supposé que le CompositionBrush est un package à l’aide d’une classe XamlCompositionBrushBase.

|Scénario                                                                   | Élément UIElement XAML                                                                                                |COMPOSITION SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Peindre une zone avec une couleur unie                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Peindre une zone avec une couleur animée                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Peindre une zone avec un dégradé statique                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Peindre une zone avec un dégradé animé                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Peindre une zone avec une image                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[Compositionsurfacebrush.](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|Peindre une zone avec une page Web                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |N/A
|Peindre une zone avec une image à l’aide de NineGrid stretch                         |[Contrôle d’image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Peindre une zone avec animée NineGrid stretch                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Peindre une zone avec une chaîne d’échange                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) avec l’interopérabilité de chaîne d’échange
|Peindre une zone avec une vidéo                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) avec l’interopérabilité multimédia
|Peindre une zone avec un dessin 2D personnalisé                                       |[CanvasControl](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) à partir de Win2D                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) avec l’interopérabilité Win2D
|Peindre une zone avec masque non animées                                       |Utilisez les [formes](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes) XAML pour définir un masque   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Peindre une zone avec un masque animé.                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Peindre une zone avec un effet de filtre animé                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Peindre une zone avec un effet appliqué aux pixels en arrière-plan        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Rubriques connexes

[COMPOSITION native DirectX et Direct2D l’interopérabilité avec BeginDraw et EndDraw](composition-native-interop.md)

[Interopérabilité de pinceau XAML avec XamlCompositionBrushBase](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
