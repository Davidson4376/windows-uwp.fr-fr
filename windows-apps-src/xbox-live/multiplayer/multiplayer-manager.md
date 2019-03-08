---
title: Gestionnaire de mode multijoueur
description: En savoir plus sur Xbox Live multijoueur manager, une API de haut niveau conçue pour le rendre plus facile à implémenter multijoueurs.
ms.assetid: f3a6c8bc-4f73-4b99-ac51-aadee73c8cfa
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 99159b5d126c671b07d37e20f1bcd61452c7d670
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594714"
---
# <a name="multiplayer-manager"></a>Gestionnaire de mode multijoueur

Xbox Live fournit prise en charge complète pour l’ajout de fonctionnalités multijoueur à vos titres, ce qui permet de votre jeu connecter des membres de Xbox Live dans le monde entier.  Cela inclut des scénarios de matchmaking riche, la possibilité pour un lecteur à rejoindre le jeu d’un ami en cours et bien plus encore. Toutefois, l’implémentation de Xbox Live multijoueur en utilisant les API de 2015 multijoueurs directement peut être une tâche complexe, nécessitant un grand degré de conception et de test pour vérifier que vous suivez les meilleures pratiques et répondant aux exigences de certification.

Mode multijoueur Manager rend facile d’ajouter des fonctionnalités multijoueur à votre jeu grâce à la gestion des sessions et matchmaking et en fournissant un état et modèle de programmation basé sur l’événement. Il est un ensemble d’API conçues pour le rendre facile à implémenter des scénarios multijoueurs pour votre Xbox Live game. Il fournit une API qui est axée sur des scénarios multijoueurs courantes telles que la lecture des jeux multijoueurs avec vos amis, jeu de gestion des invitations, jointure de gestion en cours, matchmaking et bien plus encore. Il prend en charge de plusieurs utilisateurs locaux et plus facilement votre titre à intégrer à l’annuaire de sessions multijoueur si vous utilisez un service de matchmaking tiers. La plupart de ces scénarios peuvent être effectuées avec seulement quelques appels d’API.

## <a name="key-features"></a>Principales fonctionnalités
Voici les principales fonctionnalités de l’API du Gestionnaire de mode multijoueur :

* Gestion des sessions facile et Xbox Live Matchmaking
* État et les événements en fonction de modèle de programmation.
* Garantit les meilleures pratiques de Xbox Live, ainsi que XR multijoueurs conforme.
* Prend en charge les titres XDK une Xbox et UWP.
* Implémente [mode multijoueur 2015 organigrammes](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Xbox%20One%20Multiplayer%202015%20Developer%20Flowcharts.aspx).
* Fonctionne avec les API de 2015 multijoueurs traditionnel.

>**Important** -votre jeu doit toujours implémenter les événements requis pour le mode multijoueur en ligne afin de passer la certification.

## <a name="overview"></a>Vue d’ensemble
Le Gestionnaire de mode multijoueur est axé sur les quelques concepts clés :
* `lobby_session` : Une session persistante utilisée pour la gestion des utilisateurs locaux de cet appareil et vos amis invités qui jouer ensemble. Le groupe peut jouer plusieurs essais, cartes, niveaux, etc., du jeu, et la session d’introduction effectue le suivi de ce groupe core d’amis (y compris les lecteurs locaux sur l’appareil). En règle générale, ce groupe est formé tandis que l’hôte peut accéder via les menus, discuter avec les membres du groupe pour décider quel mode de jeu de lecture.

* `game_session` : Pistes joueurs jouant à une instance spécifique du jeu. Par exemple, une concurrence, carte ou niveau. Vous pouvez créer une nouvelle session de jeu via `join_game_from_lobby` qui inclut les membres dans la session d’introduction.  Quand un membre accepte une invitation, ils seront ajoutés à la salle d’attente et de la session de jeu (si l’espace est). Lecteurs supplémentaires peuvent être ajoutés à la session de jeu si matchmaking est activé, mais ces lecteurs supplémentaires ne sont pas ajoutés à la session d’introduction. Cela signifie qu’une fois le jeu se termine, les joueurs dans la session d’introduction sont de toujours ensemble, tandis que les lecteurs supplémentaires à partir de matchmaking ne sont pas.

* `multiplayer_member` : Représente un utilisateur connecté à un périphérique local ou distant.

* `do_work` : Garantit des mises à jour de l’état du jeu appropriées sont conservées entre le titre et le Service de mode multijoueur Xbox Live. Pour garantir des performances optimales, do_work() doit être appelée fréquemment, par exemple une fois par trame. Il vous fournit une liste de `multiplayer_event` événements de rappel pour le jeu à gérer.

## <a name="state-machine"></a>Ordinateur d’état
Le `do_work()` appel n’est nécessaire pour garantir votre état est actualisée.  Pour le Gestionnaire de mode multijoueur pour faire son travail, les développeurs doivent appeler la `do_work()` méthode régulièrement. Le moyen le plus fiable pour ce faire consiste à appeler au moins une fois par trame. `do_work()` Retourne rapidement lorsqu’il n’a aucun travail à faire, il y a donc aucune raison de préoccupation sur l’appel de trop souvent.

Tous les objets retournés par l’API du Gestionnaire de mode multijoueur ne doivent pas être considérées thread-safe. Toutefois, il vous offre un contrôle pour la synchronisation des threads si vous l’appelez à partir de plusieurs threads. La bibliothèque a protection multithreading interne, mais vous devez toujours implémenter votre propre verrouillage si vous avez besoin d’un seul thread à accéder à toutes les valeurs – par exemple, parcourir la liste de members() tandis que l’appel d’un autre thread peut être `do_work()`.

Mode multijoueur Manager tient à jour un modèle état en fonction de la mise à jour les sessions en arrière-plan comme lecteurs joint, conservez ou sont des sessions de mise à jour. Afin d’éviter les problèmes de synchronisation de thread entre le thread d’interface utilisateur et le thread de votre jeu, le Gestionnaire de mode multijoueur n’actualise pas l’état visible application des sessions jusqu'à ce que vous appeliez la `do_work()` (méthode). En règle générale vous serez recevoir des notifications concernant des événements tels que les modifications de session sur un thread d’arrière-plan, puis pour le synchroniser avec un thread d’interface utilisateur pour afficher ces modifications. Avec le Gestionnaire de mode multijoueur, ce travail est effectué pour vous en arrière-plan.  Vous pouvez appeler `do_work()` sur votre thread principal au moment de votre choix, pour obtenir la dernière capture instantanée de l’état quel mode multijoueur Manager est mise en mémoire tampon pour vous en arrière-plan.

## <a name="events-and-notifications"></a>Événements et Notifications
Mode multijoueur Manager définit un ensemble d’événements significatifs (consultez `multiplayer_event_type`) et informe le titre par le biais de la `do_work()` méthode lorsqu’elles surviennent. Événements tels que les lecteurs à distance joignant ou quittant, modification des propriétés de membre ou de modification de l’état de session. Toutes les API du Gestionnaire de mode multijoueur sont synchrones. Le `do_work()` méthode retourne une liste des événements lors de ces opérations asynchrones sont terminées. Le titre doit gérer ces événements si nécessaire. Veuillez consulter la `multiplayer_event` documentation pour plus de détails de classe.

Pour chaque événement, selon le type d’événement, vous devez caster la `event_args` à la classe d’arg événement approprié pour obtenir les propriétés de l’événement. L’exemple suivant montre comment utiliser `do_work()` pour gérer les événements :

```cpp
auto eventQueue = mpInstance.do_work();
for (auto& event : eventQueue)
{
    switch (event.event_type())
    {
      case multiplayer_event_type::member_joined:
      {
        auto memberJoinedArgs =  std::dynamic_pointer_cast<member_joined_event_args>(event.event_args());
        HandleMemberJoined(memberJoinedArgs);
        break;
      }
      case multiplayer_event_type::session_property_changed:
      {
        auto sessionPropertyChangedArgs =  std::dynamic_pointer_cast<session_property_changed_event_args>(event.event_args());
        HandlePropertiesChanged(sessionPropertyChangedArgs);
        break;
      }
      . . .
    }
}

```

## <a name="scenarios"></a>Scénarios

Dans cette section, nous allons examiner quelques scénarios courants et les API vous appelleriez dans chaque scénario.  Des informations sur ce que fait le Gestionnaire de mode multijoueur en arrière-plan sont également fournies.

* [Jouer avec vos amis](multiplayer-manager/play-multiplayer-with-friends.md)
* [Rechercher une correspondance](multiplayer-manager/play-multiplayer-with-matchmaking.md)
* [Envoyer des invitations de jeu](multiplayer-manager/send-game-invites.md)
* [Gérer l’activation du protocole](multiplayer-manager/handle-protocol-activation.md)

Vous trouverez une vue d’ensemble détaillée de l’API dans [vue d’ensemble de l’API du Gestionnaire de mode multijoueur](multiplayer-manager/multiplayer-manager-api-overview.md).

## <a name="what-multiplayer-manager-does-not-do"></a>Ce que le Gestionnaire de mode multijoueur ne
Alors que le mode multijoueur Manager rend beaucoup plus facile à implémenter des scénarios multijoueurs et effectue l’abstraction des données par rapport au développeur, il existe quelques éléments, il ne gère pas ou n’est pas idéale pour.

* Jeux de serveur en ligne persistant, telles que les jeux massivement multijoueurs, ou d’autres types de jeux qui nécessitent des sessions volumineuses (plus de 100 lecteurs dans une session).
* Serveur de gestion de session de serveur

>Mode multijoueur Manager n’est pas liée à une technologie de réseau spécifique et devraient fonctionner avec n’importe quelle couche réseau.

## <a name="next-steps"></a>Étapes suivantes

Veuillez consulter le C++ ou WinRT *mode multijoueur* exemple pour obtenir un exemple de l’API.

Vous trouverez la documentation des API dans les guides de C++ ou WinRT dans l’espace de noms Microsoft::Xbox::Services::Multiplayer::Manager.  Vous pouvez également voir le `multiplayer_manager.h` en-tête.

Si vous avez des questions, commentaires, ou que vous rencontrez des problèmes à l’aide du Gestionnaire de mode multijoueur, contactez votre mère ou publier un thread de prise en charge sur les forums au [ https://forums.xboxlive.com ](https://forums.xboxlive.com).
