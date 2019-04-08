---
title: Boîtier de commande et vibration
description: Utilisez les API de boîtier de commande Windows.Gaming.Input pour détecter et lire les commandes de vibration et d’impulsion, et les envoyer aux boîtiers de commande.
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.date: 09/06/2018
ms.topic: article
keywords: windows 10, uwp, jeux, boîtier de commande, vibration
ms.localizationpriority: medium
ms.openlocfilehash: e65b22039c381bd333516bd9f98c60bbddb9621c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646924"
---
# <a name="gamepad-and-vibration"></a>Boîtier de commande et vibration

Cet article explique les notions de base de la programmation pour les boîtiers de commande Xbox One avec l’API [Windows.Gaming.Input.Gamepad][gamepad] et les API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article :

* Obtenir une liste des boîtiers de commande connectés et de leurs utilisateurs
* Détecter l’ajout ou la suppression d’un boîtier de commande
* Lire les entrées provenant d’un ou de plusieurs boîtiers de commande
* Envoyer des commandes de vibration et d’impulsion
* comment les boîtiers de commande se comportent comme des périphériques de navigation de l’interface utilisateur

## <a name="gamepad-overview"></a>Vue d’ensemble des boîtiers de commande

Les boîtiers de commande tels que les manettes sans fil Xbox Wireless Controller et Xbox Wireless Controller S sont des périphériques d’entrée de jeu à usage général. Ils représentent le périphérique d’entrée standard sur Xbox One et ils sont couramment choisis par les joueurs Windows qui ne préfèrent pas utiliser un clavier et une souris. Les boîtiers de commande sont pris en charge dans les applications UWP Windows 10 et Xbox par l’espace de noms [Windows.Gaming.Input][].

Les boîtiers de commande Xbox One sont équipés d’un pavé directionnel (ou le pavé directionnel) ; **A**, **B**, **X**, **Y**, **vue**, et **Menu** gauche des boutons ; et droit sticks analogiques, champignons et les déclencheurs ; et un total de quatre moteurs de vibration. Les deux sticks analogiques fournissent des valeurs analogiques doubles sur les axes X et Y, et fonctionnent également comme un bouton quand l’utilisateur appuie dessus vers l’intérieur. Chaque déclencheur fournit une lecture analogique qui représente l’amplitude est collecté dans.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **Paddle** buttons on its underside. These can be used to provide redundant access to game commands that are difficult to use together (such as the right thumbstick together with any of the **A**, **B**, **X**, or **Y** buttons) or to provide dedicated access to additional commands. -->

> [!NOTE]
> `Windows.Gaming.Input.Gamepad` prend également en charge les boîtiers de commande Xbox 360, qui ont la même disposition de contrôle que les boîtiers de commande standard Xbox One.

### <a name="vibration-and-impulse-triggers"></a>Gâchettes de vibration et à impulsions

Les boîtiers de commande Xbox One ont deux moteurs indépendants qui déclenchent des vibrations fortes et légères, ainsi que deux moteurs dédiés qui produisent des vibrations rapides en série au niveau de chaque gâchette (d’où le nom de _gâchette à impulsions_, qui est une fonctionnalité spécifique au boîtier de commande Xbox One).

> [!NOTE]
> Les boîtiers de commande Xbox 360 ne sont pas équipés _les déclencheurs impulsionnelle_.

Pour plus d’informations, consultez [Vue d’ensemble des gâchettes de vibration et à impulsions](#vibration-and-impulse-triggers-overview).

### <a name="thumbstick-deadzones"></a>Zones mortes (ou « dead zones ») des sticks analogiques

Quand un stick analogique se trouve à la position centrale, il devrait idéalement toujours fournir la même valeur neutre sur les axes X et Y. Or, en raison de certaines forces mécaniques et de la sensibilité du stick analogique, les valeurs réelles fournies à la position centrale ne correspondent pas exactement à la valeur neutre idéale et peuvent varier d’une lecture de valeur à une autre. Pour cette raison, vous devez toujours utiliser un petit _deadzone_&mdash;une plage de valeurs qui sont ignorés près de la position centrale idéale&mdash;pour compenser les différences de fabrication, usure mécanique ou autres gamepad problèmes.

L’utilisation de zones mortes plus larges est une solution simple pour faire la distinction entre les entrées intentionnelles et les entrées accidentelles.

Pour plus d’informations, consultez [Lecture des entrées des sticks analogiques](#reading-the-thumbsticks).

### <a name="ui-navigation"></a>Navigation d’interface utilisateur

Pour réduire la difficulté associée à la prise en charge de plusieurs périphériques d’entrée différents lors de la navigation dans l’interface utilisateur, et pour favoriser la cohérence entre les jeux et les périphériques, la plupart des périphériques d’entrée _physiques_ agissent en tant que périphérique d’entrée _logique_ distinct, appelé [contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md). Le contrôleur de navigation d’interface utilisateur fournit un vocabulaire commun pour les commandes de navigation dans l’interface utilisateur, sur plusieurs périphériques d’entrée.

Mappent les boîtiers de commande en tant que contrôleur de navigation de l’interface utilisateur, le [ensemble obligatoire](ui-navigation-controller.md#required-set) des commandes de navigation pour le stick analogique gauche, pavé directionnel, **vue**, **Menu**, **un**, et **B** boutons.

| Commande de navigation | Entrée boîtier de commande                       |
| ------------------:| ----------------------------------- |
|                 Up (Haut) | Stick analogique gauche vers le haut / bouton multidirectionnel vers le haut       |
|               Vers le bas | Stick analogique gauche vers le bas / bouton multidirectionnel vers le bas   |
|               Left (Gauche) | Stick analogique gauche vers la gauche / bouton multidirectionnel vers la gauche   |
|              Droit | Stick analogique gauche vers la droite / bouton multidirectionnel vers la droite |
|               Affichage | Bouton Afficher                         |
|               Menu | Bouton Menu                         |
|             Accept | Bouton A                            |
|             Cancel | Bouton B                            |

De plus, les boîtiers de commande mappent toutes les commandes de navigation de l’[ensemble facultatif](ui-navigation-controller.md#optional-set) aux entrées restantes.

| Commande de navigation | Entrée boîtier de commande          |
| ------------------:| ---------------------- |
|            Page précédente | Gâchette gauche           |
|          Page suivante | Gâchette droite          |
|          Page vers la gauche | Gâchette haute gauche            |
|         Page vers la droite | Gâchette haute droite           |
|          Faire défiler vers le haut | Stick analogique droit vers le haut    |
|        Faire défiler vers le bas | Stick analogique droit vers le bas  |
|        Faire défiler vers la gauche | Stick analogique droit vers la gauche  |
|       Faire défiler vers la droite | Stick analogique droit vers la droite |
|          Contexte 1 | Bouton X               |
|          Contexte 2 | Bouton Y               |
|          Contexte 3 | Appui sur stick analogique gauche  |
|          Contexte 4 | Appui sur stick analogique droit |

## <a name="detect-and-track-gamepads"></a>Détecter et suivre des boîtiers de commande

Comme les boîtiers de commande sont gérés par le système, vous n’avez pas à les créer ni à les initialiser. Le système fournit une liste des boîtiers de commande connectés et des événements de notification d’ajout ou de suppression d’un boîtier de commande.

### <a name="the-gamepads-list"></a>Liste des boîtiers de commande

La classe [Gamepad][] fournit la propriété statique [Gamepads][], qui correspond à une liste en lecture seule de tous les boîtiers de commande qui sont actuellement connectés. Vous intéresser uniquement dans certaines des boîtiers de commande connecté, il est recommandé de maintenir votre propre collection au lieu d’y accéder via le `Gamepads` propriété.

L’exemple de code suivant copie tous les boîtiers de commande connectés dans une nouvelle collection. Étant donné que les autres threads en arrière-plan seront d’accéder à cette collection (dans le [GamepadAdded][] et [GamepadRemoved][] événements), vous devez placer un verrou autour de n’importe quel code qui lit ou les mises à jour le collection.

```cpp
auto myGamepads = ref new Vector<Gamepad^>();
critical_section myLock{};

for (auto gamepad : Gamepad::Gamepads)
{
    // Check if the gamepad is already in myGamepads; if it isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), gamepad);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all gamepads.
        myGamepads->Append(gamepad);
    }
}
```

```cs
private readonly object myLock = new object();
private List<Gamepad> myGamepads = new List<Gamepad>();
private Gamepad mainGamepad;

private void GetGamepads()
{
    lock (myLock)
    {
        foreach (var gamepad in Gamepad.Gamepads)
        {
            // Check if the gamepad is already in myGamepads; if it isn't, add it.
            bool gamepadInList = myGamepads.Contains(gamepad);

            if (!gamepadInList)
            {
                // This code assumes that you're interested in all gamepads.
                myGamepads.Add(gamepad);
            }
        }
    }   
}
```

### <a name="adding-and-removing-gamepads"></a>Ajout et suppression de boîtiers de commande

Quand un boîtier de commande est ajoutée ou supprimée, le [GamepadAdded][] et [GamepadRemoved][] événements sont déclenchés. Vous pouvez inscrire des gestionnaires pour ces deux événements afin d’effectuer le suivi des boîtiers de commande actuellement connectés.

L’exemple de code suivant démarre le suivi d’un boîtier de commande qui a été ajouté.

```cpp
Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all new gamepads.
        myGamepads->Append(args);
    }
}
```

```cs
Gamepad.GamepadAdded += (object sender, Gamepad e) =>
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    lock (myLock)
    {
        bool gamepadInList = myGamepads.Contains(e);

        if (!gamepadInList)
        {
            myGamepads.Add(e);
        }
    }
};
```

L’exemple suivant arrête le suivi un boîtier de commande qui a été supprimée. Vous devez également gérer que se passe-t-il pour les boîtiers de commande que vous effectuez le suivi lorsqu’ils sont supprimés ; par exemple, ce code uniquement suit entrée à partir d’un boîtier de commande et lui affecte simplement `nullptr` lorsqu’il est supprimé. Vous devrez vérifier chaque frame si votre boîtier de commande est actif et la mise à jour le gamepad vous recueillez les informations issues lorsque les contrôleurs sont connectés et déconnectés.

```cpp
Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ myLock };

    if(myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        myGamepads->RemoveAt(indexRemoved);
    }
}
```

```cs
Gamepad.GamepadRemoved += (object sender, Gamepad e) =>
{
    lock (myLock)
    {
        int indexRemoved = myGamepads.IndexOf(e);

        if (indexRemoved > -1)
        {
            if (mainGamepad == myGamepads[indexRemoved])
            {
                mainGamepad = null;
            }

            myGamepads.RemoveAt(indexRemoved);
        }
    }
};
```

Consultez [d’entrée pratiques pour les jeux](input-practices-for-games.md) pour plus d’informations.

### <a name="users-and-headsets"></a>Utilisateurs et casques

Chaque boîtier de commande peut être associé à un compte d’utilisateur pour lier l’identité de l’utilisateur à son jeu, et peut être branché à un casque permettant d’utiliser plus facilement les fonctionnalités de chat vocal et dans le jeu. Pour en savoir plus sur le suivi des utilisateurs et l’utilisation de casques, voir [Suivi des utilisateurs et de leurs périphériques](input-practices-for-games.md#tracking-users-and-their-devices) et [Casques](headset.md).

## <a name="reading-the-gamepad"></a>Lecture des entrées du boîtier de commande

Une fois que vous avez identifié le boîtier de commande qui vous intéresse, vous pouvez commencer à collecter les entrées de ce boîtier. Toutefois, contrairement à d’autres sortes d’entrées que vous connaissez peut-être, les boîtiers de commande ne communiquent pas les changements d’état en déclenchant des événements. À la place, vous devez _interroger_ régulièrement ces boîtiers de commande pour connaître leur état actuel.

### <a name="polling-the-gamepad"></a>Interrogation du boîtier de commande

Le processus d’interrogation capture un instantané du périphérique de navigation à un moment précis. Cette approche de collecte des entrées convient à la plupart des jeux, dont la logique s’exécute généralement selon une boucle déterministe plutôt que sur la base des événements. Par ailleurs, elle facilite l’interprétation des commandes de jeu, car toutes les entrées sont collectées en même temps, et non pas les unes après les autres.

Vous interrogez un boîtier de commande en appelant la fonction [GetCurrentReading][]. Cette fonction renvoie un élément [GamepadReading][] qui contient l’état du boîtier de commande.

L’exemple de code suivant interroge un boîtier de commande pour obtenir son état actuel.

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

```cs
Gamepad gamepad = myGamepads[0];

GamepadReading reading = gamepad.GetCurrentReading();
```

En plus de l’état du boîtier de commande, chaque valeur comprend un horodatage qui indique précisément le moment d’extraction de cet état. Cet horodatage est utile pour faire le lien avec le minutage des valeurs précédentes ou de la simulation de jeu.

### <a name="reading-the-thumbsticks"></a>Lecture des entrées des sticks analogiques

Chaque stick analogique fournit une valeur analogique située entre -1.0 et +1.0 sur les axes X et Y. Sur l’axe X, une valeur de -1.0 correspond à la position de stick analogique la plus à gauche ; une valeur de +1.0 indique la position la plus à droite. Sur l’axe Y, une valeur de -1.0 correspond à la position de stick analogique la plus en bas ; une valeur de +1.0 indique la position la plus en haut. Dans les deux axes, la valeur est environ 0.0 lorsque la clé est dans la position centrale, mais il est normal que la valeur précise faire varier, même entre les lectures ultérieures ; stratégies d’atténuation de cette variation sont présentés plus loin dans cette section.

La valeur de l’axe X du stick analogique gauche est lue à partir de la propriété `LeftThumbstickX` de la structure [GamepadReading][] ; la valeur de l’axe Y est lue à partir de la propriété `LeftThumbstickY`. La valeur de l’axe X du stick analogique droit est lue à partir de la propriété `RightThumbstickX` ; la valeur de l’axe Y est lue à partir de la propriété `RightThumbstickY`.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
float rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
float rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
double rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
double rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

Si vous examinez les valeurs que les sticks analogiques fournissent, vous remarquerez qu’ils ne fournissent pas toujours la valeur neutre 0.0 quand ils se trouvent à la position centrale. Les sticks analogiques fournissent différentes valeurs proches de 0.0 chaque fois qu’ils sont déplacés, puis remis à la position centrale. Pour compenser ces variations, vous pouvez implémenter une petite _zone morte_, qui correspond à une plage de valeurs proches de la position centrale idéale à ignorer. Pour implémenter une zone morte, vous pouvez déterminer de quelle distance le stick analogique a été déplacé à partir de sa position centrale, en ignorant les valeurs qui sont plus proches que la distance de référence spécifiée. Vous pouvez calculer la distance à peu près&mdash;il n’est pas exacte stick analogique relevés étant essentiellement les valeurs polaires, pas planaires,&mdash;en utilisant le théorème de Pythagore simplement. Vous obtenez ainsi une zone morte radiale.

L’exemple suivant illustre une zone morte radiale simple calculée à l’aide du théorème de Pythagore.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const float deadzoneRadius = 0.1;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const double deadzoneRadius = 0.1;
const double deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
double oppositeSquared = leftStickY * leftStickY;
double adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

Chaque stick analogique fonctionne également comme un bouton quand l’utilisateur appuie dessus vers l’intérieur. Pour plus d’informations sur la lecture de cette entrée, consultez [Lecture des entrées des boutons](#reading-the-buttons).

### <a name="reading-the-triggers"></a>Lecture des entrées des gâchettes

Les gâchettes sont représentées par des valeurs à virgule flottante comprises entre 0.0 (gâchette entièrement relâchée) et 1.0 (gâchette entièrement enclenchée). La valeur de la gâchette gauche est lue à partir de la propriété `LeftTrigger` de la structure [GamepadReading][] ; la valeur de la gâchette droite est lue à partir de la propriété `RightTrigger`.

```cpp
float leftTrigger  = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
float rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

```cs
double leftTrigger = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
double rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>Lecture des boutons

Chacun des boutons gamepad&mdash;les quatre directions du pavé directionnel, champignons gauche et droit, appuyez sur le stick analogique gauche et droite, **A**, **B**, **X**, **Y**, **vue**, et **Menu**&mdash;fournit une lecture numérique qui indique s’il a enfoncé () ou publié (haut). Pour plus d’efficacité, les relevés de bouton ne sont pas représentées comme des valeurs booléennes individuelles ; ces derniers sont tous à la place, rassemblés dans un seul champ de bits qui est représenté par le [GamepadButtons][] énumération.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **paddle** buttons on its underside. These buttons are also represented in the `GamepadButtons` enumeration and their values are read in the same way as the standard gamepad buttons. -->

Les valeurs des boutons sont lues à partir de la propriété `Buttons` de la structure [GamepadReading][]. Comme cette propriété est un champ de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est à l’état enfoncé (position basse) lorsque le bit correspondant est défini ; dans le cas contraire, il se trouve à l’état relâché (position haute).

L’exemple suivant détermine si le bouton A est à l’état appuyé.

```cpp
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

```cs
if (GamepadButtons.A == (reading.Buttons & GamepadButtons.A))
{
    // button A is pressed
}
```

L’exemple suivant détermine si le bouton A est à l’état relâché.

```cpp
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released
}
```

```cs
if (GamepadButtons.None == (reading.Buttons & GamepadButtons.A))
{
    // button A is released
}
```

Parfois, vous souhaiterez déterminer lors des transitions à partir d’un bouton appuyé à publiées ou relâché à appuyé, si plusieurs boutons sont appuyés ou relâchés, ou si un ensemble de boutons est organisé de manière particulière&mdash;certaines enfoncé, autres pas. Pour plus d’informations sur la détection de chacun de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-gamepad-input-sample"></a>Exécuter l’exemple d’entrée de boîtier de commande

L’[exemple GamepadUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadUWP) montre comment connecter un boîtier de commande et lire son état.

## <a name="vibration-and-impulse-triggers-overview"></a>Vue d’ensemble des gâchettes de vibration et à impulsions

Les moteurs de vibration intégrés au boîtier de commande sont conçus pour offrir une expérience tactile au joueur. Les jeux utilisent cette fonctionnalité pour renforcer la sensation d’immersion, mieux communiquer les informations d’état (par exemple, les dommages subis), signaler la proximité d’objets importants, ou pour d’autres usages créatifs.

Les boîtiers de commande Xbox One comportent au total quatre moteurs de vibration indépendants. Les deux sont des moteurs de grande taille situés dans le corps de gamepad ; le moteur à gauche fournit vibration approximative, grande amplitude, tandis que le moteur de droite fournit vibration crée et plus subtile. Les deux autres moteurs sont des petits moteurs intégrés à chaque gâchette. Ils déclenchent des vibrations rapides en série qui sont directement ressenties par le joueur qui manipulent les gâchettes (d’où le nom de _gâchette à impulsions_, qui est une fonctionnalité spécifique au boîtier de commande Xbox One). L’utilisation combinée de ces quatre moteurs permet de recréer une large palette de sensations tactiles.

## <a name="using-vibration-and-impulse"></a>Utilisation des vibrations et des impulsions

Les vibrations du boîtier de commande sont contrôlées par la propriété [Vibration][] de la classe [Gamepad][]. `Vibration` est une instance de la [GamepadVibration][] structure qui se compose de quatre flottante des valeurs de points ; chaque valeur représente l’intensité d’un des moteurs.

Bien que les membres de la `Gamepad.Vibration` propriété peut être modifiée directement, il est recommandé que vous initialisez un distinct `GamepadVibration` instance aux valeurs de votre choix, puis copiez-le dans le `Gamepad.Vibration` propriété à modifier l’intensité motor réelle tous en même temps.

L’exemple suivant montre comment modifier simultanément toutes les intensités de moteur.

```cpp
// get the first gamepad
Gamepad^ gamepad = Gamepad::Gamepads->GetAt(0);

// create an instance of GamepadVibration
GamepadVibration vibration;

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

```cs
// get the first gamepad
Gamepad gamepad = Gamepad.Gamepads[0];

// create an instance of GamepadVibration
GamepadVibration vibration = new GamepadVibration();

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

### <a name="using-the-vibration-motors"></a>Utilisation des moteurs de vibration

Les moteurs de vibration gauche et droite acceptent des valeurs à virgule flottante comprises entre 0.0 (aucune vibration) et 1.0 (intensité de vibration la plus élevée). L’intensité du moteur gauche est définie par la propriété `LeftMotor` de la structure [GamepadVibration][] ; celle du moteur droit est définie par la propriété `RightMotor`.

L’exemple suivant définit l’intensité des deux moteurs de vibration et active la vibration du boîtier de commande.

```cpp
GamepadVibration vibration;
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
mainGamepad.Vibration = vibration;
```

N’oubliez pas que ces deux moteurs ne sont pas identiques. La définition de leurs propriétés à la même valeur ne produit donc pas la même vibration dans ces moteurs. Pour n’importe quelle valeur, le moteur à gauche produit une vibration selon une périodicité moindre plus forte que le droit d’automobile qui&mdash;pour la même valeur&mdash;génère une vibration crée à une fréquence plus élevée. Même avec une valeur maximale, le moteur gauche ne peut pas produire une vibration à des fréquences aussi élevées que le moteur droit, et le moteur droit ne peut pas produire une vibration aussi forte que le moteur gauche. Toutefois, du fait que les deux moteurs sont connectés physiquement par le même boîtier de commande, les joueurs ne ressentent pas les vibrations de chaque moteur de manière totalement indépendante, et ce même si les moteurs présentent des caractéristiques distinctes et des intensités de vibration différentes. Cette disposition permet d’offrir aux joueurs une plus grande palette de sensations tactiles qu’avec deux moteurs identiques.

### <a name="using-the-impulse-triggers"></a>Utilisation des gâchettes à impulsions

Chaque moteur de gâchette à impulsions accepte une valeur à virgule flottante comprise entre 0.0 (aucune vibration) et 1.0 (intensité de vibration la plus élevée). L’intensité du moteur de la gâchette gauche est définie par la propriété `LeftTrigger` de la structure [GamepadVibration][] ; celle du moteur de la gâchette droite est définie par la propriété `RightTrigger`.

L’exemple suivant définit l’intensité des deux moteurs des gâchettes à impulsions, et les active.

```cpp
GamepadVibration vibration;
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
mainGamepad.Vibration = vibration;
```

Contrairement aux autres moteurs, les deux moteurs de vibration intégrés aux gâchettes sont identiques et fournissent chacun la même vibration pour une valeur identique. Toutefois, du fait que ces moteurs ne sont pas connectés physiquement, les joueurs ressentent les vibrations de chaque moteur de manière indépendante. Cette disposition permet au joueur de ressentir simultanément des sensations distinctes au niveau des deux gâchettes, tout en facilitant la communication d’informations plus précises que les moteurs installés physiquement dans le boîtier de commande.

## <a name="run-the-gamepad-vibration-sample"></a>Exécuter l’exemple de vibration du boîtier de commande

L’[exemple GamepadVibrationUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadVibrationUWP) montre comment utiliser les moteurs de vibration et les gâchettes à impulsions du boîtier de commande pour produire des effets variés.

## <a name="see-also"></a>Voir également

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Pratiques d’entrée pour les jeux](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[Gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[Gamepads]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepads.aspx
[gamepadadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadadded.aspx
[gamepadremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.getcurrentreading.aspx
[vibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.vibration.aspx
[gamepadreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadreading.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
[gamepadvibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadvibration.aspx
