---
title: Développer l’exemple de jeu
description: Découvrez comment implémenter une superposition XAML pour un jeu UWP DirectX.
keywords: DirectX, XAML
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7cb1c9f9cf6cbc6cce0c5d4547ed503bb9a06e56
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8941704"
---
# <a name="extend-the-game-sample"></a>Étendre l’exemple de jeu

À ce stade, nous avons décrit les principaux composants d’un jeu de plateforme Windows universelle (UWP) DirectX 3D de base. Vous pouvez configurer l’infrastructure d’un jeu, notamment le fournisseur d’affichage et le pipeline de rendu, et implémenter une boucle de jeu de base. Vous pouvez également créer une superposition de l’interface utilisateur de base, incorporer des sons et implémenter des contrôles. Vous êtes sur le point de créer votre propre jeu, mais consultez les ressources suivantes si vous avez besoin d’aide et d’informations supplémentaires.

-   [Jeux et graphiques DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)
-   [Vue d’ensemble de Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345)
-   [Référence de Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476147)

## <a name="using-xaml-for-the-overlay"></a>Utilisation de XAML pour la superposition


Une autre possibilité que nous n’avons pas détaillée est l’utilisation de XAML à la place de [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) pour la superposition. XAML présente de nombreux avantages par rapport à Direct2D pour le dessin des éléments de l’interface utilisateur. L’avantage le plus important est qu’il facilite l’incorporation de l’apparence Windows 10 dans votre jeu DirectX. Un grand nombre des éléments, styles et comportements communs qui définissent une application du Windows Store sont étroitement intégrés au modèle XAML, ce qui réduit les tâches d’implémentation d’un développeur de jeux. Si la conception de votre propre jeu a une interface utilisateur complexe, envisagez d’utiliser XAML à la place de Direct2D.

Avec XAML, nous pouvons implémenter une interface de jeu qui ressemble à l’interface Direct2D utilisée précédemment.

### <a name="xaml"></a>XAML
![Superposition XAML](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![Superposition D2D](./images/simple-dx-game-extend-d2d.PNG)

Même si les résultats finaux sont similaires, il existe plusieurs différences dans l’implémentation des interfaces Direct2D et XAML.

Fonctionnalité | XAML| Direct2D
:----------|:----------- | :-----------
Définition de la superposition | Définie dans un fichier XAML, `\*.xaml`. Une fois que vous maîtrisez XAML, la création et la configuration de superpositions plus complexes sont simplifiées par rapport à Direct2D.| Définie comme une collection de primitives Direct2D et de chaînes [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) écrites et placées manuellement dans une mémoire tampon cible Direct2D. 
Éléments d’interface utilisateur | Les éléments de l’interface utilisateur XAML proviennent d’éléments standardisés qui font partie des API XAML Windows Runtime, notamment [**Windows::UI::Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) et [**Windows::UI::Xaml::Controls**](https://msdn.microsoft.com/library/windows/apps/br227716). Le code qui gère le comportement des éléments de l’interface utilisateur XAML est défini dans un fichier code-behind, Main.xaml.cpp. | Des formes simples peuvent être tracées comme des rectangles et ellipses.
Redimensionnement de fenêtre | Gère naturellement les événements de redimensionnement et de modification de l’état d’affichage, en transformant la superposition en conséquence | Nécessité de spécifier manuellement comment redessiner les composants de la superposition.


Une autre différence importante concerne la [chaîne d’échange](https://docs.microsoft.com/windows/uwp/graphics-concepts/swap-chains). Vous n’avez pas à attacher la chaîne d’échange à un objet [**Windows::UI::Core::CoreWindow**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow). Au lieu de cela, une application DirectX qui incorpore XAML associe une chaîne d’échange lors de la construction d’un objet [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel). 

L’extrait de code suivant montre comment déclarer XAML pour **SwapChainPanel** dans le fichier [**DirectXPage.xaml**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml).
```xml
<Page
    x:Class="Simple3DGameXaml.DirectXPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:Simple3DGameXaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">


    <SwapChainPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainPanel>
</Page>
```

L’objet **SwapChainPanel** est défini comme propriété [**Content**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.Content) de l’objet fenêtre actuel créé [lors du lancement](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51) par le singleton de l’application.

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```


Pour attacher la chaîne d’échange configurée à l’instance [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) définie par votre code XAML, vous devez obtenir un pointeur vers l’implémentation de l’interface [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/dn302143) native sous-jacente et appeler [**ISwapChainPanelNative::SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144) dessus, en lui transmettant votre chaîne d’échange configurée. 

L’extrait de code suivant provenant de [**DX::DeviceResources::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) le décrit en détail pour l’interopérabilité DirectX/XAML:

```cpp
        ComPtr<IDXGIDevice3> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        // When using XAML interop, the swap chain must be created for composition.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Associate swap chain with SwapChainPanel
        // UI changes will need to be dispatched back to the UI thread
        m_swapChainPanel->Dispatcher->RunAsync(CoreDispatcherPriority::High, ref new DispatchedHandler([=]()
        {
            // Get backing native interface for SwapChainPanel
            ComPtr<ISwapChainPanelNative> panelNative;
            DX::ThrowIfFailed(
                reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(IID_PPV_ARGS(&panelNative))
                );
            DX::ThrowIfFailed(
                panelNative->SetSwapChain(m_swapChain.Get())
                );
        }, CallbackContext::Any));

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }
```

Pour plus d’informations sur ce processus, consultez [Interopérabilité entre DirectX et XAML](directx-and-xaml-interop.md).

## <a name="sample"></a>Exemple

Pour télécharger la version de ce jeu qui utilise XAML pour la superposition, accédez à l’[exemple de jeu de tir Direct3D (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml).


À la différence de la version de l’exemple de jeu abordée dans le reste de ces rubriques, la version XAML définit son infrastructure dans les fichiers [App.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) et [DirectXPage.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp), au lieu de [App.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) et [GameInfoOverlay.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp), respectivement.