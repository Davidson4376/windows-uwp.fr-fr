---
author: mithom
title: Stick arcade
description: Utilisez les API de stick arcade Windows.Gaming.Input pour détecter les sticks arcade et lire leurs entrées.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, jeux, stick arcade, entrée
ms.openlocfilehash: b0411dcf1fd75ec7dc31d29a39e95f5c26073953
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.locfileid: "228837"
---
# <a name="arcade-stick"></a>Stick arcade

Cet article explique les notions de base de la programmation pour les sticks arcade XboxOne avec l’API [Windows.Gaming.Input.ArcadeStick][arcadestick] et les API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article:
* Obtenir une liste des sticks arcade connectés et de leurs utilisateurs
* Détecter l’ajout ou la suppression d’un stick arcade
* Lire les entrées provenant d’un ou de plusieurs sticks arcade
* Comprendre le comportement des sticks arcade utilisés comme périphérique de navigation


## <a name="arcade-stick-overview"></a>Vue d’ensemble des sticks arcade

Les sticks arcade sont des périphériques d’entrée appréciés pour leur capacité à reproduire la sensation des machines d’arcade autonomes et pour leurs contrôles numériques de haute précision. Les sticks arcade constituent le périphérique d’entrée parfait pour les combats tête à tête ou d’autres jeux de type arcade. Ils conviennent à tous les jeux qui fonctionnent bien avec des contrôles entièrement numériques. Les sticks arcade sont pris en charge dans les applications UWP Windows10 et XboxOne par l’espace de noms [Windows.Gaming.Input][].

Les sticks arcade XboxOne sont équipés d’une manette de jeu numérique à 8directions, de sixboutons d’**action** et de deuxboutons **spéciaux**. Ce sont des périphériques d’entrée entièrement numériques qui ne prennent pas en charge les contrôles analogiques ni la vibration. Les sticks arcade XboxOne comportent également les boutons **Afficher** et **Menu**. Ces boutons prennent en charge la navigation d’interface utilisateur, mais ils ne sont pas conçus pour prendre en charge les commandes de jeu ni pour être utilisés comme des boutons de manette de jeu.

### <a name="ui-navigation"></a>Navigation d’interface utilisateur

Pour faciliter la prise en charge de nombreux périphériques d’entrée différents pour la navigation dans l’interface utilisateur, et améliorer la cohérence entre les jeux et les périphériques, la plupart des périphériques d’entrée _physiques_ jouent simultanément le rôle de périphérique d’entrée _logique_ distinct, appelé [contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md). Le contrôleur de navigation d’interface utilisateur fournit un vocabulaire commun pour les commandes de navigation d’interface utilisateur utilisées sur les différents périphériques d’entrée.

Quand ils sont utilisés comme contrôleurs de navigation d’interface, les sticks arcade mappent l’[ensemble obligatoire](ui-navigation-controller.md#required-set) de commandes de navigation à la manette de jeu ainsi qu’aux boutons **Afficher**, **Menu**, **Action1** et **Action2**.

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

Comme les sticks arcade sont gérés par le système, vous n’avez pas à les créer ni à les initialiser. Le système fournit une liste des sticks arcade connectés ainsi que des événements de notification d’ajout ou de suppression d’un stick arcade.

### <a name="the-arcade-sticks-list"></a>Liste des sticks arcade

La classe [ArcadeStick][] fournit la propriété statique [ArcadeSticks][], qui est une liste en lecture seule de tous les sticks arcade qui sont actuellement connectés. Vous serez certainement intéressé uniquement par certains des sticks arcade connectés. C’est pourquoi nous vous recommandons de gérer votre propre collection de sticks arcade au lieu d’utiliser ceux répertoriés par la propriété `ArcadeSticks`.

L’exemple de code suivant copie tous les sticks arcade connectés dans une nouvelle collection.
```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();

for (auto arcadestick : ArcadeStick::ArcadeSticks)
{
    // This code assumes that you're interested in all arcade sticks.
    myArcadeSticks->Append(arcadestick);
}
```

### <a name="adding-and-removing-arcade-sticks"></a>Ajout et suppression de sticks arcade

Quand un stick arcade est ajouté ou supprimé, les événements [ArcadeStickAdded][] et [ArcadeStickRemoved][] sont déclenchés. Vous pouvez inscrire des gestionnaires pour ces deux événements afin d’effectuer le suivi des sticks arcade actuellement connectés.

L’exemple de code suivant démarre le suivi d’un stick arcade qui a été ajouté.
```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

L’exemple de code suivant arrête le suivi d’un stick arcade qui a été supprimé.
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

### <a name="users-and-headsets"></a>Utilisateurs et casques

Chaque stick arcade peut être associé à un compte d’utilisateur pour lier l’identité de l’utilisateur à son jeu, et peut être branché à un casque permettant d’utiliser plus facilement les fonctionnalités de chat vocal et dans le jeu. Pour en savoir plus sur le suivi des utilisateurs et l’utilisation de casques, consultez [Suivi des utilisateurs et de leurs périphériques](input-practices-for-games.md#tracking-users-and-their-devices) et [Casques](headset.md).


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

Chacun des boutons du stick arcade (les quatre boutons directionnels de la manette, les six boutons d’**action** et les deux boutons **spéciaux**) fournit une valeur numérique qui indique si le bouton est à l’état appuyé (position basse) ou à l’état relâché (position haute). Pour plus d’efficacité, les entrées de bouton ne sont pas représentées individuellement sous forme de valeurs booléennes. Elles sont toutes regroupées dans un seul champ de bits représenté par l’énumération [ArcadeStickButtons][].

> **Remarque** Les sticks arcade sont dotés de boutons supplémentaires, utilisés pour la navigation dans l’interface utilisateur, par exemple des boutons **Afficher** et **Menu**. Ces boutons ne figurent pas dans l’énumération `ArcadeStickButtons`. Leurs entrées sont lues uniquement quand le stick arcade est utilisé comme périphérique de navigation d’interface utilisateur. Pour plus d’informations, consultez [Périphérique de navigation d’interface utilisateur](ui-navigation-controller.md).

Les valeurs des boutons sont lues à partir de la propriété `Buttons` de la structure [ArcadeStickReading][]. Comme cette propriété est un champ de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est à l’état appuyé (position basse) quand le bit correspondant est défini; sinon, il est à l’état relâché (position haute).

L’exemple suivant détermine si le bouton Action1 est à l’état appuyé.
```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

L’exemple suivant détermine si le bouton Action1 est à l’état relâché.
```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

Vous pouvez avoir besoin de savoir quand un bouton passe de l’état appuyé à l’état relâché, ou inversement, si plusieurs boutons sont à l’état appuyé ou relâché, ou si un groupe de boutons présente une disposition particulière, certains présentant l’état appuyé et d’autres l’état relâché. Pour plus d’informations sur la détection de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-inputinterfacing-sample"></a>Exécuter l’exemple InputInterfacing

L’[exemple InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) montre comment utiliser les sticks arcade en tandem avec différents types de périphériques d’entrée. Il illustre aussi le comportement de ces périphériques d’entrée utilisés comme contrôleurs de navigation d’interface utilisateur.


## <a name="see-also"></a>Voir aussi
[Windows.Gaming.Input.UINavigationController][]
[Windows.Gaming.Input.IGameController][]

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
