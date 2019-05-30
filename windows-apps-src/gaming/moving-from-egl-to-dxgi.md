---
title: Comparer le code EGL avec DXGI et Direct3D
description: L’interface graphique DirectX (DXGI) et certaines API Direct3D jouent le même rôle qu’EGL. Cette rubrique vous aidera à comprendre le fonctionnement de DXGI et Direct3D 11 sous l’ange d’EGL.
ms.assetid: 90f5ecf1-dd5d-fea3-bed8-57a228898d2a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, egl, dxgi, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 3a93f78cdc0d716f9b421fdc493317208f9d17b2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368374"
---
# <a name="compare-egl-code-to-dxgi-and-direct3d"></a>Comparer le code EGL avec DXGI et Direct3D




**API importantes**

-   [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)
-   [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)
-   [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)

L’interface graphique DirectX (DXGI) et certaines API Direct3D jouent le même rôle qu’EGL. Cette rubrique vous aidera à comprendre le fonctionnement de DXGI et Direct3D 11 sous l’ange d’EGL.

À l’instar d’EGL, DXGI et Direct3D fournissent diverses méthodes qui vous permettent de configurer des ressources graphiques, d’obtenir un contexte de rendu pour le dessin des nuanceurs et d’afficher les résultats dans une fenêtre. Toutefois, DXGI et Direct3D comportent quelques options supplémentaires qui nécessitent une configuration particulière dans le cadre du portage à partir d’EGL.

> **Remarque**    ce guide repose sur la norme ouverte du groupe Khronos EGL 1.4, disponible ici : [Interface graphique de plateforme Khronos natif (EGL Version 1.4 - le 6 avril 2011) \[PDF\]](https://www.khronos.org/registry/egl/specs/eglspec.1.4.20110406.pdf). Les variantes de syntaxe propres à d’autres plateformes et langages de développement ne sont pas traitées dans cette rubrique.

 

## <a name="how-does-dxgi-and-direct3d-compare"></a>Comparaison avec DXGI et Direct3D


Avec EGL, il est relativement facile de commencer à dessiner dans une surface de fenêtre. C’est l’un des atouts d’EGL par rapport à DXGI et Direct3D. Cela s’explique par le fait qu’OpenGL ES 2.0 (et donc EGL) est une spécification implémentée par de nombreux fournisseurs de plateforme, alors que DXGI et Direct3D constituent une référence unique à laquelle les pilotes des fournisseurs de matériel doivent se conformer. Microsoft doit donc implémenter un ensemble d’API capables de prendre en charge le plus de fonctionnalités tierces possibles, au lieu de se limiter à un groupe de fonctionnalités d’un fournisseur donné ou encore de combiner des commandes d’installation propres à un fournisseur dans des API plus simples. À l’inverse, Direct3D offre un ensemble unique d’API capables de prendre en charge une très grande variété de plateformes matérielles vidéo et de niveaux de fonctionnalité. En cela, Direct3D se montre plus flexible pour les développeurs sachant parfaitement utiliser la plateforme en question.

Comme EGL, DXGI et Direct3D fournissent des API pour les opérations suivantes :

-   Obtention des données, et lecture/écriture de ces données dans un tampon de trame (appelé « chaîne de permutation » dans DXGI).
-   Association du tampon de trame avec une fenêtre d’interface utilisateur.
-   Obtention et configuration des contextes de rendu destinés au dessin.
-   Envoi des commandes au pipeline graphique pour un contexte de rendu spécifique.
-   Création et gestion des ressources des nuanceurs, et association de ces dernières avec un contexte de rendu.
-   Affichage sur des cibles de rendu spécifiques (telles que les textures).
-   Actualisation de la surface d’affichage de la fenêtre afin de présenter les résultats du rendu avec les ressources graphiques.

Pour voir le processus de base Direct3D pour la configuration du pipeline graphique, consultez le modèle d’application de 11 DirectX (Windows universel) dans Microsoft Visual Studio 2015. La classe de rendu de base fournie dans ce modèle peut servir de référence pour créer l’infrastructure graphique Direct3D 11 et y configurer les principales ressources nécessaires. Elle offre également une prise en charge de certaines fonctionnalités des applications de plateforme Windows universelle (UWP), telles que la rotation de l’écran.

EGL comprend très peu d’API en comparaison de Direct3D 11. La navigation entre les différentes API de Direct3D 11 peut se révéler compliquée si vous n’êtes pas familiarisé avec les noms et le langage technique propres à cette plateforme. Nous vous proposons ici une vue d’ensemble pour vous aider à démarrer.

En premier lieu, sachez identifier à quel élément d’interface Direct3D correspond chaque objet EGL de base :

| Abstraction EGL | Représentation Direct3D similaire                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EGLDisplay**  | Dans Direct3D (pour les applications UWP), le handle d’affichage est fourni par l’API [**Windows::UI::CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) (ou l’interface **ICoreWindowInterop** qui expose le HWND). La carte et le matériel sont configurés par le biais des interfaces COM [**IDXGIAdapter**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) et [**IDXGIDevice1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2), respectivement.                                                                                                                                                                                                                                                           |
| **EGLSurface**  | Dans Direct3D, les mémoires tampons et diverses autres ressources de fenêtre (visibles ou hors écran) sont créées et configurées par des interfaces DXGI spécifiques, parmi lesquelles [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2), qui est une implémentation d’un modèle de fabrique utilisée pour obtenir des ressources DXGI comme la chaîne de permutation [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (mémoires tampons d’affichage). [  **ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), qui représente le périphérique graphique et ses ressources, s’obtient avec [**D3D11Device::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Pour les cibles de rendu, utilisez l’interface [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview). |
| **EGLContext**  | Dans Direct3D, utilisez l’interface [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) pour définir et envoyer des commandes au pipeline graphique.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **EGLConfig**   | Dans Direct3D 11, utilisez les méthodes de l’interface [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) pour créer et configurer les ressources graphiques, telles que les mémoires tampons, les textures, les gabarits et les nuanceurs.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

 

Voici maintenant la procédure générale pour créer un affichage graphique simple, des ressources et un contexte dans DXGI et Direct3D pour une application UWP.

1.  Obtenez un handle pour l’objet [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) requis pour le thread d’interface utilisateur principal de l’application. Pour cela, appelez la méthode [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread).
2.  Pour les applications UWP, demandez une chaîne de permutation à [**IDXGIAdapter2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2) à l’aide de la méthode [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow), puis transmettez-la à la référence [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) obtenue à l’étape 1. Vous recevez en retour une instance [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1). Définissez l’étendue de cette instance à l’objet convertisseur et au thread de rendu associé.
3.  Obtenez les instances [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) et [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) en appelant la méthode [**D3D11Device::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Définissez également l’étendue de ces instances à l’objet convertisseur.
4.  Créez les nuanceurs, textures et autres ressources à l’aide des méthodes sur l’objet [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) du convertisseur.
5.  Définissez les mémoires tampons, exécutez les nuanceurs et gérez les étapes du pipeline avec les méthodes sur l’objet [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) du convertisseur.
6.  Après l’exécution du pipeline et le dessin d’une trame dans la mémoire tampon d’arrière-plan, présentez la chaîne à l’écran avec la méthode [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).

Pour plus d’informations sur ce processus, consultez [Prise en main de DirectX Graphics](https://docs.microsoft.com/windows/desktop/getting-started-with-directx-graphics). La suite du présent article couvre la plupart des étapes que vous aurez généralement à effectuer pour configurer et gérer un pipeline graphique de base.
> **Remarque**    applications de bureau de Windows ont des API différentes pour obtenir une chaîne de permutation Direct3D, tel que [ **D3D11Device::CreateDeviceAndSwapChain**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdeviceandswapchain)et n’utilisez pas un [ **CoreWindow** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) objet.

 

## <a name="obtaining-a-window-for-display"></a>Obtention d’une fenêtre d’affichage


Dans l’exemple ci-dessous, un HWND est transmis à eglGetDisplay pour obtenir une ressource de fenêtre adaptée à la plateforme Microsoft Windows. D’autres plateformes, comme iOS d’Apple (Cocoa) et Android de Google, utilisent des handles ou références différents vers les ressources de fenêtre. La syntaxe d’appel requise peut donc varier. Une fois que vous avez obtenu un affichage, vous devez l’initialiser, définir la configuration par défaut, puis créer une surface avec une mémoire tampon d’arrière-plan destinée au dessin.

Obtention d’un affichage et configuration de ce dernier avec EGL :

``` syntax
// Obtain an EGL display object.
EGLDisplay display = eglGetDisplay(GetDC(hWnd));
if (display == EGL_NO_DISPLAY)
{
  return EGL_FALSE;
}

// Initialize the display
if (!eglInitialize(display, &majorVersion, &minorVersion))
{
  return EGL_FALSE;
}

// Obtain the display configs
if (!eglGetConfigs(display, NULL, 0, &numConfigs))
{
  return EGL_FALSE;
}

// Choose the display config
if (!eglChooseConfig(display, attribList, &config, 1, &numConfigs))
{
  return EGL_FALSE;
}

// Create a surface
surface = eglCreateWindowSurface(display, config, (EGLNativeWindowType)hWnd, NULL);
if (surface == EGL_NO_SURFACE)
{
  return EGL_FALSE;
}
```

Dans Direct3D, la fenêtre principale d’une application UWP est représentée par l’objet [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow), qui peut être obtenu à partir de l’objet application en appelant [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) dans le cadre du processus d’initialisation du « fournisseur d’affichage » créé pour Direct3D. (Si vous utilisez l’interopérabilité du XAML de Direct3D, vous utilisez fournisseur d’affichage de l’infrastructure XAML.) Le processus de création d’un fournisseur d’affichage Direct3D est traité dans [comment configurer votre application pour afficher une vue](https://docs.microsoft.com/previous-versions/windows/apps/hh465077(v=win.10)).

Obtention d’un objet CoreWindow pour Direct3D :

``` syntax
CoreWindow::GetForCurrentThread();
```

Après avoir obtenu la référence [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow), activez la fenêtre afin que celle-ci exécute la méthode **Run** de votre objet principal et commence le traitement de l’événement fenêtre. Après cela, créez un [ **ID3D11Device1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) et un [ **ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)et les utiliser pour obtenir sous-jacent [ **IDXGIDevice1** ](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgidevice1) et [ **IDXGIAdapter** ](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) afin que vous pouvez obtenir un [ **IDXGIFactory2** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) objet pour créer une ressource de chaîne de permutation selon votre [ **DXGI\_échange\_chaîne\_DESC1** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) configuration.

Configuration et définition de la chaîne de permutation DXGI sur l’objet CoreWindow pour Direct3D :

``` syntax
// Called when the CoreWindow object is created (or re-created).
void SimpleDirect3DApp::SetWindow(CoreWindow^ window)
{
  // Register event handlers with the CoreWindow object.
  // ...

  // Obtain your ID3D11Device1 and ID3D11DeviceContext1 objects
  // In this example, m_d3dDevice contains the scoped ID3D11Device1 object
  // ...

  ComPtr<IDXGIDevice1>  dxgiDevice;
  // Get the underlying DXGI device of the Direct3D device.
  m_d3dDevice.As(&dxgiDevice);

  ComPtr<IDXGIAdapter> dxgiAdapter;
  dxgiDevice->GetAdapter(&dxgiAdapter);

  ComPtr<IDXGIFactory2> dxgiFactory;
  dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2), 
    &dxgiFactory);

  DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
  swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
  swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
  swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
  swapChainDesc.Stereo = false;
  swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
  swapChainDesc.SampleDesc.Quality = 0;
  swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
  swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
  swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
  swapChainDesc.Flags = 0;

  // ...

  Windows::UI::Core::CoreWindow^ window = m_window.Get();
  dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr, // Allow on all displays.
    &m_swapChainCoreWindow);
}
```

Appelez la méthode [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) après avoir préparé la trame à afficher.

Notez que Direct3D 11 ne propose pas d’abstraction identique à EGLSurface. (Il est [ **IDXGISurface1**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1), mais elle est utilisée différemment.) L’approximation conceptuelle le plus proche est la [ **ID3D11RenderTargetView** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) que nous utilisons pour attribuer une texture de l’objet ([**ID3D11Texture2D** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)) en tant que la mémoire tampon d’arrière-plan qui dessine dans notre pipeline de nuanceur.

Configuration de la mémoire tampon d’arrière-plan pour la chaîne de permutation dans Direct3D 11 :

``` syntax
ComPtr<ID3D11RenderTargetView>    m_d3dRenderTargetViewWin; // scoped to renderer object

// ...

ComPtr<ID3D11Texture2D> backBuffer2;
    
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&backBuffer2));

m_d3dDevice->CreateRenderTargetView(
  backBuffer2.Get(),
  nullptr,
    &m_d3dRenderTargetViewWin);
```

Nous vous recommandons d’appeler ce code dès que vous créez une fenêtre ou modifiez une taille de fenêtre. Lors du processus de rendu, définissez l’affichage de la cible de rendu à l’aide de la méthode [**ID3D11DeviceContext1::OMSetRenderTargets**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets). Effectuez cette opération avant de configurer les autres sous-ressources, telles que les mémoires tampons de vertex ou les nuanceurs.

``` syntax
// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

## <a name="creating-a-rendering-context"></a>Création d’un contexte de rendu


Dans EGL 1.4, un « affichage » représente un ensemble de ressources de fenêtre. En règle générale, vous configurez une « surface » d’affichage en affectant un ensemble d’attributs à l’objet d’affichage et en obtenant une surface en retour. Pour afficher le contenu de la surface, vous devez créer un contexte, puis le lier à la surface et à l’affichage.

Votre flux d’appels se présente en principe comme suit :

-   Appelez eglGetDisplay avec le handle vers une ressource d’affichage ou de fenêtre, puis obtenez un objet d’affichage.
-   Initialisez l’affichage avec eglInitialize.
-   Obtenez les configurations d’affichage disponibles et sélectionnez-en une à l’aide des méthodes eglGetConfigs et eglChooseConfig.
-   Créez une surface d’affichage avec eglCreateWindowSurface.
-   Créez un contexte d’affichage pour le dessin en utilisant eglCreateContext.
-   Liez le contexte d’affichage à l’affichage et à la surface avec eglMakeCurrent.

Dans la section précédente, nous avons créé les objets EGLDisplay et EGLSurface. Nous allons maintenant utiliser EGLDisplay pour créer un contexte et l’associer à l’affichage, à l’aide de l’objet EGLSurface configuré pour paramétrer la sortie.

Obtention d’un contexte de rendu avec EGL 1.4 :

```cpp
// Configure your EGLDisplay and obtain an EGLSurface here ...
// ...

// Create a drawing context from the EGLDisplay
context = eglCreateContext(display, config, EGL_NO_CONTEXT, contextAttribs);
if (context == EGL_NO_CONTEXT)
{
  return EGL_FALSE;
}   
   
// Make the context current
if (!eglMakeCurrent(display, surface, surface, context))
{
  return EGL_FALSE;
}
```

Dans Direct3D 11, un contexte de rendu est représenté par deux objets : l’objet [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), qui représente la carte et sert à créer des ressources pour Direct3D (par exemple, les mémoires tampons d’arrière-plan et les nuanceurs) ; l’objet [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1), que vous utilisez pour gérer le pipeline graphique et exécuter les nuanceurs.

N’oubliez pas que les niveaux de fonctionnalité Direct3D sont destinés à garantir la prise en charge des versions antérieures des plateformes matérielles Direct3D (DirectX 9.1 à DirectX 11). De nombreuses plateformes fonctionnant avec du matériel vidéo à faible consommation d’énergie, comme les tablettes, n’ont accès qu’aux fonctionnalités DirectX 9.1. En outre, la prise en charge du matériel vidéo est assurée pour les versions 9.1 à 11.

Création d’un contexte de rendu avec DXGI et Direct3D

```cpp

// ... 

UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;
ComPtr<IDXGIDevice> dxgiDevice;

D3D_FEATURE_LEVEL featureLevels[] = 
{
        D3D_FEATURE_LEVEL_11_1,
        D3D_FEATURE_LEVEL_11_0,
        D3D_FEATURE_LEVEL_10_1,
        D3D_FEATURE_LEVEL_10_0,
        D3D_FEATURE_LEVEL_9_3,
        D3D_FEATURE_LEVEL_9_2,
        D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> d3dContext;

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &d3dContext // Returns the device immediate context.
);
```

## <a name="drawing-into-a-texture-or-pixmap-resource"></a>Dessin dans une ressource de type texture ou pixmap


Pour dessiner dans une texture avec OpenGL ES 2.0, configurez une mémoire tampon de pixels (PBuffer). Après avoir créé et configuré un EGLSurface pour cette mémoire tampon, affectez-lui un contexte de rendu, puis exécutez le pipeline de nuanceurs qui dessinera dans la texture.

Dessin dans une mémoire tampon de pixels avec OpenGL ES 2.0 :

``` syntax
// Create a pixel buffer surface to draw into
EGLConfig pBufConfig;
EGLint totalpBufAttrs;

const EGLint pBufConfigAttrs[] =
{
    // Configure the pBuffer here...
};
 
eglChooseConfig(eglDsplay, pBufConfigAttrs, &pBufConfig, 1, &totalpBufAttrs);
EGLSurface pBuffer = eglCreatePbufferSurface(eglDisplay, pBufConfig, EGL_TEXTURE_RGBA); 
```

Dans Direct3D 11, créez une ressource [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d), puis définissez-la en tant que cible de rendu. Configurer la cible de rendu à l’aide [ **D3D11\_restituer\_cible\_vue\_DESC**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc). Lorsque vous appelez le [ **ID3D11DeviceContext::Draw** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) (méthode) (ou un dessin similaire\* opération sur le contexte de périphérique) à l’aide de cette cible de rendu, les résultats sont dessinées dans une texture.

Dessin dans une texture avec Direct3D 11 :

``` syntax
ComPtr<ID3D11Texture2D> renderTarget1;

D3D11_RENDER_TARGET_VIEW_DESC renderTargetDesc = {0};
// Configure renderTargetDesc here ...

m_d3dDevice->CreateRenderTargetView(
  renderTarget1.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);

// Later, in your render loop...

// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

Cette texture peut être transmise à un nuanceur si elle est associée à un objet [**ID3D11ShaderResourceView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview).

## <a name="drawing-to-the-screen"></a>Dessin à l’écran


Une fois que vous avez configuré vos mémoires tampons et mis à jour vos données à l’aide de votre objet EGLContext, exécutez les nuanceurs liés à ce dernier, puis dessinez les résultats dans la mémoire tampon d’arrière-plan avec glDrawElements. Appelez eglSwapBuffers pour afficher la mémoire tampon d’arrière-plan.

Open GL ES 2.0 : Dessin à l’écran.

``` syntax
glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
```

Dans Direct3D 11, vous configurez vos mémoires tampons et liez les nuanceurs à l’aide de la méthode [**IDXGISwapChain::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1). Puis vous appelez une de la [ **ID3D11DeviceContext1::Draw** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) \* méthodes pour exécuter les nuanceurs et dessiner les résultats à une cible de rendu configurée en tant que la mémoire tampon d’arrière-plan pour la chaîne de permutation. Enfin, vous présentez la mémoire tampon d’arrière-plan à l’affichage en appelant la méthode **IDXGISwapChain::Present1**.

Direct3D 11 : Dessin à l’écran.

``` syntax

m_d3dContext->DrawIndexed(
        m_indexCount,
        0,
        0);

// ...

m_swapChainCoreWindow->Present1(1, 0, &parameters);
```

## <a name="releasing-graphics-resources"></a>Libération des ressources graphiques


Dans EGL, vous libérez les ressources de fenêtre en transmettant EGLDisplay à eglTerminate.

Arrêt d’un affichage avec EGL 1.4 :

```cpp
EGLBoolean eglTerminate(eglDisplay);
```

Dans une application UWP, vous pouvez fermer le CoreWindow avec [**CoreWindow::Close**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.close). Cela n’est possible que pour les fenêtres d’interface utilisateur secondaires. Le thread d’interface utilisateur principal et le CoreWindow associé ne peuvent pas être fermés de la sorte. C’est le système d’exploitation qui les classe comme ayant expiré. En revanche, la fermeture d’un CoreWindow secondaire déclenche l’événement [**CoreWindow::Closed**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.closed).

## <a name="api-reference-mapping-for-egl-to-direct3d-11"></a>Index de mappage des API EGL et Direct3D 11


| API EGL                          | API ou comportement Direct3D 11 similaire                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eglBindAPI                       | Non applicable                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglBindTexImage                  | Appelez [**ID3D11Device::CreateTexture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) pour définir une texture 2D.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglChooseConfig                  | Direct3D ne fournit pas de jeux de configurations par défaut pour les tampons de trame. Configuration de la chaîne de permutation.                                                                                                                                                                                                                                                                                                                                                                                           |
| eglCopyBuffers                   | Pour copier les données en mémoire tampon, appelez la méthode [**ID3D11DeviceContext::CopyStructureCount**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copystructurecount). Pour copier une ressource, appelez la méthode [**ID3DDeviceCOntext::CopyResource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource).                                                                                                                                                                                                                                                      |
| eglCreateContext                 | Créez un contexte de périphérique Direct3D en appelant [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice), qui renvoie un handle vers un périphérique Direct3D ainsi qu’un contexte immédiat Direct3D par défaut (objet [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)). Vous pouvez également créer un contexte Direct3D différé en appelant la méthode [**ID3D11Device2::CreateDeferredContext**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nf-d3d11_2-id3d11device2-createdeferredcontext2) sur l’objet [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) renvoyé. |
| eglCreatePbufferFromClientBuffer | Chaque mémoire tampon est écrite et lue comme une sous-ressource Direct3D ([**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d), par exemple). Pour copier un type de sous-ressource vers un autre type de sous-ressource compatible, utilisez une méthode telle qu’[**ID3D11DeviceContext1:CopyResource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource).                                                                                                                                                                                                     |
| eglCreatePbufferSurface          | Pour créer un périphérique Direct3D sans chaîne de permutation, appelez la méthode statique [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Pour créer un affichage de la cible de rendu Direct3D, appelez la méthode [**ID3D11Device::CreateRenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview).                                                                                                                                                                                                                               |
| eglCreatePixmapSurface           | Pour créer un périphérique Direct3D sans chaîne de permutation, appelez la méthode statique [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Pour créer un affichage de la cible de rendu Direct3D, appelez la méthode [**ID3D11Device::CreateRenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview).                                                                                                                                                                                                                               |
| eglCreateWindowSurface           | Obtenez un objet [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (pour les mémoires tampons d’affichage) et un objet [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) (interface virtuelle utilisée par le périphérique graphique et les ressources associées). Utilisez **ID3D11Device1** pour définir un objet [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) permettant ensuite de créer le tampon de trame requis par **IDXGISwapChain1**.                                                                                         |
| eglDestroyContext                | Non applicable Utilisez [**ID3D11DeviceContext::DiscardView1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-discardview1) pour supprimer un affichage de la cible de rendu. Pour fermer l’objet [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)parent, affectez la valeur null à l’instance et attendez que la plateforme demande les ressources nécessaires. Vous ne pouvez pas supprimer directement le contexte de périphérique.                                                                                                                                                |
| eglDestroySurface                | Non applicable Les ressources graphiques sont supprimées lorsque la plateforme ferme le CoreWindow dans l’application UWP.                                                                                                                                                                                                                                                                                                                                                                                                 |
| eglGetCurrentDisplay             | Appelez [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) pour obtenir une référence à la fenêtre principale actuelle de l’application.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetCurrentSurface             | Il s’agit de l’objet [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) actuel, dont l’étendue est généralement définie sur l’objet de votre convertisseur.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetError                      | Les erreurs sont obtenues sous forme de valeurs HRESULT, qui sont renvoyées par la plupart des méthodes sur les interfaces DirectX. Si la méthode ne renvoie aucune valeur HRESULT, appelez [**GetLastError**](https://docs.microsoft.com/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror). Pour convertir une erreur système en une valeur HRESULT, utilisez le [**HRESULT\_FROM\_WIN32**](https://docs.microsoft.com/windows/desktop/api/winerror/nf-winerror-hresult_from_win32) (macro).                                                                                                                                                                                                  |
| eglInitialize                    | Appelez [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) pour obtenir une référence à la fenêtre principale actuelle de l’application.                                                                                                                                                                                                                                                                                                                                                         |
| eglMakeCurrent                   | Définissez la cible de rendu à utiliser pour dessiner sur le contexte actuel avec [**ID3D11DeviceContext1::OMSetRenderTargets**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets).                                                                                                                                                                                                                                                                                                                                  |
| eglQueryContext                  | Non applicable Toutefois, à partir d’une instance [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), vous pouvez obtenir des cibles de rendu ainsi que certaines données de configuration. Pour obtenir une liste des méthodes disponibles, cliquez sur le lien.                                                                                                                                                                                                                                                                                           |
| eglQuerySurface                  | Non applicable Néanmoins, vous pouvez obtenir des données sur les fenêtres d’affichage et sur le matériel vidéo actuel à partir des méthodes d’une instance [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1). Pour obtenir une liste des méthodes disponibles, cliquez sur le lien.                                                                                                                                                                                                                                                                               |
| eglReleaseTexImage               | Non applicable                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglReleaseThread                 | Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                                                              |
| eglSurfaceAttrib                 | Utilisez [ **D3D11\_restituer\_cible\_vue\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc) pour configurer une vue de cible de rendu Direct3D,                                                                                                                                                                                                                                                                                                                                                               |
| eglSwapBuffers                   | Utilisez [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).                                                                                                                                                                                                                                                                                                                                                                                                                     |
| eglSwapInterval                  | Voir [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1).                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| eglTerminate                     | L’objet CoreWindow utilisé pour afficher la sortie du pipeline graphique est géré par le système d’exploitation.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglWaitClient                    | Pour les surfaces partagées, utilisez IDXGIKeyedMutex. Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitGL                        | Pour les surfaces partagées, utilisez IDXGIKeyedMutex. Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitNative                    | Pour les surfaces partagées, utilisez IDXGIKeyedMutex. Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |

 

 

 




