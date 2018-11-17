---
author: scottmill
title: Mouvements de souris relatifs
description: Utilisez des contrôles de souris relatifs, qui n’utilisent pas le curseur système et ne retournent pas de coordonnées d’écran absolues, pour suivre le delta des pixels entre les mouvements de la souris dans les jeux.
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, souris, entrée
ms.assetid: 08c35e05-2822-4a01-85b8-44edb9b6898f
ms.localizationpriority: medium
ms.openlocfilehash: adf3b629095f633521b99133ce1961e5c8408ef5
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7171818"
---
# <a name="relative-mouse-movement-and-corewindow"></a>Mouvements de souris relatifs et CoreWindow

Dans les jeux, la souris est une option de contrôle commune que de nombreux joueurs ont l’habitude d’utiliser. De même, elle est indispensable à de nombreux types de jeux, notamment les jeux de tir à la première personne et à la troisième personne, ainsi que les jeux de stratégie en temps réel. Ici, nous abordons l’implémentation des contrôles relatifs basés sur la souris, qui n’utilisent pas le curseur système et ne retournent pas de coordonnées d’écran absolues. En revanche, ils suivent le delta des pixels entre les mouvements de la souris.

Certaines applications, telles que les jeux, utilisent la souris comme un périphérique d’entrée plus général. Par exemple, un modeleur 3D peut utiliser l’entrée à l’aide de la souris pour orienter un objet 3D en simulant un trackball virtuel. Par ailleurs, un jeu peut utiliser la souris pour changer la direction de la caméra via des contrôles de déplacement/vue basés sur la souris. 

Dans ces scénarios, l’application requiert des données relatives de la part de la souris. Les valeurs relatives de la souris représentent la distance parcourue par la souris depuis la dernière trame, au lieu des valeurs absolues des coordonnées x-y au sein d’une fenêtre ou d’un écran. En outre, les applications masquent souvent le curseur de la souris, dans la mesure où la position du curseur par rapport aux coordonnées de l’écran n’est pas pertinente lors de la manipulation d’un objet ou d’une scène 3D. 

Lorsque l’utilisateur effectue une action qui place l’application dans un mode de manipulation d’objet/de scène 3D relatif, l’application doit : 
- ignorer la gestion par défaut de la souris ;
- activer la gestion relative de la souris ;
- masquer le curseur de la souris en le définissant en tant que pointeur Null (nullptr). 

Lorsque l’utilisateur effectue une action qui sort l’application d’un mode de manipulation d’objet/de scène 3D relatif, l’application doit : 
- activer la gestion par défaut/absolue de la souris ;
- désactiver la gestion relative de la souris ; 
- affecter au curseur de la souris une valeur non Null (ce qui le rend visible).

> **Remarque**  
Avec ce modèle, l’emplacement du curseur de la souris en mode absolu est conservé lors du passage en mode relatif sans curseur. Le curseur réapparaît à l’écran aux coordonnées qui étaient les siennes avant le passage en mode relatif des mouvements de la souris.

 

## <a name="handling-relative-mouse-movement"></a>Gestion des mouvements de souris relatifs


Pour accéder au delta des valeurs relatives de la souris, inscrivez-vous à l’événement [MouseDevice::MouseMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mousedevice.mousemoved.aspx), comme indiqué ici.


```cpp


// register handler for relative mouse movement events
Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);


```

```cpp


void MoveLookController::OnMouseMoved(
    _In_ Windows::Devices::Input::MouseDevice^ mouseDevice,
    _In_ Windows::Devices::Input::MouseEventArgs^ args
    )
{
    float2 pointerDelta;
    pointerDelta.x = static_cast<float>(args->MouseDelta.X);
    pointerDelta.y = static_cast<float>(args->MouseDelta.Y);

    float2 rotationDelta;
    rotationDelta = pointerDelta * ROTATION_GAIN;   // scale for control sensitivity

    // update our orientation based on the command
    m_pitch -= rotationDelta.y;                     // mouse y increases down, but pitch increases up
    m_yaw   -= rotationDelta.x;                     // yaw defined as CCW around y-axis

    // limit pitch to straight up or straight down
    float limit = (float)(M_PI/2) - 0.01f;
    m_pitch = (float) __max( -limit, m_pitch );
    m_pitch = (float) __min( +limit, m_pitch );

    // keep longitude in useful range by wrapping
    if ( m_yaw >  M_PI )
        m_yaw -= (float)M_PI*2;
    else if ( m_yaw < -M_PI )
        m_yaw += (float)M_PI*2;
}

```

Le gestionnaire d’événements de cet exemple de code, **OnMouseMoved**, effectue le rendu de la vue en fonction des mouvements de la souris. La position du pointeur de la souris est passée au gestionnaire en tant qu’objet [MouseEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mouseeventargs.aspx). 

Ignorez le traitement des données de souris absolues de l’événement [CoreWindow::PointerMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointermoved.aspx) quand votre application passe en mode de gestion des valeurs relatives liées aux mouvements de la souris. Cependant, vous ne devez ignorer cette entrée que si l’événement **CoreWindow::PointerMoved** est survenu à la suite d’une entrée de souris (par opposition à une entrée tactile). Le curseur est masqué via l’affectation de **nullptr** à [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx). 

## <a name="returning-to-absolute-mouse-movement"></a>Retour aux mouvements de souris absolus

Quand l’application sort du mode de manipulation d’objet/de scène 3D et qu’elle n’utilise plus les mouvements de souris relatifs (par exemple, quand elle revient à un écran de menu), revenez au traitement normal des mouvements de souris absolus. À ce stade, arrêtez de lire les données de souris relatives, redémarrez le traitement des événements de souris (et de pointeur) standard, puis affectez à [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx) une valeur non Null. 

> **Remarque**  
Quand votre application est en mode de manipulation d’objet/de scène 3D (mode de traitement des mouvements de souris relatifs sans curseur), la souris ne peut pas invoquer d’éléments d’interface utilisateur latérale, par exemple les icônes, l’historique de navigation ou la barre de l’application. Par conséquent, il est important de fournir un mécanisme de sortie de ce mode particulier, par exemple via la fameuse touche **Échap**.

## <a name="related-topics"></a>Rubriques connexes

* [Contrôles de déplacement/vue pour les jeux](tutorial--adding-move-look-controls-to-your-directx-game.md) 
* [Contrôles tactiles pour les jeux](tutorial--adding-touch-controls-to-your-directx-game.md)