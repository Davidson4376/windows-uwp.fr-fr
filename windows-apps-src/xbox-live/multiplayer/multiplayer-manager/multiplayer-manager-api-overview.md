---
title: Vue d’ensemble de l’API du gestionnaire multijoueurs
description: En savoir plus sur l’API de gestionnaire de mode multijoueur Xbox Live.
ms.assetid: 658babf5-d43e-4f5d-a5c5-18c08fe69a66
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, multijoueurs, manager multijoueur
ms.localizationpriority: medium
ms.openlocfilehash: 7838de6845bc6c49acf649c0e859cd0d7020490f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628024"
---
# <a name="multiplayer-manager-api-overview"></a>Vue d’ensemble de l’API du Gestionnaire de mode multijoueur

Cette page décrit une vue d’ensemble de l’API du Gestionnaire de mode multijoueur, et comment elles sont utilisées dans un jeu. Il appelle les classes les plus importantes et les méthodes dans l’API. Pour plus d’informations API, consultez la documentation de référence. Pour obtenir des exemples montrant comment utiliser ces API dans une application, consultez l’exemple de mode multijoueur.

## <a name="namespace"></a>Espace de noms
Les classes de gestionnaire de mode multijoueur sont inclus l’espace de noms suivant :

| language | espace de noms |
| --- | --- |
| C++/CX | Microsoft::Xbox::Services::Multiplayer::Manager |
| C++ | xbox::services::multiplayer::manager |

Il suivantes listes décrit les principales classes que vous devez connaître :

* [Classe de gestionnaire de mode multijoueur](#multiplayer-manager-class)
* [Classe d’événements du mode multijoueur](#multiplayer-event-class)
* [Classe de membre de mode multijoueur](#multiplayer-member-class)
* [Classe de la Session d’introduction de mode multijoueur](#multiplayer-lobby-session-class)
* [Classe de la Session de jeu multijoueur](#multiplayer-game-session-class)

## <a name="multiplayer-manager-class-a-namemultiplayer-manager-class"></a>Classe de gestionnaire de mode multijoueur <a name="multiplayer-manager-class">

| language | classe |
| --- | --- |
| C++/CX | MultiplayerManager |
| C++ | multiplayer_manager |

Cette classe est la classe principale d’interaction avec le Gestionnaire de mode multijoueur. Il est une classe singleton, vous devez uniquement créer et initialiser la classe qu’une seule fois.
Cette classe contient un objet de session d’introduction unique et un objet de session de jeu unique.

Au minimum, vous devez appeler la `initialize()` et `do_work()` méthodes sur cette classe dans l’ordre pour le Gestionnaire de mode multijoueur de la fonction.

Les tableaux suivants décrivent certains, mais pas tous, les plus couramment utilisé des méthodes et propriétés pour cette classe. Pour une liste descriptive complète des membres de classe, consultez la référence.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Méthodes** | | |
| Initialize() | initialize() | Initialise le Gestionnaire de mode multijoueur. Vous devez appeler cette méthode avant d’utiliser le mode multijoueur Manager. |
| DoWork() | do_work() | Met à jour les États de session visible de l’application. Vous devez appeler cette méthode au moins une fois par frame, et votre jeu doit gérer les événements multijoueurs qui sont retournés par la méthode. |
| JoinLobby() | join_lobby() | Fournit un moyen de jointures une session de salle d’attente d’un ami via un handleId qui identifie de façon unique la salle d’attente vous voulez joindre ou lorsque l’utilisateur accepte une invitation qui provoque le titre obtenir le protocole activé. |
| JoinGameFromLobby() | join_game_from_lobby() | Rejoindre la session de jeu de la salle d’attente s’il en existe et si l’espace. Si la session n’existe pas, il crée une nouvelle session de jeu avec les membres existants de la salle d’attente. Cela ne migre pas les propriétés de la session d’introduction existantes sur à la session de jeu. Après avoir joint, vous pouvez définir les propriétés ou l’hôte via `game_session()::set_synchronized_*` API. Le titre est nécessaire pour appeler cette API sur tous les clients qui souhaitent rejoindre la session de jeu.|
| JoinGame() | join_game() | Joint une session de jeu existante, un nom de session global unique, que qui se trouvent généralement par le biais des matchmaking services tiers. Vous pouvez passer dans la liste des ID que vous souhaitez faire partie du jeu d’utilisateur de xbox.|
| FindMatch() | find_match() | Utilisez matchmaking Xbox Live pour trouver et joindre un jeu. |
| LeaveGame() | leave_game() | Laisse un jeu et retourne le membre et tous les membres locaux à la salle d’attente. |
| **Propriétés** | | |
| LobbySession | lobby_session() | Handle vers un objet qui représente la session d’introduction. |
| GameSession |  game_session() | Handle vers un objet qui représente la session de jeu. |

## <a name="multiplayer-event-class-a-namemultiplayer-event-class"></a>Classe d’événements du mode multijoueur <a name="multiplayer-event-class">

| language | classe |
| --- | --- |
| C++/CX | MultiplayerEvent |
| C++ | multiplayer_event |

Lorsque vous appelez `do_work()`, mode multijoueur Manager renvoie une liste des événements qui représentent les modifications apportées à ces sessions depuis le dernier appel `do_work()`. Ces événements incluent des modifications telles que le membre a rejoint une session, un membre a quitté une session, les propriétés de membre ont été modifiées, le client de l’hôte a changé, etc.

Pour obtenir la liste de tous les types d’événements possibles, consultez le `multiplayer_event_type` énumération.

Chaque renvoyé `multiplayer_event` inclut un `event_args`, lequel vous devez effectuer un cast de la classe event_args approprié pour le type d’événement. Par exemple, si le `event_type` est `member_joined`, puis vous castez le `event_args` font référence à un `member_joined_event_args` classe.

Votre jeu doit gérer chacun des événements en fonction des besoins après avoir appelé `do_work()`.

## <a name="multiplayer-member-class-a-namemultiplayer-member-class"></a>Classe de membre de mode multijoueur <a name="multiplayer-member-class">

| language | classe |
| --- | --- |
| C++/CX | MultiplayerMember |
| C++ | multiplayer_member |

Cette classe représente un lecteur dans une salle d’attente ou une session de jeu. La classe contient des propriétés relatives au membre, y compris le XboxUserID pour le lecteur, l’adresse de connexion de réseau pour le joueur, des propriétés personnalisées pour chaque lecteur et bien plus encore.

## <a name="multiplayer-lobby-session-class-a-namemultiplayer-lobby-session-class"></a>Classe de la Session d’introduction de mode multijoueur <a name="multiplayer-lobby-session-class">

| language | classe |
| --- | --- |
| C++/CX | MultiplayerLobbySession |
| C++ | multiplayer_lobby_session |

Une session persistante utilisée pour la gestion des utilisateurs locaux de cet appareil et vos amis invités qui jouer ensemble. Une session d’introduction doit contenir au moins un membre pour le Gestionnaire de mode multijoueur multijoueur rien à faire. Vous pouvez initialement créer une nouvelle session d’introduction en appelant le `add_local_user()` (méthode).

Les tableaux suivants décrivent certains, mais pas tous, les plus couramment utilisé des méthodes et propriétés pour cette classe. Pour une liste descriptive complète des membres de classe, consultez la référence.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Méthodes** | | |
| AddLocalUser() | add_local_user() | Ajoute un utilisateur local (un lecteur qui a signé sur l’appareil local) à la session d’introduction. Si c’est le premier membre ajouté à une session d’introduction, il crée ensuite une nouvelle session d’introduction. |
| RemoveLocalUser() | remove_local_user() | Supprime un membre spécifié de la salle d’attente et la session de jeu. |
| InviteFriends() | invite_friends() | Ouvre une Xbox Live interface utilisateur standard qui permet au joueur de sélectionner des personnes dans leur liste d’amis et puis invite ces lecteurs pour le jeu. |
| InviteUsers() | invite_users() | Invite les utilisateurs de Xbox Live sepcified au jeu. |
| SetLocalMemberConnectionAddress() | set_local_member_connection_address() | Définit l’adresse réseau pour le membre local. Jeux peuvent utiliser cette adresse de réseau pour établir des communications réseau entre les membres. |
| SetLocalMemberProperties() | set_local_member_properties() | Définit une propriété personnalisée pour un membre local. La propriété est stockée dans une chaîne JSON. |
| DeleteLocalMemberProperties() | delete_local_member_properties() | Supprime une propriété personnalisée pour un membre local. |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | Définit une propriété personnalisée pour la session d’introduction. La propriété est stockée dans une chaîne JSON. Si la propriété est partagée entre les appareils et peut-être être mises à jour par plusieurs périphériques en même temps, utilisez la version synchronisée de la méthode. |
| IsHost() | is_host() | Indique si l’appareil en cours est agissant en tant que l’hôte de la salle d’attente. |
| SetSynchronizedHost() | set_synchronized_host() | Définit l’hôte de la salle d’attente. |
| **Propriétés** | | |
| LocalMembers | local_members() | La collection des membres qui sont connectés sur l’appareil local. |
| Membres | members() | La collection des membres qui se trouvent dans la session d’introduction. |
| Propriétés | properties() | Un objet JSON qui représente une collection de propriétés pour la session d’introduction. |
| Host | host() | Le membre de l’hôte pour la salle d’attente. |


## <a name="multiplayer-game-session-class-a-namemultiplayer-game-session-class"></a>Classe de la Session de jeu multijoueur <a name="multiplayer-game-session-class">

| language | classe |
| --- | --- |
| C++/CX | MultiplayerGameSession |
| C++ | multiplayer_game_session |

La session de jeu représente le groupe de membres Xbox Live qui participent à une instance de jeu réel. Cela peut inclure des lecteurs qui ont été mis en correspondance via les services de matchmaking.

Pour démarrer une nouvelle session de jeu qui inclut des membres de la `lobby_session`, vous pouvez appeler `multiplayer_manager::join_game_from_lobby()`. Si vous souhaitez utiliser matchmaking Xbox Live, vous pouvez appeler `multiplayer_manager::find_match()`. Si vous utilisez un service de matchmaking tiers, vous pouvez appeler `multiplayer_manager::join_game()`.

Les tableaux suivants décrivent certains, mais pas tous, les plus couramment utilisé des méthodes et propriétés pour cette classe. Pour une liste descriptive complète des membres de classe, consultez la référence.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Méthodes** | | |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | Définit une propriété personnalisée pour la session de jeu. La propriété est stockée dans une chaîne JSON. Si la propriété est partagée entre les appareils et peut-être être mises à jour par plusieurs périphériques en même temps, utilisez la version synchronisée de la méthode. |
| IsHost() | is_host() | Indique si l’appareil en cours est agissant en tant que l’hôte du jeu. |
| SetSynchronizedHost() | set_synchronized_host() | Définit l’hôte du jeu. |
| **Propriétés** | | |
| Membres | members() | La collection des membres qui se trouvent dans la session de jeu. |
| Propriétés | properties() | Un objet JSON qui représente une collection de propriétés pour la session de jeu. |
| Host | host() | Le membre de l’hôte pour le jeu. |
