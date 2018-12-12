---
title: Contrôles de déplacement/vue pour les jeux
description: Découvrez comment ajouter des contrôles de déplacement/vue de souris et de clavier classiques (aussi appelés contrôles de vue à la souris) à votre jeu DirectX.
ms.assetid: 4b4d967c-3de9-8a97-ae68-0327f00cc933
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, déplacement/vue, contrôles
ms.localizationpriority: medium
ms.openlocfilehash: 222f46bbda165442003aecea0bbd138bcb844a3b
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8943276"
---
# <a name="span-iddevgamingtutorialaddingmove-lookcontrolstoyourdirectxgamespanmove-look-controls-for-games"></a><span id="dev_gaming.tutorial__adding_move-look_controls_to_your_directx_game"></span>Contrôles de déplacement/vue pour les jeux



Découvrez comment ajouter des contrôles de déplacement/vue de souris et de clavier classiques (également connus sous le nom de contrôles de vue à la souris) à votre jeu DirectX.

Nous allons également aborder la prise en charge du déplacement et de la vue pour les appareils tactiles, avec le contrôleur de déplacement défini en tant que section inférieure gauche de l’écran qui se comporte comme une entrée directionnelle, et le contrôleur de vue défini pour le reste de l’écran, avec la caméra centrée sur le dernier endroit que le joueur a touché dans cette zone.

Si ce concept de contrôle ne vous est pas du tout familier, pensez-y de la façon suivante : le clavier (ou la zone d’entrée directionnelle tactile) contrôle vos jambes dans cet espace en 3D et se comporte comme si vos jambes étaient uniquement capables de se déplacer vers l’avant ou l’arrière, ou de mitrailler à droite et à gauche. La souris (ou le pointeur tactile) contrôle votre tête. Vous utilisez votre tête pour regarder dans une direction, à gauche ou à droite, vers le haut ou vers le bas, ou quelque part dans ce plan. Si une cible se présente dans votre vue, vous utilisez la souris pour centrer votre vue caméra sur cette cible, puis appuyez sur la touche avant pour avancer vers elle ou la touche arrière pour vous en éloigner. Pour encercler la cible, vous gardez la vue caméra centrée sur la cible, et vous vous déplacez vers la gauche ou la droite en même temps. Vous pouvez constater qu’il s’agit là d’une méthode de contrôle très efficace pour naviguer dans les environnements 3D !

Ces contrôles sont généralement connus sous le nom de contrôles WASD dans le jeu, où les touches W, S, A et D sont utilisées pour le mouvement de caméra fixe de plan x-z, et la souris permet de contrôler la rotation de la caméra autour des axes x et y.

## <a name="objectives"></a>Objectifs


-   Ajouter des contrôles de déplacement/vue de base à votre jeu DirectX à la fois pour la souris et le clavier, et les écrans tactiles.
-   Implémenter une caméra subjective utilisée pour naviguer dans un environnement 3D.

## <a name="a-note-on-touch-control-implementations"></a>Note sur les implémentations de contrôles tactiles


Pour les contrôles tactiles, nous implémentons deux contrôleurs : le contrôleur de déplacement, qui gère les mouvements dans le plan x-z par rapport au point de mire de la caméra, et le contrôleur de vue, qui vise le point de mire de la caméra. Notre contrôleur de déplacement est mappé aux boutons WASD du clavier et le contrôleur de vue est mappé à la souris. Cependant, pour les contrôles tactiles, nous devons définir une zone de l’écran qui est utilisée pour les entrées directionnelles, ou les boutons WASD virtuels, avec le reste de l’écran servant d’espace d’entrée pour les contrôles de vue.

Notre écran se présente ainsi.

![Disposition du contrôleur de déplacement/vue](images/movelook-touch.png)

Lorsque vous déplacez le pointeur tactile (pas la souris!) dans la partie inférieure gauche de l’écran, tout mouvement vers le haut déplace la caméra vers l’avant. Tout mouvement vers le bas déplace la caméra vers l’arrière. Cela s’applique également aux mouvements vers la gauche et la droite à l’intérieur de l’espace de pointeur du contrôleur de déplacement. Hors de cet espace, et cela devient un contrôleur de vue, il vous suffit de toucher ou de faire glisser la caméra dans la direction dans laquelle vous souhaitez l’orienter.

## <a name="set-up-the-basic-input-event-infrastructure"></a>Configurer l’infrastructure des événements d’entrée de base


Nous devons commencer par créer notre classe de contrôle à utiliser pour gérer les événements d’entrée à partir de la souris et du clavier, puis mettre à jour la perspective de la caméra en fonction de ces entrées. Puisque nous implémentons des contrôles de déplacement/vue, nous l’appelons **MoveLookController**.

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <DirectXMath.h>

// Methods to get input from the UI pointers
ref class MoveLookController
{
};  // class MoveLookController
```

Créons maintenant un en-tête qui définit l’état du contrôleur de déplacement/vue et sa caméra subjective, plus les méthodes de base et gestionnaires d’événements qui implémentent les contrôles et mettent à jour l’état de la caméra.

```cpp
#define ROTATION_GAIN 0.004f    // Sensitivity adjustment for the look controller
#define MOVEMENT_GAIN 0.1f      // Sensitivity adjustment for the move controller

ref class MoveLookController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // The position of the controller
    float m_pitch, m_yaw;           // Orientation euler angles in radians

    // Properties of the Move control
    bool m_moveInUse;               // Specifies whether the move control is in use
    uint32 m_movePointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_moveFirstDown;          // Point where initial contact occurred
    DirectX::XMFLOAT2 m_movePointerPosition;   // Point where the move pointer is currently located
    DirectX::XMFLOAT3 m_moveCommand;            // The net command from the move control

    // Properties of the Look control
    bool m_lookInUse;               // Specifies whether the look control is in use
    uint32 m_lookPointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_lookLastPoint;          // Last point (from last frame)
    DirectX::XMFLOAT2 m_lookLastDelta;          // For smoothing

    bool m_forward, m_back;         // States for movement
    bool m_left, m_right;
    bool m_up, m_down;


public:

    // Methods to get input from the UI pointers
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

    void OnKeyDown(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    void OnKeyUp(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    // Set up the Controls that this controller supports
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );
    
internal:
    // Accessor to set position of controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

    // Accessor to set position of controller
    void SetOrientation( _In_ float pitch, _In_ float yaw );

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

    // Returns the point  which the controller is facing
    DirectX::XMFLOAT3 get_LookPoint();


};  // class MoveLookController
```

Notre code contient 4 groupes de champs privés. Passons en revue le rôle de chacun d’eux.

Définissons d’abord quelques champs utiles qui contiennent nos informations mises à jour sur notre vue caméra.

-   **m\_position** représente la position de la caméra (et, par conséquent, le plan de vue) dans la scène3D, avec les coordonnées de scène.
-   **m\_pitch** représente le tangage de la caméra, ou sa rotation de haut en bas autour de l’axe X du plan de vue, en radians.
-   **m\_yaw** représente le lacet de la caméra, ou sa rotation de gauche à droite autour de l’axe Y du plan de vue, en radians.

Définissons maintenant les champs à utiliser pour stocker des informations sur l’état et la position de nos contrôleurs. Définissons d’abord les champs dont nous avons besoin pour notre contrôleur de déplacement tactile. (Rien de spécial n’est requis pour l’implémentation clavier du contrôleur de déplacement. Nous devons juste lire les événements de clavier avec des gestionnaires spécifiques.)

-   **m\_moveInUse** indique si le contrôleur de déplacement est en cours d’utilisation.
-   **m\_movePointerID** représente l’ID unique pour le pointeur de déplacement actuel. Nous l’utilisons pour différencier le pointeur de vue du pointeur de déplacement lors de la vérification de la valeur de l’ID de pointeur.
-   **m\_moveFirstDown** est le point de l’écran où le joueur a touché pour la première fois la zone du pointeur du contrôleur de déplacement. Nous utiliserons cette valeur plus tard pour définir une zone morte afin d’empêcher les mini-mouvements de déstabiliser la vue.
-   **m\_movePointerPosition** est le point de l’écran où le joueur a actuellement placé le pointeur. Nous l’utilisons pour déterminer la direction dans laquelle l’utilisateur souhaite se déplacer en l’examinant par rapport à **m\_moveFirstDown**.
-   **m\_moveCommand** est la dernière commande calculée pour le contrôleur de déplacement: haut (avant), bas (arrière), gauche ou droite.

Définissons maintenant les champs à utiliser pour notre contrôleur de vue, les implémentations de souris et tactile.

-   **m\_lookInUse** indique si le contrôle de vue est en cours d’utilisation.
-   **m\_lookPointerID** représente l’ID unique pour le pointeur de vue actuel. Nous l’utilisons pour différencier le pointeur de vue du pointeur de déplacement lors de la vérification de la valeur de l’ID de pointeur.
-   **m\_lookLastPoint** est le dernier point, en coordonnées de scène, qui a été capturé dans la trame précédente.
-   **m\_lookLastDelta** est la différence calculée entre les éléments **m\_position** et **m\_lookLastPoint** actuels.

Enfin, définissons 6valeurs booléennes pour les 6degrés de mouvement, qui permettent d’indiquer l’état actuel de chaque action de déplacement directionnel (activé ou désactivé):

-   **m\_forward**, **m\_back**, **m\_left**, **m\_right**, **m\_up** et **m\_down**.

Nous utilisons les 6gestionnaires d’événements pour capturer les données d’entrée utilisées pour mettre à jour l’état de nos contrôleurs:

-   **OnPointerPressed**. Le joueur a appuyé sur le bouton gauche de la souris avec le pointeur dans l’écran du jeu, ou a touché l’écran.
-   **OnPointerMoved**. Le joueur a déplacé la souris avec le pointeur dans l’écran du jeu, ou a fait glisser le pointeur tactile sur l’écran.
-   **OnPointerReleased**. Le joueur a relâché le bouton gauche de la souris avec le pointeur dans l’écran du jeu, ou a arrêté de toucher l’écran.
-   **OnKeyDown**. Le joueur a appuyé sur une touche.
-   **OnKeyUp**. Le joueur a relâché une touche.

Enfin, nous utilisons les méthodes et propriétés suivantes pour accéder aux informations sur l’état des contrôleurs, les initialiser et les mettre à jour.

-   **Initialize**. Notre application appelle ce gestionnaire d’événements pour initialiser les contrôles et les associer à l’objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) qui décrit notre fenêtre d’affichage.
-   **SetPosition**. Notre application appelle cette méthode pour définir les coordonnées (x, y et z) de nos contrôles dans l’espace de scène.
-   **SetOrientation**. Notre application appelle cette méthode pour définir les tangage et lacet de la caméra.
-   **get\_Position**. Notre application accède à cette propriété pour obtenir la position actuelle de la caméra dans l’espace de scène. Vous utilisez cette propriété comme méthode de communication de la position actuelle de la caméra à l’application.
-   **get\_LookPoint**. Notre application accède à cette propriété pour obtenir le point actuel vers lequel la caméra du contrôleur est orientée.
-   **Update**. Lit l’état des contrôleurs de déplacement et de vue, et met à jour la position de la caméra. Vous appelez continuellement cette méthode à partir de la boucle principale de l’application pour actualiser les données de contrôleur de la caméra et la position de la caméra dans l’espace de scène.

Vous disposez à présent ici de tous les composants nécessaires pour implémenter vos contrôles de déplacement/vue. Rassemblons tous ces éléments.

## <a name="create-the-basic-input-events"></a>Créer les événements d’entrée de base


Le répartiteur d’événements WindowsRuntime fournit 5événements qui doivent être gérés par les instances de la classe **MoveLookController**:

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271)
-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270)

Ces événements sont implémentés sur le type [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Nous supposons que vous disposez d’un objet **CoreWindow** à manipuler. Si vous ne savez pas comment en obtenir un, voir [Configuration de votre application de plateforme Windows universelle (UWP) en C++ pour afficher une vue DirectX](https://msdn.microsoft.com/library/windows/apps/hh465077).

Comme ces événements sont déclenchés pendant que notre application est en cours d’exécution, les gestionnaires mettent à jour les informations sur l’état des contrôleurs définies dans nos champs privés.

Commençons par remplir les gestionnaires d’événements de pointeur de souris et tactile. Dans le premier gestionnaire d’événements, **OnPointerPressed()**, nous obtenons les coordonnées x-y du pointeur à partir du [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) qui gère notre affichage lorsque l’utilisateur clique sur la souris ou touche l’écran dans la zone du contrôleur de vue.

**OnPointerPressed**

```cpp
void MoveLookController::OnPointerPressed(
_In_ CoreWindow^ sender,
_In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    if ( deviceType == PointerDeviceType::Mouse )
    {
        // Action, Jump, or Fire
    }

    // Check  if this pointer is in the move control.
    // Change the values  to percentages of the preferred screen resolution.
    // You can set the x value to <preferred resolution> * <percentage of width>
    // for example, ( position.x < (screenResolution.x * 0.15) ).

    if (( position.x < 300 && position.y > 380 ) && ( deviceType != PointerDeviceType::Mouse ))
    {
        if ( !m_moveInUse ) // if no pointer is in this control yet
        {
            // Process a DPad touch down event.
            m_moveFirstDown = position;                 // Save the location of the initial contact.
            m_movePointerPosition = position;
            m_movePointerID = pointerID;                // Store the id of the pointer using this control.
            m_moveInUse = TRUE;
        }
    }
    else // This pointer must be in the look control.
    {
        if ( !m_lookInUse ) // If no pointer is in this control yet...
        {
            m_lookLastPoint = position;                         // save the point for later move
            m_lookPointerID = args->CurrentPoint->PointerId;  // store the id of pointer using this control
            m_lookLastDelta.x = m_lookLastDelta.y = 0;          // these are for smoothing
            m_lookInUse = TRUE;
        }
    }
}
```

Ce gestionnaire d’événements vérifie que le pointeur n’est pas la souris (dans le cadre de cet exemple, qui prend en charge à la fois les opérations tactiles et avec la souris) et qu’il se trouve dans la zone du contrôleur de déplacement. Si les deux critères sont respectés, il vérifie si une simple pression a été effectuée sur le pointeur, en particulier si ce clic n’est pas lié à une précédente action de vue ou de déplacement, en testant si **m\_moveInUse** présente la valeur false. Si tel est le cas, le gestionnaire capture le point dans la zone du contrôleur de déplacement là où le joueur a appuyé sur le pointeur et affecte la valeur true à **m\_moveInUse** de façon à ce qu’il ne remplace pas la position de départ de l’interaction de l’entrée du contrôleur de déplacement la prochaine fois qu’il est appelé. Il met également à jour l’ID de pointeur du contrôleur de déplacement avec l’ID du pointeur actuel.

Si le pointeur est la souris ou que le pointeur tactile ne se trouve pas dans la zone du contrôleur de déplacement, il doit figurer dans la zone du contrôleur de vue. Il affecte à **m\_lookLastPoint** la position actuelle où l’utilisateur a appuyé sur le bouton de la souris, ou touché et appuyé, réinitialise le delta, et met à jour l’ID de pointeur du contrôleur de vue avec l’ID du pointeur actuel. Il définit également le contrôleur de vue sur l’état actif.

**OnPointerMoved**

```cpp
void MoveLookController::OnPointerMoved(
    _In_ CoreWindow ^sender,
    _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2(args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y);

    // Decide which control this pointer is operating.
    if (pointerID == m_movePointerID)           // This is the move pointer.
    {
        // Move control
        m_movePointerPosition = position;       // Save the current position.

    }
    else if (pointerID == m_lookPointerID)      // This is the look pointer.
    {
        // Look control

        DirectX::XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did pointer move
        pointerDelta.y = position.y - m_lookLastPoint.y;

        DirectX::XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * ROTATION_GAIN;   // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * ROTATION_GAIN;

        m_lookLastPoint = position;                     // Save for the next time through.

                                                        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;                     // Mouse y increases down, but pitch increases up.
        m_yaw -= rotationDelta.x;                       // Yaw is defined as CCW around the y-axis.

                                                        // Limit the pitch to straight up or straight down.
        m_pitch = (float)__max(-DirectX::XM_PI / 2.0f, m_pitch);
        m_pitch = (float)__min(+DirectX::XM_PI / 2.0f, m_pitch);
    }
}
```

Le gestionnaire d’événements **OnPointerMoved** se déclenche à chaque mouvement du pointeur (dans ce cas, si le pointeur tactile est glissé sur l’écran ou que celui de la souris est déplacé avec le bouton gauche maintenu). Si l’ID de pointeur est le même que l’ID de pointeur du contrôleur de déplacement, il s’agit du pointeur de déplacement. Sinon, il convient de vérifier si le pointeur actif est le contrôleur de vue.

S’il s’agit du contrôleur de déplacement, il nous suffit de mettre à jour la position du pointeur. La position est mise à jour tant que l’événement [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) continue de se déclencher, car nous voulons comparer la position finale à la première qui a été capturée à l’aide du gestionnaire d’événements **OnPointerPressed**.

S’il s’agit du contrôleur de vue, les choses sont un peu plus compliquées. Comme nous devons calculer un nouveau point de mire et centrer la caméra dessus, nous calculons le delta entre le dernier point de mire et la position à l’écran, puis le multiplions par le facteur d’échelle que nous pouvons modifier pour augmenter ou réduire les mouvements de vue par rapport à la distance du mouvement d’écran. Cette valeur nous permet de calculer les tangage et lacet.

Enfin, nous devons désactiver les comportements du contrôleur de déplacement ou de vue lorsque le joueur cesse de déplacer la souris ou de toucher l’écran. Nous utilisons **OnPointerReleased**, que nous appelons lorsque [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) est déclenché, pour affecter à **m\_moveInUse** ou **m\_lookInUse** la valeur FALSE et désactiver le mouvement panoramique de la caméra, ainsi que pour mettre à zéro l’ID de pointeur.

**OnPointerReleased**

```cpp
void MoveLookController::OnPointerReleased(
_In_ CoreWindow ^sender,
_In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );


    if ( pointerID == m_movePointerID )    // This was the move pointer.
    {
        m_moveInUse = FALSE;
        m_movePointerID = 0;
    }
    else if (pointerID == m_lookPointerID ) // This was the look pointer.
    {
        m_lookInUse = FALSE;
        m_lookPointerID = 0;
    }
}
```

Jusqu’ici, nous avons géré tous les événements d’écran tactile. Nous allons à présent gérer les événements d’entrée par touche pour un contrôleur de déplacement basé sur clavier.

**OnKeyDown**

```cpp
void MoveLookController::OnKeyDown(
                                   __in CoreWindow^ sender,
                                   __in KeyEventArgs^ args )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // Forward
        m_forward = true;
    if ( Key == VirtualKey::S )     // Back
        m_back = true;
    if ( Key == VirtualKey::A )     // Left
        m_left = true;
    if ( Key == VirtualKey::D )     // Right
        m_right = true;
}
```

Tant que l’une de ces touches est enfoncée, ce gestionnaire d’événements affecte la valeur true à l’état de déplacement directionnel correspondant.

**OnKeyUp**

```cpp
void MoveLookController::OnKeyUp(
                                 __in CoreWindow^ sender,
                                 __in KeyEventArgs^ args)
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // forward
        m_forward = false;
    if ( Key == VirtualKey::S )     // back
        m_back = false;
    if ( Key == VirtualKey::A )     // left
        m_left = false;
    if ( Key == VirtualKey::D )     // right
        m_right = false;
}
```

Lorsque la touche est relâchée, ce gestionnaire d’événements lui réaffecte la valeur false. Lorsque nous appelons **Update**, ces états de déplacement directionnel sont vérifiés et la caméra est déplacée en conséquence. Cela est un peu plus simple que l’implémentation des contrôles tactiles!

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>Initialiser les contrôles tactiles et l’état du contrôleur


Rassemblons à présent les événements et initialisons tous les champs des états du contrôleur.

**Initialize**

```cpp
void MoveLookController::Initialize( _In_ CoreWindow^ window )
{

    // Opt in to receive touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->CharacterReceived +=
    ref new TypedEventHandler<CoreWindow^, CharacterReceivedEventArgs^>(this, &MoveLookController::OnCharacterReceived);

    window->KeyDown += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // Initialize the state of the controller.
    m_moveInUse = FALSE;                // No pointer is in the Move control.
    m_movePointerID = 0;

    m_lookInUse = FALSE;                // No pointer is in the Look control.
    m_lookPointerID = 0;

    //  Need to init this as it is reset every frame.
    m_moveCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

    SetOrientation( 0, 0 );             // Look straight ahead when the app starts.

}
```

**Initialize** fait référence à l’instance [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de l’application en tant que paramètre et inscrit les gestionnaires d’événements développés dans les événements appropriés sur cet élément **CoreWindow**. Il initialise les ID des pointeurs de déplacement et de vue, affecte la valeur zéro au vecteur de commande pour l’implémentation du contrôleur de déplacement d’écran tactile, puis définit la direction droit devant de la caméra au démarrage de l’application.

## <a name="getting-and-setting-the-position-and-orientation-of-the-camera"></a>Obtention et définition de la position et de l’orientation de la caméra


Définissons certaines méthodes pour obtenir et définir la position de la caméra par rapport à la fenêtre d’affichage.

```cpp
void MoveLookController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Accessor to set the position of the controller.
void MoveLookController::SetOrientation( _In_ float pitch, _In_ float yaw )
{
    m_pitch = pitch;
    m_yaw = yaw;
}

// Returns the position of the controller object.
DirectX::XMFLOAT3 MoveLookController::get_Position()
{
    return m_position;
}

// Returns the point at which the camera controller is facing.
DirectX::XMFLOAT3 MoveLookController::get_LookPoint()
{
    float y = sinf(m_pitch);        // Vertical
    float r = cosf(m_pitch);        // In the plane
    float z = r*cosf(m_yaw);        // Fwd-back
    float x = r*sinf(m_yaw);        // Left-right
    DirectX::XMFLOAT3 result(x,y,z);
    result.x += m_position.x;
    result.y += m_position.y;
    result.z += m_position.z;

    // Return m_position + DirectX::XMFLOAT3(x, y, z);
    return result;
}
```

## <a name="updating-the-controller-state-info"></a>Mise à jour des informations sur l’état du contrôleur


Effectuons maintenant nos calculs pour convertir les informations de coordonnées du pointeur suivies dans **m\_movePointerPosition** en nouvelles informations de coordonnées respectives de notre système de coordonnées universelles. Notre application appelle cette méthode chaque fois que nous actualisons la boucle principale de l’application. C’est donc ici que nous calculons les nouvelles informations de position du point de mire à transmettre à l’application pour la mise à jour de la matrice globale avant projection dans la fenêtre d’affichage.

```cpp
void MoveLookController::Update(CoreWindow ^window)
{
    // Check for input from the Move control.
    if (m_moveInUse)
    {
        DirectX::XMFLOAT2 pointerDelta(m_movePointerPosition);
        pointerDelta.x -= m_moveFirstDown.x;
        pointerDelta.y -= m_moveFirstDown.y;

        // Figure out the command from the touch-based virtual joystick.
        if (pointerDelta.x > 16.0f)      // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x = 1.0f;
        else
            if (pointerDelta.x < -16.0f)
            m_moveCommand.x = -1.0f;

        if (pointerDelta.y > 16.0f)      // Joystick y is up, so change sign.
            m_moveCommand.y = -1.0f;
        else
            if (pointerDelta.y < -16.0f)
            m_moveCommand.y = 1.0f;
    }

    // Poll our state bits that are set by the keyboard input events.
    if (m_forward)
        m_moveCommand.y += 1.0f;
    if (m_back)
        m_moveCommand.y -= 1.0f;

    if (m_left)
        m_moveCommand.x -= 1.0f;
    if (m_right)
        m_moveCommand.x += 1.0f;

    if (m_up)
        m_moveCommand.z += 1.0f;
    if (m_down)
        m_moveCommand.z -= 1.0f;

    // Make sure that 45 degree cases are not faster.
    DirectX::XMFLOAT3 command = m_moveCommand;
    DirectX::XMVECTOR vector;
    vector = DirectX::XMLoadFloat3(&command);

    if (fabsf(command.x) > 0.1f || fabsf(command.y) > 0.1f || fabsf(command.z) > 0.1f)
    {
        vector = DirectX::XMVector3Normalize(vector);
        DirectX::XMStoreFloat3(&command, vector);
    }
    

    // Rotate command to align with our direction (world coordinates).
    DirectX::XMFLOAT3 wCommand;
    wCommand.x = command.x*cosf(m_yaw) - command.y*sinf(m_yaw);
    wCommand.y = command.x*sinf(m_yaw) + command.y*cosf(m_yaw);
    wCommand.z = command.z;

    // Scale for sensitivity adjustment.
    wCommand.x = wCommand.x * MOVEMENT_GAIN;
    wCommand.y = wCommand.y * MOVEMENT_GAIN;
    wCommand.z = wCommand.z * MOVEMENT_GAIN;

    // Our velocity is based on the command.
    // Also note that y is the up-down axis. 
    DirectX::XMFLOAT3 Velocity;
    Velocity.x = -wCommand.x;
    Velocity.z = wCommand.y;
    Velocity.y = wCommand.z;

    // Integrate
    m_position.x += Velocity.x;
    m_position.y += Velocity.y;
    m_position.z += Velocity.z;

    // Clear movement input accumulator for use during the next frame.
    m_moveCommand = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);

}
```

Étant donné que nous ne voulons pas de sautillements lorsque le joueur utilise le contrôleur de déplacement tactile, nous définissons une zone morte virtuelle autour du pointeur avec un diamètre de 32 pixels. Nous ajoutons également la vitesse, qui est la valeur de commande plus un rendement de mouvement. (Vous pouvez ajuster ce comportement selon votre goût, pour ralentir ou accélérer la vitesse de mouvement en fonction de la distance de déplacement du pointeur dans la zone du contrôleur de déplacement.)

Lorsque nous calculons la vitesse, nous traduisons également les coordonnées reçues des contrôleurs de déplacement et de vue en mouvement du point de mire réel que nous envoyons à la méthode qui calcule notre matrice globale pour la scène. Nous commençons par inverser la coordonnée x car, si nous effectuons un clic/déplacement ou un glisser vers la gauche ou la droite avec le contrôleur de vue, le point de mire pivote dans la direction opposée dans la scène, car une caméra peut osciller autour de son axe central. Nous échangeons ensuite les axes Y et Z, car une touche haut/bas enfoncée ou un mouvement de glisser tactile (interprété comme un comportement d’axe Y) sur le contrôleur de déplacement doit être traduit en une action de caméra qui déplace le point de mire à l’intérieur et à l’extérieur de l’écran (l’axeZ).

La position finale du point de mire pour le joueur est la dernière position plus la vitesse calculée, et c’est ce qui est lu par le convertisseur lorsqu’il appelle la méthode **get\_Position** (probablement pendant l’installation de chaque trame). Après cela, la commande de déplacement est redéfinie sur zéro.

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>Mise à jour de la matrice globale avec la nouvelle position de la caméra


Nous pouvons obtenir une coordonnée d’espace de scène sur laquelle la caméra fait le point et qui est mise à jour chaque fois que vous demandez à l’application de le faire (toutes les 60 secondes dans la boucle principale de l’application, par exemple). Ce pseudo code suggère le comportement d’appel à implémenter :

```cpp
myMoveLookController->Update( m_window );   

// Update the view matrix based on the camera position.
myFirstPersonCamera->SetViewParameters(
                 myMoveLookController->get_Position(),       // Point we are at
                 myMoveLookController->get_LookPoint(),      // Point to look towards
                 DirectX::XMFLOAT3( 0, 1, 0 )                   // Up-vector
                 ); 
```

Félicitations ! Vous avez implémenté des contrôles de déplacement/vue de base à la fois pour les écrans tactiles et les contrôles d’entrée par souris/clavier dans votre jeu!



 

 




