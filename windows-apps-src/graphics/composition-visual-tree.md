---
author: scottmill
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: "Arborescence d’éléments visuels de composition"
description: "Les éléments visuels de composition constituent la structure de l’arborescence des éléments visuels sur laquelle reposent toutes les autres fonctionnalités de l’API Composition. L’API permet aux développeurs de définir et de créer un ou plusieurs objets visuels, qui représentent chacun un nœud unique dans une arborescence d’éléments visuels."
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 85def1cb24ee6915d0835b2fb071e92105367d26
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="composition-visual-tree"></a>Arborescence d’éléments visuels de composition

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Les éléments visuels de composition constituent la structure de l’arborescence des éléments visuels sur laquelle reposent toutes les autres fonctionnalités de l’API Composition. L’API permet aux développeurs de définir et de créer un ou plusieurs objets visuels, qui représentent chacun un nœud unique dans une arborescence d’éléments visuels.

## <a name="visuals"></a>Éléments visuels

Il existe trois types d’élément visuel qui composent la structure de l’arborescence d’éléments visuels plus une classe de pinceau de base contenant plusieurs sous-classes qui affectent le contenu d’un élément visuel :

-   [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) : objet de base. La plupart des propriétés sont répertoriées ici et héritées par les autres objets visuels.
-   [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) : dérive de [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), et ajoute la possibilité de créer des enfants.
-   [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) : dérive de [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810), et offre la possibilité d’associer un pinceau afin que l’élément visuel puisse rendre les pixels notamment des images, des effets ou une couleur unie.
-   [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398) : permet l’application d’un effet dans le contenu d’un élément visuel. Il existe plusieurs sous-classes de CompositionBrush.

## <a name="the-compositionvisual-sample"></a>Exemple CompositionVisual

Dans cet exemple, vous pouvez voir plusieurs carrés de couleur unie sur lesquels vous pouvez cliquer et que vous pouvez glisser dans l’écran. Lorsque vous cliquez sur un carré, il s’affiche à l’avant, effectue une rotation de 45 degrés et devient opaque lorsque vous le faites glisser.

Cela illustre un certain nombre de concepts de base sur l’utilisation de l’API, notamment :

-   Création d’un compositeur
-   Création d’un SpriteVisual avec un ColorBrush
-   Découpage d’un élément visuel
-   Rotation d’un élément visuel
-   Définition de l’opacité
-   Modification de la position de l’élément visuel dans la collection

Cet exemple illustre également trois éléments visuels :

-   [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) : objet de base. La plupart des propriétés sont répertoriées ici et héritées par les autres objets visuels.
-   [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) : dérive d’un élément visuel, et ajoute la possibilité de créer des enfants.
-   [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) : dérive d’un élément visuel, et offre la possibilité d’associer un pinceau afin que l’élément visuel puisse rendre les pixels notamment des images, des effets ou une couleur unie.

Même si cet exemple ne couvre pas des concepts comme celui des animations ou des effets plus complexes, il contient les éléments fondamentaux utilisés par tous ces systèmes.

## <a name="creating-a-compositor"></a>Création d’un compositeur

Il est facile de créer un [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) et de le stocker en tant que fabrique dans une variable. L’extrait de code suivant illustre la création d’un **Compositor** :

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>Création d’un SpriteVisual et d’un ColorBrush

En utilisant le [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789), il est facile de créer des objets quand vous en avez besoin, par exemple un [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) et un [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) :

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

Même s’il ne s’agit que de quelques lignes de code, cela illustre un concept puissant : les objets [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) sont au cœur du système d’effets. Le **SpriteVisual** permet une grande flexibilité et une interconnexion de création de couleur, d’image et d’effet. Le **SpriteVisual** est un type d’élément visuel qui peut remplir un rectangle 2D à l’aide d’un pinceau, dans ce cas de couleur unie.

## <a name="clipping-a-visual"></a>Découpage d’un élément visuel

Le [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) permet également de créer des découpes dans un [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858). Vous trouverez ci-dessous un exemple d’utilisation de [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825) pour découper chaque côté de l’élément visuel :

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

Remarque: à l’instar des autres objets de l’API, des animations peuvent être appliquées aux propriétés de [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825).

## <a name="span-idrotatingaclipspanspan-idrotatingaclipspanspan-idrotatingaclipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>Rotation d’une découpe

Un [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) peut être transformé à l’aide d’une rotation. Notez que [**RotationAngle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.rotationangle) prend en charge les radians et les degrés. Les radians sont définis pas défaut, mais il est facile de spécifier des degrés, comme le montre l’extrait de code suivant:

```cs
child.RotationAngleInDegrees = 45.0f;
```

Rotation n’est qu’un des exemples des composants de transformation fournis par l’API qui facilitent ces tâches. On trouve également Offset, Scale, Orientation, RotationAxis et 4x4 TransformMatrix.

## <a name="setting-opacity"></a>Définition de l’opacité

La définition de l’opacité d’un élément visuel est une opération simple utilisant une valeur flottante. Dans l’exemple suivant, tous les carrés sont définis au départ sur l’opacité 0,8:

```cs
visual.Opacity = 0.8f;
```

Comme la rotation, la propriété [**Opacity**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.opacity) peut être animée.

## <a name="changing-the-visuals-position-in-the-collection"></a>Modification de la position de l’élément visuel dans la collection

L’API Composition permet de modifier la position d’un élément visuel dans une [**VisualCollection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection) de plusieurs façons : vous pouvez le placer au-dessus d’un autre élément visuel avec [**InsertAbove**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertabove), le placer au-dessous avec [**InsertBelow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertbelow), le déplacer vers le haut avec [**InsertAtTop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertattop) ou vers le bas avec [**InsertAtBottom**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertatbottom).

L’exemple illustre un [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) sur lequel un utilisateur a cliqué et qui est classé vers le haut :

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>Exemple complet

Dans l’exemple complet, tous les concepts décrits ci-dessus sont utilisés conjointement pour construire une arborescence simple d’objets [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) afin de changer l’opacité sans avoir recours à XAML, WWA ni DirectX. Cet exemple illustre la création et l’ajout d’objets **Visual** enfants ainsi que la modification des propriétés.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using Windows.ApplicationModel.Core;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Core;

namespace compositionvisual
{
    class VisualProperties : IFrameworkView
    {
        //------------------------------------------------------------------------------
        //
        // VisualProperties.Initialize
        //
        // This method is called during startup to associate the IFrameworkView with the
        // CoreApplicationView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Initialize(CoreApplicationView view)
        {
            _view = view;
            _random = new Random();
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.SetWindow
        //
        // This method is called when the CoreApplication has created a new CoreWindow,
        // allowing the application to configure the window and start producing content
        // to display.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.SetWindow(CoreWindow window)
        {
            _window = window;
            InitNewComposition();
            _window.PointerPressed += OnPointerPressed;
            _window.PointerMoved += OnPointerMoved;
            _window.PointerReleased += OnPointerReleased;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerPressed
        //
        // This method is called when the user touches the screen, taps it with a stylus
        // or clicks the mouse.
        //
        //------------------------------------------------------------------------------

        void OnPointerPressed(CoreWindow window, PointerEventArgs args)
        {
            Point position = args.CurrentPoint.Position;

            //
            // Walk our list of visuals to determine who, if anybody, was selected
            //
            foreach (var child in _root.Children)
            {
                //
                // Did we hit this child?
                //
                Vector3 offset = child.Offset;
                Vector2 size = child.Size;

                if ((position.X >= offset.X) &&
                    (position.X < offset.X + size.X) &&
                    (position.Y >= offset.Y) &&
                    (position.Y < offset.Y + size.Y))
                {
                    //
                    // This child was hit. Since the children are stored back to front,
                    // the last one hit is the front-most one so it wins
                    //
                    _currentVisual = child as ContainerVisual;
                    _offsetBias = new Vector2((float)(offset.X - position.X),
                                              (float)(offset.Y - position.Y));
                }
            }

            //
            // If a visual was hit, bring it to the front of the Z order
            //
            if (_currentVisual != null)
            {
                ContainerVisual parent = _currentVisual.Parent as ContainerVisual;
                parent.Children.Remove(_currentVisual);
                parent.Children.InsertAtTop(_currentVisual);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerMoved
        //
        // This method is called when the user moves their finger, stylus or mouse with
        // a button pressed over the screen.
        //
        //------------------------------------------------------------------------------

        void OnPointerMoved(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual is selected, drag it with the pointer position and
            // make it opaque while we drag it
            //
            if (_currentVisual != null)
            {
                //
                // Set up the properties of the visual the first time it is
                // dragged. This will last for the duration of the drag
                //
                if (!_dragging)
                {
                    _currentVisual.Opacity = 1.0f;

                    //
                    // Transform the first child of the current visual so that
                    // the image is rotated
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngleInDegrees = 45.0f;
                        child.CenterPoint = new Vector3(_currentVisual.Size.X / 2, _currentVisual.Size.Y / 2, 0);
                        break;
                    }

                    //
                    // Clip the visual to its original layout rect by using an inset
                    // clip with a one-pixel margin all around
                    //
                    var clip = _compositor.CreateInsetClip();
                    clip.LeftInset = 1.0f;
                    clip.RightInset = 1.0f;
                    clip.TopInset = 1.0f;
                    clip.BottomInset = 1.0f;
                    _currentVisual.Clip = clip;

                    _dragging = true;
                }

                Point position = args.CurrentPoint.Position;
                _currentVisual.Offset = new Vector3((float)(position.X + _offsetBias.X),
                                                    (float)(position.Y + _offsetBias.Y),
                                                    0.0f);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerReleased
        //
        // This method is called when the user lifts their finger or stylus from the
        // screen, or lifts the mouse button.
        //
        //------------------------------------------------------------------------------

        void OnPointerReleased(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual was selected, make it transparent again when it is
            // released and restore the transform and clip
            //
            if (_currentVisual != null)
            {
                if (_dragging)
                {
                    //
                    // Remove the transform from the first child
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngle = 0.0f;
                        child.CenterPoint = new Vector3(0.0f, 0.0f, 0.0f);
                        break;
                    }

                    _currentVisual.Opacity = 0.8f;
                    _currentVisual.Clip = null;
                    _dragging = false;
                }

                _currentVisual = null;
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Load
        //
        // This method is called when a specific page is being loaded in the
        // application.  It is not used for this application.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Load(string unused)
        {

        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Run
        //
        // This method is called by CoreApplication.Run() to actually run the
        // dispatcher's message pump.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Run()
        {
            _window.Activate();
            _window.Dispatcher.ProcessEvents(CoreProcessEventsOption.ProcessUntilQuit);
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Uninitialize
        //
        // This method is called during shutdown to disconnect the CoreApplicationView,
        // and CoreWindow from the IFrameworkView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Uninitialize()
        {
            _window = null;
            _view = null;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.InitNewComposition
        //
        // This method is called by SetWindow(), where we initialize Composition after
        // the CoreWindow has been created.
        //
        //------------------------------------------------------------------------------

        void InitNewComposition()
        {
            //
            // Set up Windows.UI.Composition Compositor, root ContainerVisual, and associate with
            // the CoreWindow.
            //

            _compositor = new Compositor();

            _root = _compositor.CreateContainerVisual();



            _compositionTarget = _compositor.CreateTargetForCurrentView();
            _compositionTarget.Root = _root;

            //
            // Create a few visuals for our window
            //
            for (int index = 0; index < 20; index++)
            {
                _root.Children.InsertAtTop(CreateChildElement());
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.CreateChildElement
        //
        // Creates a small sub-tree to represent a visible element in our application.
        //
        //------------------------------------------------------------------------------

        Visual CreateChildElement()
        {
            //
            // Each element consists of three visuals, which produce the appearance
            // of a framed rectangle
            //
            var element = _compositor.CreateContainerVisual();
            element.Size = new Vector2(100.0f, 100.0f);

            //
            // Position this visual randomly within our window
            //
            element.Offset = new Vector3((float)(_random.NextDouble() * 400), (float)(_random.NextDouble() * 400), 0.0f);

            //
            // The outer rectangle is always white
            //
            //Note to preview API users - SpriteVisual and Color Brush replace SolidColorVisual
            //for example instead of doing 
            //var visual = _compositor.CreateSolidColorVisual() and 
            //visual.Color = Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF);
            //we now use the below

            var visual = _compositor.CreateSpriteVisual();
            element.Children.InsertAtTop(visual);
            visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
            visual.Size = new Vector2(100.0f, 100.0f);

            //
            // The inner rectangle is inset from the outer by three pixels all around
            //
            var child = _compositor.CreateSpriteVisual();
            visual.Children.InsertAtTop(child);
            child.Offset = new Vector3(3.0f, 3.0f, 0.0f);
            child.Size = new Vector2(94.0f, 94.0f);

            //
            // Pick a random color for every rectangle
            //
            byte red = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte green = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte blue = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            child.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, red, green, blue));

            //
            // Make the subtree root visual partially transparent. This will cause each visual in the subtree
            // to render partially transparent, since a visual's opacity is multiplied with its parent's
            // opacity
            //
            element.Opacity = 0.8f;

            return element;
        }

        // CoreWindow / CoreApplicationView
        private CoreWindow _window;
        private CoreApplicationView _view;

        // Windows.UI.Composition
        private Compositor _compositor;
        private CompositionTarget _compositionTarget;
        private ContainerVisual _root;
        private ContainerVisual _currentVisual;
        private Vector2 _offsetBias;
        private bool _dragging;

        // Helpers
        private Random _random;
    }


    public sealed class VisualPropertiesFactory : IFrameworkViewSource
    {
        //------------------------------------------------------------------------------
        //
        // VisualPropertiesFactory.CreateView
        //
        // This method is called by CoreApplication to provide a new IFrameworkView for
        // a CoreWindow that is being created.
        //
        //------------------------------------------------------------------------------

        IFrameworkView IFrameworkViewSource.CreateView()
        {
            return new VisualProperties();
        }


        //------------------------------------------------------------------------------
        //
        // main
        //
        //------------------------------------------------------------------------------

        static int Main(string[] args)
        {
            CoreApplication.Run(new VisualPropertiesFactory());

            return 0;
        }
    }
}
```

 

 




