---
author: mithom
title: "Pratiques d’entrée pour les jeux"
description: "Découvrez les modèles et techniques pour utiliser efficacement les périphériques d’entrée."
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, jeux, entrée"
ms.openlocfilehash: 15d56a27ad914b258bb19b80b3498510d01105cd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="input-practices-for-games"></a>Pratiques d’entrée pour les jeux

Cette page décrit les modèles et techniques pour utiliser efficacement les périphériques d’entrée dans les jeux de plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cette page:
* Comment suivre les joueurs et les périphériques d’entrée et de navigation qu’ils utilisent
* Comment détecter les transitions de bouton (appuyé à relâché, relâché à appuyé)
* Comment détecter les dispositions de boutons complexes à l’aide d’un seul et même test

## <a name="tracking-users-and-their-devices"></a>Suivi des utilisateurs et de leurs périphériques

Tous les périphériques d’entrée sont associés à un [utilisateur][Windows.System.User] afin que son identité puisse être liée à sa séquence de jeu, ses succès, ses modifications de paramètres et ses autres activités. Les utilisateurs peuvent se connecter ou se déconnecter à volonté, et il est courant qu’un utilisateur différent se connecte au périphérique d’entrée qui reste connecté au système après la déconnexion de l’utilisateur précédent. À la connexion ou déconnexion d’un utilisateur, l’événement [IGameController.UserChanged][] est déclenché. Vous pouvez inscrire un gestionnaire d’événements pour cet événement afin d’effectuer le suivi des joueurs et des périphériques qu’ils utilisent.

Une identité d’utilisateur est également le moyen par le biais duquel un périphérique d’entrée est associé à son contrôleur de navigation d’interface utilisateur correspondant.

C’est pour ces raisons qu’il convient de suivre les entrées de joueur et de les mettre en corrélation à l’aide de la propriété [User][igamecontroller.user] de la classe de périphérique (héritée de l’interface [IGameController][]).

L’exemple [UserGamepadPairingUWP _(github)_](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) illustre le suivi d’utilisateurs et de leurs périphériques.

## <a name="detecting-button-transitions"></a>Détection de transitions de boutons

Vous souhaiterez savoir parfois quand un bouton est d’abord enfoncé ou relâché, autrement dit lorsque l’état du bouton passe de relâché à appuyé, ou inversement. Pour le déterminer, vous devez mémoriser la lecture précédente du périphérique et la comparer à la lecture actuelle pour voir ce qui a changé.

L’exemple suivant illustre une approche de base pour mémoriser la lecture précédente; des boîtiers de commande sont affichés ici, mais les principes sont les mêmes pour les sticks arcade, les volants de course et les boutons de navigation d’interface utilisateur.

```cpp
GamepadReading newReading();
GamepadReading oldReading();

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.CurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased
}
```

Avant toute autre action, `Game::Loop` déplace la valeur existante de `newReading` (lecture du boîtier de commande de l’itération de boucle précédente) dans `oldReading`, puis renseigne `newReading` avec une nouvelle lecture du boîtier de commande correspondant à l’itération actuelle. Vous disposez alors des informations nécessaires pour détecter les transitions de boutons.

L’exemple suivant illustre une approche de base pour détecter les transitions de boutons.

```cpp
bool buttonJustPressed(const gamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool buttonJustReleased(gamepadButtons selection)
{
    bool newSelectionReleased = (gamepadButtons.None == (newReading.Buttons & selection));
    bool oldSelectionReleased = (gamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Ces deuxfonctions dérivent d’abord l’état booléen de la sélection de bouton de `newReading` et `oldReading`, puis effectuent une logique booléenne pour déterminer si la transition cible s’est produite. Ces fonctions retournent _true_ uniquement si la nouvelle lecture contient l’état cible (appuyé ou relâché, respectivement) *et* si l’ancienne lecture ne contient pas également l’état cible. Dans le cas contraire, elles retournent _false_.


## <a name="detecting-complex-button-arrangements"></a>Détection des dispositions de boutons complexes

Chaque bouton d’un périphérique d’entrée fournit une lecture numérique qui indique s’il est à l’état appuyé (position basse) ou relâché (position haute). Pour plus d’efficacité, les entrées de bouton ne sont pas représentées individuellement sous forme de valeurs booléennes. Elles sont toutes regroupées dans des champs de bits représentés par des énumérations propres aux périphériques, par exemple [GamepadButtons][]. Pour lire des boutons spécifiques, un masquage au niveau du bit est effectué pour isoler les valeurs qui vous intéressent. Les boutons sont à l’état appuyé (position basse) quand leur bit correspondant est défini; sinon, il est à l’état relâché (position haute).

Rappelez comme déterminer que les boutons uniques sont à l’état appuyé ou relâché; des boîtiers de commande sont affichés ici, mais les principes sont les mêmes pour les sticks arcade, les volants de course et les boutons de navigation d’interface utilisateur.

```cpp
// determines whether gamepad button A is pressed
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}

// determines whether gamepad button A is released
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released (not pressed)
}
```

Comme vous le constatez, la détermination de l’état d’un bouton unique est simple, mais vous souhaiterez peut-être savoir parfois si plusieurs boutons sont à l’état appuyé ou relâché, ou si un groupe de boutons a une disposition particulière, certains présentant l’état appuyé et d’autres l’état relâché. Tester plusieurs boutons est plus complexe que tester des boutons uniques, notamment avec le potentiel de l’état de bouton mixte, mais il existe une formule simple qui s’applique indifféremment aux tests des boutons uniques et multiples.

L’exemple suivant détermine si les boutons A et B du boîtier de commande sont tous les deux à l’état appuyé.

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are pressed
}
```

L’exemple suivant détermine si les boutons A et B du boîtier de commande sont tous les deux à l’état relâché.

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are released (not pressed)
}
```

L’exemple suivant détermine si le bouton A du boîtier de commande est à l’état appuyé tandis que le bouton B est à l’état relâché.

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

Dans la formule que ces cinqexemples ont en commun, la disposition des boutons à tester est spécifiée par l’expression située à gauche de l’opérateur d’égalité tandis que les boutons à examiner sont sélectionnés par l’expression de masquage à droite.

L’exemple suivant présente cette formule plus clairement en réécrivant l’exemple précédent.

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

Cette formule peut être appliquée pour tester n’importe quel nombre de boutons, avec toutes les dispositions de leur état.



[Windows.System.User]: https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.user]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.user.aspx
[igamecontroller.userchanged]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.userchanged.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
