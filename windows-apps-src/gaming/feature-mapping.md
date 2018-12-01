---
title: Mapper les fonctionnalités DirectX9 aux API DirectX11
description: Découvrez la façon dont les fonctionnalités utilisées par votre jeu Direct3D9 sont traduites dans Direct3D11 et la plateforme Windows universelle (UWP).
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, directx9, directx11, portage
ms.localizationpriority: medium
ms.openlocfilehash: 56bb86706795e773d21e45263f640f9fc0aa596a
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8338559"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>Mapper les fonctionnalités DirectX9 sur les API DirectX11



**Résumé**

-   [Planifier votre portage DirectX](plan-your-directx-port.md)
-   [Modifications importantes de Direct3D 9 à Direct3D 11](understand-direct3d-11-1-concepts.md)
-   Mappage des fonctionnalités


Découvrez comment les fonctionnalités utilisées par votre jeu Direct3D 9 sont traduites dans Direct3D 11 et la plateforme Windows universelle (UWP).

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>Mappage des API Direct3D 9 aux API DirectX 11


[Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) reste la base des graphismes DirectX, mais l’API a changé depuis DirectX9:

-   L’infrastructure DXGI (Microsoft DirectX Graphics Infrastructure) sert à configurer les cartes graphiques. Utilisez [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534) pour sélectionner le format des tampons, créer des chaînes d’échange, présenter des trames et créer des ressources partagées. Voir [Vue d’ensemble de DXGI](https://msdn.microsoft.com/library/windows/desktop/bb205075).
-   Un contexte de périphérique Direct3D permet de définir l’état du pipeline et de générer des commandes de rendu. La plupart de nos exemples utilisent un contexte immédiat pour effectuer un rendu directement sur le périphérique. Direct3D 11 prend aussi en charge le rendu multithread associé aux contextes différés. Voir [Présentation d’un périphérique dans Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476880).
-   Certaines fonctionnalités ont été abandonnées, notamment le pipeline à fonctions fixes. Voir [Fonctionnalités obsolètes](https://msdn.microsoft.com/library/windows/desktop/cc308047).

Pour obtenir la liste complète des fonctionnalités de Direct3D11, voir [Fonctionnalités de Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476342) et [Fonctionnalités de Direct3D11](https://msdn.microsoft.com/library/windows/desktop/hh404562).

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>Passage de Direct2D 9 à Direct2D 11


[Direct2D (Windows)](https://msdn.microsoft.com/library/windows/desktop/dd370990) occupe toujours une place importante dans les graphismes DirectX et Windows. Vous pouvez l’utiliser pour dessiner des jeux en 2D et des superpositions (affichages à tête haute) par-dessus Direct3D.

Direct2D s’exécute par-dessus Direct3D. Les jeux en 2D peuvent être implémentés à l’aide de l’une ou l’autre des API. Par exemple, un jeu en 2D implémenté avec Direct3D peut utiliser la projection orthographique, définir des valeurs Z pour contrôler l’ordre de tracé des primitives et utiliser des nuanceurs de pixels pour ajouter des effets spéciaux.

Dans la mesure où Direct2D repose sur Direct3D, il utilise aussi DXGI et les contextes de périphérique. Voir [Vue d’ensemble des API de Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd317121).

L’API [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) permet la prise en charge du texte mis en forme avec Direct2D. Voir [Présentation de DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd371554).

## <a name="replace-deprecated-helper-libraries"></a>Remplacer les bibliothèques d’assistance déconseillées


D3DX et DXUT sont déconseillés et ne peuvent pas être utilisés par des jeux UWP. Ces bibliothèques d’assistance fournissaient des ressources pour les tâches comme le chargement des textures et des maillages.

-   La procédure pas à pas [Portage simple de Direct3D9 vers UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) décrit les méthodes permettant de configurer une fenêtre, d’initialiser Direct3D et d’effectuer un rendu de base en3D.
-   La procédure pas à pas [Jeu UWP simple avec DirectX](tutorial--create-your-first-uwp-directx-game.md) décrit les tâches communes de programmation de jeux, notamment les graphismes, le chargement de fichiers, l’interface utilisateur, les commandes et le son.
-   Le projet de la communauté [Kit de ressources DirectX](http://go.microsoft.com/fwlink/p/?LinkID=248929) propose des classes d’assistance pour Direct3D11 et les applications UWP.

## <a name="move-shader-programs-from-fx-to-hlsl"></a>Déplacer les programmes de nuanceurs de FX à HLSL


La bibliothèque des utilitaires D3DX (D3DX 9, D3DX 10 et D3DX 11), y compris Effects, est déconseillée pour UWP. Tous les jeux DirectX pour UWP pilotent le pipeline graphique à l’aide de [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) sans la bibliothèque d’effets.

Visual Studio utilise toujours FXC pour compiler les objets nuanceurs. Les nuanceurs UWP sont compilés à l’avance. Le bytecode est chargé à l’exécution, puis chaque ressource de nuanceur est liée au pipeline graphique pendant l’étape de rendu appropriée. Les nuanceurs doivent être déplacés vers leurs propres fichiers .HLSL et des techniques de rendu doivent être implémentées dans votre code C++.

Pour un aperçu du chargement des ressources de nuanceur, voir [Portage simple de Direct3D9 vers UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

Direct3D 11 a introduit le modèle de nuanceur 5, qui nécessite le niveau de fonctionnalité 11\_0 (ou supérieur) de Direct3D. Voir [Fonctionnalités du modèle de nuanceur5 HLSL pour Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff471419).

## <a name="replace-xnamath-and-d3dxmath"></a>Remplacer XNAMath et D3DXMath


Le code qui utilise XNAMath (ou D3DXMath) doit être migré vers [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833). DirectXMath inclut des types pouvant être portés sur des processeurs x86, x64 et ARM. Voir [Migration du code de la bibliothèque XNA Math](https://msdn.microsoft.com/library/windows/desktop/ee418730).

Notez que les types float DirectXMath conviennent parfaitement aux nuanceurs. Par exemple, [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) et [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621) alignent des données pour les mémoires tampons constantes.

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>Remplacer DirectSound par XAudio2 (et l’audio en arrière-plan)


DirectSound n’est pas pris en charge pour UWP :

-   Utilisez [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) pour ajouter des effets sonores à votre jeu.

##  <a name="replace-directinput-with-xinput-and-uwp-apis"></a>Remplacer DirectInput par XInput et les API UWP


DirectInput n’est pas pris en charge pour UWP :

-   Utilisez les rappels d’événement d’entrée [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) pour la souris, le clavier et les entrées tactiles.
-   Utilisez [XInput](https://msdn.microsoft.com/library/windows/desktop/ee417001)1.4 pour la prise en charge des contrôleurs de jeu (et la prise en charge du casque des contrôleurs de jeu). Si vous utilisez une base de code partagé pour les applications de bureau et UWP, voir [Versions XInput](https://msdn.microsoft.com/library/windows/desktop/hh405051) pour plus d’informations sur la compatibilité descendante.
-   Inscrivez-vous aux événements [**EdgeGesture**](https://msdn.microsoft.com/library/windows/apps/hh701600) si votre jeu doit utiliser la barre de l’application.

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>Utiliser Microsoft Media Foundation au lieu de DirectShow


DirectShow ne fait plus partie de l’API DirectX (ou de l’API Windows). [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) fournit le contenu vidéo à Direct3D via les surfaces partagées. Voir [API vidéo de Direct3D11](https://msdn.microsoft.com/library/windows/desktop/hh447677).

## <a name="replace-directplay-with-networking-code"></a>Remplacer DirectPlay par du code réseau


Microsoft DirectPlay est déconseillé. Si votre jeu utilise des services réseau, vous devez fournir du code réseau conforme aux exigences UWP. Utilisez les API suivantes:

-   [Win32 et COM pour les applications UWP (réseau) (Windows)](https://msdn.microsoft.com/library/windows/apps/br205759)
-   [**Espace de noms Windows.Networking (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207124)
-   [**Espace de noms Windows.Networking.Sockets (Windows)**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [**Espace de noms Windows.Networking.Connectivity (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207308)
-   [**Espace de noms Windows.ApplicationModel.Background (Windows)**](https://msdn.microsoft.com/library/windows/apps/br224847)

Les articles suivants vous aident à ajouter des fonctionnalités réseau et à déclarer la prise en charge réseau dans le manifeste du package de votre application.

-   [Connexion avec des sockets (applications UWP en C#/VB/C++ et XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
-   [Connexion avec des WebSockets (applications UWP en C#/VB/C++ et XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh994396)
-   [Connexion aux services Web (applications UWP en C#/VB/C++ et XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
-   [Notions de base en matière de réseau](https://msdn.microsoft.com/library/windows/apps/mt280233)

Notez que toutes les applications pour UWP (y compris les jeux) utilisent des types spécifiques de tâches en arrière-plan pour maintenir la connectivité pendant la suspension de l’application. Si votre jeu doit rester en état de connexion pendant sa suspension, voir [Notions de base en matière de réseau](https://msdn.microsoft.com/library/windows/apps/mt280233).

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174336">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280493">ID3D11Device2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280498">ID3D11DeviceContext2</a></p>
<p>Les étapes du pipeline graphique sont décrites dans <a href="https://msdn.microsoft.com/library/windows/desktop/ff476882">Pipeline graphique</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174300">IDirect3D9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404556">IDXGIFactory2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404537">IDXGIAdapter2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280345">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174423">IDirect3DDevice9::Present</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174472">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p>Appelez <a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a> avec l’indicateur DXGI_PRESENT_TEST.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174322">IDirect3DBaseTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205909">IDirect3DTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174329">IDirect3DCubeTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205941">IDirect3DVolumeTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205865">IDirect3DIndexBuffer9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205915">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476351">ID3D11Buffer</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476633">ID3D11Texture1D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476635">ID3D11Texture2D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476637">ID3D11Texture3D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476628">ID3D11ShaderResourceView</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476582">ID3D11RenderTargetView</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476377">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205922">IDirect3DVertexShader9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205869">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476641">ID3D11VertexShader</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476576">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205919">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476575">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205805">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205806">IDirect3DDevice9::SetSamplerState</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404571">ID3D11BlendState1</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476375">ID3D11DepthStencilState</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/hh446828">ID3D11RasterizerState1</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476588">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174369">IDirect3DDevice9::DrawIndexedPrimitive</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174371">IDirect3DDevice9::DrawPrimitive</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476407">ID3D11DeviceContext::Draw</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476409">ID3D11DeviceContext::DrawIndexed</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173566">ID3D11DeviceContext::DrawIndexedInstanced</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173567">ID3D11DeviceContext::DrawInstanced</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173590">ID3D11DeviceContext::IASetPrimitiveTopology</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173564">ID3D11DeviceContext::DrawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174350">IDirect3DDevice9::BeginScene</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174375">IDirect3DDevice9::EndScene</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174372">IDirect3DDevice9::DrawPrimitiveUP</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174370">IDirect3DDevice9::DrawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>Aucun équivalent direct</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174470">IDirect3DDevice9::ShowCursor</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174429">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174430">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>Utilisez les API de curseur standard.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174425">IDirect3DDevice9::Reset</a></p></td>
<td align="left"><p>Le périphérique LOST et POOL_MANAGED n’existent plus. <a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a> peut être mise en échec avec une valeur renvoyée <a href="https://msdn.microsoft.com/library/windows/desktop/bb509553">DXGI_ERROR_DEVICE_REMOVED</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174373">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174374">IDirect3DDevice9:DrawTriPatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174421">IDirect3DDevice9:LightEnable</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174422">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205798">IDirect3DDevice9:SetLight</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174437">IDirect3DDevice9:SetMaterial</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174438">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174463">IDirect3DDevice9:SetTransform</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174433">IDirect3DDevice9:SetFVF</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174462">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>Le pipeline à fonctions fixes n’est plus utilisé.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174308">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174309">IDirect3DDevice9:CheckDeviceFormat</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174320">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205859">IDirect3DDevice9:ValidateDevice</a></p></td>
<td align="left"><p>Les bits de fonctionnalité ont été remplacés par les niveaux de fonctionnalité. Seuls quelques rares cas d’utilisation de format et de fonctionnalité sont facultatifs pour un niveau de fonctionnalité donné. Ils peuvent être vérifiés à l’aide des méthodes <a href="https://msdn.microsoft.com/library/windows/desktop/ff476497">ID3D11Device::CheckFeatureSupport</a> et <a href="https://msdn.microsoft.com/library/windows/desktop/bb173536">ID3D11Device::CheckFormatSupport</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="surface-format-mapping"></a>Mappage des formats de Surface


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
<strong>Remarque</strong>  swizzle .r d’utilisation dans le nuanceur pour dupliquer le rouge dans les autres composants et obtenir le comportement Direct3D 9.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>Remarque</strong>  utilisez le swizzle .rrrg dans le nuanceur pour dupliquer le rouge et déplacer le vert dans les composants alpha pour obtenir le comportement Direct3D 9.
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
<strong>Remarque</strong>  dans Direct3D 9, les données a été agrandissement de 255.0f, mais cela peut être géré dans le nuanceur.
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
<strong>Remarque</strong>  dans Direct3D 9, les données a été agrandissement de 255.0f, mais cela peut être géré dans le nuanceur.
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
<strong>Remarque</strong>  DXT1 et DXT2 sont identiques d’un point de vue API/matériel. La seule différence est en cas d’utilisation d’un alpha prémultiplié, qui peut faire l’objet d’un suivi par une application et ne requiert pas de format distinct.
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
<strong>Remarque</strong>  DXT3 et DXT4 sont identiques d’un point de vue API/matériel. La seule différence est en cas d’utilisation d’un alpha prémultiplié, qui peut faire l’objet d’un suivi par une application et ne requiert pas de format distinct.
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
<strong>Remarque</strong>  swizzle .r d’utilisation dans le nuanceur pour dupliquer le rouge dans les autres composants et obtenir le comportement D3D9.
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
<strong>Remarque</strong>  le nuanceur Obtient des valeurs UINT, mais si la partie intégrante de style Direct3D 9 sont nécessaires (0.0f, 1.0f … 255.f), UINT peut seulement être convertie en float32 dans le nuanceur.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>Remarque</strong>  le nuanceur Obtient des valeurs SINT, mais si Direct3D 9 style sont nécessaires, les valeurs SINT peut seulement être convertie en float32 dans le nuanceur.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>Remarque</strong>  le nuanceur Obtient des valeurs SINT, mais si Direct3D 9 style sont nécessaires, les valeurs SINT peut seulement être convertie en float32 dans le nuanceur.
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
<strong>Remarque</strong>  requiert le niveau de fonctionnalité 10.0 ou ultérieur
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>Remarque</strong>  requiert le niveau de fonctionnalité 10.0 ou ultérieur
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 




