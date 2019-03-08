---
title: Gérer l’activation de protocole
description: Découvrez comment utiliser Xbox Live multijoueurs pour gérer l’activation du protocole.
ms.assetid: e514bcb8-4302-4eeb-8c5b-176e23f3929f
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, manager multijoueur, l’activation de protocole
ms.localizationpriority: medium
ms.openlocfilehash: 0b5dead742e18bbf5f3e9c271109352ae48e8fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655834"
---
# <a name="handle-protocol-activation"></a>Gérer l’activation de protocole

L’activation de protocole est lorsque le système démarre automatiquement un jeu en réponse à une autre action, en général, lorsqu’un joueur accepte une invitation à un jeu à partir d’un autre lecteur.

Votre titre peut obtenir le protocole activé des manières suivantes :

* Lorsqu’un utilisateur accepte une invitation à un jeu
* Lorsqu’un utilisateur sélectionne « Joindre Game » à partir de gamercard d’un joueur.

Ce scénario explique comment gérer l’activation de protocole lors du lancement de votre titre et joindre la salle d’attente et le jeu (le cas échéant).

Vous pouvez voir un organigramme du processus ici : [Organigramme - lecteur de l’activation de protocole Handle](mpm-flowcharts/mpm-on-protocol-activation.md).

| Méthode | Événement déclenché |
| -----|----------------|
| `multiplayer_manager::join_lobby(IProtocolActivatedEventArgs^ args, User^)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Lorsqu’un joueur accepte une invitation à un jeu ou rejoint jeu d’un ami via les gamercard du joueur, le jeu est lancé sur leur appareil à l’aide de l’activation de protocole. Une fois le jeu commence, multijoueur Manager permettent les arguments d’événement de protocole activé pour joindre la salle d’attente. Si vous le souhaitez, si vous n’avez pas ajouté les utilisateurs locaux via `lobby_session()::add_local_user()`, vous pouvez passer dans la liste des utilisateurs via le `join_lobby()` API. Si l’utilisateur invité n’est pas ajouté, ou si l’invitation a été pour un autre utilisateur que l’utilisateur qui a été ajouté, `join_lobby()` échoue et fournir le `invited_xbox_user_id()` envoyé l’invitation pour dans le cadre de la `join_lobby_completed_event_args`.

Après avoir rejoint la salle d’attente, Microsoft recommande de définir l’adresse de connexion locale, ainsi que toutes les propriétés personnalisées pour le membre. Vous pouvez également définir l’hôte via `set_synchronized_host` s’il n’existe.

Enfin, le Gestionnaire de mode multijoueur sont automatiquement l’utilisateur dans la session de jeu de jointure, si un jeu est déjà en cours d’exécution et a suffisamment de place pour l’invité. Le titre sera notifié via la `join_game_completed` événement en fournissant un code d’erreur approprié et le message.

**Exemple :**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args, users);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(),
    connectionAddress);
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
