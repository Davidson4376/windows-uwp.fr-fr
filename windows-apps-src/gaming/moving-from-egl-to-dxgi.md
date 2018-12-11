---
title: Comparer le code EGL avec DXGI et Direct3D
description: L’interface graphique DirectX(DXGI) et certaines APIDirect3D jouent le même rôle qu’EGL. Cette rubrique vous aidera à comprendre le fonctionnement de DXGI et Direct3D11 sous l’angle d’EGL.
ms.assetid: 90f5ecf1-dd5d-fea3-bed8-57a228898d2a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, egl, dxgi, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 1279d5100aa00e1b94d7d56b472a0574d22c3416
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8875909"
---
# <a name="compare-egl-code-to-dxgi-and-direct3d"></a>Comparer le codeEGL avec DXGI et Direct3D




**API importantes**

-   [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)
-   [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)
-   [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)

L’interface graphique DirectX (DXGI) et certaines API Direct3D jouent le même rôle qu’EGL. Cette rubrique vous aidera à comprendre le fonctionnement de DXGI et Direct3D 11 sous l’ange d’EGL.

À l’instar d’EGL, DXGI et Direct3D fournissent diverses méthodes qui vous permettent de configurer des ressources graphiques, d’obtenir un contexte de rendu pour le dessin des nuanceurs et d’afficher les résultats dans une fenêtre. Toutefois, DXGI et Direct3D comportent quelques options supplémentaires qui nécessitent une configuration particulière dans le cadre du portage à partir d’EGL.

> **Remarque**  ce guide repose sur la spécification ouverte de Khronos Group pour EGL 1.4 disponibles ici: [\[PDF\ Khronos Native Platform Graphics Interface (EGL Version 1.4 - April 6, 2011)]](http://www.khronos.org/registry/egl/specs/eglspec.1.4.20110406.pdf). Les variantes de syntaxe propres à d’autres plateformes et langages de développement ne sont pas traitées dans cette rubrique.

 

## <a name="how-does-dxgi-and-direct3d-compare"></a>Comparaison avec DXGI et Direct3D


Avec EGL, il est relativement facile de commencer à dessiner dans une surface de fenêtre. C’est l’un des atouts d’EGL par rapport à DXGI et Direct3D. Cela s’explique par le fait qu’OpenGLES2.0 (et donc EGL) est une spécification implémentée par de nombreux fournisseurs de plateforme, alors que DXGI et Direct3D constituent une référence unique à laquelle les pilotes des fournisseurs de matériel doivent se conformer. Microsoft doit donc implémenter un ensemble d’API capables de prendre en charge le plus de fonctionnalités tierces possibles, au lieu de se limiter à un groupe de fonctionnalités d’un fournisseur donné ou encore de combiner des commandes d’installation propres à un fournisseur dans des API plus simples. À l’inverse, Direct3D offre un ensemble unique d’API capables de prendre en charge une très grande variété de plateformes matérielles vidéo et de niveaux de fonctionnalité. En cela, Direct3D se montre plus flexible pour les développeurs sachant parfaitement utiliser la plateforme en question.

Comme EGL, DXGI et Direct3D fournissent des API pour les opérations suivantes :

-   Obtention des données, et lecture/écriture de ces données dans un tampon de trame (appelé « chaîne de permutation » dans DXGI).
-   Association du tampon de trame avec une fenêtre d’interface utilisateur.
-   Obtention et configuration des contextes de rendu destinés au dessin.
-   Envoi des commandes au pipeline graphique pour un contexte de rendu spécifique.
-   Création et gestion des ressources des nuanceurs, et association de ces dernières avec un contexte de rendu.
-   Affichage sur des cibles de rendu spécifiques (telles que les textures).
-   Actualisation de la surface d’affichage de la fenêtre afin de présenter les résultats du rendu avec les ressources graphiques.

Pour voir le processus général de configuration du pipeline graphique Direct3D, consultez le modèle application DirectX 11 (Windows universel) dans Microsoft Visual Studio2015. La classe de rendu de base fournie dans ce modèle peut servir de référence pour créer l’infrastructure graphique Direct3D 11 et y configurer les principales ressources nécessaires. Elle offre également une prise en charge de certaines fonctionnalités des applications de plateforme Windows universelle (UWP), telles que la rotation de l’écran.

EGL comprend très peu d’API en comparaison de Direct3D 11. La navigation entre les différentes API de Direct3D 11 peut se révéler compliquée si vous n’êtes pas familiarisé avec les noms et le langage technique propres à cette plateforme. Nous vous proposons ici une vue d’ensemble pour vous aider à démarrer.

En premier lieu, sachez identifier à quel élément d’interface Direct3D correspond chaque objet EGL de base :

| Abstraction EGL | Représentation Direct3D similaire                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EGLDisplay**  | Dans Direct3D (pour les applications UWP), le handle d’affichage est fourni par l’API [**Windows::UI::CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) (ou l’interface **ICoreWindowInterop** qui expose le HWND). La carte et le matériel sont configurés par le biais des interfaces COM [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) et [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543), respectivement.                                                                                                                                                                                                                                                           |
| **EGLSurface**  | Dans Direct3D, les mémoires tampons et diverses autres ressources de fenêtre (visibles ou hors écran) sont créées et configurées par des interfaces DXGI spécifiques, parmi lesquelles [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556), qui est une implémentation d’un modèle de fabrique utilisée pour obtenir des ressources DXGI comme la chaîne de permutation [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) (mémoires tampons d’affichage). [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575), qui représente le périphérique graphique et ses ressources, s’obtient avec [**D3D11Device::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Pour les cibles de rendu, utilisez l’interface [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582). |
| **EGLContext**  | Dans Direct3D, utilisez l’interface [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) pour définir et envoyer des commandes au pipeline graphique.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **EGLConfig**   | Dans Direct3D 11, utilisez les méthodes de l’interface [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) pour créer et configurer les ressources graphiques, telles que les mémoires tampons, les textures, les gabarits et les nuanceurs.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

 

Voici maintenant la procédure générale pour créer un affichage graphique simple, des ressources et un contexte dans DXGI et Direct3D pour une application UWP.

1.  Obtenez un handle pour l’objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) requis pour le thread d’interface utilisateur principal de l’application. Pour cela, appelez la méthode [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589).
2.  Pour les applications UWP, demandez une chaîne de permutation à [**IDXGIAdapter2**](https://msdn.microsoft.com/library/windows/desktop/hh404537) à l’aide de la méthode [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559), puis transmettez-la à la référence [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) obtenue à l’étape 1. Vous recevez en retour une instance [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631). Définissez l’étendue de cette instance à l’objet convertisseur et au thread de rendu associé.
3.  Obtenez les instances [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) et [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) en appelant la méthode [**D3D11Device::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Définissez également l’étendue de ces instances à l’objet convertisseur.
4.  Créez les nuanceurs, textures et autres ressources à l’aide des méthodes sur l’objet [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) du convertisseur.
5.  Définissez les mémoires tampons, exécutez les nuanceurs et gérez les étapes du pipeline avec les méthodes sur l’objet [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) du convertisseur.
6.  Après l’exécution du pipeline et le dessin d’une trame dans la mémoire tampon d’arrière-plan, présentez la chaîne à l’écran avec la méthode [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797).

Pour plus d’informations sur ce processus, consultez [Prise en main de DirectX Graphics](https://msdn.microsoft.com/library/windows/desktop/hh309467). La suite du présent article couvre la plupart des étapes que vous aurez généralement à effectuer pour configurer et gérer un pipeline graphique de base.
> **Remarque**  applications de bureau Windows ont différentes API pour obtenir une chaîne de permutation Direct3D, telles que [**D3D11Device::CreateDeviceAndSwapChain**](https://msdn.microsoft.com/library/windows/desktop/ff476083)et n’utilisez pas un objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) .

 

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

Dans Direct3D, la fenêtre principale d’une application UWP est représentée par l’objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225), qui peut être obtenu à partir de l’objet application en appelant [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) dans le cadre du processus d’initialisation du « fournisseur d’affichage » créé pour Direct3D. (Avec l’interopérabilité Direct3D-XAML, vous utilisez le fournisseur d’affichage de l’infrastructure XAML.) Pour plus d’informations sur la création d’un fournisseur d’affichage Direct3D, voir la rubrique [Comment configurer votre application pour présenter un affichage](https://msdn.microsoft.com/library/windows/apps/hh465077).

Obtention d’un objet CoreWindow pour Direct3D:

``` syntax
CoreWindow::GetForCurrentThread();
```

Après avoir obtenu la référence [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225), activez la fenêtre afin que celle-ci exécute la méthode **Run** de votre objet principal et commence le traitement de l’événement fenêtre. Ensuite, créez un objet [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) et un objet [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598), et utilisez-les pour obtenir les objets [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/ff471331) et [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) sous-jacents. Ces derniers vous serviront à obtenir l’objet [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) requis pour créer une ressource de chaîne de permutation basée sur la configuration de votre objet [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528).

Configuration et définition de la chaîne de permutation DXGI sur l’objet CoreWindow pour Direct3D:

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

Appelez la méthode [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) après avoir préparé la trame à afficher.

Notez que Direct3D11 ne propose pas d’abstraction identique à EGLSurface. ([**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343) existe, mais son utilisation diffère.) La représentation conceptuelle la plus proche est l’objet [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582), qui permet de définir une texture ([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)) en tant que mémoire tampon d’arrière-plan dans laquelle le pipeline de nuanceurs pourra dessiner.

Configuration de la mémoire tampon d’arrière-plan pour la chaîne de permutation dans Direct3D11:

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

Nous vous recommandons d’appeler ce code dès que vous créez une fenêtre ou modifiez une taille de fenêtre. Lors du processus de rendu, définissez l’affichage de la cible de rendu à l’aide de la méthode [**ID3D11DeviceContext1::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464). Effectuez cette opération avant de configurer les autres sous-ressources, telles que les mémoires tampons de vertex ou les nuanceurs.

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

Dans Direct3D 11, un contexte de rendu est représenté par deux objets : l’objet [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575), qui représente la carte et sert à créer des ressources pour Direct3D (par exemple, les mémoires tampons d’arrière-plan et les nuanceurs) ; l’objet [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598), que vous utilisez pour gérer le pipeline graphique et exécuter les nuanceurs.

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

Dans Direct3D 11, créez une ressource [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635), puis définissez-la en tant que cible de rendu. Configurez la cible de rendu avec [**D3D11\_RENDER\_TARGET\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476201). Lorsque vous appelez la méthode [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407) (ou une opération Draw\* similaire sur le contexte de périphérique) avec cette cible de rendu, les résultats sont présentés sous forme de dessin dans une texture.

Dessin dans une texture avec Direct3D11:

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

Cette texture peut être transmise à un nuanceur si elle est associée à un objet [**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628).

## <a name="drawing-to-the-screen"></a>Dessin à l’écran


Une fois que vous avez configuré vos mémoires tampons et mis à jour vos données à l’aide de votre objet EGLContext, exécutez les nuanceurs liés à ce dernier, puis dessinez les résultats dans la mémoire tampon d’arrière-plan avec glDrawElements. Appelez eglSwapBuffers pour afficher la mémoire tampon d’arrière-plan.

Dessin à l’écran avec OpenGL ES 2.0 :

``` syntax
glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
```

Dans Direct3D 11, vous configurez vos mémoires tampons et liez les nuanceurs à l’aide de la méthode [**IDXGISwapChain::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797). Vous appelez ensuite l’une des méthodes [**ID3D11DeviceContext1::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407)\* pour exécuter les nuanceurs et dessiner les résultats sur la cible de rendu configurée en tant que mémoire tampon d’arrière-plan pour la chaîne de permutation. Enfin, vous présentez la mémoire tampon d’arrière-plan à l’affichage en appelant la méthode **IDXGISwapChain::Present1**.

Dessin à l’écran avec Direct3D11:

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

Dans une application UWP, vous pouvez fermer le CoreWindow avec [**CoreWindow::Close**](https://msdn.microsoft.com/library/windows/apps/br208260). Cela n’est possible que pour les fenêtres d’interface utilisateur secondaires. Le thread d’interface utilisateur principal et le CoreWindow associé ne peuvent pas être fermés de la sorte. C’est le système d’exploitation qui les classe comme ayant expiré. En revanche, la fermeture d’un CoreWindow secondaire déclenche l’événement [**CoreWindow::Closed**](https://msdn.microsoft.com/library/windows/apps/br208261).

## <a name="api-reference-mapping-for-egl-to-direct3d-11"></a>Index de mappage des API EGL et Direct3D11


| API EGL                          | API ou comportement Direct3D 11 similaire                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eglBindAPI                       | Non applicable                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglBindTexImage                  | Appelez [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521) pour définir une texture 2D.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglChooseConfig                  | Direct3D ne fournit pas de jeux de configurations par défaut pour les tampons de trame. Configuration de la chaîne de permutation.                                                                                                                                                                                                                                                                                                                                                                                           |
| eglCopyBuffers                   | Pour copier les données en mémoire tampon, appelez la méthode [**ID3D11DeviceContext::CopyStructureCount**](https://msdn.microsoft.com/library/windows/desktop/ff476393). Pour copier une ressource, appelez la méthode [**ID3DDeviceCOntext::CopyResource**](https://msdn.microsoft.com/library/windows/desktop/ff476392).                                                                                                                                                                                                                                                      |
| eglCreateContext                 | Créez un contexte de périphérique Direct3D en appelant [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082), qui renvoie un handle vers un périphérique Direct3D ainsi qu’un contexte immédiat Direct3D par défaut (objet [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)). Vous pouvez également créer un contexte Direct3D différé en appelant la méthode [**ID3D11Device2::CreateDeferredContext**](https://msdn.microsoft.com/library/windows/desktop/dn280495) sur l’objet [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) renvoyé. |
| eglCreatePbufferFromClientBuffer | Chaque mémoire tampon est écrite et lue comme une sous-ressource Direct3D ([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635), par exemple). Pour copier un type de sous-ressource vers un autre type de sous-ressource compatible, utilisez une méthode telle qu’[**ID3D11DeviceContext1:CopyResource**](https://msdn.microsoft.com/library/windows/desktop/ff476392).                                                                                                                                                                                                     |
| eglCreatePbufferSurface          | Pour créer un périphérique Direct3D sans chaîne de permutation, appelez la méthode statique [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Pour créer un affichage de la cible de rendu Direct3D, appelez la méthode [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517).                                                                                                                                                                                                                               |
| eglCreatePixmapSurface           | Pour créer un périphérique Direct3D sans chaîne de permutation, appelez la méthode statique [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Pour créer un affichage de la cible de rendu Direct3D, appelez la méthode [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517).                                                                                                                                                                                                                               |
| eglCreateWindowSurface           | Obtenez un objet [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) (pour les mémoires tampons d’affichage) et un objet [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) (interface virtuelle utilisée par le périphérique graphique et les ressources associées). Utilisez **ID3D11Device1** pour définir un objet [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) permettant ensuite de créer le tampon de trame requis par **IDXGISwapChain1**.                                                                                         |
| eglDestroyContext                | Non applicable Utilisez [**ID3D11DeviceContext::DiscardView1**](https://msdn.microsoft.com/library/windows/desktop/jj247573) pour supprimer un affichage de la cible de rendu. Pour fermer l’objet [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)parent, affectez la valeur null à l’instance et attendez que la plateforme demande les ressources nécessaires. Vous ne pouvez pas supprimer directement le contexte de périphérique.                                                                                                                                                |
| eglDestroySurface                | Non applicable. Les ressources graphiques sont supprimées lorsque la plateforme ferme le CoreWindow dans l’application UWP.                                                                                                                                                                                                                                                                                                                                                                                                 |
| eglGetCurrentDisplay             | Appelez [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) pour obtenir une référence à la fenêtre principale actuelle de l’application.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetCurrentSurface             | Il s’agit de l’objet [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) actuel, dont l’étendue est généralement définie sur l’objet de votre convertisseur.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetError                      | Les erreurs sont obtenues sous forme de valeurs HRESULT, qui sont renvoyées par la plupart des méthodes sur les interfaces DirectX. Si la méthode ne renvoie aucune valeur HRESULT, appelez [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360). Pour convertir une erreur système en anHRESULTvalue, utilisez la[**HRESULT\_FROM\_WIN32**](https://msdn.microsoft.com/library/windows/desktop/ms680746)macro.                                                                                                                                                                                                  |
| eglInitialize                    | Appelez [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) pour obtenir une référence à la fenêtre principale actuelle de l’application.                                                                                                                                                                                                                                                                                                                                                         |
| eglMakeCurrent                   | Définissez la cible de rendu à utiliser pour dessiner sur le contexte actuel avec [**ID3D11DeviceContext1::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464).                                                                                                                                                                                                                                                                                                                                  |
| eglQueryContext                  | Non applicable Toutefois, à partir d’une instance [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575), vous pouvez obtenir des cibles de rendu ainsi que certaines données de configuration. Pour obtenir une liste des méthodes disponibles, cliquez sur le lien.                                                                                                                                                                                                                                                                                           |
| eglQuerySurface                  | Non applicable Néanmoins, vous pouvez obtenir des données sur les fenêtres d’affichage et sur le matériel vidéo actuel à partir des méthodes d’une instance [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575). Pour obtenir une liste des méthodes disponibles, cliquez sur le lien.                                                                                                                                                                                                                                                                               |
| eglReleaseTexImage               | Non applicable                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglReleaseThread                 | Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                                                              |
| eglSurfaceAttrib                 | Utilisez [**D3D11\_RENDER\_TARGET\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476201) pour configurer un affichage de la cible de rendu Direct3D.                                                                                                                                                                                                                                                                                                                                                               |
| eglSwapBuffers                   | Utilisez [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797).                                                                                                                                                                                                                                                                                                                                                                                                                     |
| eglSwapInterval                  | Voir [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631).                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| eglTerminate                     | L’objet CoreWindow utilisé pour afficher la sortie du pipeline graphique est géré par le système d’exploitation.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglWaitClient                    | Pour les surfaces partagées, utilisez IDXGIKeyedMutex. Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitGL                        | Pour les surfaces partagées, utilisez IDXGIKeyedMutex. Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitNative                    | Pour les surfaces partagées, utilisez IDXGIKeyedMutex. Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                    |

 

 

 




