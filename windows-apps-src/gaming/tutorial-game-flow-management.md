---
title: Gestion du flux de jeu
description: Découvrez comment initialiser les états de jeu, gérer les événements et configurer la boucle de mise à jour de jeu.
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 10/24/2017
ms.topic: article
keywords: windows10, uwp, jeux, directx
ms.localizationpriority: medium
ms.openlocfilehash: c6d13b848a9e5d2dfc145431f732187c35c46ab6
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8939159"
---
# <a name="game-flow-management"></a>Gestion du flux de jeu

Le jeu dispose maintenant d’une fenêtre, a inscrit plusieurs gestionnaires d’événements et charge les ressources en mode asynchrone. Cette section décrit comment utiliser les états de jeu, gérer des états de jeu clés spécifiques et créer une boucle de mise à jour pour le moteur de jeu. Vous découvrirez ensuite le flux d’interface utilisateur et en saurez plus sur les gestionnaires d’événements et les événements nécessaires pour un jeu UWP.

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="game-states-used-to-manage-game-flow"></a>États de jeu utilisés pour gérer le flux de jeu

Nous utilisons les états de jeu pour gérer le flux du jeu. Dans la mesure où un utilisateur peut reprendre une application de jeu UWP qui est dans un état suspendu à tout moment, votre jeu peut présenter n’importe quel nombre d’états possibles.

Cet exemple de jeu peut présenter l’un des trois états suivants au démarrage:
* La boucle de jeu est en cours d’exécution et au milieu d’un niveau.
* La boucle de jeu n’est pas en cours d’exécution, car une partie vient de se terminer. (Le meilleur score est défini)
* Aucune partie n’a été commencée ou la partie est entre deux niveaux. (Le meilleur score est égal à 0)

Vous pouvez avoir défini le nombre d’états requis en fonction des besoins de votre jeu. Encore une fois, gardez à l’esprit que votre jeu UWP peut se terminer à tout moment. Par ailleurs, lorsqu’il reprend, le joueur s’attend à ce que le jeu se comporte comme s’il n’avait jamais cessé de jouer.

## <a name="game-states-initialization"></a>Initialisation des états de jeu

Lors de l’initialisation du jeu, vous ne vous concentrez pas simplement sur le démarrage à froid du jeu, mais également sur le redémarrage après une mise en pause ou un arrêt. L’exemple de jeu enregistre toujours l’état du jeu, ce qui donne l’impression qu’il est resté en cours d’exécution. 

Dans l’état suspendu, le jeu est interrompu, mais les ressources du jeu sont toujours en mémoire. 

De même, l’événement de reprise permet de s’assurer que l’exemple de jeu reprend là où il a été interrompu ou arrêté. Lorsque l’exemple de jeu redémarre après un arrêt, il démarre normalement, puis détermine le dernier état connu afin que le joueur puisse tout de suite continuer à jouer.

Selon l’état, différentes options sont présentées au joueur. 

* Si le jeu reprend au milieu d’un niveau, il semble suspendu et la superposition présente une option pour continuer.
* Si le jeu reprend dans un état où il est terminé, il affiche les meilleurs scores et une option pour jouer une nouvelle partie.
* Enfin, si le jeu reprend avant le début d’un niveau, la superposition présente une option de démarrage à l’utilisateur.

L’exemple de jeu ne fait pas la distinction entre le démarrage à froid du jeu, son lancement pour la première fois sans événement de pause, ou le jeu qui reprend après avoir été interrompu. Il s’agit de la conception appropriée de toute application UWP.

Dans cet exemple, l’initialisation des états de jeu survient dans [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method).

Voici un organigramme pour vous aider à visualiser le flux; il inclut l’initialisation et la boucle de mise à jour.

* L’initialisation commence au nœud __Start__ lorsque vous vérifiez l’état de jeu actuel. Pour le code de jeu, accédez à [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method).
* Pour plus d’informations sur la boucle de mise à jour, accédez à [Mise à jour du moteur de jeu](#update-game-engine). Pour le code de jeu, accédez à [__App::Update__](#appupdate-method).

![Machine à états principale de notre jeu](images/simple-dx-game-flow-statemachine.png)

### <a name="gamemaininitializegamestate-method"></a>Méthode GameMain::InitializeGameState

La méthode __InitializeGameState__ est appelée à partir de la classe de constructeur [__GameMain__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L32-L131) qui est appelée lorsque l’objet de classe __GameMain__ est créé dans la méthode [__App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123).

```cpp

GameMain::GameMain(...)
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...

    create_task([this]()
    {
        ...

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState(); //Initialization of game states occurs here.
        
        ...
    
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
        
    }, task_continuation_context::use_current());
}

```

```cpp

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle of a level.
        // We are waiting for the user to continue the game.
        //...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        //...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        m_uiControl->SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    m_uiControl->ShowGameInfoOverlay();
}

```

## <a name="update-game-engine"></a>Mise à jour du moteur de jeu

Dans la méthode [__App::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L127-L130), [__GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202) est appelé. Dans cette méthode, l’exemple a implémenté une machine à états de base pour la gestion de toutes les actions principales que le joueur peut effectuer. Le niveau le plus élevé de cette machine à états traite du chargement d’un jeu, du jeu à un niveau spécifique ou de la poursuite d’un niveau une fois que le jeu a été suspendu (par le système ou le joueur).

Dans l’exemple de jeu, le jeu peut se trouver dans l’un des 3principaux états suivants (__UpdateEngineState__):

1. __Waiting for resources__: la boucle de jeu effectue une itération, incapable de procéder à la transition tant que les ressources (en particulier, les ressources graphiques) ne sont pas disponibles. Une fois terminées les tâches asynchrones de chargement des ressources, elle met à jour l’état avec __ResourcesLoaded__. Cette situation se produit généralement entre les niveaux lorsque le niveau charge de nouvelles ressources à partir du disque, du serveur de jeux ou du serveur principal dans le cloud. Dans l’exemple de jeu, nous simulons ce comportement, car l’exemple n’a pas besoin de ressources supplémentaires par niveau à ce stade.
2. __Waiting for press__: la boucle de jeu effectue une itération, en attente d’une entrée utilisateur spécifique. Cette entrée est une action du joueur pour charger un jeu, démarrer un niveau ou continuer de jouer à un niveau. L’exemple de code fait référence à ces sous-états en tant que valeurs d’énumération __PressResultState__.
3. In __Dynamics__: la boucle de jeu est en cours d’exécution et l’utilisateur joue. Pendant que l’utilisateur joue, le jeu recherche 3conditions de transition: 
    * __TimeExpired__: l’expiration du temps défini pour un niveau
    * __LevelComplete__: la fin d’un niveau par le joueur 
    * __GameComplete__: la fin de tous les niveaux par le joueur.

Votre jeu est simplement une machine à états contenant plusieurs machines à états plus petites. Chaque état spécifique doit être défini par des critères bien particuliers. Les transitions d’un état à un autre doivent reposer sur une intervention discrète de l’utilisateur ou des actions système (telles que le chargement de ressources graphiques). Lors de la planification de votre jeu, envisagez de dessiner l’ensemble du flux de jeu, pour vous assurer que vous avez tenu compte de toutes les actions possibles de l’utilisateur ou du système. Les jeux peuvent être très compliqués et la machine à états est un outil puissant pour mieux visualiser cette complexité et la rendre plus facile à gérer.

Examinons les codes de la boucle de mise à jour ci-dessous.

### <a name="appupdate-method"></a>Méthode App::Update

Structure de la machine à états utilisée pour mettre à jour le moteur de jeu

```cpp
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        //...
        break;

    case UpdateEngineState::ResourcesLoaded:
        //...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            //...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when the player is playing, the work is handled by this Simple3DGame::RunGame method.
            switch (runState)
            {
            case GameState::TimeExpired:
                //...
                break;

            case GameState::LevelComplete:
                //...
                break;

            case GameState::GameComplete:
                //...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-user-interface"></a>Mise à jour de l’interface utilisateur

Nous devons tenir le joueur informé de l’état du système et permettre à l’état du jeu de changer en fonction des actions de l’utilisateur et des règles qui définissent le jeu. De nombreux jeux, y compris cet exemple de jeu, utilisent généralement des éléments d’interface utilisateur pour présenter ces informations au joueur. L’interface utilisateur contient des représentations de l’état du jeu et d’autres informations spécifiques au jeu, telles que le score, les munitions ou le nombre de chances restantes. L’interface utilisateur est également appelée superposition, car elle est restituée séparément de la chaîne de transformations graphiques principale et placée au-dessus de la projection3D. Certaines informations de l’interface utilisateur sont également présentées avec un affichage à tête haute (HUD) pour permettre aux utilisateurs de lire ces informations sans quitter des yeux la zone de jeu principale. Dans l’exemple de jeu, nous créons cette superposition à l’aide des API Direct2D. Nous pouvons également la créer en utilisant XAML, dont nous parlons dans [Extension de l’exemple de jeu](tutorial-resources.md).

Il existe deux composants dans l’interface utilisateur:

-   L’affichage à tête haute qui contient les scores et des informations sur l’état actuel du jeu.
-   La bitmap de pause, qui est un rectangle noir avec un texte superposé lorsque le jeu est dans l’état de pause/suspension. Il s’agit de la superposition du jeu. Nous en parlons plus tard dans [Ajout d’une interface utilisateur](tutorial--adding-a-user-interface.md).

Rien d’étonnant à cela, la superposition a également une machine à états. La superposition peut afficher un message de début de niveau ou de fin de partie. Il s’agit essentiellement d’une zone de dessin destinée à recevoir toute information sur l’état du jeu que nous affichons à l’intention du joueur lorsque le jeu est interrompu ou suspendu.

La superposition restituée peut afficher six écrans différents, selon l’état du jeu: 
1. Écran de chargement des ressources au début du jeu
2. Écran de statistiques de jeu
3. Écran de message de début de niveau
4. Écran de fin de partie lorsque tous les niveaux sont terminés sans expiration du temps
5. Écran de fin de partie lorsque le temps est écoulé
6. Écran de menu de pause

La séparation de l’interface utilisateur de la chaîne de transformations graphiques du jeu vous permet de l’utiliser indépendamment du moteur de rendu graphique du jeu et réduit considérablement la complexité du code du jeu.

Voici comment l’exemple de jeu structure la machine à états de la superposition.

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        //...
        break;

    case GameInfoOverlayState::LevelStart:
        //...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        //...
        break;

    case GameInfoOverlayState::GameOverExpired:
        //...
        break;

    case GameInfoOverlayState::Pause:
        //...
        break;
    }
}
```

## <a name="events-handling"></a>Gestion des événements

Notre exemple de code a inscrit plusieurs gestionnaires pour des événements spécifiques dans **Initialize**, **SetWindow** et **Load** dans App.cpp. Ces événements importants doivent fonctionner avant que nous puissions ajouter la mécanique du jeu ou commencer le développement graphique. Ces événements sont essentiels à une utilisation appropriée d’une application UWP. Dans la mesure où une application UWP peut être activée, désactivée, redimensionnée, ancrée, non ancrée, suspendue ou reprise à tout moment, le jeu doit s’inscrire à ces événements précis dès que possible et les gérer de façon à ce que le jeu se déroule en douceur et de manière prévisible pour le joueur.

Voici les gestionnaires d’événements utilisés dans cet exemple et les événements qu’ils gèrent.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Gestionnaire d’événements</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">Gère <a href="https://msdn.microsoft.com/library/windows/apps/br225018"><strong>CoreApplicationView::Activated</strong></a>. L’application de jeu ayant été amenée au premier plan, la fenêtre principale est activée.</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">Gère <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a>. La résolution PPP de l’affichage a été modifiée et le jeu règle ses ressources en conséquence.
<div class="alert">
<strong>Remarque</strong>[<strong>CoreWindow</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404559) coordonnées sont exprimées en DIP (Device Independent Pixel) pour [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987). Par conséquent, vous devez indiquer à Direct2D la modification des PPP afin d’afficher correctement les primitives ou composants2D.
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">Gère <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a>. L’orientation de l’affichage change et le rendu doit être mis à jour.</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">Gère <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a>. L’affichage doit être actualisé et votre jeu doit faire l’objet d’un nouveau rendu.</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Gère <a href="https://msdn.microsoft.com/library/windows/apps/br205859"><strong>CoreApplication::Resuming</strong></a>. L’application de jeu restaure le jeu qui est dans un état suspendu.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Gère <a href="https://msdn.microsoft.com/library/windows/apps/br205860"><strong>CoreApplication::Suspending</strong></a>. L’application de jeu enregistre son état sur disque. Elle dispose de 5 secondes pour enregistrer l’état dans le dispositif de stockage.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Gère <a href="https://msdn.microsoft.com/library/windows/apps/hh701591"><strong>CoreWindow::VisibilityChanged</strong></a>. L’application de jeu a modifié la visibilité: elle est devenue soit visible, soit invisible car une autre application est devenue visible.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Gère <a href="https://msdn.microsoft.com/library/windows/apps/br208255"><strong>CoreWindow::Activated</strong></a>. La fenêtre principale de l’application de jeu ayant été activée ou désactivée, elle doit supprimer le focus et interrompre le jeu, ou regagner le focus. Dans les deux cas, la superposition indique que le jeu est suspendu.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Gère <a href="https://msdn.microsoft.com/library/windows/apps/br208261"><strong>CoreWindow::Closed</strong></a>. L’application de jeu ferme la fenêtre principale et suspend le jeu.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Gère <a href="https://msdn.microsoft.com/library/windows/apps/br208283"><strong>CoreWindow::SizeChanged</strong></a>. L’application de jeu réaffecte les ressources graphiques et la superposition pour tenir compte de la modification de la taille, puis met à jour la cible de rendu.</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>Étapes suivantes

Dans cette rubrique, nous avons décrit comment le flux de jeu global est géré à l’aide des états de jeu et comment un jeu est constitué de plusieurs machines à états différentes. Nous avons également appris comment mettre à jour l’interface utilisateur et gérer les gestionnaires d’événements d’application clés. Nous pouvons maintenant nous pencher sur la boucle de rendu, le jeu et sa mécanique.
 
Vous pouvez accéder aux autres composants du jeu dans n’importe quel ordre:
* [Définir l’objet de jeu principal](tutorial--defining-the-main-game-loop.md)
* [Infrastructure de renduI: présentation du rendu](tutorial--assembling-the-rendering-pipeline.md)
* [Infrastructure de renduII: rendu de jeu](tutorial-game-rendering.md)
* [Ajouter une interface utilisateur](tutorial--adding-a-user-interface.md)
* [Ajouter des contrôles](tutorial--adding-controls.md)
* [Ajouter du son](tutorial--adding-sound.md)