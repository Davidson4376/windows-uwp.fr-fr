---
author: joannaleecy
title: Définir l’objet jeu principal
description: Examinons maintenant les détails de l’objet principal de l’exemple de jeu et la façon dont les règles qu’il implémente se traduisent en interactions avec le monde du jeu.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows10, uwp, jeux, objet principal
ms.localizationpriority: medium
ms.openlocfilehash: b94d7139f35b3a18edd66af9959a0958d0bdcbc1
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5698672"
---
# <a name="define-the-main-game-object"></a>Définir l’objet jeu principal

Une fois que vous avez conçu l’infrastructure de base de l’exemple de jeu et implémenté une machine à états qui gère l’utilisation de niveau et les comportements du système, vous devez examiner les règles et la mécanique qui transforment l’exemple de jeu en un jeu. Examinons les détails de l’objet principal de l’exemple de jeu et comment convertir les règles du jeu en interactions avec le monde du jeu.

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objectif

Découvrez comment appliquer des techniques de développement de base pour implémenter les règles du jeu et les mécanismes pour un jeu UWP DirectX.

## <a name="main-game-object"></a>Objet de jeu principal

Dans cet exemple de jeu, __Simple3DGame__ est la classe de l’objet jeu principal. Une instance d’objet __Simple3DGame__ est créée dans la méthode __App::Load__ .

L’objet de classe __Simple3DGame__ :
* Indique à l’implémentation de la logique de jeu
* Contient des méthodes qui communiquent:
    * Modifications de l’état du jeu à la machine à états définie dans l’infrastructure d’application.
    * Modifications de l’état du jeu à partir de l’application à l’objet jeu lui-même.
    * Informations de mise à jour de l’interface utilisateur (superposition et affichage à tête haute) du jeu, les animations et physique (dynamique).

    >[!Note]
    >Mise à jour des graphiques est gérée par la classe __GameRenderer__ , qui contient des méthodes pour obtenir et utiliser les ressources de périphérique graphique utilisés par le jeu. Pour plus d’informations, voir [l’infrastructure de rendu i: présentation du rendu](tutorial--assembling-the-rendering-pipeline.md).

* Sert de conteneur pour les données qui définissent une session de jeu, niveau, ou une durée, en fonction de la façon dont vous définissez votre jeu à un niveau élevé. Dans ce cas, les données d’état du jeu sont pour la durée de vie du jeu et sont initialisées une fois lorsqu’un utilisateur lance le jeu.

Pour afficher les méthodes et les données définies dans cet objet de classe, accédez à [l’objet Simple3DGame](#simple3dgame-object).

## <a name="initialize-and-start-the-game"></a>Initialisation et démarrage du jeu

Lorsqu’un joueur démarre le jeu, l’objet jeu doit initialiser son état, créer et ajouter la superposition, définir les variables qui suivent les performances du joueur et instancier les objets qui seront utilisés pour générer les niveaux. Dans cet exemple, cette opération est effectuée lors de la nouvelle instance de __GameMain__ est créée dans [__App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123). 

L’objet jeu, __Simple3DGame__, est créé dans le constructeur __GameMain__ . Il est ensuite initialisée à l’aide de la méthode [__Simple3DGame::Initialize__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250) pendant le [async créer la tâche dans le constructeur __GameMain__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74).

### <a name="simple3dgameinitialize-method"></a>Méthode Simple3DGame::Initialize

L’exemple de jeu configure les composants suivants dans l’objet jeu:

* Un nouvel objet lecture audio est créé.
* Des tableaux pour les primitives graphiques du jeu sont créés, notamment des tableaux pour les primitives de niveau, les munitions et les obstacles.
* Un emplacement pour enregistrer les données de l’état du jeu est créé: il est nommé *Game* et est placé dans l’emplacement de stockage des paramètres des données d’application spécifié par [**ApplicationData::Current**](https://msdn.microsoft.com/library/windows/apps/br241619).
* Un minuteur de jeu et la bitmap de superposition initiale, intégrée au jeu, sont créés.
* Une nouvelle caméra est créée avec un ensemble spécifique de paramètres de vue et de projection.
* Le périphérique d’entrée (contrôleur) étant défini sur les mêmes tangage et lacet de départ que la caméra, le joueur a une correspondance un-à-un entre la position du contrôle de départ et la position de la caméra.
* L’objet joueur est créé et associé à l’état actif. Nous utilisons un objet sphère pour détecter la proximité du joueur pour les murs et aux obstacles et pour empêcher l’appareil photo d’obtention placé dans une position qui peuvent s’interrompre l’immersion.
* La primitive du monde du jeu est créée.
* Les obstacles sous forme de cylindres sont créés.
* Les cibles (objets **Face**) sont créées et numérotées.
* Les sphères de munitions sont créées.
* Les niveaux sont créés.
* Les meilleurs scores sont chargés.
* Tout état de jeu précédemment enregistré est chargé.

Le jeu a maintenant des instances de tous les principaux composants : le monde, le joueur, les obstacles, les cibles et les sphères de munitions. Il a également des instances des niveaux, qui représentent les configurations de tous les composants ci-dessus ainsi que leurs comportements pour chaque niveau spécifique. Voyons comment le jeu génère les niveaux.

## <a name="build-and-load-game-levels"></a>Créer et charger des niveaux de jeu

La plupart des lourdes tâches de construction des niveaux est effectuée dans les fichiers __Level.h/.cpp__ trouvés dans le dossier __GameLevels__ de l’exemple de solution. Dans la mesure où il se concentre sur une implémentation très spécifique, nous n’allons pas aborder les ici. Il est important que le code pour chaque niveau soit exécuté en tant qu’objet __LevelN__ distinct. Si vous souhaitez étendre le jeu, vous pouvez créer un objet de **niveau** qui prend un numéro affecté en tant que paramètre et place de façon aléatoire les obstacles et cibles. Ou bien, vous pouvez lui demander de charger les données de configuration de niveau à partir d’un fichier de ressources, ou même d’Internet.

## <a name="define-the-game-play"></a>Définir l’action de jeu

À ce stade, nous avons tous les composants nécessaires pour assembler le jeu. Les niveaux ont été construits en mémoire à partir des primitives et sont prêts pour le joueur d’interagir avec.

Jeux meilleures estimée réagissent instantanément aux entrées du joueur et fournissent un retour immédiat. Cela est vrai pour n’importe quel type d’un jeu, à partir de rapidité et d’action en temps réel TIR à la première personne à des jeux de stratégie suivants sont bien pensés, tour par tour.

### <a name="simple3dgamerungame-method"></a>Méthode Simple3DGame::RunGame

Lorsque la lecture d’un niveau, le jeu est dans l’état __Dynamics__ . 

[__GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329) est la boucle de mise à jour principal qui met à jour l’état de l’application une fois par trame, comme illustré ci-dessous. Dans la boucle de mise à jour, il appelle la méthode [__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) pour gérer le travail si le jeu se trouve dans l’état __Dynamics__ .

```cpp
// Updates the application state once per frame.
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
        //...

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when playing a level, the game is in the Dynamics state. Work is handled by Simple3DGame::RunGame method.
            switch (runState)
            {
                
      //...
```
          
[__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) gère l’ensemble de données qui définit l’état actuel du jeu pour l’itération actuelle de la boucle de jeu.

Logique de flux de jeu dans __RunGame__:
*  La méthode met à jour le minuteur qui compte à rebours les secondes jusqu’à ce que le niveau soit terminé, et vérifie si le délai imparti pour ce niveau a expiré. Il s’agit d’une des règles du jeu: lorsque le temps est écoulé et toutes les cibles n’ont pas été capture, vous n’avez.
*  À la fin du temps imparti, la méthode place le jeu à l’état **TimeExpired**, puis revient à la méthode **Update** dans le code précédent.
*  S’il reste du temps, le contrôleur de déplacement/vue est interrogé dans le cadre d’une mise à jour de la position de la caméra, en particulier une mise à jour de l’angle perpendiculaire de la vue du plan de la caméra (où le joueur regarde), et de la distance selon laquelle cet angle a bougé depuis la dernière fois que le contrôleur a été interrogé.
*  La caméra est mise à jour en fonction des nouvelles données du contrôleur de déplacement/vue.
*  La dynamique, ou les animations et comportements des objets du monde du jeu indépendants du contrôle du joueur, sont mis à jour. Dans cet exemple de jeu, la méthode [__UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) est appelée pour mettre à jour le mouvement les sphères de munitions qui ont été tirées, l’animation des colonnes d’obstacles et du déplacement des cibles. Pour plus d’informations, voir [mettre à jour le monde du jeu](#update-the-game-world).
*  La méthode vérifie si les critères de réussite d’un niveau ont été remplis. Si tel est le cas, elle finalise le score du niveau et vérifie s’il s’agit du dernier niveau (6). S’il s’agit du dernier niveau, la méthode renvoie l’état de jeu **GameComplete**; sinon, elle renvoie l’état de jeu __LevelComplete__.
*  Si le niveau n’est pas terminé, la méthode renvoie l’état de jeu __Active__.

## <a name="update-the-game-world"></a>Mise à jour le monde du jeu

Dans cet exemple, lorsque le jeu est en cours d’exécution, la méthode [__Simple3DGame::UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) est appelée à partir de la méthode [__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) (qui est appelée à partir de [__GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)) pour mettre à jour des objets qui sont affichés dans une scène de jeu.

Dans la boucle __UpdateDynamics__ , appeler des méthodes qui sont utilisées pour définir le monde du jeu en mouvement, indépendamment du joueur d’entrée, créer une expérience de jeu immersive et faire en sorte que le niveau *vivante*. Cela inclut les graphiques qui a besoin d’être rendue et effectue une boucle de l’animation en cours d’exécution pour remettre sur un vivante, monde même lorsqu’il n’existe aucune entrées du joueur. Par exemple, les arborescences vrillage dans le vent, les vagues cresting le rivage, tabac machines et monstres extraterrestres étirement et de déplacer. Il englobe également l’interaction entre les objets, y compris les collisions entre la sphère du joueur et le monde, ou entre les munitions et les obstacles et cibles.

La boucle de jeu doit toujours maintenir la version de mise à jour le monde du jeu si elle est basée sur la logique de jeu, des algorithmes physiques, ou qu’il s’agisse simplement aléatoires, sauf quand le jeu est expressément suspendu. 

Dans l’exemple de jeu, ce principe porte le nom de *dynamique* et il englobe la montée et la chute des colonnes d’obstacles, ainsi que le mouvement et les comportements physiques des sphères de munitions lorsqu’elles sont tirées. 

### <a name="simple3dgameupdatedynamics-method"></a>Méthode Simple3DGame::UpdateDynamics 

Cette méthode traite quatre ensembles de calculs :

* Les positions des sphères de munitions tirées dans le monde.
* L’animation des colonnes d’obstacles.
* L’intersection du joueur et des limites du monde.
* Les collisions entre les sphères de munitions et les obstacles, les cibles, d’autres sphères de munitions et le monde.

L’animation des obstacles est une boucle définie dans **Animate.h/.cpp**. Le comportement des munitions et des collisions sont définies par des algorithmes physiques simplifiés, fournis dans le code et paramétrés par un ensemble de constantes globales pour le monde du jeu, y compris les propriétés matérielles et de pesanteur. Tout cela est calculé dans l’espace de coordonnées du monde du jeu.

### <a name="review-the-flow"></a>Passez en revue le flux

Maintenant que nous avons mis à jour tous les objets dans la scène et calculé toutes les collisions, nous devons utiliser ces informations pour dessiner les modifications visuelles correspondantes. 

Une fois que __GameMain::Update()__ se termine l’itération actuelle de la boucle de jeu, l’exemple appelle immédiatement __Render()__ pour prendre les données d’objet mis à jour et générer une nouvelle scène à présenter au joueur, comme illustré ici. Ensuite, examinons le rendu.

```cpp
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::TooSmall:
                // ...
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update(); // GameMain::Update calls Simple3DGame::RunGame() if game is in Dynamics state, uses Simple3DGame::UpdateDynamics() to update game world.
                m_renderer->Render(); //Render() is called immediately after the Update() loop
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // exiting due to window close.  Make sure to save state.
}
```

## <a name="render-the-game-worlds-graphics"></a>Rendu de graphiques du monde du jeu

Nous conseillons de mettre à jour les graphiques d’un jeu chaque fois que possible, ce qui revient à le faire, au maximum, à chaque itération de la boucle de jeu principale. Lors de l’itération de la boucle, le jeu est mis à jour, avec ou sans intervention du joueur. Cela permet d’afficher correctement les animations et comportements calculés. Imaginez que nous ayons une simple scène comportant de l’eau qui ne bouge que lorsque le joueur appuie sur un bouton. Les effets visuels seraient terriblement ennuyeux. Un bon jeu doit avoir un aspect fluide.

N’oubliez pas de boucle de l’exemple de jeu comme indiqué ci-dessus dans [__GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202). Si la fenêtre principale du jeu est visible et n’est pas ancrée ni désactivée, le jeu continue de mettre à jour et de restituer les résultats de cette mise à jour. La méthode [__de rendu__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624) , que nous allons examiner s’affiche une représentation de cet état. Cette opération est effectuée immédiatement après un appel à **mettre à jour**, qui inclut la **RunGame** pour mettre à jour des États, qui a été abordé dans la section précédente.

Cette méthode dessine la projection du monde en 3D, puis la superposition Direct2D au-dessus. Une fois terminé, elle présente la chaîne de permutation finale avec les tampons combinés à afficher.

>[!Note]
>Il existe deux États de superposition Direct2D de l’exemple de jeu: un où le jeu affiche la superposition des informations du jeu qui contient la bitmap pour le menu de pause et celui où le jeu affiche le réticule avec les rectangles de déplacement / vue de l’écran tactile contrôleur. Le texte des scores est écrit dans les deux états. Pour plus d’informations, consultez [Infrastructure de rendu I: présentation du rendu](tutorial--assembling-the-rendering-pipeline.md).

### <a name="gamerendererrender-method"></a>Méthode GameRenderer::Render

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
   
        // ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            //...
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); // Renders the 3D objects
            }

        //...
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw(); //Start drawing the overlays
        
        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game); // Renders number of hits, shots, and time
        }

        if (m_gameInfoOverlay->Visible())
        {
            d2dContext->DrawBitmap(     // Renders the game overlay
                m_gameInfoOverlay->Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        //...
    }
}
```

## <a name="simple3dgame-object"></a>Objet Simple3DGame

Voici les méthodes et les données qui sont définies dans la classe d’objet __Simple3DGame__ .

### <a name="methods"></a>Méthodes

Les méthodes internes définies sur **Simple3DGame** sont les suivantes:

-   **Initialiser**: définit les valeurs de départ des variables globales et initialise les objets de jeu. Ce sujet est abordé dans la section [initialiser et démarrer le jeu](#initialize-and-start-the-game) .
-   **LoadGame**: initialise un nouveau niveau et commence son chargement.
-   **LoadLevelAsync**: démarre une tâche asynchrone (si vous n’êtes pas familiarisé avec les tâches asynchrones, voir [Bibliothèque de modèles parallèles](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) pour initialiser le niveau, puis invoquer une tâche asynchrone sur le convertisseur pour charger les ressources de niveau spécifique de périphérique. Cette méthode s’exécute dans un thread séparé; seules les méthodes [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) (par opposition aux méthodes [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) peuvent être appelées à partir de ce thread. Toutes les méthodes de contexte de périphérique sont appelées dans la méthode **FinalizeLoadLevel**.
-   **FinalizeLoadLevel**: finit tout travail de chargement de niveau qui doit être exécuté sur le thread principal. Cela comprend tout appel aux méthodes ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) de contexte de périphérique Direct3D11.
-   **StartLevel**: démarre le jeu pour un nouveau niveau.
-   **PauseGame**: le jeu est suspendu.
-   **RunGame**: exécute une itération de la boucle de jeu. Cette méthode est appelée à partir d’**App::Update** une fois par itération de la boucle de jeu si le jeu présente l’état **Active**.
-   **OnSuspending** et **OnResuming**: suspend et reprend l’audio du jeu, respectivement.

Et les méthodes privées:

-   **LoadSavedState** et **SaveState**: charges et enregistre l’état actuel du jeu, respectivement.
-   **SaveHighScore** et **LoadHighScore**: enregistre et charge le meilleur score différentes parties, respectivement.
-   **InitializeAmmo**: réinitialise l’état de chaque objet sphère utilisé comme MUNITION dans son état d’origine au début de chaque partie.
-   **UpdateDynamics**: il s’agit d’une méthode importante, car elle met à jour tous les objets jeu selon les routines d’animation prédéfinies, la physique et entrée de contrôle. Elle représente le cœur de l’interactivité qui définit le jeu. Ce sujet est abordé dans la section [mettre à jour le monde du jeu](#update-the-game-world) .

Les autres méthodes publiques sont les accesseurs Get de propriété qui renvoient des informations sur la superposition et le jeu à l’infrastructure de l’application dans le but de les afficher.

### <a name="data"></a>Données

En haut de l’exemple de code, il existe quatre objets dont les instances sont mises à jour pendant l’exécution de la boucle de jeu.

-   **MoveLookController** objet: représente les entrées du joueur. Pour plus d’informations, consultez [Ajout de contrôles](tutorial--adding-controls.md).
-   Objet **GameRenderer** : représente le convertisseur Direct3D 11 dérivé de la classe **DirectXBase** qui gère tous les objets spécifiques au périphérique et leur rendu. Pour plus d’informations, voir [infrastructure de rendu I](tutorial--assembling-the-rendering-pipeline.md).
-   Objet **audio** : contrôle la lecture audio pour le jeu. Pour plus d’informations, consultez la section [Ajout audio](tutorial--adding-sound.md).

Le reste des variables du jeu contient les listes des primitives et leurs quantités respectives intégrées au jeu, ainsi que les données et contraintes propres au jeu.

## <a name="next-steps"></a>Étapes suivantes

À présent, vous êtes probablement intrigué par le moteur de rendu réel: comment les appels aux méthodes __de rendu__ sur les primitives mises à jour obtient transformées en pixels sur votre écran. Ce sujet est abordé en deux parties &mdash; [infrastructure de rendu i: présentation du rendu](tutorial--assembling-the-rendering-pipeline.md) et [infrastructure de rendu II: rendu de jeu](tutorial-game-rendering.md). Si vous êtes davantage intéressé par la façon dont les contrôles du joueur mettent à jour l’état du jeu, voir [Ajouter des contrôles](tutorial--adding-controls.md).
