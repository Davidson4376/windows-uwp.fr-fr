---
author: mtoepke
title: "Mapper les fonctionnalités DirectX9 aux API DirectX11"
description: "Découvrez comment les fonctionnalités utilisées par votre jeu Direct3D9 sont traduites dans Direct3D11 et la plateforme Windows universelle (UWP)."
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 6ab76921f1e8b613010f99eba6a141daca128ea5

---

# Mapper les fonctionnalités DirectX9 aux API DirectX11


\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**Récapitulatif**

-   [Planifier votre portage DirectX](plan-your-directx-port.md)
-   [Modifications importantes de Direct3D 9 à Direct3D 11](understand-direct3d-11-1-concepts.md)
-   Mappage des fonctionnalités


Découvrez comment les fonctionnalités utilisées par votre jeu Direct3D 9 sont traduites dans Direct3D 11 et la plateforme Windows universelle (UWP).

## Mappage des API Direct3D 9 aux API DirectX 11


[Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) reste la base des graphismes DirectX, mais l’API a changé depuis DirectX9:

-   L’infrastructure DXGI (Microsoft DirectX Graphics Infrastructure) sert à configurer les cartes graphiques. Utilisez [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534) pour sélectionner le format des tampons, créer des chaînes d’échange, présenter des trames et créer des ressources partagées. Voir [Vue d’ensemble de DXGI](https://msdn.microsoft.com/library/windows/desktop/bb205075).
-   Un contexte de périphérique Direct3D permet de définir l’état du pipeline et de générer des commandes de rendu. La plupart de nos exemples utilisent un contexte immédiat pour effectuer un rendu directement sur le périphérique. Direct3D 11 prend aussi en charge le rendu multithread associé aux contextes différés. Voir [Présentation d’un périphérique dans Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476880).
-   Certaines fonctionnalités ont été abandonnées, notamment le pipeline à fonctions fixes. Voir [Fonctionnalités obsolètes](https://msdn.microsoft.com/library/windows/desktop/cc308047).

Pour obtenir la liste complète des fonctionnalités de Direct3D11, voir [Fonctionnalités de Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476342) et [Fonctionnalités de Direct3D11](https://msdn.microsoft.com/library/windows/desktop/hh404562).

## Passage de Direct2D 9 à Direct2D 11


[Direct2D (Windows)](https://msdn.microsoft.com/library/windows/desktop/dd370990) occupe toujours une place importante dans les graphismes DirectX et Windows. Vous pouvez l’utiliser pour dessiner des jeux en 2D et des superpositions (affichages à tête haute) par-dessus Direct3D.

Direct2D s’exécute par-dessus Direct3D. Les jeux en 2D peuvent être implémentés à l’aide de l’une ou l’autre des API. Par exemple, un jeu en 2D implémenté avec Direct3D peut utiliser la projection orthographique, définir des valeurs Z pour contrôler l’ordre de tracé des primitives et utiliser des nuanceurs de pixels pour ajouter des effets spéciaux.

Dans la mesure où Direct2D repose sur Direct3D, il utilise aussi DXGI et les contextes de périphérique. Voir [Vue d’ensemble des API de Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd317121).

L’API [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) permet la prise en charge du texte mis en forme avec Direct2D. Voir [Présentation de DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd371554).

## Remplacer les bibliothèques d’assistance déconseillées


D3DX et DXUT sont déconseillés et ne peuvent pas être utilisés par des jeux UWP. Ces bibliothèques d’assistance fournissaient des ressources pour les tâches comme le chargement des textures et des maillages.

-   La procédure pas à pas [Portage simple de Direct3D9 vers UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) décrit les méthodes permettant de configurer une fenêtre, d’initialiser Direct3D et d’effectuer un rendu de base en3D.
-   La procédure pas à pas [Jeu UWP simple avec DirectX](tutorial--create-your-first-metro-style-directx-game.md) décrit les tâches communes de programmation de jeux, notamment les graphismes, le chargement de fichiers, l’interface utilisateur, les commandes et le son.
-   Le projet de la communauté [Kit de ressources DirectX](http://go.microsoft.com/fwlink/p/?LinkID=248929) propose des classes d’assistance pour Direct3D11 et les applications UWP.

## Déplacer les programmes de nuanceurs de FX à HLSL


La bibliothèque des utilitaires D3DX (D3DX 9, D3DX 10 et D3DX 11), y compris Effects, est déconseillée pour UWP. Tous les jeux DirectX pour UWP pilotent le pipeline graphique à l’aide de [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) sans la bibliothèque d’effets.

Visual Studio utilise toujours FXC pour compiler les objets nuanceurs. Les nuanceurs UWP sont compilés à l’avance. Le bytecode est chargé à l’exécution, puis chaque ressource de nuanceur est liée au pipeline graphique pendant l’étape de rendu appropriée. Les nuanceurs doivent être déplacés vers leurs propres fichiers .HLSL et des techniques de rendu doivent être implémentées dans votre code C++.

Pour un aperçu du chargement des ressources de nuanceur, voir [Portage simple de Direct3D9 vers UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

Direct3D 11 a introduit le modèle de nuanceur 5, qui nécessite le niveau de fonctionnalité 11\_0 (ou supérieur) de Direct3D. Voir [Fonctionnalités du modèle de nuanceur5 HLSL pour Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff471419).

## Remplacer XNAMath et D3DXMath


Le code qui utilise XNAMath (ou D3DXMath) doit être migré vers [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833). DirectXMath inclut des types pouvant être portés sur des processeurs x86, x64 et ARM. Voir [Migration du code de la bibliothèque XNA Math](https://msdn.microsoft.com/library/windows/desktop/ee418730).

Notez que les types float DirectXMath conviennent parfaitement aux nuanceurs. Par exemple, [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) et [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621) alignent des données pour les mémoires tampons constantes.

## Remplacer DirectSound par XAudio2 (et l’audio en arrière-plan)


DirectSound n’est pas pris en charge pour UWP :

-   Utilisez [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) pour ajouter des effets sonores à votre jeu.

##  Remplacer DirectInput par XInput et les API UWP


DirectInput n’est pas pris en charge pour UWP :

-   Utilisez les rappels d’événement d’entrée [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) pour la souris, le clavier et les entrées tactiles.
-   Utilisez [XInput](https://msdn.microsoft.com/library/windows/desktop/ee417001)1.4 pour la prise en charge des contrôleurs de jeu (et la prise en charge du casque des contrôleurs de jeu). Si vous utilisez une base de code partagé pour les applications de bureau et UWP, voir [Versions XInput](https://msdn.microsoft.com/library/windows/desktop/hh405051) pour plus d’informations sur la compatibilité descendante.
-   Inscrivez-vous aux événements [**EdgeGesture**](https://msdn.microsoft.com/library/windows/apps/hh701600) si votre jeu doit utiliser la barre de l’application.

## Utiliser Microsoft Media Foundation au lieu de DirectShow


DirectShow ne fait plus partie de l’API DirectX (ou de l’API Windows). [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) fournit le contenu vidéo à Direct3D via les surfaces partagées. Voir [API vidéo de Direct3D11](https://msdn.microsoft.com/library/windows/desktop/hh447677).

## Remplacer DirectPlay par du code réseau


Microsoft DirectPlay est déconseillé. Si votre jeu utilise des services réseau, vous devez fournir du code réseau conforme aux exigences UWP. Utilisez les API suivantes :

-   [Win32 et COM pour les applications du Windows Store (réseau) (Windows)](https://msdn.microsoft.com/library/windows/apps/br205759)
-   [**Espace de noms Windows.Networking (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207124)
-   [**Espace de noms Windows.Networking.Sockets (Windows)**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [**Espace de noms Windows.Networking.Connectivity (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207308)
-   [**Espace de noms Windows.ApplicationModel.Background (Windows)**](https://msdn.microsoft.com/library/windows/apps/br224847)

Les articles suivants vous aident à ajouter des fonctionnalités réseau et à déclarer la prise en charge réseau dans le manifeste du package de votre application.

-   [Connexion avec des sockets (applications du Windows Store en C#/VB/C++ et XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
-   [Connexion avec des WebSockets (applications du Windows Store en C#/VB/C++ et XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh994396)
-   [Connexion aux services Web (applications du Windows Store en C#/VB/C++ et XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
-   [Notions de base en matière de réseau](https://msdn.microsoft.com/library/windows/apps/mt280233)

Notez que toutes les applications pour UWP (y compris les jeux) utilisent des types spécifiques de tâches en arrière-plan pour maintenir la connectivité pendant la suspension de l’application. Si votre jeu doit rester en état de connexion pendant sa suspension, voir [Notions de base en matière de réseau](https://msdn.microsoft.com/library/windows/apps/mt280233).

## Mappage de fonctions


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
<td align="left"><p>[<strong>IDirect3DDevice9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174336)</p></td>
<td align="left"><p>[<strong>ID3D11Device2</strong>](https://msdn.microsoft.com/library/windows/desktop/dn280493)</p>
<p>[<strong>ID3D11DeviceContext2</strong>](https://msdn.microsoft.com/library/windows/desktop/dn280498)</p>
<p>Les étapes du pipeline graphique sont décrites dans [Pipeline graphique](https://msdn.microsoft.com/library/windows/desktop/ff476882).</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3D9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174300)</p></td>
<td align="left"><p>[<strong>IDXGIFactory2</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404556)</p>
<p>[<strong>IDXGIAdapter2</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404537)</p>
<p>[<strong>IDXGIDevice3</strong>](https://msdn.microsoft.com/library/windows/desktop/dn280345)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9::Present</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174423)</p></td>
<td align="left"><p>[<strong>IDXGISwapChain1::Present1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh446797)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::TestCooperativeLevel</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174472)</p></td>
<td align="left"><p>Appelez [<strong>IDXGISwapChain1::Present1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh446797) avec l’indicateur DXGI_PRESENT_TEST.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DBaseTexture9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174322)</p>
<p>[<strong>IDirect3DTexture9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205909)</p>
<p>[<strong>IDirect3DCubeTexture9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174329)</p>
<p>[<strong>IDirect3DVolumeTexture9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205941)</p>
<p>[<strong>IDirect3DIndexBuffer9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205865)</p>
<p>[<strong>IDirect3DVertexBuffer9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205915)</p></td>
<td align="left"><p>[<strong>ID3D11Buffer</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476351)</p>
<p>[<strong>ID3D11Texture1D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476633)</p>
<p>[<strong>ID3D11Texture2D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476635)</p>
<p>[<strong>ID3D11Texture3D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476637)</p>
<p>[<strong>ID3D11ShaderResourceView</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476628)</p>
<p>[<strong>ID3D11RenderTargetView</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476582)</p>
<p>[<strong>ID3D11DepthStencilView</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476377)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DVertexShader9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205922)</p>
<p>[<strong>IDirect3DPixelShader9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205869)</p></td>
<td align="left"><p>[<strong>ID3D11VertexShader</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476641)</p>
<p>[<strong>ID3D11PixelShader</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476576)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DVertexDeclaration9</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205919)</p></td>
<td align="left"><p>[<strong>ID3D11InputLayout</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476575)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::SetRenderState</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205805)</p>
<p>[<strong>IDirect3DDevice9::SetSamplerState</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205806)</p></td>
<td align="left"><p>[<strong>ID3D11BlendState1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404571)</p>
<p>[<strong>ID3D11DepthStencilState</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476375)</p>
<p>[<strong>ID3D11RasterizerState1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh446828)</p>
<p>[<strong>ID3D11SamplerState</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476588)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9::DrawIndexedPrimitive</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174369)</p>
<p>[<strong>IDirect3DDevice9::DrawPrimitive</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174371)</p></td>
<td align="left"><p>[<strong>ID3D11DeviceContext::Draw</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476407)</p>
<p>[<strong>ID3D11DeviceContext::DrawIndexed</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476409)</p>
<p>[<strong>ID3D11DeviceContext::DrawIndexedInstanced</strong>](https://msdn.microsoft.com/library/windows/desktop/bb173566)</p>
<p>[<strong>ID3D11DeviceContext::DrawInstanced</strong>](https://msdn.microsoft.com/library/windows/desktop/bb173567)</p>
<p>[<strong>ID3D11DeviceContext::IASetPrimitiveTopology</strong>](https://msdn.microsoft.com/library/windows/desktop/bb173590)</p>
<p>[<strong>ID3D11DeviceContext::DrawAuto</strong>](https://msdn.microsoft.com/library/windows/desktop/bb173564)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::BeginScene</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174350)</p>
<p>[<strong>IDirect3DDevice9::EndScene</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174375)</p>
<p>[<strong>IDirect3DDevice9::DrawPrimitiveUP</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174372)</p>
<p>[<strong>IDirect3DDevice9::DrawIndexedPrimitiveUP</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174370)</p></td>
<td align="left"><p>Aucun équivalent direct</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9::ShowCursor</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174470)</p>
<p>[<strong>IDirect3DDevice9::SetCursorPosition</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174429)</p>
<p>[<strong>IDirect3DDevice9::SetCursorProperties</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174430)</p></td>
<td align="left"><p>Utilisez les API de curseur standard.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::Reset</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174425)</p></td>
<td align="left"><p>Le périphérique LOST et POOL_MANAGED n’existent plus. [<strong>IDXGISwapChain1::Present1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh446797) peut être mise en échec avec une valeur renvoyée [<strong>DXGI_ERROR_DEVICE_REMOVED</strong>](https://msdn.microsoft.com/library/windows/desktop/bb509553).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9:DrawRectPatch</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174373)</p>
<p>[<strong>IDirect3DDevice9:DrawTriPatch</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174374)</p>
<p>[<strong>IDirect3DDevice9:LightEnable</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174421)</p>
<p>[<strong>IDirect3DDevice9:MultiplyTransform</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174422)</p>
<p>[<strong>IDirect3DDevice9:SetLight</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205798)</p>
<p>[<strong>IDirect3DDevice9:SetMaterial</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174437)</p>
<p>[<strong>IDirect3DDevice9:SetNPatchMode</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174438)</p>
<p>[<strong>IDirect3DDevice9:SetTransform</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174463)</p>
<p>[<strong>IDirect3DDevice9:SetFVF</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174433)</p>
<p>[<strong>IDirect3DDevice9:SetTextureStageState</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174462)</p></td>
<td align="left"><p>Le pipeline à fonctions fixes n’est plus utilisé.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9:CheckDepthStencilMatch</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174308)</p>
<p>[<strong>IDirect3DDevice9:CheckDeviceFormat</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174309)</p>
<p>[<strong>IDirect3DDevice9:GetDeviceCaps</strong>](https://msdn.microsoft.com/library/windows/desktop/bb174320)</p>
<p>[<strong>IDirect3DDevice9:ValidateDevice</strong>](https://msdn.microsoft.com/library/windows/desktop/bb205859)</p></td>
<td align="left"><p>Les bits de fonctionnalité ont été remplacés par les niveaux de fonctionnalité. Seuls quelques rares cas d’utilisation de format et de fonctionnalité sont facultatifs pour un niveau de fonctionnalité donné. Ils peuvent être vérifiés à l’aide des méthodes [<strong>ID3D11Device::CheckFeatureSupport</strong>](https://msdn.microsoft.com/library/windows/desktop/ff476497) et [<strong>ID3D11Device::CheckFormatSupport</strong>](https://msdn.microsoft.com/library/windows/desktop/bb173536).</p></td>
</tr>
</tbody>
</table>

 

## Mappage des formats de surface


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
<strong>Remarque</strong> Utilisez le swizzle .r dans le nuanceur pour dupliquer le rouge dans les autres composants et obtenir le comportement Direct3D 9.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>Remarque</strong> Utilisez le swizzle .rrrg dans le nuanceur pour dupliquer le rouge et déplacer le vert dans les composants alpha pour obtenir le comportement Direct3D 9.
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
<strong>Remarque</strong> Dans Direct3D 9, les données étaient agrandies de 255.0f, mais cela peut être géré dans le nuanceur.
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
<strong>Remarque</strong> Dans Direct3D 9, les données étaient agrandies de 255.0f, mais cela peut être géré dans le nuanceur.
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
<strong>Remarque</strong> DXT1 et DXT2 sont identiques d’un point de vue API/matériel. La seule différence est en cas d’utilisation d’un alpha prémultiplié, qui peut faire l’objet d’un suivi par une application et ne requiert pas de format distinct.
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
<strong>Remarque</strong> DXT3 et DXT4 sont identiques d’un point de vue API/matériel. La seule différence est en cas d’utilisation d’un alpha prémultiplié, qui peut faire l’objet d’un suivi par une application et ne requiert pas de format distinct.
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
<strong>Remarque</strong> Utilisez le swizzle .r dans le nuanceur pour dupliquer le rouge dans les autres composants et obtenir le comportement D3D9.
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
<strong>Remarque</strong> Le nuanceur obtient des valeurs UINT, mais si des valeurs en virgule flottante de style Direct3D 9 sont nécessaires (0.0f, 1.0f... 255.f), les valeurs UINT peuvent seulement être converties en float32 dans le nuanceur.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>Remarque</strong> Le nuanceur obtient des valeurs SINT, mais si des valeurs en virgule flottante de style Direct3D 9 sont nécessaires, les valeurs SINT peuvent seulement être converties en float32 dans le nuanceur.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>Remarque</strong> Le nuanceur obtient des valeurs SINT, mais si des valeurs en virgule flottante de style Direct3D 9 sont nécessaires, les valeurs SINT peuvent seulement être converties en float32 dans le nuanceur.
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
<strong>Remarque</strong> Requiert le niveau de fonctionnalité 10.0 ou ultérieur
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>Remarque</strong> Requiert le niveau de fonctionnalité 10.0 ou ultérieur
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 







<!--HONumber=Aug16_HO3-->


