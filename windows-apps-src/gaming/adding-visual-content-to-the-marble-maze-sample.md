---
title: Ajout de contenu visuel à l’exemple Marble Maze
description: Ce document décrit comment le jeu Marble Maze utilise Direct3D et Direct2D dans l’environnement d’application de plateforme Windows universelle (UWP) afin de vous présenter ces modèles et de vous aider à les adapter à vos propres jeux.
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
ms.date: 09/08/2017
ms.topic: article
keywords: Windows 10, uwp, jeux, exemple, directx, graphisme
ms.localizationpriority: medium
ms.openlocfilehash: ce62e065170349523062fbd42d867edfed63f47c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369081"
---
# <a name="adding-visual-content-to-the-marble-maze-sample"></a>Ajout de contenu visuel à l’exemple Marble Maze




Ce document décrit comment le jeu Marble Maze utilise Direct3D et Direct2D dans l’environnement d’application de plateforme Windows universelle (UWP) afin de vous présenter ces modèles et de vous aider à les adapter à vos propres jeux. Pour apprendre la place des composants visuels dans la structure globale de l’application Marble Maze, voir [Structure de l’application Marble Maze](marble-maze-application-structure.md).

Nous avons suivi les étapes indiquées ci-dessous lors du développement des aspects visuels de Marble Maze :

1.  Créer une infrastructure de base qui initialise les environnements Direct3D et Direct2D.
2.  Utilisez image et les programmes d’édition de modèle pour concevoir les composants 2D et 3D qui s’affichent dans le jeu.
3.  Assurez-vous que des ressources 2D et 3D chargeront correctement et qu’il apparaissent dans le jeu.
4.  Intégrer des nuanceurs de vertex et de pixels qui améliorent la qualité visuelle des éléments du jeu.
5.  Intégrer la logique du jeu, par exemple l’animation et les entrées utilisateur.

Nous nous sommes également concentrés tout d’abord sur l’ajout de composants 3D, puis sur ressources 2D. Par exemple, nous avons commencé par la logique principale du jeu avant d’ajouter le système de menus et le minuteur.

Nous devions également répéter plusieurs fois ces étapes pendant le processus de développement. Par exemple, que nous avons apporté des modifications aux modèles de maillage et marble, nous avons dû également modifier le code de nuanceur qui prend en charge ces modèles.

> [!NOTE]
> L’exemple de code correspondant à ce document est disponible dans [l’exemple de jeu Marble Maze en DirectX](https://go.microsoft.com/fwlink/?LinkId=624011).

 
Voici quelques-uns des éléments clés de ce document qui décrivent l’utilisation de DirectX et de contenu de jeu visuel, notamment l’initialisation des bibliothèques de graphiques DirectX, le chargement des ressources de scène et la mise à jour et le rendu de la scène :

-   L’ajout de contenu de jeu comprend de nombreuses étapes. Ces étapes doivent souvent être itérées. Les développeurs de jeux souvent concentrent en premier lieu sur l’ajout de contenu de jeux 3D, puis sur l’ajout de contenu 2D.
-   Plus vous prenez en charge des matériels graphiques différents, plus vous êtes en mesure de fournir une expérience de jeu de grande qualité à différents types d’utilisateur.
-   Séparez les formats au moment de la conception et au moment de l’exécution. Structurez vos éléments de conception pour une flexibilité maximum et pour permettre des itérations rapides sur le contenu. Mettez en forme et compressez vos éléments pour qu’ils soient chargés et affichés aussi rapidement que possible au moment de l’exécution.
-   Vous créez des appareils Direct3D et Direct2D dans une application pour UWP tout comme dans une application de bureau Windows classique. Une différence réside dans l’association de la chaîne d’échange à la fenêtre de sortie.
-   Quand vous concevez votre jeu, vérifiez que le format de maillage que vous choisissez prend en charge vos scénarios clés. Par exemple, si votre jeu utilise des collisions, vérifiez que vous pouvez obtenir des données de collision de vos maillages.
-   Séparez la logique du jeu et la logique de rendu en mettant à jour tous les objets de scène avant de les afficher.
-   Vous dessinez généralement vos objets de scène 3D, et n’importe quel 2D les objets qui apparaissent devant la scène.
-   Synchronisez le dessin sur le vide vertical afin que votre jeu ne dessine pas des images qui ne sont jamais affichées. Un *blank vertical* est l’heure entre lorsqu’une image termine dessin à l’analyse et commence le frame suivant.

## <a name="getting-started-with-directx-graphics"></a>Prise en main des graphiques DirectX


Lorsque nous avons commencé le jeu Marble Maze Universal Windows Platform (UWP), nous avons choisi de C++ et Direct3D 11.1, car ils sont excellents choix pour la création de jeux en 3D qui nécessitent un contrôle maximal de rendu et aux performances élevées. DirectX 11.1 prend en charge le matériel DirectX 9 à DirectX 11, ce qui vous permet de proposer votre jeu à davantage d’utilisateurs sans avoir besoin de réécrire le code pour les versions antérieures de DirectX.

Jeu Marble Maze utilise Direct3D 11.1 pour restituer les jeu des composants 3D, à savoir le marbre et labyrinthe. Marble Maze utilise également Direct2D, DirectWrite et Windows Imaging Component (WIC) pour dessiner les ressources de jeux 2D, telles que les menus et la minuterie.

Le développement de jeu nécessite une bonne planification. Si vous débutez avec DirectX graphics, nous vous recommandons de lire [DirectX : Mise en route](directx-getting-started.md) pour vous familiariser avec les concepts de base de création d’un jeu UWP DirectX. Que vous lire ce document et travailler avec le code source de labyrinthe de billes, vous pouvez faire référence aux ressources suivantes pour plus d’informations sur les graphiques de DirectX :

-   [Direct3D 11 Graphics](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11): Décrit Direct3D 11, un puissant, avec accélération matérielle 3D API graphique pour le rendu de géométrie 3D sur la plateforme Windows.
-   [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal): Décrit Direct2D, une API de graphismes 2D avec accélération matérielle qui fournit des performances élevées et rendu de haute qualité pour géométrique 2D, bitmaps et texte.
-   [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal): Décrit DirectWrite, qui prend en charge le rendu de texte de haute qualité.
-   [Windows Imaging Component](https://docs.microsoft.com/windows/desktop/wic/-wic-lh): Décrit le WIC, une plateforme extensible qui fournit des API de bas niveau pour les images numériques.

### <a name="feature-levels"></a>Niveaux de fonctionnalité

Direct3D 11 introduit un paradigme nommé *niveaux de fonctionnalité*. Un niveau de fonctionnalité est un ensemble de fonctionnalités GPU définies. Utilisez les niveaux de fonctionnalités afin que votre jeu puisse s’exécuter sur des versions antérieures de matériel Direct3D. Marble Maze prend en charge le niveau de fonctionnalité 9.1 car il ne nécessite pas les fonctionnalités avancées des niveaux supérieurs. Nous recommandons de prendre en charge la gamme de matériel la plus étendue possible afin que vos utilisateurs puissent bénéficier de la même expérience quel que soit leur ordinateur. Pour plus d’informations sur les niveaux de fonctionnalité, voir [Direct3D 11 sur du matériel avec un niveau de fonctionnalité inférieur](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel).

## <a name="initializing-direct3d-and-direct2d"></a>Initialisation Direct3D et Direct2D


Un appareil représente la carte vidéo. Vous créez des appareils Direct3D et Direct2D dans une application pour UWP tout comme dans une application de bureau Windows classique. La différence principale réside dans la connexion de la chaîne d’échange Direct3D au système de fenêtrage.

La classe **DeviceResources** est un élément fondamental de la gestion Direct3D et Direct2D. Cette classe gère l’infrastructure générale, les ressources non spécifiques à un jeu. Marble Maze définit le **MarbleMazeMain** classe pour gérer des ressources spécifiques à un jeu, ce qui a une référence à un **DeviceResources** objet afin de lui donner accès à Direct3D et Direct2D.

Pendant l’initialisation, le **DeviceResources** le constructeur crée les ressources indépendantes du périphérique et les périphériques Direct3D et Direct2D.

```cpp
// Initialize the Direct3D resources required to run. 
DX::DeviceResources::DeviceResources() :
    m_screenViewport(),
    m_d3dFeatureLevel(D3D_FEATURE_LEVEL_9_1),
    m_d3dRenderTargetSize(),
    m_outputSize(),
    m_logicalSize(),
    m_nativeOrientation(DisplayOrientations::None),
    m_currentOrientation(DisplayOrientations::None),
    m_dpi(-1.0f),
    m_deviceNotify(nullptr)
{
    CreateDeviceIndependentResources();
    CreateDeviceResources();
}
```

La classe **DeviceResources** sépare cette fonctionnalité afin d’obtenir une meilleure réponse lors de changement d’environnement. Par exemple, elle appelle la méthode **CreateWindowSizeDependentResources** quand la taille de la fenêtre change.

###  <a name="initializing-the-direct2d-directwrite-and-wic-factories"></a>Initialisation des fabriques Direct2D, DirectWrite, et WIC

La méthode **DeviceResources::CreateDeviceIndependentResources** crée les fabriques pour Direct2D, DirectWrite et WIC. Dans les graphiques DirectX, les fabriques constituent le point de départ pour la création des ressources graphiques. Spécifie de Marble Maze **D2D1\_FACTORY\_TYPE\_unique\_thématique** , car il effectue tout le dessin sur le thread principal.

```cpp
// These are the resources required independent of hardware. 
void DX::DeviceResources::CreateDeviceIndependentResources()
{
    // Initialize Direct2D resources.
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
    // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    // Initialize the Direct2D Factory.
    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory2),
            &options,
            &m_d2dFactory
            )
        );

    // Initialize the DirectWrite Factory.
    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory2),
            &m_dwriteFactory
            )
        );

    // Initialize the Windows Imaging Component (WIC) Factory.
    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory2,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  <a name="creating-the-direct3d-and-direct2d-devices"></a>Création des appareils Direct3D et Direct2D

Le **DeviceResources::CreateDeviceResources** les appels de méthode [D3D11CreateDevice](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) pour créer l’objet de périphérique qui représente la carte graphique de Direct3D. Étant donné que labyrinthe de billes prend en charge le niveau de fonctionnalité 9.1 et versions ultérieures, le **DeviceResources::CreateDeviceResources** méthode spécifie les niveaux 9.1 via 11.1 dans le **featureLevels** tableau. Direct3D parcourt la liste dans l’ordre et donne à l’application le premier niveau de fonctionnalité disponible. Par conséquent le **D3D\_fonctionnalité\_niveau** entrées de tableau sont répertoriées du plus élevé au plus bas afin que l’application obtiendra le plus haut niveau de fonctionnalité disponible. La méthode **DeviceResources::CreateDeviceResources** obtient le périphérique Direct3D 11.1 en interrogeant le périphérique Direct3D 11 qui est renvoyé par **D3D11CreateDevice**.

```cpp
// This flag adds support for surfaces with a different color channel ordering
// than the API default. It is required for compatibility with Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
    if (DX::SdkLayersAvailable())
    {
        // If the project is in a debug build, enable debugging via SDK Layers 
        // with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
    }
#endif

// This array defines the set of DirectX hardware feature levels this app will support.
// Note the ordering should be preserved.
// Don't forget to declare your application's minimum required feature level in its
// description.  All applications are assumed to support 9.1 unless otherwise stated.
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
ComPtr<ID3D11DeviceContext> context;

HRESULT hr = D3D11CreateDevice(
    nullptr,                    // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,   // Create a device using the hardware graphics driver.
    0,                          // Should be 0 unless the driver is D3D_DRIVER_TYPE_SOFTWARE.
    creationFlags,              // Set debug and Direct2D compatibility flags.
    featureLevels,              // List of feature levels this app can support.
    ARRAYSIZE(featureLevels),   // Size of the list above.
    D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for UWP apps.
    &device,                    // Returns the Direct3D device created.
    &m_d3dFeatureLevel,         // Returns feature level of device created.
    &context                    // Returns the device immediate context.
    );

if (FAILED(hr))
{
    // If the initialization fails, fall back to the WARP device.
    // For more information on WARP, see:
    // https://go.microsoft.com/fwlink/?LinkId=286690
    DX::ThrowIfFailed(
        D3D11CreateDevice(
            nullptr,
            D3D_DRIVER_TYPE_WARP, // Create a WARP device instead of a hardware device.
            0,
            creationFlags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &device,
            &m_d3dFeatureLevel,
            &context
            )
        );
}

// Store pointers to the Direct3D 11.1 API device and immediate context.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );

DX::ThrowIfFailed(
    context.As(&m_d3dContext)
    );
```

La méthode **DeviceResources::CreateDeviceResources** crée ensuite le périphérique Direct2D. Direct2D utilise Microsoft DirectX Graphics Infrastructure (DXGI) pour interagir avec Direct3D. DXGI permet le partage de surfaces mémoire vidéo entre les runtime graphiques. Marble Maze utilise l’appareil DXGI sous-jacent de l’appareil Direct3D pour créer l’appareil Direct2D à partir de la fabrique Direct2D.

```cpp
// Create the Direct2D device object and a corresponding context.
ComPtr<IDXGIDevice3> dxgiDevice;
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

Pour plus d’informations sur DXGI et l’interopérabilité entre Direct2D et Direct3D, voir [Vue d’ensemble de DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi) et [Vue d’ensemble de l’interopérabilité Direct2D et Direct3D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-and-direct3d-interoperation-overview).

### <a name="associating-direct3d-with-the-view"></a>Association de Direct3D à la vue

La méthode **DeviceResources::CreateWindowSizeDependentResources** crée les ressources graphiques qui dépendent d’une taille de fenêtre donnée, telle que la chaîne d’échange et les cibles de rendu Direct3D et Direct2D. Une différence majeure entre une application DirectX UWP et une application de bureau est la façon dont la chaîne d’échange est associée à la fenêtre de sortie. Une chaîne d’échange est chargée d’afficher la mémoire tampon grâce à laquelle l’appareil donne son rendu à l’écran. [Structure de l’application labyrinthe de billes](marble-maze-application-structure.md) décrit comment le système de fenêtrage pour une application UWP diffère d’une application de bureau. Car une application UWP ne fonctionne pas avec [HWND](https://docs.microsoft.com/windows/desktop/WinProg/windows-data-types) objets, labyrinthe de billes doit utiliser le [IDXGIFactory2::CreateSwapChainForCoreWindow](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) méthode pour associer la sortie de l’appareil à la vue. L’exemple suivant montre la partie de la méthode **DeviceResources::CreateWindowSizeDependentResources** qui crée la chaîne d’échange.

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &m_swapChain
        )
    );
```

Pour réduire la consommation électrique, ce qui est importante d’effectuer sur les appareils tels que les ordinateurs portables et tablettes alimenté par batterie, le **DeviceResources::CreateWindowSizeDependentResources** les appels de méthode le [IDXGIDevice1 :: SetMaximumFrameLatency](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) pour s’assurer que le jeu est affiché uniquement après l’espace vertical. Synchronisation avec l’espace vertical est décrite plus en détail dans la section [présenter la scène](#presenting-the-scene) dans ce document.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both 
// reduces latency and ensures that the application will only render after each
// VSync, minimizing power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

La méthode **DeviceResources::CreateWindowSizeDependentResources** initialise les ressources graphiques d’une façon qui convient à la plupart des jeux.

> [!NOTE]
> Le terme *vue* a une signification différente dans le Runtime Windows qu’elle ne dans Direct3D. Dans le Windows Runtime, une vue fait référence à la collection de paramètres d’interface utilisateur d’une application, notamment la zone d’affichage et les comportements d’entrée, auxquels s’ajoute le thread qu’elle utilise pour le traitement. Vous spécifiez la configuration et les paramètres dont vous avez besoin quand vous créez une vue. Le processus de définition de la vue de l’application est décrit dans [Structure de l’application Marble Maze](marble-maze-application-structure.md).
> Dans Direct3D, le terme vue a plusieurs significations. Un affichage des ressources définit les sous-ressources qui permettre accéder à une ressource. Par exemple, quand un objet texture est associé à une vue de ressource de nuanceur, ce nuanceur peut ensuite accéder à la texture. L’un des avantages d’une vue de ressource est que vous pouvez interpréter des données de différentes manières à différentes étapes du pipeline de rendu. Pour plus d’informations sur les affichages de ressources, consultez [affichages de ressources](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-intro).
> Quand elle est utilisée dans le cadre d’une transformation de vue ou d’une matrice de transformation de vue, elle fait référence à l’emplacement et à l’orientation de la caméra. Une transformation de vue repositionne les objets dans l’espace autour de la position et de l’orientation de la caméra. Pour plus d’informations sur les transformations de vue, voir [Transformation de vue (Direct3D 9)](https://docs.microsoft.com/windows/desktop/direct3d9/view-transform). La façon dont Marble Maze utilise les vues de ressource et de matrice est décrite en détail dans cette rubrique.

 

## <a name="loading-scene-resources"></a>Chargement des ressources de scène


Jeu Marble Maze utilise le **BasicLoader** (classe), qui est déclarée dans **BasicLoader.h**, charger des textures et des nuanceurs. Jeu Marble Maze utilise le **SDKMesh** maillages de classe à charger la 3D pour le labyrinthe et le marbre.

Pour garantir la réactivité de l’application, Marble Maze charge les ressources de scène de façon asynchrone, ou en arrière-plan. Quand les composants sont chargés en arrière-plan, votre jeu peut répondre aux événements fenêtre. Ce processus est décrit en détail dans [Chargement des composants du jeu en arrière-plan](marble-maze-application-structure.md#loading-game-assets-in-the-background) dans ce guide.

###  <a name="loading-the-2d-overlay-and-user-interface"></a>Le chargement de l’interface utilisateur et de superposition 2D

Dans Marble Maze, la superposition est l’image qui s’affiche en haut de l’écran. La superposition s’affiche toujours sur le devant de la scène. Dans le labyrinthe de billes, la superposition contient le logo de Windows et la chaîne de texte **exemple de jeu DirectX Marble Maze**. La gestion de la superposition est effectuée par le **SampleOverlay** (classe), qui est défini dans **SampleOverlay.h**. Bien que nous utilisions la superposition dans le cadre des exemples Direct3D, vous pouvez adapter ce code pour afficher des images sur le devant de votre scène.

Un aspect important de la superposition est que, puisque son contenu ne change pas, le **SampleOverlay** classe dessine, ou des caches, son contenu dans un [ID2D1Bitmap1](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1bitmap1) objet pendant l’initialisation. Au moment du dessin, la classe **SampleOverlay** n’a plus qu’à dessiner l’image bitmap à l’écran. De cette façon, les routines gourmandes en ressources, telles que le dessin de texte ne sont pas exécutées à chaque image.

L’interface utilisateur (IU) est constitué des composants 2D, tels que des menus et des affichages tête haute (HUDs), qui apparaissent devant votre scène. Marble Maze définit les éléments d’interface suivants :

-   Les éléments de menu qui permettent à l’utilisateur de démarrer le jeu et de voir les meilleurs scores.
-   Un minuteur qui compte jusqu’à trois avant de lancer le jeu.
-   Un minuteur qui compte le temps de jeu écoulé.
-   Un tableau qui répertorie les meilleurs temps.
-   Texte qui lit **suspendu** lorsque le jeu est suspendu.

Marble Maze définit les éléments d’interface utilisateur spécifiques de jeu dans **UserInterface.h**. Marble Maze définit la classe **ElementBase** comme type de base pour tous les éléments d’interface utilisateur. La classe **ElementBase** définit les attributs tels que la taille, la position, l’alignement et la visibilité d’un élément d’interface utilisateur. Elle contrôle également la mise à jour et le rendu des éléments.

```cpp
class ElementBase
{
public:
    virtual void Initialize() { }
    virtual void Update(float timeTotal, float timeDelta) { }
    virtual void Render() { }

    void SetAlignment(AlignType horizontal, AlignType vertical);
    virtual void SetContainer(const D2D1_RECT_F& container);
    void SetVisible(bool visible);

    D2D1_RECT_F GetBounds();

    bool IsVisible() const { return m_visible; }

protected:
    ElementBase();

    virtual void CalculateSize() { }

    Alignment       m_alignment;
    D2D1_RECT_F     m_container;
    D2D1_SIZE_F     m_size;
    bool            m_visible;
};
```

En fournissant une classe de base commune pour tous les éléments d’interface utilisateur, la classe **UserInterface**, qui gère l’interface utilisateur, n’a besoin de contenir qu’une seule collection d’objets **ElementBase**, ce qui simplifie la gestion de l’interface utilisateur et fournit un gestionnaire d’interface utilisateur réutilisable. Marble Maze définit les types qui dérivent de **ElementBase** qui implémentent les comportements propres au jeu. Par exemple, **HighScoreTable** définit le comportement du tableau des meilleurs scores. Pour plus d’informations sur ces types, reportez-vous au code source.

> [!NOTE]
> Étant donné que XAML vous permet de créer plus facilement des interfaces utilisateur complexes, telles que celles de simulation et jeux de stratégie, pensez à utiliser XAML pour définir votre interface utilisateur. Pour plus d’informations sur le développement d’une interface utilisateur dans XAML dans un jeu DirectX UWP, consultez [étendre l’exemple de jeu](tutorial-resources.md), qui fait référence à la 3D DirectX, exemple de jeu de tir.

 

###  <a name="loading-shaders"></a>Chargement des nuanceurs

Marble Maze utilise la méthode **BasicLoader::LoadShader** pour charger un nuanceur à partir d’un fichier.

Les nuanceurs sont les unités fondamentales de la programmation GPU des jeux modernes. Presque tout le traitement des graphiques 3D est conduit à travers les nuanceurs, s’il s’agit de transformation du modèle et éclairage de la scène ou géométrie plus complexe de traitement, à partir du caractère définir l’apparence de pavage. Pour plus d’informations sur le modèle de programmation de nuanceur, voir [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl).

Marble Maze utilise les nuanceurs de vertex et de pixels. Un nuanceur de vertex s’exécute sur un vertex d’entrée et produit un vertex en sortie. Un nuanceur de pixels utilise des valeurs numériques, des données de texture, des valeurs par vertex interpolées et d’autres données pour produire une couleur de pixel en sortie. Dans la mesure où un nuanceur transforme un élément à la fois, le matériel graphique qui fournit plusieurs pipelines de nuanceur peut traiter en parallèle des ensembles d’éléments. Le nombre de pipelines parallèles disponible au GPU peut être bien supérieur au nombre disponible à l’UC. C’est pourquoi, mêmes des nuanceurs de base peuvent améliorer de façon notable la sortie.

Le **MarbleMazeMain::LoadDeferredResources** méthode charge nuanceur d’un sommets et le nuanceur de pixels après avoir chargé la superposition. Les versions de conception de ces nuanceurs sont définies dans **BasicVertexShader.hlsl** et **BasicPixelShader.hlsl**, respectivement. Marble Maze applique ces nuanceurs à la bille et au labyrinthe lors de la phase de rendu.

Le projet Marble Maze comprend les versions .hlsl (format au moment de la conception) et .cso (format au moment de l’exécution) des fichiers du nuanceur. Lors de la compilation, Visual Studio utilise le compilateur d’effet fxc.exe pour compiler votre fichier source .hlsl en nuanceur binaire .cso. Pour plus d’informations sur l’outil compilateur d’effets, voir [Outil compilateur d’effets](https://docs.microsoft.com/windows/desktop/direct3dtools/fxc).

Le nuanceur de vertex utilise le modèle fourni, les matrices d’affichage et de projection pour transformer la géométrie d’entrée. Les données de position de la géométrie d’entrée sont transformées et rendues deux fois : une fois dans l’espace écran, ce qui est nécessaire pour le rendu et de nouveau dans l’espace du monde pour permettre au nuanceur de pixels d’effectuer des calculs d’éclairage. Le vecteur normal de surface est transformé en espace du monde, qui est également utilisé par le nuanceur de pixels pour l’éclairage. Les coordonnées de texture sont passées sans modification au nuanceur de pixels.

```hlsl
sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

Le nuanceur de pixels reçoit la sortie du nuanceur de vertex en entrée. Ce nuanceur calcule l’éclairage pour reproduire une lumière tamisée qui surplombe le labyrinthe et suit la position de la bille. L’éclairage est plus fort pour les surfaces qui pointent directement vers la lumière. Le composant de diffusion décroît progressivement vers zéro au fur et à mesure que le vecteur normal devient perpendiculaire à la lumière, et le terme ambiant diminue quand le vecteur s’éloigne de la lumière. Les points les plus proches de la bille (et donc plus proches du centre du projecteur) sont plus éclairés. Cependant, l’éclairage est modulé pour les points situés sous la bille afin de donner une ombre. Dans un environnement réel, un objet tel que la bille blanche réfléchirait de manière diffuse le projecteur sur d’autres objets de la scène. Une approximation est obtenue pour les surfaces du côté brillant de la bille. Les facteurs d’illumination supplémentaires sont relatifs à l’angle et à la distance par rapport à la bille. La couleur de pixel qui est obtenue est une composition de la texture échantillonnée et du résultat des calculs d’éclairage.

```hlsl
float4 main(sPSInput input) : SV_TARGET
{
    float3 lightDirection = float3(0, 0, -1);
    float3 ambientColor = float3(0.43, 0.31, 0.24);
    float3 lightColor = 1 - ambientColor;
    float spotRadius = 50;

    // Basic ambient (Ka) and diffuse (Kd) lighting from above.
    float3 N = normalize(input.norm);
    float NdotL = dot(N, lightDirection);
    float Ka = saturate(NdotL + 1);
    float Kd = saturate(NdotL);

    // Spotlight.
    float3 vec = input.worldPos - marblePosition;
    float dist2D = sqrt(dot(vec.xy, vec.xy));
    Kd = Kd * saturate(spotRadius / dist2D);

    // Shadowing from ball.
    if (input.worldPos.z > marblePosition.z)
        Kd = Kd * saturate(dist2D / (marbleRadius * 1.5));

    // Diffuse reflection of light off ball.
    float dist3D = sqrt(dot(vec, vec));
    float3 V = normalize(vec);
    Kd += saturate(dot(-V, N)) * saturate(dot(V, lightDirection))
        * saturate(marbleRadius / dist3D);

    // Final composite.
    float4 diffuseTexture = Texture.Sample(Sampler, input.tex);
    float3 color = diffuseTexture.rgb * ((ambientColor * Ka) + (lightColor * Kd));
    return float4(color * lightStrength, diffuseTexture.a);
}
```

> [!WARNING]
> Le nuanceur de pixels compilé contient les instructions arithmétiques 32 et les instructions de texture 1. Ce nuanceur donne de bons résultats sur les ordinateurs de bureau et les tablettes haut de gamme. Cependant, un ordinateur moins puissant peut ne pas être en mesure de traiter ce nuanceur, mais fournir quand même une fréquence d’images interactive. Ciblez du matériel de base et concevez vos nuanceurs pour qu’ils s’adaptent à ce matériel.

 

Le **MarbleMazeMain::LoadDeferredResources** méthode utilise le **BasicLoader::LoadShader** méthode pour charger les nuanceurs. L’exemple suivant charge le nuanceur de vertex. Le format de l’exécution de ce nuanceur est **BasicVertexShader.cso**. Le **m\_vertexShader** variable membre est un [ID3D11VertexShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader) objet.

```cpp
BasicLoader^ loader = ref new BasicLoader(m_deviceResources->GetD3DDevice());

D3D11_INPUT_ELEMENT_DESC layoutDesc [] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
m_vertexStride = 44; // must set this to match the size of layoutDesc above

Platform::String^ vertexShaderName = L"BasicVertexShader.cso";
loader->LoadShader(
    vertexShaderName,
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

Le **m\_inputLayout** variable membre est un [ID3D11InputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout) objet. L’objet de disposition d’entrée encapsule l’état d’entrée de l’étape d’assembleur d’entrée (IA). L’un des rôles de l’étape IA est d’améliorer l’efficacité des nuanceurs en utilisant des valeurs générées par le système, également appelées *sémantiques*, pour traiter uniquement les primitives ou les vertex qui n’ont pas encore été traités.

Utilisez la méthode [ID3D11Device::CreateInputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) pour créer une disposition d’entrée à partir d’un tableau de descriptions d’élément d’entrée. Ce tableau contient un ou plusieurs éléments d’entrée, chaque élément d’entrée décrit un élément de données de vertex à partir d’une mémoire tampon vertex. L’ensemble des descriptions d’élément d’entrée décrit tous les éléments de données de vertex de toutes les mémoires tampon de vertex qui seront liées à l’étape IA. 

**layoutDesc** dans le code ci-dessus extrait de code affiche la description de la mise en page que labyrinthe de billes utilise. La description de disposition décrit une mémoire tampon de vertex qui contient quatre éléments de données de vertex. Les éléments importants de chaque entrée dans le tableau sont le nom de la sémantique, le format de données et le décalage d’octet . Par exemple, l’élément **POSITION** spécifie la position du vertex dans l’espace d’objets. Il commence au décalage d’octet 0 et contient trois composants à virgule flottante (pour un total de 12 octets). L’élément **NORMAL** spécifie le vecteur normal. Il commence au décalage d’octet 12, car il s’affiche directement après **POSITION** dans la disposition, qui nécessite 12 octets. L’élément **NORMAL** contient un entier non signé 32 bits de quatre composants.

Comparez la disposition d’entrée avec la structure **sVSInput** définie par le nuanceur de vertex, comme indiqué dans l’exemple suivant. La structure **sVSInput** définit les éléments **POSITION**, **NORMAL** et **TEXCOORD0**. Le runtime DirectX mappe chaque élément de la disposition à la structure d’entrée définie par le nuanceur.

```hlsl
struct sVSInput
{
    float3 pos : POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
};

struct sPSInput
{
    float4 pos : SV_POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
    float3 worldPos : TEXCOORD1;
};

sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

Le document [Sémantiques](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) décrit chaque sémantique disponible en détail.

> [!NOTE]
> Dans une disposition, vous pouvez spécifier des composants supplémentaires qui ne sont pas utilisés pour activer plusieurs nuanceurs de partager la même disposition. Par exemple, l’élément **TANGENT** n’est pas utilisé par le nuanceur. Vous pouvez utiliser l’élément **TANGENT** si vous voulez tester des techniques, par exemple le mappage normal. En utilisant le mappage normal, également appelé placage de relief (bump mapping), vous pouvez créer l’effet de relief sur les surfaces des objets. Pour plus d’informations sur le placage de relief, voir [Placage de relief (Direct3D 9)](https://docs.microsoft.com/windows/desktop/direct3d9/bump-mapping).

 

Pour plus d’informations sur la phase d’assemblage d’entrée, consultez [étape de l’assembleur d’entrée](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage) et [mise en route avec l’étape de l’assembleur d’entrée](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage-getting-started).

Le processus d’utilisation de nuanceurs de vertex et de pixels pour le rendu de la scène est décrit dans la section [Rendu de la scène](#rendering-the-scene) plus loin dans ce document.

### <a name="creating-the-constant-buffer"></a>Création de la mémoire tampon constante

Direct3D met en mémoire tampon des groupes d’ensemble de données. Une mémoire tampon constante est un type de mémoire tampon que vous utilisez pour passer des données aux nuanceurs. Marble Maze utilise une mémoire tampon pour contenir la vue du modèle (ou monde) et les matrices de projection pour l’objet de scène actif.

L’exemple suivant montre comment la **MarbleMazeMain::LoadDeferredResources** méthode crée un mémoire tampon constante qui conserve les données de matrice plus tard. L’exemple crée un **D3D11\_tampon\_DESC** structure qui utilise le **D3D11\_lier\_constante\_tampon** indicateur spécifier l’utilisation comme une mémoire tampon constante. Cet exemple transmet ensuite cette structure à la méthode [ID3D11Device::CreateBuffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer). Le **m\_constantBuffer** variable est un [ID3D11Buffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer) objet.

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};

// Multiple of 16 bytes
constantBufferDesc.ByteWidth = ((sizeof(ConstantBuffer) + 15) / 16) * 16;

constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;

// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &constantBufferDesc,
        nullptr,    // leave the buffer uninitialized
        &m_constantBuffer
        )
    );
```

Le **MarbleMazeMain::Update** méthode met à jour plus tard **ConstantBuffer** objets, un pour le labyrinthe et l’autre pour le marbre. Le **MarbleMazeMain::Render** méthode lie ensuite chaque **ConstantBuffer** objet à la mémoire tampon constante avant le rendu de chaque objet. L’exemple suivant montre le **ConstantBuffer** structure, ce qui se trouve dans **MarbleMazeMain.h**.

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;

    XMFLOAT3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

Pour mieux comprendre comment constante met en mémoire tampon mappage au code de nuanceur, comparez le **ConstantBuffer** structure dans **MarbleMazeMain.h** à la **ConstantBuffer** mémoire tampon constante qui est définie par le nuanceur de sommets dans **BasicVertexShader.hlsl**:

```hlsl
cbuffer ConstantBuffer : register(b0)
{
    matrix model;
    matrix view;
    matrix projection;
    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

La disposition de la structure **ConstantBuffer** correspond à l’objet **cbuffer**. La variable **cbuffer** spécifie register b0, ce qui signifie que les données de la mémoire tampon constante sont stockées dans le registre 0. Le **MarbleMazeMain::Render** méthode spécifie inscrire 0 lorsqu’il active la mémoire tampon constante. Ce processus est décrit en détail plus loin dans ce document.

Pour plus d’informations sur les mémoires tampons constantes, voir [Présentation des mémoires tampons dans Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro). Pour plus d’informations sur le mot clé register, voir [register](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-register).

###  <a name="loading-meshes"></a>Chargement des maillages

Marble Maze utilise SDK-Mesh comme format d’exécution car ce format fournit une méthode de base pour charger les données de maillage pour les applications d’exemple. Pour une utilisation en production, utilisez un format de maillage qui correspond aux exigences spécifiques à votre jeu.

Le **MarbleMazeMain::LoadDeferredResources** chargements de méthodes de maillage données après avoir chargé les nuanceurs de sommets et de pixels. Un maillage est une collection de données vertex qui comprend souvent des informations sur les positions, les données normales, les couleurs, les matériaux et les coordonnées de texture. Mailles sont généralement créées en 3D logiciel de création et conservées dans les fichiers qui sont séparées du code d’application. La bille et le labyrinthe sont deux exemples de maillages utilisés par le jeu.

Marble Maze utilise la classe **SDKMesh** pour gérer les maillages. Cette classe est déclarée dans **SDKMesh.h**. **SDKMesh** fournit les méthodes pour charger, afficher et supprimer les données de maillage.

> [!IMPORTANT]
> Marble Maze utilise le format de la maille de kit de développement logiciel et fournit le **SDKMesh** classe à titre d’illustration uniquement. Bien que le format SDK-Mesh soit utile pour l’apprentissage et la création de prototypes, il s’agit d’un format de base qui peut ne pas convenir au développement de jeu spécifique. Nous recommandons d’utiliser un format de maillage qui correspond aux exigences spécifiques à votre jeu.

 

L’exemple suivant montre comment la **MarbleMazeMain::LoadDeferredResources** méthode utilise le **SDKMesh::Create** méthode pour charger des données pour le labyrinthe et pour la boule de maillage.

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\marble2.sdkmesh",
        false
        )
    );
```

###  <a name="loading-collision-data"></a>Chargement des données de collision

Bien que cette section ne décrive pas en détail l’implémentation de la simulation physique entre la bille et le labyrinthe dans Marble Maze, la géométrie de maillage pour le système physique est lue quand les maillages sont chargés.

```cpp
// Extract mesh geometry for the physics system.
DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_walls",
        m_collision.m_wallTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_Floor",
        m_collision.m_groundTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_floorSides",
        m_collision.m_floorTriList
        )
    );

m_physics.SetCollision(&m_collision);
float radius = m_marbleMesh.GetMeshBoundingBoxExtents(0).x / 2;
m_physics.SetRadius(radius);
```

La façon que vous chargez en grande partie des données de collision varie selon le format de l’exécution que vous utilisez. Pour plus d’informations sur comment labyrinthe de billes charge la géométrie de collision à partir d’un fichier de maillage de kit de développement logiciel, consultez le **MarbleMazeMain::ExtractTrianglesFromMesh** méthode dans le code source.

## <a name="updating-game-state"></a>Mise à jour de l’état du jeu


Marble Maze sépare la logique du jeu et la logique de rendu en mettant à jour tous les objets de scène avant de les afficher.

[Structure de l’application labyrinthe de billes](marble-maze-application-structure.md) décrit la boucle de jeu principale. La mise à jour de la scène, qui fait partie de la boucle du jeu, se produit après le traitement des événements de fenêtrage et des entrées et avant le rendu de la scène. Le **MarbleMazeMain::Update** méthode gère la mise à jour de l’interface utilisateur et le jeu.

### <a name="updating-the-user-interface"></a>Mise à jour de l’interface utilisateur

Le **MarbleMazeMain::Update** les appels de méthode le **UserInterface::Update** méthode pour mettre à jour l’état de l’interface utilisateur.

```cpp
UserInterface::GetInstance().Update(
    static_cast<float>(m_timer.GetTotalSeconds()), 
    static_cast<float>(m_timer.GetElapsedSeconds()));
```

La méthode **UserInterface::Update** met à jour chaque élément dans la collection d’interface utilisateur.

```cpp
void UserInterface::Update(float timeTotal, float timeDelta)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        (*iter)->Update(timeTotal, timeDelta);
    }
}
```

Les classes qui dérivent de **ElementBase** (défini dans **UserInterface.h**) implémentent le **mise à jour** méthode pour effectuer des comportements spécifiques. Par exemple, la méthode **StopwatchTimer::Update** met à jour le temps écoulé en fonction de la valeur fournie et met à jour le texte qui est ensuite affiché.

```cpp
void StopwatchTimer::Update(float timeTotal, float timeDelta)
{
    if (m_active)
    {
        m_elapsedTime += timeDelta;

        WCHAR buffer[16];
        GetFormattedTime(buffer);
        SetText(buffer);
    }

    TextElement::Update(timeTotal, timeDelta);
}
```

###  <a name="updating-the-scene"></a>Mise à jour de la scène

Le **MarbleMazeMain::Update** méthode met à jour le jeu en fonction de l’état actuel de l’ordinateur d’état (le **GameState**, stockée dans **m_gameState**). Lorsque le jeu est dans un état actif (**GameState::InGameActive**), met à jour de l’appareil photo pour suivre le marbre labyrinthe de billes, met à jour de la partie de matrice de vue des mémoires tampons constantes et met à jour la simulation physique.

L’exemple suivant montre comment la **MarbleMazeMain::Update** méthode met à jour la position de la caméra. Jeu Marble Maze utilise le **m\_resetCamera** variable indicateur que l’appareil photo doit être réinitialisé pour être situé directement au-dessus le marbre. La caméra est réinitialisée quand le jeu commence ou la bille tombe du labyrinthe. Quand l’écran du menu principal ou des meilleurs scores est actif, la caméra est définie à un emplacement constant. Dans le cas contraire, Marble Maze utilise le paramètre *timeDelta* pour interpoler la position de la caméra entre ses positions actuelle et cible. La position cible est légèrement au-dessus et devant la bille. L’utilisation du temps écoulé permet à la caméra de suivre la bille.

```cpp
static float eyeDistance = 200.0f;
static XMFLOAT3A eyePosition = XMFLOAT3A(0, 0, 0);

// Gradually move the camera above the marble.
XMFLOAT3A targetEyePosition;
XMStoreFloat3A(
    &targetEyePosition, 
    XMLoadFloat3A(&marblePosition) - (XMLoadFloat3A(&g) * eyeDistance));

if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&eyePosition) 
            + ((XMLoadFloat3A(&targetEyePosition) - XMLoadFloat3A(&eyePosition)) 
                * min(1, static_cast<float>(m_timer.GetElapsedSeconds()) * 8)
            )
    );
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&marblePosition) + XMVectorSet(75.0f, -150.0f, -75.0f, 0.0f));

    m_camera->SetViewParameters(
        eyePosition, 
        marblePosition, 
        XMFLOAT3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, XMFLOAT3(0.0f, 1.0f, 0.0f));
}
```

L’exemple suivant montre comment la **MarbleMazeMain::Update** méthode met à jour les mémoires tampons constantes pour le marbre et labyrinthe. La matrice de modèle, ou monde, du labyrinthe est toujours la matrice d’identité. Sauf pour la diagonale principale, dont les éléments sont des uns, la matrice d’identité est une matrice carrée composée de zéros. La matrice de modèle de la bille est basée sur sa matrice de position que multiplie sa matrice de rotation.

```cpp
// Update the model matrices based on the simulation.
XMStoreFloat4x4(&m_mazeConstantBufferData.model, XMMatrixIdentity());

XMStoreFloat4x4(
    &m_marbleConstantBufferData.model, 
    XMMatrixTranspose(
        XMMatrixMultiply(
            marbleRotationMatrix, 
            XMMatrixTranslationFromVector(XMLoadFloat3A(&marblePosition))
        )
    )
);

// Update the view matrix based on the camera.
XMFLOAT4X4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

Pour plus d’informations sur la façon dont **MarbleMazeMain::Update** méthode lit l’entrée de l’utilisateur et simule le mouvement du marbre, consultez [entrée de l’ajout et l’interactivité à l’exemple de labyrinthe de billes](adding-input-and-interactivity-to-the-marble-maze-sample.md).

## <a name="rendering-the-scene"></a>Rendu de la scène


En règle générale, les étapes suivantes sont comprises dans le rendu d’une scène.

1.  Définir la mémoire tampon du gabarit/profondeur cible de rendu actif.
2.  Effacer les vues de rendu et de gabarit.
3.  Préparer les nuanceurs de vertex et de pixels pour le dessin.
4.  Afficher les objets dans la scène 3D.
5.  Afficher n’importe quel objet 2D qui vous souhaitez voir apparaître devant la scène.
6.  Présenter l’image rendue à l’écran.

Le **MarbleMazeMain::Render** méthode lie la cible de rendu et stencil de profondeur vues, efface ces vues, dessine la scène, puis dessine la superposition.

###  <a name="preparing-the-render-targets"></a>Préparation des cibles de rendu

Avant d’effectuer le rendu de votre scène, vous devez définir la mémoire tampon de gabarit/profondeur cible de rendu active. Si votre scène ne doit pas dessiner sur chaque pixel de l’écran, effacez également les vues de rendu et de gabarit. Marble Maze efface les vues de rendu et de gabarit sur chaque image afin qu’aucun objet de l’image précédente ne soit visible.

L’exemple suivant montre comment la **MarbleMazeMain::Render** les appels de méthode le [ID3D11DeviceContext::OMSetRenderTargets](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) méthode pour définir la cible de rendu et de la mémoire tampon stencil de profondeur en cours celles.

```cpp
auto context = m_deviceResources->GetD3DDeviceContext();

// Reset the viewport to target the whole screen.
auto viewport = m_deviceResources->GetScreenViewport();
context->RSSetViewports(1, &viewport);

// Reset render targets to the screen.
ID3D11RenderTargetView *const targets[1] = 
    { m_deviceResources->GetBackBufferRenderTargetView() };

context->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

// Clear the back buffer and depth stencil view.
context->ClearRenderTargetView(
    m_deviceResources->GetBackBufferRenderTargetView(), 
    DirectX::Colors::Black);

context->ClearDepthStencilView(
    m_deviceResources->GetDepthStencilView(), 
    D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 
    1.0f, 
    0);
```

Les interfaces [ID3D11RenderTargetView](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) et [ID3D11DepthStencilView](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) prennent en charge le mécanisme de vue de texture fourni par Direct3D 10 et les versions ultérieures. Pour plus d’informations sur les vues de texture, voir [Vues de texture (Direct3D 10)](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-access-views). La méthode [OMSetRenderTargets](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) prépare l’étape de fusion/sortie du pipeline Direct3D. Pour plus d’informations sur l’étape de fusion/sortie, voir [Étape de fusion/sortie](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage).

### <a name="preparing-the-vertex-and-pixel-shaders"></a>Création du nuanceur de vertex et du nuanceur de pixels

Avant d’effectuer le rendu des objets de scène, exécutez les étapes suivantes pour préparer les nuanceurs de vertex et de pixels pour le dessin :

1.  Définissez la disposition d’entrée du nuanceur comme disposition active.
2.  Définissez les nuanceurs de vertex et de pixels comme nuanceurs actifs.
3.  Mettez à jour les mémoires tampon constantes avec les données que vous devez transmettre aux nuanceurs.

> [!IMPORTANT]
> Jeu Marble Maze utilise une paire de nuanceurs de sommets et de pixels pour tous les objets 3D. Si votre jeu utilise plus d’une paire de nuanceurs, vous devez effectuer ces étapes chaque fois que vous dessinez des objets qui utilisent des nuanceurs différents. Pour réduire la charge associée à la modification de l’état du nuanceur, nous recommandons de grouper les appels de rendu pour tous les objets qui utilisent les mêmes nuanceurs.

 

La section [Chargement des nuanceurs](#loading-shaders) de ce document décrit comment la disposition d’entrée est créée quand le nuanceur de vertex est créé. L’exemple suivant montre comment la **MarbleMazeMain::Render** méthode utilise le [ID3D11DeviceContext::IASetInputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) méthode pour définir cette disposition en tant que la disposition actuelle.

```cpp
m_deviceResources->GetD3DDeviceContext()->IASetInputLayout(m_inputLayout.Get());
```

L’exemple suivant montre comment la **MarbleMazeMain::Render** méthode utilise le [ID3D11DeviceContext::VSSetShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) et [ID3D11DeviceContext::PSSetShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) méthodes permettant de définir les nuanceurs de sommets et de pixels comme les nuanceurs en cours, respectivement.

```cpp
// Set the vertex shader stage state.
m_deviceResources->GetD3DDeviceContext()->VSSetShader(
    m_vertexShader.Get(),   // use this vertex shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetShader(
    m_pixelShader.Get(),    // use this pixel shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetSamplers(
    0,                          // starting at the first sampler slot
    1,                          // set one sampler binding
    m_sampler.GetAddressOf());  // to use this sampler
```

Après avoir **MarbleMazeMain::Render** définit les nuanceurs et leur disposition d’entrée, il utilise le [ID3D11DeviceContext::UpdateSubresource](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) méthode pour mettre à jour de la mémoire tampon constante avec le modèle, vue, et matrices de projection pour le labyrinthe. La méthode **UpdateSubresource** copie les données de matrice de la mémoire UC vers la mémoire GPU. N’oubliez pas que les composants de modèle et la vue de la **ConstantBuffer** structure sont mis à jour dans le **MarbleMazeMain::Update** (méthode). Le **MarbleMazeMain::Render** méthode appelle ensuite la [ID3D11DeviceContext::VSSetConstantBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) et [ID3D11DeviceContext::PSSetConstantBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers) méthodes à Définissez cette mémoire tampon constante comme celle en cours.

```cpp
// Update the constant buffer with the new data.
m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0);

m_deviceResources->GetD3DDeviceContext()->VSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer

m_deviceResources->GetD3DDeviceContext()->PSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer
```

Le **MarbleMazeMain::Render** méthode effectue des étapes similaires pour préparer le marbre doit être restitué.

### <a name="rendering-the-maze-and-the-marble"></a>Rendu du labyrinthe et de la bille

Après avoir activé les nuanceurs actifs, vous pouvez dessiner les objets de votre scène. Le **MarbleMazeMain::Render** les appels de méthode le **SDKMesh::Render** méthode pour afficher le maillage labyrinthe.

```cpp
m_mazeMesh.Render(
    m_deviceResources->GetD3DDeviceContext(), 
    0, 
    INVALID_SAMPLER_SLOT, 
    INVALID_SAMPLER_SLOT);
```

Le **MarbleMazeMain::Render** méthode exécute une procédure similaire pour restituer le marbre.

Comme indiqué précédemment dans ce document, la classe **SDKMesh** est fournie à des fins de démonstration, mais nous ne recommandons pas son utilisation pour la production d’un jeu de qualité. Toutefois, notez que le **SDKMesh::RenderMesh** (méthode), qui est appelée par **SDKMesh::Render**, utilise le [ID3D11DeviceContext::IASetVertexBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) et [ID3D11DeviceContext::IASetIndexBuffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) méthodes pour définir le vertex actuel et les tampons d’index qui définissent la maille, et le [ID3D11DeviceContext::DrawIndexed](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexedinstanced) méthode pour dessiner les mémoires tampons. Pour plus d’informations sur l’utilisation des mémoires tampons de vertex et d’index, voir [Présentation des mémoires tampons dans Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro).

### <a name="drawing-the-user-interface-and-overlay"></a>Dessin de l’interface utilisateur et de la superposition

Après avoir dessiné des objets de scène 3D, labyrinthe de billes dessine les éléments d’interface utilisateur 2D qui apparaissent devant la scène.

Le **MarbleMazeMain::Render** méthode se termine en dessinant l’interface utilisateur et la superposition.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render(m_deviceResources->GetOrientationTransform2D());

m_deviceResources->GetD3DDeviceContext()->BeginEventInt(L"Render Overlay", 0);
m_sampleOverlay->Render();
m_deviceResources->GetD3DDeviceContext()->EndEvent();
```

Le **UserInterface::Render** méthode utilise un [ID2D1DeviceContext](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) objet sur lequel dessiner les éléments d’interface utilisateur. Cette méthode définit l’état de dessin, dessine tous les éléments d’interface utilisateur actif, puis rétablit l’état de dessin précédent.

```cpp
void UserInterface::Render(D2D1::Matrix3x2F orientation2D)
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(orientation2D);

    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

La méthode **SampleOverlay::Render** utilise une technique identique pour dessiner l’image bitmap de superposition.

###  <a name="presenting-the-scene"></a>Présentation de la scène

Après le dessin 2D et 3D de tous les objets de scène, labyrinthe de billes présente l’image rendue à l’analyse. Il synchronise le dessin sur le vide vertical afin que votre jeu ne dessine pas des images qui ne sont jamais affichées. Marble Maze gère également les changements d’appareils quand il présente la scène.

Après le **MarbleMazeMain::Render** méthode est retournée, les appels de boucle du jeu le **DX::DeviceResources::Present** méthode pour envoyer l’image rendue à l’analyse ou l’afficher. Le **DX::DeviceResources::Present** les appels de méthode [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) pour effectuer l’opération actuelle, comme indiqué dans l’exemple suivant :

```cpp
// The first argument instructs DXGI to block until VSync, putting the application
// to sleep until the next VSync. This ensures we don't waste any cycles rendering
// frames that will never be displayed to the screen.
HRESULT hr = m_swapChain->Present(1, 0);
```

Dans cet exemple, **m\_swapChain** est un [IDXGISwapChain1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) objet. L’initialisation de cet objet est décrite dans la section [Initialisation Direct3D et Direct2D](#initializing-direct3d-and-direct2d) de ce document.

Le premier paramètre de [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1), *SyncInterval*, spécifie le nombre d’espaces verticales à attendre avant de présenter le frame. Marble Maze indique 1 afin d’attendre le vide vertical suivant.

Le [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) méthode retourne un code d’erreur qui indique que l’appareil a été supprimé ou sinon échec. Dans ce cas, Marble Maze réinitialise le périphérique.

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

## <a name="next-steps"></a>Étapes suivantes


Pour plus d’informations sur les pratiques clés en matière de périphériques d’entrée, voir [Ajout d’entrées et d’interactivité à l’exemple Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md). Ce document explique comment labyrinthe de billes prend en charge tactile, accéléromètre, contrôleurs de Xbox et entrée de la souris.

## <a name="related-topics"></a>Rubriques connexes


* [Ajout d’entrée et d’interactivité à l’exemple de labyrinthe de billes](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Structure de l’application labyrinthe de billes](marble-maze-application-structure.md)
* [Développement du labyrinthe de billes, un jeu UWP en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




