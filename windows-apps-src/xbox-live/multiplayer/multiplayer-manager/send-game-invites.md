---
title: Envoyer des invitations à des jeux
description: Découvrez comment utiliser Xbox Live multijoueurs pour permettre à un lecteur d’envoyer des invitations de jeu.
ms.assetid: 8b9a98af-fb78-431b-9a2a-876168e2fd76
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, multijoueurs, manager multijoueur, organigramme, invitation à un jeu
ms.localizationpriority: medium
ms.openlocfilehash: 85aa45558d1443638ba7dd50dbea8923125ef664
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645994"
---
# <a name="send-game-invites"></a>Envoyer des invitations à des jeux

L’un des scénarios multijoueurs plus simples consiste à autoriser passionné lire votre jeu en ligne avec vos amis. Ce scénario décrit les étapes que vous avez besoin pour envoyer des invitations à un autre lecteur pour joindre votre jeu.

Une fois que vous avez [initialisé le Gestionnaire de mode multijoueur](play-multiplayer-with-friends.md)et avez [créé une session d’introduction en ajoutant des utilisateurs locaux](play-multiplayer-with-friends.md), vous devez attendre jusqu'à ce que vous recevez le `user_added` événement avant que vous pouvez commencer à envoyer des invitations.

Il existe deux façons d’envoyer une invitation :

1. [Plateforme de Xbox invitation TCUI](#xbox-platform-invite-tcui)
2. [Titre implémenté l’interface utilisateur personnalisée](#title-implemented-custom-ui)

Vous pouvez voir un organigramme du processus ici : [Organigramme - envoyer une Invitation à un autre lecteur](mpm-flowcharts/mpm-send-invites.md).

### <a name="1-xbox-platform-invite-tcui-a-namexbox-platform-invite-tcui"></a>1) plateforme de Xbox invitation TCUI <a name="xbox-platform-invite-tcui">

| Méthode | Événement déclenché |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |

Appel `invite_friends()` fait apparaître l’UI Xbox standard pour invitant vos amis. Cela affiche une interface utilisateur qui permet le joueur pour sélectionner des amis ou des lecteurs récents à inviter au jeu. Une fois que le nombre d’accès lecteur confirmer, mode multijoueur Manager envoie les invitations aux joueurs sélectionnés.

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

### <a name="2-title-implemented-custom-uia-nametitle-implemented-custom-ui"></a>2) titre implémenté l’interface utilisateur personnalisée<a name="title-implemented-custom-ui">

| Méthode | Événement déclenché |
|-----|----------------|
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

Votre titre peut implémenter un TCUI personnalisé pour l’affichage des amis en ligne et de les inviter. Jeux peuvent utiliser le `invite_users()` méthode pour envoyer des invitations à un ensemble de personnes définis par leur ID utilisateur de Xbox Live. Cela est utile si vous préférez utiliser votre propre interface utilisateur dans le jeu au lieu de la cotation Xbox UI.

**Exemple :**

```cpp
std::vector<string_t>& xboxUserIds;
xboxUserIds.push_back();  // Add xbox_user_ids from your in-game roster list

auto result = mpInstance->lobby_session()->invite_users(xboxUserIds);
if (result.err())
{
  // handle error
}
```

**Fonctions effectuées par le Gestionnaire de mode multijoueur**

* Invitation envoie directement aux joueurs sélectionnés
