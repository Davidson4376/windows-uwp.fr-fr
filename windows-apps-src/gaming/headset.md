---
title: Casque
description: Utilisez les API de casque Windows.Gaming.Input pour détecter les casques, capturer la voix des joueurs et lire du contenu audio.
ms.assetid: 021CCA26-D339-4C8B-B084-0D499BD83ABE
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, casque
ms.localizationpriority: medium
ms.openlocfilehash: b3de68cc59c9928a52eba5caeb840e9e825eecf0
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7989442"
---
# <a name="headset"></a>Casque

Cette page décrit les concepts de base de la programmation pour les casques à l’aide des API [Windows.Gaming.Input.Headset][headset] et des API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cette page:
* Accéder à un casque connecté à un périphérique d’entrée ou de navigation
* Détecter la connexion ou la déconnexion d’un casque


## <a name="headset-overview"></a>Vue d’ensemble du casque

Les casques sont des périphériques de capture et de lecture audio utilisés la plupart du temps pour communiquer avec d’autres joueurs participant aux jeux en ligne. Cependant, ils peuvent aussi être utilisés dans les séquences de jeu ou pour d’autres usages créatifs. Les casques sont pris en charge dans les applications UWP Windows10 et Xbox par l’espace de noms [Windows.Gaming.Input][].


## <a name="detect-and-track-headsets"></a>Détecter et suivre des casques

Comme les casques sont gérés par le système, vous n’avez pas à les créer ni à les initialiser. Le système permet d’accéder à un casque via le périphérique d’entrée auquel il est connecté, et il fournit des événements pour vous informer de la connexion ou de la déconnexion d’un casque.

### <a name="igamecontrollerheadset"></a>IGameController.Headset

Tous les périphériques d’entrée dans l’espace de noms [Windows.Gaming.Input][] implémentent l’interface [IGameController][] qui définit la propriété [Headset][igamecontroller.headset] comme casque actuellement connecté au périphérique.

### <a name="connecting-and-disconnecting-headsets"></a>Connexion et déconnexion des casques.

Lorsqu’un casque est connecté ou déconnecté, les événements [HeadsetConnected][igamecontroller.headsetconnected] et [HeadsetDisconnected][igamecontroller.headsetdisconnected] se déclenchent. Vous pouvez inscrire des gestionnaires pour ces événements afin de déterminer si un casque est actuellement connecté à un périphérique d’entrée.

L’exemple suivant montre comment inscrire un gestionnaire pour l’événement `HeadsetConnected`.

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetConnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // enable headset capture and playback on this device
}
```

L’exemple suivant montre comment inscrire un gestionnaire pour l’événement `HeadsetDisconnected`.

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetDisconnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // disable headset capture and playback on this device
}
```

## <a name="using-the-headset"></a>Utilisation du casque

La classe [Headset][] est constituée de deuxchaînes qui représentent les ID du point de terminaison XAudio: l’un pour la capture audio (enregistrement à partir du microphone sur casque) et l’autre pour la restitution audio (lecture via l’écouteur du casque).

Les détails de l’utilisation du point de terminaison XAudio ne sont pas couverts ici. Pour en savoir plus, voir le [Guide de programmation XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415737.aspx) et les [informations de référence sur l’API XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415899.aspx).


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.headset]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headset.aspx
[igamecontroller.headsetconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetconnected.aspx
[igamecontroller.headsetdisconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetdisconnected.aspx
[casque]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.headset.aspx
