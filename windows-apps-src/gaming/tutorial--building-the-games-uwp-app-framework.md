---
author: joannaleecy
title: Définir l’infrastructure d’application UWP du jeu
description: La première partie du codage d’un jeu de plateforme Windows universelle (UWP) avec DirectX consiste à créer l’infrastructure qui permet à l’objet jeu d’interagir avec Windows.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows10, uwp, jeux, directx
ms.localizationpriority: medium
ms.openlocfilehash: 3444c71b4e4c610be0b7d92ac6d761340c5dd5c2
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5931302"
---
#  <a name="define-the-uwp-app-framework"></a>Définir l’infrastructure d’application UWP

Créez une infrastructure pour permettre à l’objet jeu d’interagir avec Windows, notamment les propriétés Windows Runtime telles que la gestion des événements de pause/reprise, les modifications de la sélection de fenêtre et l’ancrage.

Pour configurer cette infrastructure, procurez-vous tout d’abord un fournisseur d’affichage que le singleton de l’application, l’objet Windows Runtime qui définit une instance de votre application en cours d’exécution, peut utiliser pour accéder aux ressources graphiques nécessaires. Windows Runtime permet également à votre jeu d’avoir une connexion directe à l’interface graphique, et vous pouvez spécifier les ressources nécessaires et la façon de les gérer.

L’objet fournisseur d’affichage implémente l’interface __IFrameworkView__, qui se compose d’une série de méthodes qui doit être configurée pour créer cet exemple de jeu.

Vous devez implémenter les cinq méthodes suivantes appelées par le singleton de l’application:
* [__Initialize__](#initialize-the-view-provider)
* [__SetWindow__](#configure-the-window-and-display-behavior)
* [__Load__](#load-method-of-the-view-provider)
* [__Run__](#run-method-of-the-view-provider)
* [__Uninitialize__](#uninitialize-method-of-the-view-provider)

La méthode __Initialize__ est appelée lors du lancement de l’application. La méthode __SetWindow__ est appelée après __Initialize__. La méthode __Load__ est ensuite appelée. La méthode __Run__ est exécutée quand le jeu est en cours d’exécution. Lorsque le jeu se termine, la méthode __Uninitialize__ est appelée. Pour plus d’informations, consultez la documentation de référence de l’API [__IFrameworkView__](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview). 

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objectif

Configurer l’infrastructure pour un jeu de plateforme Windows universelle (UWP, Universal Windows Platform) DirectX et implémenter la machine à états qui définit le flux global du jeu.

## <a name="define-the-view-provider-factory-and-view-provider-object"></a>Définir la fabrique de fournisseurs d’affichages et l’objet fournisseur d’affichage

Examinons la boucle __main__ dans __App.cpp__. 

Dans cette étape, nous créons une fabrique pour l’affichage (implémente __IFrameworkViewSource__), qui à son tour crée des instances de l’objet fournisseur d’affichage (implémente __IFrameworkView__) qui définit l’affichage.

### <a name="main-method"></a>Méthode Main

Créez un élément __DirectXApplicationSource__ si l’exemple de code GitHub est chargé. (Utilisez __Direct3DApplicationSource__ si vous utilisez le modèle DirectX d’origine). Il s’agit de la fabrique de fournisseurs d’affichages qui implémente __IFrameworkViewSource__. L’interface __IFrameworkViewSource__ de la fabrique de fournisseurs d’affichages a une méthode unique, __CreateView__, qui est définie.

Dans __CoreApplication::Run__, la méthode __CreateView__ est appelée par le singleton de l’application lors de la transmission de __Direct3DApplicationSource__ ou __DirectXApplicationSource__.

__CreateView__ renvoie une référence à une nouvelle instance de votre objet application qui implémente __IFrameworkView__, qui est l’objet de classe __App__ dans cet exemple. L’objet de classe __App__ est l’objet fournisseur d’affichage.

```cpp
// The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto directXApplicationSource = ref new DirectXApplicationSource();
    CoreApplication::Run(directXApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXApplicationSource::CreateView()
{
    return ref new App();
}
```

## <a name="initialize-the-view-provider"></a>Initialiser le fournisseur d’affichage

Une fois l’objet fournisseur d’affichage créé, le singleton de l’application appelle la méthode [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) lors du lancement de l’application. Par conséquent, cette méthode doit gérer les comportements les plus fondamentaux d’un jeu UWP, comme la gestion de l’activation de la fenêtre principale et l’assurance que ce jeu peut gérer un événement de pause subite (et une possible reprise par la suite).

À ce stade, l’application de jeu peut gérer un message de pause (ou de reprise). Cependant, il n’existe toujours pas de fenêtre à utiliser et le jeu n’est pas initialisé. Quelques éléments supplémentaires sont nécessaires!

### <a name="appinitialize-method"></a>Méthode App::Initialize

Dans cette méthode, créez différents gestionnaires d’événements pour activer, suspendre et reprendre le jeu.

Obtenez un accès aux ressources de l’appareil. La fonction __make_shared__ est utilisée pour créer un élément __shared_ptr__ quand la ressource mémoire est créée pour la première fois. Un avantage de l’utilisation de la fonction __make_shared__ est qu’elle est sécurisée pour les exceptions. Elle utilise également le même appel pour allouer la mémoire pour le bloc de contrôle et la ressource, et réduit par conséquent la surcharge de construction.

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    // Register event handlers for app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="configure-the-window-and-display-behaviors"></a>Configurer la fenêtre et afficher les comportements

À présent, examinons l’implémentation de [__SetWindow__](https://msdn.microsoft.com/library/windows/apps/hh700509). Dans la méthode __SetWindow__, vous configurez la fenêtre et affichez les comportements.

### <a name="appsetwindow-method"></a>Méthode App::SetWindow

Le singleton de l’application fournit un objet [__CoreWindow__](https://msdn.microsoft.com/library/windows/apps/br208225) qui représente la fenêtre principale du jeu et met ses ressources et événements à la disposition du jeu. Avec l’existence d’une fenêtre à utiliser, le jeu peut maintenant commencer à ajouter à l’interface utilisateur de base des composants et événements.

Créez ensuite un pointeur à l’aide de la méthode __CoreCursor__ qui peut être utilisé à la fois par les contrôles tactiles et de souris.

Enfin, créez les événements de base pour le redimensionnement de fenêtre, la fermeture et les modifications PPP (si le périphérique d’affichage change). Pour plus d’informations sur les événements, accédez à [Gestion des événements](tutorial-game-flow-management.md#events-handling).

```cpp
void App::SetWindow(
    CoreWindow^ window
    )
{
    // Codes added to modify the original DirectX template project
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;
    // --end of codes added
    
    m_deviceResources->SetWindow(window);

    window->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);
        
    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    DisplayInformation^ currentDisplayInformation = DisplayInformation::GetForCurrentView();

    currentDisplayInformation->DpiChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDpiChanged);

    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);
    
    // Codes added to modify the original DirectX template project
    currentDisplayInformation->StereoEnabledChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnStereoEnabledChanged);
    // --end of codes added
    
    DisplayInformation::DisplayContentsInvalidated +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDisplayContentsInvalidated);
}
```

## <a name="load-method-of-the-view-provider"></a>Méthode Load du fournisseur d’affichage

Une fois la fenêtre principale définie, le singleton de l’application appelle [__Load__](https://msdn.microsoft.com/library/windows/apps/hh700501). Cette méthode utilise un ensemble de tâches asynchrones pour créer les objets jeu, charger les ressources graphiques et initialiser la machine à états du jeu. Si vous voulez commencer par récupérer des données ou des composants du jeu, faites-le ici plutôt que dans **SetWindow** ou **Initialize**. 

Comme Windows impose des restrictions sur le temps que peut prendre le jeu avant de commencer à traiter les entrées, avec le modèle de tâche asynchrone, vous devez effectuer la conception pour que la méthode __Load__ se termine rapidement afin de pouvoir commencer le traitement des entrées. Si le chargement dure un certain temps ou qu’il existe un grand nombre de ressources, octroyez à vos utilisateurs une barre de progression régulièrement mise à jour. Cette méthode est également utilisée pour effectuer toutes les préparations nécessaires avant le début du jeu, comme la définition des états de départ ou des valeurs globales.

Si vous débutez dans la programmation asynchrone et utilisez pour la première fois les concepts de parallélisme des tâches, accédez à [Programmation asynchrone en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

### <a name="appload-method"></a>Méthode App::Load

Créez la classe __GameMain__ qui contient les tâches de chargement.

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
        if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
````

### <a name="gamemain-constructor"></a>Constructeur GameMain

* Créez et initialisez le convertisseur de jeu. Pour plus d’informations, consultez [Infrastructure de rendu I: présentation du rendu](tutorial--assembling-the-rendering-pipeline.md).
* Créez et initialisez l’objet Simple3Dgame. Pour plus d’informations, consultez [Définir l’objet de jeu principal](tutorial--defining-the-main-game-loop.md).    
* Créez l’objet contrôle de l’interface utilisateur de jeu et affichez la superposition des informations du jeu pour montrer une barre de progression au fil du chargement des fichiers de ressources. Pour plus d’informations, consultez [Ajout d’une interface utilisateur](tutorial--adding-a-user-interface.md).
* Créez le contrôleur pour pouvoir lire les entrées de celui-ci (écran tactile, souris ou manette sans fil Xbox). Pour plus d’informations, consultez [Ajout de contrôles](tutorial--adding-controls.md).
* Une fois le contrôleur initialisé, nous avons défini deux zones rectangulaires dans les coins inférieurs droit et gauche de l’écran pour les contrôles tactiles de déplacement et de la caméra, respectivement. Le joueur utilise le rectangle inférieur gauche, défini par l’appel de la méthode **SetMoveRect**, comme un pavé de contrôle virtuel pour déplacer la caméra vers l’avant et l’arrière, ou vers la gauche et la droite. Le rectangle inférieur droit, défini par la méthode **SetFireRect**, sert de bouton virtuel pour tirer des munitions.
* Utilisez __create_task__ et __create_task::then__ pour scinder le chargement des ressources en deux étapes. Étant donné que l’accès au contexte de périphérique Direct3D 11 est limité au thread sur lequel le contexte de périphérique a été créé, tandis que l’accès au périphérique Direct3D11 pour la création d’objet est dépourvu de thread, cela signifie que la tâche **CreateGameDeviceResourcesAsync** peut s’exécuter sur un thread séparé de la tâche de fin (*FinalizeCreateGameDeviceResources*), qui s’exécute sur le thread d’origine. Nous utilisons un modèle semblable pour charger les ressources de niveau avec **LoadLevelAsync** et **FinalizeLoadLevel**.

```cpp
GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = ref new GameRenderer(m_deviceResources);
    m_game = ref new Simple3DGame();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = ref new MoveLookController(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameConstants::TouchRectangleSize, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

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

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game so spin up the async task to load the level.
            return m_game->LoadLevelAsync().then([this]()
            {
                // The m_game object may need to deal with D3D device context work so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_game->SetCurrentLevelToSavedState();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
        else
        {
            // The game is not in the middle of a level so there aren't any level
            // resources to load.  Creating a no-op task so that in both cases
            // the same continuation logic is used.
            return create_task([]()
            {
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        // Since Game loading is an async task, the app visual state
        // may be too small or not have focus.  Put the state machine
        // into the correct state to reflect these cases.

        if (m_deviceResources->GetLogicalSize().Width < GameConstants::MinPlayableWidth)
        {
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::TooSmall;
            m_controller->Active(false);
            m_uiControl->HideGameInfoOverlay();
            m_uiControl->ShowTooSmall();
            m_renderNeeded = true;
        }
        else if (!m_haveFocus)
        {
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            m_controller->Active(false);
            m_uiControl->SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
        }
    }, task_continuation_context::use_current());
}
```

## <a name="run-method-of-the-view-provider"></a>Méthode Run du fournisseur d’affichage

Les trois méthodes antérieures, __Initialize__, __SetWindow__ et __Load__ ont préparé le terrain. À présent, le jeu peut exécuter la méthode **Run** et il est temps de s’amuser! Les événements qu’il utilise pour basculer entre les états sont distribués et traités. Les graphiques sont mis à jour quand la boucle de jeu effectue une itération.

### <a name="apprun"></a>App::Run

Démarrez une boucle __while__ qui se termine lorsque le joueur ferme la fenêtre du jeu.

L’exemple de code passe dans l’un des deux états de la machine à états du moteur de jeu:
    * __Deactivated__: La fenêtre de jeu est désactivée (perd le focus) ou ancrée. Lorsque cette situation se produit, le jeu interrompt le traitement des événements et attend que la fenêtre ait de nouveau le focus ou ne soit plus ancrée.
    * __TooSmall__: Le jeu met à jour son propre état et restitue le graphique pour affichage.

Lorsque votre jeu a le focus, vous devez gérer chaque événement qui arrive dans la file d’attente de messages, et vous devez donc appeler [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) avec l’option **ProcessAllIfPresent**. D’autres options peuvent provoquer des retards dans le traitement des événements de message, ce qui peut donner la sensation que votre jeu ne répond pas ou que les comportements tactiles sont au ralenti au lieu d’être réactifs.

Quand le jeu est invisible, suspendu ou ancré, vous ne voulez pas qu’il utilise des ressources qui tournent en boucle pour envoyer des messages qui n’arriveront jamais. Dans ce cas, votre jeu doit utiliser **ProcessOneAndAllPending**, qui opère un blocage tant qu’il ne reçoit pas d‘événement, puis traite cet événement et tous les autres qui arrivent dans la file d’attente de traitement pendant le traitement du premier. [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) est ensuite immédiatement de retour une fois que la file d’attente a été traitée.

```cpp
void App::Run()
{
    m_main->Run();
}

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
                if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    WaitingForResourceLoading();
                    m_renderNeeded = true;
                }
                else if (m_updateStateNext == UpdateEngineState::ResourcesLoaded)
                {
                    // In the device lost case, we transition to the final waiting state
                    // and make sure the display is updated.
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
                    m_updateStateNext = UpdateEngineState::WaitingForPress;
                    m_uiControl->ShowGameInfoOverlay();
                    m_renderNeeded = true;
                }

                if (!m_renderNeeded)
                {
                    // The App is not currently the active window and not in a transient state so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
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

## <a name="uninitialize-method-of-the-view-provider"></a>Méthode Uninitialize du fournisseur d’affichage

Quand l’utilisateur met finalement fin à la session de jeu, nous devons nettoyer. C’est ici que la méthode **Uninitialize** intervient.

Dans Windows 10, la fermeture de la fenêtre d’application ne met pas fin du processus de l’application, mais écrit en revanche l’état du singleton de l’application en mémoire. Si quelque chose de spécial doit se produire lorsque le système doit récupérer cette mémoire, notamment un nettoyage spécifique des ressources, placez le code de ce nettoyage dans cette méthode.

### <a name="app-uninitialize"></a>App:: Uninitialize

```cpp
void App::Uninitialize()
{
}
```

## <a name="tips"></a>Conseils

Lors du développement de votre propre jeu, concevez votre code de démarrage autour de ces méthodes. Voici une liste de suggestions de base pour chaque méthode:

-   Utilisez **Initialize** pour allouer vos classes principales et connecter les gestionnaires d’événements de base.
-   Utilisez **SetWindow** pour créer votre fenêtre d’application principale et connecter tous les événements propres à la fenêtre.
-   Utilisez **Load** pour gérer tout le reste de l’installation, notamment pour commencer la création asynchrone d’objets et le chargement des ressources. Si vous devez créer des données ou fichiers temporaires, par exemple des composants générés par procédure, faites-le ici.


## <a name="next-steps"></a>Étapes suivantes

Nous avons couvert la structure de base d’un jeu UWP avec DirectX. Gardez ces cinq méthodes à l’esprit, car nous allons y faire référence dans d’autres parties de cette procédure pas à pas. Nous allons ensuite examiner de façon détaillée le mode de gestion des états de jeu et des événements pour poursuivre le jeu sous [Gestion du flux de jeu](tutorial-game-flow-management.md).



