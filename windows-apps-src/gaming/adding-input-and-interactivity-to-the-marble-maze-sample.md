---
author: mtoepke
title: "Ajout d’entrées et d’interactivité à l’exemple Marble Maze"
description: "Les jeux d’application de plateforme Windows universelle (UWP) s’exécutent sur différents appareils, tels que des ordinateurs de bureau, des ordinateurs portables et des tablettes."
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, jeux, entrée, exemple"
ms.openlocfilehash: dc667be326950151b08bbaded6d4e9a0b109523b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Ajout d’entrées et d’interactivité à l’exemple Marble Maze


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Les jeux d’application de plateforme Windows universelle (UWP) s’exécutent sur différents appareils, tels que des ordinateurs de bureau, des ordinateurs portables et des tablettes. Un appareil peut utiliser différents types de mécanismes de contrôle et d’entrée. Il est important de prendre en charge plusieurs périphériques d’entrée afin que votre jeu puisse fonctionner avec un plus grand nombre de préférences et de fonctionnalités choisies par vos utilisateurs. Ce document décrit les pratiques clés à prendre en compte quand vous utilisez des périphériques d’entrée et comment appliquer ces pratiques à l’exemple Marble Maze.

> **Remarque**   L’exemple de code correspondant à ce document est disponible dans l’[exemple de jeu Marble Maze en DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
Voici quelques éléments clés présentés dans ce document que vous devez prendre en compte quand vous travaillez avec les entrées dans votre jeu:

-   Quand cela est possible, il est important de prendre en charge différents périphériques d’entrée afin que votre jeu puisse fonctionner avec un plus grand nombre de préférences et fonctionnalités choisies par vos utilisateurs. Bien que l’utilisation d’un contrôleur de jeu et d’un capteur soit facultative, nous recommandons cette utilisation pour améliorer l’expérience des joueurs. Nous avons conçu l’API du contrôleur de jeu et du capteur pour vous aider à intégrer ces périphériques d’entrée.
-   Pour initialiser les entrées tactiles, vous devez enregistrer des événements de fenêtre, par exemple quand le pointeur est activé, libéré et déplacé. Pour initialiser l’accéléromètre, créez un objet [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687) quand vous initialisez l’application. La manette Xbox 360 ne nécessite pas cette initialisation.
-   Pour des jeux à un seul joueur, pensez à combiner les entrées de toutes les manettes Xbox 360 possibles. De cette façon, vous n’avez pas besoin d’effectuer un suivi pour savoir de quel contrôleur provient l’entrée.
-   Traitez les événements de fenêtre avant de traiter les périphériques d’entrée.
-   La manette Xbox 360 et l’accéléromètre prennent en charge l’interrogation (polling). C’est-à-dire que vous pouvez demander les données dont vous avez besoin. Pour les entrées tactiles, enregistrez les événements tactiles dans des structures de données disponibles à votre code de traitement des entrées.
-   Envisagez de normaliser les valeurs d’entrée pour utiliser un format unique. Cela permet de simplifier l’interprétation des entrées par d’autres composants de votre jeu, par exemple des simulations physiques, et de faciliter l’écriture de jeux qui fonctionnent sur différentes résolutions d’écran.

## <a name="input-devices-supported-by-marble-maze"></a>Périphériques d’entrée pris en charge par Marble Maze


Marble Maze prend en charge les périphériques de manette Xbox 360 standard, la souris et les entrées tactiles pour sélectionner des éléments de menu, et la manette Xbox 360, la souris et les entrées tactiles et l’accéléromètre pour le contrôle du jeu. Marble Maze utilise l’API XInput pour interroger la manette sur les entrées. Les fonctions tactiles permettent aux applications de suivre et de répondre aux entrées tactiles. Un accéléromètre est un capteur qui mesure la force appliquée le long des axes x, y et z. En utilisant Windows Runtime, vous pouvez interroger l’état actuel de l’accéléromètre, et recevoir des événements tactiles via le mécanisme de gestion des événements du Windows Runtime.

> **Remarque**  Ce document utilise l’interaction tactile pour faire référence à la fois aux entrées tactiles et aux entrées de la souris, et le pointeur pour faire référence à tous les appareils qui utilisent des événements de pointeur. Dans la mesure où l’interaction tactile et la souris utilisent des événements de pointeur standards, vous pouvez utiliser l’un ou l’autre périphérique pour sélectionner les éléments de menu et contrôler le jeu.

 

> **Remarque**  Le manifeste du package définit Paysage comme la rotation prise en charge pour le jeu afin d’empêcher un changement d’orientation quand vous faites pivoter le périphérique pour faire rouler la bille.

 

## <a name="initializing-input-devices"></a>Initialisation des périphériques d’entrée


La manette Xbox 360 ne nécessite pas cette initialisation. Pour initialiser l’interaction tactile, vous devez enregistrer les événements de fenêtre : l’activation du pointeur (par exemple, un utilisateur appuie sur le bouton de la souris ou touche l’écran), la libération du pointeur ou le déplacement. Pour initialiser l’accéléromètre, vous devez créer un objet [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687) quand vous initialisez l’application.

L’exemple suivant montre la manière dont le constructeur **DirectXPage** s’inscrit pour les événements de pointeur [**Windows::UI::Core::CoreIndependentInputSource::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn298471), [**Windows::UI::Core::CoreIndependentInputSource::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn298472), et [**Windows::UI::Core::CoreIndependentInputSource::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn298469) pour la classe [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834). Ces événements sont inscrits lors de l’initialisation de l’application et avant la boucle de jeu.

Ils sont gérés dans un thread distinct chargé d’appeler les gestionnaires d’événements.

Pour plus d’informations sur l’initialisation de l’application, voir [Structure de l’application Marble Maze](marble-maze-application-structure.md).

```cpp
coreInput->PointerPressed += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerPressed);
coreInput->PointerMoved += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerMoved);
coreInput->PointerReleased += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerReleased);
```

La classe MarbleMaze crée également un objet std::map pour contenir les événements tactiles. La clé de cet objet map est une valeur qui identifie de façon unique le pointeur d’entrée. Chaque clé mappe la distance entre chaque point tactile et le centre de l’écran. Marble Maze utilise ensuite ces valeurs pour calculer l’inclinaison du labyrinthe.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

La classe MarbleMaze contient un objet Accelerometer.

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

L’objet Accelerometer est initialisé dans la méthode MarbleMaze::Initialize, comme le montre l’exemple suivant. La méthode Windows::Devices::Sensors::Accelerometer::GetDefault renvoie une instance de l’accéléromètre par défaut. Si aucun accéléromètre par défaut n’existe, la valeur Accelerometer::GetDefault de m_accelerometer reste nullptr.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>Navigation dans les menus


###  <a name="tracking-xbox-360-controller-input"></a>Suivi des entrées de la manette Xbox 360

Vous pouvez utiliser la souris, les entrées tactiles ou la manette Xbox 360 pour naviguer dans les menus, comme indiqué ci-dessous :

-   Utilisez le pavé directionnel pour changer l’élément de menu actif.
-   Utilisez l’interaction tactile, le bouton A ou le bouton Démarrer pour sélectionner un élément de menu ou fermer le menu actif, par exemple le tableau des meilleurs scores.
-   Utilisez le bouton Démarrer pour interrompre ou reprendre le jeu.
-   Cliquez sur un élément de menu avec la souris pour choisir cette action.

###  <a name="tracking-touch-and-mouse-input"></a>Suivi des entrées tactiles et de la souris

Pour suivre les entrées de la manette Xbox 360, la méthode **MarbleMaze::Update** définit un tableau de boutons qui définit les comportements d’entrée. XInput fournit uniquement l’état actuel de la manette. C’est pourquoi **MarbleMaze::Update** définit également deux tableaux qui indiquent, pour chaque manette Xbox 360 possible, quel bouton a été actionné lors de la précédente image et quel bouton est actuellement actionné.

```cpp
// This array contains the constants from XINPUT that map to the 
// particular buttons that are used by the game. 
// The index of the array is used to associate the state of that button in 
// the wasButtonDown, isButtonDown, and combinedButtonPressed variables. 

static const WORD buttons[] = {
    XINPUT_GAMEPAD_A,
    XINPUT_GAMEPAD_START,
    XINPUT_GAMEPAD_DPAD_UP,
    XINPUT_GAMEPAD_DPAD_DOWN,
    XINPUT_GAMEPAD_DPAD_LEFT,
    XINPUT_GAMEPAD_DPAD_RIGHT,
    XINPUT_GAMEPAD_BACK,
};

static const int buttonCount = ARRAYSIZE(buttons);
static bool wasButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
bool isButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
```

Vous pouvez connecter jusqu’à quatre manettes Xbox 360 à un appareil Windows. Pour ne pas chercher quelle manette est active, la méthode **MarbleMaze::Update** combine les entrées de toutes les manettes.

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
```

Si votre jeu prend en charge plusieurs joueurs, vous devez suivre les entrées de chaque joueur séparément.

Dans une boucle, la méthode **MarbleMaze::Update** interroge les entrées de chaque manette et lit l’état de chaque bouton.

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

Après l’interrogation des entrées par la méthode **MarbleMaze::Update**, elle met à jour le tableau d’entrées combinées. Le tableau d’entrées combinées répertorie uniquement les boutons qui sont actionnés actuellement, mais qui ne l’étaient pas avant. Cela permet au jeu d’effectuer une action seulement au moment où l’utilisateur appuie sur un bouton, et non quand il maintient la pression sur ce dernier.

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
for (int i = 0; i < buttonCount; ++i)
{
    for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
    {
        bool pressed = !wasButtonDown[userIndex][i] && isButtonDown[userIndex][i];
        combinedButtonPressed[i] = combinedButtonPressed[i] || pressed;
    }
}
```

Une fois que la méthode **MarbleMaze::Update** a collecté les entrées de bouton, elle effectue les actions qui doivent se produire. Par exemple, lors d’un appui sur le bouton Démarrer (XINPUT\_GAMEPAD\_START), l’état du jeu passe d’actif à interrompu, ou inversement.

```cpp
// Check whether the user paused or resumed the game. 
// XINPUT_GAMEPAD_START  
if (combinedButtonPressed[1] || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;
    if (m_gameState == GameState::InGameActive)
        SetGameState(GameState::InGamePaused);
    else if (m_gameState == GameState::InGamePaused)
        SetGameState(GameState::InGameActive);
}
```

Si le menu principal est actif, l’élément de menu actif change lors d’un appui sur le pavé directionnel. Si l’utilisateur choisit la sélection actuelle, l’élément de l’interface utilisateur approprié est marqué comme ayant été choisi.

```cpp
// Handle menu navigation. 

// XINPUT_GAMEPAD_A or XINPUT_GAMEPAD_START 
bool chooseSelection = (combinedButtonPressed[0] || combinedButtonPressed[1]);

// XINPUT_GAMEPAD_DPAD_UP 
bool moveUp = combinedButtonPressed[2];

// XINPUT_GAMEPAD_DPAD_DOWN 
bool moveDown = combinedButtonPressed[3];                                           

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);

        if (m_startGameButton.GetSelected())
            m_startGameButton.SetPressed(true);

        if (m_highScoreButton.GetSelected())
            m_highScoreButton.SetPressed(true);
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
        SetGameState(GameState::MainMenu);
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
        SetGameState(GameState::HighScoreDisplay);
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

Une fois que la méthode **MarbleMaze::Update** a traité les entrées de la manette, elle enregistre l’état d’entrée actuel pour l’image suivante.

```cpp
// Update the button state for the next frame.
memcpy(wasButtonDown, isButtonDown, sizeof(wasButtonDown));
```

### <a name="tracking-touch-and-mouse-input"></a>Suivi des entrées tactiles et de la souris

Pour les entrées tactiles et de la souris, un élément de menu est choisi quand l’utilisateur le touche ou clique dessus. L’exemple suivant montre comment la méthode **MarbleMaze::Update** traite les entrées de pointeur pour sélectionner des éléments de menu. La variable membre **m_pointQueue** relève les emplacements que l’utilisateur a touchés ou sur lesquels il a cliqué. La façon dont Marble Maze collecte les entrées de pointeur est décrite en détail plus loin dans ce document, dans la section Traitement des entrées de pointeur.

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

Une fois que la méthode **MarbleMaze::Update** a traité les entrées de la manette et les entrées tactiles, elle met à jour l’état du jeu si un bouton a été actionné.

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


La boucle du jeu et la méthode **MarbleMaze::Update** permettent, ensemble, de mettre à jour l’état des objets du jeu. Si votre jeu accepte des entrées de plusieurs périphériques, vous pouvez cumuler les entrées de tous ces périphériques dans un ensemble unique de variables, ce qui vous permet d’écrire du code plus facile à mettre à jour. La méthode **MarbleMaze::Update** définit un ensemble de variables qui accumulent le mouvement de tous les périphériques.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

Le mécanisme d’entrée n’est pas toujours le même d’un périphérique à l’autre. Par exemple, l’entrée de pointeur est gérée en utilisant le modèle de gestion des événements du Windows Runtime. Inversement, vous interrogez les données d’entrée à partir de la manette Xbox 360 quand vous en avez besoin. Nous recommandons de toujours suivre le mécanisme d’entrée conseillé pour un périphérique donné. Cette section décrit comment Marble Maze lit les entrées de chaque périphérique, comment il met à jour les valeurs d’entrée combinées et comment il utilise ces valeurs pour mettre à jour l’état du jeu.

###  <a name="processing-pointer-input"></a>Traitement des entrées de pointeur

Quand vous utilisez des entrées de pointeur, appelez la méthode [**Windows::UI::Core::CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208217) pour traiter les événements de fenêtre. Appelez cette méthode dans la boucle de votre jeu avant de mettre à jour ou de générer le rendu de la scène. Marble Maze transmet **CoreProcessEventsOption::ProcessAllIfPresent** à cette méthode pour traiter tous les événements en attente, puis renvoie immédiatement une valeur. Une fois tous les événements traités, Marble Maze génère un rendu de l’image suivante et la dévoile.

```cpp
// Process windowing events.
CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
```

Windows Runtime appelle le gestionnaire enregistré pour chaque événement qui se produit. La classe **DirectXApp** enregistre les événements et transmet les informations de pointeur à la classe **MarbleMaze**.

```cpp
void DirectXApp::OnPointerPressed(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void DirectXApp::OnPointerReleased(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->RemoveTouch(args->CurrentPoint->PointerId);
}

void DirectXApp::OnPointerMoved(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

La classe **MarbleMaze** réagit aux événements de pointeur en mettant à jour l’objet map qui contient les événements tactiles. La méthode **MarbleMaze::AddTouch** est appelée lors d’un premier appui sur le pointeur, par exemple quand l’utilisateur touche l’écran pour la première fois sur un appareil tactile. La méthode **MarbleMaze::AddTouch** est appelée lorsque le pointeur se déplace. La méthode **MarbleMaze::RemoveTouch** est appelée quand le pointeur est libéré, par exemple, quand l’utilisateur ne touche plus l’écran.

```cpp
void MarbleMaze::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_windowBounds);

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMaze::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_windowBounds);
}

void MarbleMaze::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

La fonction PointToTouch traduit la position actuelle du pointeur afin que l’origine se trouve au centre de l’écran, puis met à l’échelle les coordonnées dans une plage comprise entre -1,0 et +1,0. Cela permet de calculer l’inclinaison du labyrinthe, quelle que soit la méthode d’entrée.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Rect bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

La méthode **MarbleMaze::Update** met à jour les valeurs d’entrée combinées en incrémentant le facteur d’inclinaison d’une valeur de mise à l’échelle constante. Cette valeur de mise à l’échelle a été identifiée en testant différentes valeurs.

```cpp
// Account for touch input. 
const float touchScalingFactor = 2.0f;
for (TouchMap::const_iterator iter = m_touches.cbegin(); iter != m_touches.cend(); ++iter)
{
    combinedTiltX += iter->second.x * touchScalingFactor;
    combinedTiltY += iter->second.y * touchScalingFactor;
}
```

### <a name="processing-accelerometer-input"></a>Traitement des entrées d’accéléromètre

Pour traiter les entrées d’accéléromètre, la méthode **MarbleMaze::Update** appelle la méthode [**Windows::Devices::Sensors::Accelerometer::GetCurrentReading**](https://msdn.microsoft.com/library/windows/apps/br225699). Cette méthode renvoie un objet [**Windows::Devices::Sensors::AccelerometerReading**](https://msdn.microsoft.com/library/windows/apps/br225688), qui représente une lecture de l’accéléromètre. Les propriétés **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** et **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** contiennent respectivement l’accélération de la force g le long des axes x et y.

L’exemple suivant montre comment la méthode **MarbleMaze::Update** interroge l’accéléromètre et met à jour les valeurs d’entrée combinées. Quand vous inclinez l’appareil, la bille va plus vite en raison de la gravité.

```cpp
// Account for sensors. 
const float acceleromterScalingFactor = 3.5f;
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += static_cast<float>(reading->AccelerationX) * acceleromterScalingFactor;
        combinedTiltY += static_cast<float>(reading->AccelerationY) * acceleromterScalingFactor;
    }
}
```

Dans la mesure où vous ne pouvez pas savoir si un accéléromètre est présent sur l’ordinateur de l’utilisateur, vérifiez toujours qu’un objet Accelerometer valide existe avant d’interroger l’accéléromètre.

### <a name="processing-xbox-360-controller-input"></a>Traitement des entrées de la manette Xbox 360

L’exemple suivant montre comment la méthode **MarbleMaze::Update** lit les entrées de la manette Xbox 360 et met à jour les valeurs d’entrée combinées. La méthode **MarbleMaze::Update** utilise une boucle for pour permettre la réception des entrées provenant d’une manette connectée. La méthode **XInputGetState** remplit un objet XINPUT_STATE avec l’état actuel de la manette. Les valeurs **combinedTiltX** et **combinedTiltY** sont mises à jour en fonction des valeurs x et y du stick gauche.

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

XInput définit la constante **XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE** pour le stick gauche. Il s’agit du seuil de zone morte approprié pour la plupart des jeux.

> **Important**  Quand vous utilisez la manette Xbox 360, n’oubliez pas la zone morte. La zone morte fait référence aux différences de sensibilité au mouvement initial des boîtiers de commande. Pour certaines manettes, un mouvement léger peut ne pas générer de lecture, pour d’autres, une lecture mesurable peut être obtenue. Pour tenir compte de cela dans votre jeu, créez une zone de non-mouvement pour le mouvement initial du stick. Pour plus d’informations sur la zone morte, voir [Prise en main de XInput.](https://msdn.microsoft.com/library/windows/desktop/ee417001)

 

###  <a name="applying-input-to-the-game-state"></a>Application des entrées à l’état du jeu

Les périphériques indiquent les valeurs d’entrée de différentes manières. Par exemple, les entrées du pointeur peuvent être des coordonnées d’écran, tandis que les entrées de manette peuvent être dans un format complètement différent. L’un des problèmes liés à la combinaison des entrées de plusieurs périphériques en un ensemble de valeurs d’entrée est la normalisation, c’est-à-dire la conversion de valeurs dans un format commun. Marble Maze normalise les valeurs en les mettant à l’échelle dans la plage [-1,0, 1,0]. Pour normaliser les entrées de la manette Xbox 360, Marble Maze divise les valeurs d’entrée par 32768, car les valeurs d’entrée du stick sont toujours comprises entre -32768 et 32767. La fonction **PointToTouch**, décrite précédemment dans cette section, donne des résultats similaires, en convertissant les coordonnées écran en valeurs normalisées comprises dans la plage -1,0 et +1,0.

> **Conseil**  Même si votre application utilise une méthode d’entrée, nous recommandons toujours de normaliser les valeurs d’entrée. Cela permet de simplifier l’interprétation des entrées par d’autres composants de votre jeu, par exemple des simulations physiques, et de faciliter l’écriture de jeux qui fonctionnent sur différentes résolutions d’écran.

 

Une fois que la méthode **MarbleMaze::Update** a traité les entrées, elle crée un vecteur qui représente l’effet de l’inclinaison du labyrinthe sur la bille. L’exemple suivant montre comment Marble Maze utilise la fonction **XMVector3Normalize** pour créer un vecteur de gravité normalisé. La variable *MaxTilt* définit l’inclinaison du labyrinthe et empêche son retournement.

```cpp
const float maxTilt = 1.0f / 8.0f;
XMVECTOR gravity = XMVectorSet(combinedTiltX * maxTilt, combinedTiltY * maxTilt, 1.0f, 0.0f);
gravity = XMVector3Normalize(gravity);
```

Pour terminer la mise à jour des objets de la scène, Marble Maze transmet le vecteur de gravité mis à jour à la simulation physique, met à jour la simulation physique pour le temps qui s’est écoulé depuis l’image précédente, puis met à jour la position et l’orientation de la bille. Si la bille tombe du labyrinthe, la méthode **MarbleMaze::Update** la place au dernier point de contrôle touché par la bille et réinitialise l’état de la simulation physique.

```cpp
XMFLOAT3 g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);
```

```cpp
// Only update physics when gameplay is active.
m_physics.UpdatePhysicsSimulation(timeDelta);
```

```cpp
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

Cette section ne décrit pas le fonctionnement de la simulation physique. Pour plus d’informations, voir Physics.h et Physics.cpp dans les sources Marble Maze.

## <a name="next-steps"></a>Étapes suivantes


Consultez [Ajout d’audio à l’exemple Marble Maze](adding-audio-to-the-marble-maze-sample.md) pour obtenir des informations sur les pratiques clés en matière d’audio. Ce document décrit comment Marble Maze utilise Microsoft Media Foundation et XAudio2 pour charger, mixer et lire des ressources audio.

## <a name="related-topics"></a>Rubriques connexes


* [Ajout de son à l’exemple Marble Maze](adding-audio-to-the-marble-maze-sample.md)
* [Ajout de contenu visuel à l’exemple Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Développement de Marble Maze, jeu pour UWP en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




