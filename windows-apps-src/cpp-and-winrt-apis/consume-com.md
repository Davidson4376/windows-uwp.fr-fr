---
author: stevewhims
description: Cette rubrique utilise un exemple de code complet Direct2D pour montrer comment utiliser C + / WinRT pour utiliser les classes et interfaces COM.
title: Consommer DirectX et autres API COM avec C + / WinRT
ms.author: stwhi
ms.date: 07/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c ++, RPC, winrt, COM, composant, classe, interface
ms.localizationpriority: medium
ms.openlocfilehash: b87eb90ed5ecf731cc851e81e81ad016956e5fea
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2800070"
---
# <a name="consume-directx-and-other-com-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Consommer DirectX et autres API COM avec [C + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

Vous pouvez utiliser les fonctionnalités de C + / bibliothèque WinRT pour consommer des composants COM, tels que les graphiques 2D et 3D hautes performances des APIs DirectX. C + / WinRT est la façon la plus simple d’utiliser DirectX sans compromettre les performances. Cette rubrique utilise un exemple de code Direct2D pour montrer comment utiliser C + / WinRT pour utiliser les classes et interfaces COM. Vous pouvez, bien sûr, combiner programmation COM et le module d’exécution Windows dans le même C + / WinRT project.

À la fin de cette rubrique, vous trouverez une liste de code source complet d’une application Direct2D minimale. Nous soulevez extraits de code et les utiliser pour illustrer comment utiliser des composants COM à l’aide de C + / WinRT à l’aide des différentes fonctionnalités de C + / bibliothèque WinRT.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>Pointeurs intelligents COM ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Lorsque vous programmez avec COM, vous travaillez directement avec les interfaces plutôt que des objets (c'est-à-dire l’également la valeur true en coulisses for Windows Runtime APIs, une évolution de COM). Pour appeler une fonction sur une classe COM, par exemple, vous activez la classe, obtenir une interface précédent, puis d’appeler des fonctions de cette interface. Pour accéder à l’état d’un objet, vous n’accédez pas à ses membres de données directement. au lieu de cela, vous appelez des fonctions d’accesseur et mutateur sur une interface.

Pour être plus spécifique, notre sur l’interaction avec *des pointeurs*d’interface. Et pour ce faire, nous bénéficier de l’existence du type de pointeur intelligent COM dans C + / WinRT&mdash;le type de [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) .

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
```

Le code ci-dessus illustre comment déclarer un pointeur intelligent non initialisé à une interface COM de [**ID2D1Factory1**](https://msdn.microsoft.com/library/Hh404596) . Le pointeur intelligent est initialisé, afin qu’elle pointe vers une interface **ID2D1Factory1** appartenant à n’importe quel objet réel (il ne pointe pas vers une interface tout) n’est pas encore. Mais il est susceptible de faire. et (en cours un pointeur intelligent) a la possibilité de via COM décompte de références pour gérer la durée de vie de l’objet propriétaire de l’interface qu’elle pointe vers et être le moyen par lequel vous appelez des fonctions de cette interface.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>Fonctions COM qui retournent un pointeur d’interface en tant que **void\ * \ ***

Vous pouvez appeler la fonction [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) pour écrire dans un pointeur intelligent non initialisé sous-jacent pointeur brut.

```cppwinrt
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

Le code ci-dessus appelle la fonction [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) , qui retourne un pointeur d’interface **ID2D1Factory1** via son dernier paramètre, qui a **void\ * \ *** type. De nombreuses fonctions COM renvoient un **void\ * \ ***. Pour ces fonctions, utilisez [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) comme indiqué.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>Fonctions COM qui retournent un pointeur d’interface spécifique

La fonction [**D3D11CreateDevice**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) retourne un pointeur d’interface [**ID3D11Device**](https://msdn.microsoft.com/library/Hh404596) par le biais de son paramètre l'antépénultième, qui a **ID3D11Device\ * \ *** type. Pour les fonctions qui retournent un pointeur d’interface spécifique en procédant, utilisez [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

L’exemple de code dans la section précédente indique comment appeler la fonction **D2D1CreateFactory** brute. Mais en fait, lors de l’exemple de code de cette rubrique appelle **D2D1CreateFactory**, elle utilise un modèle de fonction d’assistance qui encapsule l’API brute et l’exemple de code utilise donc réellement [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>Fonctions COM qui retournent un pointeur d’interface en tant que **IUnknown\ * \ ***

La fonction [**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) retourne un pointeur d’interface en usine DirectWrite via son dernier paramètre, qui a **IUnknown\ * \ *** type. Pour une fonction utiliser [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function), mais réinterprétation cast à **IUnknown\ * \ ***.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>Remettre en place un **winrt::com_ptr**

> [!IMPORTANT]
> Si vous avez un [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) qui est déjà en place (son pointeur brut interne dispose déjà d’une cible) et vous souhaitez qu’elle pointe vers un autre objet remettre en place, puis vous devez tout d’abord affecter `nullptr` lui&mdash;comme indiqué dans l’exemple de code ci-dessous. Si vous n’avez pas, puis un déjà insérée **com_ptr** Dessine le problème à votre attention (lorsque vous appelez [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) ou [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)) en déclarant que son pointeur interne n’est pas null.

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

Pour vérifier que la valeur d’une valeur HRESULT retournée par une fonction COM et une exception dans le cas où il représente un code d’erreur, appelez [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult).

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>Fonctions COM qui acceptent un pointeur d’interface spécifique

Vous pouvez appeler la fonction [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function) pour transmettre votre **com_ptr** à une fonction qui prend un pointeur d’interface spécifique du même type.

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>Fonctions COM qui prennent un pointeur d’interface **IUnknown**

Vous pouvez appeler la fonction libre [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function) à transmettre votre **com_ptr** à une fonction qui prend un pointeur d’interface **IUnknown** .

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>Passage et retour COM actives pointeurs

Une fonction qui prend un pointeur intelligent COM sous la forme d’un **winrt::com_ptr** doit faire par référence à une constante, ou par référence.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Une fonction qui retourne un **winrt::com_ptr** doit faire en valeur.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Un pointeur intelligent COM pour une autre interface de requête

Vous pouvez utiliser la fonction [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) pour interroger un pointeur intelligent COM pour une autre interface. La fonction lève une exception si la requête n’aboutit pas.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

Également utiliser [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#comptrtryas-function), qui retourne une valeur que vous pouvez cocher contre `nullptr` pour voir si la requête a réussi.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Liste de code source complet d’une application Direct2D minimale

Si vous souhaitez générer et exécuter cet exemple de code source puis la première, dans Visual Studio, créez un nouveau **application principaux (C + / WinRT)**. `Direct2D` est un nom pour le projet raisonnable, mais vous pouvez nommer comme vous le souhaitez. Open `App.cpp`, de supprimer tout son contenu et coller du texte dans la liste ci-dessous.

```cppwinrt
#include "pch.h"

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

        WINRT_TRACE("Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
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

## <a name="important-apis"></a>API importantes
* [WinRT::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Structure winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
