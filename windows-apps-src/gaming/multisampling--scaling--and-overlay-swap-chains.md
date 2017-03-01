---
author: mtoepke
title: "Mise à l’échelle et superpositions de chaînes d’échange"
description: "Apprenez à créer des chaînes d’échange mises à l’échelle pour accélérer le rendu sur les appareils mobiles, et utilisez la superposition des chaînes d’échange (quand cela est possible) pour améliorer la qualité visuelle."
ms.assetid: 3e4d2d19-cac3-eebc-52dd-daa7a7bc30d1
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, jeux, mise à l’échelle de chaînes d’échange, superpositions, directx"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 02088fce03c88b4166d49cd36754ac956f254199
ms.lasthandoff: 02/07/2017

---

# <a name="swap-chain-scaling-and-overlays"></a>Mise à l’échelle et superpositions de chaînes d’échange


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Apprenez à créer des chaînes d’échange mises à l’échelle pour accélérer le rendu sur les appareils mobiles, et utilisez la superposition des chaînes d’échange (quand cela est possible) pour améliorer la qualité visuelle.

## <a name="swap-chains-in-directx-112"></a>Chaînes d’échange dans DirectX 11.2


Direct3D 11.2 vous permet de créer des applications de plateforme Windows universelle (UWP) avec des chaînes de permutation mises à l’échelle à partir de résolutions non natives (réduites), ce qui permet d’accélérer les taux de remplissage. Direct3D 11.2 inclut également des API de rendu avec des surcouches vidéo matérielles afin que vous puissiez présenter une interface utilisateur dans une autre chaîne de permutation à une résolution native. Ainsi, votre jeu peut afficher l’interface utilisateur à une résolution native maximale tout en conservant une grande fluidité, ce qui permet de tirer parti des appareils mobiles et des affichages haute résolution (par exemple 3840 x 2160). Cet article explique comment utiliser la superposition des chaînes de permutation.

Direct3D 11.2 introduit également une nouvelle fonctionnalité qui permet de réduire la latence avec les chaînes de permutation d’un modèle de retournement. Voir [Réduire la latence avec des chaînes de permutation DXGI 1.3](reduce-latency-with-dxgi-1-3-swap-chains.md).

## <a name="use-swap-chain-scaling"></a>Utiliser la mise à l’échelle des chaînes de permutation


Quand votre jeu s’exécute sur du matériel de bas niveau ou sur du matériel optimisé pour l’économie d’énergie, il est préférable d’effectuer le rendu du contenu du jeu en temps réel à une résolution inférieure à la résolution native de l’écran. Pour ce faire, la chaîne d’échange qui est utilisée pour le rendu du contenu du jeu doit être inférieure à la résolution native. Sinon, une sous-région de la chaîne d’échange doit être utilisée.

1.  Commencez par créer une chaîne d’échange à la résolution native maximale.

    ```cpp
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

    swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
    swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
    swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
    swapChainDesc.Stereo = false;
    swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
    swapChainDesc.SampleDesc.Quality = 0;
    swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
    swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
    swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All Windows Store apps must use this SwapEffect.
    swapChainDesc.Flags = 0;
    swapChainDesc.Scaling = DXGI_SCALING_STRETCH;

    // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
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

    ComPtr<IDXGISwapChain1> swapChain;
    DX::ThrowIfFailed(
        dxgiFactory->CreateSwapChainForCoreWindow(
            m_d3dDevice.Get(),
            reinterpret_cast<IUnknown*>(m_window.Get()),
            &swapChainDesc,
            nullptr,
            &swapChain
            )
        );

    DX::ThrowIfFailed(
        swapChain.As(&m_swapChain)
        );
    ```

2.  Choisissez ensuite une sous-région de la chaîne de permutation à mettre à l’échelle en affectant une résolution réduite à la taille de la source.

    L’exemple des chaînes de permutation de premier plan DX calcule une taille réduite en fonction d’un pourcentage :

    ```cpp
    m_d3dRenderSizePercentage = percentage;

    UINT renderWidth = static_cast<UINT>(m_d3dRenderTargetSize.Width * percentage + 0.5f);
    UINT renderHeight = static_cast<UINT>(m_d3dRenderTargetSize.Height * percentage + 0.5f);

    // Change the region of the swap chain that will be presented to the screen.
    DX::ThrowIfFailed(
        m_swapChain->SetSourceSize(
            renderWidth,
            renderHeight
            )
        );
    ```

3.  Créez une fenêtre d’affichage qui correspond à la sous-région de la chaîne de permutation.

    ```cpp
    // In Direct3D, change the Viewport to match the region of the swap
    // chain that will now be presented from.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        static_cast<float>(renderWidth),
        static_cast<float>(renderHeight)
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

4.  Si Direct2D est utilisé, la transformation de la rotation doit être modifiée pour compenser la région source.

## <a name="create-a-hardware-overlay-swap-chain-for-ui-elements"></a>Créer une chaîne de permutation de surcouche vidéo matérielle pour les éléments d’interface utilisateur


Quand vous effectuez une mise à l’échelle d’une chaîne de permutation, tenez compte de l’inconvénient suivant : la mise à l’échelle de l’interface utilisateur est réduite également, ce qui peut la rendre floue et difficile à utiliser. Sur les appareils qui prennent en charge les chaînes de permutation de surcouche vidéo matérielle, ce problème peut être résolu. Pour ce faire, il suffit d’effectuer le rendu de l’interface utilisateur à la résolution native dans une chaîne de permutation distincte du contenu de jeu en temps réel. Notez que cette technique ne s’applique qu’aux chaînes de permutation [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Elle ne peut pas être utilisée avec l’interopérabilité XAML.

Procédez comme suit pour créer une chaîne de permutation de premier plan qui utilise la fonctionnalité de surcouche vidéo matérielle. Ces étapes doivent être effectuées après la création de la chaîne de permutation pour le contenu de jeu en temps réel, comme décrit ci-dessus.

1.  Tout d’abord, déterminez si l’adaptateur DXGI prend en charge les permutations. Récupérez l’adaptateur de sortie DXGI à partir de la chaîne de permutation :

    ```cpp
    ComPtr<IDXGIAdapter> outputDxgiAdapter;
    DX::ThrowIfFailed(
        dxgiFactory->EnumAdapters(0, &outputDxgiAdapter)
        );

    ComPtr<IDXGIOutput> dxgiOutput;
    DX::ThrowIfFailed(
        outputDxgiAdapter->EnumOutputs(0, &dxgiOutput)
        );

    ComPtr<IDXGIOutput2> dxgiOutput2;
    DX::ThrowIfFailed(
        dxgiOutput.As(&dxgiOutput2)
        );
    ```

    L’adaptateur DXGI prend en charge les superpositions si l’adaptateur de sortie retourne True pour [**SupportsOverlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411).

    ```cpp
    m_overlaySupportExists = dxgiOutput2->SupportsOverlays() ? true : false;
    ```
    
    > **Remarque**   Si l’adaptateur DXGI prend en charge les superpositions, passez à l’étape suivante. Si l’appareil ne prend pas en charge les superpositions, le rendu avec plusieurs chaînes d’échange ne sera pas efficace. À la place, effectuez un rendu de l’interface utilisateur à une résolution réduite dans la même chaîne de permutation que le contenu de jeu en temps réel.

     

2.  Créez la chaîne d’échange de premier plan avec [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559). Les options suivantes doivent être définies dans le [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528) fourni au paramètre *pDesc* :

    -   Utilisez l’indicateur de chaîne d’échange [**DXGI\_SWAP\_CHAIN\_FLAG\_FOREGROUND\_LAYER**](https://msdn.microsoft.com/library/windows/desktop/bb173076) pour spécifier qu’il s’agit d’une chaîne d’échange de premier plan.
    -   Utilisez l’indicateur de mode alpha [**DXGI\_ALPHA\_MODE\_PREMULTIPLIED**](https://msdn.microsoft.com/library/windows/desktop/hh404496). Les chaînes d’échange de premier plan sont toujours prémultipliées.
    -   Définissez l’indicateur [**DXGI\_SCALING\_NONE**](https://msdn.microsoft.com/library/windows/desktop/hh404526). Les chaînes d’échange de premier plan s’exécutent toujours à la résolution native.

    ```cpp
     foregroundSwapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER;
     foregroundSwapChainDesc.Scaling = DXGI_SCALING_NONE;
     foregroundSwapChainDesc.AlphaMode = DXGI_ALPHA_MODE_PREMULTIPLIED; // Foreground swap chain alpha values must be premultiplied.
    ```

    > **Remarque**   Redéfinissez [**DXGI\_SWAP\_CHAIN\_FLAG\_FOREGROUND\_LAYER**](https://msdn.microsoft.com/library/windows/desktop/bb173076) chaque fois que la chaîne d’échange est redimensionnée.

    ```cpp
    HRESULT hr = m_foregroundSwapChain->ResizeBuffers(
        2, // Double-buffered swap chain.
        static_cast<UINT>(m_d3dRenderTargetSize.Width),
        static_cast<UINT>(m_d3dRenderTargetSize.Height),
        DXGI_FORMAT_B8G8R8A8_UNORM,
        DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER // The FOREGROUND_LAYER flag cannot be removed with ResizeBuffers.
        );
    ```

3.  Quand deux chaînes d’échange sont utilisées, augmentez la latence d’image maximale à 2 afin que l’adaptateur DXGI ait le temps de présenter les deux chaînes d’échange en même temps (dans le même intervalle VSync).

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

4.  Les chaînes de permutation de premier plan utilisent toujours un alpha prémultiplié. Les valeurs de couleur de chaque pixel sont censées être déjà multipliées par la valeur alpha avant la présentation de l’image. Par exemple, un pixel BVRA (bleu/vert/rouge/alpha) 100 % blanc, ayant 50 % d’alpha, a la valeur (0.5, 0.5, 0.5, 0.5).

    L’étape de prémultiplication alpha peut être effectuée à l’étape de fusion/sortie en appliquant un état de fusion d’application (voir [**ID3D11BlendState**](https://msdn.microsoft.com/library/windows/desktop/ff476349)) avec le champ **SrcBlend** de la structure [**D3D11\_RENDER\_TARGET\_BLEND\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476200) défini à **D3D11\_SRC\_ALPHA**. Il est également possible d’utiliser des valeurs alpha prémultipliées.

    Si l’étape de prémultiplication alpha n’est pas effectuée, les couleurs de la chaîne de permutation de premier plan seront plus brillantes que prévu.

5.  Selon que la chaîne de permutation de premier plan a été créée ou non, la surface de dessin Direct2D des éléments de l’interface utilisateur doit éventuellement être associée à la chaîne de permutation de premier plan.

    Création d’affichages de cibles de rendu :

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

    Création de la surface de dessin Direct2D :

    ```cpp
    if (m_foregroundSwapChain)
    {
        // Create a Direct2D target bitmap for the foreground swap chain.
        ComPtr<IDXGISurface2> dxgiForegroundSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiForegroundSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiForegroundSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }
    else
    {
        // Create a Direct2D target bitmap for the swap chain.
        ComPtr<IDXGISurface2> dxgiSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
    ```

    ```cpp
    // Create a render target view of the swap chain's back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );
    ```

6.  Présentez la chaîne de permutation de premier plan avec la chaîne de permutation mise à l’échelle utilisée pour le contenu de jeu en temps réel. Comme la latence d’image a la valeur 2 pour les deux chaînes d’échange, DXGI peut les présenter dans le même intervalle VSync.

    ```cpp
    // Present the contents of the swap chain to the screen.
    void DX::DeviceResources::Present()
    {
        // The first argument instructs DXGI to block until VSync, putting the application
        // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
        // frames that will never be displayed to the screen.
        HRESULT hr = m_swapChain->Present(1, 0);

        if (SUCCEEDED(hr) && m_foregroundSwapChain)
        {
            m_foregroundSwapChain->Present(1, 0);
        }

        // Discard the contents of the render targets.
        // This is a valid operation only when the existing contents will be entirely
        // overwritten. If dirty or scroll rects are used, this call should be removed.
        m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());
        if (m_foregroundSwapChain)
        {
            m_d3dContext->DiscardView(m_d3dForegroundRenderTargetView.Get());
        }

        // Discard the contents of the depth stencil.
        m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

        // If the device was removed either by a disconnection or a driver upgrade, we
        // must recreate all device resources.
        if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            HandleDeviceLost();
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    ```

 

 





