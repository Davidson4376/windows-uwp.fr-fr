---
title: Créer des ressources de périphérique pour un tampon de profondeur
description: Découvrez comment créer les ressources de périphérique Direct3D nécessaires pour prendre en charge le test de profondeur des volumes d’ombre.
ms.assetid: 86d5791b-1faa-17e4-44a8-bbba07062756
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, jeux, direct3d, tampon de profondeur
ms.localizationpriority: medium
ms.openlocfilehash: f5ce1ec522a194111e175e41f82c4275cda4fbf5
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8326839"
---
# <a name="create-depth-buffer-device-resources"></a>Créer des ressources de périphérique pour un tampon de profondeur




Découvrez comment créer les ressources de périphérique Direct3D nécessaires pour prendre en charge le test de profondeur des volumes d’ombre. Partie 1 de la [Procédure pas à pas : implémenter des volumes d’ombre à l’aide de tampons de profondeur dans Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="resources-youll-need"></a>Ressources nécessaires


Le rendu d’un plan de profondeur pour les volumes d’ombre nécessite les ressources suivantes liées aux périphériques Direct3D :

-   une ressource (tampon) pour le plan de profondeur ;
-   une vue de profondeur/gabarit et une vue de ressource de nuanceur pour la ressource ;
-   un objet d’état de l’échantillonneur de comparaison ;
-   des tampons constants pour matrices PdV légères ;
-   une fenêtre d’affichage pour le rendu du plan d’ombres (généralement une fenêtre d’affichage carrée) ;
-   un objet d’état de rendu pour activer l’élimination de la face avant.
-   Vous aurez aussi besoin d’un objet d’état de rendu pour revenir à l’élimination de la face arrière, si ce n’est pas déjà le cas.

Notez que la création de ces ressources doit être incluse dans une routine de création de ressources liées au périphérique. De cette façon, votre convertisseur peut les recréer si (par exemple) un nouveau pilote de périphérique est installé ou si l’utilisateur déplace votre application sur un moniteur associé à une autre carte graphique.

## <a name="check-feature-support"></a>Vérifier la prise en charge des fonctionnalités


Avant de créer le plan de profondeur, appelez la méthode [**CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497) sur le périphérique Direct3D, demandez **D3D11\_FEATURE\_D3D9\_SHADOW\_SUPPORT** et fournissez une structure [**D3D11\_FEATURE\_DATA\_D3D9\_SHADOW\_SUPPORT**](https://msdn.microsoft.com/library/windows/desktop/jj247569).

```cpp
D3D11_FEATURE_DATA_D3D9_SHADOW_SUPPORT isD3D9ShadowSupported;
ZeroMemory(&isD3D9ShadowSupported, sizeof(isD3D9ShadowSupported));
pD3DDevice->CheckFeatureSupport(
    D3D11_FEATURE_D3D9_SHADOW_SUPPORT,
    &isD3D9ShadowSupported,
    sizeof(D3D11_FEATURE_D3D9_SHADOW_SUPPORT)
    );

if (isD3D9ShadowSupported.SupportsDepthAsTextureWithLessEqualComparisonFilter)
{
    // Init shadow map resources

```

Si cette fonctionnalité n’est pas prise en charge, n’essayez pas de charger des nuanceurs compilés pour le modèle de nuanceur 4 niveau 9\_x, lesquels appellent des fonctions de comparaison d’exemples. Dans de nombreux cas, l’absence de prise en charge de cette fonctionnalité signifie que le GPU est un périphérique hérité doté d’un pilote qui n’est pas mis à jour pour prendre en charge au moins WDDM 1.2. Si le périphérique prend en charge au moins le niveau de fonctionnalité 10\_0, alors vous pouvez charger un nuanceur de comparaison d’exemples compilé pour le modèle de nuanceur 4\_0 à la place.

## <a name="create-depth-buffer"></a>Créer un tampon de profondeur


Pour commencer, essayez de créer le plan de profondeur dans un format de profondeur haute précision. Configurez d’abord les propriétés de la vue de ressource de nuanceur correspondantes. Si la création de ressource échoue, par exemple en raison d’un manque de mémoire du périphérique ou d’un format non pris en charge par le matériel, essayez un format de moindre précision et modifiez la correspondance des propriétés.

Cette étape est facultative si vous avez seulement besoin d’un format de profondeur avec une faible précision, par exemple quand vous effectuez le rendu sur des périphériques Direct3D de résolution moyenne avec le niveau de fonctionnalité 9\_1.

```cpp
D3D11_TEXTURE2D_DESC shadowMapDesc;
ZeroMemory(&shadowMapDesc, sizeof(D3D11_TEXTURE2D_DESC));
shadowMapDesc.Format = DXGI_FORMAT_R24G8_TYPELESS;
shadowMapDesc.MipLevels = 1;
shadowMapDesc.ArraySize = 1;
shadowMapDesc.SampleDesc.Count = 1;
shadowMapDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE | D3D11_BIND_DEPTH_STENCIL;
shadowMapDesc.Height = static_cast<UINT>(m_shadowMapDimension);
shadowMapDesc.Width = static_cast<UINT>(m_shadowMapDimension);

HRESULT hr = pD3DDevice->CreateTexture2D(
    &shadowMapDesc,
    nullptr,
    &m_shadowMap
    );
```

Ensuite, créez les vues de ressource. Définissez la tranche MIP sur zéro dans la vue de profondeur/gabarit et les niveaux MIP sur 1 dans la vue de ressource du nuanceur. Les deux vues ont une dimension de texture TEXTURE2D et doivent utiliser un [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059) correspondant.

```cpp
D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
ZeroMemory(&depthStencilViewDesc, sizeof(D3D11_DEPTH_STENCIL_VIEW_DESC));
depthStencilViewDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
depthStencilViewDesc.Texture2D.MipSlice = 0;

D3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc;
ZeroMemory(&shaderResourceViewDesc, sizeof(D3D11_SHADER_RESOURCE_VIEW_DESC));
shaderResourceViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
shaderResourceViewDesc.Format = DXGI_FORMAT_R24_UNORM_X8_TYPELESS;
shaderResourceViewDesc.Texture2D.MipLevels = 1;

hr = pD3DDevice->CreateDepthStencilView(
    m_shadowMap.Get(),
    &depthStencilViewDesc,
    &m_shadowDepthView
    );

hr = pD3DDevice->CreateShaderResourceView(
    m_shadowMap.Get(),
    &shaderResourceViewDesc,
    &m_shadowResourceView
    );
```

## <a name="create-comparison-state"></a>Créer un état de comparaison


À présent, créez l’objet d’état de l’échantillonneur de comparaison. Le niveau de fonctionnalité 9\_1 prend uniquement en charge D3D11\_COMPARISON\_LESS\_EQUAL. Les options de filtre sont expliquées plus en détail dans [Prise en charge des plans d’ombres sur une gamme de matériels](target-a-range-of-hardware.md) ou vous pouvez choisir le filtrage de points pour des plans d’ombres plus rapides.

Notez que vous pouvez spécifier le mode d’adresse D3D11\_TEXTURE\_ADDRESS\_BORDER pour qu’il fonctionne sur les périphériques de niveau de fonctionnalité 9\_1. Cela s’applique aux nuanceurs de pixels qui ne testent pas si le pixel figure dans le tronc de cône (frustrum) de vue de la lumière avant d’effectuer le test de profondeur. En définissant chaque bordure sur 0 ou 1, vous pouvez contrôler si les pixels en dehors du tronc de cône de vue de la lumière réussissent ou échouent au test de profondeur, et par conséquent s’ils sont éclairés ou dans l’ombre.

Sur le niveau de fonctionnalité 9\_1, les valeurs requises suivantes doivent être définies comme suit : **MinLOD** sur zéro, **MaxLOD** sur **D3D11\_FLOAT32\_MAX** et **MaxAnisotropy** sur zéro.

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

## <a name="create-render-states"></a>Créer des états de rendu


Créez maintenant un état de rendu servant à activer l’élimination des faces avant. Notez que les périphériques de niveau de fonctionnalité 9\_1 requièrent que **DepthClipEnable** soit défini sur **true**.

```cpp
D3D11_RASTERIZER_DESC drawingRenderStateDesc;
ZeroMemory(&drawingRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
drawingRenderStateDesc.CullMode = D3D11_CULL_BACK;
drawingRenderStateDesc.FillMode = D3D11_FILL_SOLID;
drawingRenderStateDesc.DepthClipEnable = true; // Feature level 9_1 requires DepthClipEnable == true
DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &drawingRenderStateDesc,
        &m_drawingRenderState
        )
    );
```

Créez un état de rendu servant à activer l’élimination des faces arrière. Si votre code de rendu active déjà l’élimination des faces arrière, vous pouvez ignorer cette étape.

```cpp
D3D11_RASTERIZER_DESC shadowRenderStateDesc;
ZeroMemory(&shadowRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
shadowRenderStateDesc.CullMode = D3D11_CULL_FRONT;
shadowRenderStateDesc.FillMode = D3D11_FILL_SOLID;
shadowRenderStateDesc.DepthClipEnable = true;

DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &shadowRenderStateDesc,
        &m_shadowRenderState
        )
    );
```

## <a name="create-constant-buffers"></a>Créer des tampons constants


N’oubliez pas de créer un tampon constant pour le rendu du point de vue de la lumière. Vous pouvez aussi l’utiliser pour spécifier au nuanceur la position de la lumière. Utilisez une matrice de perspective pour les lumières à points et une matrice orthogonale pour les lumières directionnelles (comme les rayons du soleil).

```cpp
DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &viewProjectionConstantBufferDesc,
        nullptr,
        &m_lightViewProjectionBuffer
        )
    );
```

Renseignez les données de tampon constant. Mettez à jour les tampons constants une fois pendant l’initialisation, puis à nouveau si les valeurs de lumière ont changé depuis la trame précédente.

```cpp
{
    XMMATRIX lightPerspectiveMatrix = XMMatrixPerspectiveFovRH(
        XM_PIDIV2,
        1.0f,
        12.f,
        50.f
        );

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.projection,
        XMMatrixTranspose(lightPerspectiveMatrix)
        );

    // Point light at (20, 15, 20), pointed at the origin. POV up-vector is along the y-axis.
    static const XMVECTORF32 eye = { 20.0f, 15.0f, 20.0f, 0.0f };
    static const XMVECTORF32 at = { 0.0f, 0.0f, 0.0f, 0.0f };
    static const XMVECTORF32 up = { 0.0f, 1.0f, 0.0f, 0.0f };

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.view,
        XMMatrixTranspose(XMMatrixLookAtRH(eye, at, up))
        );

    // Store the light position to help calculate the shadow offset.
    XMStoreFloat4(&m_lightViewProjectionBufferData.pos, eye);
}
```

```cpp
context->UpdateSubresource(
    m_lightViewProjectionBuffer.Get(),
    0,
    NULL,
    &m_lightViewProjectionBufferData,
    0,
    0
    );
```

## <a name="create-a-viewport"></a>Créer une fenêtre d’affichage


Vous avez besoin d’une fenêtre d’affichage distincte pour restituer un plan d’ombres. La fenêtre d’affichage n’est pas une ressource basée sur un périphérique, vous êtes donc libre de la créer ailleurs dans votre code. La création de la fenêtre d’affichage avec le plan d’ombres peut faciliter la mise en relation de leurs dimensions respectives.

```cpp
// Init viewport for shadow rendering
ZeroMemory(&m_shadowViewport, sizeof(D3D11_VIEWPORT));
m_shadowViewport.Height = m_shadowMapDimension;
m_shadowViewport.Width = m_shadowMapDimension;
m_shadowViewport.MinDepth = 0.f;
m_shadowViewport.MaxDepth = 1.f;
```

Dans la suite de cette procédure pas à pas, vous allez découvrir comment créer le plan d’ombres en [effectuant le rendu dans le tampon de profondeur](render-the-shadow-map-to-the-depth-buffer.md).

 

 




