---
title: Stick arcade
description: Utilisez les API de stick arcade Windows.Gaming.Input pour détecter les sticks arcade et lire leurs entrées.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, jeux, stick arcade, entrée
ms.localizationpriority: medium
ms.openlocfilehash: 6f9e3ff29dfb17b6e2a07df52153013b5266206e
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933814"
---
# <a name="arcade-stick"></a>Stick arcade

Cet article explique les notions de base de la programmation pour les sticks arcade XboxOne avec l’API [Windows.Gaming.Input.ArcadeStick][arcadestick] et les API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article:

* Obtenir une liste des sticks arcade connectés et de leurs utilisateurs
* Détecter l’ajout ou la suppression d’un stick arcade
* Lire les entrées provenant d’un ou de plusieurs sticks arcade
* comporte des sticks arcade en tant que périphériques de navigation d’interface utilisateur

## <a name="arcade-stick-overview"></a>Vue d’ensemble des sticks arcade

Les sticks arcade sont des périphériques d’entrée appréciés pour leur capacité à reproduire la sensation des machines d’arcade autonomes et pour leurs contrôles numériques de haute précision. Les sticks arcade constituent le périphérique d’entrée parfait pour les combats tête à tête ou d’autres jeux de type arcade. Ils conviennent à tous les jeux qui fonctionnent bien avec des contrôles entièrement numériques. Les sticks arcade sont pris en charge dans les applications UWP Windows10 et XboxOne par l’espace de noms [Windows.Gaming.Input][].

Sticks arcade Xbox One sont dotés d’une manette de jeu numérique 8 voies, six boutons **d’Action** (représentées en tant que A1-A6 dans l’image ci-dessous) et deux boutons **spéciaux** (représentées en tant que S1 et S2); elles sont des périphériques d’entrée entièrement numériques qui ne prennent en charge les contrôles analogiques ni la vibration. Sticks arcade Xbox One sont également dotés de boutons **d’affichage** et de **Menu** prennent en charge la navigation d’interface utilisateur, mais ils ne sont pas conçus pour prendre en charge les commandes de jeu et ne peut pas être utilisables en tant que les boutons de manette de jeu.

![Stick avec 4 directionnelle de manette de jeu, arcade 6 boutons d’action (A1-A6) et les boutons spéciaux 2 (S1 et S2)](images/arcade-stick-1.png)

### <a name="ui-navigation"></a>Navigation d’interface utilisateur

Pour faciliter la prise en charge de nombreux périphériques d’entrée différents pour la navigation dans l’interface utilisateur, et améliorer la cohérence entre les jeux et les périphériques, la plupart des périphériques d’entrée _physiques_ jouent simultanément le rôle de périphérique d’entrée _logique_ distinct, appelé [contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md). Le contrôleur de navigation d’interface utilisateur fournit un vocabulaire commun pour les commandes de navigation d’interface utilisateur utilisées sur les différents périphériques d’entrée.

En tant qu’un contrôleur de navigation d’interface utilisateur, les sticks arcade mappent l' [ensemble](ui-navigation-controller.md#required-set) des commandes de navigation pour la manette de jeu et les boutons **d’affichage**, **Menu**, **Action 1**et **2 de l’Action** .

| Commande de navigation | Entrée stick arcade  |
| ------------------:| ------------------- |
|                 Up (Haut) | Stick vers le haut            |
|               Down (Bas) | Stick vers le bas          |
|               Left (Gauche) | Stick vers la gauche          |
|              Right (Droite) | Stick vers la droite         |
|               View (Afficher) | Bouton Afficher         |
|               Menu | Bouton Menu         |
|             Accept (Accepter) | Bouton Action1     |
|             Cancel (Annuler) | Bouton Action2     |

Les sticks arcade ne mappent aucun [ensemble facultatif](ui-navigation-controller.md#optional-set) de commandes de navigation.

## <a name="detect-and-track-arcade-sticks"></a>Détecter et suivre des sticks arcade

Détecter et de suivi sticks arcade fonctionne exactement de la même manière que pour les boîtiers de commande, sauf avec la classe [ArcadeStick][] au lieu de la classe de [boîtier de commande](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) . Pour en savoir plus, consultez la rubrique [Boîtier de commande et vibration](gamepad-and-vibration.md).

<!-- Arcade sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected arcades sticks and events to notify you when an arcade stick is added or removed.

### The arcade sticks list

The [ArcadeStick][] class provides a static property, [ArcadeSticks][], which is a read-only list of arcade sticks that are currently connected. Because you might only be interested in some of the connected arcade sticks, it's recommended that you maintain your own collection instead of accessing them through the `ArcadeSticks` property.

The following example copies all connected arcade sticks into a new collection. Note that because other threads in the background will be accessing this collection (in the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events), you need to place a lock around any code that reads or updates the collection.

```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();
critical_section myLock{};

for (auto arcadeStick : ArcadeStick::ArcadeSticks)
{
    // Check if the arcade stick is already in myArcadeSticks; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myArcadeSticks), end(myArcadeSticks), arcadeStick);

    if (it == end(myArcadeSticks))
    {
        // This code assumes that you're interested in all arcade sticks.
        myArcadeSticks->Append(arcadeStick);
    }
}
```

### Adding and removing arcade sticks

When an arcade stick is added or removed the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events are raised. You can register handlers for these events to keep track of the arcade sticks that are currently connected.

The following example starts tracking an arcade stick that's been added.

```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // Check if the just-added arcade stick is already in myArcadeSticks; if it
    // isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

The following example stops tracking an arcade stick that's been removed.

```cpp
ArcadeStick::ArcadeStickRemoved += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    unsigned int indexRemoved;

    if(myArcadeSticks->IndexOf(args, &indexRemoved))
    {
        myArcadeSticks->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each arcade stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-arcade-stick"></a>Lecture des entrées du stick arcade

Une fois que vous avez identifié le stick arcade qui vous intéresse, vous pouvez commencer à collecter les entrées de ce stick. Toutefois, contrairement à d’autres sortes d’entrées que vous connaissez peut-être, les sticks arcade ne communiquent pas les changements d’état en déclenchant des événements. À la place, vous devez _interroger_ régulièrement ces sticks arcade pour connaître leur état actuel.

### <a name="polling-the-arcade-stick"></a>Interroger le stick arcade

Le processus d’interrogation capture un instantané du stick arcade à un moment précis. Cette approche de collecte des entrées convient à la plupart des jeux, dont la logique s’exécute généralement selon une boucle déterministe plutôt que sur la base des événements. Par ailleurs, elle facilite l’interprétation des commandes de jeu, car toutes les entrées sont collectées en même temps, et non pas les unes après les autres.

Vous interrogez un stick arcade en appelant la fonction [GetCurrentReading][]. Cette fonction renvoie un [ArcadeStickReading][] qui contient l’état du stick arcade.

L’exemple de code suivant interroge un stick arcade pour obtenir son état actuel.

```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

En plus de l’état du stick arcade, chaque valeur comprend un horodatage qui indique précisément le moment d’extraction de cet état. Cet horodatage est utile pour faire le lien avec le minutage des valeurs précédentes ou de la simulation de jeu.

### <a name="reading-the-buttons"></a>Lecture des entrées des boutons

Chacun des boutons de stick arcade&mdash;quatre directions de la manette de jeu, six boutons **d’Action** et deux boutons **spéciaux** &mdash;fournit une lecture numérique qui indique si elle est enfoncé (bas) ou relâché (haut). Pour plus d’efficacité, les entrées de bouton ne sont pas représentées individuellement sous forme de valeurs booléennes; au lieu de cela, elles sont toutes regroupées dans un seul champ de bits représenté par l’énumération [ArcadeStickButtons][] .

> [!NOTE]
> Les sticks arcade sont dotés de boutons supplémentaires, utilisés pour la navigation dans l’interface utilisateur tels que les boutons **d’affichage** et de **Menu** . Ces boutons ne figurent pas dans l’énumération `ArcadeStickButtons`. Leurs entrées sont lues uniquement quand le stick arcade est utilisé comme périphérique de navigation d’interface utilisateur. Pour plus d’informations, consultez [Périphérique de navigation d’interface utilisateur](ui-navigation-controller.md).

Les valeurs des boutons sont lues à partir de la propriété `Buttons` de la structure [ArcadeStickReading][]. Comme cette propriété est un champ de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est à l’état enfoncé (position basse) lorsque le bit correspondant est défini; dans le cas contraire, il se trouve à l’état relâché (position haute).

L’exemple suivant détermine si le bouton **Action1** est enfoncé.

```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

L’exemple suivant détermine si le bouton **Action1** est l’état relâché.

```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

Vous pouvez avoir besoin de savoir quand un bouton passe de l’état enfoncé à l’état relâché, ou inversement, si plusieurs boutons sont enfoncés ou relâchés, ou si un groupe de boutons possède une disposition particulière &mdash; certains étant enfoncés et d’autres relâchés. Pour plus d’informations sur la détection de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-inputinterfacing-sample"></a>Exécuter l’exemple InputInterfacing

L’[exemple InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) montre comment utiliser les sticks arcade en tandem avec différents types de périphériques d’entrée. Il illustre aussi le comportement de ces périphériques d’entrée utilisés comme contrôleurs de navigation d’interface utilisateur.

## <a name="see-also"></a>Voir aussi

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Pratiques de saisie pour les jeux](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[arcadesticks]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadesticks.aspx
[arcadestickadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickadded.aspx
[arcadestickremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.getcurrentreading.aspx
[arcadestickreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickreading.aspx
[arcadestickbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickbuttons.aspx
