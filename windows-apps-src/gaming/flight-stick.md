---
title: Manche à balai
description: Utilisez les API de manche à balai Windows.Gaming.Input pour lire les entrées des manches à balai.
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp, jeux, entrée, manche à balai
ms.localizationpriority: medium
ms.openlocfilehash: 5eceb30c62f1e803397aff71d59b560c39736cf9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609014"
---
# <a name="flight-stick"></a>Manche à balai

Cet article explique les concepts de base de la programmation des manches à balai certifiés Xbox One à l’aide de [Windows.Gaming.Input.FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) et des API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article :

* Obtention d’une liste des manches à balai connectés et de leurs utilisateurs
* Détection de l’ajout ou de la suppression d’un manche à balai
* Lecture des entrées provenant d’un ou de plusieurs manches à balai
* Comportement des manches à balai en tant que périphériques de navigation d’interface utilisateur

## <a name="overview"></a>Vue d’ensemble

Les manches à balai sont des périphériques d’entrée de gaming qui permettent de reproduire la sensation des manches à balai utilisés dans le poste de pilotage d’un avion ou d’un vaisseau spatial. Ces manches à balai constituent le périphérique d’entrée idéal pour un contrôle de pilotage rapide et précis. Les manches à balai sont pris en charge dans les applications UWP Windows 10 et Xbox One par l’espace de noms [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input).

Les manches à balai Xbox One sont dotés des contrôles suivants :

* Manette de jeu analogique pivotable avec roulis, tangage et lacet
* Contrôle d’accélération analogique
* Deux boutons de tir
* Bouton champignon numérique à 8 voies
* Touches **Affichage** et **Menu**

> [!NOTE]
> Les touches **Affichage** et **Menu** prennent en charge la navigation d’interface utilisateur, mais non les commandes de jeu, et ne sont donc pas utilisables en tant que touches de manette de jeu.

### <a name="ui-navigation"></a>Navigation d’interface utilisateur

Pour réduire la difficulté associée à la prise en charge de plusieurs périphériques d’entrée différents lors de la navigation dans l’interface utilisateur, et pour favoriser la cohérence entre les jeux et les périphériques, la plupart des périphériques d’entrée _physiques_ agissent en tant que périphérique d’entrée _logique_ distinct, appelé [contrôleurs de navigation d’interface utilisateur](ui-navigation-controller.md). Le contrôleur de navigation d’interface utilisateur fournit un vocabulaire commun pour les commandes de navigation dans l’interface utilisateur, sur plusieurs périphériques d’entrée.

En tant que contrôleur de navigation d’interface utilisateur, un manche à balai mappe l’[ensemble obligatoire](ui-navigation-controller.md#required-set) de commandes de navigation sur la manette de jeu et sur les touches **Affichage**, **Menu**, **FirePrimary** et **FireSecondary**.

| Commande de navigation | Entrée de manche à balai                  |
| ------------------:| ----------------------------------- |
|                 Up (Haut) | Manette de jeu vers le haut                         |
|               Vers le bas | Manette de jeu vers le bas                       |
|               Left (Gauche) | Manette de jeu vers la gauche                       |
|              Droit | Manette de jeu vers la droite                      |
|               Affichage | Touche **Affichage**                     |
|               Menu | Touche **Menu**                     |
|             Accept | Touche **FirePrimary**              |
|             Cancel | Touche **FireSecondary**            |

Les manches à balai ne mappent aucun [ensemble facultatif](ui-navigation-controller.md#optional-set) de commandes de navigation.

## <a name="detect-and-track-flight-sticks"></a>Détecter et suivre des manches à balai

La détection et le suivi des manches à balai fonctionnent exactement de la même manière que pour les boîtiers de commande, sauf avec la classe [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) au lieu de la classe [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad). Pour en savoir plus, consultez la rubrique [Boîtier de commande et vibration](gamepad-and-vibration.md).

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

The following example copies all connected flight sticks into a new collection:

```cpp
auto myFlightSticks = ref new Vector<FlightStick^>();

for (auto flightStick : FlightStick::FlightSticks)
{
    // This code assumes that you're interested in all flight sticks.
    myFlightSticks->Append(flightStick);
}
```

### Adding and removing flight sticks

When a flight stick is added or removed, the [FlightStickAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

The following example starts tracking a flight stick that's been added:

```cpp
FlightStick::FlightStickAdded += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    // This code assumes that you're interested in all new flight sticks.
    myFlightSticks->Append(args);
});
```

The following example stops tracking a flight stick that's been removed:

```cpp
FlightStick::FlightStickRemoved += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    unsigned int indexRemoved;

    if (myFlightSticks->IndexOf(args, &indexRemoved))
    {
        myFlightSticks->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each flight stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-flight-stick"></a>Lecture des entrées du manche à balai

Une fois que vous avez identifié le manche à balai qui vous intéresse, vous pouvez commencer à collecter les entrées de ce dernier. Toutefois, contrairement à d’autres sortes d’entrées que vous connaissez peut-être, les manches à balai ne communiquent pas les changements d’état en déclenchant des événements. À la place, vous devez _interroger_ régulièrement ces boîtiers de commande pour connaître leur état actuel.

### <a name="polling-the-flight-stick"></a>Interrogation du manche à balai

Le processus d’interrogation capture un instantané du manche à balai à un instant précis. Cette approche de collecte des entrées convient à la plupart des jeux, dont la logique s’exécute généralement selon une boucle déterministe plutôt que sur la base des événements. Par ailleurs, elle facilite l’interprétation des commandes de jeu, car toutes les entrées sont collectées en même temps, et non les unes après les autres.

Vous interrogez un manche à balai en appelant la fonction [FlightStick.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick.GetCurrentReading). Cette fonction renvoie un objet [FlightStickReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading) qui contient l’état du manche à balai.

L’exemple de code ci-après interroge un manche à balai pour obtenir son état actuel :

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

En plus de l’état du manche à balai, chaque valeur comprend un horodateur qui indique précisément le moment de récupération de cet état. Cet horodatage est utile pour faire le lien avec le minutage des valeurs précédentes ou de la simulation de jeu.

### <a name="reading-the-joystick-and-throttle-input"></a>Lecture des entrées du manche à balai et du contrôle d’accélération

Le manche à balai fournit une valeur analogique comprise entre -1.0 et 1.0 sur les axes X, Y et Z (roulis, tangage et lacet, respectivement). Pour le roulis, une valeur de -1.0 correspond à la position de manette de jeu la plus à gauche, tandis qu’une valeur de 1.0 indique la position la plus à droite. Pour le tangage, une valeur de -1.0 correspond à la position de manette de jeu la plus basse, alors qu’une valeur de 1.0 indique la position la plus haute. Pour le lacet, une valeur de -1.0 correspond à la position pivotée au maximum dans le sens inverse des aiguilles d’une montre, alors qu’une valeur de 1.0 indique la position pivotée au maximum dans le sens des aiguilles d’une montre.

Sur tous les axes, la valeur est d’environ 0.0 lorsque la manette de jeu se trouve en position centrale, mais la valeur exacte peut varier d’une lecture à l’autre. Les stratégies de compensation de cette variation sont décrites plus loin dans cette section.

La valeur du roulis de la manette de jeu est lue à partir de la propriété [FlightStickReading.Roll](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Roll), la valeur du tangage est lue à partir de la propriété [FlightStickReading.Pitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Pitch), et la valeur du lacet est lue à partir de la propriété [FlightStickReading.Yaw](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Yaw) :

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

Si vous examinez les valeurs fournies par la manette de jeu, vous remarquerez que cette dernière n’indique pas toujours la valeur neutre 0.0 lorsqu’elle se trouve en position centrale. La manette de jeu fournit différentes valeurs proches de 0.0 chaque fois qu’elle est déplacée, puis replacée en position centrale. Pour compenser ces variations, vous pouvez implémenter une petite _zone morte_, qui correspond à une plage de valeurs proches de la position centrale idéale à ignorer.

Pour implémenter une zone morte, vous pouvez déterminer la distance de déplacement de la manette de jeu à partir de sa position centrale, et ignorer les valeurs qui sont plus proches que la distance de référence spécifiée. Vous pouvez calculer la distance de manière approximative (elle n’est pas exacte du fait que les lectures des entrées de manette de jeu sont essentiellement des valeurs polaires, et non planaires) en utilisant simplement le théorème de Pythagore. Vous obtenez ainsi une zone morte radiale.

L’exemple ci-après illustre une zone morte radiale simple calculée à l’aide du théorème de Pythagore :

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = pitch * pitch;
float adjacentSquared = roll * roll;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

### <a name="reading-the-buttons-and-hat-switch"></a>Lecture des entrées des boutons, des touches et du bouton champignon

Chacun des deux boutons de tir du manche à balai fournit une lecture numérique indiquant si le bouton se trouve à l’état enfoncé (position basse) ou relâché (position haute). Pour plus d’efficacité, les entrées de bouton ne sont pas représentées individuellement sous forme de valeurs booléennes. Elles sont toutes regroupées dans un seul champ de bits représenté par l’énumération [FlightStickButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickbuttons). En outre, le bouton champignon à 8 voies fournit une direction regroupée dans un seul champ de bit représenté par l’énumération [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition).

> [!NOTE]
> Les manches à balai sont dotés de touches supplémentaires, utilisées pour la navigation dans l’interface utilisateur, telles que les touches **Affichage** et **Menu**. Ces touches ne figurent pas dans l’énumération `FlightStickButtons`. Leurs entrées sont lues uniquement lorsque le manche à balai est utilisé comme périphérique de navigation d’interface utilisateur. Pour plus d’informations, consultez l’article [Contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md).

Les valeurs des boutons sont lues à partir de la propriété [FlightStickReading.Buttons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Buttons). Comme cette propriété est un champ de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est à l’état enfoncé (position basse) lorsque le bit correspondant est défini ; dans le cas contraire, il se trouve à l’état relâché (position haute).

L’exemple ci-après détermine si le bouton **FirePrimary** est enfoncé :

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

L’exemple ci-après détermine si le bouton **FirePrimary** est relâché :

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

Vous pouvez avoir besoin de savoir quand un bouton passe de l’état enfoncé à l’état relâché, ou inversement, si plusieurs boutons sont enfoncés ou relâchés, ou si un groupe de boutons possède une disposition particulière &mdash; certains étant enfoncés et d’autres relâchés. Pour plus d’informations sur la détection de chacun de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

La valeur du bouton champignon est lue à partir de la propriété [FlightStickReading.HatSwitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.HatSwitch). Étant donné que cette propriété constitue également un champ de bits, un masquage au niveau du bit est là encore effectué pour isoler la position du bouton champignon.

L’exemple ci-après détermine si le bouton champignon se trouve en position haute :

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

L’exemple ci-après détermine si le bouton champignon se trouve en position centrale :

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>Voir également

* [Classe de Windows.Gaming.Input.UINavigationController](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Interface de Windows.Gaming.Input.IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Pratiques d’entrée pour les jeux](input-practices-for-games.md)