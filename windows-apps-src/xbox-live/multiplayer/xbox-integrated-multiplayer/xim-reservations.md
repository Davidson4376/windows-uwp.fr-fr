---
title: Réservations d’Out-of-band XIM
description: Décrit comment utiliser Xbox intégré multijoueur (XIM) comme une solution dédiée conversation via les réservations de bande.
ms.assetid: 0ed26d19-defb-414d-a414-c4877bd0ed37
ms.date: 01/28/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, mode multijoueur intégré xbox, xim, chat
ms.localizationpriority: medium
ms.openlocfilehash: 82b38b8d0d94e4cccf501a101faee0e28a6b43c0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660834"
---
# <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>Utilisation du mode XIM en tant que solution de chat dédiée via des réservations hors bande

La plupart des applications utilisent le mode XIM pour traiter tous les aspects du rapprochement des joueurs. Après tout, la mise au point concernant l'assemblage de toutes les fonctionnalités nécessaire à la prise en charge de scénarios multijoueur communs de bout en bout est la raison pour laquelle ce mode est nommé « multijoueur intégré Xbox ». Certaines applications qui implémentent leurs propres solutions multijoueurs à l’aide de serveurs Internet dédiés également convenance les avantages de l’établissement fiable et à faible latence, la communication de faible coût peer-to-peer chat. XIM reconnaît ce besoin et prend en charge un mode d’extension qui tire parti de communication d’homologue simplifiée de XIM pour renforcer la gestion de lecteur externe qui se produisent en dehors de l’API XIM. Au lieu de déplacer dans un réseau XIM via des moyens de réseaux sociaux ou matchmaking, déplacement des lecteurs à l’aide de « réservations », garantie des espaces réservés pour des utilisateurs particuliers qui sont échangés « out-of-band » via le mécanisme de rendezvous lecteur externe de l’application.

À part le processus de déplacement, les réseaux XIM gérés à l’aide de la bande de réservations sont les mêmes comme n’importe quel autre réseau XIM. Toutes les fonctions de communication fonctionnent de façon similaire. Toutefois, matchmaking et méthodes d’API de découverte de réseau social sont nécessairement désactivées pour les réseaux XIM gérés à l’aide des réservations d’out-of-band dans la mesure où ils seraient en conflit avec l’implémentation externe de l’application. Vous ne pouvez pas envoyer des invitations à partir d’un tel réseau XIM, par exemple.

>XIM est optimisé pour fournir une solution de bout en bout simple. Par conséquent, toutes les topologies complexes ou les scénarios peuvent être parfaitement adaptés à out-of-band réservations. Si vous avez des questions sur la nécessité ou comment tirer parti des fonctionnalités de communication de XIM, contactez votre représentant Microsoft.

Les rubriques suivantes décrivent comment tirer parti des réservations d’out-of-band dans XIM. En raison des différences relativement peu de « standard » utilisation XIM décrites dans les sections précédentes, la discussion est abrégée. Vous êtes familiarisé avec [XIM à l’aide de](using-xim.md) est recommandé.


## <a name="moving-to-a-new-out-of-band-reservation-xim-network"></a>Déplacement vers un nouveau réseau XIM de réservation de hors-bande

Pour commencer à utiliser les réservations de bande, un de vos participants collectées doit déplacer dans un nouveau réseau XIM créé dans ce mode. La sélection de quels homologue participante appareil vous revient. Vous disposez peut-être déjà un concept de jeu ou le serveur hôte, qui est un choix naturel pour démarrer le processus, mais cela n’est pas nécessaire. Nous recommandons de choisir un appareil qui signale un type d’accès réseau « Ouvrir » pour obtenir les meilleurs temps du programme d’installation de connectivité. Consultez le `Windows::Networking::XboxLive` documentation de la plateforme pour plus d’informations.

Déplacement vers un XIM réseau géré par le biais des réservations de hors-bande s’effectue par l’initialisation XIM et la déclaration de l’ID d’utilisateur local Xbox prévue comme indiqué dans la procédure pas à pas l’utilisation XIM standard, mais au lieu d’appeler une méthode comme `xim::move_to_new_network()`, appelez `xim::move_to_network_using_out_of_band_reservation()` avec un chaîne de réservation NULL. Exemple :

```cpp
 xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
 xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
 xim::singleton_instance().move_to_network_using_out_of_band_reservation(nullptr);
```

La norme `xim_move_to_network_starting_state_change`, `xim_player_joined_state_change`, et `xim_move_to_network_succeeded_state_change` sera alors fournie au fil du temps lors du traitement les modifications d’état dans le standard `xim::start_processing_state_changes()` et `xim::finish_processing_state_changes()` boucle. Exemple :

```cpp
 uint32_t stateChangeCount;
 xim_state_change_array stateChanges;
 xim::singleton_instance().start_processing_state_changes(&stateChangeCount, &stateChanges);
 for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; stateChangeIndex++)
 {
     const xim_state_change * stateChange = stateChanges[stateChangeIndex];
     switch (stateChange->state_change_type)
     {
         case xim_state_change_type::player_joined:
         {
             MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change *>(stateChange));
             break;
         }

         case xim_state_change_type::player_left:
         {
             MyHandlePlayerLeft(static_cast<const xim_player_left_state_change *>(stateChange));*
             break;
         }

         ...
     }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

Une fois que le périphérique initial a traité les modifications d’état et avait son joueurs correctement déplacé dans le réseau XIM, il doit créer des réservations pour les utilisateurs supplémentaires.

## <a name="adding-players-to-a-xim-network-managed-using-out-of-band-reservations"></a>Ajout de lecteurs à un réseau XIM gérés à l’aide de réservations de hors-bande

Réseaux XIM qui sont gérés à l’aide des réservations d’out-of-band toujours signalent une valeur de `xim_allowed_player_joins::out_of_band_reservation` à partir de la `xim::allowed_player_joins()` méthode ; ils sont fermés à tous les joueurs, sauf ceux qui taches réservées pour leurs ID d’utilisateur de Xbox en appelant `xim::create_out_of_band_reservation()`. `xim::create_out_of_band_reservation()` accepte un tableau d’utilisateurs, vous pouvez donc créer ces réservations pour vos joueurs en externe collectées tous en même temps ou au fil du temps. En outre, les utilisateurs qui ont déjà des joueurs participant au réseau XIM sont ignorés, afin que vous pouvez également fournir les ID utilisateur Xbox supplémentaires comme un jeu de remplacement complet ou delta que les modifications apportées, selon ce qui est pratique. L’exemple suivant part du principe que vous avez déjà votre ensemble entièrement collectée de pointeurs de chaîne d’ID d’utilisateur Xbox dans un tableau variable 'xboxUserIds' avec 'xboxUserIdCount' nombre d’éléments contenus :

```cpp
 xim::singleton_instance().create_out_of_band_reservation(xboxUserIdCount, xboxUserIds);
```

Cela commence le processus asynchrone de la création d’une réservation pour l’ID d’utilisateur Xbox spécifiés. Lorsque l’opération se termine, XIM sera fourni une `xim_create_out_of_band_reservation_completed_state_change` qui signale la réussite ou l’échec. En cas de réussite, une chaîne de réservation sera disponible pour votre système fournir à ces ID d’utilisateur de Xbox fourni à l’opération. Chaînes de réservation créés avec succès sont valides uniquement un certain laps de temps. Cette heure n’est retournée dans `xim_create_out_of_band_reservation_completed_state_change`.

Avec une chaîne de réservation valide, votre mécanisme externe « out-of-band » permettent de recueillir les joueurs en dehors de XIM peut permettre de distribuer la chaîne à ceux énumérés. Par exemple, si vous utilisez le service d’annuaire de Session multijoueur (MPSD) Xbox Live, vous pouvez écrire cette chaîne comme une propriété de session partagée personnalisée dans le document de session (Remarque : la chaîne de réservation contient uniquement un ensemble limité de caractères qui sont toujours utilisé en toute sécurité au format JSON sans aucune séquence d’échappement ou l’encodage Base64).

Une fois que les autres utilisateurs ont récupéré leurs chaînes de réservation de la propriété de document de session partagée, ils peuvent ensuite commencer de déplacement vers le réseau XIM l’utiliser comme paramètre à `xim::move_to_network_using_out_of_band_reservation()`. L’exemple suivant suppose que la chaîne de réservation a été extrait à une variable nommée « reservationString ».

```cpp
xim::singleton_instance().move_to_network_using_out_of_band_reservation(reservationString);

```

Les opérations de déplacement exécute de façon asynchrone tout comme pour le périphérique initial spécifié d’un pointeur null pour la chaîne de réservation. Modifications d’état `xim_move_to_network_starting_state_change`, `xim_player_joined_state_change`, et `xim_move_to_network_succeeded_state_change` sera générée par le déplacement. Lorsque le déplacement a réussi, les lecteurs locaux et distants seront ajoutés. Les appareils existants sur le réseau XIM seront fournis une `xim_player_joined_state_change` pour ces nouveaux lecteurs. À ce stade, communication de conversations vocales et de texte est automatiquement activée les joueurs sur ces différents appareils dans ce réseau XIM (où politique de confidentialité et autorisent).

L’opération de déplacement doit échouer en raison d’une chaîne de réservation non valide, XIM retournera un `xim_network_exited_state_change` avec la raison `xim_network_exit_reason::invalid_reservation`. Cela peut se produire en raison de plusieurs raisons :

1. Le titre tente d’appeler `xim::move_to_network_using_out_of_band_reservation()` sur le périphérique distant avant le `xim_create_out_of_band_reservation_completed_state_change` est retourné sur l’appareil de l’hôte
1. Le titre tente d’appeler `xim::move_to_network_using_out_of_band_reservation()` sur le périphérique distant après l’expiration de la réservation.
1. Le titre tente d’appeler `xim::move_to_network_using_out_of_band_reservation()` sur le périphérique distant plusieurs fois lors de l’appel uniquement `xim::create_out_of_band_reservation()` une fois.

Quelle que soit la réussite ou l’échec, les réservations sont consommées par les opérations de déplacement qui les utilisent. Par conséquent, après chaque tentative d’utilisation, l’hôte doit générer une nouvelle chaîne de réservation en appelant `xim::create_out_of_band_reservation()` à nouveau.

## <a name="configuring-chat-targets-in-a-xim-network-managed-using-out-of-band-reservations"></a>Configuration des cibles de conversation dans un réseau XIM gérés à l’aide de réservations de hors-bande

Réseaux XIM qui sont gérés à l’aide des réservations d’out-of-band se comportent comme des réseaux XIM traditionnels en ce qui concerne la communication de la conversation. Pour prendre en charge sur la concurrence des scénarios, qui vient d’être créé des réseaux XIM sont automatiquement configurés pour prennent uniquement en charge les conversations avec d’autres joueurs qui sont membres de la même équipe ; Autrement dit, la valeur configurée est signalé par `xim::chat_targets()` par défaut est `xim_chat_targets::same_team_index_only`. Cela peut être modifié pour autoriser tout le monde de communiquer avec tout le monde (où politique de confidentialité et autorisent) en appelant `xim::set_chat_targets()`. L’exemple suivant commence la configuration de tout le monde dans le réseau XIM à utiliser un `xim_chat_targets::all_players` valeur :

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

Tous les participants sont informés du fait qu’une nouvelle cible de paramètre en vigueur quand elles sont proposées un `xim_chat_targets_changed_state_change`.

Chaque joueur dans les réseaux XIM gérés à l’aide des réservations d’out-of-band est initialement configuré avec la même valeur d’index team, zéro. Cela signifie une configuration de `xim_chat_targets::same_team_index_only` est indiscernable `xim_chat_targets::all_players` par défaut. Toutefois, vous pouvez modifier les index d’équipe du lecteur local à tout moment en appelant `xim_player::xim_local::set_team_index()`. L’exemple suivant configure un pointeur de lecteur « localPlayer » pour avoir une nouvelle valeur d’index équipe d’un :

```cpp
 localPlayer->local()->set_team_index(1);
```

Tous les appareils sont informés que le joueur a une nouvelle valeur d’index team en vigueur quand elles sont proposées à un xim_player_team_index_changed_state_change pour que le lecteur. Si la conversation cible configuration est actuellement xim_chat_targets::same_team_index_only, autres joueurs avec ce nouvel index équipe même commencera voix de l’audition et proposé conversations textuelles (confidentialité et l’autorisation de stratégie) à partir de la variation versa player et inversement. Les joueurs avec l’ancien index de l’équipe seront arrête échange de communication conversation. Si la conversation cible configuration est actuellement xim_chat_targets::all_players, index de l’équipe n’a aucun impact sur qui peuvent discuter en ligne avec lesquels.

## <a name="removing-players-from-a-xim-network-managed-using-out-of-band-reservations"></a>Suppression de lecteurs à partir d’un réseau XIM gérés à l’aide de réservations de hors-bande

Vous gérez la liste des joueurs en externe lorsque vous utilisez des réservations hors-bande, vous devrez peut-être supprimer les joueurs à partir du réseau XIM naturellement. La manière classique est pour les applications de tirer parti de l’hôte du même jeu qui a été utilisé pour créer le réseau XIM et les réservations suivantes à l’origine pour également gérer la suppression de lecteur et le faire en appelant `xim::kick_player()`. Cela supprime le lecteur spécifié et tous les joueurs sur ce même appareil à partir du réseau XIM. L’exemple suivant suppose que l’application a déterminé qu’il souhaite supprimer le `xim_player` objet vers lequel pointe la variable « playerToRemove » :

```cpp
xim::singleton_instance().kick_player(playerToRemove);
```

Le lecteur applicable (ou des lecteurs) seront fournis les nécessaires `xim_player_left_state_change` pour chaque lecteur et un `xim_network_exited_state_change` indiquant qu’ils ont été exclues à partir du réseau. Chaque joueur peut également appeler `xim::leave_network()` pour quitter sur leur propre pour le même effet.

N’oubliez pas que communication peer-to-peer XIM peut effectuer sa propre détermination à tout moment un lecteur a quitté le réseau XIM en raison de difficultés de l’environnement. Votre application doit être prête à gérer une « non sollicité » `xim_player_left_state_change` et réconcilier toute incohérence entre l’état du XIM et votre schéma de gestion de lecteur externe de façon appropriée pour votre application.

## <a name="cleaning-up-a-xim-network-managed-using-out-of-band-reservations"></a>Nettoyage d’un réseau XIM gérés à l’aide de réservations de hors-bande

Les lecteurs qui ne l’avez pas déjà été exclues à partir de la XIM réseau et pour revenir à l’état comme si seule xim::initialize() et `xim::set_intended_local_xbox_user_ids()` avait été appelée, peut commencer à l’aide de cette opération le `xim::leave_network()` méthode :

```cpp
xim::singleton_instance().leave_network();
```

Cela commence la déconnexion de façon asynchrone à partir d’autres participants. Ainsi, les appareils à distance à fournir un `xim_player_left_state_change` pour les joueurs local et l’appareil local seront fourni une `xim_player_left_state_change` pour chaque joueur, local ou distant. Lorsque tous les déconnexion sans perte de données terminée, une dernière `xim_network_exited_state_change` sera fourni. L’application peut ensuite appeler `xim::cleanup()` pour libérer toutes les ressources et revenir à l’état non initialisé :

```cpp
 xim::singleton_instance().cleanup();
```

Appel `xim::leave_network()` et attend le `xim_network_exited_state_change` afin de quitter un XIM réseau normalement est toujours recommandé quand un `xim_network_exited_state_change` n’a pas déjà été fourni. Appel `xim::cleanup()` directement peut provoquer des problèmes de performances de communication pour les autres participants pendant qu’ils êtes obligés de délai d’attente des messages à l’appareil simplement « disparu ».
