---
title: Ajout d’entrées et d’interactivité à l’exemple Marble Maze
description: Découvrez les pratiques clés à prendre en compte quand vous utilisez des périphériques d’entrée.
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, jeux, entrée, exemple
ms.localizationpriority: medium
ms.openlocfilehash: d545f696a93bfa8416e1a772ecc015867a3615c2
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2019
ms.locfileid: "9045445"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Ajout d’entrées et d’interactivité à l’exemple Marble Maze




Les jeux de plateforme Windows universelle (UWP) s’exécutent sur différents appareils, tels que des ordinateurs de bureau, des ordinateurs portables et des tablettes. Un appareil peut utiliser de nombreux mécanismes de contrôle et d’entrée. Ce document décrit les pratiques clés à prendre en compte quand vous utilisez des périphériques d’entrée et comment appliquer ces pratiques à l’exemple Marble Maze.

> [!NOTE]
> L’exemple de code correspondant à ce document est disponible dans [l’exemple de jeu Marble Maze en DirectX](https://go.microsoft.com/fwlink/?LinkId=624011).

 
Voici quelques éléments clés présentés dans ce document que vous devez prendre en compte quand vous travaillez avec les entrées dans votre jeu:

-   Quand cela est possible, il est important de prendre en charge différents périphériques d’entrée afin que votre jeu puisse fonctionner avec un plus grand nombre de préférences et fonctionnalités choisies par vos utilisateurs. Bien que l’utilisation d’un contrôleur de jeu et d’un capteur soit facultative, nous recommandons cette utilisation pour améliorer l’expérience des joueurs. Nous avons conçu les API du contrôleur de jeu et du capteur pour vous aider à intégrer ces périphériques d’entrée.

-   Pour initialiser les entrées tactiles, vous devez enregistrer des événements de fenêtre, par exemple quand le pointeur est activé, libéré et déplacé. Pour initialiser l’accéléromètre, créez un objet [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687) quand vous initialisez l’application. La manette Xbox ne nécessite pas cette initialisation.

-   Pour des jeux à un seul joueur, pensez à combiner les entrées de toutes les manettes Xbox possibles. De cette façon, vous n’avez pas besoin d’effectuer un suivi pour savoir de quel contrôleur provient l’entrée. Il vous suffit de suivre l’entrée provenant du contrôleur ajouté en dernier, comme nous le faisons dans cet exemple.

-   Traitez les événements Windows avant de traiter les périphériques d’entrée.

-   La manette Xbox et l’accéléromètre prennent en charge l’interrogation (polling). C’est-à-dire que vous pouvez demander les données dont vous avez besoin. Pour les entrées tactiles, enregistrez les événements tactiles dans des structures de données disponibles à votre code de traitement des entrées.

-   Envisagez de normaliser les valeurs d’entrée pour utiliser un format unique. Cela permet de simplifier l’interprétation des entrées par d’autres composants de votre jeu, par exemple des simulations physiques, et de faciliter l’écriture de jeux qui fonctionnent sur différentes résolutions d’écran.

## <a name="input-devices-supported-by-marble-maze"></a>Périphériques d’entrée pris en charge par Marble Maze


Marble Maze prend en charge les périphériques de manette Xbox standard, la souris et les entrées tactiles pour sélectionner des éléments de menu, et la manette Xbox, la souris et les entrées tactiles et l’accéléromètre pour le contrôle du jeu. Marble Maze utilise les API [Windows::Gaming::Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) pour interroger la manette sur les entrées. Les fonctions tactiles permettent aux applications de suivre et de répondre aux entrées tactiles. Un accéléromètre est un capteur qui mesure la force appliquée le long des axes x, y et z. En utilisant Windows Runtime, vous pouvez interroger l’état actuel de l’accéléromètre, et recevoir des événements tactiles via le mécanisme de gestion des événements du Windows Runtime.

> [!NOTE]
> Ce document utilise l’interaction tactile pour faire référence à la fois aux entrées tactiles et aux entrées de la souris, et le pointeur pour faire référence à tous les appareils qui utilisent des événements de pointeur. Dans la mesure où l’interaction tactile et la souris utilisent des événements de pointeur standards, vous pouvez utiliser l’un ou l’autre appareil pour sélectionner les éléments de menu et contrôler le jeu.

 

> [!NOTE]
> Le manifeste du package définit **Paysage** comme la rotation prise en charge pour le jeu afin d’empêcher un changement d’orientation quand vous faites pivoter l’appareil pour faire rouler la bille. Ouvrez le fichier **Package.appxmanifest** dans l’**explorateur de solutions** dans Visual Studio pour afficher le manifeste du package.

 

## <a name="initializing-input-devices"></a>Initialisation des périphériques d’entrée


La manette Xbox ne nécessite pas cette initialisation. Pour initialiser l’interaction tactile, vous devez enregistrer les événements de fenêtre: l’activation du pointeur (par exemple, le joueur appuie sur le bouton de la souris ou touche l’écran), la libération du pointeur ou le déplacement. Pour initialiser l’accéléromètre, vous devez créer un objet [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687) quand vous initialisez l’application.

L’exemple suivant montre la manière dont la méthode **App::SetWindow** permet d’inscrire le [Windows::UI::Core::CoreWindow::PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerPressed), [Windows::UI::Core::CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerReleased) et les événements de pointeur [Windows::UI::Core::CoreWindow::PointerMoved](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerMoved). Ces événements sont inscrits lors de l’initialisation de l’application et avant la boucle de jeu.

Ils sont gérés dans un thread distinct, chargé d’appeler les gestionnaires d’événements.

Pour plus d’informations sur l’initialisation de l’application, voir [Structure de l’application Marble Maze](marble-maze-application-structure.md).

```cpp
window->PointerPressed += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerPressed);

window->PointerReleased += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerReleased);

window->PointerMoved += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerMoved);
```

La classe **MarbleMazeMain** crée également un objet **std::map** pour contenir les événements tactiles. La clé de cet objet map est une valeur qui identifie de façon unique le pointeur d’entrée. Chaque clé mappe la distance entre chaque point tactile et le centre de l’écran. Marble Maze utilise ensuite ces valeurs pour calculer l’inclinaison du labyrinthe.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

La classe **MarbleMazeMain** contient également un objet [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer).

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

L’objet **Accelerometer** est initialisé dans le constructeur **MarbleMazeMain**, comme le montre l’exemple suivant. La méthode [Windows::Devices::Sensors::Accelerometer::GetDefault](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault) renvoie une instance de l’accéléromètre par défaut. Si aucun accéléromètre par défaut n’existe, la valeur**Accelerometer::GetDefault** renvoie **nullptr**.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>Navigation dans les menus

Vous pouvez utiliser la souris, les entrées tactiles ou la manette Xbox pour naviguer dans les menus, comme indiqué ci-dessous:

-   Utilisez le pavé directionnel pour changer l’élément de menu actif.
-   Utilisez l’interaction tactile, le bouton A ou le bouton Menu pour sélectionner un élément de menu ou fermer le menu actif, par exemple le tableau des meilleurs scores.
-   Utilisez le bouton Menu pour interrompre ou reprendre le jeu.
-   Cliquez sur un élément de menu avec la souris pour choisir cette action.

###  <a name="tracking-xbox-controller-input"></a>Suivi des entrées de la manette Xbox

Pour effectuer le suivi des boîtiers de commande actuellement connectés à l’appareil, **MarbleMazeMain** définit une variable de membre, **m_myGamepads**, qui est une collection d’objets [Windows::Gaming::Input::Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad). Elle est initialisée dans le constructeur comme suit:

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

En outre, le constructeur **MarbleMazeMain** enregistre les événements quand un boîtier de commande est ajouté ou supprimé:

```cpp
Gamepad::GamepadAdded += 
    ref new EventHandler<Gamepad^>([=](Platform::Object^, Gamepad^ args)
{
    m_myGamepads->Append(args);
    m_currentGamepadNeedsRefresh = true;
});

Gamepad::GamepadRemoved += 
    ref new EventHandler<Gamepad ^>([=](Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        m_myGamepads->RemoveAt(indexRemoved);
        m_currentGamepadNeedsRefresh = true;
    }
});
```

Quand un boîtier de commande est ajouté, il est ajouté à **m_myGamepads**; quand un boîtier de commande est supprimé, nous vérifions s’il se trouve dans **m_myGamepads**, et si c’est le cas, il est supprimé. Dans les deux cas, nous définissons **m_currentGamepadNeedsRefresh** sur **true**, ce qui indique que nous devons réaffecter **m_gamepad**.

Enfin, nous affectons un boîtier de commande à **m_gamepad** et nous définissons **m_currentGamepadNeedsRefresh** sur **false**:

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

Dans la méthode **Update**, nous vérifions si **m_gamepad** doit être réattribué:

```cpp
if (m_currentGamepadNeedsRefresh)
{
    auto mostRecentGamepad = GetLastGamepad();

    if (m_gamepad != mostRecentGamepad)
    {
        m_gamepad = mostRecentGamepad;
    }

    m_currentGamepadNeedsRefresh = false;
}
```

Si **m_gamepad** doit être réattribué, nous l’attribuons au boîtier de commande ajouté en dernier, à l’aide de **GetLastGamepad**, qui est défini comme suit:

```cpp
Gamepad^ MarbleMaze::MarbleMazeMain::GetLastGamepad()
{
    Gamepad^ gamepad = nullptr;

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(m_myGamepads->Size - 1);
    }

    return gamepad;
}
```

Cette méthode retourne simplement le dernier boîtier de commande dans **m_myGamepads**.

Vous pouvez connecter jusqu’à quatre manettes Xbox à un appareil Windows10. Pour ne pas avoir à chercher quelle manette est active, nous suivons simplement uniquement le boîtier de commande ajouté en dernier. Si votre jeu prend en charge plusieurs joueurs, vous devez suivre les entrées de chaque joueur séparément.

La méthode **MarbleMazeMain::Update** interroge les entrées du boîtier de commande:

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

Nous suivons la lecture de l’entrée obtenue dans la dernière image avec **m_oldReading**, et la dernière lecture de l’entrée avec **m_newReading**, obtenue en appelant [Gamepad::GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading). Cela renvoie un objet [GamepadReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadreading), qui contient des informations sur l’état actuel du boîtier de commande.

Pour vérifier si un bouton a été simplement enfoncé ou relâché, nous définissons **MarbleMazeMain::ButtonJustPressed** et **MarbleMazeMain::ButtonJustReleased**, qui comparent des lectures de bouton à partir de cette image et de la dernière. Ainsi, nous pouvons effectuer une action seulement au moment où l’utilisateur appuie sur un bouton ou le relâche, et non quand il maintient la pression dessus:

```cpp
bool MarbleMaze::MarbleMazeMain::ButtonJustPressed(GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (m_newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (m_oldReading.Buttons & selection));
    return newSelectionPressed && !oldSelectionPressed;
}

bool MarbleMaze::MarbleMazeMain::ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased = 
        (GamepadButtons::None == (m_newReading.Buttons & selection));

    bool oldSelectionReleased = 
        (GamepadButtons::None == (m_oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Les lectures [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons) sont comparées à l’aide d’opérations au niveau du bit&mdash;nous vérifions si un bouton a été enfoncé à l’aide de l’opération *AND au niveau du bit* (&). Nous déterminons si un bouton a simplement été enfoncé ou relâché en comparant l’ancienne lecture et la nouvelle.

En utilisant les méthodes décrites précédemment, nous vérifions si certains boutons ont été enfoncés et nous effectuons toutes les actions correspondantes. Par exemple, lors d’un appui sur le bouton Menu (**GamepadButtons::Menu**), l’état du jeu passe d’actif à interrompu, ou inversement.

```cpp
if (ButtonJustPressed(GamepadButtons::Menu) || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;

    if (m_gameState == GameState::InGameActive)
    {
        SetGameState(GameState::InGamePaused);
    }  
    else if (m_gameState == GameState::InGamePaused)
    {
        SetGameState(GameState::InGameActive);
    }
}
```

Nous vérifions également si le joueur appuie sur le bouton d’affichage, auquel cas nous redémarrons le jeu ou nous effaçons le tableau des meilleurs scores:

```cpp
if (ButtonJustPressed(GamepadButtons::View) || m_homeKeyPressed)
{
    m_homeKeyPressed = false;

    if (m_gameState == GameState::InGameActive ||
        m_gameState == GameState::InGamePaused ||
        m_gameState == GameState::PreGameCountdown)
    {
        SetGameState(GameState::MainMenu);
        m_inGameStopwatchTimer.SetVisible(false);
        m_preGameCountdownTimer.SetVisible(false);
    }
    else if (m_gameState == GameState::HighScoreDisplay)
    {
        m_highScoreTable.Reset();
    }
}
```

Si le menu principal est actif, l’élément de menu actif change lors d’un appui sur le pavé directionnel. Si l’utilisateur choisit la sélection actuelle, l’élément de l’interface utilisateur approprié est marqué comme ayant été choisi.

```cpp
// Handle menu navigation.
bool chooseSelection = 
    (ButtonJustPressed(GamepadButtons::A) 
    || ButtonJustPressed(GamepadButtons::Menu));

bool moveUp = ButtonJustPressed(GamepadButtons::DPadUp);
bool moveDown = ButtonJustPressed(GamepadButtons::DPadDown);

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);
        if (m_startGameButton.GetSelected())
        {
            m_startGameButton.SetPressed(true);
        }
        if (m_highScoreButton.GetSelected())
        {
            m_highScoreButton.SetPressed(true);
        }
    }
    if (moveUp || moveDown)
    {
        m_startGameButton.SetSelected(!m_startGameButton.GetSelected());
        m_highScoreButton.SetSelected(!m_startGameButton.GetSelected());
        m_audio.PlaySoundEffect(MenuChangeEvent);
    }
    break;

case GameState::HighScoreDisplay:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::MainMenu);
    }
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::HighScoreDisplay);
    }
    break;

case GameState::InGamePaused:
    if (m_pausedText.IsPressed())
    {
        m_pausedText.SetPressed(false);
        SetGameState(GameState::InGameActive);
    }
    break;
}
```

### <a name="tracking-touch-and-mouse-input"></a>Suivi des entrées tactiles et de la souris

Pour les entrées tactiles et de la souris, un élément de menu est choisi quand l’utilisateur le touche ou clique dessus. L’exemple suivant montre comment la méthode **MarbleMazeMain::Update** traite les entrées de pointeur pour sélectionner des éléments de menu. La variable membre **m_pointQueue** relève les emplacements que l’utilisateur a touchés ou sur lesquels il a cliqué. La façon dont Marble Maze collecte les entrées de pointeur est décrite en détail plus loin dans ce document, dans la section [Traitement des entrées de pointeur](#processing-pointer-input).

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

La méthode **UserInterface::HitTest** détermine si le point fourni est situé dans les limites d’un élément d’interface. Les éléments d’interface qui réussissent ce test sont marqués comme ayant été touchés. Cette méthode utilise la fonction d’assistance **PointInRect** pour déterminer si le point fourni se trouve dans les limites de chaque élément d’interface utilisateur.

```cpp
void UserInterface::HitTest(D2D1_POINT_2F point)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if (!(*iter)->IsVisible())
            continue;

        TextButton* textButton = dynamic_cast<TextButton*>(*iter);
        if (textButton != nullptr)
        {
            D2D1_RECT_F bounds = (*iter)->GetBounds();
            textButton->SetPressed(PointInRect(point, bounds));
        }
    }
}
```

### <a name="updating-the-game-state"></a>Mise à jour de l’état du jeu

Une fois que la méthode **MarbleMazeMain::Update** a traité les entrées de la manette et les entrées tactiles, elle met à jour l’état du jeu si un bouton a été actionné.

```cpp
// Update the game state if the user chose a menu option. 
if (m_startGameButton.IsPressed())
{
    SetGameState(GameState::PreGameCountdown);
    m_startGameButton.SetPressed(false);
}
if (m_highScoreButton.IsPressed())
{
    SetGameState(GameState::HighScoreDisplay);
    m_highScoreButton.SetPressed(false);
}
```

##  <a name="controlling-game-play"></a>Contrôle du jeu


La boucle du jeu et la méthode **MarbleMazeMain::Update** permettent, ensemble, de mettre à jour l’état des objets du jeu. Si votre jeu accepte des entrées de plusieurs périphériques, vous pouvez cumuler les entrées de tous ces périphériques dans un ensemble unique de variables, ce qui vous permet d’écrire du code plus facile à mettre à jour. La méthode **MarbleMazeMain::Update** définit un ensemble de variables qui accumulent le mouvement de tous les périphériques.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

Le mécanisme d’entrée n’est pas toujours le même d’un périphérique à l’autre. Par exemple, l’entrée du pointeur est gérée en utilisant le modèle de gestion des événements du Windows Runtime. Inversement, vous interrogez les données d’entrée à partir de la manette Xbox quand vous en avez besoin. Nous recommandons de toujours suivre le mécanisme d’entrée conseillé pour un périphérique donné. Cette section décrit comment Marble Maze lit les entrées de chaque périphérique, comment il met à jour les valeurs d’entrée combinées et comment il utilise ces valeurs pour mettre à jour l’état du jeu.

###  <a name="processing-pointer-input"></a>Traitement des entrées de pointeur

Quand vous utilisez des entrées de pointeur, appelez la méthode [Windows::UI::Core::CoreDispatcher::ProcessEvents](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) pour traiter les événements de fenêtre. Appelez cette méthode dans la boucle de votre jeu avant de mettre à jour ou de générer le rendu de la scène. Marble Maze appelle cela dans la méthode **App::Run**: 

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
    else
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

Si la fenêtre est visible, nous transmettons **CoreProcessEventsOption::ProcessAllIfPresent** à **ProcessEvents** pour traiter tous les événements en file d’attente et nous effectuons un retour immédiat; dans le cas contraire, nous transmettons **CoreProcessEventsOption::ProcessOneAndAllPending** pour traiter tous les événements en file d’attente et nous attendons le nouvel événement suivant. Une fois tous les événements traités, Marble Maze génère un rendu de l’image suivante et la dévoile.

Windows Runtime appelle le gestionnaire enregistré pour chaque événement qui se produit. La méthode **App::SetWindow** enregistre les événements et transmet les informations du pointeur à la classe **MarbleMazeMain**.

```cpp
void App::OnPointerPressed(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void App::OnPointerReleased(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->RemoveTouch(args->CurrentPoint->PointerId);
}

void App::OnPointerMoved(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

La classe **MarbleMazeMain** réagit aux événements de pointeur en mettant à jour l’objet map qui contient les événements tactiles. La méthode **MarbleMazeMain::AddTouch** est appelée lors d’un premier appui sur le pointeur, par exemple quand l’utilisateur touche l’écran pour la première fois sur un appareil tactile. La méthode **MarbleMazeMain::UpdateTouch** est appelée lorsque le pointeur se déplace. La méthode **MarbleMazeMain::RemoveTouch** est appelée quand le pointeur est libéré, par exemple, quand l’utilisateur ne touche plus l’écran.

```cpp
void MarbleMazeMain::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMazeMain::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());
}

void MarbleMazeMain::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

La fonction **PointToTouch** traduit la position actuelle du pointeur afin que l’origine se trouve au centre de l’écran, puis met à l’échelle les coordonnées dans une plage comprise entre -1,0 et +1,0. Cela permet de calculer l’inclinaison du labyrinthe, quelle que soit la méthode d’entrée.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

La méthode **MarbleMazeMain::Update** met à jour les valeurs d’entrée combinées en incrémentant le facteur d’inclinaison d’une valeur de mise à l’échelle constante. Cette valeur de mise à l’échelle a été identifiée en testant différentes valeurs.

```cpp
// Account for touch input.
for (TouchMap::const_iterator iter = m_touches.cbegin(); 
    iter != m_touches.cend(); 
    ++iter)
{
    combinedTiltX += iter->second.x * m_touchScaleFactor;
    combinedTiltY += iter->second.y * m_touchScaleFactor;
}
```

### <a name="processing-accelerometer-input"></a>Traitement des entrées d’accéléromètre

Pour traiter les entrées d’accéléromètre, la méthode **MarbleMazeMain::Update** appelle la méthode [Windows::Devices::Sensors::Accelerometer::GetCurrentReading](https://msdn.microsoft.com/library/windows/apps/br225699). Cette méthode renvoie un objet [Windows::Devices::Sensors::AccelerometerReading](https://msdn.microsoft.com/library/windows/apps/br225688), qui représente une lecture de l’accéléromètre. Les propriétés **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** et **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** contiennent respectivement l’accélération de la force g le long des axes x et y.

L’exemple suivant montre comment la méthode **MarbleMazeMain::Update** interroge l’accéléromètre et met à jour les valeurs d’entrée combinées. Quand vous inclinez l’appareil, la bille va plus vite en raison de la gravité.

```cpp
// Account for sensors.
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += 
            static_cast<float>(reading->AccelerationX) * m_accelerometerScaleFactor;

        combinedTiltY += 
            static_cast<float>(reading->AccelerationY) * m_accelerometerScaleFactor;
    }
}
```

Dans la mesure où vous ne pouvez pas savoir si un accéléromètre est présent sur l’ordinateur de l’utilisateur, vérifiez toujours qu’un objet [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) valide existe avant d’interroger l’accéléromètre.

### <a name="processing-xbox-controller-input"></a>Traitement des entrées de la manette Xbox

Dans la méthode **MarbleMazeMain::Update**, nous utilisons **m_newReading** pour traiter les entrées à partir du stick analogique gauche:

```cpp
float leftStickX = static_cast<float>(m_newReading.LeftThumbstickX);
float leftStickY = static_cast<float>(m_newReading.LeftThumbstickY);

auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

if ((oppositeSquared + adjacentSquared) > m_deadzoneSquared)
{
    combinedTiltX += leftStickX * m_controllerScaleFactor;
    combinedTiltY += leftStickY * m_controllerScaleFactor;
}
```

Nous vérifions si l’entrée provenant du stick analogique gauche est en dehors de la zone morte, et si c’est le cas, nous l’ajoutons à **combinedTiltX** et à **combinedTiltY** (multipliée par un facteur d’échelle) pour incliner la scène.

> [!IMPORTANT]
> Quand vous utilisez la manette Xbox, n’oubliez pas la zone morte. La zone morte fait référence aux différences de sensibilité au mouvement initial des boîtiers de commande. Pour certaines manettes, un mouvement léger peut ne pas générer de lecture, pour d’autres, une lecture mesurable peut être obtenue. Pour tenir compte de cela dans votre jeu, créez une zone de non-mouvement pour le mouvement initial du stick. Pour plus d’informations sur la zone morte, consultez la rubrique [Lecture des entrées des sticks analogiques](gamepad-and-vibration.md#reading-the-thumbsticks).

 

###  <a name="applying-input-to-the-game-state"></a>Application des entrées à l’état du jeu

Les périphériques indiquent les valeurs d’entrée de différentes manières. Par exemple, les entrées du pointeur peuvent être des coordonnées d’écran, tandis que les entrées de manette peuvent être dans un format complètement différent. L’un des problèmes liés à la combinaison des entrées de plusieurs périphériques en un ensemble de valeurs d’entrée est la normalisation, c’est-à-dire la conversion de valeurs dans un format commun. Marble Maze normalise les valeurs en les mettant à l’échelle dans la plage [-1,0, 1,0]. La fonction **PointToTouch**, décrite précédemment dans cette section, convertit les coordonnées écran en valeurs normalisées comprises dans la plage -1,0 et +1,0.

> [!TIP]
> Même si votre application utilise une méthode d’entrée, nous recommandons toujours de normaliser les valeurs d’entrée. Cela permet de simplifier l’interprétation des entrées par d’autres composants de votre jeu, par exemple des simulations physiques, et de faciliter l’écriture de jeux qui fonctionnent sur différentes résolutions d’écran.

 

Une fois que la méthode **MarbleMazeMain::Update** a traité les entrées, elle crée un vecteur qui représente l’effet de l’inclinaison du labyrinthe sur la bille. L’exemple suivant montre comment Marble Maze utilise la fonction [XMVector3Normalize](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.geometric.xmvector3normalize) pour créer un vecteur de gravité normalisé. La variable **maxTilt** définit l’inclinaison du labyrinthe et empêche son retournement.

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

Pour terminer la mise à jour des objets de la scène, Marble Maze transmet le vecteur de gravité mis à jour à la simulation physique, met à jour la simulation physique pour le temps qui s’est écoulé depuis l’image précédente, puis met à jour la position et l’orientation de la bille. Si la bille tombe du labyrinthe, la méthode **MarbleMazeMain::Update** la place au dernier point de contrôle touché par la bille et réinitialise l’état de la simulation physique.

```cpp
XMFLOAT3A g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);

if (m_gameState == GameState::InGameActive)
{
    // Only update physics when gameplay is active.
    m_physics.UpdatePhysicsSimulation(static_cast<float>(m_timer.GetElapsedSeconds()));

    // ...Code omitted for simplicity...

}

// ...Code omitted for simplicity...

// Check whether the marble fell off of the maze. 
const float fadeOutDepth = 0.0f;
const float resetDepth = 80.0f;
if (marblePosition.z >= fadeOutDepth)
{
    m_targetLightStrength = 0.0f;
}
if (marblePosition.z >= resetDepth)
{
    // Reset marble.
    memcpy(&marblePosition, &m_checkpoints[m_currentCheckpoint], sizeof(XMFLOAT3));
    oldMarblePosition = marblePosition;
    m_physics.SetPosition((const XMFLOAT3&)marblePosition);
    m_physics.SetVelocity(XMFLOAT3(0, 0, 0));
    m_lightStrength = 0.0f;
    m_targetLightStrength = 1.0f;

    m_resetCamera = true;
    m_resetMarbleRotation = true;
    m_audio.PlaySoundEffect(FallingEvent);
}
```

Cette section ne décrit pas le fonctionnement de la simulation physique. Pour plus d’informations, voir **Physics.h** et **Physics.cpp** dans les sources Marble Maze.

## <a name="next-steps"></a>Étapes suivantes


Consultez [Ajout d’audio à l’exemple Marble Maze](adding-audio-to-the-marble-maze-sample.md) pour obtenir des informations sur les pratiques clés en matière d’audio. Ce document décrit comment Marble Maze utilise Microsoft Media Foundation et XAudio2 pour charger, mixer et lire des ressources audio.

## <a name="related-topics"></a>Rubriques connexes


* [Ajout de son à l’exemple Marble Maze](adding-audio-to-the-marble-maze-sample.md)
* [Ajout de contenu visuel à l’exemple Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Développement de Marble Maze, jeu pour UWP en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




