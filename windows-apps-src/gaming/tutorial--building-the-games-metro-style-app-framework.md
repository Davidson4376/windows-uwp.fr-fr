---
author: mtoepke
title: "Définir l’infrastructure d’application de plateforme Windows universelle (UWP) du jeu"
description: "La première partie du codage d’un jeu de plateforme Windows universelle (UWP) avec DirectX consiste à créer l’infrastructure qui permet à l’objet jeu d’interagir avec Windows."
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 2ebc7bca06454f78ab375058e49f012cacb00cc8

---

#  Définir l’infrastructure d’application de plateforme Windows universelle (UWP) du jeu


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

La première partie du codage d’un jeu de plateforme Windows universelle (UWP) avec DirectX consiste à créer l’infrastructure qui permet à l’objet jeu d’interagir avec Windows. Cela inclut des propriétés Windows Runtime telles que la gestion des événements de pause/reprise, la sélection de fenêtre et l’ancrage, ainsi que les événements, interactions et transitions pour l’interface utilisateur. Nous passons en revue la façon dont l’exemple de jeu est structuré et la façon dont il définit la machine à états principale pour l’interaction du joueur avec le système.

## Objectif


-   Pour configurer l’infrastructure pour un jeu UWP DirectX et implémenter la machine à états qui définit le flux global du jeu.

## Initialisation et démarrage du fournisseur de vues


Dans tout jeu UWP DirectX, vous devez obtenir un fournisseur de vues que le singleton de l’application, l’objet Windows Runtime qui définit une instance de votre application en cours d’exécution, peut utiliser pour accéder aux ressources graphiques nécessaires. Windows Runtime permet à votre application d’avoir une connexion directe à l’interface graphique, mais vous devez spécifier les ressources nécessaires et la façon de les gérer.

Comme nous l’avons indiqué dans [Configuration du projet de jeu](tutorial--setting-up-the-games-infrastructure.md), Microsoft Visual Studio 2015 fournit une implémentation d’un convertisseur de base pour DirectX dans le fichier **Sample3DSceneRenderer.cpp**, qui est disponible quand vous sélectionnez le modèle **Application DirectX 11 (Windows universel)**.

Pour plus d’informations sur la compréhension et la création d’un fournisseur de vues et d’un convertisseur, voir [Configuration de votre UWP avec C++ et DirectX pour afficher une vue DirectX](https://msdn.microsoft.com/library/windows/apps/hh465077).

Inutile de préciser que vous devez fournir l’implémentation de 5 méthodes que le singleton de l’application appelle :

-   [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)
-   [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)
-   [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)
-   [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)
-   [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)

Dans le modèle Application DirectX 11 (Windows universel), ces 5 méthodes sont définies sur l’objet **App** dans [App.h](#code_sample). Examinons la façon dont elles sont implémentées dans ce jeu.

Méthode Initialize du fournisseur de vues

```cpp
void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}
```

Le singleton de l’application commence par appeler **Initialize**. Par conséquent, cette méthode doit gérer les comportements les plus fondamentaux d’un jeu UWP, comme la gestion de l’activation de la fenêtre principale et l’assurance que ce jeu peut gérer un événement de pause subite (et une possible reprise par la suite).

Lorsque l’application de jeu est initialisée, elle alloue une mémoire spécifique pour le contrôleur afin de permettre au joueur de commencer à entrer des données. Elle crée aussi des nouvelles instances de rendu du jeu et de la machine à états à initialiser. Nous abordons les détails dans [Définition de l’objet jeu principal](tutorial--defining-the-main-game-loop.md).

À ce stade, l’application de jeu peut gérer un message de pause (ou de reprise), et la mémoire a été allouée pour le contrôleur, le convertisseur et le jeu lui-même. Cependant, il n’existe aucune fenêtre à utiliser et le jeu n’est pas initialisé. Quelques éléments supplémentaires sont nécessaires !

Méthode SetWindow du fournisseur de vues

```cpp
void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}
```

À présent, avec un appel à une implémentation de [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), le singleton de l’application fournit un objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) qui représente la fenêtre principale du jeu et met ses ressources et événements à la disposition du jeu. Avec l’existence d’une fenêtre à utiliser, le jeu peut maintenant commencer à ajouter à l’interface utilisateur de base des composants et événements : un pointeur (utilisé à la fois par les contrôles tactiles et de souris) et les événements de base pour le redimensionnement de fenêtre, la fermeture et les modifications PPP (si le périphérique d’affichage change).

L’application de jeu initialise également le contrôleur, car il existe une fenêtre avec laquelle interagir, ainsi que l’objet jeu lui-même. Elle peut lire les entrées du contrôleur (écran tactile, souris ou manette Xbox 360).

Une fois le contrôleur initialisé, l’application définit deux zones rectangulaires dans les coins inférieurs droit et gauche de l’écran pour les contrôles tactiles de déplacement et de la caméra, respectivement. Le joueur utilise le rectangle inférieur gauche, défini par l’appel de la méthode **SetMoveRect**, comme un pavé de contrôle virtuel pour déplacer la caméra vers l’avant et l’arrière, ou vers la gauche et la droite. Le rectangle inférieur droit, défini par la méthode **SetFireRect**, sert de bouton virtuel pour tirer des munitions.

Tout commence à se mettre en place.

Méthode Load du fournisseur de vues

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
    task<void>([this]()
    {
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // in which the m_renderer object was created because the D3D device context
        // can be accessed only on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // the finalizer code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can  be accessed only on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}
```

Une fois la fenêtre principale définie, le singleton de l’application appelle **Load**. Dans l’exemple, cette méthode utilise un ensemble de tâches asynchrones (dont la syntaxe est définie dans la [Bibliothèque de modèles parallèles](https://msdn.microsoft.com/library/windows/apps/dd492418.aspx)) pour créer les objets jeu, charger les ressources graphiques et initialiser la machine à états du jeu. Avec le modèle de tâche asynchrone, la méthode Load finit rapidement et permet à l’application de commencer à traiter les entrées. Dans cette méthode, l’application affiche aussi une barre de progression au fil du chargement des fichiers de ressources.

Nous scindons le chargement des ressources en deux étapes, car l’accès au contexte de périphérique Direct3D 11 est limité au thread sur lequel le contexte de périphérique a été créé, tandis que l’accès au périphérique Direct3D 11 pour la création d’objet est dépourvu de thread. La tâche **CreateGameDeviceResourcesAsync** s’exécute sur un thread séparé à partir de la tâche de fin (*FinalizeCreateGameDeviceResources*), qui s’exécute sur le thread original. Nous utilisons un modèle semblable pour charger les ressources de niveau avec **LoadLevelAsync** et **FinalizeLoadLevel**.

Une fois les objets jeu créés et les ressources graphiques chargées, nous initialisons la machine à états du jeu avec les conditions de départ (par exemple : réglage du nombre de munitions, du nombre de niveaux et des positions des objets initiaux). Si l’état du jeu indique que le joueur reprend une partie, nous chargeons le niveau en cours (niveau du joueur lorsqu’il a interrompu la partie).

Dans la méthode **Load**, nous effectuons toutes les préparations nécessaires avant le début du jeu, comme la définition des états de départ ou des valeurs globales. Si vous voulez commencer par récupérer des données ou des composants du jeu, faites-le ici, plutôt que dans **SetWindow** ou **Initialize**. Utilisez des tâches asynchrones dans votre jeu pour tout chargement, car Windows impose des restrictions sur le temps que peut prendre le jeu avant de commencer à traiter les entrées. Si le chargement dure un certain temps (en cas de nombreuses ressources), fournissez à vos utilisateurs une barre de progression régulièrement mise à jour.

Lors du développement de votre propre jeu, concevez votre code de démarrage autour de ces méthodes. Voici une liste de suggestions de base pour chaque méthode :

-   Utilisez **Initialize** pour allouer vos classes principales et connecter les gestionnaires d’événements de base.
-   Utilisez **SetWindow** pour créer votre fenêtre d’application principale et connecter tous les événements propres à la fenêtre.
-   Utilisez **Load** pour gérer tout le reste de l’installation, notamment pour commencer la création asynchrone d’objets et le chargement des ressources. Si vous devez créer des données ou fichiers temporaires, par exemple des composants générés par procédure, faites-le ici.

Ainsi, l’exemple de jeu crée une instance de la machine à états du jeu et lui affecte la configuration de mise en marche. Il gère l’ensemble des événements système et d’entrée. Il fournit une fenêtre dans laquelle afficher le contenu. Le code du jeu est maintenant prêt à être exécuté.

Méthode Run du fournisseur de vues

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The app is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save the state.
}
```

Nous arrivons ici à la partie jeu de l’application de jeu. Après avoir exécuté les 3 méthodes et préparé le terrain, l’application de jeu exécute la méthode **Run**, il est temps de s’amuser !

Dans l’exemple de jeu, nous démarrons une boucle while qui se termine lorsque le joueur ferme la fenêtre du jeu. L’exemple de code passe dans l’un des deux états de la machine à états du moteur de jeu :

-   La fenêtre de jeu est désactivée (perd le focus) ou ancrée. Lorsque cette situation se produit, le jeu interrompt le traitement des événements et attend que la fenêtre ait de nouveau le focus ou ne soit plus ancrée.
-   Sinon, le jeu met à jour son propre état et restitue le graphique pour affichage.

Lorsque votre jeu a le focus, vous devez gérer chaque événement qui arrive dans la file d’attente de messages, et vous devez donc appeler [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) avec l’option **ProcessAllIfPresent**. D’autres options peuvent provoquer des retards dans le traitement des événements de message, ce qui donne la sensation que votre jeu ne répond pas ou que les comportements tactiles sont au ralenti au lieu d’être réactifs.

Bien entendu, lorsque l’application est invisible, suspendue ou ancrée, nous ne voulons pas qu’elle utilise des ressources qui tournent en boucle pour envoyer des messages qui n’arriveront jamais. Votre jeu doit donc utiliser **ProcessOneAndAllPending**, qui opère un blocage tant qu’il ne reçoit pas d‘événement, puis traite cet événement et tous les autres qui arrivent dans la file d’attente de traitement pendant le traitement du premier. [
            **ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) est ensuite immédiatement de retour une fois que la file d’attente a été traitée.

Le jeu est en cours d’exécution ! Les événements qu’il utilise pour basculer entre les états sont distribués et traités. Les graphiques sont mis à jour lorsque la boucle de jeu effectue une itération. Nous espérons que le joueur s’amuse. Mais les bonnes choses ont une fin...

...et nous devons procéder au nettoyage. C’est là où **Uninitialize** intervient.

Méthode Uninitialize du fournisseur de vues

```cpp
void App::Uninitialize()
{
}
```

Dans l’exemple de jeu, nous laissons le singleton de l’application du jeu tout nettoyer une fois le jeu terminé. Dans Windows 10, la fermeture de la fenêtre de l’application ne met pas fin au processus de l’application, mais écrit en revanche l’état du singleton de l’application en mémoire. Si quelque chose de spécial doit se produire lorsque le système doit récupérer cette mémoire, un nettoyage spécifique des ressources, placez le code de ce nettoyage dans cette méthode.

Gardez à l’esprit ces 5 méthodes, car nous y ferons de nouveau référence dans ce didacticiel. Examinons maintenant la structure globale du moteur de jeu ainsi que les machines à états qui le définissent.

## Initialisation de l’état du moteur de jeu


Dans la mesure où un utilisateur peut reprendre une application de jeu UWP qui est dans un état suspendu à tout moment, l’application peut présenter n’importe quel nombre d’états possibles.

L’exemple de jeu peut présenter l’un des trois états suivants au démarrage :

-   La boucle de jeu était en cours d’exécution et au milieu d’un niveau.
-   La boucle de jeu n’était pas en cours d’exécution car une partie venait de se terminer. (Le meilleur score est défini.)
-   Aucune partie n’a été commencée ou la partie est entre deux niveaux. (Le meilleur score est égal à 0.)

Bien évidemment, dans votre propre jeu, vous pouvez avoir plus ou moins d’états. Encore une fois, gardez à l’esprit que votre jeu Windows Store peut se terminer à tout moment. Par ailleurs, lorsqu’il reprend, le joueur s’attend à ce que le jeu se comporte comme s’il n’avait jamais cessé de jouer.

Dans l’exemple de jeu, le flux de code se présente comme suit.

```cpp
void App::InitializeGameState()
{
    //
    // Set up the initial state machine for handling game playing state.
    //
    if (m_game->GameActive() && m_game->LevelActive())
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...

    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        m_updateState = UpdateEngineState::WaitingForPress;
        // ...
    }
    else
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...
    }
    SetAction(GameInfoOverlayCommand::PleaseWait);
    ShowGameInfoOverlay();
}
```

L’initialisation ne concerne pas tant le « démarrage à froid » de l’application que le redémarrage de l’application une fois qu’elle s’est terminée. L’exemple de jeu enregistre toujours l’état, ce qui donne l’impression que l’application est toujours en cours d’exécution. L’état suspendu peut se résumer ainsi : le jeu est interrompu, mais les ressources du jeu sont toujours en mémoire. De même, l’événement de reprise indique que l’exemple de jeu reprend là où il a été interrompu ou arrêté. Lorsque l’exemple de jeu redémarre après un arrêt, il démarre normalement, puis détermine le dernier état connu afin que le joueur puisse tout de suite continuer à jouer.

L’organigramme présente les états initiaux et transitions pour le processus d’initialisation de l’exemple de jeu.

![Processus d’initialisation et de préparation du jeu avant le démarrage de la boucle principale](images/simple3dgame-appstartup.png)

Selon l’état, différentes options sont présentées au joueur. Si le jeu reprend au milieu d’un niveau, il semble suspendu et la superposition présente une option pour continuer. Si le jeu a repris dans un état où il est terminé, il affiche les meilleurs scores et une option pour jouer une nouvelle partie. Enfin, si le jeu reprend avant le début d’un niveau, la superposition présente une option de démarrage à l’utilisateur.

L’exemple de jeu ne fait pas la distinction entre le démarrage à froid du jeu lui-même, autrement dit un jeu lancé pour la première fois sans événement de pause, et le jeu qui reprend après avoir été interrompu. Il s’agit de la conception appropriée de toute application UWP.

## Gestion des événements


Notre exemple de code a inscrit plusieurs gestionnaires pour des événements spécifiques dans **Initialize**, **SetWindow** et **Load**. Vous avez probablement deviné qu’il s’agissait d’événements importants, car l’exemple de code effectuait ce travail bien avant de faire partie de la mécanique de jeu ou du développement graphique. Vous avez raison ! Ces événements sont essentiels à une utilisation appropriée d’une application UWP et, comme une application UWP peut être activée, désactivée, redimensionnée, ancrée, non ancrée, suspendue ou reprise à tout moment, le jeu doit s’inscrire à ces événements précis dès que possible et les gérer de façon à ce que le jeu se déroule en douceur et de manière prévisible pour le joueur.

Voici les gestionnaires d’événements de l’exemple et les événements qu’ils gèrent. Le code complet de ces gestionnaires d’événements est disponible dans [Code complet pour cette section](#code_sample)

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
<td align="left">Gère [<strong>CoreApplicationView::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br225018). L’application de jeu ayant été amenée au premier plan, la fenêtre principale est activée.</td>
</tr>
<tr class="even">
<td align="left">OnLogicalDpiChanged</td>
<td align="left">Gère [<strong>DisplayProperties::LogicalDpiChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br226150). Les PPP de la fenêtre principale du jeu ont été modifiés, et l’application de jeu règle ses ressources en conséquence.
<div class="alert">
<strong>Remarque</strong> Les coordonnées de [<strong>CoreWindow</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404559) sont affichées en DIP (pixels indépendants des appareils), comme dans [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987). Par conséquent, vous devez indiquer à Direct2D la modification des PPP afin d’afficher correctement les primitives ou composants 2D.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Gère [<strong>CoreApplication::Resuming</strong>](https://msdn.microsoft.com/library/windows/apps/br205859). L’application de jeu restaure le jeu qui est dans un état suspendu.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Gère [<strong>CoreApplication::Suspending</strong>](https://msdn.microsoft.com/library/windows/apps/br205860). L’application de jeu enregistre son état sur disque. Elle dispose de 5 secondes pour enregistrer l’état dans le dispositif de stockage.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Gère [<strong>CoreWindow::VisibilityChanged</strong>](https://msdn.microsoft.com/library/windows/apps/hh701591). L’application de jeu a modifié la visibilité : elle est devenue soit visible, soit invisible car une autre application est devenue visible.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Gère [<strong>CoreWindow::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br208255). La fenêtre principale de l’application de jeu ayant été activée ou désactivée, elle doit supprimer le focus et interrompre le jeu, ou regagner le focus. Dans les deux cas, la superposition indique que le jeu est suspendu.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Gère [<strong>CoreWindow::Closed</strong>](https://msdn.microsoft.com/library/windows/apps/br208261). L’application de jeu ferme la fenêtre principale et suspend le jeu.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Gère [<strong>CoreWindow::SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208283). L’application de jeu réaffecte les ressources graphiques et la superposition pour tenir compte de la modification de la taille, puis met à jour la cible de rendu.</td>
</tr>
</tbody>
</table>

 

Votre propre jeu doit gérer ces événements, car ils font partie de la conception d’une application UWP.

## Mise à jour du moteur de jeu


Dans la boucle de jeu de **Run**, l’exemple a implémenté une machine à états de base pour la gestion de toutes les actions principales que le joueur peut effectuer. Le niveau le plus élevé de cette machine à états traite du chargement d’un jeu, du jeu à un niveau spécifique ou de la poursuite d’un niveau une fois que le jeu a été suspendu (par le système ou le joueur).

Dans l’exemple de jeu, le jeu peut se trouver dans l’un des 3 principaux états suivants (UpdateEngineState) :

-   **Waiting for resources**. La boucle de jeu effectue une itération, incapable de procéder à la transition tant que les ressources (en particulier, les ressources graphiques) ne sont pas disponibles. Une fois terminées les tâches asynchrones de chargement des ressources, elle met à jour l’état avec **ResourcesLoaded**. Cette situation se produit généralement entre les niveaux lorsque le niveau a besoin de charger de nouvelles ressources à partir du disque. Dans l’exemple de jeu, nous simulons ce comportement, car l’exemple n’a pas besoin de ressources supplémentaires par niveau à ce stade.
-   **Waiting for press**. La boucle de jeu effectue une itération, en attente d’une entrée utilisateur spécifique. Cette entrée est une action du joueur pour charger un jeu, démarrer un niveau ou continuer de jouer à un niveau. L’exemple de code fait référence à ces sous-états en tant que valeurs d’énumération PressResultState.
-   **Dynamics**. La boucle de jeu est en cours d’exécution et l’utilisateur joue. Pendant que l’utilisateur joue, le jeu recherche 3 conditions de transition : l’expiration du temps défini pour un niveau, la fin d’un niveau par le joueur ou la fin de tous les niveaux par le joueur.

Voici la structure du code. Le code complet figure dans [Code complet pour cette section](#code_sample).

Structure de la machine à états utilisée pour mettre à jour le moteur de jeu

```cpp
void App::Update()
{
    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
        switch (m_pressResult)
        {
        case PressResultState::LoadGame:
            // ...
            break;

        case PressResultState::PlayLevel:
            // ...
            break;

        case PressResultState::ContinueLevel:
            // ...
            break;
        }
        // ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete() || m_pressComplete)
        {
            m_pressComplete = false;

            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                // ...
                break;

            case PressResultState::PlayLevel:
                // ...
                break;

            case PressResultState::ContinueLevel:
                // ...
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested() || m_pauseRequested)
        {
            // ...
        }
        else 
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                // ...
                break;

            case GameState::LevelComplete:
                // ...
                break;

            case GameState::GameComplete:
                // ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_game->GameInfoOverlayUpperLeft(), m_game->GameInfoOverlayLowerRight());
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

Visuellement, la machine à états principale du jeu se présente comme suit :

![Machine à états principale de notre jeu](images/simple3dgame-mainstatemachine.png)

Nous abordons la logique de jeu en elle-même plus en détail dans [Définir l’objet jeu principal](tutorial--defining-the-main-game-loop.md). Pour le moment, vous devez considérer votre jeu comme une machine à états. Chaque état spécifique doit présenter des critères bien particuliers pour le définir et les transitions d’un état à un autre doivent reposer sur une intervention discrète de l’utilisateur ou des actions système (telles que le chargement de ressources graphiques). Lorsque vous planifiez votre jeu, tracez un diagramme semblable à celui que nous utilisons, en vous assurant que vous traitez toutes les actions possibles que l’utilisateur ou le système peut entreprendre à un haut niveau. Les jeux peuvent être très compliqués et la machine à états est un outil puissant pour visualiser cette complexité et la rendre très facile à gérer.

Bien entendu, comme vous le constatez, il existe des machines à états dans les machines à états. Il en existe une pour le contrôleur, qui gère toutes les entrées acceptables que le joueur peut générer. Dans le diagramme, un appui est une forme d’entrée utilisateur. Cette machine à états ne s’y intéresse pas, car elle fonctionne à un niveau supérieur ; elle part du principe que la machine à états du contrôleur gère toutes les transitions qui affectent les comportements de déplacement et de tir, ainsi que les mises à jour de rendu associées. Nous abordons la gestion des états des entrées dans [Ajout de contrôles](tutorial--adding-controls.md).

## Mise à jour de l’interface utilisateur


Nous devons tenir le joueur informé de l’état du système et lui permettre de modifier l’état de haut niveau en fonction des règles du jeu. Pour la plupart des jeux, y compris cet exemple de jeu, cela s’effectue avec un affichage à tête haute qui contient des représentations de l’état du jeu, ainsi que d’autres informations spécifiques au jeu, telles que le score, les munitions ou le nombre de chances restantes. C’est ce que nous appelons la superposition, car elle est restituée séparément de la chaîne de transformations graphiques principale et placée au-dessus de la projection 3D. Dans l’exemple de jeu, nous créons cette superposition à l’aide des API Direct2D. Nous pouvons également la créer en utilisant XAML, dont nous parlons dans [Extension de l’exemple de jeu](tutorial-resources.md).

Il existe deux composants dans l’interface utilisateur :

-   L’affichage à tête haute qui contient les scores et informations sur l’état actuel du jeu.
-   La bitmap de pause, qui est un rectangle noir avec un texte superposé lorsque le jeu est dans l’état de pause/suspension. Il s’agit de la superposition du jeu. Nous en parlons plus tard dans [Ajout d’une interface utilisateur](tutorial--adding-a-user-interface.md).

Rien d’étonnant à cela, la superposition a également une machine à états. La superposition peut afficher un message de début de niveau ou de fin de partie. Il s’agit essentiellement d’une zone de dessin destinée à recevoir toute information sur l’état du jeu que nous affichons à l’intention du joueur lorsque le jeu est interrompu ou suspendu.

Voici comment l’exemple de jeu structure la machine à états de la superposition.

```cpp
void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {

    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        // ...
        break;

    case GameInfoOverlayState::LevelStart:
        // ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        // ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        // ...
        break;

    case GameInfoOverlayState::Pause:
        // ...
        break;
    }
}
```

La superposition peut afficher 6 écrans d’états, selon l’état du jeu lui-même : un écran de chargement des ressources au début du jeu, un écran de jeu, un écran de message de début de niveau, un écran de fin de partie lorsque tous les niveaux sont terminés sans expiration du temps, un écran de fin de partie lorsque le temps est écoulé et un écran de menu de pause.

La séparation de l’interface utilisateur de la chaîne de transformations graphiques du jeu vous permet de l’utiliser indépendamment du moteur de rendu graphique du jeu et réduit considérablement la complexité du code du jeu.

## Étapes suivantes


Nous avons couvert la structure de base de l’exemple de jeu et présenté un modèle adéquat pour le développement d’applications de jeu UWP avec DirectX. Naturellement, nous n’avons pas tout abordé. Nous n’avons parlé que du squelette du jeu. Examinons maintenant de façon détaillée le jeu et sa mécanique, et comment cette mécanique est implémentée en tant qu’objet jeu principal Nous passons en revue cette partie dans [Définition de l’objet jeu principal](tutorial--defining-the-main-game-loop.md).

Il est également temps d’examiner plus en détail le processeur graphique de l’exemple de jeu. Cette partie est abordée dans [Assemblage du pipeline de rendu](tutorial--assembling-the-rendering-pipeline.md).

## Exemple de code complet pour cette section


App.h

```cpp
///// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "Simple3DGame.h"

enum class UpdateEngineState
{
    WaitingForResources,
    ResourcesLoaded,
    WaitingForPress,
    Dynamics,
    Snapped,
    Suspended,
    Deactivated,
};

enum class PressResultState
{
    LoadGame,
    PlayLevel,
    ContinueLevel,
};

enum class GameInfoOverlayState
{
    Loading,
    GameStats,
    GameOverExpired,
    GameOverCompleted,
    LevelStart,
    Pause,
};

ref class App : public Windows::ApplicationModel::Core::IFrameworkView
{
internal:
    App();

public:
    // IFrameworkView Methods
    virtual void Initialize(_In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
    virtual void SetWindow(_In_ Windows::UI::Core::CoreWindow^ window);
    virtual void Load(_In_ Platform::String^ entryPoint);
    virtual void Run();
    virtual void Uninitialize();

private:
    void InitializeGameState();

    // Event Handlers
    void OnSuspending(
        _In_ Platform::Object^ sender,
        _In_ Windows::ApplicationModel::SuspendingEventArgs^ args
        );

    void OnResuming(
        _In_ Platform::Object^ sender,
        _In_ Platform::Object^ args
        );

    void UpdateViewState();

    void OnWindowActivationChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
        );

    void OnWindowSizeChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowSizeChangedEventArgs^ args
        );

    void OnWindowClosed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::CoreWindowEventArgs^ args
        );

    void OnLogicalDpiChanged(
        _In_ Platform::Object^ sender
        );

    void OnActivated(
        _In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView,
        _In_ Windows::ApplicationModel::Activation::IActivatedEventArgs^ args
        );

    void OnVisibilityChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::VisibilityChangedEventArgs^ args
        );

    void Update();
    void SetGameInfoOverlay(GameInfoOverlayState state);
    void SetAction (GameInfoOverlayCommand command);
    void ShowGameInfoOverlay();
    void HideGameInfoOverlay();
    void SetSnapped();
    void HideSnapped();

    bool                                                m_windowClosed;
    bool                                                m_renderNeeded;
    bool                                                m_haveFocus;
    bool                                                m_visible;

    MoveLookController^                                 m_controller;
    GameRenderer^                                       m_renderer;
    Simple3DGame^                                       m_game;

    UpdateEngineState                                   m_updateState;
    UpdateEngineState                                   m_updateStateNext;
    PressResultState                                    m_pressResult;
    GameInfoOverlayState                                m_gameInfoOverlayState;
    GameInfoOverlayCommand                              m_gameInfoOverlayCommand;
    uint32                                              m_loadingCount;
};

ref class Direct3DApplicationSource : Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView();
};
```

App.cpp

```cpp
//--------------------------------------------------------------------------------------
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "App.h"

using namespace concurrency;
using namespace DirectX;
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::Foundation;
using namespace Windows::Graphics::Display;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::ViewManagement;


App::App() :
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
}

//--------------------------------------------------------------------------------------

void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}

//--------------------------------------------------------------------------------------

void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Load(
    _In_ Platform::String^ /* entryPoint */
    )
{
    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and loads all the necessary resources in parallel on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}

//--------------------------------------------------------------------------------------

void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The App is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save state.
}

//--------------------------------------------------------------------------------------

void App::Uninitialize()
{
}

//--------------------------------------------------------------------------------------

void App::OnWindowSizeChanged(
    _In_ CoreWindow^ window,
    _In_ WindowSizeChangedEventArgs^ /* args */
    )
{
    UpdateViewState();
    m_renderer->UpdateForWindowSizeChange();

    // The location of the GameInfoOverlay may have changed with the size change, so update the controller.
    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowClosed(
    _In_ CoreWindow^ /* sender */,
    _In_ CoreWindowEventArgs^ /* args */
    )
{
    m_windowClosed = true;
}

//--------------------------------------------------------------------------------------

void App::OnLogicalDpiChanged(
    _In_ Platform::Object^ /* sender */
    )
{
    m_renderer->SetDpi(DisplayProperties::LogicalDpi);

    // The GameInfoOverlay may have been recreated as a result of DPI changes, so
    // regenerate the data.
    SetGameInfoOverlay(m_gameInfoOverlayState);
    SetAction(m_gameInfoOverlayCommand);
}

//--------------------------------------------------------------------------------------

void App::OnActivated(
    _In_ CoreApplicationView^ /* applicationView */,
    _In_ IActivatedEventArgs^ /* args */
    )
{
    CoreWindow::GetForCurrentThread()->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);
    CoreWindow::GetForCurrentThread()->Activate();
}

//--------------------------------------------------------------------------------------

void App::OnVisibilityChanged(
    _In_ CoreWindow^ /* sender */,
    _In_ VisibilityChangedEventArgs^ args
    )
{
    m_visible = args->Visible;
}

//--------------------------------------------------------------------------------------

void App::InitializeGameState()
{
    // Set up the initial state machine for handling the Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::ContinueLevel;
        SetGameInfoOverlay(GameInfoOverlayState::Pause);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        m_updateState = UpdateEngineState::WaitingForPress;
        m_pressResult = PressResultState::LoadGame;
        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        SetAction(GameInfoOverlayCommand::TapToContinue);
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Update()
{
    static uint32 loadCount = 0;

    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for the initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
        switch (m_pressResult)
        {
        case PressResultState::LoadGame:
            SetGameInfoOverlay(GameInfoOverlayState::GameStats);
            break;

        case PressResultState::PlayLevel:
            SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
            break;

        case PressResultState::ContinueLevel:
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            break;
        }
        m_updateState = UpdateEngineState::WaitingForPress;
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        ShowGameInfoOverlay();
        m_renderNeeded = true;
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;
                m_controller->Active(false);
                m_game->LoadGame();
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case PressResultState::PlayLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->StartLevel();
                break;

            case PressResultState::ContinueLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->ContinueGame();
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            m_game->PauseGame();
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_updateState = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            ShowGameInfoOverlay();
        }
        else
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverExpired);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;

            case GameState::LevelComplete:
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case GameState::GameComplete:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverCompleted);
                ShowGameInfoOverlay();
                m_updateState  = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowActivationChanged(
    _In_ Windows::UI::Core::CoreWindow^ /* sender */,
    _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
    )
{
    if (args->WindowActivationState == CoreWindowActivationState::Deactivated)
    {
        m_haveFocus = false;

        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of Deactivated rather than going directly back into game play
            // go to the paused state waiting for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            ShowGameInfoOverlay();
            m_game->PauseGame();
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            ShowGameInfoOverlay();
            m_renderNeeded = true;
            break;
        }
        // Don't have focus, so shutdown input processing.
        m_controller->Active(false);
    }
    else if (args->WindowActivationState == CoreWindowActivationState::CodeActivated
        || args->WindowActivationState == CoreWindowActivationState::PointerActivated)
    {
        m_haveFocus = true;

        if (m_updateState == UpdateEngineState::Deactivated)
        {
            m_updateState = m_updateStateNext;

            if (m_updateState == UpdateEngineState::WaitingForPress)
            {
                SetAction(GameInfoOverlayCommand::TapToContinue);
                m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
            }
            else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
            {
                SetAction(GameInfoOverlayCommand::PleaseWait);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::OnSuspending(
    _In_ Platform::Object^ /* sender */,
    _In_ SuspendingEventArgs^ args
    )
{
    // Save application state.
    // If your application needs time to complete a lengthy operation, it can request a deferral.
    // The SuspendingOperation has a deadline time. Make sure all your operations are complete by that time!
    // If the app doesn't return from this handler within five seconds, it will be terminated.
    SuspendingOperation^ op = args->SuspendingOperation;
    SuspendingDeferral^ deferral = op->GetDeferral();

    create_task([=]()
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // Game is in the active game play state, Stop Game Timer and Pause play and save the state.
            SetAction(GameInfoOverlayCommand::None);
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            break;

        default:
            // If it is any other state, don't save as the next state as they are transient states and have already set m_updateStateNext
            break;
        }
        m_updateState = UpdateEngineState::Suspended;

        m_controller->Active(false);
        m_game->OnSuspending();

        deferral->Complete();
    });
}

//--------------------------------------------------------------------------------------

void App::OnResuming(
    _In_ Platform::Object^ /* sender */,
    _In_ Platform::Object^ /* args */
    )
{
    if (m_haveFocus)
    {
        m_updateState = m_updateStateNext;
    }
    else
    {
        m_updateState = UpdateEngineState::Deactivated;
    }

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
    m_game->OnResuming();
    ShowGameInfoOverlay();
    m_renderNeeded = true;
}

//--------------------------------------------------------------------------------------

void App::UpdateViewState()
{
    m_renderNeeded = true;

    if (ApplicationView::Value == ApplicationViewState::Snapped)
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of SNAPPED layout rather than going directly back into game play,
            // go to the paused state and wait for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            // Avoid corrupting the m_updateStateNext on a transition from Snapped -> Snapped.
            // Otherwise, just cache the current state and return to it when leaving SNAPPED layout.

            m_updateStateNext = m_updateState;
            break;

        default:
            break;
        }

        m_updateState = UpdateEngineState::Snapped;
        m_controller->Active(false);
        HideGameInfoOverlay();
        SetSnapped();
    }
    else if (ApplicationView::Value == ApplicationViewState::Filled ||
        ApplicationView::Value == ApplicationViewState::FullScreenLandscape ||
        ApplicationView::Value == ApplicationViewState::FullScreenPortrait)
    {
        if (m_updateState == UpdateEngineState::Snapped)
        {

            HideSnapped();
            ShowGameInfoOverlay();
            m_renderNeeded = true;

            if (m_haveFocus)
            {
                if (m_updateStateNext == UpdateEngineState::WaitingForPress)
                {
                    SetAction(GameInfoOverlayCommand::TapToContinue);
                    m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
                }
                else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    SetAction(GameInfoOverlayCommand::PleaseWait);
                }

                m_updateState = m_updateStateNext;
            }
            else
            {
                m_updateState = UpdateEngineState::Deactivated;
                SetAction(GameInfoOverlayCommand::None);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        m_renderer->InfoOverlay()->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_renderer->InfoOverlay()->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_renderer->InfoOverlay()->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_renderer->InfoOverlay()->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_renderer->InfoOverlay()->SetPause();
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::SetAction (GameInfoOverlayCommand command)
{
    m_gameInfoOverlayCommand = command;
    m_renderer->InfoOverlay()->SetAction(command);
}

//--------------------------------------------------------------------------------------

void App::ShowGameInfoOverlay()
{
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideGameInfoOverlay()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::SetSnapped()
{
    m_renderer->InfoOverlay()->SetPause();
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideSnapped()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
    SetGameInfoOverlay(m_gameInfoOverlayState);
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXAppSource::CreateView()
{
    return ref new App();
}

//--------------------------------------------------------------------------------------

[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------
```

 

 







<!--HONumber=Jun16_HO4-->


