---
title: Ajouter des contrôles
description: Examinons maintenant la façon dont l’exemple de jeu implémente des contrôles de déplacement/vue dans un jeu 3D et développe des contrôles tactiles, de souris et de manette de jeu de base.
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.date: 10/24/2017
ms.topic: article
keywords: windows10, uwp, jeux, contrôles, entrée
ms.localizationpriority: medium
ms.openlocfilehash: 01c0d1361dcc0858a54792adc0d17408ed281c99
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8900869"
---
# <a name="add-controls"></a>Ajouter des contrôles


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Un jeu pour la plateforme Windows universelle (UWP) de qualité prend en charge des interfaces très diverses. Un joueur potentiel peut disposer de Windows 10 sur une tablette sans aucun bouton physique, un PC équipé d’une manette Xbox, ou le dernier jeu rayon avec une souris et un clavier. Dans notre jeu, les contrôles sont implémentés dans la classe [**MoveLookController**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp). Cette classe regroupe les trois types d’entrée (souris et clavier, tactile et boîtier de commande) dans un seul contrôleur. Il en résulte un jeu de tir en vue subjective qui utilise les contrôles de déplacement/vue standard du genre qui fonctionnent sur plusieurs appareils.

> [!NOTE]
> Pour plus d’informations sur les contrôles, consultez [Contrôles de déplacement/vue pour les jeux](tutorial--adding-move-look-controls-to-your-directx-game.md) et [Contrôles tactiles pour les jeux](tutorial--adding-touch-controls-to-your-directx-game.md).


## <a name="objective"></a>Objectif

À ce stade, nous avons un jeu qui s’affiche, mais nous ne pouvons pas déplacer notre joueur ni tirer sur les cibles. Nous allons examiner comment implémenter les contrôles de déplacement/vue d'un jeu de tir en vue subjective pour les types d'entrée suivants de notre jeu UWP DirectX.
- Souris et clavier
- Commandes tactiles
- Boîtier de commande

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="common-control-behaviors"></a>Comportements des contrôles communs


Les contrôles tactiles et les contrôles de souris/clavier ont une implémentation de base très semblable. Dans une application UWP, un pointeur est simplement un point sur l’écran. Vous pouvez le déplacer en faisant glisser la souris ou votre doigt sur l’écran tactile. Par conséquent, vous pouvez opter pour un seul ensemble d’événements sans vous demander si le joueur utilise une souris ou un écran tactile pour déplacer le pointeur et appuyer dessus.

Lorsque la classe **MoveLookController** de l’exemple de jeu est initialisée, elle s’inscrit à quatre événements propres au pointeur et à un événement propre à la souris:

Événement | Description
:------ | :-------
[**CoreWindow::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) | Le bouton gauche ou droit de la souris a été enfoncé (et maintenu), ou la surface tactile a été touchée.
[**CoreWindow::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) |La souris a été déplacée, ou une action de déplacement a été effectuée sur la surface tactile.
[**CoreWindow::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) |Le bouton gauche de la souris a été relâché, ou l’objet au contact de la surface tactile a été retiré.
[**CoreWindow::PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208275) |Le pointeur est sorti de la fenêtre principale.
[**Windows::Devices::Input::MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356) | La souris a parcouru une certaine distance. Sachez que nous nous intéressons uniquement aux valeurs relatives de déplacement de la souris, et non pas à la position X-Y en cours.


Ces gestionnaires d’événements sont définis pour démarrer l’écoute de l’entrée utilisateur dès que l'objet **MoveLookController** est initialisé dans la fenêtre d’application.
```cpp
void MoveLookController::InitWindow(_In_ CoreWindow^ window)
{
    ResetState();

    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    // There is a separate handler for mouse only relative mouse movement events.
    MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);
    //
    // ...
    //
}
```

Le code complet pour [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92) est disponible sur GitHub.


Pour déterminer à quel moment le jeu doit écouter une entrée donnée, la classe **MoveLookController** présente trois états propres au contrôleur, quel que soit le type de celui-ci:

État | Description
:----- | :-------
**Aucun** | Il s’agit de l’état initialisé pour le contrôleur. Toute entrée est ignorée puisque le jeu n’anticipe aucune entrée de contrôleur.
**WaitForInput** | Le contrôleur attend que le joueur accuse réception d'un message provenant du jeu, soit à l’aide du bouton gauche de la souris, d'un événement tactile ou du bouton Menu d'un boîtier de commande.
**Active** | Le contrôleur est en mode de jeu actif.



### <a name="waitforinput-state-and-pausing-the-game"></a>État WaitForInput et mise en pause du jeu

Le jeu passe dans l'état **WaitForInput** lorsqu'il a été mis en pause. Cela se produit lorsque le joueur déplace le pointeur hors de la fenêtre principale du jeu, ou appuie sur le bouton de pause (la touche P ou le bouton **Démarrer** du boîtier de commande). La classe **MoveLookController** enregistre cette dernière action et informe la boucle de jeu lors de l’appel de la méthode [**IsPauseRequested**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127). À ce stade, si **IsPauseRequested** renvoie la valeur **true**, la boucle de jeu appelle alors **WaitForPress** sur **MoveLookController** pour placer le contrôleur dans l’état **WaitForInput**. 


Une fois dans l'état **WaitForInput**, le jeu interrompt le traitement de pratiquement tous les événements d’entrée jusqu'à ce qu’il soit revenu à l'état **Active**. La seule exception est le bouton Pause. L'utilisation de ce dernier fait revenir le jeu à l'état actif. Alternativement au bouton pause, afin que le jeu de revenir à l’état **actif** le joueur doit sélectionner un élément de menu. 



### <a name="the-active-state"></a>L’état Active

Dans l'état **Active**, l'instance **MoveLookController** traite les événements d’entrée provenant de tous les périphériques d’entrée activés et interprète les intentions du joueur en fonction des données d’événement agrégées. Par conséquent, elle met à jour la vitesse et la direction de la vue du joueur et partage les données mises à jour avec le jeu une fois la méthode **Update** appelée à partir de la boucle de jeu.


Toutes les entrées du pointeur sont suivies dans l’état **Active**, avec différents ID de pointeur correspondant à diverses actions de pointeur.
Lorsqu’un événement [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) est reçu, **MoveLookController** obtient la valeur d’ID de pointeur créée par la fenêtre. L’ID de pointeur représente un type spécifique d’entrée. Par exemple, sur un périphérique tactile multipoint, plusieurs entrées actives différentes peuvent exister en même temps. Les ID permettent d’assurer le suivi de l’entrée utilisée par le joueur. Si un événement se trouve dans le rectangle de déplacement de l’écran tactile, un ID de pointeur est affecté pour suivre tous les événements de pointeur dans le rectangle de déplacement. Les autres événements de pointeur dans le rectangle de tir sont suivis séparément, avec un ID de pointeur distinct.


> [!NOTE]
> Les ID d'entrée de la souris et du mouvement vers la droite du joystick d'un boîtier de commande sont également gérés séparément.

Une fois que les événements de pointeur ont été mappés sur une action de jeu spécifique, il est temps de mettre à jour les données que l’objet **MoveLookController** partage avec la boucle de jeu principale.

Lorsqu’elle est appelée, la méthode [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) de l’exemple de jeu traite les entrées et met à jour les variables de vitesse et de direction de la vue (**m\_velocity** et **m\_lookdirection**), que la boucle de jeu récupère ensuite en appelant les méthodes publiques [**Velocity**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909) et [**LookDirection**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923).

> [!NOTE]
> Vous trouverez davantage de détails sur la méthode [**Update**](#the-update-method) plus loin sur cette page.




La boucle de jeu peut tester si le joueur tire en appelant la méthode **IsFiring** sur l’instance **MoveLookController**. L’instance **MoveLookController** vérifie si le joueur a appuyé sur le bouton de tir de l’un des trois types d’entrées.

```cpp
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || PollingFireInUse());
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









Examinons à présent l’implémentation de chacun des trois types de contrôle un peu plus en détail.

## <a name="adding-relative-mouse-controls"></a>Ajout de contrôles de souris relatifs


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
        rotationDelta.x  = mouseDelta.x * MoveLookConstants::RotationGain;   // scale for control sensitivity
        rotationDelta.y  = mouseDelta.y * MoveLookConstants::RotationGain;

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

## <a name="adding-touch-support"></a>Ajout de la prise en charge tactile

Les contrôles tactiles sont parfaits pour la prise en charge des utilisateurs de tablettes. Ce jeu recueille l’entrée tactile en segmentant l'écran et en en désactivant certaines zones, chacune étant alignée sur des actions spécifiques du jeu.
L'entrée tactile de ce jeu utilise trois zones.

![disposition tactile déplacement/vue](images/simple-dx-game-controls-touchzones.png)

Les commandes suivantes résument le comportement de nos contrôles tactiles.
Entrée de l’utilisateur | Action
:------- | :--------
Rectangle de déplacement | L’entrée tactile est convertie en joystick virtuel où le mouvement vertical doit être converti en mouvement de position avant/arrière et le mouvement horizontal doit être converti en mouvement de position gauche/droite.
Rectangle de tir | Tirer une sphère.
Interaction tactile en dehors du rectangle de déplacement et de tir | Modifier la rotation (tangage et lacet) de la vue caméra.

**MoveLookController** vérifie l’ID de pointeur pour déterminer où l’événement s’est produit, et entreprend l’une des actions suivantes:

-   Si l’événement [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) s’est produit dans le rectangle de déplacement ou de tir, la position du pointeur est mise à jour pour la manette.
-   Si l’événement [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) s’est produit ailleurs dans le reste de l’écran (défini comme les contrôles de vue), la modification des tangage et lacet du vecteur de direction de la vue est calculée.


Une fois que nous avons implémenté nos contrôles tactiles, les rectangles que nous avons tracés précédemment à l’aide de Direct2D indiquent aux joueurs où se trouvent les zones de déplacement, de tir et de vue.


![contrôles tactiles](images/simple-dx-game-controls-showzones.png)


Examinons maintenant comment implémenter chaque contrôle.


### <a name="move-and-fire-controller"></a>Contrôleur de déplacement et de tir
Le rectangle de contrôleur de déplacement dans le quadrant inférieur gauche de l’écran sert de pavé directionnel. En faisant glisser votre pouce vers la gauche ou la droite dans cet espace, vous faites bouger le joueur vers la gauche ou la droite, alors qu'un mouvement vers le haut et le bas déplace la caméra vers l'avant et l'arrière.
Une fois cela configuré, le fait d'appuyer sur le contrôleur de tir dans le quadrant inférieur droit de l’écran a pour effet de tirer une sphère.

Les méthodes [**SetMoveRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) et [**SetFireRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867) créent nos rectangles d’entrée, en prenant deux vecteurs 2D pour spécifier les positions des coins supérieur gauche et inférieur droit de chaque rectangle à l'écran. 


Les paramètres sont ensuite passés à **m\_fireUpperLeft** et **m\_fireLowerRight** qui nous aideront à déterminer si l’utilisateur est en contact avec l'intérieur d'un de ces rectangles. 
```cpp
m_fireUpperLeft  = upperLeft;
m_fireLowerRight = lowerRight;
```

Si l’écran est redimensionné, ces rectangles sont redessinés à la taille appropriée.


Maintenant que nous avons défini des zones pour nos contrôles, il est temps de déterminer le moment où un utilisateur les utilise effectivement.
Pour ce faire, nous définissons des gestionnaires d’événements dans la méthode **MoveLookController::InitWindow** pour quand l’utilisateur appuie, déplace ou relâche son pointeur.

```cpp
window->PointerPressed +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

window->PointerMoved +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

window->PointerReleased +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);
```


Nous allons tout d’abord déterminer ce qui se passe lorsque l’utilisateur appuie d’abord à l'intérieur du rectangle de déplacement ou de tir à l’aide de la méthode [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313).
Ici, nous vérifions s'il touche un contrôle et si un pointeur se trouve déjà dans ce contrôleur. Si c’est le premier doigt à toucher ce contrôle particulier, nous procédons comme suit.
- Stockez l’emplacement du contact dans **m\_moveFirstDown** ou **m\_fireFirstDown** sous forme de vecteur 2D.
- Affectez l’ID de pointeur à **m\_movePointerID** ou **m\_firePointerID**.
- Définissez l'indicateur **InUse** approprié (**m\_moveInUse** ou **m\_fireInUse**) sur `true` dans la mesure où nous disposons désormais d’un pointeur actif pour ce contrôle.


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

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
                    m_moveFirstDown = position;                 // Save the location of the initial contact
                    m_movePointerID = pointerID;                // Store the pointer using this control
                    m_moveInUse = true;                         // Set InUse flag to signal there is an active move pointer
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
                    m_fireLastPoint = position;     // Save the location of the initial contact
                    m_firePointerID = pointerID;    // Store the pointer using this control
                    m_fireInUse = true;             // Set InUse flag to signal there is an active fire pointer
                }
            }
            ...
```


Maintenant que nous avons déterminé si l’utilisateur touche un contrôle de déplacement ou un contrôle de tir, nous allons voir si le joueur fait des mouvements avec son doigt en contact.
À l’aide de la méthode [**MoveLookController::OnPointerMoved**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395), nous vérifions quel pointeur a bougé, puis stockons sa nouvelle position sous forme de vecteur 2D.  


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math
    
    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        
        // Move control
        if (pointerID == m_movePointerID)
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        // Look control
        else if (pointerID == m_lookPointerID)
        {
            // ...
        }
        // Fire control
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
```


Une fois que l’utilisateur a fait ses mouvements à l'intérieur des contrôles, il relâche le pointeur. À l’aide de la méthode [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500), nous déterminons quel pointeur a été relâché et effectuons une série de réinitialisations.


Si le contrôle de déplacement a été relâché, nous procédons comme suit.
- Définissons la vitesse du joueur sur `0`dans toutes les directions pour l’empêcher de se déplacer dans le jeu.
- Basculons **m\_moveInUse** sur `false` puisque l’utilisateur ne touche plus le contrôleur de déplacement.
- Définissons l’ID de pointeur de déplacement sur `0` puisqu'il n'y a plus de pointeur dans le contrôleur de déplacement.

```cpp
       if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
```


Pour le contrôle de tir, s'il a été relâché, nous nous contentons de basculer l'indicateur **m_fireInUse** sur `false` et l’ID de pointeur de tir sur `0` puisqu'il n'y a plus de pointeur dans le contrôle de tir.
```cpp
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
```

### <a name="look-controller"></a>Contrôleur de vue
Nous considérons les événements de pointeur du périphérique tactile dans les zones inutilisées de l’écran comme le contrôleur de vue. Faire glisser votre doigt autour de cette zone modifie le tangage et le lacet (la rotation) de la caméra du joueur.


Si l'événement **MoveLookController::OnPointerPressed** est déclenché sur un appareil tactile dans cette zone et que l'état du jeu est défini sur **Active**, un ID de pointeur lui est affecté.

```cpp
    if (!m_lookInUse)   // If no pointer is in this control yet:
    {
        m_lookLastPoint = position;                   // Save the pointer for a later move.
        m_lookPointerID = pointerID;                  // Store the pointer using this control.
        m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
        m_lookInUse = true;
    }
```
Vous pouvez voir le code complet de la méthode **MoveLookController::OnPointerPressed** sur [GitHub](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L252-L259).




Ici, **MoveLookController** attribue l’ID de pointeur du pointeur qui a déclenché l’événement à une variable spécifique qui correspond à la zone de vue. Dans le cas d’une entrée tactile qui se produisent dans la zone de vue, la variable **m\_lookPointerID** est définie sur l’ID de pointeur qui a déclenché l’événement. Une variable booléenne, **m\_lookInUse**, est également définie pour indiquer que le contrôle n’a pas encore été relâché.

Examinons maintenant la façon dont l’exemple de jeu gère l’événement d’écran tactile [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276).


Dans la méthode **MoveLookController::OnPointerMoved**, nous cherchons à savoir quel type d’ID de pointeur a été attribué à l’événement. S’il s’agit de **m_lookPointerID**, nous calculons le changement de position du pointeur.
Ensuite, nous utilisons ce delta pour calculer l'ampleur de modification de la rotation. Enfin, nous sommes à un moment où nous pouvons mettre à jour les valeurs **m\_pitch** et **m\_yaw** à utiliser pour modifier la rotation du joueur dans le jeu. 

```cpp
    else if (pointerID == m_lookPointerID)     // This is the look pointer.
    {
        // Look control
        XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move.
        pointerDelta.y = position.y - m_lookLastPoint.y;

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
        m_lookLastPoint = position;                             // Save for the next time through.

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);M_PI / 2.0f, m_pitch);
        }
```





Pour finir, examinons la façon dont l’exemple de jeu gère l’événement d’écran tactile [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279).
Une fois que l’utilisateur a terminé son mouvement tactile et a retiré son doigt de l’écran, [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) est lancé.
Si l’ID du pointeur qui a déclenché l’événement [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) est l’ID du pointeur de déplacement précédemment enregistré, **MoveLookController** affecte la valeur `0` à la vitesse, car le joueur a cessé de toucher la zone de vue.

```cpp
    else if (pointerID == m_lookPointerID)
    {
        m_lookInUse = false;
        m_lookPointerID = 0;
    }
```





## <a name="adding-mouse-and-keyboard-support"></a>Ajout de la prise en charge du clavier et de la souris


La disposition des contrôles de ce jeu pour le clavier et la souris est la suivante.

Entrée de l’utilisateur | Action
:------- | :--------
W | Déplacer le joueur vers l’avant
A | Déplacer le joueur vers la gauche
S | Déplacer le joueur vers l’arrière
D | Déplacer le joueur vers la droite
X | Déplacer la vue vers le haut
Barre d'espace | Déplacer la vue vers le bas
P | Mettre le jeu en pause
Mouvement de la souris | Modifier la rotation (tangage et lacet) de la vue caméra
Bouton gauche de la souris | Tirer une sphère


Pour utiliser le clavier, l’exemple de jeu enregistre deux nouveaux événements, [**CoreWindow::KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271) et [**CoreWindow::KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270), dans la méthode [**MoveLookController::InitWindow **](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88). Ces événements gèrent la pression et le relâchement d'une touche.

```cpp
window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);
```

La souris est traitée un peu différemment des contrôles tactiles, même si elle utilise un pointeur. Pour s’aligner avec notre disposition de contrôle, **MoveLookController** fait pivoter la caméra chaque fois que la souris est déplacée, et tire à chaque pression sur le bouton gauche.


Ceci est traité dans la méthode [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) de **MoveLookController**.

Dans cette méthode, nous cherchons à savoir quel type de périphérique de pointage est utilisé avec l'énumération [`Windows::Devices::Input::PointerDeviceType`](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Input.PointerDeviceType). Si le jeu est dans l'état **Active** et que **PointerDeviceType** n’est pas **Touch**, nous supposons qu'il s'agit d'une entrée de souris.

```cpp
    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Behavior for touch controls
        
        default:
        // Behavior for mouse controls
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
            break;
        }
```

Lorsque le joueur arrête d’appuyer sur un des boutons de la souris, l'événement de souris [CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased) est déclenché; cela appelle la méthode [MoveLookController::OnPointerReleased](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500), et l’entrée est terminée. À ce stade, si le bouton gauche était appuyé et est maintenant relâché, le tir de sphères cesse. Dans la mesure où la vue est toujours activée, le jeu continue d’utiliser le même pointeur de souris pour le suivi des événements de vue en cours.

```cpp
    case MoveLookControllerState::Active:
        // Touch points
        if (pointerID == m_movePointerID)
        {
            // Stop movement
        }
        else if (pointerID == m_lookPointerID)
        {
            // Stop look rotation
        }
        // Fire button has been released
        else if (pointerID == m_firePointerID)
        {
            // Stop firing
        }
        // Mouse point
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            // Mouse no longer in use so stop firing
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



Maintenant, examinons le dernier type de contrôle que nous allons prendre en charge: les boîtiers de commande. Les boîtiers de commande sont gérés séparément des contrôles tactiles et de souris, car ils n’utilisent pas l’objet pointeur. Pour cette raison, il convient d'ajouter quelques nouveaux gestionnaires d’événements et méthodes.

## <a name="adding-gamepad-support"></a>Ajout de la prise en charge du boîtier de commande


Pour ce jeu, la prise en charge du boîtier de commande est ajouté par les appels aux API [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input). Cet ensemble d’API vous donne accès aux entrées du contrôleur de jeu telles que les volants de course et les manches à balai. 


Les éléments suivants seront nos contrôles du boîtier de commande.

Entrée de l’utilisateur | Action
:------- | :--------
Stick analogique vers la gauche | Déplacer le joueur
Stick analogique vers la droite | Modifier la rotation (tangage et lacet) de la vue caméra
Gâchette droite | Tirer une sphère
Bouton Démarrer/Menu | Mettre en pause ou reprendre le jeu




Dans la méthode [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103), nous ajoutons deux nouveaux événements pour déterminer si un boîtier de commande a été [ajouté](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105) ou [supprimé](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114). Ces événements mettent à jour la propriété **m_gamepadsChanged**. Cela est utilisé dans la méthode **UpdatePollingDevices** pour vérifier si la liste des boîtiers de commande connus a changé. 

```cpp
    // Detect gamepad connection and disconnection events.
    Gamepad::GamepadAdded +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadAdded);

    Gamepad::GamepadRemoved +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadRemoved);
```

### <a name="the-updatepollingdevices-method"></a>La méthode UpdatePollingDevices

La méthode [**UpdatePollingDevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782) de l’instance **MoveLookController** vérifie immédiatement si un boîtier de commande est connecté. S'il y en a un, nous lisons son état à l'aide de [**Gamepad.GetCurrentReading**](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading). Il renvoie la structure [**GamepadReading**](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.GamepadReading), ce qui nous permet de savoir quels sont les boutons qui ont été utilisés ou les joysticks qui ont bougé.


Si l’état du jeu est **WaitForInput**, nous écoutons uniquement le bouton Démarrer/Menu du contrôleur qui permet de reprendre le jeu.


S’il est **Active**, nous vérifions l’entrée de l'utilisateur et déterminons quelle action dans le jeu doit se produire.
Par exemple, si l’utilisateur a déplacé le stick analogique gauche dans une direction spécifique, cela informe le jeu qu'il faut déplacer le joueur dans la direction de déplacement du stick analogique. Le mouvement effectué dans une direction spécifique doit dépasser le rayon de la **zone morte**; sinon, rien ne se produit. Cette zone morte est nécessaire pour gérer les « effleurements », c’est-à-dire lorsque le contrôleur détecte de petits mouvements effectués par le pouce du joueur lorsqu’il repose sur le joystick. Sans zone morte, les contrôles peuvent paraître trop sensibles à l’utilisateur.


L'entrée du joystick est comprise entre -1 et 1 pour les axes x et y. La constante suivante spécifie le rayon de la zone morte du joystick.

```cpp
#define THUMBSTICK_DEADZONE 0.25f
```

À l’aide de cette variable, nous allons commencer à traiter des entrées de joystick exploitables. Un mouvement se produit avec une valeur allant de [-1,-0,26] ou [0,26, 1] sur l'un des deux axes.

![zone morte pour les joysticks](images/simple-dx-game-controls-deadzone.png)

Cette partie de la méthode **UpdatePollingDevices** gère les joysticks gauche et droit.
Les valeurs X et Y de chaque joystick sont vérifiées pour voir si elles sont en dehors de la zone morte. Si l'une ou les deux le sont, nous mettrons à jour la composante correspondante.
Par exemple, si le joystick analogique gauche est déplacé vers la gauche sur l’axe X, nous allons ajouter -1 à la composante **x** du vecteur **m_moveCommand**. Ce vecteur est celui qui sera utilisé pour agréger tous les mouvements provenant de tous les appareils et sera plus tard utilisé pour calculer où doit se déplacer le joueur. 


```cpp
        // Use the left thumbstick to control the eye point position (position of the player)

        // Check if left thumbstick is outside of dead zone on x axis
        if (reading.LeftThumbstickX > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickX < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on x axis
            float x = static_cast<float>(reading.LeftThumbstickX);
            // Set the x of the move vector to 1 if the stick is being moved right.
            // Set to -1 if moved left. 
            m_moveCommand.x -= (x > 0) ? 1 : -1;
        }

        // Check if left thumbstick is outside of dead zone on y axis
        if (reading.LeftThumbstickY > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickY < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on y axis
            float y = static_cast<float>(reading.LeftThumbstickY);
            // Set the y of the move vector to 1 if the stick is being moved forward.
            // Set to -1 if moved backwards. 
            m_moveCommand.y += (y > 0) ? 1 : -1;
        }

```

De même manière que le joystick gauche contrôle le déplacement, le joystick droit contrôle la rotation de la caméra.


Le comportement du joystick droit s'aligne sur celui du déplacement de la souris dans notre configuration du contrôle à la souris et au clavier.
Si le joystick est en dehors de la zone morte, nous calculons la différence entre la position actuelle du pointeur actuel et la position que cherche à regarder l’utilisateur.
Ce changement de position du pointeur (**pointerDelta**) est ensuite utilisé pour mettre à jour le tangage et le lacet de la rotation de la caméra qui seront ultérieurement appliqués dans notre méthode **Update**.
Le vecteur **pointerDelta** peut sembler familier, car il est également utilisé dans la méthode [MoveLookController::OnPointerMoved](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395) pour assurer le suivi du changement de position du pointeur pour les entrées tactile et de la souris.


```cpp
        // Use the right thumbstick to control the look at position

        XMFLOAT2 pointerDelta;

        // Check if right thumbstick is outside of deadzone on x axis
        if (reading.RightThumbstickX > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickX < -THUMBSTICK_DEADZONE)
        {
            float x = static_cast<float>(reading.RightThumbstickX);
            // Register the change in the pointer along the x axis
            pointerDelta.x = x * x * x; 
        }
        // No actionable thumbstick movement. Register no change in pointer.
        else
        {
            pointerDelta.x = 0.0f;
        }
        // Check if right thumbstick is outside of deadzone on y axis
        if (reading.RightThumbstickY > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickY < -THUMBSTICK_DEADZONE)
        {
            float y = static_cast<float>(reading.RightThumbstickY);
            // Register the change in the pointer along the y axis
            pointerDelta.y = y * y * y;
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
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

Les contrôles du jeu ne seraient pas complets sans la possibilité de tirer des sphères!


Cette méthode **UpdatePollingDevices** vérifie également si la gâchette droite est utilisée. Si elle l'est, notre propriété **m_firePressed** bascule sur true, pour signaler au jeu qu'il faut commencer à tirer des sphères.
```cpp
    if (reading.RightTrigger > TRIGGER_DEADZONE)
    {
        if (!m_autoFire && !m_gamepadTriggerInUse)
        {
            m_firePressed = true;
        }

        m_gamepadTriggerInUse = true;
    }
    else
    {
        m_gamepadTriggerInUse = false;
    }
```


## <a name="the-update-method"></a>La méthode Update

Pour conclure, nous allons approfondir l'étude de la méthode [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096).
Cette méthode fusionne tous les mouvements et rotations effectués par le joueur à l'aide de toutes les entrées prises en charge pour générer un vecteur de vélocité et mettre à jour les valeurs de tangage et de lacet accessibles à notre boucle de jeu.


La méthode **Update** commence par appeler [**UpdatePollingDevices**](#the-updatepollingdevices-method) pour mettre à jour l’état du contrôleur. Cette méthode rassemble également toutes les entrées d’un boîtier de commande et ajoute ses mouvements au vecteur **m_moveCommand**. 

Dans notre méthode **Update**, nous procédons aux vérifications d'entrée suivantes.
- Si le joueur utilise le rectangle du contrôleur de déplacement, nous déterminons la modification de la position du pointeur et l'utilisons pour calculer si l’utilisateur a déplacé le pointeur hors de la zone morte du contrôleur. S'il est en dehors de la zone morte, la propriété du vecteur **m_moveCommand** est mise à jour avec la valeur du joystick virtuel.
- Si l'une des entrées de mouvement au clavier est utilisée, la valeur `1.0f` ou `-1.0f`est ajoutée à la composante correspondante du vecteur **m_moveCommand**, &mdash;`1.0f`pour avancer, et `-1.0f` pour reculer.


Une fois que toutes les entrées de mouvement ont été prises en compte, nous appliquons quelques calculs au vecteur **m_moveCommand** pour générer un nouveau vecteur qui représente la direction du joueur dans l'univers du jeu.
Nous prenons alors nos mouvements relativement à cet univers et les appliquons au joueur sous la forme d'une vélocité dans cette direction.
Enfin, nous réinitialisons le vecteur **m_moveCommand** à `(0.0f, 0.0f, 0.0f)` de sorte que tout soit prêt pour l’image suivante du jeu.


```cpp
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    // Get change in 
    if (m_moveInUse)
    {
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
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z =  wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y =  wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```


## <a name="next-steps"></a>Étapes suivantes

Maintenant que nous avons ajouté nos contrôles, nous devons ajouter une autre fonctionnalité pour créer un jeu immersif: le son!
La musique et les effets sonores étant un élément essentiel dans tout jeu, abordons l’[ajout de son](tutorial--adding-sound.md) dans la suite.
 

 

 




