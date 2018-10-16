---
author: eliotcowley
title: Contrôleur de navigation d’interface utilisateur
description: Utilisez les API du contrôleur de navigation d’interface utilisateur Windows.Gaming.Input pour détecter différents types de périphériques d’entrée et lire leurs entrées pour la navigation dans l’interface utilisateur.
ms.assetid: 5A14926D-8C2E-4DE8-AAFB-BEEB9BFE91A5
ms.author: elcowle
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, jeux, interface utilisateur, navigation
ms.localizationpriority: medium
ms.openlocfilehash: 4f95094ebf31c4b80ee8858ad849da33ff16434a
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4622898"
---
# <a name="ui-navigation-controller"></a>Contrôleur de navigation d’interface utilisateur

Cet article explique les notions de base de la programmation pour les périphériques de navigation d’interface avec l’API [Windows.Gaming.Input.UINavigationController][uinavigationcontroller] et les API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article:
* Obtenir une liste des périphériques de navigation d’interface utilisateur connectés et de leurs utilisateurs
* Détecter l’ajout ou la suppression d’un périphérique de navigation
* Lire les entrées provenant d’un ou de plusieurs périphériques de navigation d’interface utilisateur
* Comprendre le comportement des boîtiers de commande et des sticks arcade utilisés comme périphériques de navigation

## <a name="ui-navigation-controller-overview"></a>Vue d’ensemble du contrôleur de navigation d’interface utilisateur

Presque tous les jeux présentent des éléments d’interface utilisateur qui sont indépendants de l’expérience de jeu proprement dite, qu’il s’agisse simplement de menus de préparation au jeu ou de boîtes de dialogue intégrées au jeu. Les joueurs doivent pouvoir naviguer dans cette interface utilisateur sans problème, en utilisant le périphérique d’entrée de leur choix. Pour répondre à cette exigence, les développeurs sont obligés de prévoir une prise en charge de tous les types de périphériques d’entrée, sans compter le risque d’introduire des incohérences de comportement entre les jeux et les périphériques d’entrée qui déroutent les joueurs. Ce sont pour ces raisons que nous avons créé l’API [UINavigationController][].

Les contrôleurs de navigation d’interface utilisateur sont des périphériques d’entrée _logiques_ qui servent à rendre les principales commandes de navigation d’interface utilisateur compréhensibles par différents types de périphériques d’entrée _physiques_. Un _contrôleur de navigation d’interface utilisateur_ est simplement une autre manière de voir un périphérique d’entrée physique. Nous employons le terme _périphérique de navigation_ pour désigner tout périphérique d’entrée physique qui est identifié comme contrôleur de navigation. En programmant pour un périphérique de navigation plutôt que pour des périphériques d’entrée spécifiques, les développeurs n’ont plus besoin d’ajouter la prise en charge de chaque périphérique d’entrée et garantissent une cohérence globale par défaut.

Le nombre et la variété de contrôles pris en charge par chaque type de périphérique d’entrée peuvent considérablement diverger. Par ailleurs, certains périphériques d’entrée nécessitent une prise en charge d’un plus large ensemble de commandes de navigation. C’est pourquoi l’interface du contrôleur de navigation fait la distinction entre les commandes essentielles et les plus courantes, qui sont regroupées dans un _ensemble obligatoire_, et les commandes utiles mais pas indispensables, qui sont regroupées dans un _ensemble facultatif_. Chaque périphérique de navigation doit prendre en charge toutes les commandes de l’_ensemble obligatoire_. Il peut prendre en charge toutes les commandes ou certaines commandes de l’_ensemble facultatif_, ou n’en prendre en charge aucune.

### <a name="required-set"></a>Ensemble obligatoire

Les périphériques de navigation doivent prendre en charge toutes les commandes de navigation contenues dans l’_ensemble obligatoire_. Il s’agit des commandes directionnelles (haut, bas, gauche et droite), ainsi que des commandes d’affichage, de menu, d’acceptation et d’annulation.

Les commandes directionnelles sont conçues pour une [navigation en mode focusXY](../design/devices/designing-for-tv.md#xy-focus-navigation-and-interaction) simple entre des éléments d’interface utilisateur. Les commandes d’affichage et de menu s’utilisent pour afficher des informations de jeu (souvent de manière temporaire, parfois sous forme modale) et pour basculer entre les contextes de jeu et de menu, respectivement. Les commandes d’acceptation et d’annulation sont conçues pour recevoir des réponses affirmatives (oui) et des réponses négatives (non), respectivement.

Le tableau suivant récapitule ces commandes et leurs usages prévus, illustrés par des exemples.
| Commande | Usage prévu
| -------:| ---------------
|      Up (Haut) | Navigation vers le haut en mode focusXY
|    Down (Bas) | Navigation vers le bas en mode focusXY
|    Left (Gauche) | Navigation vers la gauche en mode focusXY
|   Right (Droite) | Navigation vers la droite en mode focusXY
|    View (Afficher) | Affichage des informations de jeu _(score, statistiques de jeu, objectifs, carte du monde ou d’une zone)_
|    Menu | Menu principal/ Pause _(paramètres, état, équipement, inventaire, pause)_
|  Accept (Accepter) | Réponse affirmative _(accepter, avancer, confirmer, démarrer, oui)_
|  Cancel (Annuler) | Réponse négative _(rejeter, inverser, refuser, arrêter, non)_


### <a name="optional-set"></a>Ensemble facultatif

Les périphériques de navigation peuvent aussi prendre en charge toutes les commandes ou certaines commandes de l’_ensemble facultatif_, ou n’en prendre en charge aucune. Il s’agit des commandes de pagination (haut, bas, gauche et droite), de défilement (haut, bas, gauche et droite) et contextuelles (contextes 1à4).

Les commandes contextuelles sont conçues explicitement pour des raccourcis de navigation et des commandes propres à l’application. Les commandes de pagination et de défilement sont conçues pour permettre la navigation rapide entre des pages ou des groupes d’éléments d’interface utilisateur, et permettre une navigation précise au sein des éléments d’interface utilisateur, respectivement.

Le tableau suivant récapitule ces commandes et leurs usages prévus.
|     Commande | Usage prévu
| -----------:| ------------
|      PageUp (Pg. préc) | Aller vers le haut (à la page ou au groupe supérieur/précédent, verticalement)
|    PageDown (Pg. suiv) | Aller vers le bas (à la page ou au groupe inférieur/suivant, verticalement)
|    PageLeft (Page gauche) | Aller à gauche (à la page ou au groupe à gauche/précédent, horizontalement)
|   PageRight (Page droite) | Aller à droite (à la page ou au groupe à droite/suivant, horizontalement)
|    ScrollUp (Faire défiler vers le haut) | Faire défiler vers le haut (au sein de l’élément d’interface utilisateur ou du groupe de défilement actif)
|  ScrollDown (Faire défiler vers le bas) | Faire défiler vers le bas (au sein de l’élément d’interface utilisateur ou du groupe de défilement actif)
|  ScrollLeft (Faire défiler vers la gauche) | Faire défiler vers la gauche (au sein de l’élément d’interface utilisateur ou du groupe de défilement actif)
| ScrollRight (Faire défiler vers la droite) | Faire défiler vers la droite (au sein de l’élément d’interface utilisateur ou du groupe de défilement actif)
|    Context1 (Contexte1) | Action du contexte principal
|    Context2 (Contexte2) | Action du deuxième contexte
|    Context3 (Contexte3) | Action du troisième contexte
|    Context4 (Contexte4) | Action du quatrième contexte

> **Remarque** Un jeu est libre de répondre à une commande avec une autre fonction réelle que celle qui était prévue, mais il est recommandé d’éviter les comportements inattendus. En particulier, ne modifiez pas la fonction réelle d’une commande si vous en avez besoin pour l’usage prévu. Essayez d’affecter les nouvelles fonctions à la commande la plus appropriée et affectez des fonctions homologues à des commandes homologues (comme PageUp/PageDown). Enfin, examinez quelles commandes sont prises en charge par chaque type de périphérique d’entrée et à quels contrôles elles sont mappées pour vous assurer que toutes les commandes essentielles sont accessibles à partir de tous les périphériques d’entrée.

## <a name="gamepad-arcade-stick-and-racing-wheel-navigation"></a>Navigation avec un boîtier de commande, un stick arcade et un volant de course

Tous les périphériques d’entrée pris en charge par l’espace de noms Windows.Gaming.Input sont des périphériques de navigation d’interface utilisateur.

Le tableau suivant récapitule les mappages entre les commandes de navigation de l’_ensemble obligatoire_ et les différents périphériques d’entrée.

| Commande de navigation | Entrée boîtier de commande                       | Entrée stick arcade | Entrée volant de course |
| ------------------:| ----------------------------------- | ------------------ | ------------------ |
|                 Up (Haut) | Stick analogique gauche vers le haut/ bouton multidirectionnel vers le haut       | Stick vers le haut           | Bouton multidirectionnel vers le haut           |
|               Down (Bas) | Stick analogique gauche vers le bas/ bouton multidirectionnel vers le bas   | Stick vers le bas         | Bouton multidirectionnel vers le bas         |
|               Left (Gauche) | Stick analogique gauche vers la gauche/ bouton multidirectionnel vers la gauche   | Stick vers la gauche         | Bouton multidirectionnel vers la gauche         |
|              Right (Droite) | Stick analogique gauche vers la droite/ bouton multidirectionnel vers la droite | Stick vers la droite        | Bouton multidirectionnel vers la droite        |
|               View (Afficher) | Bouton Afficher                         | Bouton Afficher        | Bouton Afficher        |
|               Menu | Bouton Menu                         | Bouton Menu        | Bouton Menu        |
|             Accept (Accepter) | BoutonA                            | Bouton Action1    | BoutonA           |
|             Annuler | BoutonB                            | Bouton Action2    | BoutonB           |

Le tableau suivant récapitule les mappages entre les commandes de navigation de l’_ensemble facultatif_ et les différents périphériques d’entrée.

| Commande de navigation | Entrée boîtier de commande          | Entrée stick arcade | Entrée volant de course    |
| ------------------:| ---------------------- | ------------------ | --------------------- |
|             PageUp (Pg. préc) | Gâchette gauche           | _non pris en charge_    | _dépendant du jeu_              |
|           PageDown (Pg. suiv) | Gâchette droite          | _non pris en charge_    | _dépendant du jeu_              |
|           PageLeft (Page gauche) | Gâchette haute gauche            | _non pris en charge_    | _dépendant du jeu_              |
|          PageRight (Page droite) | Gâchette haute droite           | _non pris en charge_    | _dépendant du jeu_              |
|           ScrollUp (Faire défiler vers le haut) | Stick analogique droit vers le haut    | _non pris en charge_    | _dépendant du jeu_              |
|         ScrollDown (Faire défiler vers le bas) | Stick analogique droit vers le bas  | _non pris en charge_    | _dépendant du jeu_              |
|         ScrollLeft (Faire défiler vers la gauche) | Stick analogique droit vers la gauche  | _non pris en charge_    | _dépendant du jeu_              |
|        ScrollRight (Faire défiler vers la droite) | Stick analogique droit vers la droite | _non pris en charge_    | _dépendant du jeu_              |
|           Context1 (Contexte1) | BoutonX               | _non pris en charge_    | BoutonX (_en général_) |
|           Context2 (Contexte2) | BoutonY               | _non pris en charge_    | BoutonY (_en général_) |
|           Context3 (Contexte3) | Appui sur stick analogique gauche  | _non pris en charge_    | _dépendant du jeu_              |
|           Context4 (Contexte4) | Appui sur stick analogique droit | _non pris en charge_    | _dépendant du jeu_              |


## <a name="detect-and-track-ui-navigation-controllers"></a>Détecter et suivre les contrôleurs de navigation d’interface utilisateur

Les contrôleurs de navigation d’interface utilisateur sont des périphériques d’entrée logiques qui représentent un périphérique physique. À ce titre, ils sont gérés de la même façon par le système. Vous n’avez pas besoin de les créer ni de les initialiser. Le système fournit une liste des contrôleurs de navigation d’interface utilisateur connectés ainsi que des événements de notification d’ajout ou de suppression d’un de ces contrôleurs.

### <a name="the-ui-navigation-controllers-list"></a>Liste des contrôleurs de navigation d’interface utilisateur

La classe [UINavigationController][] fournit la propriété statique [UINavigationControllers][], qui est une liste en lecture seule de tous les périphériques de navigation d’interface utilisateur qui sont actuellement connectés. Vous serez certainement intéressé uniquement par certains des périphériques de navigation connectés. C’est pourquoi nous vous recommandons de gérer votre propre collection de contrôleurs au lieu d’utiliser ceux répertoriés par la propriété `UINavigationControllers`.

L’exemple de code suivant copie tous les contrôleurs de navigation d’interface utilisateur connectés dans une nouvelle collection.
```cpp
auto myNavigationControllers = ref new Vector<UINavigationController^>();

for (auto device : UINavigationController::UINavigationControllers)
{
    // This code assumes that you're interested in all navigation controllers.
    myNavigationControllers->Append(device);
}
```

### <a name="adding-and-removing-ui-navigation-controllers"></a>Ajout et suppression de contrôleurs de navigation d’interface utilisateur

Quand un contrôleur de navigation d’interface utilisateur est ajouté ou supprimé, les événements [UINavigationControllerAdded][] et [UINavigationControllerRemoved][] sont déclenchés. Vous pouvez inscrire un gestionnaire d’événements pour ces deux événements afin d’effectuer le suivi des périphériques de navigation actuellement connectés.

L’exemple de code suivant démarre le suivi d’un périphérique de navigation d’interface utilisateur qui a été ajouté.
```cpp
UINavigationController::UINavigationControllerAdded += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    // This code assumes that you're interested in all new navigation controllers.
    myNavigationControllers->Append(args);
}
```

L’exemple de code suivant arrête le suivi d’un stick arcade qui a été supprimé.
```cpp
UINavigationController::UINavigationControllerRemoved += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    unsigned int indexRemoved;

    if(myNavigationControllers->IndexOf(args, &indexRemoved))
    {
        myNavigationControllers->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>Utilisateurs et casques

Chaque périphérique de navigation peut être associé à un compte d’utilisateur pour lier l’identité de l’utilisateur à ses entrées, et peut être branché à un casque pour utiliser plus facilement les fonctionnalités de guidage et de chat vocal. Pour en savoir plus sur le suivi des utilisateurs et l’utilisation de casques, consultez [Suivi des utilisateurs et de leurs périphériques](input-practices-for-games.md#tracking-users-and-their-devices) et [Casques](headset.md).

## <a name="reading-the-ui-navigation-controller"></a>Lecture des entrées du contrôleur de navigation d’interface utilisateur

Une fois que vous avez identifié le périphérique de navigation d’interface utilisateur qui vous intéresse, vous pouvez commencer à collecter les entrées de ce périphérique. Toutefois, contrairement à d’autres sortes d’entrées que vous connaissez peut-être, les périphériques de navigation ne communiquent pas les changements d’état en déclenchant des événements. À la place, vous devez _interroger_ ces périphériques pour connaître leur état actuel.

### <a name="polling-the-ui-navigation-controller"></a>Interroger le contrôleur de navigation d’interface utilisateur

Le processus d’interrogation capture un instantané du périphérique de navigation à un moment précis. Cette approche de collecte des entrées convient à la plupart des jeux, dont la logique s’exécute généralement selon une boucle déterministe plutôt que sur la base des événements. Par ailleurs, elle facilite l’interprétation des commandes de jeu, car toutes les entrées sont collectées en même temps, et non pas les unes après les autres.

Vous interrogez un périphérique de navigation en appelant la fonction [UINavigationController.GetCurrentReading][getcurrentreading]. Cette fonction renvoie un [UINavigationReading][] qui contient l’état du périphérique de navigation.

```cpp
auto navigationController = myNavigationControllers[0];

UINavigationReading reading = navigationController->GetCurrentReading();
```

### <a name="reading-the-buttons"></a>Lecture des boutons

Chaque bouton de navigation d’interface utilisateur fournit une entrée booléenne qui indique si le bouton est à l’état appuyé (position basse) ou à l’état relâché (position haute). Pour plus d’efficacité, les entrées de bouton ne sont pas représentées individuellement sous forme de valeurs booléennes. Elles sont toutes regroupées dans un des deux champs de bits représentés par les énumérations [RequiredUINavigationButtons][] et [OptionalUINavigationButtons][].

Les boutons appartenant à l’_ensemble obligatoire_ sont lus à partir de la propriété `RequiredButtons` de la structure [UINavigationReading][]. Ceux de l’_ensemble facultatif_ sont lus à partir de la propriété `OptionalButtons`. Comme ces propriétés sont des champs de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est à l’état appuyé (position basse) quand le bit correspondant est défini; sinon, il est à l’état relâché (position haute).

L’exemple suivant détermine si le bouton Accept de l’_ensemble obligatoire_ est à l’état appuyé.
```cpp
if (RequiredUINavigationButtons::Accept == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is pressed
}
```

L’exemple suivant détermine si le bouton Accept de l’_ensemble obligatoire_ est à l’état relâché.
```cpp
if (RequiredUINavigationButtons::None == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is released (not pressed)
}
```

Assurez-vous d’utiliser la propriété `OptionalButtons` et l’énumération `OptionalUINavigationButtons` quand vous lisez les valeurs de boutons appartenant à l’_ensemble facultatif_.

L’exemple suivant détermine si le bouton Context1 de l’_ensemble facultatif_ est à l’état appuyé.
```cpp
if (OptionalUINavigationButtons::Context1 == (reading.OptionalButtons & OptionalUINavigationButtons::Context1))
{
    // Context 1 is pressed
}
```

Vous pouvez avoir besoin de savoir quand un bouton passe de l’état appuyé à l’état relâché, ou inversement, si plusieurs boutons sont à l’état appuyé ou relâché, ou si un groupe de boutons a une disposition particulière, certains présentant l’état appuyé et d’autres l’état relâché. Pour plus d’informations sur la détection de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).


## <a name="run-the-ui-navigation-controller-sample"></a>Exécuter l’exemple de contrôleur de navigation d’interface utilisateur

L’[exemple InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/InputInterfacingUWP) illustre le comportement de différents périphériques d’entrée utilisés comme contrôleurs de navigation d’interface utilisateur.

## <a name="see-also"></a>Voir également
[Windows.Gaming.Input.Gamepad][]
[Windows.Gaming.Input.ArcadeStick][]
[Windows.Gaming.Input.RacingWheel][]
[Windows.Gaming.Input.IGameController][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.Gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[Windows.Gaming.Input.Arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[Windows.Gaming.Input.Racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[uinavigationcontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[uinavigationcontrollers]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollers.aspx
[uinavigationcontrolleradded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrolleradded.aspx
[uinavigationcontrollerremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollerremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.getcurrentreading.aspx
[uinavigationreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationreading.aspx
[requireduinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.requireduinavigationbuttons.aspx
[optionaluinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.optionaluinavigationbuttons.aspx
