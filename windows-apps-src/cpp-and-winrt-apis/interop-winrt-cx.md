---
description: Cette rubrique montre deux fonctions d’assistance qui peuvent être utilisées pour effectuer des conversions entre des objets C++/CX et C++/WinRT.
title: Interopérabilité entre C++/WinRT et C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, port, migrer, interopérabilité, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 558f3fa75e7dd599927a9d2ace256bf1feb98e77
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621644"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interopérabilité entre C++/WinRT et C++/CX

Stratégies pour progressivement le portage du code dans votre [C++ / c++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) projet à [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) sont abordées dans [déplacer vers C + c++ / WinRT à partir de C++ / c++ / CX](move-to-winrt-from-cx.md).

Cette rubrique montre deux fonctions d’assistance que vous pouvez utiliser pour effectuer une conversion entre C++ / c++ / CX et c++ / WinRT des objets dans le même projet. Vous pouvez les utiliser pour assurer l’interopérabilité entre le code qui utilise les projections de deux langage, ou vous pouvez utiliser les fonctions que le port de votre code à partir de C++ / c++ / CX pour C++ / c++ / WinRT.

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

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>Exemple de projet affichant les deux fonctions d’assistance en cours d’utilisation

Reproduire, de manière simple, le scénario de progressivement le portage du code en C / c++ / projet CX à C++ / c++ / WinRT, vous pouvez commencer en créant un nouveau projet dans Visual Studio à l’aide d’une des C + c++ / WinRT les modèles de projet (consultez [prise en charge de Visual Studio pour C++ / c++ / WinRT ](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)).

Cet exemple de projet illustre également comment vous pouvez utiliser des alias d’espace de noms pour les différents îlots de code, afin de traiter sinon éventuelles collisions d’espace de noms entre le C++ / c++ / WinRT projection et le C / c++ / projection de CX.

- Créer un **Visual C++** \> **Windows universel** > **application Core (C++ / c++ / WinRT)** projet.
- Dans Propriétés du projet, **C/C++** \> **général** \> **consommer l’Extension Windows Runtime** \> **Oui (/ZW)** . Cela active la prise en charge de projet pour C / c++ / CX.
- Remplacez le contenu de `App.cpp` avec la liste de codes ci-dessous.

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
* [QueryInterface (fonction)](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [WinRT::get_abi (fonction)](/uwp/cpp-ref-for-winrt/get-abi)
* [WinRT::put_abi (fonction)](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Rubriques connexes
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md)
