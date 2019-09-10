---
description: Cette rubrique montre comment porter du code WRL vers son équivalent en C++/WinRT.
title: Passer de WRL à C++/WinRT
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, porter, migrer, WRL
ms.localizationpriority: medium
ms.openlocfilehash: 7b313e80b744279f8dc955e8c07d31aba2860c3f
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393433"
---
# <a name="move-to-cwinrt-from-wrl"></a>Passer de WRL à C++/WinRT
Cette rubrique montre comment porter du code de la [bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) vers son équivalent en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

La première étape du portage vers C++/WinRT consiste à ajouter manuellement la prise en charge de C++/WinRT à votre projet (voir [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Pour cela, installez le [package NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) dans votre projet. Ouvrez le projet dans Visual Studio, cliquez sur **Projet** \> **Gérer les packages NuGet...** \> **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de la recherche, puis cliquez sur **Installer** pour installer le package correspondant à ce projet. Un effet de ce changement est la désactivation de la prise en charge de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) dans le projet. Si vous utilisez C++/CX dans le projet, vous pouvez laisser la prise en charge désactivée et mettre également à jour votre code C++/CX en C++/WinRT (voir [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md)). Vous pouvez également réactiver la prise en charge (dans les propriétés du projet, **C/C++** \> **Général** \> **Consommer l’extension Windows Runtime**\> **Oui (/ZW)** ) et vous concentrer d’abord sur le portage de votre code WRL. Le code C++/CX et C++/WinRT peut coexister dans le même projet, à l’exception de la prise en charge du compilateur XAML et les composants Windows Runtime (consultez [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md)).

Définissez la propriété de projet **Général** \> **Version de la plateforme cible** sur 10.0.17134.0 (Windows 10, version 1803) ou une version supérieure.

Dans votre fichier d’en-tête précompilé (généralement `pch.h`), incluez `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Si vous incluez des en-têtes d’API Windows projetées C++/WinRT (par exemple, `winrt/Windows.Foundation.h`), vous n’avez pas besoin d’inclure explicitement `winrt/base.h` ainsi, car il le sera automatiquement.

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptrcppwindowscomptr-class"></a>Portage des pointeurs intelligents COM WRL ([Microsoft::WRL::ComPtr](/cpp/windows/comptr-class))
Portez tout code utilisant **Microsoft::WRL::ComPtr\<T\>** pour utiliser [**winrt::com_ptr\<T\>** ](/uwp/cpp-ref-for-winrt/com-ptr). Voici un exemple de code avant et après. Dans la version *après*, la fonction membre [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) récupère le pointeur brut sous-jacent afin qu’il puisse être défini.

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> Si vous avez un [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) déjà positionné (son pointeur brut interne a déjà une cible) et que vous souhaitez le repositionner pour qu’il pointe vers un autre objet, vous devez d’abord lui affecter `nullptr`, comme indiqué dans l’exemple de code ci-dessous. Sinon, un **com_ptr** déjà positionné va vous signaler le problème (quand vous appellerez [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) ou [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)) en indiquant que son pointeur interne n’a pas une valeur null.

```cppwinrt
winrt::com_ptr<IDXGISwapChain1> m_pDXGISwapChain1;
...
// We execute the code below each time the window size changes.
m_pDXGISwapChain1 = nullptr; // Important because we're about to re-seat 
winrt::check_hresult(
    m_pDxgiFactory->CreateSwapChainForHwnd(
        m_pCommandQueue.get(), // For Direct3D 12, this is a pointer to a direct command queue, and not to the device.
        m_hWnd,
        &swapChainDesc,
        nullptr,
        nullptr,
        m_pDXGISwapChain1.put())
);
```

Dans l’exemple suivant (dans la version *après*), la fonction membre [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) récupère le pointeur brut sous-jacent sous la forme d’un pointeur vers void.

```cpp
ComPtr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(IID_PPV_ARGS(&debugController))))
{
    debugController->EnableDebugLayer();
}
```

```cppwinrt
winrt::com_ptr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(__uuidof(debugController), debugController.put_void())))
{
    debugController->EnableDebugLayer();
}
```

Remplacez **ComPtr::Get** par [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function).

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

Quand vous voulez passer le pointeur brut sous-jacent à une fonction qui attend un pointeur sur **IUnknown**, utilisez la fonction libre [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#get_unknown-function), comme illustré dans l’exemple suivant.

```cpp
ComPtr<IDXGISwapChain1> swapChain;
DX::ThrowIfFailed(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &swapChain
    )
);
```

```cppwinrt
winrt::agile_ref<winrt::Windows::UI::Core::CoreWindow> m_window; 
winrt::com_ptr<IDXGISwapChain1> swapChain;
winrt::check_hresult(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.get(),
        winrt::get_unknown(m_window.get()),
        &swapChainDesc,
        nullptr,
        swapChain.put()
    )
);
```

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>Portage d’un module WRL (Microsoft::WRL::Module)
Vous pouvez ajouter progressivement du code C++/WinRT à un projet existant qui utilise WRL pour implémenter un composant, et vos classes WRL existantes continuent à être prises en charge. Cette section montre comment.

Si vous créez un type de projet **Composant Windows Runtime (C++/WinRT)** dans Visual Studio et que vous effectuez une génération, le fichier `Generated Files\module.g.cpp` est automatiquement généré. Ce fichier contient les définitions de deux fonctions C++/WinRT utiles (indiquées ci-dessous), que vous pouvez copier et ajouter à votre projet. Ces fonctions sont **WINRT_CanUnloadNow** et **WINRT_GetActivationFactory**. Comme vous pouvez le constater, elles appellent WRL de manière conditionnelle pour vous assister, quelle que soit la phase de portage où vous vous trouvez.

```cppwinrt
HRESULT WINRT_CALL WINRT_CanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT WINRT_CALL WINRT_GetActivationFactory(HSTRING classId, void** factory)
{
    try
    {
        *factory = nullptr;
        wchar_t const* const name = WINRT_WindowsGetStringRawBuffer(classId, nullptr);

        if (0 == wcscmp(name, L"MoveFromWRLTest.Class"))
        {
            *factory = winrt::detach_abi(winrt::make<winrt::MoveFromWRLTest::factory_implementation::Class>());
            return S_OK;
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetActivationFactory(classId, reinterpret_cast<::IActivationFactory**>(factory));
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...) { return winrt::to_hresult(); }
}
```

Une fois que vous avez ces fonctions dans votre projet, au lieu d’appeler [**Module::GetActivationFactory**](/cpp/windows/module-getactivationfactory-method) directement, appelez **WINRT_GetActivationFactory** (qui appelle la fonction WRL en interne). Voici un exemple de code avant et après.

```cpp
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    auto & module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    return module.GetActivationFactory(activatableClassId, factory);
}
```

```cppwinrt
HRESULT __stdcall WINRT_GetActivationFactory(HSTRING activatableClassId, void** factory);
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    return WINRT_GetActivationFactory(activatableClassId, reinterpret_cast<void**>(factory));
}
```

Au lieu d’appeler [**Module::Terminate**](/cpp/windows/module-terminate-method) directement, appelez **WINRT_CanUnloadNow** (qui appelle la fonction WRL en interne). Voici un exemple de code avant et après.

```cpp
HRESULT __stdcall DllCanUnloadNow(void)
{
    auto &module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    HRESULT hr = (module.Terminate() ? S_OK : S_FALSE);
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

```cppwinrt
HRESULT __stdcall WINRT_CanUnloadNow();
HRESULT __stdcall DllCanUnloadNow(void)
{
    HRESULT hr = WINRT_CanUnloadNow();
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

## <a name="important-apis"></a>API importantes
* [Modèle de struct winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Struct winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Rubriques connexes
* [Présentation de C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md)
* [Bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
