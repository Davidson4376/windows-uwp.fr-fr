---
author: joannaleecy
title: Configuration
description: "Découvrez comment assembler le pipeline de rendu pour afficher les graphiques. Rendu de jeu, configuration et préparation des données."
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, jeux, rendu
localizationpriority: medium
ms.openlocfilehash: 7d2f0850dff8b3e22fbbc487068f2db37b0f107a
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2017
---
# <a name="rendering-framework-ii-game-rendering"></a>Infrastructure de renduII: rendu de jeu

Dans [Infrastructure de renduI](tutorial--assembling-the-rendering-pipeline.md), nous avons abordé la façon dont les informations de la scène sont traitées et présentées à l’écran. Nous allons maintenant revenir légèrement en arrière et voir comment préparer les données pour le rendu.

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objectif

Récapitulatif rapide de l’objectif. Il s’agit de comprendre comment configurer une infrastructure de rendu de base pour afficher la sortie graphique pour un jeu UWP DirectX. Nous pouvons les regrouper dans ces trois étapes.

 1. Établir une connexion avec l’interface graphique
 2. Préparation: créer les ressources nécessaires pour dessiner les graphiques
 3. Afficher les graphiques: rendu de l’image

[Infrastructure de rendu I: présentation du rendu](tutorial--assembling-the-rendering-pipeline.md) expliquait la façon dont les graphiques étaient rendus, portant sur les étapes1 et 3. 

Cet article explique comment configurer les autres éléments de cette infrastructure et préparer les données requises avant d’effectuer le rendu, qui est l’étape2 du processus.

## <a name="design-the-renderer"></a>Conception du convertisseur

Le convertisseur est responsable de la création et de la gestion de tous les objetsD3D11 etD2D utilisés pour générer les effets visuels du jeu. La classe __GameRenderer__ est le convertisseur pour cet exemple de jeu et elle est conçue pour répondre aux besoins du jeu en matière de rendu.

Voici quelques concepts qui peuvent vous être utiles pour la conception du convertisseur pour votre jeu:
* Dans la mesure où les API Direct3D11 sont définies comme des API [COM](https://msdn.microsoft.com/library/windows/desktop/ms694363.aspx), vous devez fournir des références [ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) aux objets définis par ces API. Ces objets sont automatiquement libérés lorsque leur dernière référence sort du contexte une fois l’exécution de l’application terminée. Pour plus d’informations, voir [ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr). Voici quelques exemples de ces objets: tampons constants, objets nuanceurs - [nuanceur de vertex](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders), [nuanceur de pixels](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders) et objets de ressources de nuanceur.
* Les tampons constants sont définis dans cette classe pour contenir diverses données requises pour le rendu.
    * L’utilisation de plusieurs tampons constants avec des fréquences différentes permet de réduire la quantité de données à envoyer à l’unité de traitement graphique (GPU) par trame. Cet exemple répartit les constantes en plusieurs tampons en fonction de la fréquence à laquelle elles doivent être mises à jour. Il s’agit d’une recommandation pour la programmation Direct3D. 
    * Dans cet exemple de jeu, 4tampons constants sont définis.
        1. __m\_constantBufferNeverChanges__ contient les paramètres d’éclairage. Il est défini une fois dans la méthode __FinalizeCreateGameDeviceResources__ et ne change plus jamais.
        2. __m\_constantBufferChangeOnResize__ contient la matrice de projection. La matrice de projection dépend de la taille et des proportions de la fenêtre. Elle est définie dans [__CreateWindowSizeDependentResources__](#createwindowsizedependentresources-method) puis mise à jour une fois que les ressources ont été chargées dans la méthode [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method). Si le rendu est en3D, elle est également modifiée deux fois par trame.
        3. __m\_constantBufferChangesEveryFrame__ contient la matrice globale. Cette matrice dépend de la position de la caméra et de la direction de la vue (perpendiculaire à la projection) et change une fois par trame dans la méthode __Render__. Cela a été détaillé précédemment dans __Infrastructure de renduI: Présentation du rendu__, sous la méthode [__GameRenderer::Render__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).
        4. __m\_constantBufferChangesEveryPrim__ contient la matrice de modèle et les propriétés matérielles de chaque primitive. La matrice de modèle transforme les vertex des coordonnées locales en coordonnées universelles. Ces constantes sont spécifiques à chaque primitive et sont mises à jour pour chaque appel de dessin. Cela a été décrit précédemment dans __Infrastructure de renduI: Présentation du rendu__, sous [Rendu de primitive](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering).
* Les objets de ressources du nuanceur qui contiennent les textures des primitives sont également définis dans cette classe.
    * Certaines de ces textures sont prédéfinies ([DDS](https://msdn.microsoft.com/library/windows/desktop/bb943991.aspx) est un format de fichier qui peut être utilisé pour stocker les textures compressées et non compressées. Les textures DDS sont utilisées pour les murs et le sol du monde, ainsi que les sphères de munitions.)
    * Dans cet exemple de jeu, les objets de ressources du nuanceur sont: __m\_sphereTexture__, __m\_cylinderTexture__, __m\_ceilingTexture__, __m\_floorTexture__, __m\_wallsTexture__.
* Les objets de nuanceur sont définis dans cette classe pour calculer les primitives et les textures. 
    * Dans cet exemple de jeu, les objets du nuanceur sont __m\_vertexShader__, __m\_vertexShaderFlat__, et __m\_pixelShader__, __m\_pixelShaderFlat__.
    * Le nuanceur de vertex traite les primitives et l’éclairage de base, tandis que le nuanceur de pixels (parfois appelé nuanceur de fragments) traite les textures et tous les effets par pixel.
    * Il existe deux versions de ces nuanceurs (régulier et plat) pour rendre des primitives différentes. Cela est dû au fait que les versions plates sont beaucoup plus simples et n’engendrent pas de mises en surbrillance spéculaires ni d’effets d’éclairage par pixel. Ces nuanceurs sont utilisés pour les murs et accélèrent le rendu sur des périphériques de faible puissance.

## <a name="gamerendererh"></a>GameRenderer.h

Examinons maintenant le code de l’objet de classe du convertisseur de l’exemple de jeu et comparons-le à __Sample3DSceneRenderer.h__ fourni dans le modèle d’application DirectX 11.

```cpp
// Class object handling the rendering of the game
ref class GameRenderer
{
internal:
    GameRenderer(const std::shared_ptr<DX::DeviceResources>& deviceResources);
    
    // Compared with Sample3DSceneRenderer.h in the DirectX 11 App template sample. 
    
    // These methods are present.
    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();

    // Added: Async related methods to prepare 3D game objects for rendering
    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();
    // --- end of async related methods section

    // Added: Methods for rendering overlay
    Simple3DGameDX::IGameUIControl^ GameUIControl()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay->Visible(); }
    // --- end of rendering overlay section

    //...
    protected private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>                m_deviceResources;

    // ...

    // Shader resource objects
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

    // Texture sampler
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>Constructeur

Examinons maintenant le constructeur __GameRenderer__ de l’exemple de jeu et comparons-le au constructeur __Sample3DSceneRenderer__ fourni dans le modèle d’application DirectX 11.

```cpp
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(const std::shared_ptr<DX::DeviceResources>& deviceResources) : //...
{
    // Compared with Sample3DSceneRenderer::Sample3DSceneRenderer in the DirectX 11 App template sample. 
    
    // Added: Create a new GameHud object to rendered text on the top left corner of the screen
    m_gameHud = ref new GameHud(
        deviceResources,
        "Windows platform samples",
        "DirectX first-person game sample"
        );
    //--- end of new GameHud object section
        
    // Added: Game info rendered as an overlay on the top right corner of the screen (eg. Hits, Shots, Time)    
    m_gameInfoOverlay = ref new GameInfoOverlay(deviceResources);
    //--- end of game info rendered as overlay section

    // These methods are present.
    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>Création et chargement des ressources graphiques DirectX

Dans l’exemple de jeu (et dans le modèle __DirectX11App (Windows universel)__ de VisualStudio), la création et le chargement des ressources du jeu sont implémentés à l’aide des deux méthodes suivantes appelées à partir du constructeur __GameRenderer__:

* [__CreateDeviceDependentResources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresources-method)

## <a name="createdevicedependentresources-method"></a>Méthode CreateDeviceDependentResources

Dans le modèle d’application DirectX11, cette méthode est utilisée pour charger les nuanceurs de vertex et de pixels de manière asynchrone, créer le tampon de nuanceur et constant, créer un maillage avec les vertex contenant les informations de position et de couleur. 

Dans l’exemple de jeu, ces opérations sur les objets de scène sont réparties entre les méthodes [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) et [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) à la place. 

Pour cet exemple de jeu, que contient cette méthode?

* Des variables instanciées (__m\_gameResourcesLoaded__ = false et __m\_levelResourcesLoaded__ = false) qui indiquent si les ressources ont été chargées avant de passer au rendu, étant donné que nous allons les charger de manière asynchrone. 
* Étant donné que l’affichage à tête haute et le rendu de superposition se trouvent dans des objets de classe séparés, appelez les méthodes __GameHud::CreateDeviceDependentResources__ et __GameInfoOverlay::CreateDeviceDependentResources__ ici.

Voici le code pour __GameRenderer::CreateDeviceDependentResources__.

```cpp
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate if resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud->CreateDeviceDependentResources();
    m_gameInfoOverlay->CreateDeviceDependentResources();
}
```
Le tableau ci-dessous répertorie les méthodes utilisées pour créer et charger les ressources. __CreateGameDeviceResourcesAsync__ et __FinalizeCreateGameDeviceResources__ sont ajoutées dans l’exemple de jeu afin que les ressources soient chargées de manière asynchrone.

|Modèle d’application DirectX11 d’origine           |Exemple de jeu                                                                |
|-------------------------------------------|---------------------------------------------------------------------------|
|CreateDeviceDependentResources             |CreateDeviceDependentResources                                             |
|                                           | - CreateGameDeviceResourcesAsync (ajoutée)                                  |
|                                           | - FinalizeCreateGameDeviceResources (ajoutée)                               |
|CreateWindowSizeDependentResources         |CreateWindowSizeDependentResources                                         |

Avant d’approfondir les autres méthodes utilisées pour créer et charger des ressources, commençons par créer le convertisseur et voyons comment il s’intègre dans la boucle de jeu.

## <a name="create-the-renderer"></a>Créer le convertisseur

Le __GameRenderer__ est créé dans le constructeur __GameMain__. Il appelle également deux autres méthodes, [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) et [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) qui sont ajoutées pour la création et le chargement des ressources.

```cpp

GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) : // ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // These methods are used in the DirectX 11 App template to create the class objects used for rendering. 
    // But are replaced in this game sample with GameRenderer as shown below.
    // m_sceneRenderer = std::unique_ptr<Sample3DSceneRenderer>(new Sample3DSceneRenderer(m_deviceResources));
    // m_fpsTextRenderer = std::unique_ptr<SampleFpsTextRenderer>(new SampleFpsTextRenderer(m_deviceResources));
    
    // Creation of GameRenderer
    m_renderer = ref new GameRenderer(m_deviceResources);
    
    //...

     create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();
    
    //...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>Méthode CreateGameDeviceResourcesAsync

La méthode __CreateGameDeviceResourcesAsync__ est appelée à partir de la méthode du constructeur __GameMain__ dans la boucle __create\_task__, car les ressources du jeu sont chargées de manière asynchrone.
        
__CreateDeviceResourcesAsync__ est une méthode qui s’exécute en tant qu’ensemble séparé de tâches asynchrones pour charger les ressources du jeu. Elle n’a accès qu’aux méthodes de périphériques Direct3D11 (celles définies sur __ID3D11Device__) et non aux méthodes de contexte de périphériques (méthodes définies sur __ID3D11DeviceContext__), car elle doit être exécutée sur un thread distinct. Elle n’offre donc pas la possibilité d’effectuer un rendu.

La méthode __FinalizeCreateGameDeviceResources__ s’exécute sur le thread principal et a accès aux méthodes de contexte de périphériques Direct3D11.

En principe:
* Utilisez uniquement les méthodes __ID3D11Device__ dans __CreateGameDeviceResourcesAsync__, car elles sont dépourvues de thread, ce qui signifie qu’elle peuvent s’exécuter sur n’importe quel thread. En principe, elles ne s’exécutent pas sur le même thread que celui sur lequel __GameRenderer__ a été créé. 
* N’utilisez pas les méthodes dans __ID3D11DeviceContext__ ici, car elles doivent s’exécuter sur un seul thread et sur le même thread que __GameRenderer__.
* Utilisez cette méthode pour créer des tampons constants.
* Utilisez cette méthode pour charger des textures (comme les fichiers.dds) et les informations relatives au nuanceur (comme les fichiers.cso) dans les [nuanceurs](tutorial--assembling-the-rendering-pipeline.md#shaders).

Cette méthode est utilisée pour:
* Créer les 4 [tampons constants](tutorial--assembling-the-rendering-pipeline.md#buffer): __m_constantbufferneverchanges__, __m\_constantBufferChangeOnResize__, __m\_constantBufferChangesEveryFrame__, __m\_ constantBufferChangesEveryPrim__
* Créer un objet [sampler-state](tutorial--assembling-the-rendering-pipeline.md#sampler-state) qui encapsule les informations d’échantillonnage pour une texture
* Créer un groupe de tâches contenant toutes les tâches asynchrones créées par la méthode. Elle attend la fin de toutes les tâches asynchrones, puis appelle __FinalizeCreateGameDeviceResources__.
* Créer un chargeur à l’aide de [BasicLoader](tutorial--assembling-the-rendering-pipeline.md#basicloader). Ajoutez les opérations de chargement asynchrones du chargeur en tant que tâches dans le groupe de tâches créé précédemment.
* Des méthodes telles que __BasicLoader::LoadShaderAsync__ et __BasicLoader::LoadTextureAsync__ sont utilisées pour charger:
    * les objets nuanceurs compilés (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso et PixelShaderFlat.cso). Pour plus d’informations, voir [Divers formats de fichier de nuanceur](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats).
    * des textures spécifiques au jeu (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds, cellfloor.dds, cellwall.dds).

```cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method.  It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC.
    // For API ref, go to: https://msdn.microsoft.com/library/windows/desktop/ff476092.aspx
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));
    
    bd.Usage = D3D11_USAGE_DEFAULT;
    // ...
    
    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476501.aspx
    
    DX::ThrowIfFailed(
        d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges) 
        );
    // ...
    
    // Define D3D11_SAMPLER_DESC. For API ref, go to: https://msdn.microsoft.com/library/windows/desktop/ff476207.aspx
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. 
    // For API ref, go to: https://msdn.microsoft.com/en-us/library/windows/desktop/aa366920(v=vs.85).aspx
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    // ...
    
    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    DX::ThrowIfFailed(
        d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures (resources).
    
    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. 
    // For more info, go to: https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader
    BasicLoader^ loader = ref new BasicLoader(d3dDevice);

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the BasicLoader::LoadShaderAsync method
    // Push these method calls into a list of tasks
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure the previous versions are set to NULL before any of the textures are loaded.
    m_sphereTexture = nullptr;
    // ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds, cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks
    tasks.push_back(loader->LoadTextureAsync("Assets\\seafloor.dds", nullptr, &m_sphereTexture));
    // ...
    
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources by introducing a delay.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    return when_all(tasks.begin(), tasks.end());
}
```

## <a name="finalizecreategamedeviceresources-method"></a>Méthode FinalizeCreateGameDeviceResources

La méthode __FinalizeCreateGameDeviceResources__ est appelée une fois que toutes les tâches de chargement de ressources qui se trouvent dans la méthode __CreateGameDeviceResourcesAsync__ sont terminées. 

* Initialisez constantBufferNeverChanges avec les positions de la lumière et la couleur. Charge les données initiales dans les tampons constants avec un appel de méthode de contexte de périphérique à __ID3D11DeviceContext::UpdateSubresource__.
* Étant donné que le chargement de ressources chargées de façon asynchrone est terminé, il est temps de les associer avec les objets jeu appropriés.
* Pour chaque objet jeu, créez le maillage et le matériel à l’aide des textures qui ont été chargées. Ensuite, associez le maillage et le matériel à l’objet jeu.
* Pour l’objet jeu cible, la texture qui se compose d’anneaux colorés concentriques, avec une valeur numérique dans la partie supérieure, n’est pas chargée à partir d’un fichier de texture. Au lieu de cela, elle est générée par procédure à l’aide du code de __TargetTexture.cpp__. La classe __TargetTexture__ crée les ressources nécessaires pour dessiner la texture dans une ressource hors écran lors de l’initialisation. La texte obtenue est ensuite associée aux objets jeu cibles appropriés.

__FinalizeCreateGameDeviceResources__ et [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) partagent des partie de code similaires pour ce qui suit:
* Utilisez __SetProjParams__ pour vous assurer que la caméra dispose de la matrice de projection appropriée. Pour plus d’informations, voir [Caméra et espace de coordonnées](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space).
* Gérez la rotation écran en postmultipliant la matrice de rotation3D par la matrice de projection de la caméra. Ensuite, mettez à jour le tampon constant __ConstantBufferChangeOnResize__ avec la matrice de projection obtenue.
* Définissez la variable globale __m\_gameResourcesLoaded__ __Boolean__ pour indiquer que les ressources sont désormais chargées dans les tampons et prêtes pour l’étape suivante. Rappelez-vous que nous avons d’abord initialisé cette variable comme étant __FALSE__ dans la méthode du constructeur __GameRenderer__, via la méthode __GameRenderer::CreateDeviceDependentResources__. 
* Lorsque la valeur de __m\_gameResourcesLoaded__ est __TRUE__, le rendu des objets de la scène peut être effectué. Cela a été traité dans l’article __Infrastructure de renduI: présentation du rendu__ sous la méthode [__GameRenderer::Render__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).

```cpp
// When creating this sample game using the DirectX 11 App template, this method needs to be created.
// This new method is called from GameMain constructor in the .then loop.
// Make sure the 2D rendering is occurring on the same thread as the main rendering.
// Note: Helper class .h and .cpp files used in this method are located in the SharedContent/cpp/GameContent folder
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize constantBufferNeverChanges with the light positions and color.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();
    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    // ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.Get(),
        0,
        nullptr,
        &constantBufferNeverChanges,
        0,
        0
        );

    // For the objects that function as targets, they have two unique generated textures.
    // One version is used to show that they have never been hit and the other is 
    // used to show that they have been hit.
    // TargetTexture is a helper class to procedurally generate textures for game
    // targets. The class creates the necessary resources to draw the texture into 
    // an off screen resource at initialization time.

    TargetTexture^ textureGenerator = ref new TargetTexture(
        d3dDevice,
        m_deviceResources->GetD2DFactory(),
        m_deviceResources->GetDWriteFactory(),
        m_deviceResources->GetD2DDeviceContext()
        );

    // CylinderMesh is a class derived from MeshObject and creates a ID3D11Buffer of
    // vertices and indices to represent a canonical cylinder (capped at
    // both ends) that is positioned at the origin with a radius of 1.0,
    // a height of 1.0 and with its axis in the +Z direction.
    // In the game sample, there are various types of mesh types:
    // CylinderMesh (vertical rods), SphereMesh (balls that the player shoots), 
    // FaceMesh (target objects), and WorldMesh (Floors and ceilings that define the enclosed area)

    MeshObject^ cylinderMesh = ref new CylinderMesh(d3dDevice, 26);
    // ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    // ...
    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {

        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

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
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            (*object)->Mesh(ref new WorldFloorMesh(d3dDevice));
        }
        // ...
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
        // ...
    }


    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera()->SetProjParams(
        XM_PI / 2,
        renderTargetSize.Width / renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that the correct projection matrix is set in the ConstantBufferChangeOnResize buffer.

    // Get the 3D rotation transform matrix. We are handling screen rotations directly to eliminate an unaligned 
    // fullscreen copy. So it is necessary to post multiply the 3D rotation matrix to the camera's projection matrix
    // to get the projection matrix that we need.

    auto orientation = m_deviceResources->GetOrientationTransform3D();

    ConstantBufferChangeOnResize changesOnResize;

    // The matrices are transposed due to the shader code expecting the matrices in the opposite
    // row/column order from the DirectX math library.

    // XMStoreFloat4x4 takes a matrix and writes the components out to sixteen single-precision floating-point values at the given address. 
    // The most significant component of the first row vector is written to the first four bytes of the address, 
    // followed by the second most significant component of the first row, and so on. The second row is then written out in a 
    // like manner to memory beginning at byte 16, followed by the third row to memory beginning at byte 32, and finally 
    // the fourth row to memory beginning at byte 48. For more API ref info, go to: 
    // https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.storing.xmstorefloat4x4.aspx

    XMStoreFloat4x4(
        &changesOnResize.projection,
        XMMatrixMultiply(
            XMMatrixTranspose(m_game->GameCamera()->Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    // Finally we set the m_gameResourcesLoaded as TRUE, so we can start rendering.
    m_gameResourcesLoaded = true;
}
```

## <a name="createwindowsizedependentresource-method"></a>Méthode CreateWindowSizeDependentResource

Les méthodes CreateWindowSizeDependentResources sont appelées à chaque modification de la taille de la fenêtre, de l’orientation, du rendu avec stéréo ou de la résolution. Dans l’exemple de jeu, la méthode met à jour la matrice de projection dans __ConstantBufferChangeOnResize__.

Les ressources de taille de la fenêtre sont mises à jour comme suit: 
* L’infrastructure de l’application obtient un des événements possibles indiquant un changement de l’état de la fenêtre. 
* Votre boucle de jeu principale est ensuite notifiée de l’événement et appelle l’instance __CreateWindowSizeDependentResources__ sur l’instance de classe principale (__GameMain__), qui appelle à son tour l’implémentation __CreateWindowSizeDependentResources__ mise en œuvre dans la classe du convertisseur de jeu (__GameRenderer__).
* La tâche principale de cette méthode consiste à s’assurer que les effets visuels ne deviennent pas confus ou incorrects suite à une modification des propriétés de la fenêtre.

Pour cet exemple de jeu, plusieurs appels de méthode sont identiques à la méthode [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method). Pour la procédure de codage pas à pas, revenez à la section précédente.

Les modifications de l’affichage à tête haute et du rendu de la taille de la fenêtre de superposition du jeu sont détaillées dans l’article [Ajouter une interface utilisateur](#tutorial--adding-a-user-interface).

```cpp
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{

    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud->CreateWindowSizeDependentResources();

    // ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    // ...

    m_gameInfoOverlay->CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera()->SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                )
            );

        d3dContext->UpdateSubresource(
            m_constantBufferChangeOnResize.Get(),
            0,
            nullptr,
            &changesOnResize,
            0,
            0
            );
    }
}
```

## <a name="next-steps"></a>Étapes suivantes

Il s’agit là du processus de base pour l’implémentation de l’infrastructure de rendu des éléments graphiques d’un jeu. Plus votre jeu est grand, plus il vous faudra mettre en place d’abstractions pour gérer les hiérarchies de types d’objets et de comportements d’animation. Vous devez implémenter des méthodes plus complexes pour le chargement et la gestion des ressources telles que les maillages et les textures. Voyons maintenant comment [ajouter une interface utilisateur](tutorial--adding-a-user-interface.md).