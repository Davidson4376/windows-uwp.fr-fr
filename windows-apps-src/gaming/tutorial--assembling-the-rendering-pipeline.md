---
author: mtoepke
title: Assembler l’infrastructure de rendu
description: Il est maintenant temps d’examiner la façon dont l’exemple de jeu utilise cette structure et ces états pour afficher ses graphismes.
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
---

# Assembler l’infrastructure de rendu


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Jusqu’à présent, vous avez vu comment structurer un jeu de plateforme Windows universelle (UWP) à utiliser avec Windows Runtime, et comment définir une machine à états pour gérer le flux du jeu. Il est maintenant temps d’examiner comment l’exemple de jeu utilise cette structure et ces états pour afficher ses graphiques. Dans cette rubrique, nous allons examiner comment implémenter une infrastructure de rendu, en partant de l’initialisation du périphérique graphique jusqu’à la présentation des objets graphiques à afficher.

## Objectif


-   Comprendre comment configurer une infrastructure de rendu de base pour afficher la sortie graphique pour un jeu UWP DirectX.

> **Remarque** Les fichiers de code suivants ne sont pas abordés ici, mais fournissent des classes et méthodes référencées dans cette rubrique, et sont [fournis en tant que code à la fin de cette rubrique](#code_sample) :
-   **Animate.h/.cpp**.
-   **BasicLoader.h/.cpp**. Fournit des méthodes pour le chargement des maillages, nuanceurs et textures, à la fois de façon synchrone et asynchrone. Très utile !
-   **MeshObject.h/.cpp**, **SphereMesh.h/.cpp**, **CylinderMesh.h/.cpp**, **FaceMesh.h/.cpp** et **WorldMesh.h/.cpp**. Contient les définitions des primitives d’objets utilisées dans le jeu, par exemple les sphères de munitions, les obstacles sous forme de cylindres et de cônes, ainsi que les murs du pas de tir. (**GameObject.cpp**, brièvement abordé dans cette rubrique, contient la méthode de rendu de ces primitives.)
-   **Level.h/.cpp** et **Level\[1-6\].h/.cpp**. Contient la configuration de chacun des six niveaux de jeu, notamment les critères de réussite ainsi que le nombre et la position des cibles et obstacles.
-   **TargetTexture.h/.cpp**. Contient un ensemble de méthodes pour le dessin des bitmaps utilisées comme textures sur les cibles.

Ces fichiers contiennent un code qui n’est pas spécifique aux jeux UWP DirectX. Cependant, vous pouvez les passer en revue séparément si vous voulez d’autres détails sur l’implémentation.

 

Cette section couvre trois fichiers clés de l’exemple de jeu ([fournis en tant que code à la fin de cette rubrique](#code_sample)) :

-   **Camera.h/.cpp**
-   **GameRenderer.h/.cpp**
-   **PrimObject.h/.cpp**

Encore une fois, nous partons du principe que vous comprenez les concepts de programmation 3D de base, tels que les maillages, vertex et textures. Pour plus d’informations sur la programmation Direct3D 11 en général, voir [Guide de programmation pour Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345).
Ceci étant dit, examinons le travail qui doit être effectué pour mettre votre jeu à l’écran.

## Vue d’ensemble de Windows Runtime et DirectX


DirectX est une partie essentielle de Windows Runtime et de l’expérience Windows 10. Tous les effets visuels de Windows 10 sont basés sur DirectX, et vous disposez de la même ligne directe à la même interface graphique de bas niveau, [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534), qui fournit une couche d’abstraction pour le matériel vidéo et ses pilotes. Toutes les API Direct3D 11 sont disponibles pour que vous puissiez communiquer directement avec DXGI. Vous obtenez ainsi des graphiques rapides et hautement performants dans vos jeux, qui vous donnent accès à toutes les dernières fonctionnalités de matériel vidéo.

Pour ajouter la prise en charge de DirectX à une application du Windows Store, vous créez un fournisseur de vues pour les ressources DirectX en implémentant les interfaces [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482) et [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478). Celles-ci fournissent un modèle de fabrique pour le type du fournisseur de vues et l’implémentation du fournisseur de vues DirectX, respectivement. Le singleton UWP, représenté par l’objet [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016), exécute cette implémentation.

Dans [Définition de l’infrastructure UWP du jeu](tutorial--building-the-games-metro-style-app-framework.md), nous avons examiné la façon dont le convertisseur s’adapte à l’infrastructure d’application de l’exemple de jeu. Examinons maintenant la façon dont le convertisseur de jeu se connecte à la vue et crée les graphiques qui définissent l’apparence du jeu.

## Définition du convertisseur


Le type abstrait **GameRenderer** hérite du type de convertisseur **DirectXBase**, ajoute la prise en charge du mode 3D stéréo et déclare des ressources et des mémoires tampons constantes pour les nuanceurs qui créent et définissent nos primitives graphiques.

Voici la définition de **GameRenderer**.

```cpp
ref class GameRenderer : public DirectXBase
{
internal:
    GameRenderer();

    virtual void Initialize(
        _In_ Windows::UI::Core::CoreWindow^ window,
        float dpi
        ) override;

    virtual void CreateDeviceIndependentResources() override;
    virtual void CreateDeviceResources() override;
    virtual void UpdateForWindowSizeChange() override;
    virtual void Render() override;
    virtual void SetDpi(float dpi) override;

    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    GameInfoOverlay^ InfoOverlay()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f
            );
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f + GameInfoOverlayConstant::Width,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f + GameInfoOverlayConstant::Height
            );
    };

protected private:
    bool                                                m_initialized;
    bool                                                m_gameResourcesLoaded;
    bool                                                m_levelResourcesLoaded;
    GameInfoOverlay^                                    m_gameInfoOverlay;
    GameHud^                                            m_gameHud;
    Simple3DGame^                                       m_game;

    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_sphereTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_cylinderTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_ceilingTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_floorTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant Buffers
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferNeverChanges;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

Dans la mesure où les API Direct3D 11 sont définies comme des API COM, vous devez fournir des références [**ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983) aux objets définis par ces API. Ces objets sont automatiquement libérés lorsque leur dernière référence sort du contexte une fois l’exécution de l’application terminée.

L’exemple de jeu déclare 4 mémoires tampons constantes spécifiques :

-   **m\_constantBufferNeverChanges**. Cette mémoire tampon constante contient les paramètres d’éclairage. Elle est définie une fois et n’est plus jamais modifiée.
-   **m\_constantBufferChangeOnResize**. Cette mémoire tampon constante contient la matrice de projection. La matrice de projection dépend de la taille et des proportions de la fenêtre. Elle est mise à jour uniquement lorsque la taille de la fenêtre change.
-   **m\_constantBufferChangesEveryFrame**. Cette mémoire tampon constante contient la matrice globale. Cette matrice dépend de la position de la caméra et de la direction de la vue (perpendiculaire à la projection) et ne change qu’une fois par trame.
-   **m\_constantBufferChangesEveryPrim**. Cette mémoire tampon constante contient la matrice de modèle et les propriétés matérielles de chaque primitive. La matrice de modèle transforme les vertex des coordonnées locales en coordonnées universelles. Ces constantes sont spécifiques à chaque primitive et sont mises à jour pour chaque appel de dessin.

L’intérêt de plusieurs tampons constants avec des fréquences différentes est de réduire la quantité de données à envoyer à l’unité de traitement graphique (GPU) par trame. Par conséquent, l’exemple répartit les constantes en plusieurs tampons en fonction de la fréquence à laquelle elles doivent être mises à jour. Il s’agit d’une recommandation pour la programmation Direct3D.

Le convertisseur contient les objets de nuanceurs qui calculent nos primitives et textures : **m\_vertexShader** et **m\_pixelShader**. Le nuanceur de vertex traite les primitives et l’éclairage de base, tandis que le nuanceur de pixels (parfois appelé nuanceur de fragments) traite les textures et tous les effets par pixel. Il existe deux versions de ces nuanceurs (régulier et plat) pour rendre des primitives différentes. Les versions plates sont beaucoup plus simples et n’engendrent pas de mises en surbrillance spéculaires ni d’effets d’éclairage par pixel. Ces nuanceurs sont utilisés pour les murs et accélèrent le rendu sur des périphériques de faible puissance.

La classe du convertisseur contient les ressources [DirectWrite et Direct2D](https://msdn.microsoft.com/library/windows/desktop/ff729481) utilisées pour la superposition et l’affichage à tête haute (l’objet **GameHud**). La superposition et l’affichage à tête haute sont tracés au-dessus de la cible de rendu lorsque la projection est terminée dans le pipeline graphique.

Le convertisseur définit également les objets de ressources du nuanceur qui contiennent les textures des primitives. Certaines de ces textures sont prédéfinies (textures DDS pour les murs et le sol du monde, ainsi que les sphères de munitions).

Il est temps de voir comment cet objet est créé !

## Initialisation du convertisseur


L’exemple de jeu appelle cette méthode **Initialize** dans le cadre de la séquence d’initialisation de CoreApplication dans **App::SetWindow**.

```cpp
void GameRenderer::Initialize(
    _In_ CoreWindow^ window,
    float dpi
    )
{
    if (!m_initialized)
    {
        m_gameHud = ref new GameHud(
            "Windows 8 Samples",
            "DirectX first-person game sample"
            );
        m_gameInfoOverlay = ref new GameInfoOverlay();
        m_initialized = true;
    }

    DirectXBase::Initialize(window, dpi);

    // Initialize could be called multiple times as a result of an error with the hardware device
    // that requires it to be reinitialized.  Because the m_gameInfoOverlay variable has resources that are
    // dependent on the device, it will need to be reinitialized each time with the new device information.
    m_gameInfoOverlay->Initialize(m_d2dDevice.Get(), m_d2dContext.Get(), m_dwriteFactory.Get(), dpi);
}
```

Il s’agit d’une méthode assez simple. Elle vérifie si le convertisseur a déjà été initialisé et, dans le cas contraire, elle instancie les objets **GameHud** et **GameInfoOverlay**.

Une fois cette opération effectuée, le processus d’initialisation du convertisseur exécute l’implémentation de base de la méthode **Initialize** fournie sur la classe **DirectXBase** dont elle est héritée.

À la fin de l’initialisation de DirectXBase, l’objet **GameInfoOverlay** est initialisé. Une fois l’initialisation terminée, il est temps de s’intéresser aux méthodes de création et de chargement des ressources graphiques pour le jeu.

## Création et chargement des ressources graphiques DirectX


Le premier point de l’ordre du jour de tout jeu est d’établir une connexion à l’interface graphique, de créer les ressources nécessaires pour dessiner les graphiques, puis de configurer une cible de rendu dans laquelle ces graphiques peuvent être dessinés. Dans l’exemple de jeu (et dans le modèle **Application DirectX 11 (Windows universel)** de Microsoft Visual Studio), ce processus est implémenté avec trois méthodes :

-   **CreateDeviceIndependentResources**
-   **CreateDeviceResources**
-   **CreateWindowSizeDependentResources**

À présent, dans l’exemple de jeu, nous remplaçons deux de ces méthodes (**CreateDeviceIndependentResources** et **CreateDeviceResources**) fournies par la classe **DirectXBase** implémentée dans le modèle **Application DirectX 11 (Windows universel)**. Pour chacune de ces méthodes de substitution, nous commençons par appeler les implémentations **DirectXBase** qu’elles remplacent, puis nous ajoutons d’autres détails sur l’implémentation propres à l’exemple de jeu. Gardez à l’esprit que l’implémentation de classe **DirectXBase** incluse dans l’exemple de jeu a été modifiée par rapport à la version fournie dans le modèle Visual Studio afin d’inclure la prise en charge de vue stéréoscopique. Elle comprend aussi la rotation préalable de l’objet **SwapBuffer**.

L’objet **GameRenderer** ne substitue pas **CreateWindowSizeDependentResources**. Nous utilisons son implémentation fournie dans la classe **DirectXBase**.

Pour plus d’informations sur les implémentations de base **DirectXBase** de ces méthodes, voir [Configuration de votre application UWP DirectX pour afficher une vue](https://msdn.microsoft.com/library/windows/apps/hh465077).

La première de ces méthodes de substitution, **CreateDeviceIndependentResources**, appelle la méthode **GameHud::CreateDeviceIndependentResources** pour créer les ressources texte [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) avec la police Segoe UI, qui est utilisée par la plupart des applications UWP.

CreateDeviceIndependentResources

```cpp
void GameRenderer::CreateDeviceIndependentResources()
{
    DirectXBase::CreateDeviceIndependentResources();
    m_gameHud->CreateDeviceIndependentResources(m_dwriteFactory.Get(), m_wicFactory.Get());
}
```

```cpp
void GameHud::CreateDeviceIndependentResources(
    _In_ IDWriteFactory* dwriteFactory,
    _In_ IWICImagingFactory* wicFactory
    )
{
    m_dwriteFactory = dwriteFactory;
    m_wicFactory = wicFactory;

    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudBodyPointSize,
            L"en-us",
            &m_textFormatBody
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Symbol",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudBodyPointSize,
            L"en-us",
            &m_textFormatBodySymbol
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Light",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudTitleHeaderPointSize,
            L"en-us",
            &m_textFormatTitleHeader
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Light",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudTitleBodyPointSize,
            L"en-us",
            &m_textFormatTitleBody
            )
        );

    DX::ThrowIfFailed(m_textFormatBody->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatBody->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatBodySymbol->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatBodySymbol->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatTitleHeader->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatTitleHeader->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatTitleBody->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatTitleBody->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
}
```

L’exemple utilise quatre formateurs de texte : deux pour le titre principal et le texte du corps du titre et deux pour le texte de corps. Cette utilisation est importante dans le texte de superposition.

La deuxième méthode, **CreateDeviceResources**, charge les ressources spécifiques pour le jeu à calculer sur le périphérique graphique. Examinons le code pour cette méthode.

CreateDeviceResources

```cpp
oid GameRenderer::CreateDeviceResources()
{
    DirectXBase::CreateDeviceResources();

    m_gameHud->CreateDeviceResources(m_d2dContext.Get());

    if (m_game != nullptr)
    {
        // The initial invocation of CreateDeviceResources occurs
        // before the Game State is initialized when the device is first
        // being created, so that the inital loading screen can be displayed.
        // Subsequent invocations of CreateDeviceResources will be a result
        // of an error with the device that requires the resources to be
        // recreated.  In this case, the game state is already initialized
        // so the game device resources need to be recreated.

        // This sample doesn't gracefully handle all the async recreation
        // of resources so an exception is thrown.
        throw Platform::Exception::CreateException(
            DXGI_ERROR_DEVICE_REMOVED,
            "GameRenderer::CreateDeviceResources - Recreation of resources after TDR not available\n"
            );
    }
}
```

```cpp
void GameHud::CreateDeviceResources(_In_ ID2D1DeviceContext* d2dContext)
{
    auto location = Package::Current->InstalledLocation;
    Platform::String^ path = Platform::String::Concat(location->Path, "\\");
    path = Platform::String::Concat(path, "windows-sdk.png");

    ComPtr<IWICBitmapDecoder> wicBitmapDecoder;
    DX::ThrowIfFailed(
        m_wicFactory->CreateDecoderFromFilename(
            path->Data(),
            nullptr,
            GENERIC_READ,
            WICDecodeMetadataCacheOnDemand,
            &wicBitmapDecoder
            )
        );

    ComPtr<IWICBitmapFrameDecode> wicBitmapFrame;
    DX::ThrowIfFailed(
        wicBitmapDecoder->GetFrame(0, &wicBitmapFrame)
        );

    ComPtr<IWICFormatConverter> wicFormatConverter;
    DX::ThrowIfFailed(
        m_wicFactory->CreateFormatConverter(&wicFormatConverter)
        );

    DX::ThrowIfFailed(
        wicFormatConverter->Initialize(
            wicBitmapFrame.Get(),
            GUID_WICPixelFormat32bppPBGRA,
            WICBitmapDitherTypeNone,
            nullptr,
            0.0,
            WICBitmapPaletteTypeCustom  // The BGRA format has no palette so this value is ignored.
            )
        );

    double dpiX = 96.0f;
    double dpiY = 96.0f;
    DX::ThrowIfFailed(
        wicFormatConverter->GetResolution(&dpiX, &dpiY)
        );

    // Create D2D Resources.
    DX::ThrowIfFailed(
        d2dContext->CreateBitmapFromWicBitmap(
            wicFormatConverter.Get(),
            BitmapProperties(
                PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
                static_cast<float>(dpiX),
                static_cast<float>(dpiY)
                ),
            &m_logoBitmap
            )
        );

    m_logoSize = m_logoBitmap->GetSize();

    DX::ThrowIfFailed(
        d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White),
            &m_textBrush
            )
        );
}
```

Dans cet exemple et dans des conditions d’exécution normale, la méthode **CreateDeviceResources** appelle simplement la méthode de classe de base, puis appelle la méthode **GameHud::CreateDeviceResources** (également répertoriée précédemment). Dans le cas où un problème se produit par la suite avec le périphérique graphique sous-jacent, ce dernier devra probablement être réinitialisé. Dans ce cas, la méthode **CreateDeviceResources** initialise un ensemble de tâches asynchrones afin de créer les ressources de périphériques de jeu, par le biais d’une séquence de deux méthodes : un appel de la méthode **CreateDeviceResourcesAsync**, puis, à la fin, un appel de la méthode **FinalizeCreateGameDeviceResources**.

CreateGameDeviceResourcesAsync et FinalizeCreateGameDeviceResources

```cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Set the Loading state to wait until any async resources have
    // been loaded before proceeding.
    m_game = game;

    // NOTE: Only the m_d3dDevice is used in this method.  It's expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the m_d3dDevice are free-threaded and are safe while any methods
    // in the m_d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.

    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers.
    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
    bd.CPUAccessFlags = 0;
    bd.ByteWidth = (sizeof(ConstantBufferNeverChanges) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangeOnResize) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangeOnResize)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryFrame) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryFrame)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryPrim) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryPrim)
        );

    D3D11_SAMPLER_DESC sampDesc;
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;
    sampDesc.MinLOD = 0;
    sampDesc.MaxLOD = FLT_MAX;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    m_cylinderTexture = nullptr;
    m_ceilingTexture = nullptr;
    m_floorTexture = nullptr;
    m_wallsTexture = nullptr;

    // Load Game specific Textures.
    tasks.push_back(loader->LoadTextureAsync("seafloor.dds", nullptr, &m_sphereTexture));
    tasks.push_back(loader->LoadTextureAsync("metal_texture.dds", nullptr, &m_cylinderTexture));
    tasks.push_back(loader->LoadTextureAsync("cellceiling.dds", nullptr, &m_ceilingTexture));
    tasks.push_back(loader->LoadTextureAsync("cellfloor.dds", nullptr, &m_floorTexture));
    tasks.push_back(loader->LoadTextureAsync("cellwall.dds", nullptr, &m_wallsTexture));
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Return the task group of all the async tasks for loading the shader and texture assets.
    return when_all(tasks.begin(), tasks.end());
}
```

```cpp
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now, associate all the resources with the appropriate
    // Game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[1] = XMFLOAT4( 3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[2] = XMFLOAT4(-3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[3] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);
    m_d3dContext->UpdateSubresource(m_constantBufferNeverChanges.Get(), 0, nullptr, &constantBufferNeverChanges, 0, 0);

    // For the targets, there are two unique generated textures.
    // Each texture image includes the number of the texture.
    // Make sure the 2-D rendering is occurring on the same thread
    // as the main rendering.

    TargetTexture^ textureGenerator = ref new TargetTexture(
        m_d3dDevice.Get(),
        m_d2dFactory.Get(),
        m_dwriteFactory.Get(),
        m_d2dContext.Get()
        );

    MeshObject^ cylinderMesh = ref new CylinderMesh(m_d3dDevice.Get(), 26);
    MeshObject^ targetMesh = ref new FaceMesh(m_d3dDevice.Get());
    MeshObject^ sphereMesh = ref new SphereMesh(m_d3dDevice.Get(), 26);

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    Material^ sphereMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        50.0f,
        m_sphereTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldFloorMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldCeilingId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_ceilingTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldCeilingMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldWallsId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_wallsTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldWallsMesh(m_d3dDevice.Get()));
        }
        else if (Cylinder^ cylinder = dynamic_cast<Cylinder^>(*object))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (Face^ target = dynamic_cast<Face^>(*object))
        {
            const int bufferLength = 16;
            char16 str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            Platform::String^ string = ref new Platform::String(str, len);

            ComPtr<ID3D11ShaderResourceView> texture;
            textureGenerator->CreateTextureResourceView(string, &texture);
            target->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            textureGenerator->CreateHitTextureResourceView(string, &texture);
            target->HitMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            target->Mesh(targetMesh);
        }
        else if (Sphere^ sphere = dynamic_cast<Sphere^>(*object))
        {
            sphere->Mesh(sphereMesh);
            sphere->NormalMaterial(sphereMaterial);
        }
    }

    // Ensure that the camera has been initialized with the right Projection
    // matrix.  The camera is not created at the time the first window resize event
    // occurs.
    m_game->GameCamera()->SetProjParams(
        XM_PI / 2, m_renderTargetSize.Width / m_renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that Projection matrix has been set in the constant buffer
    // now that all the resources are loaded.
    // DirectXBase is handling screen rotations directly to eliminate an unaligned
    // fullscreen copy.  As a result, it's necessary to post multiply the rotationTransform3D
    // matrix to the camera projection matrix.
    ConstantBufferChangeOnResize changesOnResize;
    XMStoreFloat4x4(
        &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                )
        );

    m_d3dContext->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    m_gameResourcesLoaded = true;
}
```

**CreateDeviceResourcesAsync** est une méthode qui exécute un ensemble de tâches asynchrones afin de charger les ressources de jeu. Elle n’a accès qu’aux méthodes de périphériques Direct3D 11 (celles définies sur [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)) et non aux méthodes de contexte de périphériques (méthodes définies sur [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)), car elle doit être exécutée sur un thread distinct. Elle offre donc la possibilité de n’effectuer aucun rendu. La méthode **FinalizeCreateGameDeviceResources** s’exécute sur le thread principal et a accès aux méthodes de contexte de périphériques Direct3D 11.

La séquence d’événements permettant de charger les ressources de périphériques de jeu se déroule de la manière suivante.

**CreateDeviceResourcesAsync** commence par initialiser les mémoires tampons constantes pour les primitives. Les mémoires tampons constantes sont des mémoires tampons à faible latence et à largeur fixe qui contiennent les données utilisées par un nuanceur pendant son exécution. (Imaginez ces mémoires tampons comme transmettant des données au nuanceur qui sont constantes pendant l’exécution d’un appel de dessin particulier.) Dans cet exemple, les mémoires tampons contiennent les données que les nuanceurs utilisent pour :

-   placer les sources de lumière et définir leur couleur lorsque le convertisseur est initialisé ;
-   calculer la matrice globale chaque fois que la fenêtre est redimensionnée ;
-   calculer la matrice de projection pour chaque mise à jour de trame ;
-   calculer les transformations des primitives à chaque mise à jour de rendu.

Les constantes reçoivent les informations sur la source (vertex) et transforment les données et coordonnées des vertex de l’espace du modèle en espace de périphérique. En fin de compte, ces données produisent des coordonnées texel et des pixels dans la cible de rendu.

Ensuite, l’objet convertisseur de jeu crée un chargeur pour les nuanceurs qui effectueront le calcul. (Voir **BasicLoader.cpp** dans l’exemple pour l’implémentation spécifique.)

**CreateDeviceResourcesAsync** lance ensuite des tâches asynchrones permettant de charger toutes les ressources de texture dans **ShaderResourceViews**. Après cela, les ressources de texture sont stockées dans les textures DDS (DirectDraw Surface) qui accompagnent l’exemple. La texture DDS est un format de texture avec perte qui fonctionne avec DXTC (DirectX Texture Compression). Nous utilisons ces textures pour les murs, le plafond et le sol du monde, ainsi que pour les sphères de munitions et colonnes d’obstacles.

Enfin, un groupe de tâches, contenant toutes les tâches asynchrones créées par la méthode, est renvoyé. La fonction d’appel attend la fin de toutes les tâches asynchrones, puis appelle **FinalizeCreateGameDeviceResources**.

**FinalizeCreateGameDeviceResources** charge les données initiales dans les mémoires tampons constantes avec un appel de méthode de contexte de périphérique à [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) : `m_deviceContext->UpdateSubresource`. Cette méthode crée les objets de maillage pour les sphères, les cylindres, les visages, les jeux du monde et les matières associées. Elle parcourt ensuite la liste d’objets du jeu qui associe les ressources périphériques appropriées à chaque objet.

Les textures des objets cibles avec cercles et numéros sont générées par procédure à l’aide du code dans **TargetTexture.cpp**. Le convertisseur crée une instance du type **TargetTexture**, qui crée la texture de bitmap pour les objets cibles dans le jeu lorsque nous appelons la méthode **TargetTexture::CreateTextureResourceView**. La texture produite est composée de cercles colorés concentriques, avec une valeur numérique sur le dessus. Ces ressources générées sont associées aux objets cibles du jeu appropriés.

Enfin, **FinalizeCreateGameDeviceResources** définit la variable globale booléenne `m_gameResourcesLoaded` afin d’indiquer que toutes les ressources sont désormais chargées.

Le jeu dispose des ressources pour afficher les graphiques dans la fenêtre actuelle, et il peut recréer ces ressources lorsque la fenêtre est modifiée. Examinons maintenant la caméra utilisée pour définir la vue de la scène du joueur dans cette fenêtre.

## Implémentation de l’objet caméra


Le jeu a le code en place pour mettre à jour le monde dans son propre système de coordonnées (parfois appelé espace du monde ou espace de scène). Tous les objets, y compris la caméra, sont positionnés et orientés dans cet espace. Dans l’exemple de jeu, la position de la caméra et les vecteurs de vue (le vecteur « de visualisation » qui pointe directement sur la scène depuis la caméra et le vecteur « de recherche » qui pointe vers le haut, à la perpendiculaire) définissent l’espace de la caméra. Les paramètres de projection déterminent la quantité de cet espace qui est vraiment visible dans la scène finale ; le champ de vision, les proportions et les plans de découpage définissent la transformation de la projection. Un nuanceur de vertex effectue la lourde tâche de convertir les coordonnées du modèle en coordonnées de périphérique avec l’algorithme suivant (où V est un vecteur et M est une matrice) :

`              V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)           `.

-   `M(model-to-world)` est une matrice de transformation pour les coordonnées du modèle en coordonnées universelles. Elle est fournie par la primitive. (Nous y reviendrons dans la section sur les primitives, ici.)
-   `M(world-to-view)` est une matrice de transformation pour les coordonnées universelles en coordonnées de vue. Elle est fournie par la matrice globale de la caméra.
-   `M(view-to-device)` est une matrice de transformation pour les coordonnées de vue en coordonnées de périphérique. Elle est fournie par la projection de la caméra.

Le code du nuanceur dans **VertexShader.hlsl** est chargé avec ces vecteurs et matrices à partir des mémoires tampons constantes et effectue cette transformation pour chaque vertex.

L’objet **Camera** définit les matrices globale et de projection. Examinons la façon dont l’exemple de jeu le déclare.

```cpp
ref class Camera
{
internal:
    Camera();

    // Call these from client and use Get*Matrix() to read new matrices.
    // Functions to change camera matrices
    void SetViewParams(_In_ DirectX::XMFLOAT3 eye, _In_ DirectX::XMFLOAT3 lookAt, _In_ DirectX::XMFLOAT3 up);
    void SetProjParams(_In_ float fieldOfView, _In_ float aspectRatio, _In_ float nearPlane, _In_ float farPlane);

    void LookDirection (_In_ DirectX::XMFLOAT3 lookDirection);
    void Eye (_In_ DirectX::XMFLOAT3 position);

    DirectX::XMMATRIX View();
    DirectX::XMMATRIX Projection();
    DirectX::XMMATRIX LeftEyeProjection();
    DirectX::XMMATRIX RightEyeProjection();
    DirectX::XMMATRIX World();
    DirectX::XMFLOAT3 Eye();
    DirectX::XMFLOAT3 LookAt();
    DirectX::XMFLOAT3 Up();
    float NearClipPlane();
    float FarClipPlane();
    float Pitch();
    float Yaw();

protected private:
    DirectX::XMFLOAT4X4 m_viewMatrix;                 // View matrix
    DirectX::XMFLOAT4X4 m_projectionMatrix;           // Projection matrix
    DirectX::XMFLOAT4X4 m_projectionMatrixLeft;       // Projection Matrix for Left Eye Stereo
    DirectX::XMFLOAT4X4 m_projectionMatrixRight;      // Projection Matrix for Right Eye Stereo

    DirectX::XMFLOAT4X4 m_inverseView;

    DirectX::XMFLOAT3 m_eye;                        // Camera eye position
    DirectX::XMFLOAT3 m_lookAt;                     // LookAt position
    DirectX::XMFLOAT3 m_up;                         // Up vector
    float             m_cameraYawAngle;             // Yaw angle of camera
    float             m_cameraPitchAngle;           // Pitch angle of camera

    float             m_fieldOfView;                // Field of view
    float             m_aspectRatio;                // Aspect ratio
    float             m_nearPlane;                  // Near plane
    float             m_farPlane;                   // Far plane
};
```

Il existe deux matrices 4x4 qui définissent les transformations apportées aux coordonnées de vue et de projection, **m_viewMatrix** et **m\_projectionMatrix**. (Pour la projection stéréo, vous utilisez deux matrices de projection : une pour la vue de chaque œil.) Elles sont calculées avec ces deux méthodes, respectivement :

-   **SetViewParams**
-   **SetProjParams**

Le code de ces deux méthodes se présente comme suit :

```cpp
void Camera::SetViewParams(
    _In_ XMFLOAT3 eye,
    _In_ XMFLOAT3 lookAt,
    _In_ XMFLOAT3 up
    )
{
    m_eye = eye;
    m_lookAt = lookAt;
    m_up = up;

    // Calc the view matrix.
    XMMATRIX view = XMMatrixLookAtLH(
        XMLoadFloat3(&m_eye),
        XMLoadFloat3(&m_lookAt),
        XMLoadFloat3(&m_up)
        );

    XMVECTOR det;
    XMMATRIX inverseView = XMMatrixInverse(&det, view);
    XMStoreFloat4x4(&m_viewMatrix, view);
    XMStoreFloat4x4(&m_inverseView, inverseView);

    // The axis basis vectors and camera position are stored inside the
    // position matrix in the 4 rows of the camera's world matrix.
    // To figure out the yaw/pitch of the camera, we just need the Z basis vector
    XMFLOAT3* zBasis = (XMFLOAT3*)&inverseView._31;

    m_cameraYawAngle = atan2f(zBasis->x, zBasis->z);

    float len = sqrtf(zBasis->z * zBasis->z + zBasis->x * zBasis->x);
    m_cameraPitchAngle = atan2f(zBasis->y, len);
}
//--------------------------------------------------------------------------------------
// Calculates the projection matrix based on input params
//--------------------------------------------------------------------------------------
void Camera::SetProjParams(
    _In_ float fieldOfView,
    _In_ float aspectRatio,
    _In_ float nearPlane,
    _In_ float farPlane
    )
{
    // Set attributes for the projection matrix
    m_fieldOfView = fieldOfView;
    m_aspectRatio = aspectRatio;
    m_nearPlane = nearPlane;
    m_farPlane = farPlane;
    XMStoreFloat4x4(
        &m_projectionMatrix,
        XMMatrixPerspectiveFovLH(
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane
            )
        );

    STEREO_PARAMETERS* stereoParams = nullptr;
    // change the projection matrix
    XMStoreFloat4x4(
        &m_projectionMatrixLeft,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::LEFT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );

    XMStoreFloat4x4(
        &m_projectionMatrixRight,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::RIGHT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );
}
```

Nous obtenons les données de vue et de projection produites en appelant les méthodes **View** et **Projection**, respectivement, sur l’objet **Camera**. Ces appels ont lieu lors de la prochaine étape que nous passerons en revue, la méthode **GameRenderer::Render** appelée dans la boucle de jeu.

Examinons maintenant la façon dont le jeu crée l’infrastructure pour dessiner les graphiques du jeu avec la caméra. Cela inclut la définition des primitives qui constituent le monde du jeu et ses éléments.

## Définition des primitives


Dans le code de l’exemple de jeu, nous définissons et implémentons les primitives dans deux classes de base, ainsi que les spécialisations correspondantes pour chaque type de primitive.

**MeshObject.h/.cpp** définit la classe de base pour tous les objets de maillage. Les fichiers **SphereMesh.h/.cpp**, **CylinderMesh.h/.cpp**, **FaceMesh.h/.cpp** et **WorldMesh.h/.cpp** contiennent le code qui remplit les mémoires tampons constantes pour chaque primitive avec le vertex et les données normales du vertex qui définissent la géométrie de la primitive. Ces fichiers de code sont un bon point de départ si vous cherchez à comprendre comment créer des primitives Direct3D dans votre propre application de jeu, mais nous n’en parlerons pas ici, car cela est trop propre à l’implémentation de ce jeu. Pour le moment, nous partons du principe que les mémoires tampons de vertex pour chaque primitive ont été remplies, et nous examinons la façon dont l’exemple de jeu gère ces mémoires tampons pour mettre à jour le jeu proprement dit.

La classe de base des objets qui représentent les primitives du point de vue du jeu est définie dans **GameObject.h./.cpp.** Cette classe, **GameObject**, définit les champs et méthodes pour les comportements courants entre toutes les primitives. Chaque type d’objet primitive en dérive. Examinons sa définition :

```cpp
ref class GameObject
{
internal:
    GameObject();

    // Expect these two functions to be overloaded by subclasses.
    virtual bool IsTouching(
        DirectX::XMFLOAT3 /* point */,
        float /* radius */,
        _Out_ DirectX::XMFLOAT3 *contact,
        _Out_ DirectX::XMFLOAT3 *normal
        )
    {
        *contact = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);
        *normal = DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f);
        return false;
    };

    void Render(
        _In_ ID3D11DeviceContext *context,
        _In_ ID3D11Buffer *primitiveConstantBuffer
        );

    void Active(bool active);
    bool Active();
    void Target(bool target);
    bool Target();
    void Hit(bool hit);
    bool Hit();
    void OnGround(bool ground);
    bool OnGround();
    void TargetId(int targetId);
    int  TargetId();
    void HitTime(float t);
    float HitTime();

    void     AnimatePosition(_In_opt_ Animate^ animate);
    Animate^ AnimatePosition();

    void         HitSound(_In_ SoundEffect^ hitSound);
    SoundEffect^ HitSound();

    void PlaySound(float impactSpeed, DirectX::XMFLOAT3 eyePoint);

    void Mesh(_In_ MeshObject^ mesh);

    void NormalMaterial(_In_ Material^ material);
    Material^ NormalMaterial();
    void HitMaterial(_In_ Material^ material);
    Material^ HitMaterial();

    void Position(DirectX::XMFLOAT3 position);
    void Position(DirectX::XMVECTOR position);
    void Velocity(DirectX::XMFLOAT3 velocity);
    void Velocity(DirectX::XMVECTOR velocity);
    DirectX::XMMATRIX ModelMatrix();
    DirectX::XMFLOAT3 Position();
    DirectX::XMVECTOR VectorPosition();
    DirectX::XMVECTOR VectorVelocity();
    DirectX::XMFLOAT3 Velocity();

protected private:
    virtual void UpdatePosition() {};
    // Object Data
    bool                m_active;
    bool                m_target;
    int                 m_targetId;
    bool                m_hit;
    bool                m_ground;

    DirectX::XMFLOAT3   m_position;
    DirectX::XMFLOAT3   m_velocity;
    DirectX::XMFLOAT4X4 m_modelMatrix;

    Material^           m_normalMaterial;
    Material^           m_hitMaterial;

    DirectX::XMFLOAT3   m_defaultXAxis;
    DirectX::XMFLOAT3   m_defaultYAxis;
    DirectX::XMFLOAT3   m_defaultZAxis;

    float               m_hitTime;

    Animate^            m_animatePosition;
    MeshObject^         m_mesh;

    SoundEffect^        m_hitSound;
};
```

La plupart des champs contiennent des données sur l’état, les propriétés visuelles ou la position de la primitive dans le monde du jeu. Il existe quelques méthodes en particulier qui sont nécessaires dans la plupart des jeux :

-   **Mesh**. Obtient la géométrie de maillage de la primitive, qui est stockée dans **m\_mesh**. Cette géométrie est définie dans **MeshObject.h/.cpp**.
-   **IsTouching**. Cette méthode détermine si la primitive se trouve à une distance spécifique d’un point et renvoie le point de la surface le plus proche du point et la perpendiculaire à la surface de l’objet au niveau de ce point. Étant donné que l’exemple ne se préoccupe que des collisions entre munitions et primitives, cela est suffisant pour la dynamique du jeu. Il ne s’agit pas d’une fonction d’intersection primitive-primitive universelle, même si elle pourrait servir de base à une telle fonction.
-   **AnimatePosition**. Met à jour le mouvement et l’animation de la primitive.
-   **UpdatePosition**. Met à jour la position de l’objet dans l’espace de coordonnées universelles.
-   **Render**. Place les propriétés matérielles de la primitive dans la mémoire tampon constante de primitives, puis restitue (trace) la géométrie de la primitive avec le contexte de périphérique.

Il est conseillé de créer un type d’objet de base qui définit l’ensemble minimal de méthodes pour une primitive, car la plupart des jeux ont un très grand nombre de primitives, et le code peut rapidement devenir difficile à gérer. Le code du jeu est également simplifié lorsque la boucle de mise à jour peut traiter les primitives de façon polymorphe, en laissant les objets eux-mêmes définir leurs propres comportements de mise à jour et de rendu.

Examinons le rendu de base d’une primitive dans l’exemple de jeu.

## Rendu des primitives


Les primitives de l’exemple de jeu utilisent la méthode de base **Render** implémentée sur la classe **GameObject** parente, comme ici :

```cpp
void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    m_mesh->Render(context);
}
```

La méthode **GameObject::Render** met à jour la mémoire tampon constante de primitives avec les données propres à une primitive donnée. Le jeu utilise plusieurs mémoires tampons constantes, mais n’a besoin de mettre à jour ces mémoires tampons qu’une seule fois par primitive.

Considérez les mémoires tampons constantes comme des données entrantes pour les nuanceurs qui sont exécutés pour chaque primitive. Certaines données sont statiques (**m_constantBufferNeverChanges**), certaines données sont constantes sur la trame (**m\_constantBufferChangesEveryFrame**), comme la position de la caméra, et certaines données sont propres à la primitive, comme ses couleurs et textures (**m\_constantBufferChangesEveryPrim**). Le convertisseur de jeu répartit ces données entrantes entre différentes mémoires tampons constantes pour optimiser la bande passante de mémoire utilisée par l’UC et le GPU. Cette approche permet également de réduire la quantité de données dont le GPU doit assurer le suivi. Gardez à l’esprit que le GPU a une longue file d’attente de commandes et que, chaque fois que le jeu appelle **Draw**, cette commande est mise en file d’attente avec les données qui lui sont associées. Lorsque le jeu met à jour la mémoire tampon constante de primitives et émet la commande **Draw** suivante, le pilote graphique ajoute cette commande suivante et les données associées à la file d’attente. Si le jeu dessine 100 primitives, la file d’attente peut contenir 100 copies des données de la mémoire tampon constante. Comme nous voulons réduire la quantité de données envoyées par le jeu au GPU, le jeu utilise une mémoire tampon constante de primitives distincte qui contient uniquement les mises à jour pour chaque primitive.

Si une collision (un coup) est détectée, **GameObject::Render** vérifie le contexte actuel, qui indique si la cible a été atteinte par une sphère de munitions. Si la cible a été atteinte, cette méthode applique une matière de coup qui inverse les couleurs des cercles de la cible pour indiquer un coup réussi au joueur. Sinon, elle applique la matière par défaut avec la même méthode. Dans les deux cas, elle définit la matière en appelant **Material::RenderSetup**, qui définit les constantes appropriées dans la mémoire tampon constante. Elle appelle alors [**ID3D11DeviceContext::PSSetShaderResources**](https://msdn.microsoft.com/library/windows/desktop/ff476473) pour définir la ressource de texture correspondante pour le nuanceur de pixels, et [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) et [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) pour définir les objets du nuanceur de vertex et du nuanceur de pixels proprement dits, respectivement.

Voici comment **Material::RenderSetup** configure les mémoires tampons constantes et affecte les ressources du nuanceur. Rappelons-le, notez que la mémoire tampon constante est celle utilisée pour appliquer les modifications aux primitives, de manière spécifique.

> **Remarque** La classe **Material** est définie dans **Material.h/.cpp**.

 

```cpp
void Material::RenderSetup(
    _In_ ID3D11DeviceContext* context,
    _Inout_ ConstantBufferChangesEveryPrim* constantBuffer
    )
{
    constantBuffer->meshColor = m_meshColor;
    constantBuffer->specularColor = m_specularColor;
    constantBuffer->specularPower = m_specularExponent;
    constantBuffer->diffuseColor = m_diffuseColor;

    context->PSSetShaderResources(0, 1, m_textureRV.GetAddressOf());
    context->VSSetShader(m_vertexShader.Get(), nullptr, 0);
    context->PSSetShader(m_pixelShader.Get(), nullptr, 0);
}
```

Enfin, **PrimObject::Render** appelle la méthode **Render** pour l’objet **MeshObject** sous-jacent.

```cpp
void MeshObject::Render(_In_ ID3D11DeviceContext *context)
{
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

À présent, la méthode **MeshObject::Render** de l’exemple de jeu met en file d’attente la commande de dessin pour exécuter les nuanceurs sur le GPU avec l’état de la scène en cours. Le nuanceur de vertex convertit la géométrie (vertex) des coordonnées du modèle en coordonnées de périphérique (universelles), en tenant compte de l’emplacement de la caméra et de la transformation de perspective. Enfin, les nuanceurs de pixels restituent les triangles transformés dans la mémoire tampon d’arrière-plan avec l’ensemble de textures ci-dessus.

Cela se produit au niveau du processus de rendu réel !

## Création du nuanceur de vertex et du nuanceur de pixels


À ce stade, l’exemple de jeu a défini les primitives à dessiner et les tampons constants qui définissent leur rendu. Ces tampons servent d’ensembles de paramètres aux nuanceurs qui sont exécutés sur le périphérique graphique. Ces programmes de nuanceurs sont de deux types :

-   Les nuanceurs de vertex exécutent les opérations par vertex, telles que les transformations et l’éclairage.
-   Les nuanceurs de pixels (ou de fragments) exécutent les opérations par pixel, telles que les textures et l’éclairage par pixel. Ils peuvent également être utilisés pour appliquer des effets de post-traitement sur les bitmaps, par exemple la cible de rendu final.

Le code de nuanceur est défini à l’aide du langage HLSL (High-Level Shader Language) qui, dans Direct3D 11, est compilé à partir d’un programme créé avec une syntaxe proche de celle du C. (La syntaxe complète est disponible [ici](https://msdn.microsoft.com/library/windows/desktop/bb509635).) Les deux nuanceurs principaux pour l’exemple de jeu sont définis dans **PixelShader.hlsl** et **VertexShader.hlsl**. (Deux nuanceurs de « faible puissance » définissent également les périphériques de faible puissance : **PixelShaderFlat.hlsl** et **VertexShaderFlat.hlsl**. Ces deux nuanceurs fournissent des effets limités, comme un manque de spéculaire en surbrillance sur les surfaces de textures.) Enfin, il existe un fichier .hlsli qui contient le format des mémoires tampons constantes, **ConstantBuffers.hlsli**.

**ConstantBuffers.hlsli** est défini comme suit :

```cpp
Texture2D diffuseTexture : register(t0);
SamplerState linearSampler : register(s0);

cbuffer ConstantBufferNeverChanges : register(b0)
{
    float4 lightPosition[4];
    float4 lightColor;
}

cbuffer ConstantBufferChangeOnResize : register(b1)
{
    matrix projection;
};

cbuffer ConstantBufferChangesEveryFrame : register(b2)
{
    matrix view;
};

cbuffer ConstantBufferChangesEveryPrim : register (b3)
{
    matrix world;
    float4 meshColor;
    float4 diffuseColor;
    float4 specularColor;
    float  specularExponent;
};

struct VertextShaderInput
{
    float4 position : POSITION;
    float4 normal : NORMAL;
    float2 textureUV : TEXCOORD0;
};

struct PixelShaderInput
{
    float4 position : SV_POSITION;
    float2 textureUV : TEXCOORD0;
    float3 vertexToEye : TEXCOORD1;
    float3 normal : TEXCOORD2;
    float3 vertexToLight0 : TEXCOORD3;
    float3 vertexToLight1 : TEXCOORD4;
    float3 vertexToLight2 : TEXCOORD5;
    float3 vertexToLight3 : TEXCOORD6;
};

struct PixelShaderFlatInput
{
    float4 position : SV_POSITION;
    float2 textureUV : TEXCOORD0;
    float4 diffuseColor : TEXCOORD1;
};
```

**VertexShader.hlsl** est défini comme suit :

VertexShader.hlsl

```cpp
#include "ConstantBuffers.hlsli"

PixelShaderInput main(VertextShaderInput input)
{
    PixelShaderInput output = (PixelShaderInput)0;

    output.position = mul(mul(mul(input.position, world), view), projection);
    output.textureUV = input.textureUV;

    // compute view space normal
    output.normal = normalize (mul(mul(input.normal.xyz, (float3x3)world), (float3x3)view));

    // Vertex pos in view space (normalize in pixel shader)
    output.vertexToEye = -mul(mul(input.position, world), view).xyz;

    // Compute view space vertex to light vectors (normalized)
    output.vertexToLight0 = normalize(mul(lightPosition[0], view ).xyz + output.vertexToEye);
    output.vertexToLight1 = normalize(mul(lightPosition[1], view ).xyz + output.vertexToEye);
    output.vertexToLight2 = normalize(mul(lightPosition[2], view ).xyz + output.vertexToEye);
    output.vertexToLight3 = normalize(mul(lightPosition[3], view ).xyz + output.vertexToEye);

    return output;
}
```

La fonction **main** dans **VertexShader.hlsl** effectue la séquence de transformation de vertex dont nous avons parlé dans la section sur la caméra. Elle est exécutée une fois par vertex. Les sorties produites sont transmises au code du nuanceur de pixels pour les effets de matière et de texture.

PixelShader.hlsl

```cpp
#include "ConstantBuffers.hlsli"

float4 main(PixelShaderInput input) : SV_Target
{
    float diffuseLuminance =
        max(0.0f, dot(input.normal, input.vertexToLight0)) +
        max(0.0f, dot(input.normal, input.vertexToLight1)) +
        max(0.0f, dot(input.normal, input.vertexToLight2)) +
        max(0.0f, dot(input.normal, input.vertexToLight3));

    // Normalize view space vertex-to-eye
    input.vertexToEye = normalize(input.vertexToEye);

    float specularLuminance = 
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight0))), specularExponent) +
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight1))), specularExponent) +
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight2))), specularExponent) +
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight3))), specularExponent);

    float4 specular;
    specular = specularColor * specularLuminance * 0.5f;

   return diffuseTexture.Sample(linearSampler, input.textureUV) *  diffuseColor * diffuseLuminance * 0.5f + specular;
}
```

La fonction **main** dans **PixelShader.hlsl** prend les projections 2-D des surfaces en triangle pour chaque primitive de la scène, puis calcule la valeur de couleur de chaque pixel des surfaces visibles en fonction des textures et des effets (ici, l’éclairage spéculaire) qui leur sont appliqués.

Rassemblons maintenant toutes ces idées (primitives, caméra et nuanceurs) et voyons comment l’exemple de jeu génère le processus de rendu complet.

## Rendu de la trame pour la sortie


Nous avons brièvement abordé cette méthode dans [Définition de l’objet jeu principal](tutorial--defining-the-main-game-loop.md). Examinons-la maintenant un peu plus en détail.

```cpp
void GameRenderer::Render()
{
    int renderingPasses = 1;
    if (m_stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        if (m_stereoEnabled && i > 0)
        {
            // Doing the Right Eye View
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetViewRight.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmapRight.Get());
        }
        else
        {
            // Doing the Mono or Left Eye View
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetView.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
        }

        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (m_stereoEnabled)
            {
                ConstantBufferChangeOnResize changesOnResize;
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                        )
                    );

                m_d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }
            // Update variables that change one time per frame

            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            m_d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Setup Pipeline

            m_d3dContext->IASetInputLayout(m_vertexLayout.Get());
            m_d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            m_d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            m_d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            // Get the all the objects to render from the Game state.
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(m_d3dContext.Get(), m_constantBufferChangesEveryPrim.Get());
            }
        }
        else
        {
            const float ClearColor[4] = {0.1f, 0.1f, 0.1f, 1.0f};

            // Only need to clear the background when not rendering the full 3-D scene because
            // the 3-D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (m_stereoEnabled && i > 0)
            {
                // Doing the Right Eye View
                m_d3dContext->ClearRenderTargetView(m_renderTargetViewRight.Get(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View
                m_d3dContext->ClearRenderTargetView(m_renderTargetView.Get(), ClearColor);
            }
        }

        m_d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        m_d2dContext->SetTransform(m_rotationTransform2D);

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game, m_d2dContext.Get(), m_windowBounds);
        }

        if (m_gameInfoOverlay->Visible())
        {
            m_d2dContext->DrawBitmap(
                m_gameInfoOverlay->Bitmap(),
                D2D1::RectF(
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f,
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f + GameInfoOverlayConstant::Width,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f + GameInfoOverlayConstant::Height
                    )
                );
        }

        HRESULT hr = m_d2dContext->EndDraw();
        if (hr != D2DERR_RECREATE_TARGET)
        {
            // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
            // D3D device.  All subsequent rendering will be ignored until the device is recreated.
            // This error will be propagated and the appropriate D3D error will be returned from the
            // swapchain->Present(...) call.   At that point, the sample will recreate the device
            // and all associated resources.  As a result the D2DERR_RECREATE_TARGET doesn't
            // need to be handled here.
            DX::ThrowIfFailed(hr);
        }
    }
    Present();
}
```

Le jeu dispose de toutes les pièces pour assembler une vue pour la sortie : des primitives et les règles pour leur comportement, un objet caméra pour fournir la vue du monde du jeu du joueur et les ressources graphiques pour le dessin.

Examinons à présent le processus qui permet de tout rassembler.

1.  Si le mode 3D stéréo est activé, configurez le processus de rendu suivant pour qu’il s’exécute deux fois, une fois pour chaque œil.
2.  La scène entière est englobée dans un volume du monde. Dessinez donc chaque pixel (même ceux dont vous n’avez pas besoin) pour effacer les plans de couleurs de la cible rendue. Définissez la valeur par défaut pour le tampon de profondeur/gabarit.
3.  Mettez à jour le tampon constant pour les données de mise à jour de trame en utilisant les données et la matrice globale de la caméra.
4.  Configurez le contexte Direct3D pour qu’il utilise les quatre mémoires tampons de contenu qui ont été définies précédemment.
5.  Appelez la méthode **Render** sur chaque objet primitive. Cela aboutit à un appel **Draw** ou **DrawIndexed** sur le contexte pour dessiner la géométrie de chaque primitive. Plus spécifiquement, cet appel **Draw** met en file d’attente des commandes et données pour le GPU, comme paramétré par les données de la mémoire tampon constante. Chaque appel de dessin exécute le nuanceur de vertex une fois par vertex, puis le nuanceur de pixels une fois pour chaque pixel de chaque triangle dans la primitive. Les textures font partie de l’état utilisé par le nuanceur de pixels pour effectuer le rendu.
6.  Dessinez l’affichage à tête haute et la superposition en utilisant le contexte Direct2D.
7.  Appelez **DirectXBase::Present**.

Le jeu a mis à jour l’affichage ! Il s’agit là du processus de base pour l’implémentation de l’infrastructure graphique d’un jeu. Bien entendu, plus votre jeu est volumineux, plus grand est le nombre d’abstractions que vous devez mettre en place pour gérer cette complexité, par exemple des hiérarchies entières de types d’objets et de comportements d’animations, et plus complexes sont les méthodes pour charger et gérer les composants, tels que les maillages et textures.

## Étapes suivantes


Avançons et examinons quelques points importants de l’exemple de jeu que nous n’avons fait que survoler : [la superposition de l’interface utilisateur](tutorial--adding-a-user-interface.md), [les contrôles d’entrée](tutorial--adding-controls.md) et [le son](tutorial--adding-sound.md).

## Exemple de code complet pour cette section


Camera.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class Camera
{
internal:
    Camera();

    // Call these from client and use Get*Matrix() to read new matrices.
    // Functions to change camera matrices.
    void SetViewParams(_In_ DirectX::XMFLOAT3 eye, _In_ DirectX::XMFLOAT3 lookAt, _In_ DirectX::XMFLOAT3 up);
    void SetProjParams(_In_ float fieldOfView, _In_ float aspectRatio, _In_ float nearPlane, _In_ float farPlane);

    void LookDirection (_In_ DirectX::XMFLOAT3 lookDirection);
    void Eye (_In_ DirectX::XMFLOAT3 position);

    DirectX::XMMATRIX View();
    DirectX::XMMATRIX Projection();
    DirectX::XMMATRIX LeftEyeProjection();
    DirectX::XMMATRIX RightEyeProjection();
    DirectX::XMMATRIX World();
    DirectX::XMFLOAT3 Eye();
    DirectX::XMFLOAT3 LookAt();
    DirectX::XMFLOAT3 Up();
    float NearClipPlane();
    float FarClipPlane();
    float Pitch();
    float Yaw();

protected private:
    DirectX::XMFLOAT4X4 m_viewMatrix;                 // View matrix
    DirectX::XMFLOAT4X4 m_projectionMatrix;           // Projection matrix
    DirectX::XMFLOAT4X4 m_projectionMatrixLeft;       // Projection Matrix for Left Eye Stereo
    DirectX::XMFLOAT4X4 m_projectionMatrixRight;      // Projection Matrix for Right Eye Stereo

    DirectX::XMFLOAT4X4 m_inverseView;

    DirectX::XMFLOAT3 m_eye;                        // Camera eye position
    DirectX::XMFLOAT3 m_lookAt;                     // LookAt position
    DirectX::XMFLOAT3 m_up;                         // Up vector
    float             m_cameraYawAngle;             // Yaw angle of camera
    float             m_cameraPitchAngle;           // Pitch angle of camera

    float             m_fieldOfView;                // Field of view
    float             m_aspectRatio;                // Aspect ratio
    float             m_nearPlane;                  // Near plane
    float             m_farPlane;                   // Far plane
};
```

Camera.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Camera.h"
#include "StereoProjection.h"

using namespace DirectX;

#undef min // use __min instead
#undef max // use __max instead
//--------------------------------------------------------------------------------------
// Constructor
//--------------------------------------------------------------------------------------
Camera::Camera()
{
    // Setup the view matrix.
    SetViewParams(
        XMFLOAT3(0.0f, 0.0f, 0.0f),   // default eye position.
        XMFLOAT3(0.0f, 0.0f, 1.0f),   // default look at position.
        XMFLOAT3(0.0f, 1.0f, 0.0f)    // default up vector.
        );

    // Setup the projection matrix.
    SetProjParams(XM_PI / 4, 1.0f, 1.0f, 1000.0f);
}
//--------------------------------------------------------------------------------------
void Camera::LookDirection (_In_ XMFLOAT3 lookDirection)
{
    XMFLOAT3 lookAt;
    lookAt.x = m_eye.x + lookDirection.x;
    lookAt.y = m_eye.y + lookDirection.y;
    lookAt.z = m_eye.z + lookDirection.z;

    SetViewParams(m_eye, lookAt, m_up);
}
//--------------------------------------------------------------------------------------
void Camera::Eye(_In_ XMFLOAT3 eye)
{
    SetViewParams(eye, m_lookAt, m_up);
}
//--------------------------------------------------------------------------------------
void Camera::SetViewParams(
    _In_ XMFLOAT3 eye,
    _In_ XMFLOAT3 lookAt,
    _In_ XMFLOAT3 up
    )
{
    m_eye = eye;
    m_lookAt = lookAt;
    m_up = up;

    // Calculate the view matrix.
    XMMATRIX view = XMMatrixLookAtLH(
        XMLoadFloat3(&m_eye),
        XMLoadFloat3(&m_lookAt),
        XMLoadFloat3(&m_up)
        );

    XMVECTOR det;
    XMMATRIX inverseView = XMMatrixInverse(&det, view);
    XMStoreFloat4x4(&m_viewMatrix, view);
    XMStoreFloat4x4(&m_inverseView, inverseView);

    // The axis basis vectors and camera position are stored inside the
    // position matrix in the four rows of the camera's world matrix.
    // To figure out the yaw/pitch of the camera, we just need the Z basis vector.
    XMFLOAT3 zBasis;
    XMStoreFloat3(&zBasis, inverseView.r[2]);

    m_cameraYawAngle = atan2f(zBasis.x, zBasis.z);

    float len = sqrtf(zBasis.z * zBasis.z + zBasis.x * zBasis.x);
    m_cameraPitchAngle = atan2f(zBasis.y, len);
}
//--------------------------------------------------------------------------------------
// Calculates the projection matrix based on input params.
//--------------------------------------------------------------------------------------
void Camera::SetProjParams(
    _In_ float fieldOfView,
    _In_ float aspectRatio,
    _In_ float nearPlane,
    _In_ float farPlane
    )
{
    // Set attributes for the projection matrix.
    m_fieldOfView = fieldOfView;
    m_aspectRatio = aspectRatio;
    m_nearPlane = nearPlane;
    m_farPlane = farPlane;
    XMStoreFloat4x4(
        &m_projectionMatrix,
        XMMatrixPerspectiveFovLH(
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane
            )
        );

    STEREO_PARAMETERS* stereoParams = nullptr;
    // Change the projection matrix.
    XMStoreFloat4x4(
        &m_projectionMatrixLeft,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::LEFT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );

    XMStoreFloat4x4(
        &m_projectionMatrixRight,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::RIGHT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );
}
//--------------------------------------------------------------------------------------
DirectX::XMMATRIX Camera::View()
{
    return XMLoadFloat4x4(&m_viewMatrix);
}
DirectX::XMMATRIX Camera::Projection()
{
    return XMLoadFloat4x4(&m_projectionMatrix);
}
DirectX::XMMATRIX Camera::LeftEyeProjection()
{
    return XMLoadFloat4x4(&m_projectionMatrixLeft);
}
DirectX::XMMATRIX Camera::RightEyeProjection()
{
    return XMLoadFloat4x4(&m_projectionMatrixRight);
}
DirectX::XMMATRIX Camera::World()
{
    return XMLoadFloat4x4(&m_inverseView);
}
DirectX::XMFLOAT3 Camera::Eye()
{
    return m_eye;
}
DirectX::XMFLOAT3 Camera::LookAt()
{
    return m_lookAt;
}
float Camera::NearClipPlane()
{
    return m_nearPlane;
}
float Camera::FarClipPlane()
{
    return m_farPlane;
}
float Camera::Pitch()
{
    return m_cameraPitchAngle;
}
float Camera::Yaw()
{
    return m_cameraYawAngle;
}
```

GameRenderer.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "DirectXBase.h"
#include "GameInfoOverlay.h"
#include "GameHud.h"
#include "Simple3DGame.h"

ref class Simple3DGame;
ref class GameHud;

ref class GameRenderer : public DirectXBase
{
internal:
    GameRenderer();

    virtual void Initialize(
        _In_ Windows::UI::Core::CoreWindow^ window,
        float dpi
        ) override;

    virtual void CreateDeviceIndependentResources() override;
    virtual void CreateDeviceResources() override;
    virtual void UpdateForWindowSizeChange() override;
    virtual void Render() override;
    virtual void SetDpi(float dpi) override;

    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    GameInfoOverlay^ InfoOverlay()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f
            );
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f + GameInfoOverlayConstant::Width,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f + GameInfoOverlayConstant::Height
            );
    };

protected private:
    bool                                                m_initialized;
    bool                                                m_gameResourcesLoaded;
    bool                                                m_levelResourcesLoaded;
    GameInfoOverlay^                                    m_gameInfoOverlay;
    GameHud^                                            m_gameHud;
    Simple3DGame^                                       m_game;

    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_sphereTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_cylinderTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_ceilingTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_floorTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant Buffers.
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferNeverChanges;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

GameRenderer.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "DirectXSample.h"
#include "GameRenderer.h"
#include "ConstantBuffers.h"
#include "TargetTexture.h"
#include "BasicLoader.h"
#include "CylinderMesh.h"
#include "FaceMesh.h"
#include "SphereMesh.h"
#include "WorldMesh.h"
#include "Face.h"
#include "Sphere.h"
#include "Cylinder.h"

using namespace concurrency;
using namespace DirectX;
using namespace Microsoft::WRL;
using namespace Windows::UI::Core;

//----------------------------------------------------------------------

GameRenderer::GameRenderer() :
    m_initialized(false),
    m_gameResourcesLoaded(false),
    m_levelResourcesLoaded(false)
{
}

//----------------------------------------------------------------------

void GameRenderer::Initialize(
    _In_ CoreWindow^ window,
    float dpi
    )
{
    if (!m_initialized)
    {
        m_gameHud = ref new GameHud(
            "Windows 8 Samples",
            "DirectX first-person game sample"
            );
        m_gameInfoOverlay = ref new GameInfoOverlay();
        m_initialized = true;
    }

    DirectXBase::Initialize(window, dpi);

    // Initialize could be called multiple times as a result of an error with the hardware device
    // that requires it to be reinitialized.  Since the m_gameInfoOverlay has resources that are
    // dependent on the device, it will need to be reinitialized each time with the new device information.
    m_gameInfoOverlay->Initialize(m_d2dDevice.Get(), m_d2dContext.Get(), m_dwriteFactory.Get(), dpi);
}

//----------------------------------------------------------------------

void GameRenderer::CreateDeviceIndependentResources()
{
    DirectXBase::CreateDeviceIndependentResources();
    m_gameHud->CreateDeviceIndependentResources(m_dwriteFactory.Get(), m_wicFactory.Get());
}

//----------------------------------------------------------------------

void GameRenderer::CreateDeviceResources()
{
    DirectXBase::CreateDeviceResources();

    m_gameHud->CreateDeviceResources(m_d2dContext.Get());

    if (m_game != nullptr)
    {
        // The initial invocation of CreateDeviceResources will occur
        // before the Game State is initialized when the device is first
        // being created, so that the inital loading screen can be displayed.
        // Subsequent invocations of CreateDeviceResources will be a result
        // of an error with the Device that requires the resources to be
        // recreated.  In this case, the game state is already initialized
        // so the game device resources need to be recreated.

        // This sample doesn't gracefully handle all the async recreation
        // of resources, so an exception is thrown.
        throw Platform::Exception::CreateException(
            DXGI_ERROR_DEVICE_REMOVED,
            "GameRenderer::CreateDeviceResources - Recreation of resources after TDR not available\n"
            );
    }
}

//----------------------------------------------------------------------

void GameRenderer::UpdateForWindowSizeChange()
{
    DirectXBase::UpdateForWindowSizeChange();

    m_gameHud->UpdateForWindowSizeChange(m_windowBounds);

    // Update the Projection Matrix and update the associated Constant Buffer.
    if (m_game != nullptr)
    {
        m_game->GameCamera()->SetProjParams(
            XM_PI / 2, m_renderTargetSize.Width / m_renderTargetSize.Height,
            0.01f,
            100.0f
            );
        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                )
            );

        m_d3dContext->UpdateSubresource(
            m_constantBufferChangeOnResize.Get(),
            0,
            nullptr,
            &changesOnResize,
            0,
            0
            );
    }
}

//----------------------------------------------------------------------

void GameRenderer::SetDpi(float dpi)
{
    DirectXBase::SetDpi(dpi);

    if (m_gameInfoOverlay)
    {
        m_gameInfoOverlay->SetDpi(dpi);
    }
}

//----------------------------------------------------------------------

task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Set the Loading state to wait until any async resources have
    // been loaded before proceeding.
    m_game = game;

    // NOTE: Only the m_d3dDevice is used in this method.  It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the m_d3dDevice are free-threaded and are safe while any methods
    // in the m_d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.

    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers,
    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
    bd.CPUAccessFlags = 0;
    bd.ByteWidth = (sizeof(ConstantBufferNeverChanges) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangeOnResize) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangeOnResize)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryFrame) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryFrame)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryPrim) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryPrim)
        );

    D3D11_SAMPLER_DESC sampDesc;
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;
    sampDesc.MinLOD = 0;
    sampDesc.MaxLOD = FLT_MAX;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    m_cylinderTexture = nullptr;
    m_ceilingTexture = nullptr;
    m_floorTexture = nullptr;
    m_wallsTexture = nullptr;

    // Load Game specific textures.
    tasks.push_back(loader->LoadTextureAsync("seafloor.dds", nullptr, &m_sphereTexture));
    tasks.push_back(loader->LoadTextureAsync("metal_texture.dds", nullptr, &m_cylinderTexture));
    tasks.push_back(loader->LoadTextureAsync("cellceiling.dds", nullptr, &m_ceilingTexture));
    tasks.push_back(loader->LoadTextureAsync("cellfloor.dds", nullptr, &m_floorTexture));
    tasks.push_back(loader->LoadTextureAsync("cellwall.dds", nullptr, &m_wallsTexture));
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Return the task group of all the async tasks for loading the shader and texture assets.
    return when_all(tasks.begin(), tasks.end());
}

//----------------------------------------------------------------------

void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now, associate all the resources with the appropriate
    // Game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[1] = XMFLOAT4( 3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[2] = XMFLOAT4(-3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[3] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);
    m_d3dContext->UpdateSubresource(m_constantBufferNeverChanges.Get(), 0, nullptr, &constantBufferNeverChanges, 0, 0);

    // For the targets, there are two unique generated textures.
    // Each texture image includes the number of the texture.
    // Make sure the 2D rendering is occurring on the same thread
    // as the main rendering.

    TargetTexture^ textureGenerator = ref new TargetTexture(
        m_d3dDevice.Get(),
        m_d2dFactory.Get(),
        m_dwriteFactory.Get(),
        m_d2dContext.Get()
        );

    MeshObject^ cylinderMesh = ref new CylinderMesh(m_d3dDevice.Get(), 26);
    MeshObject^ targetMesh = ref new FaceMesh(m_d3dDevice.Get());
    MeshObject^ sphereMesh = ref new SphereMesh(m_d3dDevice.Get(), 26);

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    Material^ sphereMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        50.0f,
        m_sphereTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldFloorMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldCeilingId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_ceilingTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldCeilingMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldWallsId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_wallsTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldWallsMesh(m_d3dDevice.Get()));
        }
        else if (Cylinder^ cylinder = dynamic_cast<Cylinder^>(*object))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (Face^ target = dynamic_cast<Face^>(*object))
        {
            const int bufferLength = 16;
            char16 str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            Platform::String^ string = ref new Platform::String(str, len);

            ComPtr<ID3D11ShaderResourceView> texture;
            textureGenerator->CreateTextureResourceView(string, &texture);
            target->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            textureGenerator->CreateHitTextureResourceView(string, &texture);
            target->HitMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            target->Mesh(targetMesh);
        }
        else if (Sphere^ sphere = dynamic_cast<Sphere^>(*object))
        {
            sphere->Mesh(sphereMesh);
            sphere->NormalMaterial(sphereMaterial);
        }
    }

    // Ensure that the camera has been initialized with the right Projection
    // matrix.  The camera is not created at the time the first window resize event
    // occurs.
    m_game->GameCamera()->SetProjParams(
        XM_PI / 2, m_renderTargetSize.Width / m_renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that Projection matrix has been set in the constant buffer
    // now that all the resources are loaded.
    // DirectXBase is handling screen rotations directly to eliminate an unaligned
    // fullscreen copy.  As a result, it is necessary to post multiply the rotationTransform3D
    // matrix to the camera projection matrix.
    ConstantBufferChangeOnResize changesOnResize;
    XMStoreFloat4x4(
        &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                )
        );

    m_d3dContext->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    m_gameResourcesLoaded = true;
}

//----------------------------------------------------------------------

task<void> GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;

    return create_task([this]()
    {
        // This is where additional async loading of level specific resources
        // would be done.  Because there aren't any to load, just simulate
        // by delaying for some time.
        wait(GameConstants::LevelLoadingDelay);
    });
}

//----------------------------------------------------------------------

void GameRenderer::FinalizeLoadLevelResources()
{
    // After the level specific resources had been loaded, this method is
    // where D3D context specific actions would be handled.  This method
    // runs in the same thread context as the GameRenderer was created.

    m_levelResourcesLoaded = true;
}

//----------------------------------------------------------------------

void GameRenderer::Render()
{
    int renderingPasses = 1;
    if (m_stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        if (m_stereoEnabled && i > 0)
        {
            // Doing the Right Eye View.
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetViewRight.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmapRight.Get());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetView.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
        }

        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (m_stereoEnabled)
            {
                ConstantBufferChangeOnResize changesOnResize;
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                        )
                    );

                m_d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }
            // Update variables that change one time per frame.

            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            m_d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Set up Pipeline.

            m_d3dContext->IASetInputLayout(m_vertexLayout.Get());
            m_d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            m_d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            m_d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            // Get all the objects to render from the Game state.
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(m_d3dContext.Get(), m_constantBufferChangesEveryPrim.Get());
            }
        }
        else
        {
            const float ClearColor[4] = {0.1f, 0.1f, 0.1f, 1.0f};

            // Only need to clear the background when not rendering the full 3-D scene because
            // the 3-D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (m_stereoEnabled && i > 0)
            {
                // Doing the Right Eye View.
                m_d3dContext->ClearRenderTargetView(m_renderTargetViewRight.Get(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View.
                m_d3dContext->ClearRenderTargetView(m_renderTargetView.Get(), ClearColor);
            }
        }

        m_d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        m_d2dContext->SetTransform(m_rotationTransform2D);

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game, m_d2dContext.Get(), m_windowBounds);
        }

        if (m_gameInfoOverlay->Visible())
        {
            m_d2dContext->DrawBitmap(
                m_gameInfoOverlay->Bitmap(),
                D2D1::RectF(
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f,
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f + GameInfoOverlayConstant::Width,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f + GameInfoOverlayConstant::Height
                    )
                );
        }

        HRESULT hr = m_d2dContext->EndDraw();
        if (hr != D2DERR_RECREATE_TARGET)
        {
            // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
            // D3D device.  All subsequent rendering will be ignored until the device is recreated.
            // This error will be propagated and the appropriate D3D error will be returned from the
            // swapchain->Present(...) call.   At that point, the sample will recreate the device
            // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
            // need to be handled here.
            DX::ThrowIfFailed(hr);
        }
    }
    Present();
}
```

GameObject.h

```cpp
/// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "MeshObject.h"
#include "SoundEffect.h"
#include "Animate.h"
#include "Material.h"

ref class GameObject
{
internal:
    GameObject();

    // Expect these two functions to be overloaded by subclasses.
    virtual bool IsTouching(
        DirectX::XMFLOAT3 /* point */,
        float /* radius */,
        _Out_ DirectX::XMFLOAT3 *contact,
        _Out_ DirectX::XMFLOAT3 *normal
        )
    {
        *contact = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);
        *normal = DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f);
        return false;
    };

    void Render(
        _In_ ID3D11DeviceContext *context,
        _In_ ID3D11Buffer *primitiveConstantBuffer
        );

    void Active(bool active);
    bool Active();
    void Target(bool target);
    bool Target();
    void Hit(bool hit);
    bool Hit();
    void OnGround(bool ground);
    bool OnGround();
    void TargetId(int targetId);
    int  TargetId();
    void HitTime(float t);
    float HitTime();

    void     AnimatePosition(_In_opt_ Animate^ animate);
    Animate^ AnimatePosition();

    void         HitSound(_In_ SoundEffect^ hitSound);
    SoundEffect^ HitSound();

    void PlaySound(float impactSpeed, DirectX::XMFLOAT3 eyePoint);

    void Mesh(_In_ MeshObject^ mesh);

    void NormalMaterial(_In_ Material^ material);
    Material^ NormalMaterial();
    void HitMaterial(_In_ Material^ material);
    Material^ HitMaterial();

    void Position(DirectX::XMFLOAT3 position);
    void Position(DirectX::XMVECTOR position);
    void Velocity(DirectX::XMFLOAT3 velocity);
    void Velocity(DirectX::XMVECTOR velocity);
    DirectX::XMMATRIX ModelMatrix();
    DirectX::XMFLOAT3 Position();
    DirectX::XMVECTOR VectorPosition();
    DirectX::XMVECTOR VectorVelocity();
    DirectX::XMFLOAT3 Velocity();

protected private:
    virtual void UpdatePosition() {};
    // Object Data.
    bool                m_active;
    bool                m_target;
    int                 m_targetId;
    bool                m_hit;
    bool                m_ground;

    DirectX::XMFLOAT3   m_position;
    DirectX::XMFLOAT3   m_velocity;
    DirectX::XMFLOAT4X4 m_modelMatrix;

    Material^           m_normalMaterial;
    Material^           m_hitMaterial;

    DirectX::XMFLOAT3   m_defaultXAxis;
    DirectX::XMFLOAT3   m_defaultYAxis;
    DirectX::XMFLOAT3   m_defaultZAxis;

    float               m_hitTime;

    Animate^            m_animatePosition;
    MeshObject^         m_mesh;

    SoundEffect^        m_hitSound;
};


__forceinline void GameObject::Active(bool active)
{
    m_active = active;
}

__forceinline bool GameObject::Active()
{
    return m_active;
}

__forceinline void GameObject::Target(bool target)
{
    m_target = target;
}

__forceinline bool GameObject::Target()
{
    return m_target;
}

__forceinline void GameObject::Hit(bool hit)
{
    m_hit = hit;
}

__forceinline bool GameObject::Hit()
{
    return m_hit;
}

__forceinline void GameObject::OnGround(bool ground)
{
    m_ground = ground;
}

__forceinline bool GameObject::OnGround()
{
    return m_ground;
}

__forceinline void GameObject::TargetId(int targetId)
{
    m_targetId = targetId;
}

__forceinline int GameObject::TargetId()
{
    return m_targetId;
}

__forceinline void GameObject::HitTime(float t)
{
    m_hitTime = t;
}

__forceinline float GameObject::HitTime()
{
    return m_hitTime;
}

__forceinline void GameObject::Position(DirectX::XMFLOAT3 position)
{
    m_position = position;
    // Update any internal states that are dependent on the position.
    // UpdatePosition is a virtual function that is specific to the derived class.
    UpdatePosition();
}

__forceinline void GameObject::Position(DirectX::XMVECTOR position)
{
    XMStoreFloat3(&m_position, position);
   // Update any internal states that are dependent on the position.
    // UpdatePosition is a virtual function that is specific to the derived class.
    UpdatePosition();
}

__forceinline DirectX::XMFLOAT3 GameObject::Position()
{
    return m_position;
}

__forceinline DirectX::XMVECTOR GameObject::VectorPosition()
{
    return DirectX::XMLoadFloat3(&m_position);
}

__forceinline void GameObject::Velocity(DirectX::XMFLOAT3 velocity)
{
    m_velocity = velocity;
}

__forceinline void GameObject::Velocity(DirectX::XMVECTOR velocity)
{
    XMStoreFloat3(&m_velocity, velocity);
}

__forceinline DirectX::XMFLOAT3 GameObject::Velocity()
{
    return m_velocity;
}

__forceinline DirectX::XMVECTOR GameObject::VectorVelocity()
{
    return DirectX::XMLoadFloat3(&m_velocity);
}

__forceinline void GameObject::AnimatePosition(_In_opt_ Animate^ animate)
{
    m_animatePosition = animate;
}

__forceinline Animate^ GameObject::AnimatePosition()
{
    return m_animatePosition;
}

__forceinline void GameObject::NormalMaterial(_In_ Material^ material)
{
    m_normalMaterial = material;
}

__forceinline Material^ GameObject::NormalMaterial()
{
    return m_normalMaterial;
}

__forceinline void GameObject::HitMaterial(_In_ Material^ material)
{
    m_hitMaterial = material;
}

__forceinline Material^ GameObject::HitMaterial()
{
    return m_hitMaterial;
}

__forceinline void GameObject::Mesh(_In_ MeshObject^ mesh)
{
    m_mesh = mesh;
}

__forceinline void GameObject::HitSound(_In_ SoundEffect^ hitSound)
{
    m_hitSound = hitSound;
}

__forceinline SoundEffect^ GameObject::HitSound()
{
    return m_hitSound;
}

__forceinline DirectX::XMMATRIX GameObject::ModelMatrix()
{
    return DirectX::XMLoadFloat4x4(&m_modelMatrix);
}
```

GameObject.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "GameObject.h"
#include "ConstantBuffers.h"
#include "GameConstants.h"

using namespace DirectX;

//--------------------------------------------------------------------------------

GameObject::GameObject() :
    m_normalMaterial(nullptr),
    m_hitMaterial(nullptr)
{
    m_active          = false;
    m_target          = false;
    m_targetId        = 0;
    m_hit             = false;
    m_ground          = true;

    m_position        = XMFLOAT3(0.0f, 0.0f, 0.0f);
    m_velocity        = XMFLOAT3(0.0f, 0.0f, 0.0f);
    m_defaultXAxis    = XMFLOAT3(1.0f, 0.0f, 0.0f);
    m_defaultYAxis    = XMFLOAT3(0.0f, 1.0f, 0.0f);
    m_defaultZAxis    = XMFLOAT3(0.0f, 0.0f, 1.0f);
    XMStoreFloat4x4(&m_modelMatrix, XMMatrixIdentity());

    m_hitTime         = 0.0f;

    m_animatePosition = nullptr;
}

//----------------------------------------------------------------------

void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    m_mesh->Render(context);
}

//----------------------------------------------------------------------

void GameObject::PlaySound(float impactSpeed, XMFLOAT3 eyePoint)
{
    if (m_hitSound != nullptr)
    {
        // Determine the sound volume adjustment based on velocity.
        float adjustment;
        if (impactSpeed < GameConstants::Sound::MinVelocity)
        {
            adjustment = 0.0f;  // Too soft.  Don't play sound.
        }
        else
        {
            adjustment = min(1.0f, impactSpeed / GameConstants::Sound::MaxVelocity);
            adjustment = GameConstants::Sound::MinAdjustment + adjustment * (1.0f - GameConstants::Sound::MinAdjustment);
        }

        // Compute Distance to eyePoint to adjust the volume based on that distance.
        XMVECTOR cameraToPosition = XMLoadFloat3(&eyePoint) - VectorPosition();
        float distToPositionSquared = XMVectorGetX(XMVector3LengthSq(cameraToPosition));

        float volume = min(distToPositionSquared, 1.0f);
        // Scale
        // Sound is proportional to how hard the ball is hitting.
        volume = adjustment * volume;

        m_hitSound->PlaySound(volume);
    }
}
```

Animate.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Animate:
// This is an abstract class for animations.  It defines a set of
// capabilities for animating XMFLOAT3 variables.  An animation has the following
// characteristics:
//     Start - the time for the animation to start.
//     Duration - the length of time the animation is to run.
//     Continuous - whether the animation loops after duration is reached or just
//         stops.
// There are two query functions:
//     IsActive - determines if the animation is active at time t.
//     IsFinished - determines if the animation is done at time t.
// It is expected that each derived class will provide an Evaluate method for the
// specific kind of animation.

ref class Animate abstract
{
internal:
    Animate();

    virtual DirectX::XMFLOAT3 Evaluate (_In_ float t) = 0;

    bool  IsActive(_In_ float t) { return ((t >= m_startTime) && (m_continuous || (t < (m_startTime + m_duration)))); };
    bool  IsFinished(_In_ float t) { return (!m_continuous && (t >= (m_startTime + m_duration))); }
    float Start();
    void  Start(_In_ float start);
    float Duration();
    void  Duration(_In_ float duration);
    bool  Continuous();
    void  Continuous(_In_ bool continuous);

protected private:
    bool  m_continuous;      // if true means at end cycle back to beginning
    float m_startTime;       // time at which animation begins
    float m_duration;        // for continuous, this is the duration of 1 cycle through path
};

//----------------------------------------------------------------------

// AnimateLinePosition:
// This class is a specialization of Animate that defines an animation of a position vector
// along a straight line defined in world coordinates from startPosition to endPosition.

ref class AnimateLinePosition: public Animate
{
internal:
    AnimateLinePosition(
        _In_ DirectX::XMFLOAT3 startPosition,
        _In_ DirectX::XMFLOAT3 endPosition,
        _In_ float duration,
        _In_ bool continuous
        );
    virtual DirectX::XMFLOAT3 Evaluate(_In_ float t) override;

private:
    DirectX::XMFLOAT3 m_startPosition;
    DirectX::XMFLOAT3 m_endPosition;
    float             m_length;
};

//----------------------------------------------------------------------

struct LineSegment
{
    DirectX::XMFLOAT3 position;
    float             length;
    float             uStart;
    float             uLength;
};

// AnimateLineListPosition:
// This class is a specialization of Animate that defines an animation of a position vector
// along a set of line segments defined by a set of points.  The animation along the path is
// such that the evaluation of the position along the path will be uniform independent of
// the length of each of the line segments.  A continuous loop can be achieved by having the
// first and last points of the list be the same.

ref class AnimateLineListPosition: public Animate
{
internal:
    AnimateLineListPosition(
        _In_ unsigned int count,
        _In_reads_(count) DirectX::XMFLOAT3 position[],
        _In_ float duration,
        _In_ bool continuous
        );
    virtual DirectX::XMFLOAT3 Evaluate(_In_ float t) override;

private:
    unsigned int             m_count;
    float                    m_totalLength;
    std::vector<LineSegment> m_segment;
};

//----------------------------------------------------------------------

// AnimateCirclePosition:
// This class is a specialization of Animate that defines an animation of a position vector
// along a circular path centered at 'center' with a starting point of 'startPosition'.  The
// distance between 'center' and 'startPosition' defines the radius of the circle.  The plane
// of the circle will pass through 'center' and 'startPosition' and have a normal of 'planeNormal'.
// The direction of the animation can be either clockwise or counterclockwise based
// on the 'clockwise' parameter.

ref class AnimateCirclePosition: public Animate
{
internal:
    AnimateCirclePosition(
        _In_ DirectX::XMFLOAT3 center,
        _In_ DirectX::XMFLOAT3 startPosition,
        _In_ DirectX::XMFLOAT3 planeNormal,
        _In_ float duration,
        _In_ bool continuous,
        _In_ bool clockwise
        );
    virtual DirectX::XMFLOAT3 Evaluate(_In_ float t) override;

private:
    DirectX::XMFLOAT4X4 m_rotationMatrix;
    DirectX::XMFLOAT3   m_center;
    DirectX::XMFLOAT3   m_planeNormal;
    DirectX::XMFLOAT3   m_startPosition;
    float               m_radius;
    bool                m_clockwise;
};

//----------------------------------------------------------------------
            
            
```

Animate.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Animate.h"

using namespace DirectX;

//----------------------------------------------------------------------

Animate::Animate():
    m_continuous(false),
    m_startTime(0.0f),
    m_duration(10.0f)
{
}

//----------------------------------------------------------------------

float Animate::Start()
{
    return m_startTime;
}

//----------------------------------------------------------------------

void Animate::Start(_In_ float start)
{
    m_startTime = start;
}

//----------------------------------------------------------------------

float Animate::Duration()
{
    return m_duration;
}

//----------------------------------------------------------------------

void Animate::Duration(_In_ float duration)
{
    m_duration = duration;
}

//----------------------------------------------------------------------

bool Animate::Continuous()
{
    return m_continuous;
}

//----------------------------------------------------------------------

void Animate::Continuous(_In_ bool continuous)
{
    m_continuous = continuous;
}

//----------------------------------------------------------------------

AnimateLinePosition::AnimateLinePosition(
    _In_ XMFLOAT3 startPosition,
    _In_ XMFLOAT3 endPosition,
    _In_ float duration,
    _In_ bool continuous)
{
    m_startPosition = startPosition;
    m_endPosition = endPosition;
    m_duration = duration;
    m_continuous = continuous;

    m_length = XMVectorGetX(
        XMVector3Length(XMLoadFloat3(&endPosition) - XMLoadFloat3(&startPosition))
        );
}

//----------------------------------------------------------------------

XMFLOAT3 AnimateLinePosition::Evaluate(_In_ float t)
{
    if (t <= m_startTime)
    {
        return m_startPosition;
    }

    if ((t >= (m_startTime + m_duration)) && !m_continuous)
    {
        return m_endPosition;
    }

    float startTime = m_startTime;
    if (m_continuous)
    {
        // For continuous operation, move the start time forward to
        // eliminate previous iterations.
        startTime += ((int)((t - m_startTime) / m_duration)) * m_duration;
    }

    float u = (t - startTime) / m_duration;
    XMFLOAT3 currentPosition;
    currentPosition.x = m_startPosition.x + (m_endPosition.x - m_startPosition.x)*u;
    currentPosition.y = m_startPosition.y + (m_endPosition.y - m_startPosition.y)*u;
    currentPosition.z = m_startPosition.z + (m_endPosition.z - m_startPosition.z)*u;

    return currentPosition;
}

//----------------------------------------------------------------------

AnimateLineListPosition::AnimateLineListPosition(
    _In_ unsigned int count,
    _In_reads_(count) XMFLOAT3 position[],
    _In_ float duration,
    _In_ bool continuous)
{
    m_duration = duration;
    m_continuous = continuous;
    m_count = count;

    std::vector<LineSegment> segment(m_count);
    m_segment = segment;
    m_totalLength = 0.0f;

    m_segment[0].position = position[0];
    for (unsigned int i = 1; i < count; i++)
    {
        m_segment[i].position = position[i];
        m_segment[i - 1].length = XMVectorGetX(
            XMVector3Length(
                XMLoadFloat3(&m_segment[i].position) -
                XMLoadFloat3(&m_segment[i - 1].position)
                )
            );
        m_totalLength += m_segment[i - 1].length;
    }

    // Parameterize the segments to ensure uniform evaluation along the path.
    float u = 0.0f;
    for (unsigned int i = 0; i < (count - 1); i++)
    {
        m_segment[i].uStart = u;
        m_segment[i].uLength = (m_segment[i].length / m_totalLength);
        u += m_segment[i].uLength;
    }
    m_segment[count-1].uStart = 1.0f;
}

//----------------------------------------------------------------------

XMFLOAT3 AnimateLineListPosition::Evaluate(_In_ float t)
{
    if (t <= m_startTime)
    {
        return m_segment[0].position;
    }

    if ((t >= (m_startTime + m_duration)) && !m_continuous)
    {
        return m_segment[m_count-1].position;
    }

    float startTime = m_startTime;
    if (m_continuous)
    {
        // For continuous operation, move the start time forward to
        // eliminate previous iterations.
        startTime += ((int)((t - m_startTime) / m_duration)) * m_duration;
    }

    float u = (t - startTime) / m_duration;
    // Find the right segment.
    unsigned int i = 0;
    while (u > m_segment[i + 1].uStart)
    {
        i++;
    }

    u -= m_segment[i].uStart;
    u /= m_segment[i].uLength;

    XMFLOAT3 currentPosition;
    currentPosition.x = m_segment[i].position.x + (m_segment[i + 1].position.x - m_segment[i].position.x)*u;
    currentPosition.y = m_segment[i].position.y + (m_segment[i + 1].position.y - m_segment[i].position.y)*u;
    currentPosition.z = m_segment[i].position.z + (m_segment[i + 1].position.z - m_segment[i].position.z)*u;

    return currentPosition;
}

//----------------------------------------------------------------------

AnimateCirclePosition:: AnimateCirclePosition(
    _In_ XMFLOAT3 center,
    _In_ XMFLOAT3 startPosition,
    _In_ XMFLOAT3 planeNormal,
    _In_ float duration,
    _In_ bool continuous,
    _In_ bool clockwise)
{
    m_center = center;
    m_planeNormal = planeNormal;
    m_startPosition = startPosition;
    m_duration = duration;
    m_continuous = continuous;
    m_clockwise = clockwise;

    XMVECTOR coordX = XMLoadFloat3(&m_startPosition) - XMLoadFloat3(&m_center);
    m_radius = XMVectorGetX(XMVector3Length(coordX));

    XMVector3Normalize(coordX);

    XMVECTOR coordZ = XMLoadFloat3(&m_planeNormal);
    XMVector3Normalize(coordZ);

    XMVECTOR coordY;
    if (m_clockwise)
    {
        coordY = XMVector3Cross(coordZ, coordX);
    }
    else
    {
        coordY = XMVector3Cross(coordX, coordZ);
    }

    XMVECTOR vectorX = XMVectorSet(1.0f, 0.0f, 0.0f, 1.0f);
    XMVECTOR vectorY = XMVectorSet(0.0f, 1.0f, 0.0f, 1.0f);
    XMMATRIX mat1 = XMMatrixIdentity();
    XMMATRIX mat2 = XMMatrixIdentity();

    if (!XMVector3Equal(coordX, vectorX))
    {
        float angle;
        angle = XMVectorGetX(
            XMVector3AngleBetweenVectors(vectorX, coordX)
            );
        if ((angle * angle) > 0.025)
        {
            XMVECTOR axis1 = XMVector3Cross(vectorX, coordX);

            mat1 = XMMatrixRotationAxis(axis1, angle);
            vectorY = XMVector3TransformCoord(vectorY, mat1);
        }
    }
    if (!XMVector3Equal(vectorY, coordY))
    {
        float angle;
        angle = XMVectorGetX(
            XMVector3AngleBetweenVectors(vectorY, coordY)
            );
        if ((angle * angle) > 0.025)
        {
            XMVECTOR axis2 = XMVector3Cross(vectorY, coordY);
            mat2 = XMMatrixRotationAxis(axis2, angle);
        }
    }
    XMStoreFloat4x4(
        &m_rotationMatrix,
        mat1 *
        mat2 *
        XMMatrixTranslation(m_center.x, m_center.y, m_center.z)
        );
}

//----------------------------------------------------------------------

XMFLOAT3 AnimateCirclePosition::Evaluate(_In_ float t)
{
    if (t <= m_startTime)
    {
        return m_startPosition;
    }

    if ((t >= (m_startTime + m_duration)) && !m_continuous)
    {
        return m_startPosition;
    }

    float startTime = m_startTime;
    if (m_continuous)
    {
        // For continuous operation, move the start time forward to
        // eliminate previous iterations.
        startTime += ((int)((t - m_startTime) / m_duration)) * m_duration;
    }

    float u = (t - startTime) / m_duration * XM_2PI;

    XMFLOAT3 currentPosition;
    currentPosition.x = m_radius * cos(u);
    currentPosition.y = m_radius * sin(u);
    currentPosition.z = 0.0f;

    XMStoreFloat3(
        &currentPosition,
        XMVector3TransformCoord(
            XMLoadFloat3(&currentPosition),
            XMLoadFloat4x4(&m_rotationMatrix)
            )
        );

    return currentPosition;
}

//----------------------------------------------------------------------
            
            
```

BasicLoader.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "BasicReaderWriter.h"

// A simple loader class that provides support for loading shaders, textures,
// and meshes from files on disk. Provides synchronous and asynchronous methods.
ref class BasicLoader
{
internal:
    BasicLoader(
        _In_ ID3D11Device* d3dDevice,
        _In_opt_ IWICImagingFactory2* wicFactory = nullptr
        );

    void LoadTexture(
        _In_ Platform::String^ filename,
        _Out_opt_ ID3D11Texture2D** texture,
        _Out_opt_ ID3D11ShaderResourceView** textureView
        );

    concurrency::task<void> LoadTextureAsync(
        _In_ Platform::String^ filename,
        _Out_opt_ ID3D11Texture2D** texture,
        _Out_opt_ ID3D11ShaderResourceView** textureView
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11PixelShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11PixelShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11ComputeShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11ComputeShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11GeometryShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11GeometryShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
        _In_ uint32 numEntries,
        _In_reads_opt_(numStrides) const uint32* bufferStrides,
        _In_ uint32 numStrides,
        _In_ uint32 rasterizedStream,
        _Out_ ID3D11GeometryShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
        _In_ uint32 numEntries,
        _In_reads_opt_(numStrides) const uint32* bufferStrides,
        _In_ uint32 numStrides,
        _In_ uint32 rasterizedStream,
        _Out_ ID3D11GeometryShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11HullShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11HullShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11DomainShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11DomainShader** shader
        );

    void LoadMesh(
        _In_ Platform::String^ filename,
        _Out_ ID3D11Buffer** vertexBuffer,
        _Out_ ID3D11Buffer** indexBuffer,
        _Out_opt_ uint32* vertexCount,
        _Out_opt_ uint32* indexCount
        );

    concurrency::task<void> LoadMeshAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11Buffer** vertexBuffer,
        _Out_ ID3D11Buffer** indexBuffer,
        _Out_opt_ uint32* vertexCount,
        _Out_opt_ uint32* indexCount
        );

private:
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<IWICImagingFactory2> m_wicFactory;
    BasicReaderWriter^ m_basicReaderWriter;

    template <class DeviceChildType>
    inline void SetDebugName(
        _In_ DeviceChildType* object,
        _In_ Platform::String^ name
        );

    Platform::String^ GetExtension(
        _In_ Platform::String^ filename
        );

    void CreateTexture(
        _In_ bool decodeAsDDS,
        _In_reads_bytes_(dataSize) byte* data,
        _In_ uint32 dataSize,
        _Out_opt_ ID3D11Texture2D** texture,
        _Out_opt_ ID3D11ShaderResourceView** textureView,
        _In_opt_ Platform::String^ debugName
        );

    void CreateInputLayout(
        _In_reads_bytes_(bytecodeSize) byte* bytecode,
        _In_ uint32 bytecodeSize,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11InputLayout** layout
        );

    void CreateMesh(
        _In_ byte* meshData,
        _Out_ ID3D11Buffer** vertexBuffer,
        _Out_ ID3D11Buffer** indexBuffer,
        _Out_opt_ uint32* vertexCount,
        _Out_opt_ uint32* indexCount,
        _In_opt_ Platform::String^ debugName
        );
};
```

BasicLoader.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "BasicLoader.h"
#include "BasicShapes.h"
#include "DDSTextureLoader.h"
#include "DirectXSample.h"
#include <memory>

using namespace Microsoft::WRL;
using namespace Windows::Storage;
using namespace Windows::Storage::Streams;
using namespace Windows::Foundation;
using namespace Windows::ApplicationModel;
using namespace std;
using namespace concurrency;

BasicLoader::BasicLoader(
    _In_ ID3D11Device* d3dDevice,
    _In_opt_ IWICImagingFactory2* wicFactory
    ) :
    m_d3dDevice(d3dDevice),
    m_wicFactory(wicFactory)
{
    // Create a new BasicReaderWriter to do raw file I/O.
    m_basicReaderWriter = ref new BasicReaderWriter();
}

template <class DeviceChildType>
inline void BasicLoader::SetDebugName(
    _In_ DeviceChildType* object,
    _In_ Platform::String^ name
    )
{
#if defined(_DEBUG)
    // Only assign debug names in debug builds.

    char nameString[1024];
    int nameStringLength = WideCharToMultiByte(
        CP_ACP,
        0,
        name->Data(),
        -1,
        nameString,
        1024,
        nullptr,
        nullptr
        );

    if (nameStringLength == 0)
    {
        char defaultNameString[] = "BasicLoaderObject";
        DX::ThrowIfFailed(
            object->SetPrivateData(
                WKPDID_D3DDebugObjectName,
                sizeof(defaultNameString) - 1,
                defaultNameString
                )
            );
    }
    else
    {
        DX::ThrowIfFailed(
            object->SetPrivateData(
                WKPDID_D3DDebugObjectName,
                nameStringLength - 1,
                nameString
                )
            );
    }
#endif
}

Platform::String^ BasicLoader::GetExtension(
    _In_ Platform::String^ filename
    )
{
    int lastDotIndex = -1;
    for (int i = filename->Length() - 1; i >= 0 && lastDotIndex == -1; i--)
    {
        if (*(filename->Data() + i) == '.')
        {
            lastDotIndex = i;
        }
    }
    if (lastDotIndex != -1)
    {
        std::unique_ptr<wchar_t[]> extension(new wchar_t[filename->Length() - lastDotIndex]);
        for (unsigned int i = 0; i < filename->Length() - lastDotIndex; i++)
        {
            extension[i] = tolower(*(filename->Data() + lastDotIndex + 1 + i));
        }
        return ref new Platform::String(extension.get());
    }
    return "";
}

void BasicLoader::CreateTexture(
    _In_ bool decodeAsDDS,
    _In_reads_bytes_(dataSize) byte* data,
    _In_ uint32 dataSize,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView,
    _In_opt_ Platform::String^ debugName
    )
{
    ComPtr<ID3D11ShaderResourceView> shaderResourceView;
    ComPtr<ID3D11Texture2D> texture2D;

    if (decodeAsDDS)
    {
        ComPtr<ID3D11Resource> resource;

        if (textureView == nullptr)
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                nullptr
                );
        }
        else
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                &shaderResourceView
                );
        }

        DX::ThrowIfFailed(
            resource.As(&texture2D)
            );
    }
    else
    {
        if (m_wicFactory.Get() == nullptr)
        {
            // A WIC factory object is required in order to load texture
            // assets stored in non-DDS formats.  If BasicLoader was not
            // initialized with one, create one as needed.
            DX::ThrowIfFailed(
                CoCreateInstance(
                    CLSID_WICImagingFactory,
                    nullptr,
                    CLSCTX_INPROC_SERVER,
                    IID_PPV_ARGS(&m_wicFactory)
                    )
                );
        }

        ComPtr<IWICStream> stream;
        DX::ThrowIfFailed(
            m_wicFactory->CreateStream(&stream)
            );

        DX::ThrowIfFailed(
            stream->InitializeFromMemory(
                data,
                dataSize
                )
            );

        ComPtr<IWICBitmapDecoder> bitmapDecoder;
        DX::ThrowIfFailed(
            m_wicFactory->CreateDecoderFromStream(
                stream.Get(),
                nullptr,
                WICDecodeMetadataCacheOnDemand,
                &bitmapDecoder
                )
            );

        ComPtr<IWICBitmapFrameDecode> bitmapFrame;
        DX::ThrowIfFailed(
            bitmapDecoder->GetFrame(0, &bitmapFrame)
            );

        ComPtr<IWICFormatConverter> formatConverter;
        DX::ThrowIfFailed(
            m_wicFactory->CreateFormatConverter(&formatConverter)
            );

        DX::ThrowIfFailed(
            formatConverter->Initialize(
                bitmapFrame.Get(),
                GUID_WICPixelFormat32bppPBGRA,
                WICBitmapDitherTypeNone,
                nullptr,
                0.0,
                WICBitmapPaletteTypeCustom
                )
            );

        uint32 width;
        uint32 height;
        DX::ThrowIfFailed(
            bitmapFrame->GetSize(&width, &height)
            );

        std::unique_ptr<byte[]> bitmapPixels(new byte[width * height * 4]);
        DX::ThrowIfFailed(
            formatConverter->CopyPixels(
                nullptr,
                width * 4,
                width * height * 4,
                bitmapPixels.get()
                )
            );

        D3D11_SUBRESOURCE_DATA initialData;
        ZeroMemory(&initialData, sizeof(initialData));
        initialData.pSysMem = bitmapPixels.get();
        initialData.SysMemPitch = width * 4;
        initialData.SysMemSlicePitch = 0;

        CD3D11_TEXTURE2D_DESC textureDesc(
            DXGI_FORMAT_B8G8R8A8_UNORM,
            width,
            height,
            1,
            1
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateTexture2D(
                &textureDesc,
                &initialData,
                &texture2D
                )
            );

        if (textureView != nullptr)
        {
            CD3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc(
                texture2D.Get(),
                D3D11_SRV_DIMENSION_TEXTURE2D
                );

            DX::ThrowIfFailed(
                m_d3dDevice->CreateShaderResourceView(
                    texture2D.Get(),
                    &shaderResourceViewDesc,
                    &shaderResourceView
                    )
                );
        }
    }

    SetDebugName(texture2D.Get(), debugName);

    if (texture != nullptr)
    {
        *texture = texture2D.Detach();
    }
    if (textureView != nullptr)
    {
        *textureView = shaderResourceView.Detach();
    }
}

void BasicLoader::CreateInputLayout(
    _In_reads_bytes_(bytecodeSize) byte* bytecode,
    _In_ uint32 bytecodeSize,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11InputLayout** layout
    )
{
    if (layoutDesc == nullptr)
    {
        // If no input layout is specified, use the BasicVertex layout.
        const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
        {
            { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        };

        DX::ThrowIfFailed(
            m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                bytecode,
                bytecodeSize,
                layout
                )
            );
    }
    else
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateInputLayout(
                layoutDesc,
                layoutDescNumElements,
                bytecode,
                bytecodeSize,
                layout
                )
            );
    }
}

void BasicLoader::CreateMesh(
    _In_ byte* meshData,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount,
    _In_opt_ Platform::String^ debugName
    )
{
    // The first 4 bytes of the BasicMesh format define the number of vertices in the mesh.
    uint32 numVertices = *reinterpret_cast<uint32*>(meshData);

    // The following 4 bytes define the number of indices in the mesh.
    uint32 numIndices = *reinterpret_cast<uint32*>(meshData + sizeof(uint32));

    // The next segment of the BasicMesh format contains the vertices of the mesh.
    BasicVertex* vertices = reinterpret_cast<BasicVertex*>(meshData + sizeof(uint32) * 2);

    // The last segment of the BasicMesh format contains the indices of the mesh.
    uint16* indices = reinterpret_cast<uint16*>(meshData + sizeof(uint32) * 2 + sizeof(BasicVertex) * numVertices);

    // Create the vertex and index buffers with the mesh data.

    D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
    vertexBufferData.pSysMem = vertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC vertexBufferDesc(numVertices * sizeof(BasicVertex), D3D11_BIND_VERTEX_BUFFER);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            vertexBuffer
            )
        );

    D3D11_SUBRESOURCE_DATA indexBufferData = {0};
    indexBufferData.pSysMem = indices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC indexBufferDesc(numIndices * sizeof(uint16), D3D11_BIND_INDEX_BUFFER);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            indexBuffer
            )
        );

    SetDebugName(*vertexBuffer, Platform::String::Concat(debugName, "_VertexBuffer"));
    SetDebugName(*indexBuffer, Platform::String::Concat(debugName, "_IndexBuffer"));

    if (vertexCount != nullptr)
    {
        *vertexCount = numVertices;
    }
    if (indexCount != nullptr)
    {
        *indexCount = numIndices;
    }
}

void BasicLoader::LoadTexture(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    Platform::Array<byte>^ textureData = m_basicReaderWriter->ReadData(filename);

    CreateTexture(
        GetExtension(filename) == "dds",
        textureData->Data,
        textureData->Length,
        texture,
        textureView,
        filename
        );
}

task<void> BasicLoader::LoadTextureAsync(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(
            GetExtension(filename) == "dds",
            textureData->Data,
            textureData->Length,
            texture,
            textureView,
            filename
            );
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateVertexShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);

    if (layout != nullptr)
    {
        CreateInputLayout(
            bytecode->Data,
            bytecode->Length,
            layoutDesc,
            layoutDescNumElements,
            layout
            );

        SetDebugName(*layout, filename);
    }
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the layoutDesc array as well as the SemanticName strings,
    // both of which are pointers to data whose lifetimes may be shorter than that
    // of this method's task.
    shared_ptr<vector<D3D11_INPUT_ELEMENT_DESC>> layoutDescCopy;
    shared_ptr<vector<string>> layoutDescSemanticNamesCopy;
    if (layoutDesc != nullptr)
    {
        layoutDescCopy.reset(
            new vector<D3D11_INPUT_ELEMENT_DESC>(
                layoutDesc,
                layoutDesc + layoutDescNumElements
                )
            );

        layoutDescSemanticNamesCopy.reset(
            new vector<string>(layoutDescNumElements)
            );

        for (uint32 i = 0; i < layoutDescNumElements; i++)
        {
            layoutDescSemanticNamesCopy->at(i).assign(layoutDesc[i].SemanticName);
        }
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateVertexShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);

        if (layout != nullptr)
        {
            if (layoutDesc != nullptr)
            {
                // Reassign the SemanticName elements of the layoutDesc array copy to point
                // to the corresponding copied strings. Performing the assignment inside the
                // lambda body ensures that the lambda will take a reference to the shared_ptr
                // that holds the data.  This will guarantee that the data is still valid when
                // CreateInputLayout is called.
                for (uint32 i = 0; i < layoutDescNumElements; i++)
                {
                    layoutDescCopy->at(i).SemanticName = layoutDescSemanticNamesCopy->at(i).c_str();
                }
            }

            CreateInputLayout(
                bytecode->Data,
                bytecode->Length,
                layoutDesc == nullptr ? nullptr : layoutDescCopy->data(),
                layoutDescNumElements,
                layout
                );

            SetDebugName(*layout, filename);
        }
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreatePixelShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreatePixelShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11ComputeShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateComputeShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11ComputeShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateComputeShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11GeometryShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateGeometryShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11GeometryShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateGeometryShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
    _In_ uint32 numEntries,
    _In_reads_opt_(numStrides) const uint32* bufferStrides,
    _In_ uint32 numStrides,
    _In_ uint32 rasterizedStream,
    _Out_ ID3D11GeometryShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateGeometryShaderWithStreamOutput(
            bytecode->Data,
            bytecode->Length,
            streamOutDeclaration,
            numEntries,
            bufferStrides,
            numStrides,
            rasterizedStream,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
    _In_ uint32 numEntries,
    _In_reads_opt_(numStrides) const uint32* bufferStrides,
    _In_ uint32 numStrides,
    _In_ uint32 rasterizedStream,
    _Out_ ID3D11GeometryShader** shader
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the streamOutDeclaration array as well as the SemanticName
    // strings, both of which are pointers to data whose lifetimes may be shorter
    // than that of this method's task.
    shared_ptr<vector<D3D11_SO_DECLARATION_ENTRY>> streamOutDeclarationCopy;
    shared_ptr<vector<string>> streamOutDeclarationSemanticNamesCopy;
    if (streamOutDeclaration != nullptr)
    {
        streamOutDeclarationCopy.reset(
            new vector<D3D11_SO_DECLARATION_ENTRY>(
                streamOutDeclaration,
                streamOutDeclaration + numEntries
                )
            );

        streamOutDeclarationSemanticNamesCopy.reset(
            new vector<string>(numEntries)
            );

        for (uint32 i = 0; i < numEntries; i++)
        {
            streamOutDeclarationSemanticNamesCopy->at(i).assign(streamOutDeclaration[i].SemanticName);
        }
    }
    
    // Create a copy of the bufferStrides array, which is a pointer to data
    // whose lifetime may be shorter than that of this method's task.
    shared_ptr<vector<uint32>> bufferStridesCopy;
    if (bufferStrides != nullptr)
    {
        bufferStridesCopy.reset(
            new vector<uint32>(
                bufferStrides,
                bufferStrides + numStrides
                )
            );
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        if (streamOutDeclaration != nullptr)
        {
            // Reassign the SemanticName elements of the streamOutDeclaration array copy to
            // point to the corresponding copied strings. Performing the assignment inside the
            // lambda body ensures that the lambda will take a reference to the shared_ptr
            // that holds the data.  This will guarantee that the data is still valid when
            // CreateGeometryShaderWithStreamOutput is called.
            for (uint32 i = 0; i < numEntries; i++)
            {
                streamOutDeclarationCopy->at(i).SemanticName = streamOutDeclarationSemanticNamesCopy->at(i).c_str();
            }
        }

        DX::ThrowIfFailed(
            m_d3dDevice->CreateGeometryShaderWithStreamOutput(
                bytecode->Data,
                bytecode->Length,
                streamOutDeclaration == nullptr ? nullptr : streamOutDeclarationCopy->data(),
                numEntries,
                bufferStrides == nullptr ? nullptr : bufferStridesCopy->data(),
                numStrides,
                rasterizedStream,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11HullShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateHullShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11HullShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateHullShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11DomainShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateDomainShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11DomainShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateDomainShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    Platform::Array<byte>^ meshData = m_basicReaderWriter->ReadData(filename);

    CreateMesh(
        meshData->Data,
        vertexBuffer,
        indexBuffer,
        vertexCount,
        indexCount,
        filename
        );
}

task<void> BasicLoader::LoadMeshAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ meshData)
    {
        CreateMesh(
            meshData->Data,
            vertexBuffer,
            indexBuffer,
            vertexCount,
            indexCount,
            filename
            );
    });
}
```

MeshObject.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// MeshObject:
// This class is the generic (abstract) representation of D3D11 Indexed triangle
// list.  Each of the derived classes is just the constructor for the specific
// geometry primitive.  This abstract class does not place any requirements on
// the format of the geometry directly.
// The primary method of the MeshObject is Render.  The default implementation
// just sets the IndexBuffer, VertexBuffer, and topology to a TriangleList and
// makes a  DrawIndexed call on the context.  It assumes all other state has
// already been set on the context.

ref class MeshObject abstract
{
internal:
    MeshObject();

    virtual void Render(_In_ ID3D11DeviceContext *context);

protected private:
    Microsoft::WRL::ComPtr<ID3D11Buffer>  m_vertexBuffer;
    Microsoft::WRL::ComPtr<ID3D11Buffer>  m_indexBuffer;
    int                                   m_vertexCount;
    int                                   m_indexCount;
};
            
            
```

MeshObject.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "MeshObject.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

MeshObject::MeshObject():
    m_vertexCount(0),
    m_indexCount(0)
{
}

//--------------------------------------------------------------------------------

void MeshObject::Render(_In_ ID3D11DeviceContext *context)
{
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->DrawIndexed(m_indexCount, 0, 0);
}

//--------------------------------------------------------------------------------
            
            
```

SphereMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// SphereMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent a canonical sphere that is
// positioned at the origin with a radius of 1.0.

#include "MeshObject.h"

ref class SphereMesh: public MeshObject
{
internal:
    SphereMesh(_In_ ID3D11Device *device, uint32 segments);
};
            
            
```

SphereMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "SphereMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

SphereMesh::SphereMesh(_In_ ID3D11Device *device, uint32 segments)
{
    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    uint32 slices = segments / 2;
    uint32 numVertices = (slices + 1) * (segments + 1) + 1;
    uint32 numIndices = slices * segments * 3 * 2;

    std::vector<PNTVertex> point(numVertices);
    std::vector<uint16> index(numIndices);

    // To make the texture look right on the top and bottom of the sphere,
    // each slice will have 'segments + 1' vertices.  The top and bottom
    // vertices will all be coincident, but have different U texture cooordinates.
    uint32 p = 0;
    for (uint32 a = 0; a <= slices; a++)
    {
        float angle1 = static_cast<float>(a) / static_cast<float>(slices) * XM_PI;
        float z = cos(angle1);
        float r = sin(angle1);
        for (uint32 b = 0; b <= segments; b++)
        {
            float angle2 = static_cast<float>(b) / static_cast<float>(segments) * XM_2PI;
            point[p].position = XMFLOAT3(r * cos(angle2), r * sin(angle2), z);
            point[p].normal = point[p].position;
            point[p].textureCoordinate = XMFLOAT2((1.0f-z) / 2.0f, static_cast<float>(b) / static_cast<float>(segments));
            p++;
        }
    }
    m_vertexCount = p;

    p = 0;
    for (uint16 a = 0; a < slices; a++)
    {
        uint16 p1 = a * (segments + 1);
        uint16 p2 = (a + 1) * (segments + 1);

        // Generate two triangles for each segment around the slice.
        for (uint16 b = 0; b < segments; b++)
        {
            if (a < (slices - 1))
            {
                // For all but the bottom slice, add the triangle with one
                // vertex in the a slice and two vertices in the a + 1 slice.
                // Skip it for the bottom slice since the triangle would be
                // degenerate as all the vertices in the bottom slice are coincident.
                index[p] = b + p1;
                index[p+1] = b + p2;
                index[p+2] = b + p2 + 1;
                p = p + 3;
            }
            if (a > 0)
            {
                // For all but the top slice, add the triangle with two
                // vertices in the a slice and one vertex in the a + 1 slice.
                // Skip it for the top slice since the triangle would be
                // degenerate as all the vertices in the top slice are coincident.
                index[p] = b + p1;
                index[p+1] = b + p2 + 1;
                index[p+2] = b + p1 + 1;
                p = p + 3;
            }
        }
    }
    m_indexCount = p;

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = point.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(uint16) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = index.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}
            
            
```

CylinderMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// CylinderMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent a canonical cylinder (capped at
// both ends) that is positioned at the origin with a radius of 1.0,
// a height of 1.0 and with its axis in the +Z direction.

#include "MeshObject.h"

ref class CylinderMesh: public MeshObject
{
internal:
    CylinderMesh(_In_ ID3D11Device *device, uint32 segments);
};
            
            
```

CylinderMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "CylinderMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

CylinderMesh::CylinderMesh(_In_ ID3D11Device *device, uint32 segments)
{
    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    uint32 numVertices = 6 * (segments + 1) + 1;
    uint32 numIndices = 3 * segments * 3 * 2;

    std::vector<PNTVertex> point(numVertices);
    std::vector<uint16> index(numIndices);

    uint32 p = 0;
    // Top center point (multiple points for texture coordinates).
    for (uint32 a = 0; a <= segments; a++)
    {
        point[p].position = XMFLOAT3(0.0f, 0.0f, 1.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, 1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 0.0f);
        p++;
    }
    // Top edge of cylinder: Normals point up for lighting of top surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 1.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, 1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 0.0f);
        p++;
    }
    // Top edge of cylinder: Normals point out for lighting of the side surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 1.0f);
        point[p].normal = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 0.0f);
        p++;
    }
    // Bottom edge of cylinder: Normals point out for lighting of the side surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].normal = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 1.0f);
        p++;
    }
    // Bottom edge of cylinder: Normals point down for lighting of the bottom surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, -1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 1.0f);
        p++;
    }
    // Bottom center of cylinder: Normals point down for lighting on the bottom surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        point[p].position = XMFLOAT3(0.0f, 0.0f, 0.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, -1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 1.0f);
        p++;
    }
    m_vertexCount = p;

    p = 0;
    for (uint16 a = 0; a < 6; a += 2)
    {
        uint16 p1 = a*(segments + 1);
        uint16 p2 = (a+1)*(segments + 1);
        for (uint16 b = 0; b < segments; b++)
        {
            if (a < 4)
            {
                index[p] = b + p1;
                index[p+1] = b + p2;
                index[p+2] = b + p2 + 1;
                p = p + 3;
            }
            if (a > 0)
            {
                index[p] = b + p1;
                index[p+1] = b + p2 + 1;
                index[p+2] = b + p1 + 1;
                p = p + 3;
            }
        }
    }
    m_indexCount = p;

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = point.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(uint16) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = index.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}
            
            
```

FaceMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// FaceMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent a canonical face defined as a
// rectangle at the origin extending 1 unit in the +X and
// 1 unit in the +Y direction.
// The face is defined to be two sided, so it is visible from either
// side.

#include "MeshObject.h"

ref class FaceMesh: public MeshObject
{
internal:
    FaceMesh(_In_ ID3D11Device *device);
};
            
            
```

FaceMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "FaceMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

FaceMesh::FaceMesh(_In_ ID3D11Device *device)
{
    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    PNTVertex target_vertices[] =
    {
        {XMFLOAT3(0.0f, 0.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(1.0f, 1.0f)},
        {XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 1.0f)},
        {XMFLOAT3(1.0f, 1.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(1.0f, 0.0f)}
    };
    WORD target_indices[] =
    {
        0, 1, 2,
        0, 2, 3,
        0, 2, 1,
        0, 3, 2
    };

    m_vertexCount = 4;
    m_indexCount = 12;

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = target_vertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = target_indices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}
            
            
```

WorldMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "MeshObject.h"

// WorldCeilingMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent the ceiling of the bounding cube
// of the world.
// The vertices are defined by a position, a normal and a single
// 2D texture coordinate.

ref class WorldCeilingMesh: public MeshObject
{
internal:
    WorldCeilingMesh(_In_ ID3D11Device *device);
};

// WorldFloorMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent the floor of the bounding cube
// of the world.

ref class WorldFloorMesh: public MeshObject
{
internal:
    WorldFloorMesh(_In_ ID3D11Device *device);
};

// WorldWallsMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent the walls of the bounding cube
// of the world.

ref class WorldWallsMesh: public MeshObject
{
internal:
    WorldWallsMesh(_In_ ID3D11Device *device);
};
            
            
```

WorldMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "WorldMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

WorldCeilingMesh::WorldCeilingMesh(_In_ ID3D11Device *device)
{
    PNTVertex cellVertices[] =
    {
        // CEILING
        {XMFLOAT3(-4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2(-0.15f, 0.0f)},
        {XMFLOAT3( 4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2( 1.25f, 0.0f)},
        {XMFLOAT3(-4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2(-0.15f, 2.1f)},
        {XMFLOAT3( 4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2( 1.25f, 2.1f)},
    };

    WORD cellIndices[] = {
        0, 1, 2,
        1, 3, 2,
    };

    m_vertexCount = 4;
    m_indexCount = 6;

    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellVertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellIndices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}

WorldFloorMesh::WorldFloorMesh(_In_ ID3D11Device *device)
{
    PNTVertex cellVertices[] =
    {
        // FLOOR
        {XMFLOAT3(-4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3( 4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(1.0f, 0.0f)},
        {XMFLOAT3(-4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3( 4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(1.0f, 1.5f)},
    };

    WORD cellIndices[] = {
        0, 1, 2,
        1, 3, 2,
    };

    m_vertexCount = 4;
    m_indexCount = 6;

    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellVertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellIndices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}

WorldWallsMesh::WorldWallsMesh(_In_ ID3D11Device *device)
{
    PNTVertex cellVertices[] =
    {
        // WALL
        {XMFLOAT3(-4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3( 4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(2.0f, 0.0f)},
        {XMFLOAT3(-4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3( 4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(2.0f, 1.5f)},
        // WALL
        {XMFLOAT3(4.0f,  3.0f,  6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(4.0f,  3.0f, -6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 0.0f)},
        {XMFLOAT3(4.0f, -3.0f,  6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3(4.0f, -3.0f, -6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 1.5f)},
        // WALL
        {XMFLOAT3( 4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(-4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(2.0f, 0.0f)},
        {XMFLOAT3( 4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3(-4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(2.0f, 1.5f)},
        // WALL
        {XMFLOAT3(-4.0f,  3.0f, -6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(-4.0f,  3.0f,  6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 0.0f)},
        {XMFLOAT3(-4.0f, -3.0f, -6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3(-4.0f, -3.0f,  6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 1.5f)},
    };

    WORD cellIndices[] = {
         0,  1,  2,
         1,  3,  2,
         4,  5,  6,
         5,  7,  6,
         8,  9, 10,
         9, 11, 10,
        12, 13, 14,
        13, 15, 14,
    };

    m_vertexCount = 16;
    m_indexCount = 24;

    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellVertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellIndices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}

            
            
```

Level.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level:
// This is an abstract class from which all of the levels of the game are derived.
// Each level potentially overrides up to four methods:
//     Initialize - (required) takes a list of objects and enables the objects that
//         are active for the level as well as setting their positions and
//         any animations associated with the objects.
//     Update - this method is called once per time step and is expected to
//         determine if the level has been completed.  The Level class provides
//         a 'standard' Update method which checks each object that is a target
//         and disables any active targets that have been hit.  It returns true
//         once there are no active targets remaining.
//     SaveState - method to save any Level specific state.  Default is defined as
//         not saving any state.
//     LoadState - method to restore any Level specific state.  Default is defined
//         as not restoring any state.

#include "GameObject.h"
#include "PersistentState.h"

ref class Level abstract
{
internal:
    virtual void Initialize(
        std::vector<GameObject^> objects
        ) = 0;

    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        );

    virtual void SaveState(PersistentState^ state);
    virtual void LoadState(PersistentState^ state);

    Platform::String^ Objective();
    float TimeLimit();

protected private:
    Platform::String^ m_objective;
    float             m_timeLimit;
};
            
            
```

Level.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level.h"

//----------------------------------------------------------------------

bool Level::Update(
    float /* time */,
    float /* elapsedTime */,
    float /* timeRemaining*/,
    std::vector<GameObject^> objects
    )
{
    int left = 0;

    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && (*object)->Target())
        {
            if ((*object)->Hit())
            {
                (*object)->Active(false);
            }
            else
            {
                left++;
            }
        }
    }
    return (left == 0);
}

//----------------------------------------------------------------------

void Level::SaveState(PersistentState^ /* state */)
{
}

//----------------------------------------------------------------------

void Level::LoadState(PersistentState^ /* state */)
{
}

//----------------------------------------------------------------------

Platform::String^ Level::Objective()
{
    return m_objective;
}

//----------------------------------------------------------------------

float Level::TimeLimit()
{
    return m_timeLimit;
}

//----------------------------------------------------------------------            
            
```

Level1.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level1:
// This class defines the first level of the game.  There are nine active targets.
// Each of the targets is stationary and can be hit in any order.

#include "Level.h"

ref class Level1: public Level
{
internal:
    Level1();
    virtual void Initialize(std::vector<GameObject^> objects) override;
};
            
```

Level1.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level1.h"
#include "Face.h"

using namespace DirectX;

//----------------------------------------------------------------------

Level1::Level1()
{
    m_timeLimit = 20.0f;
    m_objective =  "Hit each of the targets before time runs out.\nTouch to aim.  Tap in right box to fire.  Drag in left box to move.";
}

//----------------------------------------------------------------------

void Level1::Initialize(std::vector<GameObject^> objects)
{
    XMFLOAT3 position[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
        XMFLOAT3( 1.5f,  0.0f, -3.0f),
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
        XMFLOAT3( 2.0f,  0.0f,  0.0f),
        XMFLOAT3( 0.0f,  0.0f,  0.0f),
        XMFLOAT3(-2.0f,  0.0f,  0.0f)
    };

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Active(true);
                target->Target(true);
                target->Hit(false);
                target->AnimatePosition(nullptr);
                target->Position(position[targetCount]);
                targetCount++;
            }
            else
            {
                (*object)->Active(false);
            }
        }
        else
        {
            (*object)->Active(false);
        }
    }
}

//----------------------------------------------------------------------
            
            
```

Level2.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level2:
// This class defines the second level of the game.  It derives from the
// first level.  In this level, the targets must be hit in numeric order.

#include "Level1.h"

ref class Level2: public Level1
{
internal:
    Level2();
    virtual void Initialize(std::vector<GameObject^> objects) override;

    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        ) override;

    virtual void SaveState(PersistentState^ state) override;
    virtual void LoadState(PersistentState^ state) override;

private:
    int m_nextId;
};
            
            
```

Level2.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level2.h"
#include "Face.h"

//----------------------------------------------------------------------

Level2::Level2()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the targets in ORDER before time runs out.";
}

//----------------------------------------------------------------------

void Level2::Initialize(std::vector<GameObject^> objects)
{
    Level1::Initialize(objects);

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Target(targetCount == 0 ? true : false);
                targetCount++;
            }
        }
    }
    m_nextId = 1;
}

//----------------------------------------------------------------------

bool Level2::Update(
    float /* time */,
    float /* elapsedTime */,
    float /* timeRemaining */,
    std::vector<GameObject^> objects
    )
{
    int left = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && ((*object)->TargetId() > 0))
        {
            if ((*object)->Hit()  && ((*object)->TargetId() == m_nextId))
            {
                (*object)->Active(false);
                m_nextId++;
            }
            else
            {
                left++;
            }
        }
        if ((*object)->Active() && ((*object)->TargetId() == m_nextId))
        {
            (*object)->Target(true);
        }
    }
    return (left == 0);
}

//----------------------------------------------------------------------

void Level2::SaveState(PersistentState^ state)
{
    state->SaveInt32(":NextTarget", m_nextId);
}

//----------------------------------------------------------------------

void Level2::LoadState(PersistentState^ state)
{
    m_nextId = state->LoadInt32(":NextTarget", 1);
}

//----------------------------------------------------------------------            
            
```

Level3.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level3:
// This class defines the third level of the game.  In this level, each of the
// nine targets is moving along closed paths and can be hit
// in any order.

#include "Level.h"

ref class Level3: public Level
{
internal:
    Level3();
    virtual void Initialize(std::vector<GameObject^> objects) override;
};
            
            
```

Level3.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level3.h"
#include "Face.h"
#include "Animate.h"

using namespace DirectX;

//----------------------------------------------------------------------

Level3::Level3()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the moving targets before time runs out.";
}

//----------------------------------------------------------------------

void Level3::Initialize(std::vector<GameObject^> objects)
{
    XMFLOAT3 position[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
        XMFLOAT3( 1.5f,  0.0f, -5.5f),
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
        XMFLOAT3( 0.0f, -3.6f,  0.0f),
        XMFLOAT3( 0.0f, -3.6f,  0.0f),
        XMFLOAT3( 0.0f, -3.6f,  0.0f)
    };
    XMFLOAT3 LineList1[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
        XMFLOAT3(-0.5f,  1.0f,  1.0f),
        XMFLOAT3(-0.5f, -2.5f,  1.0f),
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
    };
    XMFLOAT3 LineList2[] =
    {
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
        XMFLOAT3(-2.0f,  2.0f, -1.5f),
        XMFLOAT3(-2.0f, -2.5f, -1.5f),
        XMFLOAT3( 1.5f, -2.5f, -1.5f),
        XMFLOAT3( 1.5f, -2.5f, -3.0f),
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
    };
    XMFLOAT3 LineList3[] =
    {
        XMFLOAT3(1.5f,  0.0f, -5.5f),
        XMFLOAT3(1.5f,  1.0f, -5.5f),
        XMFLOAT3(1.5f, -2.5f, -5.5f),
        XMFLOAT3(1.5f,  0.0f, -5.5f),
    };
    XMFLOAT3 LineList4[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
        XMFLOAT3( 1.0f, -1.0f, -5.5f),
        XMFLOAT3( 1.0f,  1.0f, -5.5f),
        XMFLOAT3(-2.5f,  1.0f, -5.5f),
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
    };
    XMFLOAT3 LineList5[] =
    {
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
        XMFLOAT3( 2.0f, -2.0f, -5.0f),
        XMFLOAT3( 2.0f,  1.0f, -5.0f),
        XMFLOAT3(-2.5f,  1.0f, -5.0f),
        XMFLOAT3(-2.5f, -2.0f, -5.0f),
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
    };
    XMFLOAT3 LineList6[] =
    {
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
        XMFLOAT3(-2.5f, -2.0f, -5.5f),
        XMFLOAT3(-2.5f,  1.0f, -5.5f),
        XMFLOAT3( 1.5f,  1.0f, -5.5f),
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
    };

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Active(true);
                target->Target(true);
                target->Hit(false);
                target->Position(position[targetCount]);
                switch (targetCount)
                {
                case 0:
                    target->AnimatePosition(ref new AnimateLineListPosition(4, LineList1, 10.0f, true));
                    break;
                case 1:
                    target->AnimatePosition(ref new AnimateLineListPosition(6, LineList2, 15.0f, true));
                    break;
                case 2:
                    target->AnimatePosition(ref new AnimateLineListPosition(4, LineList3, 15.0f, true));
                    break;
                case 3:
                    target->AnimatePosition(ref new AnimateLineListPosition(5, LineList4, 15.0f, true));
                    break;
                case 4:
                    target->AnimatePosition(ref new AnimateLineListPosition(6, LineList5, 15.0f, true));
                    break;
                case 5:
                    target->AnimatePosition(ref new AnimateLineListPosition(5, LineList6, 15.0f, true));
                    break;
                case 6:
                    target->AnimatePosition(
                        ref new AnimateCirclePosition(
                            XMFLOAT3(0.0f, -2.5f, 0.0f),
                            XMFLOAT3(0.0f, -3.6f, 0.0f),
                            XMFLOAT3(0.0f,  0.0f, 1.0f),
                            9.0f,
                            true,
                            true
                            )
                        );
                    break;
                case 7:
                    target->AnimatePosition(
                        ref new AnimateCirclePosition(
                            XMFLOAT3(0.0f, -2.5f, 0.0f),
                            XMFLOAT3(0.0f, -3.6f, 0.0f),
                            XMFLOAT3(0.0f,  0.0f, 1.0f),
                            9.0f,
                            true,
                            true
                            )
                        );
                    target->AnimatePosition()->Start(3.0f);
                    break;
                case 8:
                    target->AnimatePosition(
                        ref new AnimateCirclePosition(
                            XMFLOAT3(0.0f, -2.5f, 0.0f),
                            XMFLOAT3(0.0f, -3.6f, 0.0f),
                            XMFLOAT3(0.0f,  0.0f, 1.0f),
                            9.0f,
                            true,
                            true
                            )
                        );
                    target->AnimatePosition()->Start(6.0f);
                    break;
                }
                targetCount++;
            }
            else
            {
                target->Active(false);
            }
        }
        else
        {
            (*object)->Active(false);
        }
    }
}

//----------------------------------------------------------------------
            
            
```

Level4.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level4:
// This class defines the fourth level of the game.  It derives from the
// third level.  The targets must be hit in numeric order.

#include "Level3.h"

ref class Level4: public Level3
{
internal:
    Level4();
    virtual void Initialize(std::vector<GameObject^> objects) override;

    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        ) override;

    virtual void SaveState(PersistentState^ state) override;
    virtual void LoadState(PersistentState^ state) override;

private:
    int m_nextId;
};
            
            
```

Level4.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level4.h"
#include "Face.h"

//----------------------------------------------------------------------

Level4::Level4()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the moving targets in ORDER before time runs out.";
}

//----------------------------------------------------------------------

void Level4::Initialize(std::vector<GameObject^> objects)
{
    Level3::Initialize(objects);

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Target(targetCount == 0 ? true : false);
                targetCount++;
            }
        }
    }
    m_nextId = 1;
}

//----------------------------------------------------------------------

bool Level4::Update(
    float /* time */,
    float /* elapsedTime */,
    float /* timeRemaining */,
    std::vector<GameObject^> objects
    )
{
    int left = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && ((*object)->TargetId() > 0))
        {
            if ((*object)->Hit() && ((*object)->TargetId() == m_nextId))
            {
                (*object)->Active(false);
                m_nextId++;
            }
            else
            {
                left++;
            }
        }
        if ((*object)->Active() && ((*object)->TargetId() == m_nextId))
        {
            (*object)->Target(true);
        }
    }
    return (left == 0);
}

//----------------------------------------------------------------------

void Level4::SaveState(PersistentState^ state)
{
    state->SaveInt32(":NextTarget", m_nextId);
}

//----------------------------------------------------------------------

void Level4::LoadState(PersistentState^ state)
{
    m_nextId = state->LoadInt32(":NextTarget", 1);
}

//----------------------------------------------------------------------
            
            
```

Level5.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level5:
// This class defines the fifth level of the game.  It derives from the
// third level.  This level introduces obstacles that move into place
// during game play.  The targets may be hit in any order.

#include "Level3.h"

ref class Level5: public Level3
{
internal:
    Level5();
    virtual void Initialize(std::vector<GameObject^> objects) override;
};
            
            
```

Level5.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level5.h"
#include "Cylinder.h"
#include "Animate.h"

using namespace DirectX;

//----------------------------------------------------------------------

Level5::Level5()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the moving targets while avoiding the obstacles before time runs out.";
}

//----------------------------------------------------------------------

void Level5::Initialize(std::vector<GameObject^> objects)
{
    Level3::Initialize(objects);

    XMFLOAT3 obstacleStartPosition[] =
    {
        XMFLOAT3(-4.5f, -3.0f, 0.0f),
        XMFLOAT3(4.5f, -3.0f, 0.0f),
        XMFLOAT3(0.0f, 3.01f, -2.0f),
        XMFLOAT3(-1.5f, -3.0f, -6.5f),
        XMFLOAT3(1.5f, -3.0f, -6.5f)
    };
    XMFLOAT3 obstacleEndPosition[] =
    {
        XMFLOAT3(-2.0f, -3.0f, 0.0f),
        XMFLOAT3(2.0f, -3.0f, 0.0f),
        XMFLOAT3(0.0f, -3.0f, -2.0f),
        XMFLOAT3(-1.5f, -3.0f, -4.0f),
        XMFLOAT3(1.5f, -3.0f, -4.0f)
    };
    float obstacleStartTime[] =
    {
        2.0f,
        5.0f,
        8.0f,
        11.0f,
        14.0f
    };

    int obstacleCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Cylinder^ obstacle = dynamic_cast<Cylinder^>(*object))
        {
            if (obstacleCount < 5)
            {
                obstacle->Active(true);
                obstacle->Position(obstacleStartPosition[obstacleCount]);
                obstacle->AnimatePosition(
                    ref new AnimateLinePosition(
                        obstacleStartPosition[obstacleCount],
                        obstacleEndPosition[obstacleCount],
                        10.0,
                        false
                        )
                    );
                obstacle->AnimatePosition()->Start(obstacleStartTime[obstacleCount]);
                obstacleCount ++;
            }
        }
    }
}

//----------------------------------------------------------------------            
            
```

Level6.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level6:
// This class defines the sixth and final level of the game.  It derives from the
// fifth level.  In this level, the targets do not disappear when they are hit.
// The target will stay highlighted for two seconds.  As this is the last level,
// the only criteria for completion is time expiring.

#include "Level5.h"

ref class Level6: public Level5
{
internal:
    Level6();
    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        ) override;
};
            
            
```

Level6.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level6.h"

//----------------------------------------------------------------------

Level6::Level6()
{
    m_timeLimit = 20.0f;
    m_objective = "Hit as many moving targets as possible while avoiding the obstacles before time runs out.";
}

//----------------------------------------------------------------------

bool Level6::Update(
    float time,
    float elapsedTime,
    float timeRemaining,
    std::vector<GameObject^> objects
    )
{
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && (*object)->Target())
        {
            if ((*object)->Hit() && ((*object)->HitTime() < (time - 2.0f)))
            {
                (*object)->Hit(false);
            }
        }
    }
    return ((timeRemaining - elapsedTime) <= 0.0f);
}

//----------------------------------------------------------------------            
            
```

TargetTexture.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// TargetTexture:
// This is a helper class to procedurally generate textures for game
// targets.  There are two versions of the textures, one when it is
// hit and the other is when it is not.
// The class creates the necessary resources to draw the texture into
// an off screen resource at initialization time.

ref class TargetTexture
{
internal:
    TargetTexture(
        _In_ ID3D11Device1*      d3dDevice,
        _In_ ID2D1Factory1*      d2dFactory,
        _In_ IDWriteFactory1*    dwriteFactory,
        _In_ ID2D1DeviceContext* d2dContext
        );

    void CreateTextureResourceView(
        _In_ Platform::String^ name,
        _Out_ ID3D11ShaderResourceView** textureResourceView
        );
    void CreateHitTextureResourceView(
        _In_ Platform::String^ name,
        _Out_ ID3D11ShaderResourceView** textureResourceView
        );

protected private:
    Microsoft::WRL::ComPtr<ID3D11Device1>           m_d3dDevice;
    Microsoft::WRL::ComPtr<ID2D1Factory1>           m_d2dFactory;
    Microsoft::WRL::ComPtr<ID2D1DeviceContext>      m_d2dContext;
    Microsoft::WRL::ComPtr<IDWriteFactory1>         m_dwriteFactory;

    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_redBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_blueBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_greenBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_whiteBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_blackBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_yellowBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_clearBrush;

    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry1;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry2;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry3;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry4;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry5;

    Microsoft::WRL::ComPtr<IDWriteTextFormat>       m_textFormat;
};            
            
```

TargetTexture.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "TargetTexture.h"
#include "DirectXSample.h"

using namespace Microsoft::WRL;
using namespace Windows::Graphics::Display;

TargetTexture::TargetTexture(
    _In_ ID3D11Device1* d3dDevice,
    _In_ ID2D1Factory1* d2dFactory,
    _In_ IDWriteFactory1* dwriteFactory,
    _In_ ID2D1DeviceContext* d2dContext
    )
{
    m_d3dDevice = d3dDevice;
    m_d2dFactory = d2dFactory;
    m_dwriteFactory = dwriteFactory;
    m_d2dContext = d2dContext;

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Red, 1.f),
            &m_redBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::CornflowerBlue, 1.0f),
            &m_blueBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Green, 1.f),
            &m_greenBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White, 1.f),
            &m_whiteBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Black, 1.f),
            &m_blackBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Yellow, 1.f),
            &m_yellowBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White, 0.0f),
            &m_clearBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                50.0f,
                50.0f
                ),
            &m_circleGeometry1)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                100.0f,
                100.0f
                ),
            &m_circleGeometry2)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                150.0f,
                150.0f
                ),
            &m_circleGeometry3)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                200.0f,
                200.0f
                ),
            &m_circleGeometry4)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                250.0f,
                250.0f
                ),
            &m_circleGeometry5)
            );

    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            425,        // fontsize
            L"en-US",   // locale
            &m_textFormat
            )
        );

    // Center the text horizontally.
    DX::ThrowIfFailed(
        m_textFormat->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_CENTER)
        );

    // Center the text vertically.
    DX::ThrowIfFailed(
        m_textFormat->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_CENTER)
        );
}
//----------------------------------------------------------------------
void TargetTexture::CreateTextureResourceView(
    _In_ Platform::String^ name,
    _Out_ ID3D11ShaderResourceView** textureResourceView
    )
{
    // Allocate a offscreen D3D surface for D2D to render our 2D content into
    D3D11_TEXTURE2D_DESC texDesc;
    texDesc.ArraySize = 1;
    texDesc.BindFlags = D3D11_BIND_RENDER_TARGET | D3D11_BIND_SHADER_RESOURCE;
    texDesc.CPUAccessFlags = 0;
    texDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    texDesc.Height = 512;
    texDesc.Width = 512;
    texDesc.MipLevels = 1;
    texDesc.MiscFlags = 0;
    texDesc.SampleDesc.Count = 1;
    texDesc.SampleDesc.Quality = 0;
    texDesc.Usage = D3D11_USAGE_DEFAULT;

    ComPtr<ID3D11Texture2D> offscreenTexture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(&texDesc, nullptr, &offscreenTexture)
        );

    // Convert the Direct2D texture into a Shader Resource View
    ComPtr<ID3D11ShaderResourceView> texture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateShaderResourceView(offscreenTexture.Get(), nullptr, &texture)
        );
#if defined(_DEBUG)
    {
        char debugName[100];
        int l = sprintf_s(debugName, sizeof(debugName) / sizeof(debugName[0]), "Simple3DGame Target %ls", name->Data());
        DX::ThrowIfFailed(
            texture->SetPrivateData(WKPDID_D3DDebugObjectName, l, debugName)
            );
    }
#endif

    ComPtr<IDXGISurface> dxgiSurface;
    DX::ThrowIfFailed(
        offscreenTexture.As(&dxgiSurface)
        );

    // Create a D2D render target which can draw into our offscreen D3D
    // surface. Given that we use a constant size for the texture, we
    // fix the DPI at 96.

    D2D1_BITMAP_PROPERTIES1 properties;
    properties.pixelFormat.format = DXGI_FORMAT_B8G8R8A8_UNORM;
    properties.pixelFormat.alphaMode = D2D1_ALPHA_MODE_PREMULTIPLIED;
    properties.dpiX = 96;
    properties.dpiY = 96;
    properties.bitmapOptions = D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW;
    properties.colorContext = nullptr;

    ComPtr<ID2D1Bitmap1> renderTarget;
    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiSurface.Get(),
            &properties,
            &renderTarget
            )
        );

    m_d2dContext->SetTarget(renderTarget.Get());
    float saveDpiX;
    float saveDpiY;

    m_d2dContext->GetDpi(&saveDpiX, &saveDpiY);
    m_d2dContext->SetDpi(96.0f, 96.0f);

    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());

    D2D1_SIZE_F renderTargetSize = renderTarget->GetSize();

    m_d2dContext->Clear(D2D1::ColorF(D2D1::ColorF::White));
    m_d2dContext->FillGeometry(m_circleGeometry5.Get(), m_redBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry4.Get(), m_blueBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry3.Get(), m_greenBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry2.Get(), m_yellowBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry1.Get(), m_blackBrush.Get());
    m_d2dContext->DrawText(
        name->Data(),
        name->Length(),
        m_textFormat.Get(),
        D2D1::RectF(0, 0, renderTargetSize.width, renderTargetSize.height),
        m_whiteBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->SetTarget(nullptr);
    m_d2dContext->SetDpi(saveDpiX, saveDpiY);

    *textureResourceView = texture.Detach();
}
//----------------------------------------------------------------------
void TargetTexture::CreateHitTextureResourceView(
    _In_ Platform::String^ name,
    _Out_ ID3D11ShaderResourceView** textureResourceView
    )
{
    // Allocate a offscreen D3D surface for D2D to render our 2D content into
    D3D11_TEXTURE2D_DESC texDesc;
    texDesc.ArraySize = 1;
    texDesc.BindFlags = D3D11_BIND_RENDER_TARGET | D3D11_BIND_SHADER_RESOURCE;
    texDesc.CPUAccessFlags = 0;
    texDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    texDesc.Height = 512;
    texDesc.Width = 512;
    texDesc.MipLevels = 1;
    texDesc.MiscFlags = 0;
    texDesc.SampleDesc.Count = 1;
    texDesc.SampleDesc.Quality = 0;
    texDesc.Usage = D3D11_USAGE_DEFAULT;

    ComPtr<ID3D11Texture2D> offscreenTexture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(&texDesc, nullptr, &offscreenTexture)
        );

    // Convert the Direct2D texture into a Shader Resource View.
    ComPtr<ID3D11ShaderResourceView> texture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateShaderResourceView(offscreenTexture.Get(), nullptr, &texture)
        );
#if defined(_DEBUG)
    {
        char debugName[100];
        int l = sprintf_s(debugName, sizeof(debugName) / sizeof(debugName[0]), "Simple3DGame HitTarget %ls", name->Data());
        DX::ThrowIfFailed(
            texture->SetPrivateData(WKPDID_D3DDebugObjectName, l, debugName)
            );
    }
#endif

    ComPtr<IDXGISurface> dxgiSurface;
    DX::ThrowIfFailed(
        offscreenTexture.As(&dxgiSurface)
        );

    // Create a D2D render target which can draw into our offscreen D3D
    // surface. Given that we use a constant size for the texture, we
    // fix the DPI at 96.

    D2D1_BITMAP_PROPERTIES1 properties;
    properties.pixelFormat.format = DXGI_FORMAT_B8G8R8A8_UNORM;
    properties.pixelFormat.alphaMode = D2D1_ALPHA_MODE_PREMULTIPLIED;
    properties.dpiX = 96;
    properties.dpiY = 96;
    properties.bitmapOptions = D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW;
    properties.colorContext = nullptr;

    ComPtr<ID2D1Bitmap1> renderTarget;
    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiSurface.Get(),
            &properties,
            &renderTarget
            )
        );

    m_d2dContext->SetTarget(renderTarget.Get());
    float saveDpiX;
    float saveDpiY;

    m_d2dContext->GetDpi(&saveDpiX, &saveDpiY);
    m_d2dContext->SetDpi(96.0f, 96.0f);

    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());

    D2D1_SIZE_F renderTargetSize = renderTarget->GetSize();

    m_d2dContext->Clear(D2D1::ColorF(D2D1::ColorF::Black));
    m_d2dContext->FillGeometry(m_circleGeometry5.Get(), m_yellowBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry4.Get(), m_greenBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry3.Get(), m_blueBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry2.Get(), m_redBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry1.Get(), m_whiteBrush.Get());
    m_d2dContext->DrawText(
        name->Data(),
        name->Length(),
        m_textFormat.Get(),
        D2D1::RectF(0, 0, renderTargetSize.width, renderTargetSize.height),
        m_blackBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->SetTarget(nullptr);
    m_d2dContext->SetDpi(saveDpiX, saveDpiY);

    *textureResourceView = texture.Detach();
}
//----------------------------------------------------------------------
            
            
```

Material.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Material:
// This class maintains the properties that represent how an object will
// look when it is rendered.  This includes the color of the object, the
// texture used to render the object, and the vertex and pixel shader that
// should be used for rendering.
// The RenderSetup method sets the appropriate values into the constantBuffer
// and calls the appropriate D3D11 context methods to set up the rendering pipeline
// in the graphics hardware.

#include "ConstantBuffers.h"

ref class Material
{
internal:
    Material(
        DirectX::XMFLOAT4 meshColor,
        DirectX::XMFLOAT4 diffuseColor,
        DirectX::XMFLOAT4 specularColor,
        float specularExponent,
        _In_ ID3D11ShaderResourceView* textureResourceView,
        _In_ ID3D11VertexShader* vertexShader,
        _In_ ID3D11PixelShader* pixelShader
        );

    void RenderSetup(
        _In_ ID3D11DeviceContext* context,
        _Inout_ ConstantBufferChangesEveryPrim* constantBuffer
        );

    void SetTexture(_In_ ID3D11ShaderResourceView* textureResourceView)
    {
        m_textureRV = textureResourceView;
    }

protected private:
    DirectX::XMFLOAT4   m_meshColor;
    DirectX::XMFLOAT4   m_diffuseColor;
    DirectX::XMFLOAT4   m_hitColor;
    DirectX::XMFLOAT4   m_specularColor;
    float               m_specularExponent;

    Microsoft::WRL::ComPtr<ID3D11VertexShader>       m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>        m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView> m_textureRV;
};
            
            
```

Material.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Material.h"
#include "GameConstants.h"

using namespace DirectX;

//--------------------------------------------------------------------------------

Material::Material(
    XMFLOAT4 meshColor,
    XMFLOAT4 diffuseColor,
    XMFLOAT4 specularColor,
    float specularExponent,
    _In_ ID3D11ShaderResourceView* textureResourceView,
    _In_ ID3D11VertexShader* vertexShader,
    _In_ ID3D11PixelShader* pixelShader
    )
{
    m_meshColor = meshColor;
    m_diffuseColor = diffuseColor;
    m_specularColor = specularColor;
    m_specularExponent = specularExponent;

    m_vertexShader = vertexShader;
    m_pixelShader = pixelShader;
    m_textureRV = textureResourceView;
}

//--------------------------------------------------------------------------------

void Material::RenderSetup(
    _In_ ID3D11DeviceContext* context,
    _Inout_ ConstantBufferChangesEveryPrim* constantBuffer
    )
{
    constantBuffer->meshColor = m_meshColor;
    constantBuffer->specularColor = m_specularColor;
    constantBuffer->specularPower = m_specularExponent;
    constantBuffer->diffuseColor = m_diffuseColor;

    context->PSSetShaderResources(0, 1, m_textureRV.GetAddressOf());
    context->VSSetShader(m_vertexShader.Get(), nullptr, 0);
    context->PSSetShader(m_pixelShader.Get(), nullptr, 0);
}            
            
```

> **Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui développent des applications de la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Rubriques connexes


* [Créer un jeu UWP simple avec DirectX](tutorial--create-your-first-metro-style-directx-game.md)

 

 






<!--HONumber=May16_HO2-->


