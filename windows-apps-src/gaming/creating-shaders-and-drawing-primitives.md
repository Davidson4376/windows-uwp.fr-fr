---
title: Créer des nuanceurs et tracer des primitives
description: Nous vous montrons ici comment utiliser les fichiers HLSL sources pour compiler et créer des nuanceurs que vous pouvez utiliser ensuite pour tracer des primitives à l’écran.
ms.assetid: 91113bbe-96c9-4ef9-6482-39f1ff1a70f4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, jeux, nuanceurs, primitives, directx
ms.localizationpriority: medium
ms.openlocfilehash: 5173adc26e0730ccb80f93fe0c12af286b0c1a49
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8782727"
---
# <a name="create-shaders-and-drawing-primitives"></a>Créer des nuanceurs et tracer des primitives



Nous vous montrons ici comment utiliser les fichiers HLSL sources pour compiler et créer des nuanceurs que vous pouvez ensuite utiliser pour tracer des primitives à l’écran.

Nous créons et traçons un triangle jaune à l’aide de nuanceurs de vertex et de pixels. Après avoir créé le périphérique Direct3D, la chaîne de permutation et la vue de cible de rendu, nous effectuons la lecture des données des fichiers binaires d’objets des nuanceurs sur le disque.

**Objectif :** créer les nuanceurs et tracer des primitives.

## <a name="prerequisites"></a>Prérequis


Nous partons du principe que vous êtes familiarisé avec C++. Vous avez également besoin d’une expérience de base dans les concepts de programmation graphique.

Nous supposons en outre que vous avez suivi la rubrique [Démarrage rapide : configuration de ressources DirectX et affichage d’une image](setting-up-directx-resources.md).

**Durée de réalisation :** 20 minutes

## <a name="instructions"></a>Instructions

### <a name="1-compiling-hlsl-source-files"></a>1. Compilation de fichiers sources HLSL

Microsoft Visual Studio utilise le compilateur de code HLSL [fxc.exe](https://msdn.microsoft.com/library/windows/desktop/bb232919) pour compiler les fichiers sources .hlsl (SimpleVertexShader.hlsl et SimplePixelShader.hlsl) en fichiers objets .cso de nuanceur binaire (SimpleVertexShader.cso et SimplePixelShader.cso). Pour plus d’informations sur le compilateur de code HLSL, voir Outil compilateur d’effet. Pour plus d’informations sur la compilation de code de nuanceurs, voir [Compilation de nuanceurs](https://msdn.microsoft.com/library/windows/desktop/bb509633).

Le code suivant correspond au fichier SimpleVertexShader.hlsl:

```hlsl
struct VertexShaderInput
{
    DirectX::XMFLOAT2 pos : POSITION;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // For this lesson, set the vertex depth value to 0.5, so it is guaranteed to be drawn.
    vertexShaderOutput.pos = float4(input.pos, 0.5f, 1.0f);

    return vertexShaderOutput;
}
```

Le code suivant correspond au fichier SimplePixelShader.hlsl:

```hlsl
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

### <a name="2-reading-data-from-disk"></a>2. Lecture de données sur disque

Nous utilisons la fonction DX::ReadDataAsync de DirectXHelper.h dans le modèle d’application DirectX 11 (Windows universel) pour lire de façon asynchrone des données à partir d’un fichier sur le disque.

### <a name="3-creating-vertex-and-pixel-shaders"></a>3. Création de nuanceurs de vertex et de pixels

Nous effectuons la lecture des données du fichier SimpleVertexShader.cso et les attribuons au tableau d’octets *vertexShaderBytecode*. Nous appelons [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) avec un tableau d’octets pour créer le nuanceur de vertex ([**ID3D11VertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476641)). Nous attribuons la valeur 0,5 à la profondeur du vertex dans le code source SimpleVertexShader.hlsl pour nous assurer que notre triangle se trace bien. Nous remplissons un tableau de structures [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) pour décrire la disposition du code du nuanceur de vertex, puis nous appelons [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) pour créer la disposition. Le tableau possède un élément de disposition qui définit la position du vertex. Nous effectuons la lecture des données du fichier SimplePixelShader.cso et les attribuons au tableau d’octets *pixelShaderBytecode*. Nous appelons [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) avec un tableau d’octets pour créer le nuanceur de pixels ([**ID3D11PixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476576)). Nous définissons la valeur de pixel sur (1,1,1,1) dans le code source SimplePixelShader.hlsl pour changer la couleur de notre triangle au jaune. Vous pouvez changer la couleur en modifiant cette valeur.

Nous créons des tampons de vertex et d’index qui définissent un simple triangle. Pour ce faire, nous définissons tout d’abord le triangle, nous décrivons ensuite les tampons de vertex et d’index ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) et [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)) à l’aide de la définition du triangle, puis nous appelons enfin [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) une fois pour chaque tampon.

```cpp
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        // Load the raw vertex shader bytecode from disk and create a vertex shader with it.
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
          // For this lesson, this is simply a DirectX::XMFLOAT2 vector defining the vertex position.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
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

        // Create vertex and index buffers that define a simple triangle.
        auto createTriangleTask = (createPSTask && createVSTask).then([this] () {

          DirectX::XMFLOAT2 triangleVertices[] =
          {
              float2(-0.5f, -0.5f),
              float2( 0.0f,  0.5f),
              float2( 0.5f, -0.5f),
          };

          unsigned short triangleIndices[] =
          {
              0, 1, 2,
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(float2) * ARRAYSIZE(triangleVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = triangleVertices;
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
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(triangleIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = triangleIndices;
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
        });
```

Nous faisons appel aux nuanceurs de vertex et aux nuanceurs de pixels, à la disposition des nuanceurs de vertex et aux tampons de vertex et d’index pour tracer un triangle jaune.

### <a name="4-drawing-the-triangle-and-presenting-the-rendered-image"></a>4. Traçage du triangle et présentation de l’image rendue

L’exécution du code entre dans une boucle sans fin pour effectuer le rendu de façon continue et afficher la scène. Nous appelons [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) pour préciser que la cible de sortie correspond à la cible de rendu. Nous appelons [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388) avec { 0.071f, 0.04f, 0.561f, 1.0f } pour remplacer la cible de rendu par une couleur bleue unie.

Dans la boucle sans fin, nous traçons un triangle jaune sur la surface bleue.

**Pour tracer un triangle jaune**

1.  Tout d’abord, nous appelons [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) pour décrire le flux des données du tampon de vertex à l’étape de l’assembleur/entrée.
2.  Ensuite, nous appelons [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) et [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) pour lier les tampons de vertex et d’index à l’étape de l’assembleur/entrée.
3.  Puis nous appelons [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) à l’aide de la valeur [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP) pour indiquer à l’étape de l’assembleur d’entrée d’interpréter les données de vertex comme une bande de triangles.
4.  Nous appelons [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) pour initialiser l’étape du nuanceur de vertex avec son code, et [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) pour initialiser l’étape du nuanceur de pixels avec le code correspondant.
5.  Enfin, nous appelons [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) pour tracer le triangle et l’envoyer au pipeline de rendu.

Nous appelons [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) pour présenter l’image rendue dans la fenêtre.

```cpp
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

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(float2);
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

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(triangleIndices),
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


Nous avons créé et tracé un triangle jaune à l’aide de nuanceurs de vertex et de pixels.

Nous allons à présent créer un cube 3D en orbite et lui appliquer des effets d’éclairage.

[Utilisation de la profondeur et d’effets sur des primitives](using-depth-and-effects-on-primitives.md)

 

 




