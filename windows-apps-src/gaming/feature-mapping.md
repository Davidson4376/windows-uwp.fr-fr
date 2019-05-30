---
title: Mapper les fonctionnalités DirectX 9 aux API DirectX 11
description: Découvrez comment les fonctionnalités utilisées par votre jeu Direct3D 9 sont traduites dans Direct3D 11 et la plateforme Windows universelle (UWP).
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, directx 9, directx 11, portage
ms.localizationpriority: medium
ms.openlocfilehash: 51bc293a779a96db75ce83da68cb3beea54b9618
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368725"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>Mapper les fonctionnalités DirectX 9 aux API DirectX 11



**Résumé**

-   [Planifier votre port DirectX](plan-your-directx-port.md)
-   [Modifications importantes par rapport à partir de Direct3D 9 à Direct3D 11](understand-direct3d-11-1-concepts.md)
-   Mappage des fonctionnalités


Découvrez comment les fonctionnalités utilisées par votre jeu Direct3D 9 sont traduites dans Direct3D 11 et la plateforme Windows universelle (UWP).

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>Mappage des API Direct3D 9 aux API DirectX 11


[Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) reste la base des graphismes DirectX, mais l’API a changé depuis DirectX 9 :

-   L’infrastructure DXGI (Microsoft DirectX Graphics Infrastructure) sert à configurer les cartes graphiques. Utilisez [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi) pour sélectionner le format des tampons, créer des chaînes d’échange, présenter des trames et créer des ressources partagées. Voir [Vue d’ensemble de DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi).
-   Un contexte de périphérique Direct3D permet de définir l’état du pipeline et de générer des commandes de rendu. La plupart de nos exemples utilisent un contexte immédiat pour effectuer un rendu directement sur le périphérique. Direct3D 11 prend aussi en charge le rendu multithread associé aux contextes différés. Voir [Présentation d’un périphérique dans Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro).
-   Certaines fonctionnalités ont été abandonnées, notamment le pipeline à fonctions fixes. Voir [Fonctionnalités obsolètes](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated).

Pour obtenir la liste complète des fonctionnalités de Direct3D 11, voir [Fonctionnalités de Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-features) et [Fonctionnalités de Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-1-features).

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>Passage de Direct2D 9 à Direct2D 11


[Direct2D (Windows)](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal) occupe toujours une place importante dans les graphismes DirectX et Windows. Vous pouvez l’utiliser pour dessiner des jeux en 2D et des superpositions (affichages à tête haute) par-dessus Direct3D.

Direct2D s’exécute par-dessus Direct3D. Les jeux en 2D peuvent être implémentés à l’aide de l’une ou l’autre des API. Par exemple, un jeu en 2D implémenté avec Direct3D peut utiliser la projection orthographique, définir des valeurs Z pour contrôler l’ordre de tracé des primitives et utiliser des nuanceurs de pixels pour ajouter des effets spéciaux.

Dans la mesure où Direct2D repose sur Direct3D, il utilise aussi DXGI et les contextes de périphérique. Voir [Vue d’ensemble des API de Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/the-direct2d-api).

L’API [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal) permet la prise en charge du texte mis en forme avec Direct2D. Voir [Présentation de DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/introducing-directwrite).

## <a name="replace-deprecated-helper-libraries"></a>Remplacer les bibliothèques d’assistance déconseillées


D3DX et DXUT sont déconseillés et ne peuvent pas être utilisés par des jeux UWP. Ces bibliothèques d’assistance fournissaient des ressources pour les tâches comme le chargement des textures et des maillages.

-   La procédure pas à pas [Portage simple de Direct3D 9 vers UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) décrit les méthodes permettant de configurer une fenêtre, d’initialiser Direct3D et d’effectuer un rendu de base en 3D.
-   La procédure pas à pas [Jeu UWP simple avec DirectX](tutorial--create-your-first-uwp-directx-game.md) décrit les tâches communes de programmation de jeux, notamment les graphismes, le chargement de fichiers, l’interface utilisateur, les commandes et le son.
-   Le projet de la communauté [Kit de ressources DirectX](https://go.microsoft.com/fwlink/p/?LinkID=248929) propose des classes d’assistance pour Direct3D 11 et les applications UWP.

## <a name="move-shader-programs-from-fx-to-hlsl"></a>Déplacer les programmes de nuanceurs de FX à HLSL


La bibliothèque des utilitaires D3DX (D3DX 9, D3DX 10 et D3DX 11), y compris Effects, est déconseillée pour UWP. Tous les jeux DirectX pour UWP pilotent le pipeline graphique à l’aide de [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl) sans la bibliothèque d’effets.

Visual Studio utilise toujours FXC pour compiler les objets nuanceurs. Les nuanceurs UWP sont compilés à l’avance. Le bytecode est chargé à l’exécution, puis chaque ressource de nuanceur est liée au pipeline graphique pendant l’étape de rendu appropriée. Les nuanceurs doivent être déplacés vers leurs propres fichiers .HLSL et des techniques de rendu doivent être implémentées dans votre code C++.

Pour un aperçu du chargement des ressources de nuanceur, voir [Portage simple de Direct3D 9 vers UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

Direct3D 11 introduit 5 de modèle de nuanceur, ce qui nécessite le niveau de fonctionnalité Direct3D 11\_0 (ou version ultérieure). Voir [Fonctionnalités du modèle de nuanceur 5 HLSL pour Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3dhlsl/overviews-direct3d-11-hlsl).

## <a name="replace-xnamath-and-d3dxmath"></a>Remplacer XNAMath et D3DXMath


Le code qui utilise XNAMath (ou D3DXMath) doit être migré vers [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal). DirectXMath inclut des types pouvant être portés sur des processeurs x86, x64 et ARM. Voir [Migration du code de la bibliothèque XNA Math](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration).

Notez que les types float DirectXMath conviennent parfaitement aux nuanceurs. Par exemple, [**XMFLOAT4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) et [**XMFLOAT4X4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) alignent des données pour les mémoires tampons constantes.

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>Remplacer DirectSound par XAudio2 (et l’audio en arrière-plan)


DirectSound n’est pas pris en charge pour UWP :

-   Utilisez [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) pour ajouter des effets sonores à votre jeu.

##  <a name="replace-directinput-with-xinput-and-uwp-apis"></a>Remplacer DirectInput par XInput et les API UWP


DirectInput n’est pas pris en charge pour UWP :

-   Utilisez les rappels d’événement d’entrée [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) pour la souris, le clavier et les entrées tactiles.
-   Utilisez [XInput](https://docs.microsoft.com/windows/desktop/xinput/getting-started-with-xinput) 1.4 pour la prise en charge des contrôleurs de jeu (et la prise en charge du casque des contrôleurs de jeu). Si vous utilisez une base de code partagé pour les applications de bureau et UWP, voir [Versions XInput](https://docs.microsoft.com/windows/desktop/xinput/xinput-versions) pour plus d’informations sur la compatibilité descendante.
-   Inscrivez-vous aux événements [**EdgeGesture**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.EdgeGesture) si votre jeu doit utiliser la barre de l’application.

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>Utiliser Microsoft Media Foundation au lieu de DirectShow


DirectShow ne fait plus partie de l’API DirectX (ou de l’API Windows). [Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) fournit le contenu vidéo à Direct3D via les surfaces partagées. Voir [API vidéo de Direct3D 11](https://docs.microsoft.com/windows/desktop/medfound/direct3d-11-video-apis).

## <a name="replace-directplay-with-networking-code"></a>Remplacer DirectPlay par du code réseau


Microsoft DirectPlay est déconseillé. Si votre jeu utilise des services réseau, vous devez fournir du code réseau conforme aux exigences UWP. Utilisez les API suivantes :

-   [Win32 et COM pour les applications UWP (Windows) (mise en réseau)](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)
-   [**Espace de noms Windows.Networking (Windows)** ](https://docs.microsoft.com/uwp/api/Windows.Networking)
-   [**Espace de noms Windows.Networking.Sockets (Windows)** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets)
-   [**Espace de noms Windows.Networking.Connectivity (Windows)** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity)
-   [**Espace de noms Windows.ApplicationModel.Background (Windows)** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)

Les articles suivants vous aident à ajouter des fonctionnalités réseau et à déclarer la prise en charge réseau dans le manifeste du package de votre application.

-   [Connexion avec des sockets (applications UWP à l’aide de C#/VB/C++ et XAML) (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh452976(v=win.10))
-   [Connexion avec les WebSockets (applications UWP à l’aide de C#/VB/C++ et XAML) (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh994396(v=win.10))
-   [Connexion aux services web (applications UWP à l’aide de C#/VB/C++ et XAML) (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh761504(v=win.10))
-   [Notions de base relatives aux réseaux](https://docs.microsoft.com/windows/uwp/networking/networking-basics)

Notez que toutes les applications pour UWP (y compris les jeux) utilisent des types spécifiques de tâches en arrière-plan pour maintenir la connectivité pendant la suspension de l’application. Si votre jeu doit rester en état de connexion pendant sa suspension, voir [Notions de base en matière de réseau](https://docs.microsoft.com/windows/uwp/networking/networking-basics).

## <a name="function-mapping"></a>Mappage de fonctions


Aidez-vous du tableau suivant pour convertir du code Direct3D 9 en Direct3D 11. Il peut aussi vous permettre de faire la distinction entre le périphérique et le contexte de périphérique.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D 9</th>
<th align="left">Équivalent Direct3D 11</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3ddevice9">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2">ID3D11Device2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11devicecontext2">ID3D11DeviceContext2</a></p>
<p>Les étapes du pipeline graphique sont décrites dans <a href="https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline">Pipeline graphique</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9">IDirect3D9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2">IDXGIFactory2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2">IDXGIAdapter2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nn-dxgi1_3-idxgidevice3">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-present">IDirect3DDevice9::Present</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-testcooperativelevel">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p>Appelez <a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a> avec l’indicateur DXGI_PRESENT_TEST.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dbasetexture9">IDirect3DBaseTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dtexture9">IDirect3DTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dcubetexture9">IDirect3DCubeTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvolumetexture9">IDirect3DVolumeTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dindexbuffer9">IDirect3DIndexBuffer9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexbuffer9">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer">ID3D11Buffer</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture1d">ID3D11Texture1D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d">ID3D11Texture2D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture3d">ID3D11Texture3D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview">ID3D11ShaderResourceView</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview">ID3D11RenderTargetView</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexshader9">IDirect3DVertexShader9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dpixelshader9">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader">ID3D11VertexShader</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexdeclaration9">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setrenderstate">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setsamplerstate">IDirect3DDevice9::SetSamplerState</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11blendstate1">ID3D11BlendState1</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilstate">ID3D11DepthStencilState</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11rasterizerstate1">ID3D11RasterizerState1</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11samplerstate">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitive">IDirect3DDevice9::DrawIndexedPrimitive</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitive">IDirect3DDevice9::DrawPrimitive</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw">ID3D11DeviceContext::Draw</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed">ID3D11DeviceContext::DrawIndexed</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawindexedinstanced">ID3D11DeviceContext::DrawIndexedInstanced</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawinstanced">ID3D11DeviceContext::DrawInstanced</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetprimitivetopology">ID3D11DeviceContext::IASetPrimitiveTopology</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawauto">ID3D11DeviceContext::DrawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-beginscene">IDirect3DDevice9::BeginScene</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-endscene">IDirect3DDevice9::EndScene</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitiveup">IDirect3DDevice9::DrawPrimitiveUP</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitiveup">IDirect3DDevice9::DrawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>Aucun équivalent direct</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-showcursor">IDirect3DDevice9::ShowCursor</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorposition">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorproperties">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>Utilisez les API de curseur standard.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-reset">IDirect3DDevice9::Reset</a></p></td>
<td align="left"><p>Le périphérique LOST et POOL_MANAGED n’existent plus. <a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a> peut être mise en échec avec une valeur renvoyée <a href="https://docs.microsoft.com/windows/desktop/direct3ddxgi/dxgi-error">DXGI_ERROR_DEVICE_REMOVED</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawrectpatch">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawtripatch">IDirect3DDevice9:DrawTriPatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-lightenable">IDirect3DDevice9:LightEnable</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-multiplytransform">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setlight">IDirect3DDevice9:SetLight</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setmaterial">IDirect3DDevice9:SetMaterial</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setnpatchmode">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settransform">IDirect3DDevice9:SetTransform</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setfvf">IDirect3DDevice9:SetFVF</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settexturestagestate">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>Le pipeline à fonctions fixes n’est plus utilisé.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdepthstencilmatch">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdeviceformat">IDirect3DDevice9:CheckDeviceFormat</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-getdevicecaps">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-validatedevice">IDirect3DDevice9:ValidateDevice</a></p></td>
<td align="left"><p>Les bits de fonctionnalité ont été remplacés par les niveaux de fonctionnalité. Seuls quelques rares cas d’utilisation de format et de fonctionnalité sont facultatifs pour un niveau de fonctionnalité donné. Ils peuvent être vérifiés à l’aide des méthodes <a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport">ID3D11Device::CheckFeatureSupport</a> et <a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-checkformatsupport">ID3D11Device::CheckFormatSupport</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="surface-format-mapping"></a>Mappage des formats de surface


Utilisez le tableau suivant pour convertir les formats Direct3D 9 en formats DXGI.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Format Direct3D 9</th>
<th align="left">Format Direct3D 11</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>D3DFMT_UNKNOWN</p></td>
<td align="left"><p>DXGI_FORMAT_UNKNOWN</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8B8</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8A8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8A8_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8X8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8X8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R5G6B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G6R5_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X1R5G5B5</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A1R5G5B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G5R5A1_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A4R4G4B4</p></td>
<td align="left"><p>DXGI_FORMAT_B4G4R4A4_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R3G3B2</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8</p></td>
<td align="left"><p>DXGI_FORMAT_A8_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R3G3B2</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4R4G4B4</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2B10G10R10</p></td>
<td align="left"><p>DXGI_FORMAT_R10G10B10A2</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8B8G8R8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p>
<p>DXGI_FORMAT_R8G8B8A8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8B8G8R8</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2R10G10B10</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8P8</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_P8</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8_UNORM</p>
<div class="alert">
<strong>Remarque</strong>    astuce de .r d’utilisation dans le nuanceur de dupliquer rouge à d’autres composants pour obtenir le comportement de Direct3D 9.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>Remarque</strong>    permet astuce .rrrg dans le nuanceur en double rouge et déplacer vert pour les composants alphabétiques pour obtenir le comportement de Direct3D 9.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A4L4</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L6V5U5</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8L8V8U8</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_Q8W8V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_W11V11U10</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A2W10V10U10</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_UYVY</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8_B8G8</p></td>
<td align="left"><p>DXGI_FORMAT_G8R8_G8B8_UNORM</p>
<div class="alert">
<strong>Remarque</strong>    dans Direct3D 9, les données a été mis à l’échelle par 255.0f, mais cela peut être géré dans le nuanceur.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_YUY2</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G8R8_G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_B8G8_UNORM</p>
<div class="alert">
<strong>Remarque</strong>    dans Direct3D 9, les données a été mis à l’échelle par 255.0f, mais cela peut être géré dans le nuanceur.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT1</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM &amp; DXGI_FORMAT_BC1_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT2</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM &amp; DXGI_FORMAT_BC1_UNORM_SRGB</p>
<div class="alert">
<strong>Remarque</strong>    DXT1 et DXT2 sont identiques à partir d’un point de vue API/matériel. La seule différence est en cas d’utilisation d’un alpha prémultiplié, qui peut faire l’objet d’un suivi par une application et ne requiert pas de format distinct.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT3</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM &amp; DXGI_FORMAT_BC2_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT4</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM &amp; DXGI_FORMAT_BC2_UNORM_SRGB</p>
<div class="alert">
<strong>Remarque</strong>    DXT3 et DXT4 sont identiques à partir d’un point de vue API/matériel. La seule différence est en cas d’utilisation d’un alpha prémultiplié, qui peut faire l’objet d’un suivi par une application et ne requiert pas de format distinct.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT5</p></td>
<td align="left"><p>DXGI_FORMAT_BC3_UNORM &amp; DXGI_FORMAT_BC3_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16 &amp; D3DFMT_D16_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D15S1</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24S8</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24X8</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24X4S4</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32F_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24FS8</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_S1D15</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_S8D24</p></td>
<td align="left"><p>DXGI_FORMAT_D24_UNORM_S8_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8D24</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4S4D24</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UNORM</p>
<div class="alert">
<strong>Remarque</strong>    astuce de .r d’utilisation dans le nuanceur de dupliquer rouge à d’autres composants pour obtenir un comportement D3D9.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_INDEX16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_INDEX32</p></td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_Q16W16V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_MULTI2_ARGB8</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A32B32G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_CxV8U8</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT1</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT2</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT3</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT4</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPED3DCOLOR</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UBYTE4</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UINT</p>
<div class="alert">
<strong>Remarque</strong>    le nuanceur Obtient les valeurs UINT, mais si Direct3D 9 style intégrale à virgule flottante est nécessaires (0.0f, 1.0f... 255.f), UINT peut uniquement être converti en float32 dans le nuanceur.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>Remarque</strong>    le nuanceur Obtient les valeurs de Saint, mais si les valeurs en virgule flottante Direct3D 9 style intégrés sont nécessaires, Saint peut uniquement être converti en float32 dans le nuanceur.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>Remarque</strong>    le nuanceur Obtient les valeurs de Saint, mais si les valeurs en virgule flottante Direct3D 9 style intégrés sont nécessaires, Saint peut uniquement être converti en float32 dans le nuanceur.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_UBYTE4N</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_USHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_USHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UDEC3</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_DEC3N</p></td>
<td align="left"><p>Non disponible</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT16_2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT16_4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>FourCC 'ATI1'</p></td>
<td align="left"><p>DXGI_FORMAT_BC4_UNORM</p>
<div class="alert">
<strong>Remarque</strong>    nécessite le niveau de fonctionnalité 10.0 ou version ultérieur
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>Remarque</strong>    nécessite le niveau de fonctionnalité 10.0 ou version ultérieur
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 




