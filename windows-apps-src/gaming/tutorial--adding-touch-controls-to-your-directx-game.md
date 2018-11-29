---
title: Contrôles tactiles pour les jeux
description: Découvrez comment ajouter des contrôles tactiles de base à votre jeu de plateforme Windows universelle (UWP) en C++ avec DirectX.
ms.assetid: 9d40e6e4-46a9-97e9-b848-522d61e8e109
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, tactile, contrôles, directx, entrée
ms.localizationpriority: medium
ms.openlocfilehash: e8892219b485d320bb77f90ac0d172e8e2403392
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7975358"
---
# <a name="touch-controls-for-games"></a>Contrôles tactiles pour les jeux



Découvrez comment ajouter des contrôles tactiles de base à votre jeu de plateforme Windows universelle (UWP) en C++ avec DirectX. Nous allons vous montrer comment ajouter des contrôles tactiles pour déplacer une caméra de plan fixe dans un environnement Direct3D, où le glissement avec un doigt ou un stylet décale la perspective de la caméra.

Vous pouvez incorporer ces contrôles à des jeux où le joueur doit faire glisser pour effectuer un défilement ou un panoramique dans un environnement 3D, par exemple une carte ou un terrain de jeux. Par exemple, dans un jeu de stratégie ou de casse-tête, vous pouvez utiliser ces contrôles pour permettre au joueur de voir un environnement de jeu plus grand que l’écran par un mouvement panoramique gauche ou droit.

> **Remarque**notre code fonctionne également avec les contrôles panoramiques la souris. Les événements associés au pointeur étant abstraits par les API Windows Runtime, ils peuvent gérer les événements de pointeur tactiles ou avec la souris.

 

## <a name="objectives"></a>Objectifs


-   Créer un simple contrôle de glisser tactile pour le panoramique d’une caméra de plan fixe dans un jeu DirectX.

## <a name="set-up-the-basic-touch-event-infrastructure"></a>Configurer l’infrastructure des événements tactiles de base


Commençons par définir notre type de contrôleur de base, **CameraPanController** dans le cas présent. Ici, nous définissons un contrôleur comme une idée abstraite correspondant à l’ensemble de comportements que l’utilisateur peut effectuer.

La classe **CameraPanController** est une collection régulièrement mise à jour d’informations sur l’état du contrôleur de la caméra, et permet à notre application d’obtenir ces informations à partir de sa boucle de mise à jour.

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <directxmath.h>

// Methods to get input from the UI pointers
ref class CameraPanController
{
}
```

Créons maintenant un en-tête qui définit l’état du contrôleur de la caméra, plus les méthodes de base et gestionnaires d’événements qui implémentent les interactions du contrôleur de la caméra.

```cpp
ref class CameraPanController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // the position of the camera

    // Properties of the camera pan control
    bool m_panInUse;                
    uint32 m_panPointerID;          
    DirectX::XMFLOAT2 m_panFirstDown;           
    DirectX::XMFLOAT2 m_panPointerPosition;   
    DirectX::XMFLOAT3 m_panCommand;         
    
internal:
    // Accessor to set the position of the controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

       // Accessor to set the fixed "look point" of the controller
       DirectX::XMFLOAT3 get_FixedLookPoint();

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

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

    // Set up the Controls supported by this controller
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );

};  // Class CameraPanController
```

Les champs privés contiennent l’état actuel du contrôleur de la caméra. Passons-les en revue.

-   **m\_position** représente la position de la caméra dans l’espace de scène. Dans cet exemple, la valeur de coordonnée z est fixée à 0. Nous pourrions utiliser DirectX::XMFLOAT2 pour représenter cette valeur, mais dans le cadre de cet exemple et à des fins d’extensibilité future, nous utilisons DirectX::XMFLOAT3. Nous transmettons cette valeur par le biais de la propriété **get\_Position** à l’application proprement dite afin qu’elle puisse mettre à jour la fenêtre d’affichage en conséquence.
-   **m\_panInUse** est une valeur booléenne qui indique si une opération panoramique est active ou, plus spécifiquement, si le joueur touche l’écran et déplace la caméra.
-   **m\_panPointerID** représente un ID unique pour le pointeur. Nous ne l’utiliserons pas dans cet exemple, mais il est conseillé d’associer la classe de l’état du contrôleur à un pointeur spécifique.
-   **m\_panFirstDown** est le point de l’écran où le joueur a touché pour la première fois l’écran ou a cliqué à l’aide de la souris pendant l’action panoramique de la caméra. Nous utiliserons cette valeur plus tard pour définir une zone morte afin d’empêcher tout sautillement lorsque l’écran est touché ou que la souris bouge légèrement.
-   **m\_panPointerPosition** est le point de l’écran où le joueur a actuellement placé le pointeur. Nous l’utilisons pour déterminer la direction dans laquelle l’utilisateur souhaite se déplacer en l’examinant par rapport à **m\_panFirstDown**.
-   **m\_panCommand** est la dernière commande calculée pour le contrôleur de la caméra: haut, bas, gauche ou droite. Dans la mesure où nous utilisons une caméra fixée au plan x-y, il pourrait s’agir d’une valeur DirectX::XMFLOAT2 à la place.

Nous utilisons ces 3gestionnaires d’événements pour mettre à jour les informations sur l’état du contrôleur de la caméra.

-   **OnPointerPressed** est un gestionnaire d’événements que notre application appelle lorsque le joueur appuie un doigt sur la surface tactile et que le pointeur est déplacé vers les coordonnées du point sur lequel il a appuyé.
-   **OnPointerMoved** est un gestionnaire d’événements que notre application appelle lorsque le joueur balaye du doigt la surface tactile. Il effectue la mise à jour avec les nouvelles coordonnées du chemin de glissement.
-   **OnPointerReleased** est un gestionnaire d’événements que notre application appelle lorsque le joueur retire le doigt de la surface tactile.

Enfin, nous utilisons les méthodes et propriétés suivantes pour accéder aux informations sur l’état du contrôleur de la caméra, les initialiser et les mettre à jour.

-   **Initialize** est un gestionnaire d’événements que notre application appelle pour initialiser les contrôles et les associer à l’objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) qui décrit la fenêtre d’affichage.
-   **SetPosition** est une méthode que notre application appelle pour définir les coordonnées (x, y et z) des contrôles dans l’espace de scène. Notez que la coordonnée z présente la valeur 0 tout au long de ce didacticiel.
-   **get\_Position** est une propriété à laquelle notre application accède pour obtenir la position actuelle de la caméra dans l’espace de scène. Vous utilisez cette propriété comme méthode de communication de la position actuelle de la caméra à l’application.
-   **get\_FixedLookPoint** est une propriété à laquelle notre application accède pour obtenir le point actuel vers lequel la caméra du contrôleur est orientée. Dans cet exemple, il est verrouillé perpendiculairement au plan x-y.
-   **Update** est une méthode qui lit l’état du contrôleur et met à jour la position de la caméra. Vous appelez continuellement cet &lt;élément&gt; à partir de la boucle principale de l’application pour actualiser les données de contrôleur de la caméra et la position de la caméra dans l’espace de scène.

Vous disposez à présent ici de tous les composants nécessaires pour implémenter les contrôles tactiles. Vous pouvez détecter quand et où les événements de pointeur de souris ou tactile se sont produits, et identifier l’action. Vous pouvez définir la position et l’orientation de la caméra par rapport à l’espace de scène et assurer le suivi des modifications. Enfin, vous pouvez communiquer la nouvelle position de la caméra à l’application appelante.

Rassemblons maintenant tous ces éléments.

## <a name="create-the-basic-touch-events"></a>Créer les événements tactiles de base


Le répartiteur d’événements Windows Runtime fournit 3événements qui doivent être gérés par notre application:

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)

Ces événements sont implémentés sur le type [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Nous supposons que vous disposez d’un objet **CoreWindow** à manipuler. Pour plus d’informations, voir [Configuration d’une application UWP en C++ pour afficher une vue DirectX](https://msdn.microsoft.com/library/windows/apps/hh465077).

Comme ces événements sont déclenchés pendant que notre application est en cours d’exécution, les gestionnaires mettent à jour les informations sur l’état du contrôleur de la caméra définies dans nos champs privés.

Commençons par remplir les gestionnaires d’événements de pointeur tactile. Dans le premier gestionnaire d’événements (**OnPointerPressed**), nous obtenons les coordonnées x-y du pointeur de la part de l’élément [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) qui gère notre affichage lorsque l’utilisateur clique avec la souris ou touche l’écran.

**OnPointerPressed**

```cpp
void CameraPanController::OnPointerPressed(
                                           _In_ CoreWindow^ sender,
                                           _In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    

       if ( !m_panInUse )   // If no pointer is in this control yet.
    {
       m_panFirstDown = position;                   // Save the location of the initial contact.
       m_panPointerPosition = position;
       m_panPointerID = pointerID;              // Store the id of the pointer using this control.
       m_panInUse = TRUE;
    }
    
}
```

Nous utilisons ce gestionnaire pour indiquer à l’instance **CameraPanController** actuelle que le contrôleur de la caméra doit être considéré comme actif en affectant à **m\_panInUse** la valeur TRUE. Ainsi, lorsque l’application appelle **Update**, elle utilise les données de la position actuelle pour mettre à jour la fenêtre d’affichage.

Maintenant que nous avons établi les valeurs de base pour le mouvement de la caméra lorsque l’utilisateur touche l’écran ou clique/appuie dans la fenêtre d’affichage, nous devons identifier les actions à effectuer lorsque l’utilisateur fait glisser le point sur lequel il a appuyé ou déplace la souris avec le bouton enfoncé.

Le gestionnaire d’événements **OnPointerMoved** se déclenche chaque fois que le pointeur se déplace, au niveau de chaque graduation selon laquelle le joueur fait glisser le pointeur sur l’écran. Nous devons tenir l’application informée de l’emplacement actuel du pointeur; voici comment procéder pour cela.

**OnPointerMoved**

```cpp
void CameraPanController::OnPointerMoved(
                                        _In_ CoreWindow ^sender,
                                        _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panPointerPosition = position;
}
```

Enfin, nous devons désactiver le mouvement panoramique de la caméra lorsque le joueur cesse de toucher l’écran. Nous utilisons **OnPointerReleased**, qui est appelé lorsque [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) est déclenché, pour affecter à **m\_panInUse** la valeur FALSE et désactiver le mouvement panoramique de la caméra, ainsi que pour affecter la valeur 0 à l’ID de pointeur.

**OnPointerReleased**

```cpp
void CameraPanController::OnPointerReleased(
                                             _In_ CoreWindow ^sender,
                                             _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panInUse = FALSE;
    m_panPointerID = 0;
}
```

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>Initialiser les contrôles tactiles et l’état du contrôleur


Rassemblons les événements et initialisons tous les champs des états de base du contrôleur de la caméra.

**Initialize**

```cpp
void CameraPanController::Initialize( _In_ CoreWindow^ window )
{

    // Start recieving touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerReleased);


    // Initialize the state of the controller.
    m_panInUse = FALSE;             
    m_panPointerID = 0;

    //  Initialize this as it is reset on every frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

**Initialize** fait référence à l’instance [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de l’application en tant que paramètre et inscrit les gestionnaires d’événements développés dans les événements appropriés sur cet élément **CoreWindow**.

## <a name="getting-and-setting-the-position-of-the-camera-controller"></a>Obtention et définition de la position du contrôleur de la caméra


Définissons certaines méthodes pour obtenir et définir la position du contrôleur de la caméra dans l’espace de scène.

```cpp
void CameraPanController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Returns the position of the controller object
DirectX::XMFLOAT3 CameraPanController::get_Position()
{
    return m_position;
}

DirectX::XMFLOAT3 CameraPanController::get_FixedLookPoint()
{
    // For this sample, we don't need to use the trig functions because our
    // look point is fixed. 
    DirectX::XMFLOAT3 result= m_position;
    result.z += 1.0f;
    return result;    

}
```

**SetPosition** est une méthode publique que nous pouvons appeler à partir de notre application si nous devons définir un point spécifique comme position du contrôleur de la caméra.

**get\_Position** est la propriété publique la plus importante: elle permet à l’application d’obtenir la position actuelle du contrôleur de la caméra dans l’espace de scène et donc de mettre à jour la fenêtre d’affichage en conséquence.

**get\_FixedLookPoint** est une propriété publique qui, dans cet exemple, obtient un point de mire qui est perpendiculaire au plan x-y. Vous pouvez modifier cette méthode pour utiliser les fonctions trigonométriques, sin et cos, lors du calcul des valeurs des coordonnées x, y et z si vous voulez créer d’autres angles obliques pour la caméra fixe.

## <a name="updating-the-camera-controller-state-information"></a>Mise à jour des informations sur l’état du contrôleur de la caméra


Effectuons maintenant nos calculs pour convertir les informations de coordonnées du pointeur suivies dans **m\_panPointerPosition** en nouvelles informations de coordonnées respectives de notre espace de scène3D. Notre application appelle cette méthode chaque fois que nous actualisons la boucle principale de l’application. Nous calculons alors les nouvelles informations de position que nous souhaitons transmettre à l’application utilisée pour la mise à jour de la matrice globale avant projection dans la fenêtre d’affichage.

```cpp

void CameraPanController::Update( CoreWindow ^window )
{
    if ( m_panInUse )
    {
        pointerDelta.x = m_panPointerPosition.x - m_panFirstDown.x;
        pointerDelta.y = m_panPointerPosition.y - m_panFirstDown.y;

        if ( pointerDelta.x > 16.0f )        // Leave 32 pixel-wide dead spot for being still.
            m_panCommand.x += 1.0f;
        else
            if ( pointerDelta.x < -16.0f )
                m_panCommand.x += -1.0f;

        if ( pointerDelta.y > 16.0f )        
            m_panCommand.y += 1.0f;
        else
            if (pointerDelta.y < -16.0f )
                m_panCommand.y += -1.0f;
    }

       DirectX::XMFLOAT3 command = m_panCommand;
   
    // Our velocity is based on the command.
    DirectX::XMFLOAT3 Velocity;
    Velocity.x =  command.x;
    Velocity.y =  command.y;
    Velocity.z =  0.0f;

    // Integrate
    m_position.x = m_position.x + Velocity.x;
    m_position.y = m_position.y + Velocity.y;
    m_position.z = m_position.z + Velocity.z;

    // Clear the movement input accumulator for use during the next frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

Comme nous ne voulons pas que les sautillements tactiles ou de souris donnent un mouvement saccadé au panoramique de la caméra, nous définissons une zone morte autour du pointeur avec un diamètre de 32 pixels. Nous avons également une valeur de vitesse, qui est ici de 1:1 avec la traversée de pixels du pointeur au-delà de la zone morte. Vous pouvez ajuster ce comportement pour ralentir ou accélérer la vitesse de mouvement.

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>Mise à jour de la matrice globale avec la nouvelle position de la caméra


Nous pouvons maintenant obtenir une coordonnée d’espace de scène sur laquelle la caméra fait le point et qui est mise à jour chaque fois que vous demandez à l’application de le faire (toutes les 60 secondes dans la boucle principale de l’application, par exemple). Ce pseudo code suggère le comportement d’appel à implémenter :

```cpp
 myCameraPanController->Update( m_window ); 

 // Update the view matrix based on the camera position.
 myCamera->MyMethodToComputeViewMatrix(
        myController->get_Position(),        // The position in the 3D scene space.
        myController->get_FixedLookPoint(),      // The point in the space we are looking at.
        DirectX::XMFLOAT3( 0, 1, 0 )                    // The axis that is "up" in our space.
        );  
```

Félicitations ! Vous avez implémenté un ensemble simple de contrôles tactiles de panoramique de la caméra dans votre jeu.


 

 

 




