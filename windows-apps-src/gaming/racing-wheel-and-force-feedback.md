---
author: eliotcowley
title: Volant de course et retour de force
description: Utilisez les API de volant de course Windows.Gaming.Input pour détecter, déterminer les fonctionnalités, lire et envoyer des commandes de retour de force aux volants de course.
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.author: wdg-dev-content
ms.date: 05/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, jeux, volant de course, retour de force
ms.localizationpriority: medium
ms.openlocfilehash: bab652322e1a744dc82ce318b3be0f4a4a9d63ec
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874357"
---
# <a name="racing-wheel-and-force-feedback"></a>Volant de course et retour de force

Cet article explique les notions de base de la programmation pour les volants de course Xbox One avec l’API [Windows.Gaming.Input.RacingWheel][racingwheel] et les API associées pour la plateforme Windows universelle (UWP).

Voici les procédures que vous allez découvrir à la lecture de cet article:

* Obtenir une liste des volants de course connectés et de leurs utilisateurs
* Détecter l’ajout ou la suppression d’un volant de course
* Lire les entrées provenant d’un ou de plusieurs volants de course
* Envoyer des commandes de retour de force
* Comprendre le comportement des volants de course utilisés en tant que périphériques de navigation d’interface utilisateur

## <a name="racing-wheel-overview"></a>Volant de course: présentation

Un volant de course est un périphérique d’entrée dont l’utilisation rappelle celle du volant d’une voiture de course réelle. Les volants de course sont idéaux pour les jeux de course de type arcade ou simulation, qui mettent en scène des voitures ou des camions. Ils sont pris en charge dans les applications UWP Windows 10 et Xbox One, via l’espace de noms [Windows.Gaming.Input][].

Il existe différentes gammes de prix pour les volants de course Xbox One. Les plus chers présentent généralement des fonctionnalités de retour de force et d’entrée plus performantes et en plus grand nombre. Tous les volants de course sont dotés d’un volant analogique, de contrôles de freinage et d’accélération, ainsi que des boutons. Certains volants de course sont également dotés de contrôles d’embrayage, de frein à main, de levier de vitesse et de retour de force. Cependant, les volants de course ne sont pas tous équipés du même ensemble de fonctionnalités. De plus, il se peut qu’ils ne prennent pas tous en charge les mêmes fonctions&mdash;par exemple, certains volants peuvent accepter des plages différentes de rotation et les leviers de vitesses peuvent gérer différents nombres de vitesses.

### <a name="device-capabilities"></a>Fonctionnalités de l’appareil

Différents volants de course Xbox One proposent un ensemble de fonctionnalités de périphérique en option et des niveaux divers de prise en charge de ces fonctionnalités. Ce degré de variation entre un type de périphérique d’entrée est unique pour tous les périphériques pris en charge par les API [Windows.Gaming.Input][]. De plus, la plupart des périphériques que vous gérerez prendront en charge au moins quelques fonctionnalités ou autres variantes en option. De ce fait, il est important de déterminer les fonctionnalités de chaque volant de course connecté et d’assurer la prise en charge de la variante complète des fonctions adaptées à votre jeu.

Pour en savoir plus, voir [Identification des fonctionnalités de volant de course](#determining-racing-wheel-capabilities).

### <a name="force-feedback"></a>Retour de force

Certains volants de course Xbox One proposent un retour de force réel&mdash; ils peuvent appliquer des forces réelles sur un axe de contrôle, comme un volant&mdash; et non une simple vibration. Les jeux utilisent cette fonctionnalité pour renforcer la sensation d’immersion (_dégâts d’un accident simulé_, «sensation de conduite») et accroître les difficultés associées à la conduite.

Pour en savoir plus, voir [Présentation du retour de force](#force-feedback-overview).

### <a name="ui-navigation"></a>Navigation d’interface utilisateur

Pour réduire la difficulté associée à la prise en charge de plusieurs périphériques d’entrée différents lors de la navigation dans l’interface utilisateur, et pour favoriser la cohérence entre les jeux et les périphériques, la plupart des périphériques d’entrée _physiques_ agissent en tant que périphérique d’entrée _logique_ distinct, appelé [contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md). Le contrôleur de navigation d’interface utilisateur fournit un vocabulaire commun pour les commandes de navigation dans l’interface utilisateur, sur plusieurs périphériques d’entrée.

Comme ils se concentrent tout particulièrement sur les contrôles analogiques et le degré de variante entre les différents volants de course, ils sont généralement dotés d’un bouton multidirectionnel et de boutons **Afficher**, **Menu**, **A**, **B**, **X** et **Y** qui ressemblent à ceux d’un [boîtier de commande](gamepad-and-vibration.md); ces boutons, qui n’ont pas pour but de prendre en charge les commandes de jeu, ne peuvent pas être facilement utilisés en tant que boutons de volant de course.

En tant que contrôleurs de navigation dans l’interface utilisateur, les volants de course incluent l’[ensemble requis](ui-navigation-controller.md#required-set) de commandes de navigation, placées dans le stick analogique gauche, le bouton multidirectionnel et les boutons **Afficher**, **Menu**, **A** et **B**.

| Commande de navigation | Entrée volant de course |
| ------------------:| ------------------ |
|                 Haut | Bouton multidirectionnel vers le haut           |
|               Bas | Bouton multidirectionnel vers le bas         |
|               Gauche | Bouton multidirectionnel vers la gauche         |
|              Droit | Bouton multidirectionnel vers la droite        |
|               View (Afficher) | Bouton Afficher        |
|               Menu | Bouton Menu        |
|             Accept (Accepter) | BoutonA           |
|             Annuler | Bouton B           |

De plus, certains volants de course peuvent mapper certaines commandes d’un [ensemble de commandes de navigation en option](ui-navigation-controller.md#optional-set) à d’autres entrées prises en charge, mais ces mappages peuvent varier d’un appareil à l’autre. Envisagez également la prise en charge de ces commandes, en vous assurant qu’elles ne sont pas essentielles à la navigation au sein de l’interface de votre jeu.

| Commande de navigation | Entrée volant de course    |
| ------------------:| --------------------- |
|            Page précédente | _Dépendant du jeu_              |
|          Page suivante | _Dépendant du jeu_              |
|          Page vers la gauche | _Dépendant du jeu_              |
|         Page vers la droite | _Dépendant du jeu_              |
|          Faire défiler vers le haut | _Dépendant du jeu_              |
|        Faire défiler vers le bas | _Dépendant du jeu_              |
|        Faire défiler vers la gauche | _Dépendant du jeu_              |
|       Faire défiler vers la droite | _Dépendant du jeu_              |
|          Contexte1 | Bouton X (_en général_) |
|          Contexte2 | Bouton Y (_en général_) |
|          Contexte3 | _Dépendant du jeu_              |
|          Contexte4 | _Dépendant du jeu_              |

## <a name="detect-and-track-racing-wheels"></a>Détecter et effectuer le suivi des volants de course

La détection et le suivi des volants de course fonctionnent exactement de la même manière que pour les boîtiers de commande, sauf avec la classe [RacingWheel][] au lieu de la classe [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad). Pour en savoir plus, consultez la rubrique [Boîtier de commande et vibration](gamepad-and-vibration.md).

<!-- Racing wheels are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected racing wheels and events to notify you when a racing wheel is added or removed.

### The racing wheels list

The [RacingWheel][] class provides a static property, [RacingWheels][], which is a read-only list of racing wheels that are currently connected. Because you might only be interested in some of the connected racing wheels, it's recommended that you maintain your own collection instead of accessing them through the `RacingWheels` property.

The following example copies all connected racing wheels into a new collection.
```cpp
auto myRacingWheels = ref new Vector<RacingWheel^>();

for (auto racingwheel : RacingWheel::RacingWheels)
{
    // This code assumes that you're interested in all racing wheels.
    myRacingWheels->Append(racingwheel);
}
```

### Adding and removing racing wheels

When a racing wheel is added or removed the [RacingWheelAdded][] and [RacingWheelRemoved][] events are raised. You can register handlers for these events to keep track of the racing wheels that are currently connected.

The following example starts tracking an racing wheels that's been added.
```cpp
RacingWheel::RacingWheelAdded += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    // This code assumes that you're interested in all new racing wheels.
    myRacingWheels->Append(args);
}
```

The following example stops tracking a racing wheel that's been removed.
```cpp
RacingWheel::RacingWheelRemoved += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    unsigned int indexRemoved;

    if(myRacingWheels->IndexOf(args, &indexRemoved))
    {
        myRacingWheels->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each racing wheel can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-racing-wheel"></a>Lecture des données du volant de course

Une fois que vous avez identifié le volant de course qui vous intéresse, vous pouvez commencer à collecter les entrées correspondantes. Toutefois, contrairement à d’autres sortes d’entrées que vous connaissez peut-être, les volants de course ne communiquent pas les changements d’état en déclenchant des événements. À la place, vous devez les _interroger_ régulièrement pour connaître leur état actuel.

### <a name="polling-the-racing-wheel"></a>Interrogation du volant de course

Le processus d’interrogation capture un instantané du volant de course à un moment précis. Cette approche de collecte des entrées convient à la plupart des jeux, dont la logique s’exécute généralement selon une boucle déterministe plutôt que sur la base des événements. Par ailleurs, elle facilite l’interprétation des commandes de jeu, car toutes les entrées sont collectées en même temps, et non pas les unes après les autres.

Vous interrogez un volant de course en appelant la fonction [GetCurrentReading][]. Cette fonction renvoie un élément [RacingWheelReading][] qui contient l’état du volant de course.

L’exemple de code suivant interroge un volant de course pour obtenir son état actuel.

```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

En plus de l’état du volant de course, chaque valeur comprend un horodatage qui indique précisément le moment d’extraction de cet état. Cet horodatage est utile pour faire le lien avec le minutage des valeurs précédentes ou de la simulation de jeu.

### <a name="determining-racing-wheel-capabilities"></a>Identification des fonctionnalités de volant de course

La plupart des contrôles du volant de course sont facultatifs ou prennent en charge des variantes, même dans les contrôles requis; vous devez donc déterminer les fonctionnalités de chaque volant de course avant de pouvoir traiter les entrées regroupées dans chaque valeur du volant de course.

Les contrôles en option sont les suivants: frein à main, embrayage et leviers de vitesses; vous pouvez déterminer si un volant de course connecté gère ces contrôles en lisant les propriétés [HasHandbrake][], [HasClutch][] et [HasPatternShifter][] du volant de course, respectivement. Ce contrôle est pris en charge si la valeur de la propriété est **true**; dans le cas contraire, elle n’est pas gérée.

```cpp
if (racingwheel->HasHandbrake)
{
    // the handbrake is supported
}

if (racingwheel->HasClutch)
{
    // the clutch is supported
}

if (racingwheel->HasPatternShifter)
{
    // the pattern shifter is supported
}
```

De plus, les contrôles susceptibles de varier sont le volant et le levier de vitesses. La variation du volant peut correspondre au degré de rotation physique qu’un volant réel peut prendre en charge, alors que celle du levier de vitesses peut correspondre au nombre de vitesses avant qu’il gère. Vous pouvez déterminer l’angle de rotation le plus étendu pris en charge par le volant réel en lisant la propriété `MaxWheelAngle` du volant de course; cette valeur correspond à l’angle physique maximal pris en charge, en degrés, dans le sens des aiguilles d’une montre (positif), également géré dans le sens contraire (négatif). Vous pouvez déterminer la vitesse avant la plus grande que le levier de vitesses prend en charge en lisant la propriété `MaxPatternShifterGear` du volant de course; cette valeur correspond à la vitesse avant gérée la plus élevée (incluse)&mdash; si la valeur est de4, alors le levier de vitesses gère la marche arrière, le point mort, mais également la première, la seconde, la troisième et la quatrième vitesse.

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

Pour finir, certains volants de course prennent en charge le retour de force. Vous pouvez déterminer si un volant connecté gère le retour de force en lisant la propriété [WheelMotor][] associée au volant. Ce retour est géré si la valeur de la propriété `WheelMotor` est différente de **null**. Une valeur null indique qu’il n’est pas géré.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

Pour en savoir plus sur l’utilisation de la fonction de retour de force des volants de course qui la prennent en charge, voir [Présentation du retour de force](#force-feedback-overview).

### <a name="reading-the-buttons"></a>Lecture des boutons

Chaque bouton du volant de course &mdash; quatre directions du bouton directionnel, boutons **Vitesse précédente** et **Vitesse suivante** et 16boutons supplémentaires &mdash; fournit des entrées numériques, qui signalent s’il est enclenché (vers le bas) ou relâché (vers le haut). Pour plus d’efficacité, les entrées de bouton ne sont pas représentées individuellement sous forme de valeurs booléennes. Elles sont toutes regroupées dans un seul champ de bits représenté par l’énumération [RacingWheelButtons][].

> [!NOTE]
> Les volants de course sont dotés de boutons supplémentaires, utilisés pour la navigation dans l’interface utilisateur, par exemple des boutons **Afficher** et **Menu**. Ces boutons ne font pas partie de l’énumération `RacingWheelButtons`; vous ne pouvez lire leurs valeurs qu’en accédant au volant de navigation en tant que périphérique de navigation dans l'interface utilisateur. Pour en savoir plus, voir [Périphérique de navigation d’interface utilisateur](ui-navigation-controller.md).

Les valeurs des boutons sont lues à partir de la propriété `Buttons` de la structure [RacingWheelReading][]. Comme cette propriété est un champ de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est à l’état enfoncé (position basse) lorsque le bit correspondant est défini; dans le cas contraire, il se trouve à l’état relâché (position haute).

L’exemple suivant détermine si le bouton **Vitesse suivante** est à l’état appuyé.

```cpp
if (RacingWheelButtons::NextGear == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is pressed
}
```

L’exemple suivant détermine si le bouton Vitesse suivante est à l’état relâché.

```cpp
if (RacingWheelButtons::None == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is released (not pressed)
}
```

Vous pouvez avoir besoin de savoir quand un bouton passe de l’état enfoncé à l’état relâché, ou inversement, si plusieurs boutons sont enfoncés ou relâchés, ou si un groupe de boutons possède une disposition particulière &mdash; certains étant enfoncés et d’autres relâchés. Pour plus d’informations sur la détection de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

### <a name="reading-the-wheel"></a>Lecture des données du volant

Le volant est un contrôle requis, qui fournit une valeur analogique située entre -1.0 et +1.0. Une valeur de -1.0 correspond à la position de volant la plus à gauche; une valeur de +1.0 indique la position la plus à droite. La valeur du volant est lue à partir de la propriété `Wheel` de la structure [RacingWheelReading][].

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

Même si les valeurs du volant correspondent à différents degrés de rotation physique dans le volant réel, selon la plage de rotations prise en charge par le volant de course physique, il n’est généralement pas nécessaire de mettre les valeurs du volant à l’échelle; les volants gérant des degrés de rotation plus étendus permettent simplement une plus grande précision.

### <a name="reading-the-throttle-and-brake"></a>Lecture des valeurs d’accélération et de freinage

L’accélération et le freinage sont des contrôles requis, qui fournissent des valeurs analogiques incluses entre 0.0 (élément entièrement relâché) et 1.0 (élément entièrement enclenché) représentées en tant que valeurs à virgule flottante. La valeur du contrôle d’accélération est lue à partir de la propriété `Throttle` de la structure [RacingWheelReading][]; la valeur du contrôle de freinage est lue à partir de la propriété `Brake`.

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>Lecture des valeurs de frein à main et d’embrayage

Le frein à main et l’embrayage sont des contrôles en option, qui fournissent des valeurs analogiques incluses entre 0.0 (élément entièrement relâché) et 1.0 (élément entièrement enclenché) représentées en tant que valeurs à virgule flottante. La valeur du contrôle de frein à main est lue à partir de la propriété `Handbrake` de la structure [RacingWheelReading][]; la valeur du contrôle d’embrayage est lue à partir de la propriété `Clutch`.

```cpp
float handbrake = 0.0;
float clutch = 0.0;

if(racingwheel->HasHandbrake)
{
    handbrake = reading.Handbrake;  // returns a value between 0.0 and 1.0
}

if(racingwheel->HasClutch)
{
    clutch = reading.Clutch;        // returns a value between 0.0 and 1.0
}
```

### <a name="reading-the-pattern-shifter"></a>Lecture de la valeur du levier de vitesses

Le levier de vitesses est un contrôle en option, qui fournit une valeur numérique incluse entre -1 et [MaxPatternShifterGear][], représentée en tant que valeur entière signée. Une valeur de -1 ou 0 correspond à la _marche arrière_ et au _point mort_, respectivement; des valeurs positives croissantes indiquent des vitesses avant supérieures à [MaxPatternShifterGear][] (valeur incluse). La valeur du levier de vitesses est lue à partir de la propriété `PatternShifterGear` de la structure [RacingWheelReading][].

```cpp
if (racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear;
}
```

> [!NOTE]
> Le levier de vitesses, lorsqu’il est pris en charge, se trouve à côté des boutons **Vitesse précédente** et **Vitesse suivante** requis, qui affectent également la vitesse actuelle de la voiture du joueur. Pour unifier ces entrées lorsque les deux éléments sont présents, vous pouvez tout simplement ignorer le levier de vitesses (et l’embrayage) lorsqu’un joueur choisit une voiture avec une transmission automatique, et ignorer les boutons **Vitesse précédente** et **Vitesse suivante** lorsqu’il opte pour une transmission manuelle, uniquement si le volant de course est doté d’un contrôle de levier de vitesses. Vous pouvez implémenter une autre stratégie d’unification si celle que nous avons décrite n'est pas adaptée à votre jeu.

## <a name="run-the-inputinterfacing-sample"></a>Exécuter l’exemple InputInterfacing

L’[exemple InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) indique comment utiliser les volants de course et différents types de périphériques d’entrée en parallèle, et décrit leur comportement de ces périphériques en tant que contrôleurs de navigation dans l’interface de navigation.

## <a name="force-feedback-overview"></a>Présentation du retour de force

De nombreux volants de course incluent une fonctionnalité de retour de force, qui fournit au joueur une expérience de conduite plus difficile, mais aussi plus immersive. Les volants de course prenant en charge le retour de force sont généralement dotés d’un seul moteur, qui applique une force au volant le long d’un axe unique, celui de la rotation de la roue. Le retour de charge est pris en charge dans les applications UWP Windows 10 et Xbox One, par l’espace de noms [Windows.Gaming.Input.ForceFeedback][].

> [!NOTE]
> Les API de retour de force peuvent gérer plusieurs axes de force, mais aucun volant de course Xbox One ne gère actuellement un axe de retour autre que celui de la rotation de la roue.

## <a name="using-force-feedback"></a>Utilisation du retour de force

Ces sections décrivent les bases de la programmation des effets de retour de force pour les volants de course Xbox One. Ce retour est appliqué via différents effets, qui sont d’abord chargés sur l’appareil devant appliquer ce retour. Les effets peuvent être démarrés, interrompus, repris et arrêtés de la même manière que les effets sonores, mais vous devez d’abord déterminer les fonctionnalités de retour du volant de course.

### <a name="determining-force-feedback-capabilities"></a>Identification des fonctionnalités de retour de force

Vous pouvez déterminer si un volant connecté gère le retour de force en lisant la propriété [WheelMotor][] associée au volant. Si la propriété `WheelMotor` a la valeur **null**, cela signifie que le retour de force n’est pas pris en charge; toute autre valeur indique qu’il est géré et vous pouvez déterminer les fonctionnalités de retour spécifiques du moteur, par exemple les axes susceptibles d’être affectés.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    auto axes = racingwheel->WheelMotor->SupportedAxes;

    if(ForceFeedbackEffectAxes::X == (axes & ForceFeedbackEffectAxes::X))
    {
        // Force can be applied through the X axis
    }

    if(ForceFeedbackEffectAxes::Y == (axes & ForceFeedbackEffectAxes::Y))
    {
        // Force can be applied through the Y axis
    }

    if(ForceFeedbackEffectAxes::Z == (axes & ForceFeedbackEffectAxes::Z))
    {
        // Force can be applied through the Z axis
    }
}
```

### <a name="loading-force-feedback-effects"></a>Chargement des effets de retour de force

Ces effets sont chargés sur l’appareil devant les appliquer, puis «exécutés» de manière autonome lorsque le jeu le commande. Nous fournissons certains effets de base, mais vous pouvez créer des effets personnalisés via une classe qui implémente l’interface [IForceFeedbackEffect][].

| Classe d’effet         | Description de l’effet                                                                     |
| -------------------- | -------------------------------------------------------------------------------------- |
| ConditionForceEffect | Effet appliquant une force variable en réponse à un capteur dans l’appareil. |
| ConstantForceEffect  | Effet appliquant une force constante le long d’un vecteur.                                  |
| PeriodicForceEffect  | Effet appliquant une force variable définie par une forme d’onde, le long d’un vecteur.           |
| RampForceEffect      | Effet appliquant une force qui augmente ou baisse de manière linéaire, le long d’un vecteur.          |

```cpp
using FFLoadEffectResult = ForceFeedback::ForceFeedbackLoadEffectResult;

auto effect = ref new Windows.Gaming::Input::ForceFeedback::ConstantForceEffect();
auto time = TimeSpan(10000);

effect->SetParameters(Windows::Foundation::Numerics::float3(1.0f, 0.0f, 0.0f), time);

// Here, we assume 'racingwheel' is valid and supports force feedback

IAsyncOperation<FFLoadEffectResult>^ request
    = racingwheel->WheelMotor->LoadEffectAsync(effect);

auto loadEffectTask = Concurrency::create_task(request);

loadEffectTask.then([this](FFLoadEffectResult result)
{
    if (FFLoadEffectResult::Succeeded == result)
    {
        // effect successfully loaded
    }
    else
    {
        // effect failed to load
    }
}).wait();
```

### <a name="using-force-feedback-effects"></a>Utilisation des effets de retour de force

Une fois chargés, les effets peuvent être démarrés, interrompus, repris et arrêtés de manière synchrone, grâce à un appel à des fonctions sur la propriété `WheelMotor` du volant de course, ou de manière individuelle, grâce à un appel à des fonctions sur l’effet de retour lui-même. En général, il est recommandé de charger tous les effets que vous souhaitez utiliser sur l’appareil devant les exécuter avant que le jeu ne se lance, puis d’utiliser les fonctions `SetParameters` correspondantes pour mettre à jour les effets au fur et à mesure de la progression du jeu.

```cpp
if (ForceFeedbackEffectState::Running == effect->State)
{
    effect->Stop();
}
else
{
    effect->Start();
}
```

Pour finir, vous pouvez activer, désactiver ou réinitialiser l’ensemble du système de retour de force sur un volant de course spécifique, de manière asynchrone, quand vous le souhaitez.

## <a name="see-also"></a>Voir aussi

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Pratiques de saisie pour les jeux](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[racingwheels]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheels.aspx
[racingwheeladded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheeladded.aspx
[racingwheelremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheelremoved.aspx
[haspatternshifter]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.haspatternshifter.aspx
[hashandbrake]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hashandbrake.aspx
[hasclutch]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hasclutch.aspx
[maxpatternshiftergear]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxpatternshiftergear.aspx
[maxwheelangle]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxwheelangle.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.getcurrentreading.aspx
[wheelmotor]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.wheelmotor.aspx
[racingwheelreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelreading.aspx
[racingwheelbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelbuttons.aspx
