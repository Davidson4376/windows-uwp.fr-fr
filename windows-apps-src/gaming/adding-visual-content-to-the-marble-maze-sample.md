---
author: mtoepke
title: Ajout de contenu visuel à l’exemple Marble Maze
description: Ce document décrit comment le jeu Marble Maze utilise Direct3D et Direct2D dans l’environnement d’application de plateforme Windows universelle (UWP) afin de vous présenter ces modèles et de vous aider à les adapter à vos propres jeux.
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, jeux, exemple, directx, graphisme
ms.openlocfilehash: b8ee07dc45e53f2ea73f87111fa9eb155854f10a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.locfileid: "230722"
---
# <a name="adding-visual-content-to-the-marble-maze-sample"></a>Ajout de contenu visuel à l’exemple Marble Maze


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Ce document décrit comment le jeu Marble Maze utilise Direct3D et Direct2D dans l’environnement d’application de plateforme Windows universelle (UWP) afin de vous présenter ces modèles et de vous aider à les adapter à vos propres jeux. Pour apprendre la place des composants visuels dans la structure globale de l’application Marble Maze, voir [Structure de l’application Marble Maze](marble-maze-application-structure.md).

Nous avons suivi les étapes indiquées ci-dessous lors du développement des aspects visuels de Marble Maze :

1.  Créer une infrastructure de base qui initialise les environnements Direct3D et Direct2D.
2.  Utiliser des programmes d’édition de modèles et d’image pour concevoir les éléments 2D et 3D du jeu.
3.  Garantir le chargement et l’affichage correct des éléments 2D et 3D dans le jeu.
4.  Intégrer des nuanceurs de vertex et de pixels qui améliorent la qualité visuelle des éléments du jeu.
5.  Intégrer la logique du jeu, par exemple l’animation et les entrées utilisateur.

Nous nous sommes tout d’abord concentrés sur l’ajout d’éléments 3D, puis d’éléments 2D. Par exemple, nous avons commencé par la logique principale du jeu avant d’ajouter le système de menus et le minuteur.

Nous devions également répéter plusieurs fois ces étapes pendant le processus de développement. Par exemple, lors de modifications des modèles de maillage et de bille, nous avons dû également modifier le code du nuanceur qui prend en charge ces modèles.

> **Remarque**   L’exemple de code correspondant à ce document est disponible dans l’[exemple de jeu Marble Maze en DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
Voici quelques-uns des éléments clés de ce document qui décrivent l’utilisation de DirectX et de contenu de jeu visuel, notamment l’initialisation des bibliothèques de graphiques DirectX, le chargement des ressources de scène et la mise à jour et le rendu de la scène:

-   L’ajout de contenu de jeu comprend de nombreuses étapes. Ces étapes doivent souvent être itérées. Les développeurs de jeu commencent par ajouter du contenu 3D avant d’ajouter du contenu 2D.
-   Plus vous prenez en charge des matériels graphiques différents, plus vous êtes en mesure de fournir une expérience de jeu de grande qualité à différents types d’utilisateur.
-   Séparez les formats au moment de la conception et au moment de l’exécution. Structurez vos éléments de conception pour une flexibilité maximum et pour permettre des itérations rapides sur le contenu. Mettez en forme et compressez vos éléments pour qu’ils soient chargés et affichés aussi rapidement que possible au moment de l’exécution.
-   Vous créez des appareils Direct3D et Direct2D dans une application pour UWP tout comme dans une application de bureau Windows classique. Une différence réside dans l’association de la chaîne d’échange à la fenêtre de sortie.
-   Quand vous concevez votre jeu, vérifiez que le format de maillage que vous choisissez prend en charge vos scénarios clés. Par exemple, si votre jeu utilise des collisions, vérifiez que vous pouvez obtenir des données de collision de vos maillages.
-   Séparez la logique du jeu et la logique de rendu en mettant à jour tous les objets de scène avant de les afficher.
-   En règle générale, vous dessinez vos objets de scène 3D, puis les objets 2D qui s’affichent sur le devant de la scène.
-   Synchronisez le dessin sur le vide vertical afin que votre jeu ne dessine pas des images qui ne sont jamais affichées.

## <a name="getting-started-with-directx-graphics"></a>Prise en main des graphiques DirectX


Quand nous avons commencé à imaginer le jeu de la plateforme Windows universelle (UWP), notre choix s’est tourné vers C++ et Direct3D 11.1, tous deux parfaits pour créer des jeux 3D qui nécessitent un contrôle optimal sur le rendu et pour obtenir d’excellentes performances. DirectX 11.1 prend en charge le matériel DirectX 9 à DirectX 11, ce qui vous permet de proposer votre jeu à davantage d’utilisateurs sans avoir besoin de réécrire le code pour les versions antérieures de DirectX.

Marble Maze utilise Direct3D 11.1 pour le rendu des éléments 3D, c’est-à-dire la bille et le labyrinthe. Marble Maze utilise également Direct2D, DirectWrite, et Windows Imaging Component (WIC) pour le dessin des éléments de jeu 2D, comme les menus et le minuteur. Enfin, Marble Maze utilise XAML pour offrir une barre de l’application et vous permettre d’ajouter des contrôles XAML.

Le développement de jeu nécessite une bonne planification. S’il s’agit de votre première utilisation des graphiques DirectX, nous recommandons la lecture de Création d’un jeu DirectX pour vous familiariser avec les concepts de base liés à la création d’un jeu DirectX UWP. Au fur et à mesure de votre lecture de ce document et de l’utilisation du code source de Marble Maze, vous pouvez consulter les ressources suivantes pour des informations détaillées sur les graphiques DirectX.

-   [Graphismes Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476080) Décrit Direct3D11, une API graphique3D à accélération matérielle puissante pour le rendu d’éléments géométriques3D sur la plateforme Windows.
-   [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) Décrit Direct2D, une API graphique2D à accélération matérielle qui fournit des performances élevées et un rendu de grande qualité pour les éléments géométriques2D, les images bitmap et le texte.
-   [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) Décrit DirectWrite, qui prend en charge un rendu de haute qualité du texte.
-   [Composant Imagerie Windows](https://msdn.microsoft.com/library/windows/desktop/ee719902) Décrit WIC, une plateforme extensible qui fournit une API de bas niveau pour les images numériques.

### <a name="feature-levels"></a>Niveaux de fonctionnalité

Direct3D 11 propose un modèle de référence nommé niveaux de fonctionnalité. Un niveau de fonctionnalité est un ensemble de fonctionnalités GPU définies. Utilisez les niveaux de fonctionnalités afin que votre jeu puisse s’exécuter sur des versions antérieures de matériel Direct3D. Marble Maze prend en charge le niveau de fonctionnalité 9.1 car il ne nécessite pas les fonctionnalités avancées des niveaux supérieurs. Nous recommandons de prendre en charge la gamme de matériel la plus étendue possible afin que vos utilisateurs puissent bénéficier de la même expérience quel que soit leur ordinateur. Pour plus d’informations sur les niveaux de fonctionnalité, voir [Direct3D11 sur du matériel avec un niveau de fonctionnalité inférieur](https://msdn.microsoft.com/library/windows/desktop/ff476872).

## <a name="initializing-direct3d-and-direct2d"></a>Initialisation Direct3D et Direct2D


Un appareil représente la carte vidéo. Vous créez des appareils Direct3D et Direct2D dans une application pour UWP tout comme dans une application de bureau Windows classique. La différence principale réside dans la connexion de la chaîne d’échange Direct3D au système de fenêtrage.

Les *applications DirectX11 et XAML (Windows universel)* proposent certaines fonctions de rendu3D et de système d’exploitation génériques à partir des fonctions inhérentes au jeu. La classe **DeviceResources** est un élément fondamental de la gestion Direct3D et Direct2D. Elle gère l’infrastructure générale et non les ressources spécifiques du jeu. Marble Maze définit la classe **MarbleMaze** afin de gérer les ressources intrinsèques du jeu. Il dispose ainsi d’une référence à un objet **DeviceResources** qui lui procure un accès à Direct3D et Direct2D.

Lors de l’initialisation, la méthode **DeviceResources::Initialize** crée des ressources indépendantes du périphérique ainsi que les périphériques Direct3D et Direct2D.

```cpp
// Initialize the Direct3D resources required to run. 
void DeviceResources::DeviceResources(CoreWindow^ window, float dpi)
{
    m_window = window;

    CreateDeviceIndependentResources();
    CreateDeviceResources();
    CreateWindowSizeDependentResources();
    SetDpi(dpi);
}
```

La classe **DeviceResources** sépare cette fonctionnalité afin d’obtenir une meilleure réponse lors de changement d’environnement. Par exemple, elle appelle la méthode **CreateWindowSizeDependentResources** quand la taille de la fenêtre change.

###  <a name="initializing-the-direct2d-directwrite-and-wic-factories"></a>Initialisation des fabriques Direct2D, DirectWrite, et WIC

La méthode **DeviceResources::CreateDeviceIndependentResources** crée les fabriques pour Direct2D, DirectWrite et WIC. Dans les graphiques DirectX, les fabriques constituent le point de départ pour la création des ressources graphiques. Marble Maze spécifie **D2D1\_FACTORY\_TYPE\_SINGLE\_THREADED**, car il effectue le dessin sur le thread principal.

```cpp
// These are the resources required independent of hardware. 
void DeviceResources::CreateDeviceIndependentResources()
{
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
     // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory1),
            &options,
            &m_d2dFactory
            )
        );

    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory),
            &m_dwriteFactory
            )
        );

    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  <a name="creating-the-direct3d-and-direct2d-devices"></a>Création des appareils Direct3D et Direct2D

La méthode **DeviceResources::CreateDeviceResources** appelle [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) pour créer l’objet périphérique qui représente la carte vidéo Direct3D. Dans la mesure où Marble Maze prend en charge les niveaux de fonctionnalité9.1 et supérieurs, la méthode **DeviceResources::CreateDeviceResources** spécifie les niveaux9.1 à 11.1 dans le tableau de valeurs **\\**. Direct3D parcourt la liste dans l’ordre et donne à l’application le premier niveau de fonctionnalité disponible. C’est pourquoi les entrées du tableau **D3D\_FEATURE\_LEVEL** sont répertoriées en ordre décroissant, afin que l’application puisse obtenir le niveau de fonctionnalité le plus élevé disponible. La méthode **DeviceResources::CreateDeviceResources** obtient le périphérique Direct3D11.1 en interrogeant le périphérique Direct3D11 qui est renvoyé par **D3D11CreateDevice**.

```cpp
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

// Create the DX11 API device object, and get a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
DX::ThrowIfFailed(
    D3D11CreateDevice(
        nullptr,                    // Specify null to use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                          // Leave as 0 unless it is a software device.
        creationFlags,              // Optionally, set debug and Direct2D compatibility flags.
        featureLevels,              // A list of feature levels that this app can support.
        ARRAYSIZE(featureLevels),   // The number of entries in the above list.
        D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for modern.
        &device,                    // Returns the Direct3D device created.
        &m_featureLevel,            // Returns the feature level of the device created.
        &context                    // Returns the device immediate context.
        )
    );    

// Get the Direct3D 11.1 device by querying the Direct3D 11 device.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );
```

La méthode **DeviceResources::CreateDeviceResources** crée ensuite le périphérique Direct2D. Direct2D utilise Microsoft DirectX Graphics Infrastructure (DXGI) pour interagir avec Direct3D. DXGI permet le partage de surfaces mémoire vidéo entre les runtime graphiques. Marble Maze utilise l’appareil DXGI sous-jacent de l’appareil Direct3D pour créer l’appareil Direct2D à partir de la fabrique Direct2D.

```cpp
// Obtain the underlying DXGI device of the Direct3D 11.1 device.
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

// Obtain the Direct2D device for 2-D rendering.
DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

// And get its corresponding device context object.
DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

```cpp
// Obtain the underlying DXGI device of the Direct3D 11.1 device.
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

// Obtain the Direct2D device for 2-D rendering.
DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

// And get its corresponding device context object.
DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

Pour plus d’informations sur DXGI et l’interopérabilité entre Direct2D et Direct3D, voir [Vue d’ensemble de DXGI](https://msdn.microsoft.com/library/windows/desktop/bb205075) et [Vue d’ensemble de l’interopérabilité Direct2D et Direct3D](https://msdn.microsoft.com/library/windows/desktop/dd370966).

### <a name="associating-direct3d-with-the-view"></a>Association de Direct3D à la vue

La méthode **DeviceResources::CreateWindowSizeDependentResources** crée les ressources graphiques qui dépendent d’une taille de fenêtre donnée, telle que la chaîne d’échange et les cibles de rendu Direct3D et Direct2D. Une différence majeure entre une application DirectX UWP et une application de bureau est la façon dont la chaîne d’échange est associée à la fenêtre de sortie. Une chaîne d’échange est chargée d’afficher la mémoire tampon grâce à laquelle l’appareil donne son rendu à l’écran. Le document Structure de l’application Marble Maze décrit les différences entre le système de fenêtrage d’une application pour UWP et d’une application de bureau. Dans la mesure où une application du Windows Store n’utilise pas d’objets **HWND**, Marble Maze doit utiliser la méthode [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) pour associer la sortie de périphérique à la vue. L’exemple suivant montre la partie de la méthode **DeviceResources::CreateWindowSizeDependentResources** qui crée la chaîne d’échange.

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window),
        &swapChainDesc,
        nullptr,    // Allow on all displays.
        &m_swapChain
        )
    );
```

Pour réduire la consommation d’énergie, qui est un élément important pour les appareils sur batterie, tels que les ordinateurs portables et les tablettes, la méthode **DeviceResources::CreateWindowSizeDependentResources** appelle la méthode [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334) afin que le jeu soit rendu uniquement après le vide vertical. La synchronisation avec le vide vertical est décrite plus en détail dans la section Présentation de la scène, de ce document.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both reduces  
// latency and ensures that the application will only render after each VSync, minimizing  
// power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

La méthode **DeviceResources::CreateWindowSizeDependentResources** initialise les ressources graphiques d’une façon qui convient à la plupart des jeux.

> **Remarque**   Le terme *vue* a une signification différente dans Windows Runtime et Direct3D. Dans le Windows Runtime, une vue fait référence à la collection de paramètres d’interface utilisateur d’une application, notamment la zone d’affichage et les comportements d’entrée, auxquels s’ajoute le thread qu’elle utilise pour le traitement. Vous spécifiez la configuration et les paramètres dont vous avez besoin quand vous créez une vue. Le processus de définition de la vue de l’application est décrit dans [Structure de l’application Marble Maze](marble-maze-application-structure.md). Dans Direct3D, le terme vue a plusieurs significations. Tout d’abord, une vue de ressource définit les sous-ressources auxquelles cette ressource peut accéder. Par exemple, quand un objet texture est associé à une vue de ressource de nuanceur, ce nuanceur peut ensuite accéder à la texture. L’un des avantages d’une vue de ressource est que vous pouvez interpréter des données de différentes manières à différentes étapes du pipeline de rendu. Pour plus d’informations sur les vues de ressource, voir [Vues de texture (Direct3D10)](https://msdn.microsoft.com/library/windows/desktop/bb205128). Quand elle est utilisée dans le cadre d’une transformation de vue ou d’une matrice de transformation de vue, elle fait référence à l’emplacement et à l’orientation de la caméra. Une transformation de vue repositionne les objets dans l’espace autour de la position et de l’orientation de la caméra. Pour plus d’informations sur les transformations de vue, voir [Transformation de vue (Direct3D9)](https://msdn.microsoft.com/library/windows/desktop/bb206342). La façon dont Marble Maze utilise les vues de ressource et de matrice est décrite en détail dans cette rubrique.

 

## <a name="loading-scene-resources"></a>Chargement des ressources de scène


Marble Maze utilise la classe **BasicLoader**, qui est déclarée dans BasicLoader.h, pour charger les textures et les nuanceurs. Marble Maze utilise la classe **SDKMesh** pour charger les maillages3D pour le labyrinthe et la bille.

Pour garantir la réactivité de l’application, Marble Maze charge les ressources de scène de façon asynchrone, ou en arrière-plan. Quand les éléments sont chargés en arrière-plan, votre jeu peut répondre aux événements de fenêtrage. Ce processus est décrit en détail dans [Chargement des composants du jeu en arrière-plan](marble-maze-application-structure.md#loading-game-assets-in-the-background) dans ce guide.

###  <a name="loading-the-2-d-overlay-and-user-interface"></a>Chargement de la superposition 2D et de l’interface utilisateur

Dans Marble Maze, la superposition est l’image qui s’affiche en haut de l’écran. La superposition s’affiche toujours sur le devant de la scène. Dans Marble Maze, la superposition contient le logo Windows et la chaîne de texte «Exemple de jeu DirectX Marble Maze». La gestion de la superposition est effectuée par la classe **SampleOverlay**, qui est définie dans SampleOverlay.h. Bien que nous utilisions la superposition dans le cadre des exemples Direct3D, vous pouvez adapter ce code pour afficher des images sur le devant de votre scène.

Dans la mesure où le contenu de la superposition ne change pas, la classe **SampleOverlay** dessine, ou met en cache, son contenu dans un objet [**ID2D1Bitmap1**](https://msdn.microsoft.com/library/windows/desktop/hh404349) lors de l’initialisation. Au moment du dessin, la classe **SampleOverlay** n’a plus qu’à dessiner l’image bitmap à l’écran. De cette façon, les routines gourmandes en ressources, telles que le dessin de texte ne sont pas exécutées à chaque image.

L’interface utilisateur (UI) se compose d’éléments 2D, tels que des menus et des affichages tête haute (HUD), qui s’affichent sur le devant de la scène. Marble Maze définit les éléments d’interface suivants :

-   Les éléments de menu qui permettent à l’utilisateur de démarrer le jeu et de voir les meilleurs scores.
-   Un minuteur qui compte jusqu’à trois avant de lancer le jeu.
-   Un minuteur qui compte le temps de jeu écoulé.
-   Un tableau qui répertorie les meilleurs temps.
-   Le texte « Interrompu » quand le jeu est en pause.

Marble Maze définit des éléments d’interface spécifiques au jeu dans UserInterface.h. Marble Maze définit la classe **ElementBase** comme type de base pour tous les éléments d’interface utilisateur. La classe **ElementBase** définit les attributs tels que la taille, la position, l’alignement et la visibilité d’un élément d’interface utilisateur. Elle contrôle également la mise à jour et le rendu des éléments.

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

> **Remarque**   Dans la mesure où XAML vous permet de créer plus facilement des interfaces utilisateur complexes, comme celles des jeux de simulation ou de stratégie, pensez à utiliser XAML pour définir votre interface utilisateur. Pour plus d’informations sur le développement d’une interface utilisateur en XAML dans un jeu UWP DirectX, voir [Développer l’exemple de jeu (Windows)](tutorial-resources.md). Ce document fait référence à l’exemple de jeu de tir DirectX 3D.

 

###  <a name="loading-shaders"></a>Chargement des nuanceurs

Marble Maze utilise la méthode **BasicLoader::LoadShader** pour charger un nuanceur à partir d’un fichier.

Les nuanceurs sont les unités fondamentales de la programmation GPU des jeux modernes. Les nuanceurs traitent la plupart des graphiques 3D, qu’il s’agisse de transformation de modèle ou d’éclairage de scène ou de traitement géométrique plus complexe (apparence des personnages ou pavage). Pour plus d’informations sur le modèle de programmation de nuanceur, voir [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561).

Marble Maze utilise les nuanceurs de vertex et de pixels. Un nuanceur de vertex s’exécute sur un vertex d’entrée et produit un vertex en sortie. Un nuanceur de pixels utilise des valeurs numériques, des données de texture, des valeurs par vertex interpolées et d’autres données pour produire une couleur de pixel en sortie. Dans la mesure où un nuanceur transforme un élément à la fois, le matériel graphique qui fournit plusieurs pipelines de nuanceur peut traiter en parallèle des ensembles d’éléments. Le nombre de pipelines parallèles disponible au GPU peut être bien supérieur au nombre disponible à l’UC. C’est pourquoi, mêmes des nuanceurs de base peuvent améliorer de façon notable la sortie.

La méthode **MarbleMaze::LoadDeferredResources** charge un nuanceur de vertex et un nuanceur de pixels après le chargement de la superposition. Les versions au moment de la conception de ces nuanceurs sont définies respectivement dans BasicVertexShader.hlsl et BasicPixelShader.hlsl. Marble Maze applique ces nuanceurs à la bille et au labyrinthe lors de la phase de rendu.

Le projet Marble Maze comprend les versions .hlsl (format au moment de la conception) et .cso (format au moment de l’exécution) des fichiers du nuanceur. Lors de la compilation, Visual Studio utilise le compilateur d’effet fxc.exe pour compiler votre fichier source .hlsl en nuanceur binaire .cso. Pour plus d’informations sur l’outil compilateur d’effets, voir [Outil compilateur d’effets](https://msdn.microsoft.com/library/windows/desktop/bb232919).

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

> **Attention**  Le nuanceur de pixels compilé contient 32instructions arithmétiques et 1instruction de texture. Ce nuanceur donne de bons résultats sur les ordinateurs de bureau et les tablettes haut de gamme. Cependant, un ordinateur moins puissant peut ne pas être en mesure de traiter ce nuanceur, mais fournir quand même une fréquence d’images interactive. Ciblez du matériel de base et concevez vos nuanceurs pour qu’ils s’adaptent à ce matériel.

 

La méthode **MarbleMaze::LoadDeferredResources** utilise la méthode **BasicLoader::LoadShader** pour charger les nuanceurs. L’exemple suivant charge le nuanceur de vertex. Le format d’exécution pour ce nuanceur est BasicVertexShader.cso. La variable membre **m\_vertexShader** est un objet [**ID3D11VertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476641).

```cpp
\loader->LoadShader(
    L"BasicVertexShader.cso",
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

La variable membre **m_inputLayout** est un objet [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575). L’objet de disposition d’entrée encapsule l’état d’entrée de l’étape d’assembleur d’entrée (IA). L’un des rôles de l’étape IA est d’améliorer l’efficacité des nuanceurs en utilisant des valeurs générées par le système, également appelées *sémantiques*, pour traiter uniquement les primitives ou les vertex qui n’ont pas encore été traités. Utilisez la méthode [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) pour créer une disposition d’entrée à partir d’un tableau de descriptions d’élément d’entrée. Ce tableau contient un ou plusieurs éléments d’entrée, chaque élément d’entrée décrit un élément de données de vertex à partir d’une mémoire tampon vertex. L’ensemble des descriptions d’élément d’entrée décrit tous les éléments de données de vertex de toutes les mémoires tampon de vertex qui seront liées à l’étape IA. L’exemple suivant montre la description de disposition utilisée par Marble Maze. La description de disposition décrit une mémoire tampon de vertex qui contient quatre éléments de données de vertex. Les éléments importants de chaque entrée dans le tableau sont le nom de la sémantique, le format de données et le décalage d’octet . Par exemple, l’élément **POSITION** spécifie la position du vertex dans l’espace d’objets. Il commence au décalage d’octet0 et contient trois composants à virgule flottante (pour un total de 12octets). L’élément **NORMAL** spécifie le vecteur normal. Il commence au décalage d’octet12, car il s’affiche directement après **POSITION** dans la disposition, qui nécessite 12octets. L’élément **NORMAL** contient un entier non signé 32bits de quatrecomposants.

```cpp
D3D11_INPUT_ELEMENT_DESC layoutDesc[] = 
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD",  0, DXGI_FORMAT_R32G32_FLOAT,   0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT,  0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 }, 
};
m_vertexStride = 44; // You must set this to match the size of layoutDesc above.
```

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

Le document [Sémantiques](https://msdn.microsoft.com/library/windows/desktop/bb509647) décrit chaque sémantique disponible en détail.

> **Remarque**   Dans une disposition, vous pouvez indiquer des composants supplémentaires qui ne sont pas utilisés pour permettre à plusieurs nuanceurs de partager la même disposition. Par exemple, l’élément **TANGENT** n’est pas utilisé par le nuanceur. Vous pouvez utiliser l’élément **TANGENT** si vous voulez tester des techniques, par exemple le mappage normal. En utilisant le mappage normal, également appelé placage de relief (bump mapping), vous pouvez créer l’effet de relief sur les surfaces des objets. Pour plus d’informations sur le placage de relief, voir [Placage de relief (Direct3D9)](https://msdn.microsoft.com/library/windows/desktop/bb172379).

 

Pour plus d’informations sur l’état de l’étape d’assembly d’entrée, voir [Étape de l’assembleur d’entrée](https://msdn.microsoft.com/library/windows/desktop/bb205116) et [Prise en main de l’état d’assembleur d’entrée](https://msdn.microsoft.com/library/windows/desktop/bb205117).

Le processus d’utilisation de nuanceurs de vertex et de pixels pour le rendu de la scène est décrit dans la section [Rendu de la scène](#rendering-the-scene) plus loin dans ce document.

### <a name="creating-the-constant-buffer"></a>Création de la mémoire tampon constante

Direct3D met en mémoire tampon des groupes d’ensemble de données. Une mémoire tampon constante est un type de mémoire tampon que vous utilisez pour passer des données aux nuanceurs. Marble Maze utilise une mémoire tampon pour contenir la vue du modèle (ou monde) et les matrices de projection pour l’objet de scène actif.

L’exemple suivant montre comment la méthode **MarbleMaze::LoadDeferredResources** crée une mémoire tampon constante qui contiendra les données de la matrice. L’exemple crée une structure **D3D11\_BUFFER\_DESC** qui utilise l’indicateur **D3D11\_BIND\_CONSTANT\_BUFFER** pour spécifier l’utilisation en tant que mémoire tampon constante. Cet exemple transmet ensuite cette structure à la méthode [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501). La variable **m_constantBuffer** est un objet [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351).

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth           = ((sizeof(ConstantBuffer) + 15) / 16) * 16; // Multiple of 16 bytes
constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;
// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_d3dDevice->CreateBuffer(
        &constantBufferDesc,
        nullptr,             // Leave the buffer uninitialized.
        &m_constantBuffer
        )
    );
```

La méthode **MarbleMaze::Update** met ensuite à jour les objets **ConstantBuffer**, un pour le labyrinthe et un pour la bille. La méthode **MarbleMaze::Render** lie chaque objet **ConstantBuffer** à la mémoire tampon constante avant le rendu de chaque objet. L’exemple suivant montre la structure **ConstantBuffer**, qui se trouve dans MarbleMaze.h.

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    float4x4 model;
    float4x4 view;
    float4x4 projection;

    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

Pour mieux comprendre comment les mémoires tampons constantes sont mappées au code du nuanceur, comparez la structure **ConstantBuffer** à la mémoire tampon constante **SimpleConstantBuffer** définie par le nuanceur de vertex dans BasicVertexShader.hlsl:

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

La disposition de la structure **ConstantBuffer** correspond à l’objet **cbuffer**. La variable **cbuffer** spécifie register b0, ce qui signifie que les données de la mémoire tampon constante sont stockées dans le registre0. La méthode **MarbleMaze::Render** spécifie register0 quand elle active la mémoire tampon constante. Ce processus est décrit en détail plus loin dans ce document.

Pour plus d’informations sur les mémoires tampons constantes, voir [Présentation des mémoires tampons dans Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476898). Pour plus d’informations sur le mot clé register, voir [**register**](https://msdn.microsoft.com/library/windows/desktop/dd607359).

###  <a name="loading-meshes"></a>Chargement des maillages

Marble Maze utilise SDK-Mesh comme format d’exécution car ce format fournit une méthode de base pour charger les données de maillage pour les applications d’exemple. Pour une utilisation en production, utilisez un format de maillage qui correspond aux exigences spécifiques à votre jeu.

La méthode **MarbleMaze::LoadDeferredResources** charge les données de maillage après avoir chargé les nuanceurs de vertex et de pixels. Un maillage est une collection de données vertex qui comprend souvent des informations sur les positions, les données normales, les couleurs, les matériaux et les coordonnées de texture. Les maillages sont créés dans un logiciel de création 3D et tenus à jour dans des fichiers séparés du code de l’application. La bille et le labyrinthe sont deux exemples de maillages utilisés par le jeu.

Marble Maze utilise la classe **SDKMesh** pour gérer les maillages. Cette classe est déclarée dans SDKMesh.h. **SDKMesh** fournit les méthodes pour charger, afficher et supprimer les données de maillage.

> **Important**   Marble Maze utilise le format SDK-Mesh et fournit la classe **SDKMesh** à des fins d’illustration uniquement. Bien que le format SDK-Mesh soit utile pour l’apprentissage et la création de prototypes, il s’agit d’un format de base qui peut ne pas convenir au développement de jeu spécifique. Nous recommandons d’utiliser un format de maillage qui correspond aux exigences spécifiques à votre jeu.

 

L’exemple suivant montre comment la méthode **MarbleMaze::LoadDeferredResources** utilise la méthode **SDKMesh::Create** pour charger les données de maillage pour le labyrinthe et la bille.

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_d3dDevice.Get(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_d3dDevice.Get(),
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

La façon dont vous chargez les données de collision dépend du format d’exécution que vous utilisez. Pour plus d’informations sur la façon dont Marble Maze charge la géométrie de collision à partir d’un fichier SDK-Mesh, voir la méthode **MarbleMaze::ExtractTrianglesFromMesh** dans le code source.

## <a name="updating-game-state"></a>Mise à jour de l’état du jeu


Marble Maze sépare la logique du jeu et la logique de rendu en mettant à jour tous les objets de scène avant de les afficher.

Le document Structure de l’application Marble Maze décrit la boucle du jeu principale. La mise à jour de la scène, qui fait partie de la boucle du jeu, se produit après le traitement des événements de fenêtrage et des entrées et avant le rendu de la scène. La méthode **MarbleMaze::Update** gère la mise à jour de l’interface utilisateur et du jeu.

### <a name="updating-the-user-interface"></a>Mise à jour de l’interface utilisateur

La méthode **MarbleMaze::Update** appelle la méthode **UserInterface::Update** pour mettre à jour l’état de l’interface utilisateur.

```cpp
UserInterface::GetInstance().Update(timeTotal, timeDelta);
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

Les classes dérivées de **ElementBase** implémentent la méthode **Update** pour effectuer des comportements spécifiques. Par exemple, la méthode **StopwatchTimer::Update** met à jour le temps écoulé en fonction de la valeur fournie et met à jour le texte qui est ensuite affiché.

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

La méthode **MarbleMaze::Update** met à jour le jeu en fonction de l’état de la machine à états active. Quand le jeu est dans un état actif, Marble Maze met à jour la caméra pour qu’elle suive la bille, met à jour la partie de matrice de vue des mémoires tampon constante et met à jour la simulation physique.

L’exemple suivant montre comment la méthode **MarbleMaze::Update** met à jour la position de la caméra. Marble Maze utilise la variable **m\_resetCamera** pour indiquer que la caméra doit être réinitialisée afin qu’elle se trouve directement au-dessus de la bille. La caméra est réinitialisée quand le jeu commence ou la bille tombe du labyrinthe. Quand l’écran du menu principal ou des meilleurs scores est actif, la caméra est définie à un emplacement constant. Dans le cas contraire, Marble Maze utilise le paramètre *timeDelta* pour interpoler la position de la caméra entre ses positions actuelle et cible. La position cible est légèrement au-dessus et devant la bille. L’utilisation du temps écoulé permet à la caméra de suivre la bille.

```cpp
static float eyeDistance = 200.0f;
static float3 eyePosition = float3(0, 0, 0);

// Gradually move the camera above the marble.
float3 targetEyePosition = marblePosition - (eyeDistance * float3(g.x, g.y, g.z));
if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    eyePosition = eyePosition + ((targetEyePosition - eyePosition) * min(1, timeDelta * 8));
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    eyePosition = marblePosition + float3(75.0f, -150.0f, -75.0f);
    m_camera->SetViewParameters(eyePosition, marblePosition, float3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, float3(0.0f, 1.0f, 0.0f));
}
```

L’exemple suivant montre comment la méthode **MarbleMaze::Update** met à jour les mémoires tampons constantes pour la bille et le labyrinthe. La matrice de modèle, ou monde, du labyrinthe est toujours la matrice d’identité. Sauf pour la diagonale principale, dont les éléments sont des uns, la matrice d’identité est une matrice carrée composée de zéros. La matrice de modèle de la bille est basée sur sa matrice de position que multiplie sa matrice de rotation. Les fonctions **mul** et **translation** sont définies dans BasicMath.h.

```cpp
// Update the model matrices based on the simulation.
m_mazeConstantBufferData.model = identity();
m_marbleConstantBufferData.model = mul(
    translation(marblePosition.x, marblePosition.y, marblePosition.z),
    marbleRotationMatrix
    );

// Update the view matrix based on the camera.
float4x4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

Pour plus d’informations sur la façon dont la méthode **MarbleMaze::Update** lit les entrées utilisateur et simule le mouvement de la bille, voir [Ajout d’entrées et d’interactivité à l’exemple Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md).

## <a name="rendering-the-scene"></a>Rendu de la scène


En règle générale, les étapes suivantes sont comprises dans le rendu d’une scène.

1.  Définir la mémoire tampon du gabarit/profondeur cible de rendu actif.
2.  Effacer les vues de rendu et de gabarit.
3.  Préparer les nuanceurs de vertex et de pixels pour le dessin.
4.  Afficher les objets 3D dans la scène.
5.  Afficher les objets 2D qui doivent apparaître sur le devant de la scène.
6.  Présenter l’image rendue à l’écran.

La méthode **MarbleMaze::Render** lie les vues de gabarit/profondeur et cible de rendu, efface ces vues, dessine la scène et la superposition.

###  <a name="preparing-the-render-targets"></a>Préparation des cibles de rendu

Avant d’effectuer le rendu de votre scène, vous devez définir la mémoire tampon de gabarit/profondeur cible de rendu active. Si votre scène ne doit pas dessiner sur chaque pixel de l’écran, effacez également les vues de rendu et de gabarit. Marble Maze efface les vues de rendu et de gabarit sur chaque image afin qu’aucun objet de l’image précédente ne soit visible.

L’exemple suivant montre comment la méthode **MarbleMaze::Render** appelle la méthode [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) pour définir comme actives la cible de rendu et la mémoire tampon de gabarit/profondeur. La variable membre **m\_renderTargetView**, objet [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582), et la variable membre **m\_depthStencilView**, objet [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377), sont définies et initialisées par la classe **DirectXBase**.

```cpp
// Bind the render targets.
m_d3dContext->OMSetRenderTargets(
    1,
    m_renderTargetView.GetAddressOf(),
    m_depthStencilView.Get()
    );

// Clear the render target and depth stencil to default values. 
const float clearColor[4] = { 0.0f, 0.0f, 0.0f, 1.0f };

m_d3dContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    clearColor
    );

m_d3dContext->ClearDepthStencilView(
    m_depthStencilView.Get(),
    D3D11_CLEAR_DEPTH,
    1.0f,
    0
    );
```

Les interfaces [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) et [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377) prennent en charge le mécanisme de vue de texture fourni par Direct3D10 et les versions ultérieures. Pour plus d’informations sur les vues de texture, voir [Vues de texture (Direct3D10)](https://msdn.microsoft.com/library/windows/desktop/bb205128). La méthode [**OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) prépare l’étape de fusion/sortie du pipeline Direct3D. Pour plus d’informations sur l’étape de fusion/sortie, voir [Étape de fusion/sortie](https://msdn.microsoft.com/library/windows/desktop/bb205120).

### <a name="preparing-the-vertex-and-pixel-shaders"></a>Création du nuanceur de vertex et du nuanceur de pixels

Avant d’effectuer le rendu des objets de scène, exécutez les étapes suivantes pour préparer les nuanceurs de vertex et de pixels pour le dessin :

1.  Définissez la disposition d’entrée du nuanceur comme disposition active.
2.  Définissez les nuanceurs de vertex et de pixels comme nuanceurs actifs.
3.  Mettez à jour les mémoires tampon constantes avec les données que vous devez transmettre aux nuanceurs.

> **Important**  Marble Maze utilise une paire de nuanceurs de vertex et de pixels pour tous les objets3D. Si votre jeu utilise plus d’une paire de nuanceurs, vous devez effectuer ces étapes chaque fois que vous dessinez des objets qui utilisent des nuanceurs différents. Pour réduire la charge associée à la modification de l’état du nuanceur, nous recommandons de grouper les appels de rendu pour tous les objets qui utilisent les mêmes nuanceurs.

 

La section [Chargement des nuanceurs](#loading-shaders) de ce document décrit comment la disposition d’entrée est créée quand le nuanceur de vertex est créé. L’exemple suivant montre comment la méthode **MarbleMaze::Render** utilise la méthode [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) pour définir cette disposition comme active.

```cpp
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

L’exemple suivant montre comment la méthode **MarbleMaze::Render** utilise les méthodes [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) et [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) pour définir les nuanceurs de vertex et de pixels comme nuanceurs actifs.

```cpp
// Set the vertex shader stage state.
m_d3dContext->VSSetShader(
    m_vertexShader.Get(),   // Use this vertex shader. 
    nullptr,                // Don't use shader linkage.
    0                       // Don't use shader linkage.
    );

// Set the pixel shader stage state.
m_d3dContext->PSSetShader(
    m_pixelShader.Get(),    // Use this pixel shader. 
    nullptr,                // Don't use shader linkage.
    0                       // Don't use shader linkage.
    );

m_d3dContext->PSSetSamplers(
    0,                       // Starting at the first sampler slot
    1,                       // set one sampler binding
    m_sampler.GetAddressOf() // to use this sampler.
    );
```

Une fois que **MarbleMaze::Render** a défini les nuanceurs et leur disposition d’entrée, elle utilise la méthode [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) pour mettre à jour la mémoire tampon constante avec les matrices de modèle, de vue et de projection pour le labyrinthe. La méthode **UpdateSubresource** copie les données de matrice de la mémoire UC vers la mémoire GPU. N’oubliez pas que les composants de modèle et de vue de la structure **ConstantBuffer** sont mis à jour dans la méthode **MarbleMaze::Update**. La méthode **MarbleMaze::Render** appelle ensuite les méthodes [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491) et [**ID3D11DeviceContext::PSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476470) pour définir cette mémoire tampon constante comme active.

```cpp
// Update the constant buffer with the new data.
m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0
    );

m_d3dContext->VSSetConstantBuffers(
    0,                // Starting at the first constant buffer slot
    1,                // set one constant buffer binding
    m_constantBuffer.GetAddressOf() // to use this buffer.
    );

m_d3dContext->PSSetConstantBuffers(
    0,                // Starting at the first constant buffer slot
    1,                // set one constant buffer binding
    m_constantBuffer.GetAddressOf() // to use this buffer.
    );
```

La méthode **MarbleMaze::Render** effectue des étapes similaires pour préparer le rendu de la bille.

### <a name="rendering-the-maze-and-the-marble"></a>Rendu du labyrinthe et de la bille

Après avoir activé les nuanceurs actifs, vous pouvez dessiner les objets de votre scène. La méthode **MarbleMaze::Render** appelle la méthode **SDKMesh::Render** pour effectuer le rendu du maillage du labyrinthe.

```cpp
m_mazeMesh.Render(m_d3dContext.Get(), 0, INVALID_SAMPLER_SLOT, INVALID_SAMPLER_SLOT);
```

La méthode **MarbleMaze::Render** effectue des étapes similaires pour le rendu de la bille.

Comme indiqué précédemment dans ce document, la classe **SDKMesh** est fournie à des fins de démonstration, mais nous ne recommandons pas son utilisation pour la production d’un jeu de qualité. Cependant, notez que la méthode **SDKMesh::RenderMesh**, qui est appelée par **SDKMesh::Render**, utilise les méthodes [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) et [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) pour définir les mémoires tampons de vertex et d’index actives qui définissent le maillage, et la méthode [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476410) pour dessiner les mémoires tampons. Pour plus d’informations sur l’utilisation des mémoires tampons de vertex et d’index, voir [Présentation des mémoires tampons dans Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476898).

### <a name="drawing-the-user-interface-and-overlay"></a>Dessin de l’interface utilisateur et de la superposition

Après avoir dessiné les objets de scène 3D, Marble Maze dessine les éléments d’interface 2D qui s’affichent sur le devant de la scène.

La méthode **MarbleMaze::Render** se termine en dessinant l’interface utilisateur et la superposition.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render();

m_sampleOverlay->Render();
```

La méthode **UserInterface::Render** utilise un objet [**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479) pour dessiner les éléments d’interface utilisateur. Cette méthode définit l’état de dessin, dessine tous les éléments d’interface utilisateur actif, puis rétablit l’état de dessin précédent.

```cpp
void UserInterface::Render()
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    m_d2dContext->EndDraw();
    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

La méthode **SampleOverlay::Render** utilise une technique identique pour dessiner l’image bitmap de superposition.

###  <a name="presenting-the-scene"></a>Présentation de la scène

Une fois les objets de scène 2D et 3D dessinés, Marble Maze présente l’image rendue à l’écran. Il synchronise le dessin sur le vide vertical afin que votre jeu ne dessine pas des images qui ne sont jamais affichées. Marble Maze gère également les changements d’appareils quand il présente la scène.

Une fois la méthode **MarbleMaze::Render** renvoyée, la boucle de jeu appelle la méthode **MarbleMaze::Present** pour envoyer l’image rendue à l’écran. La classe **MarbleMaze** ne remplace pas la méthode **DirectXBase::Present**. La méthode **DirectXBase::Present** appelle [**IDXGISwapChain1::Present**](https://msdn.microsoft.com/library/windows/desktop/hh446797) pour effectuer l’opération de présentation, comme indiqué dans l’exemple suivant:

```cpp
// The application may optionally specify "dirty" or "scroll" rects 
// to improve efficiency in certain scenarios. 
// In this sample, however, we do not utilize those features.
DXGI_PRESENT_PARAMETERS parameters = {0};
parameters.DirtyRectsCount = 0;
parameters.pDirtyRects = nullptr;
parameters.pScrollRect = nullptr;
parameters.pScrollOffset = nullptr;

// The first argument instructs DXGI to block until VSync, putting the  
// application to sleep until the next VSync.  
// This ensures we don't waste any cycles rendering frames that will  
// never be displayed to the screen.
HRESULT hr = m_swapChain->Present1(1, 0, &parameters);
```

Dans cet exemple, **m_swapChain** est un objet [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631). L’initialisation de cet objet est décrite dans la section [Initialisation Direct3D et Direct2D](#initializing-direct3d-and-direct2d) de ce document.

Le premier paramètre de [**IDXGISwapChain1::Present**](https://msdn.microsoft.com/library/windows/desktop/hh446797), *SyncInterval*, spécifie le nombre de vides verticaux en attente avant la présentation de l’image. Marble Maze indique 1 afin d’attendre le vide vertical suivant. Un vide vertical représente le temps entre la fin du dessin d’une image à l’écran et le début de l’image suivante.

La méthode [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) retourne un code d’erreur qui indique que le périphérique a été supprimé ou qu’il est en panne. Dans ce cas, Marble Maze réinitialise le périphérique.

```cpp
// Reinitialize the renderer if the device was disconnected  
// or the driver was upgraded. 
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    Initialize(m_window, m_dpi);
}
else
{
    DX::ThrowIfFailed(hr);
}
```

## <a name="next-steps"></a>Étapes suivantes


Pour plus d’informations sur les pratiques clés en matière de périphériques d’entrée, voir [Ajout d’entrées et d’interactivité à l’exemple Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md). Ce document explique comment Marble Maze prend en charge les entrées tactiles, d’accéléromètre, de contrôleur Xbox 360 et de souris.

## <a name="related-topics"></a>Rubriques connexes


* [Ajout d’entrées et d’interactivité à l’exemple Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Structure de l’application Marble Maze](marble-maze-application-structure.md)
* [Développement de Marble Maze, jeu pour UW en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




