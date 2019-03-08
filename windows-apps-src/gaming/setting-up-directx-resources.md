---
title: Configurer des ressources DirectX et afficher une image
description: Voici comment créer un périphérique Direct3D, une chaîne d’échange et un affichage de cible de rendu, et comment présenter l’image rendue à l’écran.
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, directx, ressources, images
ms.localizationpriority: medium
ms.openlocfilehash: b650f77627e02427b2861a2e6d0df7d1fc86831a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628364"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>Configurer des ressources DirectX et afficher une image



Voici comment créer un périphérique Direct3D, une chaîne d’échange et un affichage de cible de rendu, et comment présenter l’image rendue à l’écran.

**Objectif :** Pour configurer des ressources DirectX dans une application C++ Universal Windows Platform (UWP) et pour afficher une couleur unie.

## <a name="prerequisites"></a>Conditions préalables


Nous partons du principe que vous êtes familiarisé avec C++. Vous avez également besoin d’une expérience de base dans les concepts de programmation graphique.

**Durée :** 20 minutes.

## <a name="instructions"></a>Instructions

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. Déclarer des variables de l’interface Direct3D avec ComPtr

Nous déclarons les variables d’interface Direct3D à l’aide du modèle de [pointeur intelligent](https://msdn.microsoft.com/library/windows/apps/hh279674.aspx) ComPtr issu de la bibliothèque de modèles C++ Windows Runtime (WRL) afin de pouvoir gérer la durée de vie de ces variables en parant à toute exception. Nous pouvons ensuite utiliser ces variables pour accéder à la classe [**ComPtr class**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) et à ses membres. Exemple :

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

Si vous déclarez [ **ID3D11RenderTargetView** ](https://msdn.microsoft.com/library/windows/desktop/ff476582) avec ComPtr, vous pouvez ensuite utiliser de ComPtr **GetAddressOf** méthode pour obtenir l’adresse du pointeur pour  **ID3D11RenderTargetView** (\*\*ID3D11RenderTargetView) à passer à [ **ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464). **OMSetRenderTargets** établit la liaison entre la cible de rendu et l’[étape de fusion/sortie](https://msdn.microsoft.com/library/windows/desktop/bb205120) pour indiquer la cible de rendu comme cible de sortie.

Après le démarrage de l’exemple d’application, celui-ci s’initialise et se charge, puis est ensuite prêt à s’exécuter.

### <a name="2-creating-the-direct3d-device"></a>2. Création du périphérique Direct3D

Pour utiliser l’API Direct3D pour effectuer le rendu d’une scène, nous devons créer tout d’abord un périphérique Direct3D qui représente la carte graphique. Pour créer le périphérique Direct3D, nous devons appeler la fonction [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Nous spécifions 9.1 via 11.1 dans le tableau de niveaux de [ **D3D\_fonctionnalité\_niveau** ](https://msdn.microsoft.com/library/windows/desktop/ff476329) valeurs. Direct3D parcourt le tableau dans l’ordre et retourne le niveau de fonctionnalité le plus élevé pris en charge. Par conséquent, pour obtenir le niveau de fonctionnalité le plus élevé disponible, nous répertorions les **D3D\_fonctionnalité\_niveau** tableau entrées du plus élevé au plus bas. Nous transmettons le [ **D3D11\_créer\_appareil\_BGRA\_prise en charge** ](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_BGRA_SUPPORT) indicateur pour le *indicateurs* paramètre afin de vous Ressources de Direct3D interagissent avec Direct2D. Si nous utilisons la version debug, nous passons également le [ **D3D11\_créer\_appareil\_déboguer** ](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_DEBUG) indicateur. Pour plus d’informations sur le débogage d’applications, voir [Utilisation de la couche de débogage pour déboguer des applications](https://msdn.microsoft.com/library/windows/desktop/jj200584).

Nous obtenons le périphérique Direct3D 11.1 ([**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)) et le contexte du périphérique ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)) en interrogeant le périphérique et son contexte retournés de [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082).

```cpp
        // First, create the Direct3D device.

        // This flag is required in order to enable compatibility with Direct2D.
        UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
        // If the project is in a debug build, enable debugging via SDK Layers with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

        // This array defines the ordering of feature levels that D3D should attempt to create.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_1
        };

        ComPtr<ID3D11Device> d3dDevice;
        ComPtr<ID3D11DeviceContext> d3dDeviceContext;
        DX::ThrowIfFailed(
            D3D11CreateDevice(
                nullptr,                    // Specify nullptr to use the default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                nullptr,                    // leave as nullptr if hardware is used
                creationFlags,              // optionally set debug and Direct2D compatibility flags
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,          // always set this to D3D11_SDK_VERSION
                &d3dDevice,
                nullptr,
                &d3dDeviceContext
                )
            );

        // Retrieve the Direct3D 11.1 interfaces.
        DX::ThrowIfFailed(
            d3dDevice.As(&m_d3dDevice)
            );

        DX::ThrowIfFailed(
            d3dDeviceContext.As(&m_d3dDeviceContext)
            );
```

### <a name="3-creating-the-swap-chain"></a>3. Création de la chaîne de permutation

Nous créons ensuite une chaîne d’échange que le périphérique utilise pour le rendu et l’affichage. Nous déclarer et initialiser un [ **DXGI\_échange\_chaîne\_DESC1** ](https://msdn.microsoft.com/library/windows/desktop/hh404528) structure pour décrire la chaîne de permutation. Ensuite, nous configurons la chaîne de permutation en tant que modèle de retournement (autrement dit, une chaîne de permutation a la [ **DXGI\_échange\_effet\_FLIP\_séquentiel** ](https://msdn.microsoft.com/library/windows/desktop/bb173077#DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL) valeur définie dans le **SwapEffect** membre) et définissez le **Format** membre à [ **DXGI\_FORMAT\_B8G8R8A8\_UNORM** ](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM). Nous définissons le **nombre** membre de la [ **DXGI\_exemple\_DESC** ](https://msdn.microsoft.com/library/windows/desktop/bb173072) structure qui le **SampleDesc** membre Spécifie 1 et le **qualité** membre **DXGI\_exemple\_DESC** à zéro, car le modèle de retournement ne prend pas en charge plusieurs exemples de l’anticrénelage (MSAA). Nous attribuons au membre **BufferCount** la valeur 2 afin que la chaîne d’échange puisse utiliser un tampon d’affichage à présenter au périphérique d’affichage et une mémoire tampon d’arrière-plan qui sert de cible de rendu.

Nous obtenons le périphérique DXGI sous-jacent en interrogeant le périphérique Direct3D 11.1. Pour réduire le plus possible la consommation d’énergie, ce qui est important sur les périphériques à batterie tels que les ordinateurs portables et les tablettes, nous appelons la méthode [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334) avec la valeur 1 comme nombre maximal d’images de mémoire tampon d’arrière-plan que l’infrastructure DXGI peut placer en file d’attente. Cela permet de s’assurer que le rendu de l’application ne se fait qu’après le vide vertical.

Pour créer enfin la chaîne d’échange, nous devons obtenir la fabrique parente du périphérique DXGI. Nous appelons [**IDXGIDevice::GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) pour obtenir la carte du périphérique, puis [**IDXGIObject::GetParent**](https://msdn.microsoft.com/library/windows/desktop/bb174542) sur la carte pour obtenir la fabrique parente ([**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556)). Pour créer une chaîne d’échange, nous appelons [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) avec le descripteur de la chaîne d’échange et la fenêtre principale de l’application.

```cpp
            // If the swap chain does not exist, create it.
            DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

            swapChainDesc.Stereo = false;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.Scaling = DXGI_SCALING_NONE;
            swapChainDesc.Flags = 0;

            // Use automatic sizing.
            swapChainDesc.Width = 0;
            swapChainDesc.Height = 0;

            // This is the most common swap chain format.
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;

            // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Count = 1;
            swapChainDesc.SampleDesc.Quality = 0;

            // Use two buffers to enable the flip effect.
            swapChainDesc.BufferCount = 2;

            // We recommend using this swap effect for all applications.
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;


            // Once the swap chain description is configured, it must be
            // created on the same adapter as the existing D3D Device.

            // First, retrieve the underlying DXGI Device from the D3D Device.
            ComPtr<IDXGIDevice2> dxgiDevice;
            DX::ThrowIfFailed(
                m_d3dDevice.As(&dxgiDevice)
                );

            // Ensure that DXGI does not queue more than one frame at a time. This both reduces
            // latency and ensures that the application will only render after each VSync, minimizing
            // power consumption.
            DX::ThrowIfFailed(
                dxgiDevice->SetMaximumFrameLatency(1)
                );

            // Next, get the parent factory from the DXGI Device.
            ComPtr<IDXGIAdapter> dxgiAdapter;
            DX::ThrowIfFailed(
                dxgiDevice->GetAdapter(&dxgiAdapter)
                );

            ComPtr<IDXGIFactory2> dxgiFactory;
            DX::ThrowIfFailed(
                dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
                );

            // Finally, create the swap chain.
            CoreWindow^ window = m_window.Get();
            DX::ThrowIfFailed(
                dxgiFactory->CreateSwapChainForCoreWindow(
                    m_d3dDevice.Get(),
                    reinterpret_cast<IUnknown*>(window),
                    &swapChainDesc,
                    nullptr, // Allow on all displays.
                    &m_swapChain
                    )
                );
```

### <a name="4-creating-the-render-target-view"></a>4. Création de la vue de la cible de rendu

Pour effectuer le rendu de graphismes dans la fenêtre, nous devons créer un affichage de cible de rendu. Nous appelons [**IDXGISwapChain::GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb174570) pour obtenir la mémoire tampon d’arrière-plan de la chaîne d’échange à utiliser lorsque nous créons l’affichage de cible de rendu. Nous spécifions la mémoire tampon d’arrière-plan en tant que texture 2D ([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)). Pour créer l’affichage de cible de rendu, nous appelons [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517) avec la mémoire tampon d’arrière-plan de la chaîne d’échange. Nous devons spécifier pour dessiner dans la fenêtre entière core en spécifiant le port de vue ([**D3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/ff476260)) en tant que la taille totale de mémoire tampon d’arrière-plan de la chaîne de permutation. Nous utilisons la fenêtre d’affichage dans un appel à [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) pour lier la fenêtre d’affichage à l’[étape du module de rastérisation](https://msdn.microsoft.com/library/windows/desktop/bb205125) du pipeline. L’étape du module de rastérisation convertit les informations sur les vecteurs en une image raster. Dans ce cas, nous n’avons pas besoin de conversion, car nous cherchons simplement à afficher une couleur unie.

```cpp
        // Once the swap chain is created, create a render target view.  This will
        // allow Direct3D to render graphics to the window.

        ComPtr<ID3D11Texture2D> backBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                backBuffer.Get(),
                nullptr,
                &m_renderTargetView
                )
            );


        // After the render target view is created, specify that the viewport,
        // which describes what portion of the window to draw to, should cover
        // the entire window.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_VIEWPORT viewport;
        viewport.TopLeftX = 0.0f;
        viewport.TopLeftY = 0.0f;
        viewport.Width = static_cast<float>(backBufferDesc.Width);
        viewport.Height = static_cast<float>(backBufferDesc.Height);
        viewport.MinDepth = D3D11_MIN_DEPTH;
        viewport.MaxDepth = D3D11_MAX_DEPTH;

        m_d3dDeviceContext->RSSetViewports(1, &viewport);
```

### <a name="5-presenting-the-rendered-image"></a>5. Présentation de l’image rendue

L’exécution du code entre dans une boucle sans fin pour effectuer le rendu de façon continue et afficher la scène.

Dans cette boucle, nous appelons :

1.  [**ID3D11DeviceContext::OMSetRenderTargets** ](https://msdn.microsoft.com/library/windows/desktop/ff476464) pour spécifier la cible de rendu en tant que la cible de sortie.
2.  [**ID3D11DeviceContext::ClearRenderTargetView** ](https://msdn.microsoft.com/library/windows/desktop/ff476388) pour effacer la cible de rendu pour une couleur unie.
3.  [**IDXGISwapChain::Present** ](https://msdn.microsoft.com/library/windows/desktop/bb174576) pour présenter l’image rendue dans la fenêtre.

Étant donné que nous avons préalablement défini une latence d’image maximale de 1, Windows ralentit habituellement la boucle de rendu pour la caler à la fréquence de rafraîchissement de l’écran, généralement autour de 60 Hz. Windows ralentit la boucle de rendu en mettant l’application en veille lorsqu’elle appelle [**Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576). Windows met l’application en veille jusqu’à ce que l’écran soit rafraîchi.

```cpp
        // Enter the render loop.  Note that UWP apps should never exit.
        while (true)
        {
            // Process events incoming to the window.
            m_window->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
        }
```

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. Redimensionnement de la fenêtre d’application et de la mémoire tampon de la chaîne de permutation

Si la taille de la fenêtre de l’application change, l’application doit redimensionner les tampons de la chaîne d’échange, recréer l’affichage de cible de rendu, puis présenter l’image rendue redimensionnée. Pour redimensionner les tampons de la chaîne d’échange, nous appelons [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577). Dans cet appel, nous laissons le nombre de tampons et le format des mémoires tampons inchangé (la *BufferCount* paramètre à deux et le *NewFormat* paramètre [ **DXGI\_ Mettre en forme\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM)). Nous calquons la taille de la mémoire tampon d’arrière-plan de la chaîne d’échange sur la taille de la fenêtre redimensionnée. Après le redimensionnement des tampons, nous devons créer la nouvelle cible de rendu et présenter la nouvelle image rendue de façon similaire à l’initialisation de l’application.

```cpp
            // If the swap chain already exists, resize it.
            DX::ThrowIfFailed(
                m_swapChain->ResizeBuffers(
                    2,
                    0,
                    0,
                    DXGI_FORMAT_B8G8R8A8_UNORM,
                    0
                    )
                );
```

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes


Nous avons créé un périphérique Direct3D, une chaîne d’échange et un affichage de cible de rendu, puis avons présenté l’image rendue à l’écran.

À présent, nous devons également tracer un triangle à l’écran.

[Création des nuanceurs et primitives de dessin](creating-shaders-and-drawing-primitives.md)

 

 




