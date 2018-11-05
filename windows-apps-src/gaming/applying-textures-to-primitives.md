---
author: mtoepke
title: Appliquer des textures aux primitives
description: Nous chargeons des données brutes, appliquées à une primitive 3D avec le cube créé à la rubrique Utilisation de la profondeur et d’effets sur des primitives.
ms.assetid: aeed09e3-c47a-4dd9-d0e8-d1b8bdd7e9b4
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, jeux, textures, directx
ms.localizationpriority: medium
ms.openlocfilehash: 252613bbea7f4cdb720758d3435cf0920dd93efa
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6030553"
---
# <a name="apply-textures-to-primitives"></a>Appliquer des textures aux primitives



Dans cette rubrique, nous chargeons des données de texture brutes et les appliquons à une primitive 3D à l’aide du cube que nous avons créé à la rubrique [Utilisation de la profondeur et d’effets sur des primitives](using-depth-and-effects-on-primitives.md). Nous introduisons aussi un modèle simple de produits/points d’éclairage, où la tonalité (plus clair ou plus sombre) des surfaces du cube se détermine en fonction de leur distance et de l’angle relatifs à une source de lumière.

**Objectif :** appliquer des textures aux primitives.

## <a name="prerequisites"></a>Prérequis


Nous partons du principe que vous êtes familiarisé avec C++. Vous avez également besoin d’une expérience de base dans les concepts de programmation graphique.

Nous supposons en outre que vous avez suivi la rubrique [Démarrage rapide : configuration de ressources DirectX et affichage d’une image](setting-up-directx-resources.md), [Création de nuanceurs et traçage de primitives](creating-shaders-and-drawing-primitives.md) et [Utilisation de la profondeur et d’effets sur des primitives](using-depth-and-effects-on-primitives.md).

**Durée de réalisation :** 20 minutes.

<a name="instructions"></a>Instructions
------------

### <a name="1-defining-variables-for-a-textured-cube"></a>1. Définition de variables pour un cube avec texture

Tout d’abord, nous devons définir les structures **BasicVertex** et **ConstantBuffer** pour le cube auquel est appliquée une texture. Ces structures spécifient les positions, les orientations et les textures de vertex pour le cube et le type d’affichage de ce dernier. À défaut, nous déclarons des variables comme nous l’avons fait dans le didacticiel précédent, [Utilisation de la profondeur et d’effets sur des primitives](using-depth-and-effects-on-primitives.md).

```cpp
struct BasicVertex
{
    DirectX::XMFLOAT3 pos;  // Position
    DirectX::XMFLOAT3 norm; // Surface normal vector
    DirectX::XMFLOAT2 tex;  // Texture coordinate
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

### <a name="2-creating-vertex-and-pixel-shaders-with-surface-and-texture-elements"></a>2. Création de nuanceurs de vertex et de nuanceurs de pixels par le biais d’éléments de surface et de texture

Nous créons ici des nuanceurs de vertex et nuanceurs de pixels plus complexes que ceux du didacticiel précédent, [Utilisation de la profondeur et d’effets sur des primitives](using-depth-and-effects-on-primitives.md). Le nuanceur de vertex de l’application transforme chaque position de vertex en espace de projection et passe la coordonnée de texture du vertex à travers le nuanceur de pixels.

Le tableau de structures [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) de l’application qui décrit la disposition du code du nuanceur de vertex inclut trois éléments de disposition : un élément définit la position du vertex, un autre définit le vecteur normal de la surface (c’est-à-dire l’orientation habituelle de la surface) et un troisième élément qui définit les coordonnées de texture.

Nous créons les tampons de vertex, d’index et constant qui définissent un cube avec texture en orbite.

**Pour définir un cube en orbite avec texture**

1.  Tout d’abord, définissez le cube. Chaque vertex se voit attribué une position, un vecteur normal de surface et des coordonnées de texture. Nous utilisons plusieurs vertex pour chaque coin dans le but de permettre à différents vecteurs normaux et coordonnées de texture d’être définis pour chaque face.
2.  Ensuite, nous décrivons les tampons vertex et d’index ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) et [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)) à l’aide de la définition du cube. Nous appelons [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) une fois pour chaque tampon.
3.  Ensuite, nous créons une mémoire tampon constante ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092)) pour transmettre la matrice de modèle, la matrice globale et la matrice de projection au nuanceur de vertex. Nous pouvons ensuite utiliser le tampon constant pour faire pivoter le cube et lui appliquer une projection de perspective. Nous appelons [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) pour créer la mémoire tampon constante.
4.  Enfin, nous indiquons la transformation de la vue correspondant à une position de caméra de X = 0, Y = 1, Z = 2.

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
          // These correspond to the elements of the BasicVertex struct defined above.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
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
          // multiple vertices are used for each corner to allow different normal vectors and
          // texture coordinates to be defined for each face.
          BasicVertex cubeVertices[] =
          {
              { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Y (top face)
              { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Y (bottom face)
              { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +X (right face)
              { DirectX::XMFLOAT3(0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -X (left face)
              { DirectX::XMFLOAT3(-0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Z (front face)
              { DirectX::XMFLOAT3( 0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3( 0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Z (back face)
              { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },
          };

          unsigned short cubeIndices[] =
          {
              0, 1, 2,
              0, 2, 3,

              4, 5, 6,
              4, 6, 7,

              8, 9, 10,
              8, 10, 11,

              12, 13, 14,
              12, 14, 15,

              16, 17, 18,
              16, 18, 19,

              20, 21, 22,
              20, 22, 23
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(BasicVertex) * ARRAYSIZE(cubeVertices);
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
```

### <a name="3-creating-textures-and-samplers"></a>3. Création de textures et d’échantillons

Dans cette rubrique, nous appliquons les données de texture à un cube plutôt que des couleurs comme dans le didacticiel précédent, [Utilisation de la profondeur et d’effets sur des primitives](using-depth-and-effects-on-primitives.md).

Nous exploitons les données de texture brutes pour créer des textures.

**Pour créer des textures et des échantillonneurs**

1.  Tout d’abord, nous effectuons la lecture des données de texture brutes du fichier texturedata.bin sur disque.
2.  Nous construisons ensuite une structure [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) qui fait référence aux données brutes de texture.
3.  Nous remplissons ensuite une structure [**D3D11\_TEXTURE2D\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476253) pour décrire la texture. Nous transmettons les structures [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) et **D3D11\_TEXTURE2D\_DESC** dans un appel à [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521) pour créer la texture.
4.  Ensuite, nous créons une vue de ressource/nuanceur de la texture de sorte que les nuanceurs puissent utiliser la texture. Pour créer la vue de ressource/nuanceur, nous remplissons une structure [**D3D11\_SHADER\_RESOURCE\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476211) pour décrire la vue et transmettre sa description et la texture à [**ID3D11Device::CreateShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476519). En règle générale, vous devez faire correspondre la description de la vue et celle de la texture.
5.  Nous créons ensuite un état d’échantillonneur pour la texture. Cet état d’échantillonneur utilise les données de texture appropriées pour indiquer comment la couleur d’une coordonnée de texture particulière se détermine. Nous remplissons une structure [**D3D11\_SAMPLER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476207) pour décrire l’état de l’échantillonneur. Nous transmettons ensuite la structure **D3D11\_SAMPLER\_DESC** dans un appel à [**ID3D11Device::CreateSamplerState**](https://msdn.microsoft.com/library/windows/desktop/ff476518) pour créer l’état de l’échantillonneur.
6.  Enfin, nous déclarons une variable *degree* qui sert à animer le cube en le faisant tourner à chaque image.

```cpp
        
        // Load the raw texture data from disk and construct a subresource description that references it.
        auto loadTDTask = DX::ReadDataAsync(L"texturedata.bin");
          
        auto constructSubresourceTask = loadTDTask.then([this](const std::vector<byte>& vertexShaderBytecode) {  
        
          D3D11_SUBRESOURCE_DATA textureSubresourceData = {0};
          textureSubresourceData.pSysMem = textureData->Data;

          // Specify the size of a row in bytes, known as a priori about the texture data.
          textureSubresourceData.SysMemPitch = 1024;

          // As this is not a texture array or 3D texture, this parameter is ignored.
          textureSubresourceData.SysMemSlicePitch = 0;

          // Create a texture description from information known as a priori about the data.
          // Generalized texture loading code can be found in the Resource Loading sample.
          D3D11_TEXTURE2D_DESC textureDesc = {0};
          textureDesc.Width = 256;
          textureDesc.Height = 256;
          textureDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
          textureDesc.Usage = D3D11_USAGE_DEFAULT;
          textureDesc.CPUAccessFlags = 0;
          textureDesc.MiscFlags = 0;

          // Most textures contain more than one MIP level.  For simplicity, this sample uses only one.
          textureDesc.MipLevels = 1;

          // As this will not be a texture array, this parameter is ignored.
          textureDesc.ArraySize = 1;

          // Don't use multi-sampling.
          textureDesc.SampleDesc.Count = 1;
          textureDesc.SampleDesc.Quality = 0;

          // Allow the texture to be bound as a shader resource.
          textureDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE;

          ComPtr<ID3D11Texture2D> texture;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateTexture2D(
                  &textureDesc,
                  &textureSubresourceData,
                  &texture
                  )
              );

          // Once the texture is created, we must create a shader resource view of it
          // so that shaders may use it.  In general, the view description will match
          // the texture description.
          D3D11_SHADER_RESOURCE_VIEW_DESC textureViewDesc;
          ZeroMemory(&textureViewDesc, sizeof(textureViewDesc));
          textureViewDesc.Format = textureDesc.Format;
          textureViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
          textureViewDesc.Texture2D.MipLevels = textureDesc.MipLevels;
          textureViewDesc.Texture2D.MostDetailedMip = 0;

          ComPtr<ID3D11ShaderResourceView> textureView;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateShaderResourceView(
                  texture.Get(),
                  &textureViewDesc,
                  &textureView
                  )
              );

          // Once the texture view is created, create a sampler.  This defines how the color
          // for a particular texture coordinate is determined using the relevant texture data.
          D3D11_SAMPLER_DESC samplerDesc;
          ZeroMemory(&samplerDesc, sizeof(samplerDesc));

          samplerDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;

          // The sampler does not use anisotropic filtering, so this parameter is ignored.
          samplerDesc.MaxAnisotropy = 0;

          // Specify how texture coordinates outside of the range 0..1 are resolved.
          samplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
          samplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
          samplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;

          // Use no special MIP clamping or bias.
          samplerDesc.MipLODBias = 0.0f;
          samplerDesc.MinLOD = 0;
          samplerDesc.MaxLOD = D3D11_FLOAT32_MAX;

          // Don't use a comparison function.
          samplerDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;

          // Border address mode is not used, so this parameter is ignored.
          samplerDesc.BorderColor[0] = 0.0f;
          samplerDesc.BorderColor[1] = 0.0f;
          samplerDesc.BorderColor[2] = 0.0f;
          samplerDesc.BorderColor[3] = 0.0f;

          ComPtr<ID3D11SamplerState> sampler;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateSamplerState(
                  &samplerDesc,
                  &sampler
                  )
              );
        });

        // This value will be used to animate the cube by rotating it every frame;
        float degree = 0.0f;
```

### <a name="4-rotating-and-drawing-the-textured-cube-and-presenting-the-rendered-image"></a>4. Rotation et dessin du cube avec texture, et présentation de l’image rendue

Comme dans les didacticiels précédents, l’exécution du code entre dans une boucle sans fin pour effectuer le rendu de façon continue et afficher la scène. Nous appelons la fonction inline **rotationY** (BasicMath.h) en indiquant une valeur de rotation pour définir les valeurs chargées de faire pivoter la matrice de modèle du cube sur l’axe Y. Nous appelons ensuite [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) pour mettre à jour le tampon constant et faire pivoter le modèle du cube. Ensuite, nous appelons [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) pour préciser la cible de sortie et la vue de profondeur/gabarit. Nous appelons [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388) pour effacer la cible de rendu et lui attribuer une couleur unie bleue, puis appelons [**ID3D11DeviceContext::ClearDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476387) pour effacer le tampon de profondeur.

Dans la boucle sans fin, nous traçons également le cube avec texture sur la surface bleue.

**Pour tracer le cube avec texture**

1.  Tout d’abord, nous appelons [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) pour décrire le flux des données du tampon de vertex à l’étape de l’assembleur/entrée.
2.  Ensuite, nous appelons [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) et [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) pour lier les tampons de vertex et d’index à l’étape de l’assembleur/entrée.
3.  Puis nous appelons [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) à l’aide de la valeur [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP) pour indiquer à l’étape de l’assembleur d’entrée d’interpréter les données de vertex comme une bande de triangles.
4.  Nous appelons [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) pour initialiser l’étape du nuanceur de vertex avec son code, et [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) pour initialiser l’étape du nuanceur de pixels avec le code correspondant.
5.  Ensuite, nous appelons [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491) pour définir le tampon constant utilisé par l’étape du pipeline de nuanceur de vertex.
6.  Nous appelons [**PSSetShaderResources**](https://msdn.microsoft.com/library/windows/desktop/ff476473) pour établir la liaison entre la vue de ressource/nuanceur de la texture et l’étape du pipeline du nuanceur de pixels.
7.  Nous appelons [**PSSetSamplers**](https://msdn.microsoft.com/library/windows/desktop/ff476471) pour attribuer l’étape du pipeline du nuanceur de pixels à l’état de l’échantillonneur.
8.  Enfin, nous appelons [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) pour tracer le cube et l’envoyer au pipeline de rendu.

Comme nous l’avons vu dans les didacticiels précédents, nous appelons [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) pour présenter l’image rendue dans la fenêtre.

```cpp
            // Update the constant buffer to rotate the cube model.
            m_constantBufferData.model = DirectX::XMMatrixRotationY(-degree);
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
            UINT stride = sizeof(BasicVertex);
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

            m_d3dDeviceContext->PSSetShaderResources(
                0,
                1,
                textureView.GetAddressOf()
                );

            m_d3dDeviceContext->PSSetSamplers(
                0,
                1,
                sampler.GetAddressOf()
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

## <a name="summary"></a>Récapitulatif


Nous avons chargé les données de texture brutes et les avons appliquées à une primitive3D.

 

 




