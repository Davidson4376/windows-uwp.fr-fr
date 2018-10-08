---
author: stevewhims
description: Cette rubrique montre deux fonctions d’assistance qui peuvent être utilisées pour effectuer des conversions entre des objets C++/CX et C++/WinRT.
title: Interopérabilité entre C++/WinRT et C++/CX
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, port, migrer, interopérabilité, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: b60b0d7c201f172261de1546fc250e40b8cd670f
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4420165"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interopérabilité entre C++/WinRT et C++/CX
Cette rubrique montre deux fonctions d’assistance qui peuvent être utilisées pour effectuer une conversion entre [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) et [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) objets. Vous pouvez les utiliser pour permettre l’interopérabilité entre le code qui utilise les deux projections de langage, ou vous pouvez utiliser les fonctions comme vous transférez votre code de C++ / CX vers C++ / WinRT (voir [déplacer vers C++ / WinRT de C++ / CX](move-to-winrt-from-cx.md)).

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

## <a name="code-example"></a>Exemple de code
Voici un exemple de code (basé sur le modèle de projet **Blank App** C++/CX) montrant les deux fonctions d’assistance en cours d’utilisation. Il montre également comment vous pouvez utiliser des alias d’espace de noms pour les différents îlots pour éviter les collisions potentielles d’espace de noms entre la projection C++/WinRT et la projection C++/CX.

```cppwinrt
// MainPage.xaml.cpp

#include "pch.h"
#include "MainPage.xaml.h"
#include <winrt/Windows.Foundation.h>
#include <sstream>

using namespace InteropExample;

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows::Foundation;
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

MainPage::MainPage()
{
    InitializeComponent();

    winrt::init_apartment(winrt::apartment_type::single_threaded);

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
}
```

## <a name="important-apis"></a>API importantes
* [Interface IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Fonction QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [Fonction winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Fonction winrt::put_abi](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Rubriquesassociées
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
