---
title: Contrôleur de jeu brut
description: Utilisez les API du contrôleur de jeu brut Windows.Gaming.Input pour lire les entrées de tous les types de contrôleur de jeu ou presque.
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.date: 03/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, entrée, contrôleur de jeu brut
ms.localizationpriority: medium
ms.openlocfilehash: 7b5f4d49ad49cf9f9065fe17788456e9dd2a4a4e
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8793886"
---
# <a name="raw-game-controller"></a>Contrôleur de jeu brut

Cet article décrit les notions de base de la programmation de pratiquement tous les types de contrôleur de jeu avec l’API [Windows.Gaming.Input.RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) et les API associées pour la plateforme Windows universelle (UWP).

À la lecture de cet article, vous allez découvrir comment:

* obtenir la liste des contrôleurs de jeu bruts connectés et de leurs utilisateurs;
* détecter l’ajout ou la suppression d’un contrôleur de jeu brut;
* obtenir les fonctionnalités d’un contrôleur de jeu brut;
* lire les entrées d’un contrôleur de jeu brut.

## <a name="overview"></a>Vue d’ensemble

Un contrôleur de jeu brut est une représentation générique d’un contrôleur de jeu dont les entrées existent sur différentes sortes de contrôleurs de jeu courants. Ces entrées sont affichées sous forme de simples tableaux de boutons, commutateurs et axes sans nom. En utilisant un contrôleur de jeu brut, vous pouvez permettre aux clients de créer des mappages d’entrée personnalisés, quel que soit le type de contrôleur qu’ils utilisent.

La classe [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) est vraiment conçue pour les scénarios dans lesquels les autres classes d’entrée ([ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) et ainsi de suite) ne répondent pas à vos besoins&mdash;Si vous recherchez un contrôleur plus générique et supposez que les clients utiliseront plusieurs types de contrôleur de jeu, alors cette classe est faite pour vous.

## <a name="detect-and-track-raw-game-controllers"></a>Détecter et suivre les contrôleurs de jeu bruts

La détection et le suivi des contrôleurs de jeu bruts fonctionnent exactement de la même manière que pour les boîtiers de commande, sauf avec la classe [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) au lieu de la classe [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad). Pour en savoir plus, consultez la rubrique [Boîtier de commande et vibration](gamepad-and-vibration.md).

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

The following example copies all connected raw game controllers into a new collection:

```cpp
auto myRawGameControllers = ref new Vector<RawGameController^>();

for (auto rawGameController : RawGameController::RawGameControllers)
{
    // This code assumes that you're interested in all raw game controllers.
    myRawGameControllers->Append(rawGameController);
}
```

### Adding and removing raw game controllers

When a raw game controller is added or removed, the [RawGameControllerAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

The following example starts tracking a raw game controller that's been added:

```cpp
RawGameController::RawGameControllerAdded += 
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    // This code assumes that you're interested in all new raw game controllers.
    myRawGameControllers->Append(args);
});
```

The following example stops tracking a raw game controller that's been removed:

```cpp
RawGameController::RawGameControllerRemoved +=
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    unsigned int indexRemoved;

    if (myRawGameControllers->IndexOf(args, &indexRemoved))
    {
        myRawGameControllers->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each raw game controller can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>Obtenir les fonctionnalités d’un contrôleur de jeu brut

Après avoir identifié le contrôleur de jeu brut qui vous intéresse, vous pouvez rassembler les informations sur ses fonctionnalités. Vous pouvez obtenir le nombre de boutons du contrôleur de jeu brut avec [RawGameController.ButtonCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount), le nombre d’axes analogiques avec [RawGameController.AxisCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount) et le nombre de commutateurs avec [RawGameController.SwitchCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount). En outre, vous pouvez obtenir le type d’un commutateur à l’aide de [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_).

L’exemple de code suivant récupère le nombre d’entrées d’un contrôleur de jeu brut:

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

L’exemple de code suivant détermine le type de chaque commutateur:

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>Lecture du contrôleur de jeu brut

Une fois que vous connaissez le nombre d’entrées d’un contrôleur de jeu brut, vous êtes prêt à les collecter à partir de celui-ci. Toutefois, contrairement à d’autres sortes d’entrées que vous connaissez peut-être, un contrôleur de jeu brut ne communique pas les changements d’état en déclenchant des événements. À la place, vous devez _interroger_ régulièrement un contrôleur de jeu brut pour connaître son état actuel.

### <a name="polling-the-raw-game-controller"></a>Interrogation du contrôleur de jeu brut

Le processus d’interrogation capture un instantané du contrôleur de jeu brut à un moment précis. Cette approche de collecte des entrées convient à la plupart des jeux, dont la logique s’exécute généralement selon une boucle déterministe plutôt que sur la base des événements. Par ailleurs, elle facilite l’interprétation des commandes de jeu, car toutes les entrées sont collectées en même temps, et non les unes après les autres.

Vous pouvez interroger un contrôleur de jeu brut en appelant [RawGameController.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___). Cette fonction renseigne les tableaux de boutons, commutateurs et axes qui contiennent l’état du contrôleur de jeu brut.

L’exemple de code suivant interroge un contrôleur de jeu brut pour connaître son état actuel:

```cpp
Platform::Array<bool>^ currentButtonReading =
    ref new Platform::Array<bool>(buttonCount);

Platform::Array<GameControllerSwitchPosition>^ currentSwitchReading =
    ref new Platform::Array<GameControllerSwitchPosition>(switchCount);

Platform::Array<double>^ currentAxisReading = ref new Platform::Array<double>(axisCount);

rawGameController->GetCurrentReading(
    currentButtonReading,
    currentSwitchReading,
    currentAxisReading);
```

Comme il n’existe aucune garantie sur la position dans chaque tableau qui contient la valeur d’entrée commune aux différents types de contrôleur, vous devez vérifier de quelle entrée il s’agit à l’aide des méthodes [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) et [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_).

**GetButtonLabel** vous indique le texte ou le symbole qui est imprimé sur le bouton physique, plutôt que la fonction du bouton. Cette méthode convient donc mieux comme aide pour l’interface utilisateur pour les cas dans lesquels vous souhaitez donner au joueur des indications sur les fonctions de tel ou tel bouton. **GetSwitchKind** vous indique le type de commutateur (autrement dit, le nombre de positions qu’il compte), mais pas le nom.

Comme aucune méthode standard ne permet d’obtenir l’étiquette d’un axe ou d’un commutateur, vous devez donc tester vous-même les entrées pour identifier l’entrée en question.

Si vous disposez d’un contrôleur spécifique que vous souhaitez prendre en charge, vous pouvez obtenir les méthodes [RawGameController.HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId) et [RawGameController.HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) et vérifier si elles correspondent à ce contrôleur. La position de chaque entrée dans chaque tableau est identique pour tous les contrôleurs ayant les mêmes **HardwareProductId** et **HardwareVendorId**. Vous n’avez donc pas à vous soucier de votre logique potentiellement incohérente entre les différents contrôleurs du même type.

Outre l’état du contrôleur de jeu brut, chaque lecture retourne un horodatage qui indique précisément le moment de récupération de cet état. Cet horodatage est utile pour faire le lien avec le minutage des lectures précédentes ou de la simulation de jeu.

### <a name="reading-the-buttons-and-switches"></a>Lecture des boutons et des commutateurs

Chacun des boutons du contrôleur de jeu brut fournit une lecture numérique qui indique s’il est enfoncé (bas) ou relâché (haut). Les lectures de bouton sont représentées comme des valeurs booléennes individuelles dans un tableau unique. L’étiquette de chaque bouton peut être trouvée à l’aide de [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) avec l’index de la valeur booléenne indiquée dans le tableau. Chaque valeur est représentée en tant que [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel).

L’exemple suivant détermine si le bouton**XboxA** est enfoncé:

```cpp
for (uint32_t i = 0; i < buttonCount; i++)
{
    if (currentButtonReading[i])
    {
        GameControllerButtonLabel buttonLabel = rawGameController->GetButtonLabel(i);

        if (buttonLabel == GameControllerButtonLabel::XboxA)
        {
            // XboxA is pressed.
        }
    }
}
```

Vous pouvez avoir besoin de savoir quand un bouton passe de l’état enfoncé à l’état relâché, ou inversement, si plusieurs boutons sont enfoncés ou relâchés, ou si un groupe de boutons possède une disposition particulière, certains étant enfoncés et d’autres relâchés. Pour plus d’informations sur la détection de chacun de ces états, consultez les sections [Détection de transitions de boutons](input-practices-for-games.md#detecting-button-transitions) et [Détection des dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

Les valeurs des commutateurs sont indiquées sous forme de tableau de [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition). Comme cette propriété est un champ de bits, un masquage au niveau du bit est utilisé pour isoler le sens du commutateur.

L’exemple ci-après détermine si chaque commutateur se trouve en position haute:

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    if (GameControllerSwitchPosition::Up ==
        (currentSwitchReading[i] & GameControllerSwitchPosition::Up))
    {
        // The switch is in the up position.
    }
}
```

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>Lecture des entrées analogiques (sticks, déclencheurs, accélérateurs, etc.)

Un axe analogique fournit une valeur comprise entre 0,0 et 1,0. Cela inclut chaque dimension d’un stick, par exemple X et Y pour des sticks standard, voire des axes X, Y et Z (respectivement cylindre, pas et lacet) pour les manches à balai.

Les valeurs peuvent représenter des déclencheurs analogiques, des accélérateurs ou tout autre type d’entrée analogique. Comme ces valeurs ne sont pas indiquées avec des étiquettes, nous vous suggérons de tester votre code avec une gamme de périphériques d’entrée pour vous assurer qu’elles correspondent à vos hypothèses.

Sur tous les axes, la valeur est proche de0,5 pour un stick quand il se trouve à la position centrale, mais la valeur exacte peut varier d’une lecture à l’autre. Les stratégies de compensation de cette variation sont décrites plus loin, dans cette section.

L’exemple suivant montre comment lire les valeurs analogiques d’un contrôleur Xbox:

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

Si vous examinez les valeurs que les sticks fournissent, vous remarquerez qu’ils ne fournissent pas toujours la valeur neutre0,5 quand ils se trouvent à la position centrale. Ils fournissent différentes valeurs proches de0,5 chaque fois qu’ils sont déplacés, puis remis à la position centrale. Pour compenser ces variations, vous pouvez implémenter une petite _zone morte_, qui correspond à une plage de valeurs proches de la position centrale idéale à ignorer.

Pour implémenter une zone morte, vous pouvez déterminer la distance de déplacement du stick à partir de sa position centrale, et ignorer les valeurs qui sont plus proches que la distance de référence spécifiée. Vous pouvez calculer la distance de manière approximative (elle n’est pas exacte du fait que les lectures des entrées de stick sont essentiellement des valeurs polaires, et non planaires) en utilisant simplement le théorème de Pythagore. Vous obtenez ainsi une zone morte radiale.

L’exemple ci-après illustre une zone morte radiale simple calculée à l’aide du théorème de Pythagore:

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = leftStickY * leftStickY;
float adjacentSquared = leftStickX * leftStickX;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

<!--## Run the RawGameControllerUWP sample

The [RawGameControllerUWP sample (GitHub)](TODO: Link) demonstrates how to use raw game controllers. TODO: More information-->

## <a name="see-also"></a>Articles associés

* [Entrées pour les jeux](input-for-games.md)
* [Pratiques de saisie pour les jeux](input-practices-for-games.md)
* [Espace de noms Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Classe Windows.Gaming.Input.RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)
* [Interface Windows.Gaming.Input.IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Interface Windows.Gaming.Input.IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)