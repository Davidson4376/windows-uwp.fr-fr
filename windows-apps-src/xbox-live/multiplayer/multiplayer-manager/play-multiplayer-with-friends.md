---
title: Lire un jeu multijoueur avec des amis
description: Découvrez comment utiliser Xbox Live multijoueurs pour permettre à un lecteur multijoueur avec vos amis.
ms.assetid: 6eefee0e-6c0d-473a-97e7-f3e45f574712
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, multijoueurs, manager multijoueur, diagramme de flux
ms.localizationpriority: medium
ms.openlocfilehash: d0d946cdbd29fb51d98c8febb52c5855f948b4ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659964"
---
# <a name="play-a-multiplayer-game-with-friends"></a>Lire un jeu multijoueur avec des amis

L’un des scénarios multijoueurs plus simples consiste à autoriser passionné lire votre jeu en ligne avec vos amis. Ce scénario couvre les étapes de base que vous devez implémenter à l’aide du Gestionnaire de mode multijoueur.

## <a name="inviting-friends"></a>Inviter des amis

Il existe quatre étapes impliqués lorsque vous utilisez le Gestionnaire de mode multijoueur pour envoyer une invitation à ami de l’utilisateur, puis pour cet ami à participer à la partie en cours d’exécution :

1. [Initialiser le Gestionnaire de mode multijoueur](#initialize-multiplayer-manager)
2. [Créer la session d’introduction en ajoutant des utilisateurs locaux](#create-lobby)
3. [Envoyer des invitations à vos amis](#send-invites)
4. [Accepter les invitations](#accept-invites)
5. [Rejoindre une session de jeu à partir de la salle d’attente](#join-game)

Les étapes 1, 2, 3 et 5 sont effectuées sur l’appareil réalisant l’invitation.  Étape 4 est initialisé en général sur l’ordinateur de l’invité après le lancement de l’application par le biais de l’activation de protocole.

Vous pouvez voir un organigramme du processus ici : [Organigramme - Play un jeu multijoueur/co-op avec des amis](mpm-flowcharts/mpm-play-with-friends.md).

### <a name="1-initialize-multiplayer-manager-a-nameinitialize-multiplayer-manager"></a>(1) initialiser le Gestionnaire de mode multijoueur <a name="initialize-multiplayer-manager">

| Appel | Événement déclenché |
|-----|----------------|
| `multiplayer_manager::initialize(lobbySessionTemplateName)` | Non applicable |

L’objet de session d’introduction est automatiquement créé lors de l’initialisation du Gestionnaire de jeux multijoueurs, en supposant qu’un nom de modèle de session valide (configuré dans la configuration du service) est spécifié. Notez que cela ne crée pas de l’instance de la session d’introduction sur le service.

**Exemple :**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

mpInstance->initialize(lobbySessionTemplateName);
```


### <a name="2-create-the-lobby-session-by-adding-local-usersa-namecreate-lobby"></a>(2) de créer la session d’introduction en ajoutant des utilisateurs locaux<a name="create-lobby">

| Méthode | Événement déclenché |
|-----|----------------|
| `multiplayer_lobby_session::add_local_user()` | `user_added_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Ici, vous ajoutez localement signé Xbox Live pour les utilisateurs à la session d’introduction. Cela héberge une nouvelle salle d’attente lorsque le premier utilisateur est ajouté. Pour tous les autres utilisateurs, ils seront ajoutés à la salle d’attente existante en tant qu’utilisateurs secondaire. Cette API sera également publier la salle d’attente dans l’interpréteur de commandes pour vos amis à joindre. Vous pouvez envoyer des invitations, définir les propriétés de la salle d’attente, accéder aux membres de salle d’attente via lobby() uniquement une fois que vous avez ajouté l’utilisateur local.

Après avoir rejoint la salle d’attente, Microsoft recommande de définir l’adresse de connexion locale, ainsi que toutes les propriétés personnalisées pour le membre.

Vous devez répéter ce processus pour tous les utilisateurs connectés localement.

**Exemple : (utilisateur local)**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

auto result = mpInstance->lobby_session()->add_local_user(xboxLiveContext->user());
if (result.err())
{
  // handle error
}

// Set member connection address
string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(),
    connectionAddress);

// Set custom member properties
mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
```

**Exemple : (plusieurs utilisateurs locales)**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();
string_t connectionAddress = L"1.1.1.1";

for (User^ user : User::Users)
{
  if( user->IsSignedIn )
  {
    auto result = mpInstance.lobby_session()->add_local_user(user);
    if (result.err())
    {
      // handle error
    }

    // Set member connection address
    mpInstance->lobby_session()->set_local_member_connection_address(
        xboxLiveContext->user(), connectionAddress);

    // Set custom member properties
    mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
  }
}

```


Les modifications sont traités par lot sur la prochaine `do_work()` appeler.  
Se déclenche manager multijoueur un `user_added` événement chaque fois qu’un utilisateur est ajouté à la session d’introduction. Il est recommandé que vous examinez le code d’erreur de l’événement pour voir si cet utilisateur a été ajouté avec succès. En cas de défaillance, un message d’erreur fournira détaillant les raisons de l’échec.

**Fonctions effectuées par le Gestionnaire de mode multijoueur**

* Inscrire des activité en temps réel et des abonnements de mode multijoueur avec le service de multijoueur avec Xbox Live
* Créer une Session de la salle d’attente
* Joindre tous les lecteurs locaux comme active
* Télécharger SDA
* Définir les propriétés de membre
* Enregistrer les événements de modification de Session
* Définir la Session de la salle d’attente comme Session Active

### <a name="3-send-invites-to-friends-a-namesend-invites"></a>(3) envoi convie à vos amis <a name="send-invites">

| Méthode | Événement déclenché |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

Ensuite, vous souhaitez faire apparaître l’UI Xbox standard pour invitant vos amis. Cela affiche une interface utilisateur qui permet le joueur pour sélectionner des amis ou des lecteurs récents à inviter au jeu. Une fois que le nombre d’accès lecteur confirmer, mode multijoueur Manager envoie les invitations aux joueurs sélectionnés.

Jeux peuvent également utiliser le `invite_users()` méthode pour envoyer des invitations à un ensemble de personnes définis par leur ID utilisateur de Xbox Live. Cela est utile si vous préférez utiliser votre propre interface utilisateur dans le jeu au lieu de la cotation Xbox UI.

**Exemple :**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**Fonctions effectuées par le Gestionnaire de mode multijoueur**

* Apporte de la Xbox stock titre pouvant être appelé par l’interface utilisateur (TCUI)
* Invitation envoie directement aux joueurs sélectionnés

### <a name="4-accept-invites-a-nameaccept-invites"></a>(4) accepter des invitations <a name="accept-invites">

| Méthode | Événement déclenché |
| -----|----------------|
| `multiplayer_manager::join_lobby(Windows::ApplicationModel::Activation::IProtocolActivatedEventArgs^ args)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Lorsqu’un joueur invité accepte une invitation à un jeu ou joint une partie de vos amis via une interface utilisateur de l’interpréteur de commandes, le jeu est lancé sur leur appareil à l’aide de l’activation de protocole. Une fois le jeu commence, multijoueur Manager permettent les arguments d’événement de protocole activé pour joindre la salle d’attente. Si vous le souhaitez, si vous n’avez pas ajouté les utilisateurs locaux via lobby_session()::add_local_user(), vous pouvez passer dans la liste des utilisateurs via l’API join_lobby(). Si l’utilisateur invité n’est pas ajouté soit via lobby_session()::add_local_user() join_lobby(), puis join_lobby() échoue et fournir le invited_xbox_user_id() l’invitation a été envoyée pour le cadre de la join_lobby_completed_event_args().

Après avoir rejoint la salle d’attente, Microsoft recommande de définir l’adresse de connexion locale, ainsi que toutes les propriétés personnalisées pour le membre. Vous pouvez également définir l’hôte via set_synchronized_host s’il n’existe.

Enfin, le Gestionnaire de mode multijoueur sont automatiquement l’utilisateur dans la session de jeu de jointure, si un jeu est déjà en cours d’exécution et a suffisamment de place pour l’invité. Le titre est informé par l’événement join_game_completed en fournissant un code d’erreur approprié et le message.

**Exemple :**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);
```

Erreur ou de réussite est gérée la `join_lobby_completed` événement

**Fonctions effectuées par le Gestionnaire de mode multijoueur**

* Inscrire RTA & abonnements multijoueurs
* Rejoindre la session de la salle d’attente
 * Nettoyage de l’état existant d’introduction
 * Joindre tous les lecteurs locaux comme active
 * Télécharger SDA
 * Définir les propriétés de membre
* Enregistrer les événements de modification de Session
* Définir la Session de la salle d’attente comme Session Active
* Rejoindre la Session de jeu (s’il existe)
 * Handle de transfert utilise

### <a name="5-join-a-game-session-from-the-lobby-a-namejoin-game"></a>(5) rejoindre une session de jeu à partir de la salle d’attente <a name="join-game">

| Méthode | Événement déclenché |
|-----|----------------|
| `multiplayer_manager::join_game_from_lobby()` | `join_game_completed_event` |

Une fois les invitations ont été acceptées, et l’hôte est prêt à commencer à jouer au jeu, vous pouvez démarrer un nouveau jeu qui inclut les membres de la session d’introduction en appelant `join_game_from_lobby()`.

**Exemple :**

```cpp
auto result = mpInstance.join_game_from_lobby();
if (result.err())
{
  // handle error
}
```

Erreur ou de réussite est gérée par le biais de `join_game_completed` événement

**Fonctions effectuées par le Gestionnaire de mode multijoueur**

* Créer la Session de jeu
 * Joindre tous les lecteurs locaux comme active
 * Télécharger SDA
 * Définir les propriétés de membre
* Enregistrer les événements de modification de Session
* Publier le jeu via une Session de la salle d’attente
