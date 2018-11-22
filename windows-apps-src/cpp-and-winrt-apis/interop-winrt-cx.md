---
author: stevewhims
description: Cette rubrique montre deux fonctions d’assistance qui peuvent être utilisées pour effectuer des conversions entre des objets C++/CX et C++/WinRT.
title: Interopérabilité entre C++/WinRT et C++/CX
ms.author: stwhi
ms.date: 10/09/2018
ms.topic: article
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, port, migrer, interopérabilité, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: ca3cc69065ef2898aebafc832da1639985231d60
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7561380"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interopérabilité entre C++/WinRT et C++/CX

Stratégies pour le portage progressivement le code de votre [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) de projet à [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) décrits dans [transférer vers C++ / WinRT à partir de C++ / CX](move-to-winrt-from-cx.md).

Cette rubrique montre deux fonctions d’assistance que vous pouvez utiliser pour convertir entre C++ / CX et C++ / WinRT des objets dans le même projet. Vous pouvez les utiliser pour l’interopérabilité entre le code qui utilise les deux projections de langage, ou vous pouvez utiliser les fonctions comme vous portez votre code à partir de C++ / CX vers C++ / WinRT.

## <a name="fromcx-and-tocx-functions"></a>Fonctions from_cx et to_cx
La fonction d’assistance ci-dessous convertit un objet C++/CX en un objet équivalent C++/WinRT. La fonction caste un objet C++/CX vers son pointeur d’interface [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) sous-jacent. Elle appelle ensuite [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) sur ce pointeur pour rechercher l’interface par défaut de l’objet C++/WinRT. **QueryInterface** est l’équivalent de l’interface binaire d’application (ABI) Windows Runtime de l’extension safe_cast C++/CX. Puis, la fonction [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) récupère l’adresse du pointeur d’interface **IUnknown** sous-jacent d’un objet C++/WinRT afin qu’elle puisse être définie sur une autre valeur.

```cppwinrt
template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

La fonction d’assistance ci-dessous convertit un objet C++/WinRT en un objet équivalent C++/CX. La fonction [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) extrait un pointeur vers l’interface **IUnknown** sous-jacente d’un objet C++/WinRT. La fonction caste ce pointeur vers un objet C++/CX avant d’utiliser l’extension safe-cast C++/CX pour rechercher le type C++/CX demandé.

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>Exemple de projet montrant les deux fonctions d’assistance en cours d’utilisation

À reproduire, d’une manière simple, le scénario de portage progressivement le code de C++ / projet CX vers C++ / WinRT, vous pouvez commencer en créant un nouveau projet dans Visual Studio à l’aide d’une des C++ / WinRT les modèles de projet (voir [prise en charge de Visual Studio pour C++ / WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)).

Cet exemple de projet montre également comment vous pouvez utiliser des alias d’espace de noms pour les différents îlots du code, afin d’éviter les collisions potentielles espace de noms entre la C++ / WinRT projection et C++ / projection de CX.

- Créer un **Visual C++** \> **Windows universel** > **Core App (C++ / WinRT)** projet.
- Dans les propriétés du projet, **C/C++** \> **Général** \> **Consommer l’Extension Windows Runtime** \> **Oui (/ZW)**. Cela active la prise en charge de projet pour C++ / CX.
- Remplacez le contenu du `App.cpp` avec le code ci-dessous.

```cppwinrt
// App.cpp
#include "pch.h"
#include <sstream>

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows;
    using namespace Windows::ApplicationModel::Core;
    using namespace Windows::Foundation;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI;
    using namespace Windows::UI::Core;
    using namespace Windows::UI::Composition;
}

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}

struct App : winrt::implements<App, winrt::IFrameworkViewSource, winrt::IFrameworkView>
{
    winrt::CompositionTarget m_target{ nullptr };
    winrt::VisualCollection m_visuals{ nullptr };
    winrt::Visual m_selected{ nullptr };
    winrt::float2 m_offset{};

    winrt::IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(winrt::CoreApplicationView const &)
    {
    }

    void Load(winrt::hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        winrt::CoreWindow window = winrt::CoreWindow::GetForCurrentThread();
        window.Activate();

        winrt::CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(winrt::CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(winrt::CoreWindow const & window)
    {
        winrt::Compositor compositor;
        winrt::ContainerVisual root = compositor.CreateContainerVisual();
        m_target = compositor.CreateTargetForCurrentView();
        m_target.Root(root);
        m_visuals = root.Children();

        window.PointerPressed({ this, &App::OnPointerPressed });
        window.PointerMoved({ this, &App::OnPointerMoved });

        window.PointerReleased([&](auto && ...)
        {
            m_selected = nullptr;
        });
    }

    void OnPointerPressed(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        winrt::float2 const point = args.CurrentPoint().Position();

        for (winrt::Visual visual : m_visuals)
        {
            winrt::float3 const offset = visual.Offset();
            winrt::float2 const size = visual.Size();

            if (point.x >= offset.x &&
                point.x < offset.x + size.x &&
                point.y >= offset.y &&
                point.y < offset.y + size.y)
            {
                m_selected = visual;
                m_offset.x = offset.x - point.x;
                m_offset.y = offset.y - point.y;
            }
        }

        if (m_selected)
        {
            m_visuals.Remove(m_selected);
            m_visuals.InsertAtTop(m_selected);
        }
        else
        {
            AddVisual(point);
        }
    }

    void OnPointerMoved(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        if (m_selected)
        {
            winrt::float2 const point = args.CurrentPoint().Position();

            m_selected.Offset(
            {
                point.x + m_offset.x,
                point.y + m_offset.y,
                0.0f
            });
        }
    }

    void AddVisual(winrt::float2 const point)
    {
        winrt::Compositor compositor = m_visuals.Compositor();
        winrt::SpriteVisual visual = compositor.CreateSpriteVisual();

        static winrt::Color colors[] =
        {
            { 0xDC, 0x5B, 0x9B, 0xD5 },
            { 0xDC, 0xED, 0x7D, 0x31 },
            { 0xDC, 0x70, 0xAD, 0x47 },
            { 0xDC, 0xFF, 0xC0, 0x00 }
        };

        static unsigned last = 0;
        unsigned const next = ++last % _countof(colors);
        visual.Brush(compositor.CreateColorBrush(colors[next]));

        float const BlockSize = 100.0f;

        visual.Size(
        {
            BlockSize,
            BlockSize
        });

        visual.Offset(
        {
            point.x - BlockSize / 2.0f,
            point.y - BlockSize / 2.0f,
            0.0f,
        });

        m_visuals.InsertAtTop(visual);

        m_selected = visual;
        m_offset.x = -BlockSize / 2.0f;
        m_offset.y = -BlockSize / 2.0f;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);

    winrt::CoreApplication::Run(winrt::make<App>());
}
```

## <a name="important-apis"></a>API importantes
* [Interface IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Fonction QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [Fonction winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Fonction winrt::put_abi](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Rubriquesassociées
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Transférer vers C++/WinRT à partir de C++/CX](move-to-winrt-from-cx.md)
