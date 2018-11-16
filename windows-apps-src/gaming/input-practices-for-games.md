---
author: eliotcowley
title: Pratiques d’entrée pour les jeux
description: Découvrez les modèles et techniques pour utiliser efficacement les périphériques d’entrée.
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.author: elcowle
ms.date: 11/20/2017
ms.topic: article
keywords: windows 10, uwp, jeux, entrée
ms.localizationpriority: medium
ms.openlocfilehash: ed0d611c761315e42decb89e1a5a5ad84f4b067a
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6843515"
---
# <a name="input-practices-for-games"></a>Pratiques d’entrée pour les jeux

Cette page décrit les modèles et techniques pour utiliser efficacement les périphériques d’entrée dans les jeux de plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cette page:

* Comment suivre les joueurs et les périphériques d’entrée et de navigation qu’ils utilisent
* Comment détecter les transitions de bouton (appuyé à relâché, relâché à appuyé)
* Comment détecter les dispositions de boutons complexes à l’aide d’un seul et même test

## <a name="choosing-an-input-device-class"></a>Choix d’une classe de périphérique d’entrée

Vous disposez d’une multitude de types d’API d’entrée, comme [ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) et [Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad). Comment choisir l’API à utiliser pour votre jeu?

Vous devez déterminer l’API qui offre l’entrée la mieux adaptée à votre jeu. Par exemple, si vous créez un jeu pour plateforme 2D, vous pouvez probablement vous contenter d’utiliser la classe **Gamepad** sans avoir à vous soucier des fonctionnalités supplémentaires offertes par les autres classes. Cette approche impose uniquement au jeu la prise en charge des boîtiers de commande et fournit une interface cohérente qui fonctionnera avec de nombreux types de boîtiers de commande sans nécessiter de code supplémentaire.

En revanche, dans le cas de simulations de courses ou de pilotage complexes, vous aurez besoin d’énumérer tous les objets [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) en guise de données de base pour vous assurer qu’ils prennent en charge tous les appareils de niche susceptibles d’être utilisés par les joueurs passionnés, y compris les périphériques tels que les pédales ou les contrôles d’accélération séparés qui sont encore utilisés pour les jeux à un seul joueur. 

À ce stade, vous pouvez utiliser la méthode **FromGameController** d’une classe d’entrée, telle que [Gamepad.FromGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.fromgamecontroller), pour déterminer s’il existe une vue mieux adaptée à chaque périphérique. Par exemple, si le périphérique est également un **boîtier de commande**, vous pouvez ajuster l’interface utilisateur de mappage de touches en conséquence et offrir une sélection de mappages de touches par défaut judicieuse. (Cette approche se distingue avantageusement de celle qui consiste à utiliser uniquement des objets **RawGameController** et qui oblige le joueur à configurer manuellement les entrées du boîtier de commande.) 

Une autre possibilité consiste à rechercher l’ID du fournisseur (VID) et l’ID produit (PID) d’un objet **RawGameController** (à l’aide des propriétés [HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) et [HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId), respectivement) et à fournir des suggestions de mappages de touches pour les périphériques les plus répandus, tout en assurant la compatibilité avec les périphériques inconnus qui feront leur apparition par la suite au moyen de mappages manuels effectués par le joueur.

## <a name="keeping-track-of-connected-controllers"></a>Suivi des contrôleurs connectés

Même si chaque type de contrôleur comprend une liste des contrôleurs connectés (comme [Gamepad.Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.Gamepads)), il est judicieux de maintenir votre propre liste de contrôleurs. Voir [Liste des boîtiers de commande](gamepad-and-vibration.md#the-gamepads-list) pour plus d’informations (chaque type de contrôleur comporte une section portant le même nom dans sa propre rubrique).

Toutefois, que se passe-t-il lorsque le joueur débranche son contrôleur ou en branche un autre? Vous devez gérer ces événements et mettre à jour votre liste en conséquence. Voir [Ajout et suppression de boîtiers de commande](gamepad-and-vibration.md#adding-and-removing-gamepads) pour plus d’informations (de même, chaque type de contrôleur comporte une section portant le même nom dans sa propre rubrique).

Étant donné que les événements ajoutés et supprimés sont déclenchés de façon asynchrone, vous pouvez obtenir des résultats incorrects lors du traitement de votre liste des contrôleurs. Par conséquent, chaque fois que vous accédez à votre liste de contrôleurs, vous devez la verrouiller afin qu’un seul thread puisse y accéder à la fois. Cette opération peut être effectuée avec le [Runtime d’accès concurrentiel](https://docs.microsoft.com/cpp/parallel/concrt/concurrency-runtime), en particulier la [classe critical_section](https://docs.microsoft.com/cpp/parallel/concrt/reference/critical-section-class), dans **&lt;ppl.h&gt;**.

Une autre chose à se rappeler est que la liste des contrôleurs connectés sera initialement vide. Elle prendra une ou deux secondes pour se remplir. Donc, si vous affectez uniquement le boîtier de commande en cours dans la méthode start, elle sera **null **!

Pour résoudre ce problème, vous devez avoir une méthode qui «actualise» le boîtier de commande principal (dans un jeu à un seul joueur; les jeux multijoueurs nécessitent des solutions plus sophistiquées). Vous devez ensuite appeler cette méthode à la fois dans votre gestionnaire d'événement de contrôleur ajouté et de contrôleur supprimé, ou dans votre méthode de mise à jour.

La méthode suivante retourne simplement le premier boîtier de commande de la liste (ou **nullptr** si la liste est vide). Vous devez simplement penser à vérifier les **nullptr** chaque fois que vous faite une action quelconque avec le contrôleur. Vous pouvez choisir de bloquer l’expérience de jeu lorsqu’il n'y a aucun contrôleur connecté (par exemple, en mettant le jeu en pause), ou de la laisser continuer, tout en ignorant les entrées.

```cpp
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Gaming::Input;
using namespace concurrency;

Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();

Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}
```

Pour voir une vue d’ensemble, voici un exemple illustrant comment gérer les entrées à partir d’un boîtier de commande:

```cpp
#include <algorithm>
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Foundation;
using namespace Windows::Gaming::Input;
using namespace concurrency;

static Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();
static Gamepad^          m_gamepad = nullptr;
static critical_section  m_lock{};

void Start()
{
    // Register for gamepad added and removed events.
    Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(&OnGamepadAdded);
    Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(&OnGamepadRemoved);

    // Add connected gamepads to m_myGamepads.
    for (auto gamepad : Gamepad::Gamepads)
    {
        OnGamepadAdded(nullptr, gamepad);
    }
}

void Update()
{
    // Update the current gamepad if necessary.
    if (m_gamepad == nullptr)
    {
        auto gamepad = GetFirstGamepad();

        if (m_gamepad != gamepad)
        {
            m_gamepad = gamepad;
        }
    }

    if (m_gamepad != nullptr)
    {
        // Gather gamepad reading.
    }
}

// Get the first gamepad in the list.
Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}

void OnGamepadAdded(Platform::Object^ sender, Gamepad^ args)
{
    // Check if the just-added gamepad is already in m_myGamepads; if it isn't, 
    // add it.
    critical_section::scoped_lock lock{ m_lock };
    auto it = std::find(begin(m_myGamepads), end(m_myGamepads), args);

    if (it == end(m_myGamepads))
    {
        m_myGamepads->Append(args);
    }
}

void OnGamepadRemoved(Platform::Object^ sender, Gamepad^ args)
{
    // Remove the gamepad that was just disconnected from m_myGamepads.
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ m_lock };

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == m_myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        m_myGamepads->RemoveAt(indexRemoved);
    }
}
```

## <a name="tracking-users-and-their-devices"></a>Suivi des utilisateurs et de leurs périphériques

Tous les périphériques d’entrée sont associés à un [utilisateur](https://docs.microsoft.com/uwp/api/windows.system.user) afin que son identité puisse être liée à sa séquence de jeu, ses succès, ses modifications de paramètres et ses autres activités. Les utilisateurs peuvent se connecter ou se déconnecter à volonté, et il est courant qu’un utilisateur différent se connecte à un périphérique d’entrée qui reste connecté au système après la déconnexion de l’utilisateur précédent. À la connexion ou déconnexion d’un utilisateur, l’événement [IGameController.UserChanged](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.UserChanged) est déclenché. Vous pouvez inscrire un gestionnaire d’événements pour cet événement afin d’effectuer le suivi des joueurs et des périphériques qu’ils utilisent.

Une identité d’utilisateur est également le moyen par lequel un périphérique d’entrée est associé au [contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md) qui lui correspond.

C’est pourquoi il convient de suivre les entrées de joueur et de les mettre en corrélation à l’aide de la propriété [User](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.User) de la classe de périphérique (héritée de l’interface [IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)).

L’exemple [UserGamepadPairingUWP (GitHub)](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) illustre le suivi d’utilisateurs et de leurs périphériques.

## <a name="detecting-button-transitions"></a>Détection de transitions de boutons

Vous souhaiterez savoir parfois quand un bouton est d’abord enfoncé ou relâché, autrement dit lorsque l’état du bouton passe de relâché à appuyé, ou inversement. Pour le déterminer, vous devez mémoriser la lecture précédente du périphérique et la comparer à la lecture actuelle pour voir ce qui a changé.

L’exemple ci-après illustre une approche de base pour mémoriser la lecture précédente; des boîtiers de commande sont affichés ici, mais les principes sont les mêmes pour les sticks analogiques Arcade, les volants de course et les autres types de périphériques d’entrée.

```cpp
Gamepad gamepad;
GamepadReading newReading();
GamepadReading oldReading();

// Called at the start of the game.
void Game::Start()
{
    gamepad = Gamepad::Gamepads[0];
}

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.GetCurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased (see below)
}
```

Avant toute autre action, `Game::Loop` déplace la valeur existante de `newReading` (lecture du boîtier de commande de l’itération de boucle précédente) dans `oldReading`, puis renseigne `newReading` avec une nouvelle lecture du boîtier de commande correspondant à l’itération actuelle. Vous disposez alors des informations nécessaires pour détecter les transitions de boutons.

L’exemple ci-près illustre une approche de base pour détecter les transitions de boutons:

```cpp
bool ButtonJustPressed(const GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased =
        (GamepadButtons.None == (newReading.Buttons & selection));

    bool oldSelectionReleased =
        (GamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Ces deuxfonctions commencent par déduire l’état booléen de la sélection de bouton de `newReading` et `oldReading`, puis appliquent une logique booléenne pour déterminer si la transition cible s’est produite. Ces fonctions retournent **true** uniquement si la nouvelle lecture contient l’état cible (appuyé ou relâché, respectivement) *et* si l’ancienne lecture ne contient pas également l’état cible. Dans le cas contraire, elles retournent **false**.

## <a name="detecting-complex-button-arrangements"></a>Détection des dispositions de boutons complexes

Chaque bouton d’un périphérique d’entrée fournit une lecture numérique qui indique s’il est à l’état enfoncé (position basse) ou relâché (position haute). Pour plus d’efficacité, les entrées de bouton ne sont pas représentées individuellement sous forme de valeurs booléennes. Elles sont toutes regroupées dans des champs de bits représentés par des énumérations propres aux périphériques, par exemple [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons). Pour lire des boutons spécifiques, un masquage au niveau du bit est effectué pour isoler les valeurs qui vous intéressent. Un bouton est à l’état enfoncé (position basse) lorsque le bit correspondant est défini; dans le cas contraire, il se trouve à l’état relâché (position haute).

Souvenez-vous comment déterminer que les boutons uniques sont enfoncés ou relâchés; des boîtiers de commande sont affichés ici, mais les principes sont les mêmes pour les sticks analogiques Arcade, les volants de course et les autres types de périphériques d’entrée.

```cpp
GamepadReading reading = gamepad.GetCurrentReading();

// Determines whether gamepad button A is pressed.
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // The A button is pressed.
}

// Determines whether gamepad button A is released.
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // The A button is released (not pressed).
}
```

Comme vous le constatez, la détermination de l’état d’un bouton unique est simple, mais vous souhaiterez peut-être parfois savoir si plusieurs boutons sont enfoncés ou relâchés, ou si un groupe de boutons possède une disposition particulière, certains étant enfoncés et d’autres relâchés. Tester plusieurs boutons est plus complexe que tester des boutons uniques, notamment avec le potentiel de l’état de bouton mixte, mais il existe une formule simple qui s’applique indifféremment aux tests des boutons uniques et multiples.

L’exemple ci-après détermine si les boutonsA et B du boîtier de commande sont tous les deux enfoncés:

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

L’exemple ci-après détermine si les boutons A et B du boîtier de commande sont tous les deux relâchés:

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

L’exemple ci-après détermine si le boutonA du boîtier de commande est enfoncé tandis que le boutonB est relâché:

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Dans la formule que ces cinqexemples ont en commun, la disposition des boutons à tester est spécifiée par l’expression située à gauche de l’opérateur d’égalité tandis que les boutons à examiner sont sélectionnés par l’expression de masquage à droite.

L’exemple ci-après présente cette formule plus clairement en réécrivant l’exemple précédent:

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Cette formule peut être appliquée pour tester n’importe quel nombre de boutons, avec toutes les dispositions de leur état.

## <a name="get-the-state-of-the-battery"></a>Obtenir l’état de la batterie

Pour n’importe quel contrôleur de jeu qui implémente l'interface [IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo), vous pouvez appeler [TryGetBatteryReport](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport) sur l’instance de contrôleur pour obtenir un objet [BatteryReport](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport) qui fournit des informations sur la batterie dans le contrôleur. Vous pouvez obtenir des propriétés telles que la vitesse de charge de la batterie ([ChargeRateInMilliwatts](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts)), la capacité énergétique estimée d'une batterie neuve ([DesignCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours)) et la capacité énergétique de la batterie actuelle complètement chargée ([FullChargeCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours)).

Pour les contrôleurs de jeu qui prennent en charge la création de rapports détaillés sur la batterie, vous pouvez obtenir ces informations et d'autres sur la batterie, comme expliqué dans la section [Obtenir des informations sur la batterie](../devices-sensors/get-battery-info.md). Toutefois, la plupart des contrôleurs de jeu ne prennent pas en charge ce niveau de rapport sur la batterie et utilisent plutôt un matériel moins coûteux. Pour ces contrôleurs, vous devez garder à l'esprit les considérations suivantes:

* **ChargeRateInMilliwatts** et **DesignCapacityInMilliwattHours** seront toujours **NULL**.

* Vous pouvez obtenir le pourcentage de batterie en calculant [RemainingCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours) / **FullChargeCapacityInMilliwattHours**. Vous devez ignorer les valeurs de ces propriétés et ne traiter que le pourcentage calculé.

* Le pourcentage évoqué au paragraphe précédent sera toujours l'un des suivants:

    * 100% (Complète)
    * 70% (Moyenne)
    * 40% (Faible)
    * 10% (Critique)

Si votre code exécute une action (comme étendre une IU) en fonction du pourcentage restant d'autonomie de la batterie, assurez-vous qu’il se conforme aux valeurs ci-dessus. Par exemple, si vous souhaitez avertir le joueur lorsque la batterie du contrôleur est faible, faites-le lorsque son niveau atteint 10%.

## <a name="see-also"></a>Articles associés

* [Classe Windows.System.User](https://docs.microsoft.com/uwp/api/windows.system.user)
* [Interface Windows.Gaming.Input.IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Énumération Windows.Gaming.Input.GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)
