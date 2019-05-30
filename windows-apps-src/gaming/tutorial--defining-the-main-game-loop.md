---
title: Définir l’objet jeu principal
description: Examinons maintenant les détails de l’objet principal de l’exemple de jeu et la façon dont les règles qu’il implémente se traduisent en interactions avec le monde du jeu.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jeux, objet principal
ms.localizationpriority: medium
ms.openlocfilehash: a3c47f3c22c41e7ca73c8a8b5d4e26dc27fab343
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367649"
---
# <a name="define-the-main-game-object"></a>Définir l’objet jeu principal

Une fois que vous avez disposé l’infrastructure de base de l’exemple de jeu et implémenté une machine à états qui gère l’utilisateur de haut niveau et les comportements du système, vous souhaiterez examiner les règles et les mécanismes qui déclenchent l’exemple de jeu dans un jeu. Nous allons examiner les détails de l’objet principal de l’exemple de jeu et comment traduire des règles du jeu dans les interactions avec le monde du jeu.

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objectif

Découvrez comment appliquer des techniques de développement de base pour implémenter des règles du jeu et mécanismes pour un jeu UWP DirectX.

## <a name="main-game-object"></a>Objet de jeu principal

Dans cet exemple de jeu, __Simple3DGame__ est la classe d’objet de jeu principale. Une instance de __Simple3DGame__ est construit dans le __App::Load__ (méthode).

Le __Simple3DGame__ objet de classe :
* Spécifie l’implémentation de la logique de jeu
* Contient des méthodes qui communiquent :
    * Modifications de l’état du jeu à l’ordinateur d’état défini dans le cadre de l’application.
    * Modifications de l’état du jeu à partir de l’application à l’objet de jeu lui-même.
    * Détails de la mise à jour du jeu de l’interface utilisateur (recouvrement et affichage tête haute), d’animations et de physique (la dynamique).

    >[!Note]
    >La mise à jour de graphiques est gérée par le __GameRenderer__ (classe), qui contient des méthodes pour obtenir et utiliser des ressources de périphérique de graphiques utilisés par le jeu. Pour plus d’informations, consultez [framework rendu i : Introduction au rendu](tutorial--assembling-the-rendering-pipeline.md).

* Sert de conteneur pour les données qui définit une session de jeu, niveau, ou la durée de vie, en fonction de la façon dont vous définissez votre jeu à un niveau élevé. Dans ce cas, les données d’état du jeu sont pour la durée de vie du jeu et sont initialisées une seule fois lorsqu’un utilisateur lance le jeu.

Pour afficher les méthodes et les données définies dans cet objet de classe, accédez à [Simple3DGame objet](#simple3dgame-object).

## <a name="initialize-and-start-the-game"></a>Initialiser et démarrer le jeu

Lorsqu’un joueur démarre le jeu, l’objet jeu doit initialiser son état, créer et ajouter la superposition, définir les variables qui suivent les performances du joueur et instancier les objets qui seront utilisés pour générer les niveaux. Dans cet exemple, cela s’effectue lorsque le nouveau __GameMain__ instance est créée en [ __App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123). 

L’objet de jeu, __Simple3DGame__, est créé dans le __GameMain__ constructeur. Il est ensuite initialisé à l’aide de la [ __Simple3DGame::Initialize__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250) méthode pendant le [async créer la tâche dans le __GameMain__ constructeur](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74).

### <a name="simple3dgameinitialize-method"></a>Simple3DGame::Initialize (méthode)

L’exemple de jeu configure les composants suivants dans l’objet de jeu :

* Un nouvel objet lecture audio est créé.
* Des tableaux pour les primitives graphiques du jeu sont créés, notamment des tableaux pour les primitives de niveau, les munitions et les obstacles.
* Un emplacement pour enregistrer les données de l’état du jeu est créé : il est nommé *Game* et est placé dans l’emplacement de stockage des paramètres des données d’application spécifié par [**ApplicationData::Current**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current).
* Un minuteur de jeu et la bitmap de superposition initiale, intégrée au jeu, sont créés.
* Une nouvelle caméra est créée avec un ensemble spécifique de paramètres de vue et de projection.
* Le périphérique d’entrée (contrôleur) étant défini sur les mêmes tangage et lacet de départ que la caméra, le joueur a une correspondance un-à-un entre la position du contrôle de départ et la position de la caméra.
* L’objet joueur est créé et associé à l’état actif. Nous utilisons un objet de la sphère pour détecter la proximité du joueur à murs et les obstacles et bien placé dans une position qui peut-être interrompre immersion que l’appareil photo.
* La primitive du monde du jeu est créée.
* Les obstacles sous forme de cylindres sont créés.
* Les cibles (objets **Face**) sont créées et numérotées.
* Les sphères de munitions sont créées.
* Les niveaux sont créés.
* Les meilleurs scores sont chargés.
* Tout état de jeu précédemment enregistré est chargé.

Le jeu a maintenant des instances de tous les principaux composants : le monde, le joueur, les obstacles, les cibles et les sphères de munitions. Il a également des instances des niveaux, qui représentent les configurations de tous les composants ci-dessus ainsi que leurs comportements pour chaque niveau spécifique. Voyons comment le jeu génère les niveaux.

## <a name="build-and-load-game-levels"></a>Créer et charger des niveaux de jeu

La plupart du gros du travail pour la construction de niveau est effectuée le __Level.h/.cpp__ fichiers trouvés dans le __GameLevels__ dossier de l’exemple de solution. Car elle se concentre sur une implémentation très spécifique, nous n’allons pas aborder les ici. Il est important que le code pour chaque niveau soit exécuté en tant qu’objet __LevelN__ distinct. Si vous souhaitez étendre le jeu, vous pouvez créer un **niveau** objet qui accepte un nombre affecté en tant que paramètre et de façon aléatoire place les obstacles et les cibles. Ou bien, vous pouvez y charger des données de configuration du niveau à partir d’un fichier de ressources, ou même Internet.

## <a name="define-the-game-play"></a>Définir le jeu

À ce stade, nous avons tous les composants nécessaires pour assembler le jeu. Les niveaux ont été construits dans la mémoire depuis les primitives et sont prêts pour le joueur puisse commencer à interagir avec.

Estimée meilleurs jeux réagissent instantanément à entrée d’un joueur et de fournissent des commentaires immédiats. Cela est vrai pour n’importe quel type d’un jeu, à partir de twitch-action, en temps réel tir subjectifs, aux jeux de stratégie bien pensée, en alternance.

### <a name="simple3dgamerungame-method"></a>Simple3DGame::RunGame (méthode)

Lors de la lecture d’un niveau, le jeu est dans le __Dynamics__ état. 

[__GameMain::Update__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329) est la boucle principale de mise à jour qui met à jour l’état de l’application une fois par frame, comme indiqué ci-dessous. Dans la boucle de mise à jour, il appelle le [ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) méthode qui traitera la tâche si le jeu se trouve dans le __Dynamics__ état.

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
          
[__Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) gère le jeu de données qui définit l’état actuel de l’exécution du jeu pour l’itération actuelle de la boucle du jeu.

Jeux de logique de flux dans __RunGame__:
*  La méthode met à jour le minuteur qui compte à rebours les secondes jusqu’à ce que le niveau soit terminé, et vérifie si le délai imparti pour ce niveau a expiré. Il s’agit d’une des règles du jeu : lorsque le temps est écoulé et toutes les cibles n’ont pas été capture, il est partie terminée.
*  À la fin du temps imparti, la méthode place le jeu à l’état **TimeExpired**, puis revient à la méthode **Update** dans le code précédent.
*  S’il reste du temps, le contrôleur de déplacement/vue est interrogé dans le cadre d’une mise à jour de la position de la caméra, en particulier une mise à jour de l’angle perpendiculaire de la vue du plan de la caméra (où le joueur regarde), et de la distance selon laquelle cet angle a bougé depuis la dernière fois que le contrôleur a été interrogé.
*  La caméra est mise à jour en fonction des nouvelles données du contrôleur de déplacement/vue.
*  La dynamique, ou les animations et comportements des objets du monde du jeu indépendants du contrôle du joueur, sont mis à jour. Dans cet exemple de jeu, le [ __UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) méthode est appelée pour mettre à jour le mouvement de la couleur des sphères munitions déclenché, l’animation des obstacles pilier et le déplacement des cibles. Pour plus d’informations, consultez [le monde du jeu de mise à jour](#update-the-game-world).
*  La méthode vérifie si les critères de réussite d’un niveau ont été remplis. Si tel est le cas, elle finalise le score du niveau et vérifie s’il s’agit du dernier niveau (6). S’il s’agit du dernier niveau, la méthode renvoie l’état de jeu **GameComplete** ; sinon, elle renvoie l’état de jeu __LevelComplete__.
*  Si le niveau n’est pas terminé, la méthode renvoie l’état de jeu __Active__.

## <a name="update-the-game-world"></a>Mettre à jour le monde du jeu

Dans cet exemple, lors de l’exécution du jeu, le [ __Simple3DGame::UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) méthode est appelée à partir de la [ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)(méthode) (qui est appelée à partir de [ __GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)) pour mettre à jour des objets qui sont rendus dans une scène de jeu.

Dans le __UpdateDynamics__ boucle, appelez les méthodes qui sont utilisées pour définir le monde du jeu en mouvement, indépendamment de l’entrée d’un joueur, créer une expérience de jeu immersive et rendre le niveau arrivé *actif*. Cela inclut les graphiques qui doit être affiché et l’animation en cours d’exécution de boucles pour mettre sur un vivant world même lorsqu’il n’existe aucune entrée d’un joueur. Par exemple, les arborescences vrillage dans le vent, ondes cresting le long de lignes de la terre, fumeur de machines et étrangers monstres étirer et déplacement. Il englobe également l’interaction entre les objets, y compris les collisions entre la sphère du joueur et le monde, ou entre les munitions et les obstacles et cibles.

La boucle du jeu doit toujours conserver la mise à jour le monde du jeu si elle est basée sur la logique de jeu, les algorithmes physiques, ou s’il s’agit tout simplement aléatoire, sauf lorsque le jeu est en pause en particulier. 

Dans l’exemple de jeu, ce principe porte le nom de *dynamique* et il englobe la montée et la chute des colonnes d’obstacles, ainsi que le mouvement et les comportements physiques des sphères de munitions lorsqu’elles sont tirées. 

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics (méthode) 

Cette méthode traite quatre ensembles de calculs :

* Les positions des sphères de munitions tirées dans le monde.
* L’animation des colonnes d’obstacles.
* L’intersection du joueur et des limites du monde.
* Les collisions entre les sphères de munitions et les obstacles, les cibles, d’autres sphères de munitions et le monde.

L’animation des obstacles est une boucle définie dans **Animate.h/.cpp**. Le comportement de la munitions et des conflits sont définies par les algorithmes physique simplifiée, fourni dans le code et paramétrés par un ensemble de constantes globales pour le monde du jeu, y compris les propriétés de matériau et de gravité. Tout cela est calculé dans l’espace de coordonnées du monde du jeu.

### <a name="review-the-flow"></a>Passez en revue le flux

Maintenant que nous avons mis à jour tous les objets dans la scène et calculé des conflits, nous devons utiliser ces informations pour dessiner l’élément visuel change correspondante. 

Après avoir __GameMain::Update()__ se termine l’itération actuelle de la boucle du jeu, l’exemple appelle immédiatement __Render()__ pour prendre les données de l’objet mis à jour et de générer une nouvelle scène à présenter à l’acteur, en tant que indiqué ici. Ensuite, examinons le rendu.

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

## <a name="render-the-game-worlds-graphics"></a>Rendu des graphiques de jeu mondial

Nous conseillons de mettre à jour les graphiques d’un jeu chaque fois que possible, ce qui revient à le faire, au maximum, à chaque itération de la boucle de jeu principale. Lors de l’itération de la boucle, le jeu est mis à jour, avec ou sans intervention du joueur. Cela permet d’afficher correctement les animations et comportements calculés. Imaginez que nous ayons une simple scène comportant de l’eau qui ne bouge que lorsque le joueur appuie sur un bouton. Les effets visuels seraient terriblement ennuyeux. Un bon jeu doit avoir un aspect fluide.

Rappeler la boucle de l’exemple de jeu comme indiqué ci-dessus dans [ __GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202). Si la fenêtre principale du jeu est visible et n’est pas ancrée ni désactivée, le jeu continue de mettre à jour et de restituer les résultats de cette mise à jour. Le [ __restituer__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624) méthode nous allons examiner une représentation sous forme de cet état s’affiche désormais. Cela s’effectue immédiatement après un appel à **mettre à jour**, qui inclut **RunGame** aux États de mise à jour, ce qui a été abordé dans la section précédente.

Cette méthode dessine la projection du monde en 3D, puis la superposition Direct2D au-dessus. Une fois terminé, elle présente la chaîne de permutation finale avec les tampons combinés à afficher.

>[!Note]
>Il existe deux États pour un segment de recouvrement de l’exemple de jeu Direct2D : un où le jeu affiche la superposition de jeu d’informations qui contient l’image bitmap pour le menu de pause et celui où le jeu affiche le réticule, ainsi que les rectangles pour l’apparence de déplacement écran tactile contrôleur. Le texte des scores est écrit dans les deux états. Pour plus d’informations, consultez [framework rendu i : Introduction au rendu](tutorial--assembling-the-rendering-pipeline.md).

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

## <a name="simple3dgame-object"></a>Objet de Simple3DGame

Voici les méthodes et les données qui sont définies dans le __Simple3DGame__ classe d’objet.

### <a name="methods"></a>Méthodes

Les méthodes internes définies sur **Simple3DGame** incluent :

-   **Initialiser**: Définit les valeurs de départ des variables globales et initialise les objets jeu. Cela est couvert dans le [initialiser et démarrer le jeu](#initialize-and-start-the-game) section.
-   **LoadGame**: Initialise un nouveau niveau et commence son chargement.
-   **LoadLevelAsync**: Démarre une tâche asynchrone (si vous n’êtes pas familiarisé avec les tâches asynchrones, consultez [bibliothèque de modèles parallèles](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) pour initialiser le niveau, puis appelez une tâche asynchrone sur le convertisseur pour charger les ressources de niveau spécifique d’appareil. Cette méthode s’exécute dans un thread séparé ; seules les méthodes [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) (par opposition aux méthodes [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) peuvent être appelées à partir de ce thread. Toutes les méthodes de contexte de périphérique sont appelées dans la méthode **FinalizeLoadLevel**.
-   **FinalizeLoadLevel**: Finit tout travail de chargement de niveau à effectuer sur le thread principal. Cela comprend tout appel aux méthodes ([**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) de contexte de périphérique Direct3D 11.
-   **StartLevel**: Démarre le jeu sur un nouveau niveau.
-   **PauseGame**: Suspend le jeu.
-   **RunGame**: Exécute une itération de la boucle de jeu. Cette méthode est appelée à partir d’**App::Update** une fois par itération de la boucle de jeu si le jeu présente l’état **Active**.
-   **OnSuspending** et **OnResuming**: Suspend et reprend la partie audio du jeu, respectivement.

Et les méthodes privées :

-   **LoadSavedState** et **SaveState**: Charge et enregistre l’état actuel du jeu, respectivement.
-   **SaveHighScore** et **LoadHighScore**: Enregistre et charge les meilleurs scores des différentes parties, respectivement.
-   **InitializeAmmo**: Redéfinit l’état de chaque objet sphère utilisé comme munition dans son état d’origine au début de chaque partie.
-   **UpdateDynamics**: Il s’agit d’une méthode importante, car elle met à jour tous les objets jeu selon l’entrée de contrôle, la physique et les routines d’animation prédéfinies. Elle représente le cœur de l’interactivité qui définit le jeu. Cela est couvert dans le [le monde du jeu de mise à jour](#update-the-game-world) section.

Les autres méthodes publiques sont les accesseurs Get de propriété qui renvoient des informations sur la superposition et le jeu à l’infrastructure de l’application dans le but de les afficher.

### <a name="data"></a>Données

En haut de l’exemple de code, il existe quatre objets dont les instances sont mises à jour pendant l’exécution de la boucle de jeu.

-   **MoveLookController** objet : Représente l’entrée d’un joueur. Pour plus d’informations, consultez [Ajout de contrôles](tutorial--adding-controls.md).
-   **GameRenderer** objet : Représente le convertisseur de Direct3D 11 dérivé de la **DirectXBase** classe qui gère tous les objets spécifiques à l’appareil et leur rendu. Pour plus d’informations, consultez [framework de rendu je](tutorial--assembling-the-rendering-pipeline.md).
-   **Audio** objet : Contrôle la lecture audio pour le jeu. Pour plus d’informations, consultez [Ajout de son](tutorial--adding-sound.md).

Le reste des variables du jeu contient les listes des primitives et leurs quantités respectives intégrées au jeu, ainsi que les données et contraintes propres au jeu.

## <a name="next-steps"></a>Étapes suivantes

À ce stade, vous êtes probablement curieux de savoir le moteur de rendu réel : comment les appels à la __restituer__ méthodes sur les primitives de mise à jour obtient transformés en pixels sur votre écran. Ce sujet est traité en deux parties &mdash; [framework rendu i : Introduction au rendu](tutorial--assembling-the-rendering-pipeline.md) et [framework rendu II : Rendu de jeux](tutorial-game-rendering.md). Si vous êtes davantage intéressé par la façon dont les contrôles du joueur mettent à jour l’état du jeu, voir [Ajouter des contrôles](tutorial--adding-controls.md).
