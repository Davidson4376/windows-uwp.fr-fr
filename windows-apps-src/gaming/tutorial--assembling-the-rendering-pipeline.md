---
title: Présentation du rendu
description: Découvrez comment assembler le pipeline de rendu pour afficher les graphiques. Présentation du rendu.
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: windows10, uwp, jeux, rendu
ms.localizationpriority: medium
ms.openlocfilehash: 6724aedf898706dd4c5bf728616c918d64b2fb32
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8202579"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>Infrastructure de renduI: présentation du rendu

Dans les rubriques précédentes, vous avez vu comment structurer un jeu de plateforme Windows universelle (UWP) à utiliser avec Windows Runtime et comment définir une machine à états pour gérer le flux du jeu. Vous allez maintenant découvrir comment assembler l'infrastructure du rendu. Examinons comment l’exemple de jeu restitue la scène avec Direct3D11 (communément appelé DirectX 11).

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

Direct3D 11 comporte un ensemble d’API qui permettent d’accéder aux fonctionnalités avancées d'un matériel graphique hautes performances pouvant être utilisées pour créer des graphiques 3D pour les applications gourmandes en graphiques telles que les jeux.

Le rendu de graphiques de jeu à l’écran est en fait un rendu d'une séquence d’images à l’écran. Dans chaque image, vous devez restituer les objets qui sont visibles dans la scène, en fonction de la présentation. 

Afin d’afficher une image, vous devez transmettre les informations de la scène requises au matériel afin qu’elles puissent être vues à l’écran. Si vous souhaitez afficher un élément à l’écran, vous devez démarrer le rendu dès que le jeu commence à s’exécuter.

## <a name="objective"></a>Objectif

Pour configurer une infrastructure de rendu de base afin d'afficher la sortie graphique pour un jeu UWP DirectX, vous pouvez regrouper les graphiques en suivant ces trois étapes.

 1. Établir une connexion avec l’interface graphique
 2. Créer les ressources nécessaires pour dessiner les graphiques
 3. Afficher les graphiques avec le rendu de l’image

Cet article explique comment les graphiques sont restitués et couvre les étapes1 et 3.

[Infrastructure de rendu II: rendu de jeu](tutorial-game-rendering.md) couvre l’étape2; comment configurer l’infrastructure de rendu et comment les données sont préparées pour obtenir le rendu.

## <a name="get-started"></a>Prise en main

Avant de commencer, vous devez vous familiariser avec les concepts de rendu graphique de base. Si vous débutez avec Direct3D et les fonctionnalités de rendu, consultez [Termes et concepts](#terms-and-concepts) pour lire une brève description des graphiques et des termes utilisés dans cet article pour décrire le rendu.

Pour ce jeu, l'objet de classe __GameRenderer__ représente le convertisseur pour cet exemple de jeu.  Le convertisseur se charge de la création et de la gestion de tous les objetsDirect3D 11 et Direct2D utilisés pour générer les effets visuels du jeu.  Il conserve également une référence à l'objet __Simple3DGame__ utilisé pour récupérer la liste des objets pour le rendu, ainsi que l’état du jeu pour l'affichage à tête haute (HUD). 

Dans cette partie du didacticiel, nous allons nous concentrer sur le rendu d’objets 3D dans le jeu.

## <a name="establish-a-connection-to-the-graphics-interface"></a>Établir une connexion avec l’interface graphique

Pour accéder au matériel dédié au rendu, consultez l’article sur l'infrastructure UWP sous [__App::Initialize__](tutorial--building-the-games-uwp-app-framework.md#appinitialize-method).

La __fonction make\_shared__ illustrée [ci-dessous](#appinitialize-method), est utilisée pour créer un __shared\_ptr__ sur [__DX::DeviceResources__](#dxdeviceresources), ce qui permet également d’accéder à l’appareil. 

Dans Direct3D11, un [appareil](#device) est utilisé pour allouer et détruire les objets, restituer des primitives et communiquer avec la carte graphique via le pilote graphique.

### <a name="appinitialize-method"></a>Méthode App::Initialize

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    //...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>Afficher les graphiques avec le rendu de l’image

La scène du jeu doit restituer le rendu lorsque le jeu est lancé. Les instructions pour restituer le rendu démarrent dans la méthode [__GameMain::Run__](#gameamainrun-method), comme illustré ci-dessous.

Le flux simple est le suivant:
1. __Mise à jour__
2. __Afficher__
3. __Présent__

### <a name="gamemainrun-method"></a>Méthode GameMain::Run

```cpp
// Comparing this to App::Run() in the DirectX 11 App template
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            // Added: Switching different game states since this game sample is more complex
            switch (m_updateState) 
            {

                // ...
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                // 1. Update
                // Also exist in DirectX 11 App template: Update loop to get the latest game changes
                Update();
                
                // 2. Render
                // Present also in DirectX 11 App template: Prepare to render
                m_renderer->Render();
                
                // 3. Present
                // Present also in DirectX 11 App template: Present the 
                // contents of the swap chain to the screen.
                m_deviceResources->Present(); 
                
                // Added: resetting a variable we've added to indicate that 
                // render has been done and no longer needed to render
                m_renderNeeded = false; 
            }
        }
        else
        {   
            // Present also in DirectX 11 App template
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending); 
        }
    }
    m_game->OnSuspending();  // exiting due to window close. Make sure to save state.
}
```

### <a name="update"></a>Mise à jour

Consultez l'article [Gestion du flux de jeux](tutorial-game-flow-management.md) pour plus d’informations sur la façon dont les états du jeu sont mis à jour dans la méthode [__App::Update__ et __GameMain::Update__](tutorial-game-flow-management.md#appupdate-method).

### <a name="render"></a>Afficher

Le rendu est implémenté en appelant la méthode [__GameRenderer::Render__](#gamerendererrender-method) dans __GameMain::Run__.

Si le [rendu stéréo](#stereo-rendering) est activé, il y a deux passes de rendu: une pour le œil droit et une pour le œil gauche. Dans chaque passe de rendu, nous lions la cible du rendu et la [vue de profondeur-gabarit](#depth-stencil-view) à l'appareil. Nous effaçons également la vue de profondeur-gabarit par la suite.

> [!Note]
> Le rendu stéréo peut être obtenu à l’aide d’autres méthodes telles que la passe stéréo unique à l’aide de l’instanciation de vertex ou des nuanceurs de géométrie. La méthode avec deux passes de rendu est plus lente, mais elle est plus pratique pour obtenir un rendu stéréo.

Dès que le jeu est créé et que les ressources sont chargées, mettez à jour la [matrice de projection](#projection-transform-matrix), une fois par passe de rendu. Les objets sont légèrement différents dans chaque affichage. Ensuite, configurez le [pipeline de rendu graphique](#rendering-pipeline). 

> [!Note]
> Consultez [Créer et charger les ressources graphiques DirectX](tutorial-game-rendering.md#create-and-load-directx-graphic-resources) pour plus d’informations sur la façon de charger les ressources.

Dans cet exemple de jeu, le convertisseur est conçu pour utiliser un schéma de vertex standard sur tous les objets. Cela simplifie la conception du nuanceur et facilite les modifications entre les nuanceurs, indépendamment de la géométrie des objets.

#### <a name="gamerendererrender-method"></a>Méthode GameRenderer::Render

Définissez le contexte Direct3D pour utiliser un schéma de vertex d'entrée. Les objets du schéma d’entrée décrivent comment diffuser les données de la mémoire tampon du vertex dans le [pipeline de rendu](#rendering-pipeline). 

Ensuite, nous avons paramétré le contexte Direct3D pour qu'il utilise les mémoires [tampons constantes](#constant-buffers) définies plus tôt, qui sont utilisées par le stade de pipeline du [nuanceur de vertex](#vertex-shaders-and-pixel-shaders) et le stade de pipeline du [nuanceur de pixels](#vertex-shaders-and-pixel-shaders). 

> [!Note]
> Consultez [Infrastructure de rendu II: rendu de jeu](tutorial-game-rendering.md) pour plus d’informations sur la définition des mémoires tampons constantes.

Dans la mesure où le même schéma d’entrée et le même jeu de mémoires tampons constantes sont utilisés pour tous les nuanceurs qui se trouvent dans le pipeline, ils sont définis une fois par trame.

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx

            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            
            // Clears the depth stencil view.
            // A depth stencil view contains the format and buffer to hold depth and stencil info.
            // For more info about depth stencil view, go to: 
            // https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-stencil-view--dsv-
            // A depth buffer is used to store depth information to control which areas of 
            // polygons are rendered rather than hidden from view. To learn more about a depth buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-buffers
            // A stencil buffer is used to mask pixels in an image, to produce special effects. 
            // The mask determines whether a pixel is drawn or not,
            // by setting the bit to a 1 or 0. To learn more about a stencil buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/stencil-buffers

            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() }; 
            
            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap()); 
        }

        // Render the scene objects
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (stereoEnabled)
            {
                // When doing stereo, it is necessary to update the projection matrix once per rendering pass.

                auto orientation = m_deviceResources->GetOrientationTransform3D();

                ConstantBufferChangeOnResize changesOnResize;

                // Apply either a left or right eye projection, which is an offset from the middle
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Setup the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, 
            // go to https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476454.aspx
            d3dContext->IASetInputLayout(m_vertexLayout.Get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476491.aspx

            d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476470.aspx

            d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); /
            }
        }
        else
        {
            const float ClearColor[4] = {0.5f, 0.5f, 0.8f, 1.0f};

            // Only need to clear the background when not rendering the full 3D scene since
            // the 3D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (i > 0)
            {
                // Doing the Right Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), ClearColor);
            }
        }

        // Start of 2D rendering
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // ...
    }
}
```

### <a name="primitive-rendering"></a>Rendu des primitives

Lors du rendu de la scène, vous parcourez tous les objets qui doivent être rendus. Les étapes ci-dessous sont répétées pour chaque objet (primitive).

* Mettez à jour la mémoire tampon constante (__m\_constantBufferChangesEveryPrim__) avec la [matrice de transformation universelle](#world-transform-matrix) et les informations sur le matériel du modèle.
* Le __m\_constantBufferChangesEveryPrim__ contient des paramètres pour chaque objet.  Il inclut l’objet de la matrice de transformation universelle ainsi que les propriétés du matériel telles que la couleur et l'exposant spéculaire pour les calculs d’éclairage.
* Définir le contexte Direct3D pour utiliser le schéma de vertex d’entrée afin que les données de l’objet de maillage soient transmises à l’étape d’assembleur d’entrée (IA) du [pipeline de rendu](#rendering-pipeline)
* Définissez le contexte Direct3D pour utiliser un [tampon d’index](#index-buffer) dans l’étape IA. Fournissez les informations des primitives: type, ordre des données.
* Soumettez un appel de dessin pour dessiner la primitive indexée, non instanciée. La méthode __GameObject::Render__ met à jour la mémoire [tampon constante](#constant-buffer-or-shader-constant-buffer) de la primitive avec les données propres à une primitive donnée. Cela aboutit à un appel __DrawIndexed__ sur le contexte pour dessiner la géométrie de chaque primitive. Plus spécifiquement, cet appel de dessin met en file d’attente des commandes et des données pour le processeur graphique virtuel (GPU), comme paramétré par les données de la mémoire tampon constante. Chaque appel de dessin exécute le [nuanceur de vertex](#vertex-shaders-and-pixel-shaders) une fois par vertex, puis le [nuanceur de pixels](#vertex-shaders-and-pixel-shaders) une fois pour chaque pixel de chaque triangle dans la primitive. Les textures font partie de l’état utilisé par le nuanceur de pixels pour effectuer le rendu.

Les mémoires tampons constantes ont plusieurs fonctions:
    * Le jeu utilise plusieurs mémoires tampons constantes, mais n’a besoin de les mettre à jour qu’une seule fois par primitive. Comme indiqué plus haut, les mémoires tampons constantes servent de données pour les nuanceurs qui sont exécutés pour chaque primitive. Certaines données sont statiques (__m_constantBufferNeverChanges__), certaines données sont constantes sur la trame (__m\_constantBufferChangesEveryFrame__), comme la position de la caméra, et certaines données sont propres à la primitive, comme ses couleurs et textures (__m\_constantBufferChangesEveryPrim__)
    * Le [convertisseur](#renderer) de jeu répartit ces données entrantes entre différentes mémoires tampons constantes pour optimiser la bande passante de mémoire utilisée par l’UC et le GPU. Cette approche permet également de réduire la quantité de données dont le GPU doit assurer le suivi. Le GPU a une longue file d’attente de commandes et que, chaque fois que le jeu appelle __Draw__, cette commande est mise en file d’attente avec les données qui lui sont associées. Lorsque le jeu met à jour la mémoire tampon constante de primitives et émet la commande __Draw__ suivante, le pilote graphique ajoute cette commande suivante et les données associées à la file d’attente. Si le jeu dessine 100primitives, la file d’attente peut contenir 100copies des données de la mémoire tampon constante. Pour réduire la quantité de données envoyées par le jeu au GPU, le jeu utilise une mémoire tampon constante de primitives distincte qui contient uniquement les mises à jour pour chaque primitive.

#### <a name="gameobjectrender-method"></a>Méthode GameObject::Render

```cpp
void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m\_active || (m\_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    // Put the model matrix info into a constant buffer, in world matrix.
    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    // Check to see which material to use on the object.
    // If a collision (a hit) is detected, GameObject::Render checks the current context, which 
    // indicates whether the target has been hit by an ammo sphere. If the target has been hit, 
    // this method applies a hit material, which reverses the colors of the rings of the target to 
    // indicate a successful hit to the player. Otherwise, it applies the default material 
    // with the same method. In both cases, it sets the material by calling Material::RenderSetup, 
    // which sets the appropriate constants into the constant buffer. Then, it calls 
    // ID3D11DeviceContext::PSSetShaderResources to set the corresponding texture resource for the 
    // pixel shader, and ID3D11DeviceContext::VSSetShader and ID3D11DeviceContext::PSSetShader 
    // to set the vertex shader and pixel shader objects themselves, respectively.

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }

    // Update the primitive constant buffer with the object model's info.
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    // Render the mesh.
    // See MeshObject::Render method below.
    m_mesh->Render(context);
}

#### MeshObject::Render method

void MeshObject::Render(\_In\_ ID3D11DeviceContext *context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476453.aspx
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476455.aspx
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476409.aspx
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="present"></a>Présent

Nous appelons la méthode __DX::DeviceResources::Present__ permettant d'insérer le contenu que nous avons placé dans les mémoires tampons et de l’afficher.

Nous utilisons le terme «chaîne d’échange» pour désigner une collection de mémoires tampons utilisées pour la présentation des images à l’utilisateur. Chaque fois qu’une application présente une nouvelle image à afficher, la première mémoire tampon de la chaîne d’échange prend la place de la mémoire tampon affichée. Ce processus est désigné sous le terme d’échange ou d’inversion. Pour plus d’informations, voir l’article [Chaînes d’échange](../graphics-concepts/swap-chains.md).

* La méthode __Present__ de l’interface __IDXGISwapChain1__ demande le blocage de [DXGI](#dxgi) jusqu'à la synchronisation verticale (VSync), plaçant l’application en veille jusqu'à la prochaine synchronisation verticale. Cela garantit que vous ne perdez pas les cycles de rendu d’images qui ne seront jamais affichés à l’écran.
* La méthode __DiscardView__ de l'interface __ID3D11DeviceContext3__ ignore le contenu de la [cible de rendu](#render-target). Il s’agit d’une opération valide uniquement lorsque le contenu existant est entièrement remplacé. Si des redirections incorrectes ou de défilement sont utilisées, cet appel doit être supprimé.
* À l’aide de la même méthode __DiscardView__, ignorez le contenu de la [profondeur-gabarit](#depth-stencil).
* La méthode __HandleDeviceLost__ est utilisée pour gérer le scénario si l'[appareil](#device) est supprimé. Si l'appareil a été supprimé par une déconnexion ou une mise à niveau du pilote, vous devez recréer toutes les ressources de l'appareil. Pour plus d’informations, voir [Gérer des scénarios de suppression d'appareil dans Direct3D11](handling-device-lost-scenarios.md).

> [!Tip]
> Pour obtenir une fréquence d’images fluide, vous devez vous assurer que le volume de travail pour afficher une image est adapté à l'intervalle VSync.

```cpp
// Present the contents of the swap chain to the screen.
void DX::DeviceResources::Present()
{
    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    HRESULT hr = m_swapChain->Present(1, 0);

    // Discard the contents of the render target.
    // This is a valid operation only when the existing contents will be entirely
    // overwritten. If dirty or scroll rects are used, this call should be removed.
    m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());

    // Discard the contents of the depth-stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    // For more info about how to handle a device lost scenario, go to:
    // https://docs.microsoft.com/windows/uwp/gaming/handling-device-lost-scenarios
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        DX::ThrowIfFailed(hr);
    }
}
```

## <a name="next-steps"></a>Étapes suivantes

Cet article explique comment un graphique est affiché à l'écran et fournit une brève description de certains termes de rendu utilisés. Pour en savoir plus sur le rendu, consultez l'article [Infrastructure de renduII: rendu de jeu](tutorial-game-rendering.md) et découvrez comment préparer les données requises avant le rendu.

## <a name="terms-and-concepts"></a>Termes et concepts

### <a name="simple-game-scene"></a>Scène de jeu simple

Une scène de jeu simple est constituée de plusieurs objets avec plusieurs sources de lumière.

Une forme d’objet est définie par un ensemble de coordonnées X, Y, Z dans l’espace. L’emplacement de rendu réel dans le monde du jeu peut être déterminé en appliquant une matrice de transformation à la position des coordonnées X, Y, Z. Il peut également y avoir un ensemble de coordonnées de texture, U et V, qui spécifient la façon dont une matière est appliquée à l’objet. Cela définit les propriétés de surface de l’objet et vous donne la possibilité de voir si un objet possède une surface rugueuse comme une balle de tennis ou une surface lisse et brillante comme une boule de bowling.

Les informations de scène et d’objet sont utilisées par l’infrastructure de rendu pour recréer la scène image par image, la rendant vivante sur votre écran.

### <a name="rendering-pipeline"></a>Pipeline de rendu

Le pipeline de rendu est le processus par lequel les informations de la scène 3D sont convertie en une image affichée à l’écran. Dans Direct3D 11, ce pipeline est programmable. Vous pouvez adapter les étapes pour prendre en charge vos besoins de rendu. Les étapes qui utilisent des noyaux de nuanceur communs sont programmables au moyen du langage de programmation HLSL. On l'appelle également le rendu des graphiques de pipeline ou simplement le pipeline.

Pour créer ce pipeline, vous devez connaître:
* [HLSL](#HLSL). Nous recommandons l’utilisation de HLSL Shader Model 5.1 et les versions supérieures pour les jeux UWP DirectX.
* [Nuanceurs](#Shaders)
* [Nuanceurs de vertex et de pixels](#vertext-shaders-pixel-shaders)
* [Étapes du nuanceur](#shader-stages)
* [Différents formats de fichier de nuanceur](#various-shader-file-formats)

Pour plus d’informations, voir [Comprendre le pipeline de rendu Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/dn643746.aspx) et [Pipeline graphique](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx).

#### <a name="hlsl"></a>HLSL

HLSL est le langage HLSL pour DirectX. À l’aide du langage HLSL, vous pouvez créer des nuanceurs programmables de type C pour le pipeline Direct3D. Pour plus d'informations, consultez la page [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561.aspx).

#### <a name="shaders"></a>Nuanceurs

Les nuanceurs peuvent être considérés comme un ensemble d’instructions qui déterminent la façon dont la surface d’un objet s’affiche lors du rendu. Ceux programmés à l’aide du langage HLSL sont appelés des nuanceurs HLSL. Les fichiers de code source pour les nuanceurs [HLSL])(#hlsl) portent l’extension de fichier .hlsl. Ces nuanceurs peuvent être compilés au moment de la création ou lors de l’exécution et définis lors de l’exécution dans l’étape du pipeline appropriée; un objet nuanceur compilé a une extension de fichier .cso.

Les nuanceurs Direct3D 9 peuvent être conçus à l’aide du modèle de nuanceur1, du modèle de nuanceur2 et du modèle de nuanceur 3; les nuanceurs Direct3D 10 peuvent uniquement être conçus à l'aide du modèle de nuanceur4. Les nuanceurs Direct3D 11 peuvent être conçu sur le modèle de nuanceur 5. Direct3D 11.3 et Direct3D 12 peuvent être conçus sur le modèle de nuanceur 5.1 et Direct3D 12 peut également être conçu sur le modèle de nuanceur 6.

#### <a name="vertex-shaders-and-pixel-shaders"></a>Nuanceurs de vertex et de pixels

Les données sont entrées dans le pipeline graphique en tant que flux de primitives et sont traitées par différents nuanceurs tels que les nuanceurs de vertex et les nuanceurs de pixels. 

Les nuanceurs de vertex traitent les vertex, notamment en effectuant des opérations telles que des transformations et l'application d'une apparence et d'un éclairage.  Les nuanceurs de pixels permettent d’appliquer des techniques d’ombrage enrichies telles que l’éclairage par pixel et le post-traitement. Ils combinent des constantes, des données de texture, des valeurs interpolées par sommet et d’autres données pour produire des sorties par pixel. 

#### <a name="shader-stages"></a>Étapes du nuanceur

Une séquence de ces différents nuanceurs définis pour traiter ce flux de primitives correspond aux étapes de nuanceur dans un pipeline de rendu. Les étapes réelles dépendent de la version de Direct3D, mais elles incluent généralement les étapes de vertex, de géométrie et de pixels. Il existe également d'autres étapes, telles que les nuanceurs de coque et de domaine pour le pavage et le nuanceur de calcul. Toutes ces étapes sont complètement programmables à l’aide de [HLSL])(#hlsl). Pour plus d’informations, consultez [Pipeline graphique](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx).

#### <a name="various-shader-file-formats"></a>Différents formats de fichier de nuanceur

Extensions de fichier de code de nuanceur:
    * Un fichier avec l’extension .hlsl contient le code source [HLSL])(#hlsl).
    * Un fichier avec l’extension .cso contient un objet nuanceur compilé.
    * Un fichier avec l’extension .h est un fichier d’en-tête, mais dans un contexte de code de nuanceur, ce fichier d’en-tête définit un tableau d’octets qui contient les données du nuanceur.
    * Un fichier avec l’extension .hlsli contient le format des mémoires tampons constantes. Dans l’exemple de jeu, le fichier est __Shaders__>__ConstantBuffers.hlsli__.

> [!Note]
> Vous pouvez incorporer le nuanceur soit en chargeant un fichier .cso lors de l’exécution, soit en ajoutant le fichier .h dans votre code exécutable. Mais vous ne pouvez pas utiliser les deux pour le même nuanceur.

### <a name="deeper-understanding-of-directx"></a>Mieux comprendre DirectX

Direct3D 11 est un ensemble d’API qui peuvent créer des graphiques pour les applications qui demandent d’importantes ressources graphiques comme les jeux, où une carte graphique de qualité est nécessaire pour traiter des calculs complexes. Cette section explique brièvement les concepts de la programmation graphique Direct3D 11: la ressource, la sous-ressource, l'appareil et le contexte de l'appareil.

#### <a name="resource"></a>Ressource

Si vous débutez, vous pouvez considérer les ressources (également appelées ressources de l'appareil) comme des informations permettant de rendre un objet comme une texture, une position ou une couleur. Les ressources fournissent des données au pipeline et définissent ce qui est rendu au cours de votre scène. Les ressources peuvent être chargées à partir de vos jeux ou créées dynamiquement au moment de l’exécution.

Une ressource est en fait une zone de la mémoire accessible par le [pipeline](#rendering-pipeline) Direct3D. Pour permettre un accès efficace du pipeline à la mémoire, les données fournies au pipeline (géométrie d’entrée, ressources des nuanceurs et textures) doivent être stockées dans une ressource. Il existe 2types de ressources à partir desquelles l’ensemble des ressources Direct3D dérivent: une mémoire tampon et une texture. Jusqu’à 128ressources peuvent être actives à chaque étape du pipeline. Pour plus d'informations, voir [Resources](../graphics-concepts/resources.md).

#### <a name="subresource"></a>Sous-ressource

Le terme «sous-ressource» fait référence à un sous-ensemble d’une ressource. Direct3D peut référencer une ressource entière ou des sous-ensembles d’une ressource. Pour plus d'informations, voir [Sous-resource](../graphics-concepts/resource-types.md#subresources).

#### <a name="depth-stencil"></a>Gabarit-profondeur

Une ressource de gabarit-profondeur contient le format et la mémoire tampon permettant de contenir les informations de profondeur et de gabarit. Elle est créée à l’aide d’une ressource de texture. Pour plus d’informations sur la création d’une ressource de gabarit-profondeur, voir [Configuration de la fonctionnalité de profondeur-gabarit](https://msdn.microsoft.com/library/windows/desktop/bb205074.aspx). Nous pouvons accéder à la ressource de profondeur-gabarit par le biais de la vue de profondeur-gabarit implémentée à l’aide de l'interface [ID3D11DepthStencilView](https://msdn.microsoft.com/library/windows/desktop/ff476377.aspx).

Les informations de profondeur nous indiquent les zones de polygones à afficher, plutôt que celles à masquer. Les informations de gabarit indiquent quels pixels sont masqués. Elle peut être utilisée pour produire des effets spéciaux dans la mesure où elle détermine si un pixel est dessiné ou non; définit le bit 1 ou le bit 0. 

Pour plus d’informations, voir: [Affichage du gabarit de profondeur](../graphics-concepts/depth-stencil-view--dsv-.md), [tampon de profondeur](../graphics-concepts/depth-buffers.md), et [Mémoire tampon de gabarit](../graphics-concepts/stencil-buffers.md).

#### <a name="render-target"></a>Cible de rendu

Une cible de rendu est une ressource qui nous pouvons écrire à la fin d’une passe de rendu. Elle est généralement créée à l’aide de la méthode [ID3D11Device::CreateRenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476517.aspx) en utilisant la chaîne d'échange de tampon d’arrière-plan (qui est également une ressource) en tant que paramètre d’entrée. 

Chaque cible de rendu doit également avoir une vue de profondeur-gabarit correspondante, car lorsque nous utilisons [OMSetRenderTargets](https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx) pour définir la cible de rendu avant de l’utiliser, il requiert également une vue de profondeur-gabarit. Nous pouvons accéder à la ressource de cible de rendu par le biais de l’affichage de cible de rendu implémenté à l’aide de l'interface [ID3D11RenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476582.aspx). 

#### <a name="device"></a>Appareil

Pour ceux qui débutent avec Direct3D11, vous pouvez considérer un appareil comme le moyen utilisé pour allouer et détruire les objets, restituer des primitives et communiquer avec la carte graphique via le pilote graphique. 

Plus précisément, un appareil Direct3D est le composant de rendu de Direct3D. Un appareil encapsule et stocke l’état de rendu, exécute des transformations et des opérations d’éclairage, et rastérise une image sur une surface. Pour plus d’informations, voir [Appareils](../graphics-concepts/devices.md).

Un appareil est représenté par l'interface [ID3D11Device](https://msdn.microsoft.com/library/windows/desktop/ff476379.aspx). En d’autres termes, l’interface ID3D11Device représente une carte graphique virtuelle et est utilisée pour créer les ressources qui appartiennent à un appareil. 

Notez qu’il existe différentes versions de ID3D11Device, [ID3D11Device5](https://msdn.microsoft.com/library/windows/desktop/mt492478.aspx) est la version la plus récente et ajoute de nouvelles méthodes à celles de ID3D11Device4. Pour plus d’informations sur la façon dont Direct3D communique avec le matériel sous-jacent, voir [Architecture du modèle de pilote de périphérique Windows (WDDM)](https://docs.microsoft.com/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture).

Chaque application doit avoir au moins un appareil, la plupart des applications créent uniquement un appareil. Créez un appareil pour un des pilotes de matériel installés sur votre ordinateur en appelant __D3D11CreateDevice__ ou __D3D11CreateDeviceAndSwapChain__ et en spécifiant le type de pilote avec l’indicateur D3D\_DRIVER\_TYPE. Chaque appareil peut utiliser un ou plusieurs contextes d'appareil, en fonction de la fonctionnalité souhaitée. Pour plus d’information, voir [Fonction D3D11CreateDevice](https://msdn.microsoft.com/library/windows/desktop/ff476082.aspx).

#### <a name="device-context"></a>Contexte d'appareil

Un contexte d'appareil sert à définir l’état du [pipeline](#rendering-pipeline) et à générer des commandes de rendu à l'aide des [ressources](#resource) appartenant à un [appareil](#device). 

Direct3D 11 implémente deux types de contextes d'appareil, un pour le rendu immédiat et l’autre pour le rendu différé. Les deux contextes sont représentés par une interface [ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476385.aspx).  

Les interfaces __ID3D11DeviceContext__ ont des versions différentes; __ID3D11DeviceContext4__ ajoute de nouvelles méthodes à celles de __ID3D11DeviceContext3__.

Remarque: __ID3D11DeviceContext4__ est introduite dans Windows10Creators Update et est la dernière version de l'interface __ID3D11DeviceContext__. Les applications ciblant Windows10Creators Update doivent utiliser cette interface au lieu des versions antérieures. Pour plus d’informations, voir [ID3D11DeviceContext4](https://msdn.microsoft.com/library/windows/desktop/mt492481.aspx).

#### <a name="dxdeviceresources"></a>DX::DeviceResources

La classe __DX::DeviceResources__ se trouve dans les fichiers __DeviceResources.cpp__/__.h__ et gère l’ensemble des ressources de l'appareil DirectX. Dans l’exemple de projet de jeu et le projet de modèle d'application DirectX11, ces fichiers se trouvent dans le dossier __Commons__. Vous pouvez obtenir la dernière version de ces fichiers lorsque vous créez un nouveau projet de modèle application DirectX11 dans Visual Studio2015 ou les versions ultérieures.

### <a name="buffer"></a>Mémoire tampon

Une ressource de mémoire tampon est un ensemble de données dont le type a été intégralement spécifié, regroupées en éléments. Vous pouvez utiliser les mémoires tampons pour stocker une grande variété de données, telles que les vecteurs de position, les vecteurs normaux, les coordonnées de texture dans une mémoire tampon de vertex, les index dans une mémoire tampon d’index ou l’état de l’appareil. Les éléments de mémoire tampon peuvent comporter des valeurs de données compressées (comme des valeurs de surface R8G8B8A8), des entiers8bits uniques ou quatre valeurs à virgule flottante de 32bits.

Il existe trois types de mémoires tampons: la mémoire tampon de vertex, la mémoire tampon d’index et la mémoire tampon constante.

#### <a name="vertex-buffer"></a>Mémoire tampon de vertex

Contient les données de vertex utilisées pour définir votre géométrie. Les données de vertex comprennent les coordonnées de position, les données de couleur, les données de coordonnées de texture, les données normales, etc. 

#### <a name="index-buffer"></a>Mémoire tampon d’index

Contient des décalages d’entier dans les tampons de vertex; elles ont vocation à accroître l’efficacité du rendu des primitives. Un tampon d’index contient un ensemble séquentiel d’index de 16 ou 32bits; chaque index est utilisé pour identifier un sommet dans un tampon de sommet.

#### <a name="constant-buffer-or-shader-constant-buffer"></a>Mémoire tampon constante ou mémoire tampon constante de nuanceur

Vous permet de fournir efficacement des données de nuanceur au pipeline. Vous pouvez utiliser les mémoires tampons constantes en tant qu’entrées pour les nuanceurs qui s’exécutent pour chaque primitive et stockent les résultats de l'étape de flux de sortie du pipeline de rendu. D’un point de vue conceptuel, une mémoire tampon constante ressemble à un tampon de vertex à élément unique.

#### <a name="design-and-implementation-of-buffers"></a>Conception et mise en œuvre des mémoires tampons

Vous pouvez concevoir des mémoires tampons en fonction du type de données, par exemple, comme dans notre exemple de jeu, une mémoire tampon est créée pour les données statiques, une autre pour les données qui sont constantes sur la trame et une autre pour les données spécifiques à une primitive.

Tous les types de mémoires tampons sont encapsulés par l'interface __ID3D11Buffer__ et vous pouvez créer une ressource de mémoire tampon en appelant __ID3D11Device::CreateBuffer__. Toutefois, une mémoire tampon doit être liée au pipeline avant d’être accessible. Les mémoires tampons peuvent être liées à plusieurs étapes du pipeline simultanément pour la lecture. Une mémoire tampon peut également être liée à une étape du pipeline unique pour l’écriture; toutefois, la même mémoire tampon ne peut pas être liée pour lire et écrire simultanément.

Liez les mémoires tampons pour:
    * L'étape de l’assembleur d’entrée en appelant les méthodes __ID3D11DeviceContext__ comme __ID3D11DeviceContext::IASetVertexBuffers__ et __ID3D11DeviceContext::IASetIndexBuffer__
    * L'étape de la sortie du flux en appelant __ID3D11DeviceContext::SOSetTargets__
    * L'étape du nuanceur en appelant les méthodes de nuanceur, comme __ID3D11DeviceContext::VSSetConstantBuffers__

Pour plus d’informations, voir [Présentation des mémoires tampons dans Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476898.aspx).

### <a name="dxgi"></a>DXGI

Microsoft DirectX Graphics Infrastructure (DXGI) est un nouveau sous-système introduit avec Windows Vista qui encapsule les tâches de bas niveau requises par Direct3D 10, 10.1, 11 et 11.1. Il convient de faire attention lorsque vous utilisez DXGI dans une application multithread, afin d’éviter les blocages. Pour plus d’informations, voir [Infrastructure DXGI (DirectX Graphics): meilleures pratiques - Multithreading](https://msdn.microsoft.com/library/windows/desktop/ee417025.aspx#multithreading_and_dxgi)

### <a name="feature-level"></a>Niveau de fonctionnalité

Le niveau de fonctionnalité est un concept introduit dans Direct3D 11 pour gérer la diversité des cartes vidéo dans les ordinateurs virtuels nouveaux et existants. Un niveau de fonctionnalité est un ensemble de fonctionnalités de processeur graphique (GPU) définies. 

Chaque carte vidéo implémente un certain niveau de fonctionnalité DirectX en fonction des GPU installés. Dans les versions antérieures de MicrosoftDirect3D, vous pouviez avoir la version de Direct3D, la carte vidéo implémentée et ensuite votre application en conséquence. 

Avec le niveau de fonctionnalité, lorsque vous créez un appareil, vous pouvez tenter de créer un appareil pour le niveau de fonctionnalité que vous souhaitez demander. Si la création d'appareil fonctionne, ce niveau de fonctionnalité existe, si ce n’est pas, le matériel ne prend pas en charge ce niveau de fonctionnalité. Vous pouvez essayer de recréer un appareil avec un niveau de fonctionnalité inférieur ou vous pouvez choisir de quitter l’application. Par exemple, le niveau de fonctionnalité 12\_0 nécessite Direct3D 11.3 ou Direct3D 12 et le modèle de nuanceur 5.1. Pour plus d’informations, voir [Niveaux de fonctionnalité Direct3D: vue d’ensemble pour chaque niveau de fonctionnalité](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx#Overview).

À l’aide de niveaux de fonctionnalité, vous pouvez développer une application Direct3D9, Microsoft Direct3D10 ou Direct3D11 et exécuter sur du matériel 9, 10 ou 11 (avec quelques exceptions). Pour plus d’informations, voir [Niveaux de fonctionnalité Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx).

### <a name="stereo-rendering"></a>Rendu stéréo

Le rendu stéréo est utilisé pour améliorer l’illusion de profondeur. Il utilise deux images, une à partir du œil gauche et l’autre à partir du œil droit, pour afficher une scène à l’écran. 

D'un point de vue mathématique, nous appliquons une matrice de projection stéréo, qui a un léger décalage horizontal à droite et à gauche de la matrice de projection mono régulière afin d'effectuer cette opération.

Nous avons fait deux passes de rendu pour obtenir un rendu stéréo dans cet exemple de jeu:
* Liez à la cible de rendu de droite, appliquez une projection droite, puis dessinez l’objet de primitive.
* Liez à la cible de rendu de gauche, appliquez une projection gauche, puis dessinez l’objet de primitive.

### <a name="camera-and-coordinate-space"></a>Espace de coordonnées et de la caméra

Le jeu a le code en place pour mettre à jour le monde dans son propre système de coordonnées (parfois appelé espace du monde ou espace de scène). Tous les objets, y compris la caméra, sont positionnés et orientés dans cet espace. Pour plus d'informations, voir [Systèmes de coordonnées](../graphics-concepts/coordinate-systems.md).

Un nuanceur de vertex effectue la lourde tâche de convertir les coordonnées du modèle en coordonnées de périphérique avec l’algorithme suivant (oùV est un vecteur etM est une matrice).

V(appareil) = V(modèle) x M(modèle à universel) x M(universel à vue) x M(vue à appareil).

où: 
* M(modèle à universel) est une matrice de transformation des coordonnées du modèle en coordonnées universelles, également connue sous le nom de [matrice de transformation universelle](#world-transform-matrix). Elle est fournie par la primitive.
* M(universel à vue) est une matrice de transformation des coordonnées universelles en coordonnées de vue, également connue sous le nom de [matrice de transformation de vue](#view-transform-matrix).
    * Elle est fournie par la matrice globale de la caméra. Elle est définie par la position de la caméra et les vecteurs de vue (le vecteur « de visualisation » qui pointe directement sur la scène depuis la caméra et le vecteur « de recherche » qui pointe vers le haut, à la perpendiculaire).
    * Dans l’exemple de jeu, __m_viewmatrix__ est la matrice de transformation calculé à l’aide de __Camera::SetViewParams__ 
* M(vue à appareil) est une matrice de transformation des coordonnées de vue en coordonnées d'appareil, également connue sous le nom de [matrice de transformation de projection](#projection-transform-matrix).
    * Elle est fournie par la projection de la caméra. Elle indique combien de cet espace est vraiment visible dans la scène finale. Le champ de vision, les proportions et les plans de découpage définissent la matrice de transformation de projection.
    * Dans l’exemple de jeu, __m\_projectionMatrix__ définit les coordonnées de projection, calculées à l’aide de la transformation __Camera::SetProjParams__ (pour la projection stéréo, vous utilisez deux matrices de projection: une pour l’affichage de chaque œil.) 

Le code du nuanceur dans VertexShader.hlsl est chargé avec ces vecteurs et matrices à partir des mémoires tampons constantes et effectue cette transformation pour chaque vertex.

### <a name="coordinate-transformation"></a>Transformation de coordonnées

Direct3D utilise trois transformations pour modifier vos coordonnées du modèle 3D en coordonnées de pixels (espace d'écran). Ces transformations sont la transformation universelle, la transformation de vue et la transformation de projection. Pour plus d’informations, accédez à [Présentation de la transformation](../graphics-concepts/transform-overview.md).

#### <a name="world-transform-matrix"></a>Matrice de transformation universelle

Une transformation universelle change les coordonnées depuis l'espace du modèle, ou les vertex sont définis par rapport à l’origine locale d’un modèle, vers l'espace universel, où les vertex sont définis par rapport à l’origine commune à tous les objets dans une scène. Par essence, la transformation universelle place un modèle dans le monde. Pour plus d’informations, voir [Transformation universelle](../graphics-concepts/world-transform.md).

####  <a name="view-transform-matrix"></a>Matrice de transformation de vue

La transformation de vue localise l'observateur dans l’espace universel, transformant les vertex en espace de caméra. Dans l'espace caméra, la caméra (ou l’observateur) se trouve à l’origine, dans la direction z positive. Pour plus d’informations, accédez à [Transformation de vue](../graphics-concepts/view-transform.md).

####  <a name="projection-transform-matrix"></a>Matrice de transformation de projection

La transformation de projection convertit le tronc de cône d’affichage en une forme cubique. Un tronc de cône d'affichage est un volume3D dans une scène positionnée par rapport à la fenêtre d’affichage de la caméra. Une fenêtre d’affichage est un rectangle 2D dans lequel une scène3D est projetée. Pour plus d’informations, voir [Fenêtres d’affichage et découpage](../graphics-concepts/viewports-and-clipping.md).

L’extrémité proche du tronc de cône d’affichage étant plus petite que son extrémité éloignée, cela a pour effet de développer les objets situés à proximité de la caméra; c’est de cette manière que la perspective est appliquée à la scène. Ainsi, les objets qui sont plus proches du joueur apparaissent plus gros et les objets qui sont éloignés apparaissent plus petits.

Mathématiquement, la transformation de projection est une matrice qui est généralement une projection avec une mise à l'échelle et une perspective. Elle fonctionne comme l’objectif d’un appareil photo. Pour plus d’informations, voir [Transformation de projection](../graphics-concepts/projection-transform.md).

### <a name="sampler-state"></a>État de l’échantillonneur

L'état de l’échantillonneur détermine comment les données sont échantillonnées à l’aide des modes d'adressage de texture, du filtrage et du niveau de détail. L'échantillonnage s’effectue chaque fois qu’un pixel de texture ou texel, est lu à partir d’une texture.

Une texture contient un tableau d’éléments de texture, ou pixels de texture. La position de chaque texel est indiquée par (u, v), où u est la largeur et v est la hauteur, et est mappée entre 0 et 1 en fonction de la largeur et de la hauteur de la texture. Les coordonnées de texture qui en résultent sont utilisées pour adresser un texel lors de l’échantillonnage d’une texture.

Lorsque les coordonnées de texture sont inférieures à 0 ou supérieures à 1, le mode d’adressage de texture définit la façon dont la coordonnée de texture adresse un emplacement texel. Par exemple, lorsque vous utilisez __TextureAddressMode.Clamp__, les coordonnées en dehors de la plage 0-1 sont ancrées à une valeur maximale 1 et à une valeur minimale 0 avant l’échantillonnage.

Si la texture est trop grande ou trop petite pour le polygone, elle est filtrée en fonction de l’espace. Un filtre d’agrandissement agrandit une texture, un filtre de réduction réduit la texture pour qu'elle tienne dans une zone plus petite. L'agrandissement de texture répète la texel d'exemple pour une ou plusieurs adresses ce qui donne une image floue. La réduction de texture est plus complexe, car elle requiert la combinaison de plusieurs valeurs de texel en une seule valeur. Cela peut entraîner des alias ou des bords crénelés en fonction des données de texture. La méthode la plus répandue de réduction consiste à utiliser un mipmap. Un mipmap est une texture à plusieurs niveaux. La taille de chaque niveau est inférieure par puissance de deux par rapport à la taille du niveau précédent de texture 1 x 1. Lors la réduction est utilisée, un jeu choisit le niveau de mipmap le plus proche de la taille requise au moment du rendu. 

### <a name="basicloader"></a>BasicLoader

__BasicLoader__ est une classe de chargeur simple qui prend en charge le chargement des nuanceurs, des textures et des maillages à partir des fichiers sur disque. Elle fournit des méthodes synchrones et asynchrones. Dans cet exemple de jeu, les fichiers BasicLoader.h/.cpp se trouvent dans le dossier __Commons__.

Pour plus d’informations, voir [BasicLoader](complete-code-for-basicloader.md).