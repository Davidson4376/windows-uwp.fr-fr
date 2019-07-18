---
description: Cette rubrique utilise un exemple de code complet Direct2D pour montrer comment utiliser C++/WinRT pour consommer des classes et interfaces COM.
title: Consommer des composants COM avec C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, norme, c++, cpp, winrt, COM, composant, classe, interface
ms.localizationpriority: medium
ms.openlocfilehash: bb28ec7afa22f81033bfce2aff530119e53a4b91
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660158"
---
# <a name="consume-com-components-with-cwinrt"></a>Consommer des composants COM avec C++/WinRT

Vous pouvez utiliser les fonctionnalités de la bibliothèque [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) pour consommer des composants COM, tels que les graphismes 2D et 3D haute performance des API DirectX. C++/WinRT est le moyen le plus simple d’utiliser DirectX sans compromettre les performances. Cette rubrique est basée sur un exemple de code Direct2D qui permet d’illustrer l’utilisation de C++/WinRT pour consommer des classes et des interfaces COM. Vous pouvez, bien sûr, mélanger la programmation COM et Windows Runtime dans le même projet C++/WinRT.

À la fin de cette rubrique, vous trouverez le code source complet d’une application Direct2D minimale. Nous allons utiliser des extraits de ce code pour illustrer la consommation des composants COM avec C++/WinRT et diverses fonctionnalités de la bibliothèque C++/WinRT.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>Pointeurs intelligents COM ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Quand vous programmez avec COM, vous utilisez directement des interfaces et non des objets (cela est également vrai pour les API Windows Runtime, qui sont une évolution de COM). Pour appeler une fonction sur une classe COM, par exemple, vous l’activez, vous récupérez une interface, puis vous appelez des fonctions sur cette interface. Pour accéder à l’état d’un objet, vous n’accédez pas directement à ses membres de données. Vous appelez en fait des fonctions d’accesseur et de mutateur sur une interface.

Pour être plus précis, nous parlons d’interaction avec les *pointeurs* d’interface. Et pour cela, nous tirons parti de l’existence du type de pointeur intelligent COM avec C++/WinRT, le type [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr).

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

Le code ci-dessus montre comment déclarer un pointeur intelligent non initialisé sur une interface COM [**ID2D1Factory1**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1). Le pointeur intelligent n’est pas initialisé. Il ne pointe donc pas encore vers une interface **ID2D1Factory1** appartenant à un objet réel (il ne pointe pas du tout vers une interface). Toutefois, il a le potentiel pour le faire. Dans la mesure où il est un pointeur intelligent, il peut tirer parti du comptage de références COM pour manager la durée de vie de l’objet propriétaire de l’interface vers laquelle il pointe. De plus, il peut servir à appeler des fonctions sur cette interface.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>Fonctions COM qui retournent un pointeur d’interface en tant que **void**

Vous pouvez appeler la fonction [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) pour écrire dans le pointeur brut sous-jacent d’un pointeur intelligent non initialisé.

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

Le code ci-dessus appelle la fonction [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory), qui retourne un pointeur d’interface **ID2D1Factory1** via son dernier paramètre, dont le type est **void\*\*** . De nombreuses fonctions COM retournent un **void\*\*** . Pour de telles fonctions, utilisez [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function), comme indiqué.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>Fonctions COM qui retournent un pointeur d’interface spécifique

La fonction [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) retourne un pointeur d’interface [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) via son avant-avant-dernier paramètre, dont le type est **ID3D11Device\*\*** . Pour les fonctions qui retournent un pointeur d’interface spécifique comme celui-ci, utilisez [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

L’exemple de code de la section précédente montre comment appeler la fonction brute **D2D1CreateFactory**. Toutefois, quand l’exemple de code de cette rubrique appelle **D2D1CreateFactory**, il utilise un modèle de fonction d’assistance qui inclut dans un wrapper l’API brute. Ainsi, l’exemple de code utilise en réalité [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>Fonctions COM qui retournent un pointeur d’interface en tant que **IUnknown**

La fonction [**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) retourne un pointeur d’interface de fabrique DirectWrite via son dernier paramètre, dont le type est [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown). Pour une telle fonction, utilisez [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function), mais castez-le par réinterprétation vers **IUnknown**.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>Repositionner un **winrt::com_ptr**

> [!IMPORTANT]
> Si vous avez un [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) déjà positionné (son pointeur brut interne a déjà une cible) et si vous souhaitez le repositionner pour qu’il pointe vers un autre objet, vous devez d’abord lui affecter `nullptr`, comme indiqué dans l’exemple de code ci-dessous. Sinon, un **com_ptr** déjà positionné va vous signaler le problème (quand vous appellerez [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) ou [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)) en indiquant que son pointeur interne n’a pas une valeur null.

```cppwinrt
winrt::com_ptr<ID2D1SolidColorBrush> brush;
...
    brush.put()
...
brush = nullptr; // Important because we're about to re-seat
target->CreateSolidColorBrush(
    color_orange,
    D2D1::BrushProperties(0.8f),
    brush.put()));
```

## <a name="handle-hresult-error-codes"></a>Prendre en charge les codes d’erreur HRESULT

Pour vérifier la valeur d’un HRESULT retourné par une fonction COM, et lever une exception s’il représente un code d’erreur, appelez [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult).

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>Fonctions COM qui acceptent un pointeur d’interface spécifique

Vous pouvez appeler la fonction [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) pour passer votre **com_ptr** à une fonction qui accepte un pointeur d’interface spécifique du même type.

```cppwinrt
... ExampleFunction(
    winrt::com_ptr<ID2D1Factory1> const& factory,
    winrt::com_ptr<IDXGIDevice> const& dxdevice)
{
    ...
    winrt::check_hresult(factory->CreateDevice(dxdevice.get(), ...));
    ...
}
```

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>Fonctions COM qui acceptent un pointeur d’interface **IUnknown**

Vous pouvez appeler la fonction libre [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#get_unknown-function) pour passer votre **com_ptr** à une fonction qui accepte un pointeur d’interface **IUnknown**.

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>Passage et retour de pointeurs intelligents COM

Une fonction qui accepte un pointeur intelligent COM sous la forme d’un **winrt::com_ptr** doit le faire par référence constante ou par référence.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Une fonction qui retourne un **winrt::com_ptr** doit le faire par valeur.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Interroger un pointeur intelligent COM d’une autre interface

Vous pouvez utiliser la fonction [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) pour interroger un pointeur intelligent COM d’une autre interface. La fonction lève une exception si la requête n’aboutit pas.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

Vous pouvez également utiliser [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function), qui retourne une valeur à comparer à `nullptr` pour déterminer si la requête a abouti.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Code source complet d’une application Direct2D minimale

Si vous souhaitez générer et exécuter cet exemple de code source, commencez par créer dans Visual Studio une **application Core (C++/WinRT)** . `Direct2D` est un nom approprié pour le projet, mais vous pouvez le changer comme bon vous semble. Ouvrez `App.cpp`, supprimez tout son contenu, puis collez les lignes de code ci-dessous.

Le code ci-dessous utilise la [fonction winrt::com_ptr::capture](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcapture-function) dans la mesure du possible. `WINRT_ASSERT` est une définition de macro, qui se développe en [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
#include "pch.h"
#include <d2d1_1.h>
#include <d3d11.h>
#include <dxgi1_2.h>
#include <winrt/Windows.Graphics.Display.h>

using namespace winrt;

using namespace Windows;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::UI;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

namespace
{
    winrt::com_ptr<ID2D1Factory1> CreateFactory()
    {
        D2D1_FACTORY_OPTIONS options{};

#ifdef _DEBUG
        options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

        winrt::com_ptr<ID2D1Factory1> factory;

        winrt::check_hresult(D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            options,
            factory.put()));

        return factory;
    }

    HRESULT CreateDevice(D3D_DRIVER_TYPE const type, winrt::com_ptr<ID3D11Device>& device)
    {
        WINRT_ASSERT(!device);

        return D3D11CreateDevice(
            nullptr,
            type,
            nullptr,
            D3D11_CREATE_DEVICE_BGRA_SUPPORT,
            nullptr, 0,
            D3D11_SDK_VERSION,
            device.put(),
            nullptr,
            nullptr);
    }

    winrt::com_ptr<ID3D11Device> CreateDevice()
    {
        winrt::com_ptr<ID3D11Device> device;
        HRESULT hr{ CreateDevice(D3D_DRIVER_TYPE_HARDWARE, device) };

        if (DXGI_ERROR_UNSUPPORTED == hr)
        {
            hr = CreateDevice(D3D_DRIVER_TYPE_WARP, device);
        }

        winrt::check_hresult(hr);
        return device;
    }

    winrt::com_ptr<ID2D1DeviceContext> CreateRenderTarget(
        winrt::com_ptr<ID2D1Factory1> const& factory,
        winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(factory);
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<ID2D1Device> d2device;
        winrt::check_hresult(factory->CreateDevice(dxdevice.get(), d2device.put()));

        winrt::com_ptr<ID2D1DeviceContext> target;
        winrt::check_hresult(d2device->CreateDeviceContext(D2D1_DEVICE_CONTEXT_OPTIONS_NONE, target.put()));
        return target;
    }

    winrt::com_ptr<IDXGIFactory2> GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<IDXGIAdapter> adapter;
        winrt::check_hresult(dxdevice->GetAdapter(adapter.put()));

        winrt::com_ptr<IDXGIFactory2> factory;
        factory.capture(adapter, &IDXGIAdapter::GetParent);
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        surface.capture(swapchain, &IDXGISwapChain1::GetBuffer, 0);

        D2D1_BITMAP_PROPERTIES1 const props{ D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_IGNORE)) };

        winrt::com_ptr<ID2D1Bitmap1> bitmap;

        winrt::check_hresult(target->CreateBitmapFromDxgiSurface(surface.get(),
            props,
            bitmap.put()));

        target->SetTarget(bitmap.get());
    }

    winrt::com_ptr<IDXGISwapChain1> CreateSwapChainForCoreWindow(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIFactory2> const factory{ GetDxgiFactory(device) };

        DXGI_SWAP_CHAIN_DESC1 props{};
        props.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
        props.SampleDesc.Count = 1;
        props.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        props.BufferCount = 2;
        props.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;

        winrt::com_ptr<IDXGISwapChain1> swapChain;

        winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
            device.get(),
            winrt::get_unknown(CoreWindow::GetForCurrentThread()),
            &props,
            nullptr, // all or nothing
            swapChain.put()));

        return swapChain;
    }

    constexpr D2D1_COLOR_F color_white{ 1.0f,  1.0f,  1.0f,  1.0f };
    constexpr D2D1_COLOR_F color_orange{ 0.92f,  0.38f,  0.208f,  1.0f };
}

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::com_ptr<ID2D1Factory1> m_factory;
    winrt::com_ptr<ID2D1DeviceContext> m_target;
    winrt::com_ptr<IDXGISwapChain1> m_swapChain;
    winrt::com_ptr<ID2D1SolidColorBrush> m_brush;
    float m_dpi{};

    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const&)
    {
    }

    void Load(hstring const&)
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };

        window.SizeChanged([&](auto&&...)
        {
            if (m_target)
            {
                ResizeSwapChainBitmap();
                Render();
            }
        });

        DisplayInformation const display{ DisplayInformation::GetForCurrentView() };
        m_dpi = display.LogicalDpi();

        display.DpiChanged([&](DisplayInformation const& display, IInspectable const&)
        {
            if (m_target)
            {
                m_dpi = display.LogicalDpi();
                m_target->SetDpi(m_dpi, m_dpi);
                CreateDeviceSizeResources();
                Render();
            }
        });

        m_factory = CreateFactory();
        CreateDeviceIndependentResources();
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };
        window.Activate();

        Render();
        CoreDispatcher const dispatcher{ window.Dispatcher() };
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const&) {}

    void Draw()
    {
        m_target->Clear(color_white);

        D2D1_SIZE_F const size{ m_target->GetSize() };
        D2D1_RECT_F const rect{ 100.0f, 100.0f, size.width - 100.0f, size.height - 100.0f };
        m_target->DrawRectangle(rect, m_brush.get(), 100.0f);

        char buffer[1024];
        (void)snprintf(buffer, sizeof(buffer), "Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
        ::OutputDebugStringA(buffer);
    }

    void Render()
    {
        if (!m_target)
        {
            winrt::com_ptr<ID3D11Device> const device{ CreateDevice() };
            m_target = CreateRenderTarget(m_factory, device);
            m_swapChain = CreateSwapChainForCoreWindow(device);

            CreateDeviceSwapChainBitmap(m_swapChain, m_target);

            m_target->SetDpi(m_dpi, m_dpi);

            CreateDeviceResources();
            CreateDeviceSizeResources();
        }

        m_target->BeginDraw();
        Draw();
        m_target->EndDraw();

        HRESULT const hr{ m_swapChain->Present(1, 0) };

        if (S_OK != hr && DXGI_STATUS_OCCLUDED != hr)
        {
            ReleaseDevice();
        }
    }

    void ReleaseDevice()
    {
        m_target = nullptr;
        m_swapChain = nullptr;

        ReleaseDeviceResources();
    }

    void ResizeSwapChainBitmap()
    {
        WINRT_ASSERT(m_target);
        WINRT_ASSERT(m_swapChain);

        m_target->SetTarget(nullptr);

        if (S_OK == m_swapChain->ResizeBuffers(0, // all buffers
            0, 0, // client area
            DXGI_FORMAT_UNKNOWN, // preserve format
            0)) // flags
        {
            CreateDeviceSwapChainBitmap(m_swapChain, m_target);
            CreateDeviceSizeResources();
        }
        else
        {
            ReleaseDevice();
        }
    }

    void CreateDeviceIndependentResources()
    {
    }

    void CreateDeviceResources()
    {
        winrt::check_hresult(m_target->CreateSolidColorBrush(
            color_orange,
            D2D1::BrushProperties(0.8f),
            m_brush.put()));
    }

    void CreateDeviceSizeResources()
    {
    }

    void ReleaseDeviceResources()
    {
        m_brush = nullptr;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>Utilisation des types COM, tels que BSTR et VARIANT

Comme vous pouvez le voir, C++/WinRT prend en charge l’implémentation et l’appel des interfaces COM. Pour utiliser des types COM, tels que BSTR et VARIANT, nous recommandons d’employer les wrappers fournis par les [WIL (bibliothèques d’implémentation Windows)](https://github.com/Microsoft/wil), par exemple **wil::unique_bstr** et **wil::unique_variant** (qui gèrent les durées de vie des ressources).

[WIL](https://github.com/Microsoft/wil) remplace les frameworks comme ATL (Active Template Library) et la prise en charge COM du compilateur Visual C++. Nous vous recommandons de l’utiliser au lieu d’écrire vos propres wrappers, ou d’utiliser des types COM tels que BSTR et VARIANT dans leur forme brute (avec les API appropriées).

## <a name="avoiding-namespace-collisions"></a>Évitement des collisions d’espaces de noms

Il est courant en C++/WinRT, comme le montre le listing de code de cette rubrique, d’utiliser librement les directives using. Dans certains cas, toutefois, cela peut poser un problème : l’importation de noms en conflit dans l’espace de noms global. Voici un exemple :

C++/WinRT contient un type nommé[**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) alors que COM définit un type nommé [ **::IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown). Prenons l’exemple de code suivant dans un projet C++/WinRT qui consomme des en-têtes COM.

```cppwinrt
using namespace winrt::Windows::Foundation;
...
void MyFunction(IUnknown*); // error C2872:  'IUnknown': ambiguous symbol
```

Le nom non qualifié *IUnknown* est en conflit dans l’espace de noms global, ce qui entraîne l’erreur de compilation *symbole ambigu*. À la place, vous pouvez isoler la version C++/WinRT du nom dans l’espace de noms **winrt**, comme suit.

```cppwinrt
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

Ou, si vous préférez le côté pratique de `using namespace winrt`, vous pouvez l’utiliser. Il vous suffit de qualifier la version globale de *IUnknown*, comme suit.

```cppwinrt
using namespace winrt;
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(::IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

Bien entendu, cela fonctionne avec n’importe quel espace de noms C++/WinRT.

```cppwinrt
namespace winrt
{
    using namespace Windows::Storage;
    using namespace Windows::System;
}
```

Vous pouvez ensuite faire référence à **winrt::Windows::Storage::StorageFile**, par exemple, simplement en tant que **winrt::StorageFile**.

## <a name="important-apis"></a>API importantes
* [fonction winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [modèle de struct winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [struct winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
