---
title: Lors de la récupération utilisateur de système de Windows sur UWP
description: Découvrez comment récupérer l’utilisateur du système Windows dans un jeu de plateforme universelle Windows (UWP).
ms.date: 06/07/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, utilisateur système
ms.localizationpriority: medium
ms.openlocfilehash: c46f7e98c2dea3b23beb2cec80816067d4c4e341
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632754"
---
# <a name="retrieving-the-windows-system-user-in-a-universal-windows-platform-uwp-title"></a>Récupération de l’utilisateur du système Windows dans un titre de la plateforme universelle Windows (UWP)

## <a name="windowssystemuser"></a>Windows.System.User

Un [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) objet représente un utilisateur Windows local. Sur la console Xbox, il permet à plusieurs utilisateur windows se connecte à la fois dans une seule session interactive, si votre application est une application multi-utilisateur, puis un objet Windows.System.User est requis pour construire un XboxLiveUser afin d’accéder aux services live. Sur d’autres plateformes windows, tels que des PC ou Windows phone, il a uniquement autoriser un utilisateur de windows sur un périphérique ou une seule session interactive, en passant un Windows.System.User pour construire un XboxLiveUser n’est pas requis.

## <a name="ways-to-retrieve-windows-system-user"></a>Façons de récupérer les utilisateurs du système Windows

* **À l’aide de méthodes statiques à partir de [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) classe.**

  [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) classe fournit un ensemble de la méthode statique pour aider à récupérer des objets de Windows.System.User. Par exemple, vous pouvez appeler FindAllAsync pour obtenir tous les utilisateurs windows active.

* **[UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker)**

  [Windows.System.UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker) classe fournit des méthodes pour lancer le sélecteur d’utilisateur l’interface utilisateur et de sélectionner un utilisateur de système de windows dans les scénarios d’utilisateurs multiples.

* **[IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller)**

  [Windows.Gaming.Input.IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller) est l’interface principale pour tous les appareils de contrôleur (gamepad, volant, stick vol etc.). Vous pouvez obtenir l’objet de Windows.System.User associé avec le contrôleur de jeu en appelant sa propriété de l’utilisateur.  

  Voici le contrôleur de jeu de deux implémentée par windows [ArcadeStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick), [Gamepad](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamepad), [RacingWheel](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.racingwheel).

* **[UserDeviceAssociation](https://docs.microsoft.com/en-us/uwp/api/windows.system.userdeviceassociation)**

  Vous pouvez appeler la méthode statique FindUserFromDeviceId pour trouver l’utilisateur avec l’id d’appareil associé. Vous pouvez généralement obtenir l’id d’appareil à partir des événements d’entrée de windows, telles que [Windows.UI.Xaml.Input.KeyRoutedEventArgs](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs), [Windows.UI.Core.KeyEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.ui.core.keyeventargs)
