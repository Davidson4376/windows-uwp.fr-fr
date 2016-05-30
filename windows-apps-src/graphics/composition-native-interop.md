---
author: scottmill
ms.assetid: 16ad97eb-23f1-0264-23a9-a1791b4a5b95
title: Interopération DirectX et Direct2D de composition en mode natif avec BeginDraw et EndDraw
description: L’API Windows.UI.Composition fournit des interfaces pour interopération en mode natif, permettant ainsi de déplacer directement le contenu dans le compositeur.
---
# Interopération DirectX et Direct2D de composition en mode natif avec BeginDraw et EndDraw

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

L’API Windows.UI.Composition fournit les interfaces pour interopération en mode natif [**ICompositorInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620068), [**ICompositionDrawingSurfaceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620058) et [**ICompositionGraphicsDeviceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620065), permettant ainsi de déplacer directement le contenu dans le compositeur.

L’interopération en mode natif est structurée autour des objets surface qui sont soutenus par les textures de DirectX. Les surfaces sont créées à partir d’un objet usine appelé [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749). Cet objet est soutenu par un objet périphérique Direct2D ou Direct3D sous-jacent, qu’il utilise pour allouer de la mémoire vidéo pour les surfaces. L’API composition ne crée jamais le périphérique DirectX sous-jacent. Il incombe à l’application d’en créer un et de le transmettre à l’objet **CompositionGraphicsDevice**. Une application peut créer plusieurs objets **CompositionGraphicsDevice** à la fois. Elle peut également utiliser le même périphérique DirectX en tant que périphérique de rendu pour plusieurs objets **CompositionGraphicsDevice**.

## Création d’une surface

Chaque [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) fait office d’usine de surfaces. Chaque surface est créée avec une taille initiale (qui peut être égale à 0,0), mais pas de pixels valides. Dans son état initial, une surface peut être consommée immédiatement dans une arborescence visuelle, par exemple, via un [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) et un [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433), mais dans cet état initial, la surface n’a aucun effet sur la sortie de l’écran. En fait, elle est entièrement transparente même si le mode alpha spécifié est « opaque ».

Il arrive que les périphériques DirectX puissent être inutilisables. Cela peut se produire, entre autres, si l’application transmet des arguments non valides à certaines API DirectX, si la carte graphique est réinitialisée par le système ou si le pilote est mis à jour. Direct3D possède une API qu’une application peut utiliser pour détecter de façon asynchrone si le périphérique est perdu pour une raison quelconque. Si un périphérique DirectX est perdu, l’application doit l’ignorer, en créer un et le transmettre à tous les objets [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) précédemment associés au périphérique DirectX incorrect.

## Chargement des pixels dans une surface

Pour charger des pixels dans la surface, l’application doit appeler la méthode [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx), qui retourne une interface DirectX représentant une texture ou un contexte Direct2D, en fonction des demandes de l’application. L’application doit ensuite effectuer le rendu ou charger des pixels dans cette texture. Quand l’application a terminé, elle doit appeler la méthode [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060). C’est seulement à ce stade que les nouveaux pixels sont disponibles pour une composition, mais ils n’apparaissent pas à l’écran tant que toutes les modifications apportées à l’arborescence d’éléments visuels n’ont pas été validées. Si l’arborescence d’éléments visuels est validée avant l’appel de **EndDraw**, la mise à jour en cours d’exécution n’est pas visible à l’écran, et la surface continue d’afficher le contenu qu’elle avait avant de **BeginDraw**. À l’appel de **EndDraw**, le pointeur contexte Direct2D ou texture retourné par BeginDraw est invalidé. Une application ne doit jamais mettre en cache ce pointeur au-delà de l’appel de **EndDraw**.

L’application peut appeler uniquement BeginDraw sur une seule surface à la fois pour tout [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) donné. Après avoir appelé [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx), l’application doit appeler [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) sur cette surface avant d’appeler **BeginDraw** sur une autre. Comme l’API est agile, l’application est chargée de synchroniser ces appels s’il souhaite effectuer le rendu à partir de plusieurs threads de travail. Si une application souhaite interrompre le rendu d’une surface et basculer vers une autre de manière temporaire, l’application peut utiliser la méthode [**SuspendDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620064.aspx). Cela permet à une autre **BeginDraw** de réussir, mais ne rend pas la première mise à jour de surface disponible pour la composition à l’écran. Ce faisant, l’application peut effectuer plusieurs mises à jour de manière transactionnelle. Lorsqu’une surface est suspendue, l’application peut continuer la mise à jour en appelant la méthode [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062). Elle peut également déclarer que la mise à jour est terminée en appelant **EndDraw**. Cela signifie qu’une seule surface peut être mise à jour activement à la fois pour tout **CompositionGraphicsDevice** donné. Chaque périphérique graphique conserve cet état indépendamment des autres ; une application peut donc être affichée simultanément sur deux surfaces si elles appartiennent à des périphériques graphiques différents. Toutefois, cela empêche le regroupement de la mémoire vidéo de ces deux surfaces et diminue donc l’efficacité de la mémoire.

Les méthodes [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx), [**SuspendDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620064.aspx), [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) et [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) renvoient des échecs si l’application effectue une opération incorrecte (par exemple, si elle passe des arguments non valides ou si elle appelle **BeginDraw** sur une surface avant d’appeler **EndDraw** sur une autre). Ces types d’échec représentent des bogues d’application et sont censés être gérés par un échec rapide. **BeginDraw** peut également renvoyer un échec si le périphérique DirectX sous-jacent est perdu. Cet échec n’est pas fatal, car l’application peut recréer son périphérique DirectX et réessayer. L’application est censée gérer la perte du périphérique en ignorant simplement le rendu. Si **BeginDraw** échoue pour une raison quelconque, l’application ne doit pas non plus appeler **EndDraw**, car le début n’a jamais eu lieu au préalable.

## Défilement

Pour des raisons de performances, lorsqu’une application appelle [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx), il n’est pas garanti que le contenu de la texture retournée soit le contenu précédent de la surface. L’application doit supposer que le contenu est aléatoire et elle doit s’assurer que tous les pixels sont touchés, soit en effaçant la surface avant le rendu, soit en dessinant un contenu suffisamment opaque pour couvrir la totalité du rectangle mis à jour. Combiné au fait que le pointeur texture est uniquement valide entre les appels de **BeginDraw** et [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060), cela empêche l’application de copier le contenu précédent en dehors de la surface. C’est pourquoi, nous fournissons une méthode [**Scroll**](https://msdn.microsoft.com/library/windows/apps/mt620063), qui permet à l’application d’effectuer une copie de pixels de même surface.

## Exemple d’utilisation

L’exemple suivant illustre un scénario très simple, où une application crée des surfaces de dessin et utilise [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx) et [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) pour remplir les surfaces avec du texte. L’application utilise DirectWrite pour disposer le texte (détails non affichés), puis utilise Direct2D pour en effectuer le rendu. Le périphérique graphique de composition accepte directement le périphérique Direct2D au moment de l’initialisation. Cela permet à **BeginDraw** de retourner un pointeur d’interface ID2D1DeviceContext, ce qui est beaucoup plus efficace que de créer un contexte Direct2D via l’application pour encapsuler une interface ID3D11Texture2D retournée à chaque opération de dessin.

```cpp
//------------------------------------------------------------------------------
//
// Copyright (C) Microsoft. All rights reserved.
//
//------------------------------------------------------------------------------

#include "stdafx.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::Graphics::DirectX;
using namespace Windows::UI::Composition;

// This is an app-provided helper to render lines of text
class SampleText
{
private:
    // The text to draw
    ComPtr<IDWriteTextLayout> _text;

    // The composition surface that we use in the visual tree
    ComPtr<ICompositionDrawingSurfaceInterop> _drawingSurfaceInterop;

    // The device that owns the surface
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;

    // For managing our event notifier
    EventRegistrationToken _deviceReplacedEventToken;

public:
    SampleText(IDWriteTextLayout* text, ICompositionGraphicsDevice* compositionGraphicsDevice) :
        _text(text),
        _compositionGraphicsDevice(compositionGraphicsDevice)
    {
        // Create the surface just big enough to hold the formatted text block.
        DWRITE_TEXT_METRICS metrics;
        FailFastOnFailure(text->GetMetrics(&metrics));
        Windows::Foundation::Size surfaceSize = { metrics.width, metrics.height };
        ComPtr<ICompositionDrawingSurface> drawingSurface;
        FailFastOnFailure(_compositionGraphicsDevice->CreateDrawingSurface(
            surfaceSize,
            DirectXPixelFormat::DirectXPixelFormat_B8G8R8A8UIntNormalized,
            DirectXAlphaMode::DirectXAlphaMode_Ignore,
            &drawingSurface));

        // Cache the interop pointer, since that's what we always use.
        FailFastOnFailure(drawingSurface.As(&_drawingSurfaceInterop));

        // Draw the text
        DrawText();

        // If the rendering device is lost, the application will recreate and replace it. We then
        // own redrawing our pixels.
        FailFastOnFailure(_compositionGraphicsDevice->add_RenderingDeviceReplaced(
            Callback<RenderingDeviceReplacedEventHandler>([this](
                ICompositionGraphicsDevice* source, IRenderingDeviceReplacedEventArgs* args)
                -> HRESULT
            {
                // Draw the text again
                DrawText();
                return S_OK;
            }).Get(),
            &_deviceReplacedEventToken));
    }

    ~SampleText()
    {
        FailFastOnFailure(_compositionGraphicsDevice->remove_RenderingDeviceReplaced(
            _deviceReplacedEventToken));
    }

    // Return the underlying surface to the caller
    ComPtr<ICompositionSurface> get_Surface()
    {
        // To the caller, the fact that we have a drawing surface is an implementation detail.
        // Return the base interface instead
        ComPtr<ICompositionSurface> surface;
        FailFastOnFailure(_drawingSurfaceInterop.As(&surface));
        return surface;
    }

private:
    // We may detect device loss on BeginDraw calls. This helper handles this condition or other
    // errors.
    bool CheckForDeviceRemoved(HRESULT hr)
    {
        if (SUCCEEDED(hr))
        {
            // Everything is fine -- go ahead and draw
            return true;
        }
        else if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            // We can't draw at this time, but this failure is recoverable. Just skip drawing for
            // now. We will be asked to draw again once the Direct3D device is recreated
            return false;
        }
        else
        {
            // Any other error is unexpected and, therefore, fatal
            FailFast();
        }
    }

    // Renders the text into our composition surface
    void DrawText()
    {
        // Begin our update of the surface pixels. If this is our first update, we are required
        // to specify the entire surface, which nullptr is shorthand for (but, as it works out,
        // any time we make an update we touch the entire surface, so we always pass nullptr).
        ComPtr<ID2D1DeviceContext> d2dDeviceContext;
        POINT offset;
        if (CheckForDeviceRemoved(_drawingSurfaceInterop->BeginDraw(nullptr,
            __uuidof(ID2D1DeviceContext), &d2dDeviceContext, &offset)))
        {
            // Create a solid color brush for the text. A more sophisticated application might want
            // to cache and reuse a brush across all text elements instead, taking care to recreate
            // it in the event of device removed.
            ComPtr<ID2D1SolidColorBrush> brush;
            FailFastOnFailure(d2dDeviceContext->CreateSolidColorBrush(
                D2D1::ColorF(D2D1::ColorF::Black, 1.0f), &brush));

            // Draw the line of text at the specified offset, which corresponds to the top-left
            // corner of our drawing surface. Notice we don't call BeginDraw on the D2D device
            // context; this has already been done for us by the composition API.
            d2dDeviceContext->DrawTextLayout(D2D1::Point2F(offset.x, offset.y), _text.Get(),
                brush.Get());

            // Our update is done. EndDraw never indicates rendering device removed, so any
            // failure here is unexpected and, therefore, fatal.
            FailFastOnFailure(_drawingSurfaceInterop->EndDraw());
        }
    }
};

class SampleApp
{
    ComPtr<ICompositor> _compositor;
    ComPtr<ID2D1Device> _d2dDevice;
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;
    std::vector<ComPtr<SampleText>> _textSurfaces;

public:
    // Run once when the application starts up
    void Initialize(ICompositor* compositor)
    {
        // Cache the compositor (created outside of this method)
        _compositor = compositor;

        // Create a Direct2D device (helper implementation not shown here)
        FailFastOnFailure(CreateDirect2DDevice(&_d2dDevice));

        // To create a composition graphics device, we need to QI for another interface
        ComPtr<ICompositorInterop> compositorInterop;
        FailFastOnFailure(_compositor.As(&compositorInterop));

        // Create a graphics device backed by our D3D device
        FailFastOnFailure(compositorInterop->CreateGraphicsDevice(
            _d2dDevice.Get(),
            &_compositionGraphicsDevice));
    }

    // Called when Direct3D signals the device lost event
    void OnDirect3DDeviceLost()
    {
        // Create a new device
        FailFastOnFailure(CreateDirect2DDevice(_d2dDevice.ReleaseAndGetAddressOf()));

        // Restore our composition graphics device to good health
        ComPtr<ICompositionGraphicsDeviceInterop> compositionGraphicsDeviceInterop;
        FailFastOnFailure(_compositionGraphicsDevice.As(&compositionGraphicsDeviceInterop));
        FailFastOnFailure(compositionGraphicsDeviceInterop->SetRenderingDevice(_d2dDevice.Get()));
    }

    // Create a surface that is asynchronously filled with an image
    ComPtr<ICompositionSurface> CreateSurfaceFromTextLayout(IDWriteTextLayout* text)
    {
        // Create our wrapper object that will handle downloading and decoding the image (assume
        // throwing new here)
        SampleText* textSurface = new SampleText(text, _compositionGraphicsDevice.Get());

        // Keep our image alive
        _textSurfaces.push_back(textSurface);

        // The caller is only interested in the underlying surface
        return textSurface->get_Surface();
    }

    // Create a visual that holds an image
    ComPtr<IVisual> CreateVisualFromTextLayout(IDWriteTextLayout* text)
    {
        // Create a sprite visual
        ComPtr<ISpriteVisual> spriteVisual;
        FailFastOnFailure(_compositor->CreateSpriteVisual(&spriteVisual));

        // The sprite visual needs a brush to hold the image
        ComPtr<ICompositionSurfaceBrush> surfaceBrush;
        FailFastOnFailure(_compositor->CreateSurfaceBrushWithSurface(
            CreateSurfaceFromTextLayout(text).Get(),
            &surfaceBrush));

        // Associate the brush with the visual
        ComPtr<ICompositionBrush> brush;
        FailFastOnFailure(surfaceBrush.As(&brush));
        FailFastOnFailure(spriteVisual->put_Brush(brush.Get()));

        // Return the visual to the caller as the base class
        ComPtr<IVisual> visual;
        FailFastOnFailure(spriteVisual.As(&visual));

        return visual;
    }

private:
    // This helper (implementation not shown here) creates a Direct2D device and registers
    // for a device loss notification on the underlying Direct3D device. When that notification is
    // raised, assume the OnDirect3DDeviceLost method is called.
    HRESULT CreateDirect2DDevice(ID2D1Device** ppDevice);
};
```

 

 






<!--HONumber=May16_HO2-->


