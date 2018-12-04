---
title: Échantillonnage multiple des applications de plateforme Windows universelle (UWP)
description: Découvrez comment utiliser l’échantillonnage multiple dans des applications de plateforme Windows universelle (UWP) générées avec Direct3D.
ms.assetid: 1cd482b8-32ff-1eb0-4c91-83eb52f08484
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, échantillonnage multiple, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 0c1634af8589a97f5070ff85909fe12ab16bf8d6
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8476884"
---
# <a name="span-iddevgamingmultisamplingmulti-sampleantialiasinginwindowsstoreappsspan-multisampling-in-universal-windows-platform-uwp-apps"></a><span id="dev_gaming.multisampling__multi-sample_anti_aliasing__in_windows_store_apps"></span> Échantillonnage multiple des applications de plateforme Windows universelle (UWP)



Découvrez comment utiliser l’échantillonnage multiple dans des applications de plateforme Windows universelle (UWP) générées avec Direct3D. L’échantillonnage multiple, également appelé anticrénelage multi-échantillon, est une technique graphique utilisée pour réduire l’apparence des bords crénelés. Elle consiste à dessiner plus de pixels que leur nombre réel dans la cible de rendu final, puis à effectuer une moyenne des valeurs afin de conserver l’apparence d’un bord « partiel » dans certains pixels. Pour une description détaillée du fonctionnement réel de l’échantillonnage multiple dans Direct3D, voir [Règles de rastérisation de l’anticrénelage multi-échantillon](https://msdn.microsoft.com/library/windows/desktop/cc627092#Multisample).

## <a name="multisampling-and-the-flip-model-swap-chain"></a>Échantillonnage multiple et chaîne d’échange de modèle de retournement


Les applications UWP qui utilisent DirectX doivent utiliser des chaînes d’échange de modèle de retournement. Les chaînes d’échange de modèle de retournement ne prennent pas en charge directement l’échantillonnage multiple. Toutefois, l’échantillonnage multiple peut être appliqué de manière différente en affichant la scène vers une vue de la cible de rendu échantillonnée plusieurs fois, puis en résolvant la cible de rendu échantillonnée plusieurs fois dans la mémoire tampon d’arrière-plan avant sa présentation. Cet article décrit les étapes nécessaires à l’ajout de l’échantillonnage multiple à vos applications UWP.

### <a name="how-to-use-multisampling"></a>Comment utiliser l’échantillonnage multiple

Les niveaux de fonctionnalités Direct3D garantissent la prise en charge des possibilités minimales et spécifiques de dénombrement d’échantillons. En outre, ils garantissent que certains formats de mémoire tampon seront disponibles pour la prise en charge de l’échantillonnage multiple. Les périphériques graphiques prennent souvent en charge un éventail plus large de formats et de nombres d’échantillons que le minimum requis. La prise en charge de l’échantillonnage multiple peut être déterminée au moment de l’exécution en vérifiant la prise en charge des fonctionnalités d’échantillonnage multiple avec des formats DXGI spécifiques, puis en vérifiant les nombres d’échantillons utilisables avec chaque format pris en charge.

1.  Appelez [**ID3D11Device::CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497) pour connaître les formats DXGI utilisables avec l’échantillonnage multiple. Fournissez les formats de cibles de rendu utilisables par votre jeu. Comme la cible de rendu et la cible de résolution doivent utiliser le même format, vérifiez [**D3D11\_FORMAT\_SUPPORT\_MULTISAMPLE\_RENDERTARGET**](https://msdn.microsoft.com/library/windows/desktop/ff476134) et **D3D11\_FORMAT\_SUPPORT\_MULTISAMPLE\_RESOLVE**.

    **Niveau de fonctionnalité 9:** Bien que le niveau de fonctionnalité 9 appareils [garantissent la prise en charge des formats des cibles de rendu échantillonnée plusieurs fois](https://msdn.microsoft.com/library/windows/desktop/ff471324#MultiSample_RenderTarget), prise en charge n’est pas garantie pour les cibles de résolution. Cette vérification est nécessaire avant toute tentative d’utilisation de la technique d’échantillonnage multiple décrite dans cette rubrique.

    Le code suivant vérifie la prise en charge de l’échantillonnage multiple pour l’ensemble des valeurs de DXGI\_FORMAT:

    ```cpp
    // Determine the format support for multisampling.
    for (UINT i = 1; i < DXGI_FORMAT_MAX; i++)
    {
        DXGI_FORMAT inFormat = safe_cast<DXGI_FORMAT>(i);
        UINT formatSupport = 0;
        HRESULT hr = m_d3dDevice->CheckFormatSupport(inFormat, &formatSupport);

        if ((formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RESOLVE) &&
            (formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RENDERTARGET)
            )
        {
            m_supportInfo->SetFormatSupport(i, true);
        }
        else
        {
            m_supportInfo->SetFormatSupport(i, false);
        }
    }
    ```

2.  Pour chaque format pris en charge, déterminez quelle est la prise en charge du nombre d’échantillons en appelant [**ID3D11Device::CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499).

    Le code suivant vérifie la prise en charge de la taille des échantillons pour les formats DXGI pris en charge:

    ```cpp
    // Find available sample sizes for each supported format.
    for (unsigned int i = 0; i < DXGI_FORMAT_MAX; i++)
    {
        for (unsigned int j = 1; j < MAX_SAMPLES_CHECK; j++)
        {
            UINT numQualityFlags;

            HRESULT test = m_d3dDevice->CheckMultisampleQualityLevels(
                (DXGI_FORMAT) i,
                j,
                &numQualityFlags
                );

            if (SUCCEEDED(test) && (numQualityFlags > 0))
            {
                m_supportInfo->SetSampleSize(i, j, 1);
                m_supportInfo->SetQualityFlagsAt(i, j, numQualityFlags);
            }
        }
    }
    ```

    > **Remarque**  utilisation [**ID3D11Device2::CheckMultisampleQualityLevels1**](https://msdn.microsoft.com/library/windows/desktop/dn280494) à la place si vous avez besoin vérifier la prise en charge à échantillonnage multiple pour sous forme de vignettes tampons de ressources.

     

3.  Créez un tampon et un affichage de la cible de rendu avec le nombre d’échantillons souhaité. Utilisez les mêmes valeurs de DXGI\_FORMAT, de largeur et de hauteur pour la chaîne d’échange, mais spécifiez un nombre d’échantillons supérieur à1 et utilisez une dimension de texture échantillonnée plusieurs fois (**D3D11\_RTV\_DIMENSION\_TEXTURE2DMS** par exemple). Si nécessaire, vous pouvez recréer la chaîne d’échange avec de nouveaux paramètres optimaux pour l’échantillonnage multiple.

    Le code suivant crée une cible de rendu échantillonnée plusieurs fois :

    ```cpp
    float widthMulti = m_d3dRenderTargetSize.Width;
    float heightMulti = m_d3dRenderTargetSize.Height;

    D3D11_TEXTURE2D_DESC offScreenSurfaceDesc;
    ZeroMemory(&offScreenSurfaceDesc, sizeof(D3D11_TEXTURE2D_DESC));

    offScreenSurfaceDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    offScreenSurfaceDesc.Width = static_cast<UINT>(widthMulti);
    offScreenSurfaceDesc.Height = static_cast<UINT>(heightMulti);
    offScreenSurfaceDesc.BindFlags = D3D11_BIND_RENDER_TARGET;
    offScreenSurfaceDesc.MipLevels = 1;
    offScreenSurfaceDesc.ArraySize = 1;
    offScreenSurfaceDesc.SampleDesc.Count = m_sampleSize;
    offScreenSurfaceDesc.SampleDesc.Quality = m_qualityFlags;

    // Create a surface that's multisampled.
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &offScreenSurfaceDesc,
        nullptr,
        &m_offScreenSurface)
        );

    // Create a render target view. 
    CD3D11_RENDER_TARGET_VIEW_DESC renderTargetViewDesc(D3D11_RTV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
        m_offScreenSurface.Get(),
        &renderTargetViewDesc,
        &m_d3dRenderTargetView
        )
        );
    ```

4.  Le tampon de profondeur doit avoir la même largeur, la même hauteur, le même nombre d’échantillons et la même dimension de texture que la cible de rendu échantillonnée plusieurs fois.

    Le code suivant crée un tampon de profondeur échantillonné plusieurs fois :

    ```cpp
    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT,
        static_cast<UINT>(widthMulti),
        static_cast<UINT>(heightMulti),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL,
        D3D11_USAGE_DEFAULT,
        0,
        m_sampleSize,
        m_qualityFlags
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &depthStencilDesc,
        nullptr,
        &depthStencil
        )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
        depthStencil.Get(),
        &depthStencilViewDesc,
        &m_d3dDepthStencilView
        )
        );
    ```

5.  Il faut ensuite créer la fenêtre d’affichage, car la largeur et la hauteur de la fenêtre d’affichage doivent également correspondre à celles de la cible de rendu.

    Le code suivant crée une fenêtre d’affichage :

    ```cpp
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        widthMulti / m_scalingFactor,
        heightMulti / m_scalingFactor
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

6.  Effectuez le rendu de chaque image vers la cible de rendu échantillonnée plusieurs fois. Une fois le rendu terminé, appelez [**ID3D11DeviceContext::ResolveSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476474) avant de présenter l’image. Cela indique à Direct3D qu’il doit effectuer l’opération d’échantillonnage multiple, calculer la valeur de chaque pixel à afficher et placer le résultat en mémoire tampon d’arrière-plan. La mémoire tampon d’arrière-plan contient l’image finale sans crénelage et peut être présentée.

    Le code suivant résout la sous-ressource avant de présenter l’image :

    ```cpp
    if (m_sampleSize > 1)
    {
        unsigned int sub = D3D11CalcSubresource(0, 0, 1);

        m_d3dContext->ResolveSubresource(
            m_backBuffer.Get(),
            sub,
            m_offScreenSurface.Get(),
            sub,
            DXGI_FORMAT_B8G8R8A8_UNORM
            );
    }

    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    hr = m_swapChain->Present(1, 0);
    ```

 

 




