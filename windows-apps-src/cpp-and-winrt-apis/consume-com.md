---
description: Cette rubrique utilise un exemple de code complet Direct2D pour montrer comment utiliser C++ / c++ / WinRT pour consommer des classes et interfaces COM.
title: Utiliser des composants COM avec C++/WinRT
ms.date: 07/23/2018
ms.topic: article
keywords: Windows 10, uwp, standard, c ++, cpp, winrt, COM, composant, classe, interface
ms.localizationpriority: medium
ms.openlocfilehash: 129477689e12de2634b422a0fc4487b283e3bf03
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644804"
---
# <a name="consume-com-components-with-cwinrt"></a>Utiliser des composants COM avec C++/WinRT

Vous pouvez utiliser les fonctionnalités de la [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) bibliothèque pour consommer des composants COM, tels que les graphiques de 2D et 3D hautes performances des APIs DirectX. C++ / c++ / WinRT est la façon la plus simple d’utiliser DirectX sans compromettre les performances. Cette rubrique utilise un exemple de code Direct2D pour montrer comment utiliser C++ / c++ / WinRT pour consommer des classes et interfaces COM. Vous pouvez, bien sûr, combiner des programmation COM et Windows Runtime en C++ même / c++ / WinRT projet.

À la fin de cette rubrique, vous trouverez une liste de code source complet d’une application Direct2D minimale. Nous allons extraits à partir de ce code de courbes d’élévation et utilisez-les pour illustrer comment consommer des composants COM à l’aide de C++ / c++ / WinRT à l’aide de différentes fonctions de C / c++ / WinRT bibliothèque.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>Pointeurs intelligents COM ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Lorsque vous programmez avec COM, vous travaillez directement avec les interfaces plutôt qu’avec des objets (c'est-à-dire du également true dans les coulisses de Windows Runtime APIs, qui sont une évolution de COM). Pour appeler une fonction sur une classe COM, par exemple, vous activez la classe, obtenir une interface en retour, et puis d’appeler des fonctions sur cette interface. Pour accéder à l’état d’un objet, vous n’accédez pas à ses membres de données directement. au lieu de cela, vous appelez des fonctions d’accesseur et mutateur sur une interface.

Pour être plus précis, nous parlons d’interagir avec l’interface *pointeurs*. Et pour ce faire, nous tirer parti de l’existence du type de pointeur intelligent COM en C / c++ / WinRT&mdash;le [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) type.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
```

Le code ci-dessus montre comment déclarer un pointeur intelligent non initialisé à une [ **ID2D1Factory1** ](https://msdn.microsoft.com/library/Hh404596) interface COM. Le pointeur intelligent n’est pas initialisé, donc il n’est pas encore pointant vers un **ID2D1Factory1** interface appartenant à n’importe quel objet réel (il ne pointe pas vers une interface du tout). Mais il est susceptible de faire. et (qui est un pointeur intelligent), il a la possibilité via COM décompte de références pour gérer la durée de vie de l’objet propriétaire de l’interface vers laquelle il pointe et être le moyen par lequel vous appelez des fonctions sur cette interface.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>Les fonctions COM qui retournent un pointeur d’interface en tant que **void**

Vous pouvez appeler la [ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) fonction pour écrire dans un pointeur intelligent non initialisé sous-jacente du pointeur brut.

```cppwinrt
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

Le code ci-dessus appelle le [ **D2D1CreateFactory** ](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) (fonction), qui retourne un **ID2D1Factory1** pointeur d’interface par le biais de son dernier paramètre, qui a **void \* \***  type. De nombreuses fonctions COM retournent un **void\*\***. Pour ces fonctions, utilisez [ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) comme indiqué.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>Fonctions COM qui retournent un pointeur d’interface spécifique

Le [ **D3D11CreateDevice** ](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) fonction renvoie une [ **ID3D11Device** ](https://msdn.microsoft.com/library/Hh404596) pointeur d’interface par le biais de son paramètre l'antépénultième, ce qui a **ID3D11Device\* \***  type. Pour les fonctions qui retournent un pointeur d’interface spécifique comme celle-ci, utilisez [ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

L’exemple de code dans la section avant celle-ci montre comment appeler brut **D2D1CreateFactory** (fonction). Mais en fait, lorsque l’exemple de code pour cette rubrique appelle **D2D1CreateFactory**, il utilise un modèle de fonction d’assistance qui encapsule l’API brute et par conséquent, l’exemple de code utilise réellement [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>Les fonctions COM qui retournent un pointeur d’interface en tant que **IUnknown**

Le [ **DWriteCreateFactory** ](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) fonction retourne un pointeur d’interface de fabrique DirectWrite par le biais de son dernier paramètre, qui a [ **IUnknown** ](https://msdn.microsoft.com/library/windows/desktop/ms680509)type. Pour une telle fonction, utilisez [ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function), mais effectuer un cast de réinterprétation à **IUnknown**.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>Remettre en place un **winrt::com_ptr**

> [!IMPORTANT]
> Si vous avez un [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) qui est déjà en place (son pointeur brut interne dispose déjà d’une cible) et vous souhaitez remettre en place qu’il pointe vers un autre objet, vous devez tout d’abord affecter `nullptr` à ce dernier&mdash;comme indiqué dans l’exemple de code ci-dessous. Si vous n’est pas, puis un déjà-assis **com_ptr** Dessine le problème à votre attention (lorsque vous appelez [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) ou [ **com_ptr :: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)) en déclarant que son pointeur interne n’est pas null.

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

## <a name="handle-hresult-error-codes"></a>Gérer les codes d’erreur HRESULT

Pour vérifier la valeur d’une valeur HRESULT retournée à partir d’une fonction COM et lève une exception dans le cas où il représente un code d’erreur, appelez [ **winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult).

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>Fonctions COM qui acceptent un pointeur d’interface spécifique

Vous pouvez appeler la [ **com_ptr::get** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function) (fonction) pour transmettre votre **com_ptr** à une fonction qui accepte un pointeur d’interface spécifique du même type.

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>Des fonctions COM utilisant un **IUnknown** pointeur d’interface

Vous pouvez appeler la [ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function) gratuit de fonction pour passer votre **com_ptr** à une fonction qui accepte un **IUnknown** interface pointeur.

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>Passage et le retour de COM de pointeurs intelligents

Une fonction qui accepte un pointeur intelligent COM sous la forme d’un **winrt::com_ptr** doit le faire par référence à une constante, ou par référence.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Une fonction qui retourne un **winrt::com_ptr** doit le faire par valeur.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Un pointeur intelligent COM pour une interface différente de la requête

Vous pouvez utiliser la [ **com_ptr::as** ](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) (fonction) pour interroger un pointeur intelligent COM pour une interface différente. La fonction lève une exception si la requête n’aboutit pas.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

Vous pouvez également utiliser [ **com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#comptrtryas-function), qui retourne une valeur que vous pouvez vérifier par rapport aux `nullptr` pour voir si la requête a réussi.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Liste de code source complet d’une application Direct2D minimale

Si vous souhaitez générer et exécuter cet exemple de code source, puis tout d’abord, dans Visual Studio, créez un **application Core (C++ / c++ / WinRT)**. `Direct2D` est un nom raisonnable pour le projet, mais vous pouvez le nommer comme vous le souhaitez. Ouvrez `App.cpp`, supprimer tout son contenu et collez dans la liste ci-dessous.

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
        winrt::check_hresult(adapter->GetParent(__uuidof(factory), factory.put_void()));
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        winrt::check_hresult(swapchain->GetBuffer(0, __uuidof(surface), surface.put_void()));

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
    CoreApplication::Run(App());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>Utilisation des types COM, tels que BSTR et VARIANT

Comme vous pouvez le voir, C++ / c++ / WinRT fournit la prise en charge pour les implémenter et appeler des interfaces COM. L’utilisation des types COM, tels que BSTR et VARIANT, il existe toujours la possibilité d’utiliser ceux dans leur forme brute (conjointement avec les API appropriées). Vous pouvez également utiliser les wrappers fournis par une infrastructure, tels que le [bibliothèque ATL (Active Template)](/cpp/atl/active-template-library-atl-concepts), ou par le compilateur Visual C++ [prise en charge COM](/cpp/cpp/compiler-com-support), ou même par vos propres wrappers.

## <a name="important-apis"></a>API importantes
* [winrt::check_hresult function](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr struct template](/uwp/cpp-ref-for-winrt/com-ptr)
* [WinRT::Windows::Foundation::IUnknown struct](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
