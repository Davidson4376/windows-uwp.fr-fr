---
title: Technologie interop DirectX et XAML
description: Vous pouvez utiliser XAML (Extensible Application Markup Language) et Microsoft DirectX conjointement dans votre jeu de plateforme Windows universelle (UWP).
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
---

# Technologie interop DirectX et XAML


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Vous pouvez utiliser XAML (Extensible Application Markup Language) et Microsoft DirectX conjointement dans votre jeu de plateforme Windows universelle (UWP). En combinant XAML et DirectX, vous pouvez générer des infrastructures d’interface utilisateur flexibles qui interopèrent avec votre contenu DirectX, ce qui s’avère particulièrement utile dans le cas d’applications qui utilisent beaucoup de ressources graphiques. Cette rubrique décrit la structure d’une application UWP qui utilise DirectX et identifie les types qu’il est important d’utiliser au moment de générer votre application UWP pour la faire fonctionner avec DirectX.

> **Remarque** Les API DirectX n’étant pas définies en tant que types Windows Runtime, il est d’usage d’utiliser des extensions de composant Visual C++ (C++/CX) pour développer des composants XAMLUWP qui interopèrent avec DirectX. En outre, vous pouvez créer une application UWP en XAML et C# qui utilise DirectX, à condition d’inclure les appels DirectX dans un wrapper au sein d’un fichier de métadonnées Windows Runtime distinct.

 

## XAML et DirectX


DirectX fournit deux bibliothèques complètes pour les graphismes 2D et 3D : Direct2D et Microsoft Direct3D. Bien que XAML assure une prise en charge des primitives et des effets 2D de base, nombreuses sont les applications, notamment de modélisation et de jeux, qui nécessitent une prise en charge de graphismes plus complexes. Pour celles-ci, vous pouvez utiliser Direct2D et Direct3D pour restituer tout ou partie des graphismes et utiliser XAML pour tout le reste.

Dans le scénario d’interopérabilité de XAML et DirectX, vous devez connaître les deux concepts suivants :

-   Les surfaces partagées sont des régions dimensionnées de l’affichage, définies par XAML, dans lesquelles vous pouvez dessiner indirectement à l’aide de DirectX en utilisant les types [**Windows::UI::Xaml::Media::Brush**](https://msdn.microsoft.com/library/windows/apps/br228076). Pour les surfaces partagées, vous ne contrôlez pas les appels à présentation des chaînes de permutation. Les mises à jour appliquées à une surface partagée sont synchronisées avec les mises à jour de l’infrastructure XAML.
-   La chaîne de permutation proprement dite. Celle-ci fournit la mémoire tampon d’arrière-plan du pipeline de rendu DirectX, la zone de mémoire présentée à l’affichage, une fois la cible de rendu complète.

Déterminez ce pour quoi vous utilisez DirectX. Voulez-vous l’utiliser pour composer ou animer un contrôle unique qui doit tenir dans les dimensions de la fenêtre d’affichage ? La surface composite créée peut-elle être masquée par d’autres surfaces ou par les bords de l’écran ? Contiendra-t-elle la sortie qui doit être rendue et contrôlée en temps réel, comme dans un jeu ?

Une fois que vous avez déterminé comment vous envisagez d’utiliser DirectX, utilisez l’un de ces types Windows Runtime pour incorporer le rendu DirectX dans votre application du Windows Store :

-   Si vous voulez composer une image statique ou dessiner une image complexe selon des intervalles basés sur des événements, dessinez sur une surface partagée avec [**Windows::UI::Xaml::Media::Imaging::SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041). Ce type gère une surface de dessin DirectX dimensionnée. En général, ce type est utilisé pour composer une image ou une texture sous forme d’image bitmap à afficher dans un document ou un élément d’interface. En revanche, il s’avère peu efficace dans un contexte d’interactivité en temps réel, comme les jeux élaborés. Cela s’explique par le fait que les mises à jour appliquées à un objet **SurfaceImageSource** sont synchronisées avec les mises à jour de l’interface utilisateur XAML, ce qui peut introduire un temps de latence dans la rétroaction visuelle à l’utilisateur avec, par exemple, une fréquence d’images fluctuante ou un sentiment de faible réactivité aux entrées en temps réel. Les mises à jour restent néanmoins suffisamment rapides pour les contrôles dynamiques ou les simulations de données.

    Les objets graphiques [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) peuvent être composés avec d’autres éléments d’interface XAML. Vous pouvez les transformer ou les projeter et l’infrastructure XAML respecte les valeurs d’opacité ou d’index-z.

-   Si l’image est plus grande que l’espace fourni par l’écran et si elle peut être recentrée ou zoomée par l’utilisateur, utilisez [**Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050). Ce type gère une surface de dessin DirectX dimensionnée de format supérieur à l’écran. Tout comme [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041), il s’avère utile pour composer dynamiquement une image ou un contrôle complexe. De même, à la manière de **SurfaceImageSource**, il se révèle peu efficace pour les jeux élaborés. Parmi les éléments XAML susceptibles d’utiliser un **VirtualSurfaceImageSource**, citons les contrôles de carte ou une visionneuse de documents volumineux et riches en images.

-   Si vous utilisez DirectX pour présenter des graphismes mis à jour en temps réel ou dans un contexte où les mises à jour doivent se produire à intervalles réguliers et avec peu de latence, utilisez la classe [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834). Les graphismes seront ainsi actualisés sans être synchronisés avec le minuteur d’actualisation de l’infrastructure XAML. Ce type vous permet d’accéder directement à la chaîne de permutation du périphérique graphique ([**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)) et de disposer la couche XAML sur la cible de rendu. Ce type est tout indiqué pour les jeux et autres applications DirectX plein écran qui nécessitent une interface utilisateur XAML. Pour utiliser cette approche, vous devez bien connaître DirectX, notamment les technologies Microsoft DXGI (DirectX Graphics Infrastructure), Direct2D et Direct3D. Pour plus d’informations, voir [Guide de programmation pour Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345).

## SurfaceImageSource


[
            **SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) propose des surfaces partagées DirectX dans lesquelles il est possible de dessiner et compose les éléments de contenu de l’application.

Voici le processus de base de création et de mise à jour d’un objet [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) dans le code-behind :

1.  Définissez la taille de la surface partagée en passant la hauteur et la largeur au constructeur [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041). Vous pouvez également indiquer si la surface a besoin d’une prise en charge alpha (opacité).

    Par exemple :

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Obtenez un pointeur vers [**ISurfaceImageSourceNative**](https://msdn.microsoft.com/library/windows/desktop/hh848322). Effectuez un cast de l’objet [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) en tant qu’[**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) (ou **IUnknown**), puis appelez **QueryInterface** dessus pour obtenir l’implémentation d’**ISurfaceImageSourceNative** sous-jacente. Les méthodes définies dans cette implémentation permettent de définir l’appareil et d’exécuter les opérations de dessin.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNative> m_sisNative;
    // ...
    IInspectable* sisInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);
    sisInspectable->QueryInterface(__uuidof(ISurfaceImageSourceNative), (void **)&m_sisNative);
    ```

3.  Définissez le périphérique DXGI en appelant d’abord [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082), puis en passant le périphérique et le contexte à [**ISurfaceImageSourceNative::SetDevice**](https://msdn.microsoft.com/library/windows/desktop/hh848325). Par exemple :

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device>              m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext>           m_d3dContext;
    // ...
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext
            )
        );  
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    // ...
    m_sisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Fournissez un pointeur vers l’objet [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) à [**ISurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) et dessinez dans cette surface à l’aide de DirectX. Seule est dessinée la zone désignée pour la mise à jour dans le paramètre *updateRect*.

    > **Remarque** Vous pouvez avoir une seule opération [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) en attente active à la fois par [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527).

     

    This method returns the point (x,y) offset of the updated target rectangle in the *offset* parameter. You use this offset to determine where to draw into inside the [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565).

    ```cpp
    ComPtr<IDXGISurface> surface;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(updateRect, &surface, &offset);
    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED || beginDrawHR == DXGI_ERROR_DEVICE_RESET)
    {
              // Device changed
    }
    else
    {
        // Draw to IDXGISurface (the surface paramater)
    }
    ```

5.  Appelez [**ISurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324) pour compléter l’image bitmap. Passez cette image bitmap à un [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101).

    ```cpp
    m_sisNative->EndDraw();
    // ...
    // The SurfaceImageSource object's underlying ISurfaceImageSourceNative object contains the completed bitmap.
    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

6.  Utilisez [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) pour dessiner l’image bitmap.

> **Remarque** L’appel de [**SurfaceImageSource::SetSource**](https://msdn.microsoft.com/library/windows/apps/br243255) (hérité de **IBitmapSource::SetSource**) lève actuellement une exception. Ne l’appelez pas à partir de votre objet [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041).

 

## VirtualSurfaceImageSource


[
            **VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) étend [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) lorsque le contenu est potentiellement plus volumineux pour l’écran et donc que le contenu doit être virtualisé pour un rendu optimal.

[
            **VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) est différent de [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) dans le sens où il utilise un rappel, [**IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337), que vous implémentez pour mettre à jour des régions de la surface à mesure qu’elles s’affichent à l’écran. Vous n’avez pas besoin d’effacer les régions masquées, car l’infrastructure XAML s’en charge à votre place.

Voici le processus de base de création et de mise à jour d’un objet [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) dans le code-behind :

1.  Créez une instance de [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) avec la taille que vous voulez. Par exemple :

    `VirtualSurfaceImageSource^ virtualSIS = ref new VirtualSurfaceImageSource(2000, 2000);`

2.  Obtenez un pointeur vers [**IVirtualSurfaceImageSourceNative**](https://msdn.microsoft.com/library/windows/desktop/hh848328). Effectuez un cast de l’objet [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) en tant qu’[**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) ou [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509), puis appelez [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) dessus pour obtenir l’implémentation d’**IVirtualSurfaceImageSourceNative** sous-jacente. Les méthodes définies dans cette implémentation permettent de définir l’appareil et d’exécuter les opérations de dessin.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    // ...
    IInspectable* vsisInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);
    vsisInspectable->QueryInterface(__uuidof(IVirtualSurfaceImageSourceNative), (void **)&m_vsisNative);
    ```

3.  Définissez le périphérique DXGI en appelant [**IVirtualSurfaceImageSourceNative::SetDevice**](https://msdn.microsoft.com/library/windows/desktop/hh848325). Par exemple :

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device>              m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext>           m_d3dContext;
    // ...
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext
            )
        );  
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    // ...
    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Appelez [**IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848334) en passant une référence à votre implémentation de [**IVirtualSurfaceUpdatesCallbackNative**](https://msdn.microsoft.com/library/windows/desktop/hh848336).

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }
    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    L’infrastructure appelle votre implémentation de [**IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848334) lorsqu’une région de [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) doit être mise à jour.

    Cela se produit lorsque l’infrastructure détermine que la région doit être dessinée (par exemple, lorsque l’utilisateur recentre ou zoome la vue de la surface) ou après que l’application a appelé [**IVirtualSurfaceImageSourceNative::Invalidate**](https://msdn.microsoft.com/library/windows/desktop/hh848332) sur cette région.

5.  Dans [**IVirtualSurfaceImageSourceNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337), utilisez les méthodes [**IVirtualSurfaceImageSourceNative::GetUpdateRectCount**](https://msdn.microsoft.com/library/windows/desktop/hh848329) et [**IVirtualSurfaceImageSourceNative::GetUpdateRects**](https://msdn.microsoft.com/library/windows/desktop/hh848330) pour déterminer la ou les régions de la surface à dessiner.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  

                  m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);
            std::unique_ptr<RECT[]> drawingBounds(new RECT[drawingBoundsCount]);
            m_vsisNative->GetUpdateRects(drawingBounds.get(), drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  Enfin, pour chaque région à mettre à jour :

    1.  Fournissez un pointeur vers l’objet [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) à [**IVirtualSurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) et dessinez dans cette surface à l’aide de DirectX. Seule sera dessinée la zone désignée pour la mise à jour dans le paramètre *updateRect*.

        Comme dans le cas de la méthode [**IlSurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323), cette méthode retourne le décalage de points (x,y) du rectangle cible mis à jour dans le paramètre *offset*. Ce décalage permet de déterminer où dessiner à l’intérieur de l’objet [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565).

        > **Remarque** Vous pouvez avoir une seule opération [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) en attente active à la fois par [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527).

         

        ```cpp
        ComPtr<IDXGISurface> bigSurface;

        HRESULT beginDrawHR = m_vsisNative->BeginDraw(updateRect, &bigSurface, &offset);
        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED || beginDrawHR == DXGI_ERROR_DEVICE_RESET)
        {
                  // Device changed
        }
        else
        {
            // Draw to IDXGISurface
        }
        ```

    2.  Draw the specific content to that region, but constrain your drawing to the bounded regions for better performance.

    3.  Call [**IVirtualSurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324). The result is a bitmap.

## SwapChainPanel et jeux


[
            **SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) est le type Windows Runtime conçu pour prendre en charge les graphismes et les jeux élaborés, où vous gérez directement la chaîne de permutation. Dans ce cas, vous créez votre propre chaîne de permutation DirectX et gérez la présentation de votre contenu rendu. Vous pouvez ensuite ajouter des éléments XAML à l’objet **SwapChainPanel**, tels que des menus, des affichages tête haute et autres superpositions d’interface.

Pour garantir les performances, le type [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) impose certaines limitations :

-   Le nombre maximal d’instances de [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) par application est de 4.
-   Les propriétés **Opacity**, **RenderTransform**, **Projection** et **Clip** héritées par [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) ne sont pas prises en charge.
-   Vous devez attribuer à la chaîne de permutation DirectX une hauteur et une largeur (dans [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) correspondant aux dimensions actuelles de la fenêtre d’application. À défaut, le contenu à afficher est mis à l’échelle (à l’aide de **DXGI\_SCALING\_STRETCH**).
-   Vous devez affecter au mode d’ajustement de la chaîne de permutation DirectX (dans [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) la valeur **DXGI\_SCALING\_STRETCH**.
-   Vous ne pouvez pas affecter au mode alpha de la chaîne de permutation DirectX (dans [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) la valeur **DXGI\_ALPHA\_MODE\_PREMULTIPLIED**.
-   Vous devez créer la chaîne de permutation DirectX en appelant [**IDXGIFactory2::CreateSwapChainForComposition**](https://msdn.microsoft.com/library/windows/desktop/hh404558).

Vous mettez à jour [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) en fonction des besoins de votre application et non des mises à jour de l’infrastructure XAML. Si vous avez besoin de synchroniser les mises à jour de **SwapChainPanel** avec celles de l’infrastructure XAML, inscrivez-vous à l’événement [**Windows::UI::Xaml::Media::CompositionTarget::Rendering**](https://msdn.microsoft.com/library/windows/apps/br228127). Sinon, vous devez prévoir des problèmes inter-threads si vous essayez de mettre à jour les éléments XAML à partir d’un thread différent de celui qui met à jour **SwapChainPanel**.

Il existe par ailleurs des recommandations générales à suivre au moment de concevoir votre application pour qu’elle utilise [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834).

-   [
            **SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) hérite de [**Windows::UI::Xaml::Controls::Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) et prend en charge un comportement de mise en page similaire. Familiarisez-vous avec le type **Grid** et ses propriétés.

-   Une fois qu’une chaîne de permutation DirectX a été définie, tous les événements d’entrée déclenchés pour [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) opèrent de la même manière que pour tout autre élément XAML. Vous ne définissez pas de pinceau d’arrière-plan pour **SwapChainPanel** et vous n’avez pas besoin de gérer directement les événements d’entrée de l’objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de l’application comme c’est le cas dans les applications DirectX qui n’utilisent pas **SwapChainPanel**.

-   • L’ensemble du contenu de l’arborescence visuelle d’éléments XAML située sous un enfant direct d’un [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) est découpé à la taille de disposition de l’enfant immédiat de l’objet **SwapChainPanel**. Tout contenu transformé en dehors de ces limites de disposition n’est pas restitué. Par conséquent, placez le contenu XAML que vous animez à l’aide d’un élément [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) XAML dans l’arborescence visuelle située sous un élément dont les limites de disposition sont suffisamment étendues pour accueillir l’animation dans son ensemble.

-   Limitez le nombre d’éléments XAML visuels immédiats situés sous un [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834). Si possible, regroupez les éléments qui se trouvent très près les uns des autres sous un parent commun. Mais vous devez trouver un compromis entre le nombre d’enfants visuels immédiats et la taille des enfants. En effet, les performances globales peuvent pâtir de la présence d’éléments XAML trop nombreux ou inutilement grands. De même, ne créez pas d’élément XAML enfant unique en plein écran pour le **SwapChainPanel** de votre application, car cela accroît l’« overdraw » dans l’application et nuit aux performances. En règle générale, ne créez pas plus de 8 enfants visuels XAML immédiats pour le **SwapChainPanel** de votre application et attribuez à chaque élément une taille de disposition juste nécessaire pour accueillir le contenu visuel de l’élément. Toutefois, l’arborescence visuelle d’éléments située sous un élément enfant du **SwapChainPanel** peut être suffisamment complexe sans pour autant grever lourdement les performances.

> **Remarque** En règle générale, vos applications DirectX doivent créer des chaînes de permutation dans l’orientation paysage, égales à la taille de la fenêtre d’affichage (qui correspond habituellement à la résolution d’écran native dans la plupart des jeux Windows Store). Cela garantit que votre application utilise l’implémentation de chaîne de permutation optimale lorsqu’elle ne possède aucune superposition XAML visible. Si l’application est pivotée en mode portrait, elle doit appeler [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) sur la chaîne de permutation existante, appliquer une transformation au contenu, si nécessaire, puis appeler de nouveau [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144) sur la même chaîne de permutation. De même, votre application doit appeler de nouveau **SetSwapChain** sur la même chaîne de permutation chaque fois que celle-ci est redimensionnée à la suite d’un appel de [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577).

 

Voici le processus de base de création et de mise à jour d’un objet [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) dans le code-behind :

1.  Obtenez une instance d’un panneau de chaîne de permutation pour votre application. Les instances sont indiquées dans votre code XAML par la balise `<SwapChainPanel>`.

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    Voici un exemple de balise `<SwapChainPanel>`.

    ```xaml
    <SwapChainPanel x:Name=&quot;swapChainPanel&quot;>
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width=&quot;300*&quot;/>
            <ColumnDefinition Width=&quot;1069*&quot;/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  Obtenez un pointeur vers [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143). Effectuez un cast de l’objet [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) en tant qu’[**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) (ou **IUnknown**), puis appelez **QueryInterface** dessus pour obtenir l’implémentation d’**ISwapChainPanelNative** sous-jacente.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Créez le périphérique DXGI et la chaîne de permutation et attribuez à cette dernière la valeur [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143) en la passant à [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144).

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  Dessinez sur la chaîne de permutation DirectX et présentez-la pour afficher le contenu.

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    Les éléments XAML sont actualisés lorsque la logique de disposition/rendu de Windows Runtime signale une mise à jour.

## Rubriques connexes


* [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)
* [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Guide de programmation pour Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 




<!--HONumber=Mar16_HO1-->
