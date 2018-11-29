---
title: Générer le rendu du mappage d’ombre dans le tampon de profondeur
description: Générez le rendu du point de vue de la lumière pour créer un mappage de profondeur en deux dimensions qui représente le volume de l’ombre.
ms.assetid: 7f3d0208-c379-8871-cc48-027047c6c2d0
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, rendu, mappage d’ombres, tampon de profondeur, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 27cd535dc51a330937c345acf352677a42c652eb
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7985103"
---
# <a name="render-the-shadow-map-to-the-depth-buffer"></a>Générer le rendu du mappage d’ombre dans le tampon de profondeur




Générez le rendu du point de vue de la lumière pour créer un mappage de profondeur en deux dimensions qui représente le volume de l’ombre. Le mappage de profondeur masque l’espace qui sera rendu dans l’ombre. Partie 2 de la [Procédure pas à pas : implémenter des volumes d’ombre à l’aide de tampons de profondeur dans Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="clear-the-depth-buffer"></a>Effacer le tampon de profondeur


Effacez toujours le tampon de profondeur avant d’y générer un rendu.

```cpp
context->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), DirectX::Colors::CornflowerBlue);
context->ClearDepthStencilView(m_shadowDepthView.Get(), D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);
```

## <a name="render-the-shadow-map-to-the-depth-buffer"></a>Générer le rendu du mappage d’ombre dans le tampon de profondeur


Pour la passe de rendu d’ombre, spécifiez un tampon de profondeur mais ne spécifiez pas de cible de rendu.

Spécifiez la fenêtre d’affichage de la lumière, un nuanceur de vertex et définissez les tampons constants de l’espace lumineux. Utilisez l’élimination de la face avant pour cette passe pour optimiser les valeurs de profondeur placées dans le tampon de l’ombre.

Notez que sur la plupart des périphériques, vous pouvez spécifier nullptr pour le nuanceur de pixels (ou évitez totalement de spécifier un nuanceur de pixels). Mais certains pilotes risquent de lever une exception quand vous appelez le dessin sur le périphérique Direct3D avec un jeu de nuanceurs de pixels null. Pour éviter cette exception, vous pouvez définir un nuanceur de pixels minimal pour la passe de rendu d’ombre. La sortie de ce nuanceur est rejetée ; il peut appeler [**discard**](https://msdn.microsoft.com/library/windows/desktop/bb943995) sur chaque pixel.

Générez le rendu des objets pouvant projeter des ombres, mais ne vous embêtez pas à générer le rendu de la géométrie qui ne peut pas en projeter (comme un sol dans une salle ou des objets supprimés de la passe d’ombre pour des raisons d’optimisation).

```cpp
void ShadowSceneRenderer::RenderShadowMap()
{
    auto context = m_deviceResources->GetD3DDeviceContext();

    // Render all the objects in the scene that can cast shadows onto themselves or onto other objects.

    // Only bind the ID3D11DepthStencilView for output.
    context->OMSetRenderTargets(
        0,
        nullptr,
        m_shadowDepthView.Get()
        );

    // Note that starting with the second frame, the previous call will display
    // warnings in VS debug output about forcing an unbind of the pixel shader
    // resource. This warning can be safely ignored when using shadow buffers
    // as demonstrated in this sample.

    // Set rendering state.
    context->RSSetState(m_shadowRenderState.Get());
    context->RSSetViewports(1, &m_shadowViewport);

    // Each vertex is one instance of the VertexPositionTexNormColor struct.
    UINT stride = sizeof(VertexPositionTexNormColor);
    UINT offset = 0;
    context->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset
        );

    context->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT, // Each index is one 16-bit unsigned integer (short).
        0
        );

    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->IASetInputLayout(m_inputLayout.Get());

    // Attach our vertex shader.
    context->VSSetShader(
        m_simpleVertexShader.Get(),
        nullptr,
        0
        );

    // Send the constant buffers to the Graphics device.
    context->VSSetConstantBuffers(
        0,
        1,
        m_lightViewProjectionBuffer.GetAddressOf()
        );

    context->VSSetConstantBuffers(
        1,
        1,
        m_rotatedModelBuffer.GetAddressOf()
        );

    // In some configurations, it's possible to avoid setting a pixel shader
    // (or set PS to nullptr). Not all drivers are tolerant of this, so to be
    // safe set a minimal shader here.
    //
    // Direct3D will discard output from this shader because the render target
    // view is unbound.
    context->PSSetShader(
        m_textureShader.Get(),
        nullptr,
        0
        );

    // Draw the objects.
    context->DrawIndexed(
        m_indexCountCube,
        0,
        0
        );
}
```

**Optimisez le tronc de cône de l’affichage:** assurez-vous que votre implémentation calcule un tronc de cône de l’affichage étroit afin d’obtenir le niveau de précision le plus élevé possible de votre tampon de profondeur. Voir [Techniques courantes pour améliorer les mappages de profondeur d’ombre](https://msdn.microsoft.com/library/windows/desktop/ee416324) pour obtenir plus de conseils sur la technique d’ombrage.

## <a name="vertex-shader-for-shadow-pass"></a>Nuanceur de vertex pour la passe d’ombre


Utilisez une version simplifiée de votre nuanceur de vertex pour générer uniquement le rendu de la position du vertex dans l’espace lumineux. N’incluez pas de normales d’éclairage, de transformations secondaires, etc.

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    return output;
}
```

Dans la partie suivante de cette procédure pas à pas, découvrez comment ajouter des ombres en [générant le rendu avec un test de profondeur](render-the-scene-with-depth-testing.md).

 

 




