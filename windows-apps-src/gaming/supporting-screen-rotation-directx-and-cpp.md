---
title: Prise en charge de l’orientation de l’écran (DirectX et C++)
description: Rubrique, nous allons examiner les meilleures pratiques en matière de gestion de la rotation écran dans votre application UWP DirectX, afin que le matériel graphique de l’appareil Windows 10 sont utilisées efficacement.
ms.assetid: f23818a6-e372-735d-912b-89cabeddb6d4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, orientation de l’écran, directx
ms.localizationpriority: medium
ms.openlocfilehash: eb86cfaefe7112d408a17a54bf4f4b482c218be8
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8886861"
---
# <a name="supporting-screen-orientation-directx-and-c"></a>Prise en charge de l’orientation de l’écran (DirectX et C++)



Votre application de plateforme Windows universelle (UWP) prend en charge plusieurs orientations d’écran lorsque vous gérez l’événement [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268). Rubrique, nous allons examiner les meilleures pratiques en matière de gestion de la rotation écran dans votre application UWP DirectX, afin que le matériel graphique de l’appareil Windows 10 sont utilisées efficacement.

Avant de commencer, souvenez-vous que le matériel graphique génère toujours des données de pixel de la même manière, quelle que soit l’orientation de l’appareil. Les appareils Windows 10 peuvent déterminer leur orientation d’affichage actuelle (à l’aide un capteur ou d’une bascule logicielle) et permettre aux utilisateurs de modifier les paramètres d’affichage. En raison de cette option, Windows 10 lui-même gère la rotation des images pour vous assurer qu’ils sont «dans le bon sens» en fonction de l’orientation de l’appareil. Par défaut, votre application reçoit une notification signalant que quelque chose a changé d’orientation, par exemple une taille de fenêtre. Lorsque cela se produit, Windows 10 fait pivoter immédiatement l’image pour l’affichage final. Pour trois des quatre orientations de l’écran spécifiques (discutées plus bas), Windows 10 utilise puissance de calcul et des ressources graphiques supplémentaires pour afficher l’image finale.

Pour les applications DirectX UWP, l’objet [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258) fournit des données d’orientation de l’affichage de base qui peuvent être interrogées par votre application. L’orientation par défaut est *paysage*, où la largeur en pixels de l’affichage est supérieure à sa hauteur ; l’autre orientation est *portrait*, où l’affichage subit une rotation de 90 degrés dans l’un ou l’autre sens et la largeur devient inférieure à la hauteur.

Windows 10 définit quatre modes d’orientation d’affichage spécifique:

-   Paysage: la valeur par défaut pour Windows 10 orientation de l’affichage et est considéré comme l’angle de base ou d’identité pour la rotation (0 degré).
-   Portrait: l’affichage a subi une rotation de 90degrés dans le sens des aiguilles d’une montre (ou de 270degrés dans le sens inverse).
-   Paysage (renversé): l’affichage a subi une rotation de 180degrés (retournement).
-   Portrait (renversé): l’affichage a subi une rotation de 270degrés dans le sens des aiguilles d’une montre (ou de 90degrés dans le sens inverse).

Lorsque l’affichage bascule d’une orientation à une autre, Windows 10 effectue en interne une opération de rotation pour aligner l’image dessinée avec la nouvelle orientation et l’utilisateur voit une image à l’endroit à l’écran.

En outre, Windows 10 affiche des animations de transition automatique afin de créer une expérience utilisateur FLUIDE lors du basculement d’une orientation à une autre. Le changement d’orientation de l’affichage est présenté à l’utilisateur sous la forme d’une animation de rotation et de zoom fixe de l’image d’écran affichée. Délai est accordé à l’application pour la disposition de la nouvelle orientation par Windows 10.

Globalement, voici le processus général de gestion des modifications de l’orientation de l’écran :

1.  Utilisez une combinaison des valeurs de limites de fenêtre et des données d’orientation de l’écran pour maintenir la chaîne d’échange alignée avec l’orientation d’affichage native de l’appareil.
2.  Signaler à Windows 10 de l’orientation de la chaîne d’échange à l’aide de [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801).
3.  Modifiez le code de rendu pour générer des images alignées avec l’orientation de l’appareil choisie par l’utilisateur.

## <a name="resizing-the-swap-chain-and-pre-rotating-its-contents"></a>Redimensionnement de la chaîne d’échange et prérotation de son contenu


Pour effectuer un redimensionnement de base de l’affichage dans votre application DirectX UWP et une prérotation de son contenu, procédez comme suit :

1.  Gérez l’événement [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268).
2.  Redimensionnez la chaîne d’échange aux nouvelles dimensions de la fenêtre.
3.  Appelez [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) pour définir l’orientation de la chaîne d’échange.
4.  Recréez toutes les ressources qui dépendent de la taille de la fenêtre, telles que vos cibles de rendu et autres mémoires tampons de données de pixels.

Examinons maintenant ces étapes un peu plus en détail.

La première étape consiste à enregistrer un gestionnaire pour l’événement [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268). Cet événement est déclenché dans votre application chaque fois que l’orientation de l’écran change, par exemple lorsque l’écran subit une rotation.

Pour gérer l’événement [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268), vous devez connecter votre gestionnaire pour **DisplayInformation::OrientationChanged** dans la méthode [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) requise, qui est l’une des méthodes de l’interface [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) que votre fournisseur d’affichage doit mettre en œuvre.

Dans cet exemple de code, le gestionnaire d’événements pour [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) est une méthode nommée **OnOrientationChanged**. Lorsque l’événement **DisplayInformation::OrientationChanged** est déclenché, il appelle à son tour une méthode nommée **SetCurrentOrientation** qui appelle ensuite **CreateWindowSizeDependentResources**.

```cpp
void App::SetWindow(CoreWindow^ window)
{
  // ... Other UI event handlers assigned here ...
  
    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);

  // ...
}
}
```

```cpp
void App::OnOrientationChanged(DisplayInformation^ sender, Object^ args)
{
    m_deviceResources->SetCurrentOrientation(sender->CurrentOrientation);
    m_main->CreateWindowSizeDependentResources();
}

// This method is called in the event handler for the OrientationChanged event.
void DX::DeviceResources::SetCurrentOrientation(DisplayOrientations currentOrientation)
{
    if (m_currentOrientation != currentOrientation)
    {
        m_currentOrientation = currentOrientation;
        CreateWindowSizeDependentResources();
    }
}
```

Ensuite, vous redimensionnez la chaîne d’échange pour la nouvelle orientation de l’écran et la préparez pour la rotation du contenu du pipeline graphique lorsque le rendu est effectué. Dans cet exemple, **DirectXBase::CreateWindowSizeDependentResources** est une méthode qui gère l’appel d’IDXGISwapChain::ResizeBuffers, la définition de matrices de rotation 3D et 2D, l’appel de SetRotation et la recréation de vos ressources.

```cpp
void DX::DeviceResources::CreateWindowSizeDependentResources() 
{
    // Clear the previous window size specific context.
    ID3D11RenderTargetView* nullViews[] = {nullptr};
    m_d3dContext->OMSetRenderTargets(ARRAYSIZE(nullViews), nullViews, nullptr);
    m_d3dRenderTargetView = nullptr;
    m_d2dContext->SetTarget(nullptr);
    m_d2dTargetBitmap = nullptr;
    m_d3dDepthStencilView = nullptr;
    m_d3dContext->Flush();

    // Calculate the necessary render target size in pixels.
    m_outputSize.Width = DX::ConvertDipsToPixels(m_logicalSize.Width, m_dpi);
    m_outputSize.Height = DX::ConvertDipsToPixels(m_logicalSize.Height, m_dpi);
    
    // Prevent zero size DirectX content from being created.
    m_outputSize.Width = max(m_outputSize.Width, 1);
    m_outputSize.Height = max(m_outputSize.Height, 1);

    // The width and height of the swap chain must be based on the window's
    // natively-oriented width and height. If the window is not in the native
    // orientation, the dimensions must be reversed.
    DXGI_MODE_ROTATION displayRotation = ComputeDisplayRotation();

    bool swapDimensions = displayRotation == DXGI_MODE_ROTATION_ROTATE90 || displayRotation == DXGI_MODE_ROTATION_ROTATE270;
    m_d3dRenderTargetSize.Width = swapDimensions ? m_outputSize.Height : m_outputSize.Width;
    m_d3dRenderTargetSize.Height = swapDimensions ? m_outputSize.Width : m_outputSize.Height;

    if (m_swapChain != nullptr)
    {
        // If the swap chain already exists, resize it.
        HRESULT hr = m_swapChain->ResizeBuffers(
            2, // Double-buffered swap chain.
            lround(m_d3dRenderTargetSize.Width),
            lround(m_d3dRenderTargetSize.Height),
            DXGI_FORMAT_B8G8R8A8_UNORM,
            0
            );

        if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
        {
            // If the device was removed for any reason, a new device and swap chain will need to be created.
            HandleDeviceLost();

            // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
            // and correctly set up the new device.
            return;
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    else
    {
        // Otherwise, create a new one using the same adapter as the existing Direct3D device.
        DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

        swapChainDesc.Width = lround(m_d3dRenderTargetSize.Width); // Match the size of the window.
        swapChainDesc.Height = lround(m_d3dRenderTargetSize.Height);
        swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
        swapChainDesc.Stereo = false;
        swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
        swapChainDesc.SampleDesc.Quality = 0;
        swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
        swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
        swapChainDesc.Flags = 0;    
        swapChainDesc.Scaling = DXGI_SCALING_NONE;
        swapChainDesc.AlphaMode = DXGI_ALPHA_MODE_IGNORE;

        // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
        ComPtr<IDXGIDevice3> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForCoreWindow(
                m_d3dDevice.Get(),
                reinterpret_cast<IUnknown*>(m_window.Get()),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }

    // Set the proper orientation for the swap chain, and generate 2D and
    // 3D matrix transformations for rendering to the rotated swap chain.
    // Note the rotation angle for the 2D and 3D transforms are different.
    // This is due to the difference in coordinate spaces.  Additionally,
    // the 3D matrix is specified explicitly to avoid rounding errors.

    switch (displayRotation)
    {
    case DXGI_MODE_ROTATION_IDENTITY:
        m_orientationTransform2D = Matrix3x2F::Identity();
        m_orientationTransform3D = ScreenRotation::Rotation0;
        break;

    case DXGI_MODE_ROTATION_ROTATE90:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(90.0f) *
            Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
        m_orientationTransform3D = ScreenRotation::Rotation270;
        break;

    case DXGI_MODE_ROTATION_ROTATE180:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(180.0f) *
            Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
        m_orientationTransform3D = ScreenRotation::Rotation180;
        break;

    case DXGI_MODE_ROTATION_ROTATE270:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(270.0f) *
            Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
        m_orientationTransform3D = ScreenRotation::Rotation90;
        break;

    default:
        throw ref new FailureException();
    }


    //SDM: only instance of SetRotation
    DX::ThrowIfFailed(
        m_swapChain->SetRotation(displayRotation)
        );

    // Create a render target view of the swap chain back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );

    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT, 
        lround(m_d3dRenderTargetSize.Width),
        lround(m_d3dRenderTargetSize.Height),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
            &depthStencilDesc,
            nullptr,
            &depthStencil
            )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2D);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
            depthStencil.Get(),
            &depthStencilViewDesc,
            &m_d3dDepthStencilView
            )
        );
    
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);

    // Create a Direct2D target bitmap associated with the
    // swap chain back buffer and set it as the current target.
    D2D1_BITMAP_PROPERTIES1 bitmapProperties = 
        D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
            m_dpi,
            m_dpi
            );

    ComPtr<IDXGISurface2> dxgiBackBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiBackBuffer))
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiBackBuffer.Get(),
            &bitmapProperties,
            &m_d2dTargetBitmap
            )
        );

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());

    // Grayscale text anti-aliasing is recommended for all UWP apps.
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

}

```

Après avoir enregistré les valeurs actuelles de hauteur et de largeur de la fenêtre pour le prochain appel de cette méthode, convertissez les valeurs DIP (Device Independent Pixel) pour les limites d’affichage en pixels. Dans l’exemple, vous appelez **ConvertDipsToPixels**, qui est une fonction simple qui exécute le code suivant :

` floor((dips * dpi / 96.0f) + 0.5f);`

Vous ajoutez 0,5f pour garantir un arrondi à la valeur entière la plus proche.

Notez en passant que les coordonnées [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) sont toujours définies en DIP. Pour Windows 10 et versions antérieures de Windows, un DIP est défini comme 1/96e de pouce et aligné à définition du système d’exploitation de *distance*. Lorsque l’orientation de l’affichage bascule en mode portrait, l’application inverse la hauteur et la largeur de l’objet **CoreWindow** et la taille (limites) de la cible de rendu doit changer en conséquence. Les coordonnées Direct3D étant toujours en pixels physiques, vous devez effectuer une conversion des valeurs DIP de **CoreWindow** en valeurs de pixels entières avant de passer ces valeurs à Direct3D pour configurer la chaîne d’échange.

Du point de vue du traitement, vous effectuez un peu plus de travail que si vous redimensionniez simplement la chaîne d’échange: en fait, vous faites pivoter les composants Direct2D et Direct3D de votre image avant de les composer pour la présentation et vous signalez à la chaîne d’échange que vous avez rendu les résultats dans une nouvelle orientation. Voici quelques détails supplémentaires sur ce processus, comme illustré dans l’exemple de code pour **DX::DeviceResources::CreateWindowSizeDependentResources** :

-   Déterminez la nouvelle orientation de l’affichage. Si l’affichage a basculé de paysage à portrait, ou inversement, permutez les valeurs de hauteur et de largeur (converties de valeurs DIP en pixels, bien entendu) pour les limites d’affichage.

-   Ensuite, vérifiez si la chaîne d’échange a été créée. Si ce n’est pas le cas, créez-la en appelant [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559). Autrement, redimensionnez les mémoires tampons de la chaîne d’échange existante aux nouvelles dimensions d’affichage en appelant [**IDXGISwapchain:ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577). Bien qu’il ne soit pas obligatoire de redimensionner la chaîne d’échange pour l’événement de rotation (après tout, vous générez le contenu déjà pivoté par votre pipeline de rendu), d’autres événements de changement de taille, tels que des événements d’alignement et de remplissage, exigent un redimensionnement.

-   Après cela, définissez la transformation matricielle 2D ou 3D appropriée à appliquer aux pixels ou vertex (respectivement) dans le pipeline graphique lors de leur rendu dans la chaîne d’échange. Nous avons quatre matrices de rotation possibles:

    -   Paysage (DXGI\_MODE\_ROTATION\_IDENTITY)
    -   Portrait (DXGI\_MODE\_ROTATION\_ROTATE270)
    -   Paysage (renversé) (DXGI\_MODE\_ROTATION\_ROTATE180)
    -   Portrait (renversé) (DXGI\_MODE\_ROTATION\_ROTATE90)

    La matrice correcte est sélectionnée basé sur les données fournies par Windows 10 (par exemple, les résultats de [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268)) pour déterminer l’orientation de l’affichage, et elle sera multipliée par les coordonnées de chaque pixel (Direct2D) ou de vertex (Direct3D) dans la scène, efficacement rotation leur alignement avec l’orientation de l’écran. (Notez que dans Direct2D, l’origine de l’écran est définie comme le coin supérieur gauche, alors que dans Direct3D, elle est définie comme le centre logique de la fenêtre.)

> **Remarque**  pour plus d’informations sur les transformations 2D utilisées pour la rotation et comment les définir, voir [les matrices de définition pour la rotation de l’écran (2D)](#appendix-a-applying-matrices-for-screen-rotation-2-d). Pour plus d’informations sur les transformations 3D utilisées pour la rotation, voir [Définition de matrices pour la rotation de l’écran (3D)](#appendix-b-applying-matrices-for-screen-rotation-3-d).

 

Nous en arrivons maintenant au point important : appelez [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) et fournissez-lui votre matrice de rotation mise à jour, comme ceci :

`m_swapChain->SetRotation(rotation);`

Vous stockez également la matrice de rotation sélectionnée là où votre méthode de rendu peut l’obtenir lorsqu’elle calcule la nouvelle projection. Vous utiliserez cette matrice lors du rendu de votre projection 3D finale ou de la composition de votre disposition 2D finale. (Elle n’est pas appliquée automatiquement pour vous.)

Après cela, créez une nouvelle cible de rendu pour l’affichage 3D pivoté, ainsi qu’une nouvelle mémoire tampon de gabarit de profondeur pour l’affichage. Définissez la fenêtre d’affichage de rendu 3D pour la scène pivotée en appelant [**ID3D11DeviceContext:RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480).

Pour finir, si vous avez des images 2D à faire pivoter ou à disposer, créez une cible de rendu 2D en tant que bitmap accessible en écriture pour la chaîne d’échange redimensionnée à l’aide de [**ID2D1DeviceContext::CreateBitmapFromDxgiSurface**](https://msdn.microsoft.com/library/windows/desktop/hh404482) et composez votre nouvelle disposition pour l’orientation mise à jour. Définissez les propriétés dont vous avez besoin sur la cible de rendu, telles que le mode anticrénelage (comme illustré dans l’exemple de code).

Maintenant, présentez la chaîne d’échange.

## <a name="reduce-the-rotation-delay-by-using-corewindowresizemanager"></a>Réduire le délai de rotation à l’aide de CoreWindowResizeManager


Par défaut, Windows 10 fournit un bref mais remarquable le laps de temps pour n’importe quelle application, quel que soit le langage pour effectuer la rotation de l’image ou le modèle d’application. Il est toutefois probable que si votre application effectue le calcul de rotation à l’aide de l’une des techniques décrites ici, cette rotation sera terminée bien avant la fin de ce laps de temps. Vous souhaiteriez récupérer ce temps et terminer l’animation de rotation, n’est-ce pas ? C’est ici que [**CoreWindowResizeManager**](https://msdn.microsoft.com/library/windows/apps/jj215603) entre en jeu.

Voici comment utiliser [**CoreWindowResizeManager**](https://msdn.microsoft.com/library/windows/apps/jj215603) : quand un événement [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) est déclenché, appelez [**CoreWindowResizeManager::GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh404170) dans le gestionnaire d’événements pour obtenir une instance de **CoreWindowResizeManager** et, une fois la disposition de la nouvelle orientation terminée et présentée, appelez la méthode [**NotifyLayoutCompleted**](https://msdn.microsoft.com/library/windows/apps/jj215605) pour signaler à Windows qu’il peut terminer l’animation de rotation et afficher l’écran d’application.

Voici ce à quoi pourrait ressembler le code dans votre gestionnaire d’événements pour [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) :

```cpp
CoreWindowResizeManager^ resizeManager = Windows::UI::Core::CoreWindowResizeManager::GetForCurrentView();

// ... build the layout for the new display orientation ...

resizeManager->NotifyLayoutCompleted();
```

Lorsqu’un utilisateur fait pivoter l’orientation de l’affichage, Windows 10 affiche une animation indépendante de votre application en tant que commentaires à l’utilisateur. Cette animation se décompose en trois parties, dans l’ordre suivant :

-   Windows 10 réduit l’image d’origine.
-   Windows 10 maintient l’image pendant le temps que nécessaire pour reconstruire la nouvelle disposition. Il s’agit du délai que vous souhaiterez probablement réduire, car votre application n’a pas besoin de tout ce temps.
-   Lorsque la fenêtre de disposition expire, ou quand une notification d’achèvement de disposition est reçue, Windows fait pivoter l’image puis effectue des zooms en fondu enchaîné vers la nouvelle orientation.

Comme indiqué dans la troisième puce, lorsqu’une application appelle [**NotifyLayoutCompleted**](https://msdn.microsoft.com/library/windows/apps/jj215605), Windows 10 arrête la fenêtre de délai d’expiration, se termine l’animation de rotation et redonne le contrôle à votre application, qui dessine maintenant dans la nouvelle orientation d’affichage. L’effet global est que votre application semble maintenant un peu plus fluide et réceptive et qu’elle fonctionne un peu plus efficacement !

## <a name="appendix-a-applying-matrices-for-screen-rotation-2-d"></a>Annexe A : application de matrices pour la rotation de l’écran (2D)


Dans l’exemple de code fourni dans [Redimensionnement de la chaîne d’échange et prérotation de son contenu](#resizing-the-swap-chain-and-pre-rotating-its-contents) (et dans l'[exemple de rotation de chaîne d’échange DXGI](http://go.microsoft.com/fwlink/p/?linkid=257600)), vous aurez peut-être remarqué que nous avions des matrices de rotation distinctes pour la sortie Direct2D et la sortie Direct3D. Examinons tout d’abord les matrices 2D.

Il existe deux raisons pour lesquelles nous ne pouvons pas appliquer les mêmes matrices de rotation au contenu Direct2D et Direct3D :

-   Premièrement, ils utilisent différents modèles de coordonnées cartésiennes. Direct2D utilise la règle de priorité à droite, selon laquelle la coordonnée « y » augmente en valeur positive lorsqu’on se déplace vers le haut par rapport à l’origine. En revanche, Direct3D utilise la règle de priorité à gauche, selon laquelle la coordonnée « y » augmente en valeur positive lorsqu’on se déplace vers la droite par rapport à l’origine. Le résultat est que l’origine des coordonnées d’écran se trouve en haut à gauche pour Direct2D, tandis que l’origine de l’écran (le plan de projection) se trouve en bas à gauche pour Direct3D. (Pour plus d’informations, voir [Systèmes de coordonnées 3D](https://msdn.microsoft.com/library/windows/apps/bb324490.aspx).)

    ![Système de coordonnées Direct3D.](images/direct3d-origin.png)![Système de coordonnées Direct2D.](images/direct2d-origin.png)

-   Deuxièmement, les matrices de rotation 3D doivent être spécifiées de manière explicite afin d’éviter toute erreur d’arrondi.

La chaîne d’échange suppose que l’origine se trouve en bas à gauche. Vous devez par conséquent effectuer une rotation pour aligner le système de coordonnées Direct2D à priorité à droite avec le système à priorité à gauche utilisé par la chaîne d’échange. Plus spécifiquement, vous devez repositionner l’image sous la nouvelle orientation à priorité à gauche en multipliant la matrice de rotation par une matrice de traduction pour l’origine du système de coordonnées qui a pivoté, puis transformer l’image de l’espace de coordonnées de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) vers l’espace de coordonnées de la chaîne d’échange. Votre application doit également appliquer de façon cohérente cette transformation lorsque la cible de rendu Direct2D est connectée à la chaîne d’échange. Toutefois, si votre application dessine sur des surfaces intermédiaires qui ne sont pas associées directement à la chaîne d’échange, n’appliquez pas cette transformation d’espace de coordonnées.

Votre code de sélection de la matrice correcte parmi les quatre rotations possibles pourrait ressembler à ceci (notez la transformation vers la nouvelle origine de système de coordonnées) :

```cpp
   
// Set the proper orientation for the swap chain, and generate 2D and
// 3D matrix transformations for rendering to the rotated swap chain.
// Note the rotation angle for the 2D and 3D transforms are different.
// This is due to the difference in coordinate spaces.  Additionally,
// the 3D matrix is specified explicitly to avoid rounding errors.

switch (displayRotation)
{
case DXGI_MODE_ROTATION_IDENTITY:
    m_orientationTransform2D = Matrix3x2F::Identity();
    m_orientationTransform3D = ScreenRotation::Rotation0;
    break;

case DXGI_MODE_ROTATION_ROTATE90:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(90.0f) *
        Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
    m_orientationTransform3D = ScreenRotation::Rotation270;
    break;

case DXGI_MODE_ROTATION_ROTATE180:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(180.0f) *
        Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
    m_orientationTransform3D = ScreenRotation::Rotation180;
    break;

case DXGI_MODE_ROTATION_ROTATE270:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(270.0f) *
        Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
    m_orientationTransform3D = ScreenRotation::Rotation90;
    break;

default:
    throw ref new FailureException();
}
    
```

Une fois que vous avez la matrice de rotation et l’origine correctes pour l’image 2D, définissez-la avec un appel à [**ID2D1DeviceContext::SetTransform**](https://msdn.microsoft.com/library/windows/desktop/dd742857) entre vos appels à [**ID2D1DeviceContext::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/dd371768) et [**ID2D1DeviceContext::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/dd371924).

**Avertissement**  Direct2D n’a pas une pile de transformations. Si votre application utilise également [**ID2D1DeviceContext::SetTransform**](https://msdn.microsoft.com/library/windows/desktop/dd742857) dans le cadre de son code de dessin, cette matrice doit être post-multipliée en toute autre transformation que vous avez appliquée.

 

```cpp
    ID2D1DeviceContext* context = m_deviceResources->GetD2DDeviceContext();
    Windows::Foundation::Size logicalSize = m_deviceResources->GetLogicalSize();

    context->SaveDrawingState(m_stateBlock.Get());
    context->BeginDraw();

    // Position on the bottom right corner.
    D2D1::Matrix3x2F screenTranslation = D2D1::Matrix3x2F::Translation(
        logicalSize.Width - m_textMetrics.layoutWidth,
        logicalSize.Height - m_textMetrics.height
        );

    context->SetTransform(screenTranslation * m_deviceResources->GetOrientationTransform2D());

    DX::ThrowIfFailed(
        m_textFormat->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_TRAILING)
        );

    context->DrawTextLayout(
        D2D1::Point2F(0.f, 0.f),
        m_textLayout.Get(),
        m_whiteBrush.Get()
        );

    // Ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = context->EndDraw();
```

La prochaine fois que vous présenterez la chaîne d’échange, votre image 2D subira une rotation afin de correspondre à la nouvelle orientation de l’affichage.

## <a name="appendix-b-applying-matrices-for-screen-rotation-3-d"></a>Annexe B : application de matrices pour la rotation de l’écran (3D)


Dans l’exemple de code fourni dans [Redimensionnement de la chaîne d’échange et prérotation de son contenu](#resizing-the-swap-chain-and-pre-rotating-its-contents) (et dans l’[exemple de rotation de chaîne d’échange DXGI](http://go.microsoft.com/fwlink/p/?linkid=257600)), nous avons défini une matrice de transformation spécifique pour chaque orientation d’écran possible. Examinons maintenant les matrices pour la rotation de scènes en 3D. Comme précédemment, vous créez un ensemble de matrices pour chacune des quatre orientations possibles. Pour éviter toute erreur d’arrondi et par conséquent les artefacts visuels mineurs, déclarez les matrices de manière explicite dans votre code.

La configuration de ces matrices de rotation 3D s’effectue comme suit. Les matrices illustrées dans l’exemple de code suivant sont des matrices de rotation standard pour des rotations de 0, 90, 180 et 270degrés des vertex qui définissent des points dans l’espace de scène 3D de la caméra. La valeur de coordonnées [x, y, z] de chaque vertex dans la scène est multipliée par cette matrice de rotation lorsque la projection 2D de la scène est calculée.

```cpp
   
// 0-degree Z-rotation
static const XMFLOAT4X4 Rotation0( 
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 90-degree Z-rotation
static const XMFLOAT4X4 Rotation90(
    0.0f, 1.0f, 0.0f, 0.0f,
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 180-degree Z-rotation
static const XMFLOAT4X4 Rotation180(
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, -1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 270-degree Z-rotation
static const XMFLOAT4X4 Rotation270( 
    0.0f, -1.0f, 0.0f, 0.0f,
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );            
    }
```

Vous pouvez définir le type de rotation sur la chaîne d’échange à l’aide d’un appel à [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801), comme ceci :

`   m_swapChain->SetRotation(rotation);`

Maintenant, dans votre méthode de rendu, mettez en œuvre du code semblable au suivant:

``` syntax
struct ConstantBuffer // This struct is provided for illustration.
{
    // Other constant buffer matrices and data are defined here.

    float4x4 projection; // Current matrix for projection
} ;
ConstantBuffer  m_constantBufferData;          // Constant buffer resource data

// ...

// Rotate the projection matrix as it will be used to render to the rotated swap chain.
m_constantBufferData.projection = mul(m_constantBufferData.projection, m_rotationTransform3D);
```

Désormais, lorsque vous appelez votre méthode de rendu, elle multiplie la matrice de rotation actuelle (telle que spécifiée par la variable de classe **m\_orientationTransform3D**) par la matrice de projection actuelle et assigne le résultat de cette opération comme nouvelle matrice de projection pour votre convertisseur. Présentez la chaîne d’échange pour voir la scène dans l’orientation d’affichage mise à jour.

 

 




