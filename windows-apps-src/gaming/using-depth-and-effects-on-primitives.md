---
author: mtoepke
title: Utiliser la profondeur et les effets sur des primitives
description: Nous décrivons ici comment utiliser la profondeur, la perspective, la couleur et d’autres effets sur des primitives.
ms.assetid: 71ef34c5-b4a3-adae-5266-f86ba257482a
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, jeux, profondeur, effets, primitives, directx
ms.localizationpriority: medium
ms.openlocfilehash: f81c441910cd0d0205641a119c243cb22d0b695e
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7304841"
---
# <a name="use-depth-and-effects-on-primitives"></a>Utiliser la profondeur et les effets sur des primitives



Nous décrivons ici comment utiliser la profondeur, la perspective, la couleur et d’autres effets sur des primitives.

**Objectif :** créer un objet 3D et appliquer un éclairage de vertex de base et son coloriage.

## <a name="prerequisites"></a>Prérequis


Nous partons du principe que vous êtes familiarisé avec C++. Vous avez également besoin d’une expérience de base dans les concepts de programmation graphique.

Nous supposons en outre que vous avez suivi les rubriques [Démarrage rapide : configuration de ressources DirectX et affichage d’une image](setting-up-directx-resources.md) et [Création de nuanceurs et traçage de primitives](creating-shaders-and-drawing-primitives.md).

**Durée de réalisation :** 20 minutes.

<a name="instructions"></a>Instructions
------------

### <a name="1-defining-cube-variables"></a>1. Définition des variables du cube

Tout d’abord, nous devons définir les structures **SimpleCubeVertex** et **ConstantBuffer** pour le cube. Ces structures spécifient les positions et les couleurs de vertex pour le cube et le type d’affichage de ce dernier. Nous déclarons [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377) et [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351) à l’aide de [**ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx), et nous déclarons une instance de **ConstantBuffer**.

```cpp
struct SimpleCubeVertex
{
    DirectX::XMFLOAT3 pos;   // Position
    DirectX::XMFLOAT3 color; // Color
};

struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};

// This class defines the application as a whole.
ref class Direct3DTutorialFrameworkView : public IFrameworkView
{
private:
    Platform::Agile<CoreWindow> m_window;
    ComPtr<IDXGISwapChain1> m_swapChain;
    ComPtr<ID3D11Device1> m_d3dDevice;
    ComPtr<ID3D11DeviceContext1> m_d3dDeviceContext;
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    ComPtr<ID3D11DepthStencilView> m_depthStencilView;
    ComPtr<ID3D11Buffer> m_constantBuffer;
    ConstantBuffer m_constantBufferData;
```

### <a name="2-creating-a-depth-stencil-view"></a>2. Création d’une vue du stencil de profondeur

En plus de créer la vue de la cible de rendu, nous devons créer une vue du stencil de profondeur. La vue de profondeur/gabarit permet à Direct3D d’effectuer efficacement le rendu d’objets proches de la caméra devant des objets plus éloignés. Avant de pouvoir créer une vue appliquée à un tampon de stencil de profondeur, nous devons créer ce tampon. Nous remplissons une structure [**D3D11\_TEXTURE2D\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476253) pour décrire le tampon de stencil de profondeur, puis nous appelons [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521) pour créer ce tampon. Pour créer la vue du stencil de profondeur, nous remplissons une structure [**D3D11\_DEPTH\_STENCIL\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476112) afin de décrire la vue du stencil de profondeur, puis nous transmettons cette description et le tampon de stencil de profondeur à [**ID3D11Device::CreateDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476507).

```cpp
        // Once the render target view is created, create a depth stencil view.  This
        // allows Direct3D to efficiently render objects closer to the camera in front
        // of objects further from the camera.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_TEXTURE2D_DESC depthStencilDesc;
        depthStencilDesc.Width = backBufferDesc.Width;
        depthStencilDesc.Height = backBufferDesc.Height;
        depthStencilDesc.MipLevels = 1;
        depthStencilDesc.ArraySize = 1;
        depthStencilDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
        depthStencilDesc.SampleDesc.Count = 1;
        depthStencilDesc.SampleDesc.Quality = 0;
        depthStencilDesc.Usage = D3D11_USAGE_DEFAULT;
        depthStencilDesc.BindFlags = D3D11_BIND_DEPTH_STENCIL;
        depthStencilDesc.CPUAccessFlags = 0;
        depthStencilDesc.MiscFlags = 0;
        ComPtr<ID3D11Texture2D> depthStencil;
        DX::ThrowIfFailed(
            m_d3dDevice->CreateTexture2D(
                &depthStencilDesc,
                nullptr,
                &depthStencil
                )
            );

        D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
        depthStencilViewDesc.Format = depthStencilDesc.Format;
        depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
        depthStencilViewDesc.Flags = 0;
        depthStencilViewDesc.Texture2D.MipSlice = 0;
        DX::ThrowIfFailed(
            m_d3dDevice->CreateDepthStencilView(
                depthStencil.Get(),
                &depthStencilViewDesc,
                &m_depthStencilView
                )
            );
```

### <a name="3-updating-perspective-with-the-window"></a>3. Mise à jour de la perspective avec la fenêtre

Nous mettons à jour les paramètres de projection de la perspective pour la mémoire tampon constante en fonction des dimensions de la fenêtre. Nous corrigeons les paramètres pour appliquer un champ de vision de 70degrés avec une plage de profondeur de 0,01 à 100.

```cpp
        // Finally, update the constant buffer perspective projection parameters
        // to account for the size of the application window.  In this sample,
        // the parameters are fixed to a 70-degree field of view, with a depth
        // range of 0.01 to 100.  For a generalized camera class, see Lesson 5.

        float xScale = 1.42814801f;
        float yScale = 1.42814801f;
        if (backBufferDesc.Width > backBufferDesc.Height)
        {
            xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
        }
        else
        {
            yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
        }

        m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

### <a name="4-creating-vertex-and-pixel-shaders-with-color-elements"></a>4. Création de nuanceurs de vertex et de pixels à l’aide d’éléments de couleur

Pour l’application, nous créons des nuanceurs de vertex et de pixels plus complexes que ceux que nous avons décrits dans le didacticiel précédent, [Création de nuanceurs et traçage de primitives](creating-shaders-and-drawing-primitives.md). Le nuanceur de vertex de l’application transforme chaque position de vertex en espace de projection et transmet la couleur du vertex au nuanceur de pixels.

Le tableau des structures [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) de l’application qui décrivent la disposition du code pour les nuanceurs de vertex comporte deux éléments de disposition : l’un définit la position du vertex, et l’autre définit la couleur.

Nous créons les tampons vertex et d’index et la mémoire tampon constante pour définir un cube en orbite.

**Pour définir un cube en orbite**

1.  Nous commençons par définir le cube. Nous attribuons à chaque vertex une couleur en plus d’une position. Cela permet au nuanceur de pixels de colorer différemment chaque face pour les distinguer facilement.
2.  Ensuite, nous décrivons les tampons vertex et d’index ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) et [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)) à l’aide de la définition du cube. Nous appelons [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) une fois pour chaque tampon.
3.  Ensuite, nous créons une mémoire tampon constante ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092)) pour transmettre la matrice de modèle, la matrice globale et la matrice de projection au nuanceur de vertex. Nous pouvons ensuite utiliser le tampon constant pour faire pivoter le cube et lui appliquer une projection de perspective. Nous appelons [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) pour créer le tampon constant.
4.  Ensuite, nous indiquons la transformation de la vue correspondant à une position de caméra de X = 0, Y = 1, Z = 2.
5.  Enfin, nous déclarons une variable *degree* qui sert à animer le cube en le faisant pivoter à chaque image.

```cpp
        
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        
        auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode) {        
          ComPtr<ID3D11VertexShader> vertexShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateVertexShader(
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  nullptr,
                  &vertexShader
                  )
              );

          // Create an input layout that matches the layout defined in the vertex shader code.
          // For this lesson, this is simply a DirectX::XMFLOAT3 vector defining the vertex position, and
          // a DirectX::XMFLOAT3 vector defining the vertex color.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
          };

          ComPtr<ID3D11InputLayout> inputLayout;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateInputLayout(
                  basicVertexLayoutDesc,
                  ARRAYSIZE(basicVertexLayoutDesc),
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  &inputLayout
                  )
              );
        });
        
        
        // Load the raw pixel shader bytecode from disk and create a pixel shader with it.
        auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode) {
          ComPtr<ID3D11PixelShader> pixelShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreatePixelShader(
                  pixelShaderBytecode->Data,
                  pixelShaderBytecode->Length,
                  nullptr,
                  &pixelShader
                  )
              );
        });
        
        
        // Create vertex and index buffers that define a simple unit cube.
        auto createCubeTask = (createPSTask && createVSTask).then([this] () {

          // In the array below, which will be used to initialize the cube vertex buffers,
          // each vertex is assigned a color in addition to a position.  This will allow
          // the pixel shader to color each face differently, enabling them to be distinguished.
          SimpleCubeVertex cubeVertices[] =
          {
              { float3(-0.5f, 0.5f, -0.5f), float3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
              { float3( 0.5f, 0.5f, -0.5f), float3(1.0f, 1.0f, 0.0f) },
              { float3( 0.5f, 0.5f,  0.5f), float3(1.0f, 1.0f, 1.0f) },
              { float3(-0.5f, 0.5f,  0.5f), float3(0.0f, 1.0f, 1.0f) },

              { float3(-0.5f, -0.5f,  0.5f), float3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
              { float3( 0.5f, -0.5f,  0.5f), float3(1.0f, 0.0f, 1.0f) },
              { float3( 0.5f, -0.5f, -0.5f), float3(1.0f, 0.0f, 0.0f) },
              { float3(-0.5f, -0.5f, -0.5f), float3(0.0f, 0.0f, 0.0f) },
          };

          unsigned short cubeIndices[] =
          {
              0, 1, 2,
              0, 2, 3,

              4, 5, 6,
              4, 6, 7,

              3, 2, 5,
              3, 5, 4,

              2, 1, 6,
              2, 6, 5,

              1, 7, 6,
              1, 0, 7,

              0, 3, 4,
              0, 4, 7
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = cubeVertices;
          vertexBufferData.SysMemPitch = 0;
          vertexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> vertexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &vertexBufferDesc,
                  &vertexBufferData,
                  &vertexBuffer
                  )
              );

          D3D11_BUFFER_DESC indexBufferDesc;
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(cubeIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = cubeIndices;
          indexBufferData.SysMemPitch = 0;
          indexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> indexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &indexBufferDesc,
                  &indexBufferData,
                  &indexBuffer
                  )
              );


          // Create a constant buffer for passing model, view, and projection matrices
          // to the vertex shader.  This will allow us to rotate the cube and apply
          // a perspective projection to it.

          D3D11_BUFFER_DESC constantBufferDesc = {0};
          constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
          constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
          constantBufferDesc.CPUAccessFlags = 0;
          constantBufferDesc.MiscFlags = 0;
          constantBufferDesc.StructureByteStride = 0;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &constantBufferDesc,
                  nullptr,
                  &m_constantBuffer
                  )
              );

          // Specify the view transform corresponding to a camera position of
          // X = 0, Y = 1, Z = 2.  For a generalized camera class, see Lesson 5.

          m_constantBufferData.view = DirectX::XMFLOAT4X4(
              -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
               0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
               0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
               0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f
              );

        });
        
        // This value will be used to animate the cube by rotating it every frame.
        float degree = 0.0f;
        
```

### <a name="5-rotating-and-drawing-the-cube-and-presenting-the-rendered-image"></a>5. Rotation et dessin du cube, et présentation de l’image rendue

L’exécution du code entre dans une boucle sans fin pour effectuer le rendu de façon continue et afficher la scène. Nous appelons la fonction incluse **rotationY** (BasicMath.h) en indiquant une valeur de rotation pour définir les valeurs chargées de faire pivoter la matrice de modèle du cube sur l’axe Y. Nous appelons ensuite [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) pour mettre à jour la mémoire tampon constante et faire pivoter le modèle du cube. Nous appelons [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) pour préciser que la cible de sortie correspond à la cible de rendu. Au cours de cet appel d’**OMSetRenderTargets**, nous transmettons la vue du stencil de profondeur. Nous appelons [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388) pour effacer la cible de rendu et lui attribuer une couleur unie bleue, puis nous appelons [**ID3D11DeviceContext::ClearDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476387) pour effacer le tampon de profondeur.

Dans la boucle sans fin, nous traçons également le cube sur la surface bleue.

**Pour tracer le cube**

1.  Tout d’abord, nous appelons [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) pour décrire le flux des données du tampon vertex à l’étape de l’assembleur d’entrée.
2.  Ensuite, nous appelons [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) et [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) pour lier les tampons de vertex et d’index à l’étape de l’assembleur/entrée.
3.  Puis nous appelons [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) à l’aide de la valeur [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP) pour indiquer à l’étape de l’assembleur d’entrée d’interpréter les données de vertex comme une bande de triangles.
4.  Nous appelons [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) pour initialiser l’étape du nuanceur de vertex avec son code, et [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) pour initialiser l’étape du nuanceur de pixels avec le code correspondant.
5.  Ensuite, nous appelons [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491) pour définir le tampon constant utilisé par l’étape du pipeline de vertex shader.
6.  Enfin, nous appelons [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) pour tracer le cube et l’envoyer au pipeline de rendu.

Nous appelons [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) pour présenter l’image rendue dans la fenêtre.

```cpp
            // Update the constant buffer to rotate the cube model.
            m_constantBufferData.model = XMMatrixRotationY(-degree);
            degree += 1.0f;

            m_d3dDeviceContext->UpdateSubresource(
                m_constantBuffer.Get(),
                0,
                nullptr,
                &m_constantBufferData,
                0,
                0
                );

            // Specify the render target and depth stencil we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                m_depthStencilView.Get()
                );

            // Clear the render target to a solid color, and reset the depth stencil.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            m_d3dDeviceContext->ClearDepthStencilView(
                m_depthStencilView.Get(),
                D3D11_CLEAR_DEPTH,
                1.0f,
                0
                );

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(SimpleCubeVertex);
            UINT offset = 0;
            m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset
                );

            m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0
                );

            m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

            // Set the vertex and pixel shader stage state.
            m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0
                );

            m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf()
                );

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(cubeIndices),
                0,
                0
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
```

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes


Nous avons employé la profondeur, la perspective, la couleur et d’autres effets sur des primitives.

L’étape suivante consiste à appliquer des textures aux primitives.

[Application de textures aux primitives](applying-textures-to-primitives.md)

 

 




