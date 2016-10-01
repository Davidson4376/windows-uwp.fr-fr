---
author: mtoepke
title: "Ajouter des contrôles"
description: "Examinons maintenant la façon dont l’exemple de jeu implémente des contrôles de déplacement/vue dans un jeu 3D et développe des contrôles tactiles, de souris et de manette de jeu de base."
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 49214f3bc14b6a475a77c5dbb7c0f08bb0818df6

---

# Ajouter des contrôles


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Examinons maintenant la façon dont l’exemple de jeu implémente des contrôles de déplacement/vue dans un jeu 3D et développe des contrôles tactiles, de souris et de manette de jeu de base.

## Objectif


-   Implémenter des contrôles tactiles, de souris/clavier et de manette Xbox, dans un jeu de plateforme Windows universelle (UWP) avec DirectX.

## Contrôles et applications de jeu UWP


Un jeu UWP de qualité prend en charge des interfaces très diverses. Un joueur potentiel peut disposer de Windows 10 sur une tablette sans aucun bouton physique, d’un ordinateur multimédia équipé d’une manette Xbox 360 ou de la dernière plateforme de jeu pour PC avec une souris et un clavier de jeu très performants. Votre jeu doit prendre en charge tous ces périphériques si sa conception le permet.

Cet exemple prend en charge les trois. Il s’agit d’un simple jeu de tir subjectif, et les contrôles de déplacement/vue qui sont standard pour ce type de jeu sont aisément implémentés pour l’ensemble des trois formes d’entrées.

Pour plus d’informations sur les contrôles, et plus particulièrement sur les contrôles de déplacement/vue, voir [Contrôles de déplacement/vue pour les jeux](tutorial--adding-move-look-controls-to-your-directx-game.md) et [Contrôles tactiles pour les jeux](tutorial--adding-touch-controls-to-your-directx-game.md).

## Comportements de contrôles communs


Les contrôles tactiles et les contrôles de souris/clavier ont une implémentation de base très semblable. Dans une application UWP, un pointeur est simplement un point sur l’écran. Vous pouvez le déplacer en faisant glisser la souris ou votre doigt sur l’écran tactile. Par conséquent, vous pouvez opter pour un seul ensemble d’événements sans vous demander si le joueur utilise une souris ou un écran tactile pour déplacer le pointeur et appuyer dessus.

Lorsque la classe **MoveLookController** de l’exemple de jeu est initialisée, elle s’inscrit à quatre événements propres au pointeur et à un événement propre à la souris :

-   [**CoreWindow::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278). Le bouton gauche ou droit de la souris a été enfoncé (et maintenu), ou la surface tactile a été touchée.
-   [**CoreWindow::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276). La souris a été déplacée, ou une action de déplacement a été effectuée sur la surface tactile.
-   [**CoreWindow::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279). Le bouton gauche de la souris a été relâché, ou l’objet au contact de la surface tactile a été retiré.
-   [**CoreWindow::PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208275). Le pointeur est sorti de la fenêtre principale.
-   [**Windows::Devices::Input::MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356) ; La souris a parcouru une certaine distance. Sachez que nous nous intéressons uniquement aux valeurs relatives de déplacement de la souris, et non pas à la position x-y en cours.

```cpp
void MoveLookController::Initialize(
    _In_ CoreWindow^ window
    )
{
    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // A separate handler for mouse only relative mouse movement events.
    Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);

    ResetState();
    m_state = MoveLookControllerState::None;

    m_pitch = 0.0f;
    m_yaw   = 0.0f;
}
```

La manette Xbox est gérée séparément, en utilisant les API [XInput](https://msdn.microsoft.com/library/windows/desktop/hh405053). Nous allons parler de l’implémentation des contrôles de manette de jeu dans un moment.

Dans l’exemple de jeu, la classe **MoveLookController** présente trois états propres à la manette, quel que soit le type de contrôle :

-   **None**. Il s’agit de l’état initialisé pour la manette. Le jeu n’anticipe aucune entrée de manette.
-   **WaitForInput**. Le jeu est suspendu et attend que le joueur continue.
-   **Active**. Le jeu est en cours d’exécution et traite les entrées du joueur.

L’état **Active** est l’état affiché lorsque le joueur joue activement au jeu. Dans cet état, l’instance **MoveLookController** traite les événements d’entrée provenant de tous les périphériques d’entrée activés et interprète les intentions du joueur en fonction des données d’événement agrégées. Par conséquent, elle met à jour la vitesse et la direction (le vecteur normal au plan) de la vue du joueur et partage les données mises à jour avec le jeu une fois la mise à jour appelée à partir de la boucle de jeu.

Ayez à l’esprit que le joueur peut entreprendre plusieurs actions à la fois. Par exemple, il peut tirer des sphères tout en déplaçant la caméra. Toutes ces entrées sont suivies dans l’état **Active**, avec différents ID de pointeur correspondant à diverses actions de pointeur. Cette opération est nécessaire car, du point de vue du joueur, un événement de pointeur dans le rectangle de tir est différent de celui dans le rectangle de déplacement ou dans le reste de l’écran.

Lorsqu’un événement [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) est reçu, **MoveLookController** obtient la valeur d’ID de pointeur créée par la fenêtre. L’ID de pointeur représente un type spécifique d’entrée. Par exemple, sur un périphérique tactile multipoint, plusieurs entrées actives différentes peuvent exister en même temps. Les ID permettent d’assurer le suivi de l’entrée utilisée par le joueur. Si un événement se trouve dans le rectangle de déplacement de l’écran tactile, un ID de pointeur est affecté pour suivre tous les événements de pointeur dans le rectangle de déplacement. D’autres événements de pointeur dans le rectangle de tir sont suivis séparément, avec un ID de pointeur distinct. (Nous en parlerons plus en détail dans la section sur les contrôles tactiles.)

Les entrées provenant de la souris possèdent encore un autre ID et sont également gérées séparément.

Une fois que les événements de pointeur ont été mappés sur une action de jeu spécifique, il est temps de mettre à jour les données que l’objet **MoveLookController** partage avec la boucle de jeu principale.

Lorsqu’elle est appelée, la méthode **Update** de l’exemple de jeu traite les entrées et met à jour les variables de vitesse et de direction de la vue (**m\_velocity** et **m\_lookdirection**), que la boucle de jeu récupère ensuite en appelant les méthodes **Velocity** et **LookDirection** publiques sur l’instance **MoveLookController**.

```cpp
void MoveLookController::Update()
{
    UpdateGameController();

    if (m_moveInUse)
    {
        // Move control.
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        if (fabsf(pointerDelta.x) > 16.0f)         // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x -= pointerDelta.x/fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y/fabsf(pointerDelta.y);
    }

    // Poll our state bits set by the keyboard input events.
    if (m_forward)
    {
        m_moveCommand.y += 1.0f;
    }
    if (m_back)
    {
        m_moveCommand.y -= 1.0f;
    }
    if (m_left)
    {
        m_moveCommand.x += 1.0f;
    }
    if (m_right)
    {
        m_moveCommand.x -= 1.0f;
    }
    if (m_up)
    {
        m_moveCommand.z += 1.0f;
    }
    if (m_down)
    {
        m_moveCommand.z -= 1.0f;
    }

    // Make sure that 45deg cases are not faster.
    if (fabsf(m_moveCommand.x) > 0.1f ||
        fabsf(m_moveCommand.y) > 0.1f ||
        fabsf(m_moveCommand.z) > 0.1f)
    {
        XMStoreFloat3(&m_moveCommand, XMVector3Normalize(XMLoadFloat3(&m_moveCommand)));
    }

    // Rotate command to align with our direction (world coordinates).
    XMFLOAT3 wCommand;
    wCommand.x =  m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y =  m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z =  m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command, y is up.
    m_velocity.x = -wCommand.x * MOVEMENT_GAIN;
    m_velocity.z =  wCommand.y * MOVEMENT_GAIN;
    m_velocity.y =  wCommand.z * MOVEMENT_GAIN;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```

La boucle de jeu peut tester si le joueur tire en appelant la méthode **IsFiring** sur l’instance **MoveLookController**. L’instance **MoveLookController** vérifie si le joueur a appuyé sur le bouton de tir de l’un des trois types d’entrées.

```cpp
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || m_xinputTriggerInUse);
        }
        else
        {
            if (m_firePressed)
            {
                m_firePressed = false;
                return true;
            }
        }
    }
    return false;
}
```

Si le joueur déplace le pointeur hors de la fenêtre principale du jeu, ou appuie sur le bouton de pause (la touche P ou le bouton de démarrage de la manette Xbox), le jeu doit être suspendu. **MoveLookController** enregistre cette dernière action et informe la boucle de jeu lors de l’appel de la méthode **IsPauseRequested**. À ce stade, si **IsPauseRequested** renvoie la valeur **true**, la boucle de jeu appelle alors **WaitForPress** sur **MoveLookController** pour placer la manette dans l’état **WaitForInput**. Ensuite, **MoveLookController** attend que le joueur sélectionne l’un des éléments de menu à charger, continue ou quitte le jeu, puis arrête le traitement des événements d’entrée du jeu jusqu’à ce que son état redevienne **Active**.

Consultez l’[exemple de code complet pour cette section](#code_sample).

Examinons à présent l’implémentation de chacun des trois types de contrôles un peu plus en détail.

## Implémentation des contrôles de souris relatifs


Si le déplacement de la souris est détecté, nous souhaitons utiliser ce déplacement pour déterminer les nouveaux tangage et lacet de la caméra. Nous y parvenons en implémentant les contrôles de souris relatifs, avec lesquels nous gérons la distance relative parcourue par la souris (écart entre le début et la fin du déplacement) par opposition à l’enregistrement des coordonnées des pixels x-y absolues du mouvement.

Pour ce faire, nous obtenons les modifications dans X (déplacement horizontal) et les coordonnées Y (déplacement vertical) en examinant les champs [**MouseDelta::X**](https://msdn.microsoft.com/library/windows/apps/hh758353) et **MouseDelta::Y** sur l’objet argument [**Windows::Device::Input::MouseEventArgs::MouseDelta**](https://msdn.microsoft.com/library/windows/apps/hh758358) renvoyé par l’événement [**MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356).

```cpp
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice^ /* mouseDevice */,
    _In_ MouseEventArgs^ args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args->MouseDelta.X);
        mouseDelta.y = static_cast<float>(args->MouseDelta.Y);

        XMFLOAT2 rotationDelta;
        rotationDelta.x  = mouseDelta.x * ROTATION_GAIN;   // scale for control sensitivity
        rotationDelta.y  = mouseDelta.y * ROTATION_GAIN;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in same range by wrapping.
        if (m_yaw >  XM_PI)
        {
            m_yaw -= XM_PI * 2.0f;
        }
        else if (m_yaw < -XM_PI)
        {
            m_yaw += XM_PI * 2.0f;
        }
        break;
    }
}
```

## Implémentation des contrôles tactiles


Les contrôles tactiles sont les plus difficiles à développer, car ce sont les plus complexes et ils nécessitent le plus de réglages de précision pour être efficaces. Dans l’exemple de jeu, un rectangle dans le quadrant inférieur gauche de l’écran est utilisé comme bouton multidirectionnel, où le glissement de votre pouce vers la gauche et la droite dans cet espace fait glisser la caméra vers la gauche et la droite, et le glissement de votre pouce vers le haut et le bas fait glisser la caméra vers l’avant et l’arrière. Appuyer sur un rectangle dans le quadrant inférieur droit permet de tirer des sphères. La visée (tangage et lacet) est contrôlée en faisant glisser votre doigt sur les parties de l’écran qui ne sont pas réservées au déplacement et au tir ; lorsque votre doigt se déplace, la caméra (avec réticule fixe) se déplace de la même manière.

Les rectangles de déplacement et de tir sont créés par deux méthodes dans l’exemple de code :

```cpp
void SetMoveRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
 void SetFireRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
```

Nous considérons les événements de pointeur de périphérique tactile pour les autres zones de l’écran comme des commandes de vue. Si l’écran est redimensionné, ces rectangles doivent être recalculés (et redessinés).

Si un événement de pointeur d’appareil tactile est déclenché dans l’une de ces zones et que le jeu présente l’état **Active**, un ID de pointeur lui est affecté, comme mentionné plus tôt.

```cpp
void MoveLookController::OnPointerPressed(
    _In_ CoreWindow^ sender,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    UINT32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        // ...
        // Game is paused, wait for click inside the game window.
        // ...
        break;

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact.
                    m_movePointerID = pointerID;                // Store the pointer using this control.
                    m_moveInUse = true;
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;
                    m_firePointerID = pointerID;
                    m_fireInUse = true;
                }
            }
            else
            {
                if (!m_lookInUse)   // If no pointer is in this control yet:
                {
                    m_lookLastPoint = position;                   // Save the pointer for a later move.
                    m_lookPointerID = pointerID;                  // Store the pointer using this control.
                    m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
                    m_lookInUse = true;
                }
            }
            break;

        default:
            // ...
            // Handle mouse input here.
                                                // ...
            break;
        }
        break;
    }
    return;
}
```

Si un événement [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) s’est produit dans l’une des trois zones de contrôle, le rectangle de déplacement, le rectangle de tir ou le reste de l’écran (le contrôle de vue), **MoveLookController** affecte l’ID de pointeur correspondant au pointeur qui a déclenché l’événement à une variable spécifique qui correspond à la zone de l’écran dans laquelle l’événement a été déclenché. Par exemple, si l’événement s’est produit dans le rectangle de déplacement, l’ID de pointeur qui a déclenché l’événement est affecté à la variable **m\_movePointerID**. Une variable « en cours d’utilisation » booléenne (**m\_lookInUse**, dans l’exemple) est également définie pour indiquer que le contrôle n’a pas encore été libéré.

Examinons maintenant la façon dont l’exemple de jeu gère l’événement d’écran tactile [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276).

```cpp
void MoveLookController::OnPointerMoved(
    _In_ CoreWindow^ sender,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    UINT32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        if (pointerID == m_movePointerID)     // This is the move pointer.
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        else if (pointerID == m_lookPointerID)     // This is the look pointer.
        {
            // Look control
            XMFLOAT2 pointerDelta;
            pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move.
            pointerDelta.y = position.y - m_lookLastPoint.y;

            XMFLOAT2 rotationDelta;
            rotationDelta.x = pointerDelta.x * ROTATION_GAIN;       // Scale for control sensitivity.
            rotationDelta.y = pointerDelta.y * ROTATION_GAIN;
            m_lookLastPoint = position;                             // Save for the next time through.

            // Update our orientation based on the command.
            m_pitch -= rotationDelta.y;
            m_yaw   += rotationDelta.x;

            // Limit pitch to straight up or straight down.
            m_pitch = __max(-XM_PI / 2.0f, m_pitch);
            m_pitch = __min(+XM_PI / 2.0f, m_pitch);
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
        else if (pointerID == m_mousePointerID)
        {
            // ...
        }
        break;
    }
}
```

**MoveLookController** vérifie l’ID de pointeur pour déterminer où l’événement s’est produit, et entreprend l’une des actions suivantes :

-   Si l’événement [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) s’est produit dans le rectangle de déplacement ou de tir, la position du pointeur est mise à jour pour la manette.
-   Si l’événement [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) s’est produit ailleurs dans le reste de l’écran (défini comme les contrôles de vue), la modification des tangage et lacet du vecteur de direction de la vue est calculée.

Enfin, examinons la façon dont l’exemple de jeu gère l’événement d’écran tactile [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279).

```cpp
void MoveLookController::OnPointerReleased(
    _In_ CoreWindow^ sender,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    UINT32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            // ...
        }
        break;
    }
}
```

Si l’ID du pointeur qui a déclenché l’événement [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) est l’ID du pointeur de déplacement précédemment enregistré, **MoveLookController** affecte la valeur 0 à la vitesse, car le joueur a cessé de toucher le rectangle de déplacement. Si la valeur0 n’était pas affectée à la vitesse, le joueur continuerait de se déplacer! Si vous voulez implémenter une certaine forme d’inertie, ajoutez ici la méthode qui commence à redéfinir la vitesse sur 0 lors des futurs appels de la méthode **Update** à partir de la boucle de jeu.

Sinon, si l’événement [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) s’est déclenché dans le rectangle de tir ou la zone de vue, **MoveLookController** réinitialise les ID de pointeur spécifiques.

Ce sont les éléments de base de l’implémentation des contrôles d’écran tactile dans l’exemple de jeu. Passons aux contrôles de souris et de clavier.

## Implémentation des contrôles de souris et de clavier


L’exemple de jeu implémente les contrôles de souris et de clavier suivants :

-   Les touches W, S, A et D déplacent la vue du joueur vers l’avant, l’arrière, la gauche et la droite, respectivement. Le fait d’appuyer sur X et la barre d’espace déplace la vue vers le haut et le bas, respectivement.
-   Le jeu est suspendu si vous appuyez sur la touche P.
-   Le déplacement de la souris permet au joueur de contrôler la rotation (tangage et lacet) de la vue caméra.
-   Cliquer sur le bouton gauche permet de tirer une sphère.

Pour utiliser le clavier, l’exemple de jeu s’inscrit à deux événements supplémentaires : [**CoreWindow::KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271) et [**CoreWindow::KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270), qui gèrent les actions d’appuyer sur une touche et de la libérer, respectivement.

```cpp
window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);
```

La souris est traitée un peu différemment des contrôles tactiles, même si elle utilise un pointeur. Bien évidemment, elle n’utilise pas les rectangles de déplacement ni de tir, car le joueur serait très embarrassé: comment pourrait-il appuyer en même temps sur les contrôles de déplacement et de tir? Comme mentionné plus tôt, le contrôleur **MoveLookController** lance les contrôles de vue chaque fois que la souris est déplacée, et les contrôles de tir chaque fois que le bouton gauche de la souris est enfoncé, comme illustré ici.

```cpp
void MoveLookController::OnPointerPressed(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (position.x > m_buttonUpperLeft.x &&
            position.x < m_buttonLowerRight.x &&
            position.y > m_buttonUpperLeft.y &&
            position.y < m_buttonLowerRight.y)
        {
            // Wait until the button is released before setting the variable.
            m_buttonPointerID = pointerID;
            m_buttonInUse = true;

        }
        break;

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact.
                    m_movePointerID = pointerID;                // Store the pointer using this control.
                    m_moveInUse = true;
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;
                    m_firePointerID = pointerID;
                    m_fireInUse = true;
                    if (!m_autoFire)
                    {
                        m_firePressed = true;
                    }
                }
            }
            else
            {
                if (!m_lookInUse)   // If no pointer is in this control yet:
                {
                    m_lookLastPoint = position;                   // Save the pointer for a later move.
                    m_lookPointerID = pointerID;                  // Store the pointer using this control.
                    m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
                    m_lookInUse = true;
                }
            }
            break;

        default:
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            if (!m_autoFire && (!m_mouseLeftInUse && leftButton))
            {
                m_firePressed = true;
            }

            if (!m_mouseInUse)
            {
                m_mouseInUse = true;
                m_mouseLastPoint = position;
                m_mousePointerID = pointerID;
                m_mouseLeftInUse = leftButton;
                m_mouseRightInUse = rightButton;
                m_lookLastDelta.x = m_lookLastDelta.y = 0;          // These are for smoothing.
            }
            else
            {

            }
            break;
        }

        break;
    }

    return;
}
```

Examinons maintenant la façon dont l’exemple de jeu gère l’événement de souris [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279).

```cpp
void MoveLookController::OnPointerReleased(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            m_mouseInUse = false;

            // Don't clear the mouse pointer ID so that Move events still result in Look changes.
            // m_mousePointerID = 0;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
        }
        break;
    }
}
```

Lorsque le joueur arrête d’appuyer sur l’un des boutons de la souris, l’action est terminée, les tirs de sphères sont stoppés. Toutefois, dans la mesure où la vue est toujours activée, le jeu continue d’utiliser le même pointeur de souris pour le suivi des événements de vue en cours.

Examinons maintenant le dernier des types de contrôles : la manette Xbox. Elle est gérée séparément des contrôles tactiles et de souris, car elle n’utilise pas l’objet pointeur.

## Implémentation des contrôles de la manette Xbox


Dans l’exemple de jeu, la prise en charge de la manette Xbox est ajoutée par des appels aux API [XInput](https://msdn.microsoft.com/library/windows/desktop/hh405053), qui sont un ensemble d’API conçues pour simplifier la programmation des manettes de jeu. Dans l’exemple de jeu, nous utilisons le stick analogique gauche de la manette Xbox pour les déplacements du joueur, le stick analogique droit pour les contrôles de vue et la gâchette droite pour tirer. Nous utilisons le bouton de démarrage pour suspendre et reprendre le jeu.

La méthode **Update** sur l’instance **MoveLookController** vérifie immédiatement si une manette de jeu est connectée, puis vérifie son état.

```cpp
void MoveLookController::UpdateGameController()
{
    if (!m_isControllerConnected)
    {
        // Check for controller connection by trying to get the capabilties.
        DWORD capsResult = XInputGetCapabilities(0, XINPUT_FLAG_GAMEPAD, &m_xinputCaps);
        if (capsResult != ERROR_SUCCESS)
        {
            return;
        }
        // Device is connected.
        m_isControllerConnected = true;
        m_xinputStartButtonInUse = false;
        m_xinputTriggerInUse = false;
    }

    DWORD stateResult = XInputGetState(0, &m_xinputState);
    if (stateResult != ERROR_SUCCESS)
    {
        // Device is no longer connected.
        m_isControllerConnected = false;
    }

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_pausePressed = true;
        }
        // Use the Right Thumb joystick on the XBox controller to control
        // the eye point position control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.

        if (m_xinputState.Gamepad.sThumbLX > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLX < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float x = (float)m_xinputState.Gamepad.sThumbLX/32767.0f;
            m_moveCommand.x -= x / fabsf(x);
        }

        if (m_xinputState.Gamepad.sThumbLY > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLY < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float y = (float)m_xinputState.Gamepad.sThumbLY/32767.0f;
            m_moveCommand.y += y / fabsf(y);
        }

        // Use the Left Thumb Joystick on the XBox controller to control
        // the look at control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.
        XMFLOAT2 pointerDelta;
        if (m_xinputState.Gamepad.sThumbRX > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRX < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.x = (float)m_xinputState.Gamepad.sThumbRX/32767.0f;
            pointerDelta.x = pointerDelta.x * pointerDelta.x * pointerDelta.x;
        }
        else
        {
            pointerDelta.x = 0.0f;
        }
        if (m_xinputState.Gamepad.sThumbRY > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRY < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.y = (float)m_xinputState.Gamepad.sThumbRY/32767.0f;
            pointerDelta.y = pointerDelta.y * pointerDelta.y * pointerDelta.y;
        }
        else
        {
            pointerDelta.y = 0.0f;
        }

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x *  0.08f;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y *  0.08f;

        // Update our orientation based on the command.
        m_pitch += rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);

        // Check the state of the A button.  This is used to indicate fire control.

        if (m_xinputState.Gamepad.bRightTrigger > XINPUT_GAMEPAD_TRIGGER_THRESHOLD)
        {
            if (!m_autoFire && !m_xinputTriggerInUse)
            {
                m_firePressed = true;
            }
            m_xinputTriggerInUse = true;
        }
        else
        {
            m_xinputTriggerInUse = false;
        }
        break;
    }
}
```

Si la manette de jeu présente l’état **Active**, cette méthode vérifie si un utilisateur a bougé le stick analogique gauche dans une direction spécifique. Toutefois, le mouvement effectué dans une direction spécifique sur le stick doit dépasser le rayon de la zone morte; sinon, rien ne se produit. Cette zone morte est nécessaire pour gérer les « effleurements », c’est-à-dire lorsque la manette détecte des mini-mouvements effectués par le pouce du joueur lorsqu’il repose sur le stick. En l’absence de cette zone morte, le joueur peut s’énerver très rapidement, car les contrôles ne sont pas du tout stables.

La méthode **Update** effectue alors la même vérification sur le stick droit pour voir si le joueur a modifié la direction de la vue de la caméra, à condition que le mouvement sur le stick dépasse le rayon d’une autre zone morte.

**Update** calcule les nouveaux tangage et lacet, puis vérifie si l’utilisateur a appuyé sur la gâchette analogique droite, notre bouton de tir.

Et voici comment cet exemple implémente tout un ensemble d’options de contrôle. Encore une fois, gardez à l’esprit qu’une application du Windows Store de qualité prend en charge un large éventail d’options de contrôle. Ainsi, les utilisateurs avec différents périphériques et facteurs de forme peuvent jouer de la façon qu’ils préfèrent !

## Étapes suivantes


Nous avons passé en revue chaque composant majeur d’un jeu WUP DirectX, sauf un : le composant audio ! La musique et les effets sonores étant un élément essentiel dans tout jeu, abordons l’[ajout de son](tutorial--adding-sound.md) !

## Exemple de code complet pour cette section


MoveLookController.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Uncomment to print debug tracing.
// #define MOVELOOKCONTROLLER_TRACE 1

enum class MoveLookControllerState
{
    None,
    WaitForInput,
    Active,
};

ref class MoveLookController
{
internal:
    MoveLookController();

    void Initialize(
        _In_ Windows::UI::Core::CoreWindow^ window
        );
    void SetMoveRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
    void SetFireRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
    void WaitForPress(
        _In_ DirectX::XMFLOAT2 UpperLeft,
        _In_ DirectX::XMFLOAT2 LowerRight
        );
    void WaitForPress();

    void Update();
    bool IsFiring();
    bool IsPressComplete();
    bool IsPauseRequested();

    DirectX::XMFLOAT3 Velocity();
    DirectX::XMFLOAT3 LookDirection();
    float Pitch();
    void  Pitch(_In_ float pitch);
    float Yaw();
    void  Yaw(_In_ float yaw);
    bool  Active();
    void  Active(_In_ bool active);

    bool AutoFire();
    void AutoFire(_In_ bool AutoFire);

protected:
    void OnPointerPressed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnPointerMoved(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnPointerReleased(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnPointerExited(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnKeyDown(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );
    void OnKeyUp(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    void OnMouseMoved(
        _In_ Windows::Devices::Input::MouseDevice^ mouseDevice,
        _In_ Windows::Devices::Input::MouseEventArgs^ args
        );

#ifdef MOVELOOKCONTROLLER_TRACE
    void DebugTrace(const wchar_t *format, ...);
#endif

private:
    // Properties of the controller object
    MoveLookControllerState     m_state;
    DirectX::XMFLOAT3           m_velocity;             // How far we move it this frame
    float                       m_pitch;
    float                       m_yaw;                  // Orientation euler angles in radians

    // Properties of the Move control
    DirectX::XMFLOAT2           m_moveUpperLeft;        // Bounding box where this control will activate
    DirectX::XMFLOAT2           m_moveLowerRight;
    bool                        m_moveInUse;            // The move control is in use.
    uint32                      m_movePointerID;        // The id of the pointer in this control.
    DirectX::XMFLOAT2           m_moveFirstDown;        // The point where the initial contact occurred.
    DirectX::XMFLOAT2           m_movePointerPosition;  // The point where the move pointer is currently located.
    DirectX::XMFLOAT3           m_moveCommand;          // The net command from the move control.

    // Properties of the Look control
    bool                        m_lookInUse;            // The look control is in use.
    uint32                      m_lookPointerID;        // The id of the pointer in this control.
    DirectX::XMFLOAT2           m_lookLastPoint;        // The last point (from last frame)
    DirectX::XMFLOAT2           m_lookLastDelta;        // for smoothing.

    // Properties of the Fire control
    bool                        m_autoFire;
    bool                        m_firePressed;
    DirectX::XMFLOAT2           m_fireUpperLeft;        // The bounding box where this control will activate.
    DirectX::XMFLOAT2           m_fireLowerRight;
    bool                        m_fireInUse;            // The fire control is in use.
    UINT32                      m_firePointerID;        // The id of the pointer in this control.
    DirectX::XMFLOAT2           m_fireLastPoint;        // The last fire position.

    // Properties of the Mouse control. This is a combination of Look and Fire.
    bool                        m_mouseInUse;
    uint32                      m_mousePointerID;
    DirectX::XMFLOAT2           m_mouseLastPoint;
    bool                        m_mouseLeftInUse;
    bool                        m_mouseRightInUse;

    bool                        m_buttonInUse;
    uint32                      m_buttonPointerID;
    DirectX::XMFLOAT2           m_buttonUpperLeft;
    DirectX::XMFLOAT2           m_buttonLowerRight;
    bool                        m_buttonPressed;
    bool                        m_pausePressed;

    // XBox Input related members
    bool                        m_isControllerConnected;  // Do we have a controller connected?
    XINPUT_CAPABILITIES         m_xinputCaps;             // The capabilites of the controller.
    XINPUT_STATE                m_xinputState;            // The current state of the controller.
    bool                        m_xinputStartButtonInUse;
    bool                        m_xinputTriggerInUse;

    // Input states for Keyboard
    bool                        m_forward;
    bool                        m_back;                    // States for movement
    bool                        m_left;
    bool                        m_right;
    bool                        m_up;
    bool                        m_down;
    bool                        m_pause;

private:
    void     ResetState();
    void     UpdateGameController();
};
```

MoveLookController.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "MoveLookController.h"
#include "DirectXSample.h"

using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI;
using namespace Windows::Foundation;
using namespace Microsoft::WRL;
using namespace DirectX;
using namespace Windows::Devices::Input;
using namespace Windows::System;

#define ROTATION_GAIN 0.008f        // The sensitivity adjustment for the look controller.
#define MOVEMENT_GAIN 2.f           // The sensitivity adjustment for the move controller.

// A basic Move/Look Controller class such as in an FPS
// horizontal (x-z-plane) movement on left virtual joystick
// also supports WASD keyboard input
// steering and orientation via left mouse down or touch drag.

//----------------------------------------------------------------------

MoveLookController::MoveLookController():
    m_autoFire(true),
    m_isControllerConnected(false)
{
}

//----------------------------------------------------------------------
// Set up the controls supported by this controller.

void MoveLookController::Initialize(
    _In_ CoreWindow^ window
    )
{
    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // A separate handler for mouse only relative mouse movement events.
    Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);

    ResetState();
    m_state = MoveLookControllerState::None;

    m_pitch = 0.0f;
    m_yaw   = 0.0f;
}

//----------------------------------------------------------------------

bool MoveLookController::IsPauseRequested()
{
    switch (m_state)
    {
    case MoveLookControllerState::Active:
        UpdateGameController();
        if (m_pausePressed)
        {

            m_pausePressed = false;
            return true;
        }
        else
        {
            return false;
        }
    }
    return false;
}

//----------------------------------------------------------------------

bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || m_xinputTriggerInUse);
        }
        else
        {
            if (m_firePressed)
            {
                m_firePressed = false;
                return true;
            }
        }
    }
    return false;
}

//----------------------------------------------------------------------

bool MoveLookController::IsPressComplete()
{
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        UpdateGameController();
        if (m_buttonPressed)
        {

            m_buttonPressed = false;
            return true;
        }
        else
        {
            return false;
        }
        break;
    }

    return false;
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerPressed(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (position.x > m_buttonUpperLeft.x &&
            position.x < m_buttonLowerRight.x &&
            position.y > m_buttonUpperLeft.y &&
            position.y < m_buttonLowerRight.y)
        {
            // Wait until button released before setting variable.
            m_buttonPointerID = pointerID;
            m_buttonInUse = true;

        }
        break;

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact.
                    m_movePointerID = pointerID;                // Store the pointer using this control.
                    m_moveInUse = true;
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;
                    m_firePointerID = pointerID;
                    m_fireInUse = true;
                    if (!m_autoFire)
                    {
                        m_firePressed = true;
                    }
                }
            }
            else
            {
                if (!m_lookInUse)   // If no pointer is in this control yet:
                {
                    m_lookLastPoint = position;                   // Save the point for a later move.
                    m_lookPointerID = pointerID;                  // Store the pointer using this control.
                    m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
                    m_lookInUse = true;
                }
            }
            break;

        default:
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            if (!m_autoFire && (!m_mouseLeftInUse && leftButton))
            {
                m_firePressed = true;
            }

            if (!m_mouseInUse)
            {
                m_mouseInUse = true;
                m_mouseLastPoint = position;
                m_mousePointerID = pointerID;
                m_mouseLeftInUse = leftButton;
                m_mouseRightInUse = rightButton;
                m_lookLastDelta.x = m_lookLastDelta.y = 0;          // These are for smoothing.
            }
            else
            {

            }
            break;
        }

        break;
    }

    return;
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerMoved(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        if (pointerID == m_movePointerID)     // This is the move pointer.
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        else if (pointerID == m_lookPointerID)     // This is the look pointer.
        {
            // Look control
            XMFLOAT2 pointerDelta;
            pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move?
            pointerDelta.y = position.y - m_lookLastPoint.y;

            XMFLOAT2 rotationDelta;
            rotationDelta.x = pointerDelta.x * ROTATION_GAIN;       // Scale for control sensitivity.
            rotationDelta.y = pointerDelta.y * ROTATION_GAIN;
            m_lookLastPoint = position;                             // Save for the next time through.

            // Update our orientation based on the command.
            m_pitch -= rotationDelta.y;
            m_yaw   += rotationDelta.x;

            // Limit pitch to straight up or straight down.
            float limit = XM_PI / 2.0f - 0.01f;
            m_pitch = __max(-limit, m_pitch);
            m_pitch = __min(+limit, m_pitch);

            // Keep longitude in the same range by wrapping.
            if (m_yaw >  XM_PI)
            {
                m_yaw -= XM_PI * 2.0f;
            }
            else if (m_yaw < -XM_PI)
            {
                m_yaw += XM_PI * 2.0f;
            }
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
        else if (pointerID == m_mousePointerID)
        {
            m_mouseLeftInUse  = pointProperties->IsLeftButtonPressed;
            m_mouseRightInUse = pointProperties->IsRightButtonPressed;;
            m_mouseLastPoint = position;                            // save for next time through

            // Handle mouse movement via a separate relative mouse movement handler (OnMouseMoved).
        }
        break;
    }
}

//----------------------------------------------------------------------

void MoveLookController::OnMouseMoved(
    _In_ MouseDevice^ /* mouseDevice */,
    _In_ MouseEventArgs^ args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args->MouseDelta.X);
        mouseDelta.y = static_cast<float>(args->MouseDelta.Y);

        XMFLOAT2 rotationDelta;
        rotationDelta.x  = mouseDelta.x * ROTATION_GAIN;   // Scale for control sensitivity.
        rotationDelta.y  = mouseDelta.y * ROTATION_GAIN;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in same range by wrapping.
        if (m_yaw >  XM_PI)
        {
            m_yaw -= XM_PI * 2.0f;
        }
        else if (m_yaw < -XM_PI)
        {
            m_yaw += XM_PI * 2.0f;
        }
        break;
    }
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerReleased(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            m_mouseInUse = false;

            // Don't clear the mouse pointer ID so that Move events still result in Look changes.
            // m_mousePointerID = 0;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
        }
        break;
    }
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerExited(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position  = XMFLOAT2(pointerPosition.X, pointerPosition.Y);        // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = false;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            m_mouseInUse = false;
            m_mousePointerID = 0;
            m_mouseLeftInUse = false;
            m_mouseRightInUse = false;
        }
        break;
    }
}

//----------------------------------------------------------------------

void MoveLookController::OnKeyDown(
    _In_ CoreWindow^ /* sender */,
    _In_ KeyEventArgs^ args
    )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if (Key == VirtualKey::W)       // forward
        m_forward = true;
    if (Key == VirtualKey::S)       // back
        m_back = true;
    if (Key == VirtualKey::A)       // left
        m_left = true;
    if (Key == VirtualKey::D)       // right
        m_right = true;
    if (Key == VirtualKey::Space)   // up
        m_up = true;
    if (Key == VirtualKey::X)       // down
        m_down = true;
    if (Key == VirtualKey::P)       // Pause
        m_pause = true;
}

//----------------------------------------------------------------------

void MoveLookController::OnKeyUp(
    _In_ CoreWindow^ /* sender */,
    _In_ KeyEventArgs^ args
    )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if (Key == VirtualKey::W)       // Forward
        m_forward = false;
    if (Key == VirtualKey::S)       // Back
        m_back = false;
    if (Key == VirtualKey::A)       // Left
        m_left = false;
    if (Key == VirtualKey::D)       // Right
        m_right = false;
    if (Key == VirtualKey::Space)   // Up
        m_up = false;
    if (Key == VirtualKey::X)       // Down
        m_down = false;
    if (Key == VirtualKey::P)
    {
        if (m_pause)
        {
            // Trigger pause only one time on button release.
            m_pausePressed = true;
            m_pause = false;
        }
    }
}

//----------------------------------------------------------------------

void MoveLookController::ResetState()
{
    // Reset the state of the controller.
    // Disable any active pointer IDs to stop all interaction.
    m_buttonPressed = false;
    m_pausePressed = false;
    m_buttonInUse = false;
    m_moveInUse = false;
    m_lookInUse = false;
    m_fireInUse = false;
    m_mouseInUse = false;
    m_mouseLeftInUse = false;
    m_mouseRightInUse = false;
    m_movePointerID = 0;
    m_lookPointerID = 0;
    m_firePointerID = 0;
    m_mousePointerID = 0;
    m_velocity = XMFLOAT3(0.0f, 0.0f, 0.0f);

    m_xinputStartButtonInUse = false;
    m_xinputTriggerInUse = false;

    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
    m_forward = false;
    m_back = false;
    m_left = false;
    m_right = false;
    m_up = false;
    m_down = false;
    m_pause = false;
}

//----------------------------------------------------------------------

void MoveLookController::SetMoveRect (
    _In_ XMFLOAT2 upperLeft,
    _In_ XMFLOAT2 lowerRight
    )
{
    m_moveUpperLeft  = upperLeft;
    m_moveLowerRight = lowerRight;
}

//----------------------------------------------------------------------

void MoveLookController::SetFireRect (
    _In_ XMFLOAT2 upperLeft,
    _In_ XMFLOAT2 lowerRight
    )
{
    m_fireUpperLeft  = upperLeft;
    m_fireLowerRight = lowerRight;
}

//----------------------------------------------------------------------

void MoveLookController::WaitForPress(
    _In_ XMFLOAT2 upperLeft,
    _In_ XMFLOAT2 lowerRight
    )
{

    ResetState();
    m_state = MoveLookControllerState::WaitForInput;
    m_buttonUpperLeft  = upperLeft;
    m_buttonLowerRight = lowerRight;

    // Turn on the mouse cursor.
    CoreWindow::GetForCurrentThread()->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);
}

//----------------------------------------------------------------------

void MoveLookController::WaitForPress()
{
    ResetState();
    m_state = MoveLookControllerState::WaitForInput;
    m_buttonUpperLeft.x = 0.0f;
    m_buttonUpperLeft.y = 0.0f;
    m_buttonLowerRight.x = 0.0f;
    m_buttonLowerRight.y = 0.0f;

    // Turn on the mouse cursor.
    CoreWindow::GetForCurrentThread()->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);
}

//----------------------------------------------------------------------

XMFLOAT3 MoveLookController::Velocity()
{
    return m_velocity;
}

//----------------------------------------------------------------------

XMFLOAT3 MoveLookController::LookDirection()
{
    XMFLOAT3 lookDirection;

    float r = cosf(m_pitch);              // In the plane
    lookDirection.y = sinf(m_pitch);      // Vertical
    lookDirection.z = r * cosf(m_yaw);    // Fwd-back
    lookDirection.x = r * sinf(m_yaw);    // Left-right

    return lookDirection;
}

//----------------------------------------------------------------------

float MoveLookController::Pitch()
{
    return m_pitch;
}

//----------------------------------------------------------------------

void MoveLookController::Pitch(_In_ float pitch)
{
    m_pitch = pitch;
}

//----------------------------------------------------------------------

float MoveLookController::Yaw()
{
    return m_yaw;
}

//----------------------------------------------------------------------

void MoveLookController::Yaw(_In_ float yaw)
{
    m_yaw = yaw;
}

//----------------------------------------------------------------------

void MoveLookController::Active(_In_ bool active)
{
    ResetState();

    if (active)
    {
        m_state = MoveLookControllerState::Active;
        // Turn the mouse cursor off (hidden).
        CoreWindow::GetForCurrentThread()->PointerCursor = nullptr;
    }
    else
    {
        m_state = MoveLookControllerState::None;
        // Turn the mouse cursor on.
        auto window = CoreWindow::GetForCurrentThread();
        if (window)
        {
            // Protect case where there isn't a window associated with the current thread.
            // This happens on initialization.
            window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);
        }
    }
}

//----------------------------------------------------------------------

bool MoveLookController::Active()
{
    if (m_state == MoveLookControllerState::Active)
    {
        return true;
    }
    else
    {
        return false;
    }
}

//----------------------------------------------------------------------

void MoveLookController::AutoFire(_In_ bool autoFire)
{
    m_autoFire = autoFire;
}

//----------------------------------------------------------------------

bool MoveLookController::AutoFire()
{
    return m_autoFire;
}

//----------------------------------------------------------------------

void MoveLookController::Update()
{
    UpdateGameController();

    if (m_moveInUse)
    {
        // Move control.
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        if (fabsf(pointerDelta.x) > 16.0f)         // leave 32 pixel-wide dead spot for being still
            m_moveCommand.x -= pointerDelta.x/fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y/fabsf(pointerDelta.y);
    }

    // Poll our state bits set by the keyboard input events.
    if (m_forward)
    {
        m_moveCommand.y += 1.0f;
    }
    if (m_back)
    {
        m_moveCommand.y -= 1.0f;
    }
    if (m_left)
    {
        m_moveCommand.x += 1.0f;
    }
    if (m_right)
    {
        m_moveCommand.x -= 1.0f;
    }
    if (m_up)
    {
        m_moveCommand.z += 1.0f;
    }
    if (m_down)
    {
        m_moveCommand.z -= 1.0f;
    }

    // Make sure that 45 deg cases are not faster.
    if (fabsf(m_moveCommand.x) > 0.1f ||
        fabsf(m_moveCommand.y) > 0.1f ||
        fabsf(m_moveCommand.z) > 0.1f)
    {
        XMStoreFloat3(&m_moveCommand, XMVector3Normalize(XMLoadFloat3(&m_moveCommand)));
    }

    // Rotate command to align with our direction (world coordinates).
    XMFLOAT3 wCommand;
    wCommand.x =  m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y =  m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z =  m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command, y is up.
    m_velocity.x = -wCommand.x * MOVEMENT_GAIN;
    m_velocity.z =  wCommand.y * MOVEMENT_GAIN;
    m_velocity.y =  wCommand.z * MOVEMENT_GAIN;

    // Clear the movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}

//----------------------------------------------------------------------

void MoveLookController::UpdateGameController()
{
    if (!m_isControllerConnected)
    {
        // Check for controller connection by trying to get the capabilties.
        DWORD capsResult = XInputGetCapabilities(0, XINPUT_FLAG_GAMEPAD, &m_xinputCaps);
        if (capsResult != ERROR_SUCCESS)
        {
            return;
        }
        // The device is connected.
        m_isControllerConnected = true;
        m_xinputStartButtonInUse = false;
        m_xinputTriggerInUse = false;
    }

    DWORD stateResult = XInputGetState(0, &m_xinputState);
    if (stateResult != ERROR_SUCCESS)
    {
        // The device is no longer connected.
        m_isControllerConnected = false;
    }

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_pausePressed = true;
        }
        // Use the Right Thumb joystick on the XBox controller to control
        // the eye point position control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.

        if (m_xinputState.Gamepad.sThumbLX > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLX < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float x = (float)m_xinputState.Gamepad.sThumbLX/32767.0f;
            m_moveCommand.x -= x / fabsf(x);
        }

        if (m_xinputState.Gamepad.sThumbLY > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLY < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float y = (float)m_xinputState.Gamepad.sThumbLY/32767.0f;
            m_moveCommand.y += y / fabsf(y);
        }

        // Use the Left Thumb Joystick on the XBox controller to control
        // the look at control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.
        XMFLOAT2 pointerDelta;
        if (m_xinputState.Gamepad.sThumbRX > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRX < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.x = (float)m_xinputState.Gamepad.sThumbRX/32767.0f;
            pointerDelta.x = pointerDelta.x * pointerDelta.x * pointerDelta.x;
        }
        else
        {
            pointerDelta.x = 0.0f;
        }
        if (m_xinputState.Gamepad.sThumbRY > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRY < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.y = (float)m_xinputState.Gamepad.sThumbRY/32767.0f;
            pointerDelta.y = pointerDelta.y * pointerDelta.y * pointerDelta.y;
        }
        else
        {
            pointerDelta.y = 0.0f;
        }

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x *  0.08f;       // scale for control sensitivity
        rotationDelta.y = pointerDelta.y *  0.08f;

        // Update our orientation based on the command.
        m_pitch += rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);

        // Check the state of the A button.  This is used to indicate fire control.

        if (m_xinputState.Gamepad.bRightTrigger > XINPUT_GAMEPAD_TRIGGER_THRESHOLD)
        {
            if (!m_autoFire && !m_xinputTriggerInUse)
            {
                m_firePressed = true;
            }
            m_xinputTriggerInUse = true;
        }
        else
        {
            m_xinputTriggerInUse = false;
        }
        break;
    }
}
```

> **Remarque**  
Cet article s’adresse aux développeurs de Windows10 qui développent des applications de la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Rubriques connexes


[Créer un jeu UWP simple avec DirectX](tutorial--create-your-first-metro-style-directx-game.md)

 

 







<!--HONumber=Aug16_HO3-->


