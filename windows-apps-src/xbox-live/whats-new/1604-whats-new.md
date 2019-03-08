---
title: Nouveautés pour le Kit de développement Xbox Live - avril 2016
description: Nouveautés pour le Kit de développement Xbox Live - avril 2016
ms.assetid: a6f26ffd-f136-4753-b0cd-92b0da522d93
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9ce63a0174fa0c4158764b8bca2443d58d0aefd9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660764"
---
# <a name="whats-new-for-the-xbox-live-sdk---april-2016"></a>Nouveautés pour le Kit de développement Xbox Live - avril 2016

Veuillez consulter la [What ' s New - mars 2016](1603-whats-new.md) article pour ce qui a été ajouté dans 1603

## <a name="os-and-tool-support"></a>Prise en charge du système d’exploitation et d’outil
Le Xbox Live SDK prend en charge Windows 10 RTM [Version 10.0.10240] et Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="tournaments"></a>Tournois
- L’outil de tournois de Xbox Live est désormais inclus avec le SDK.  Vous pouvez le voir dans le répertoire des outils, ainsi que des informations sur son utilisation.
- Les API pour tournois sont également disponibles.  Consultez l’espace de noms Xbox::Services::Tournaments
- Documentation est également disponible dans le Guide de programmation.

## <a name="documentation"></a>Documentation
- [Guide de résolution des problèmes de connexion](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md) répertorie certaines stratégies générales pour déboguer les échecs de connexion, ainsi que les étapes à suivre selon de code d’erreur.
- Le [place de marché](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-marketplace/marketplace-and-downloadable-content) docs pour les développeurs Xbox One uniquement figurent désormais dans le Guide de programmation.  Les développeurs UWP doivent continuer à consulter les partenaires pour des informations sur le magasin.
- Il existe un [XDK pour le portage UWP guide](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) vous pouvez faire référence si vous souhaitez migrer un titre Xbox One vers la plateforme Windows universelle.
- Consultez le [affinée la limitation du débit](../using-xbox-live/best-practices/fine-grained-rate-limiting.md) article pour obtenir une description de comment celles-ci sont appliquées pour les différents points de terminaison de Service Xbox Live et scénarios, mais aussi d’informations sur ce que sont les limites.

## <a name="multiplayer-manager"></a>Gestionnaire de mode multijoueur
Le [mode multijoueur Manager](../multiplayer/multiplayer-manager.md) n’est plus dans expérimentale.  Nous avons intégré les commentaires des développeurs utilisant cette API et certaines API devenir plus cohérents entre eux.  Utilisez le Gestionnaire de mode multijoueur en tant que point de départ lors de votre développement multijoueur, car elle fournit une API plus simple que beaucoup des complexités de l’API de 2015 multijoueurs gère pour vous.

Dans les sections, suivantes, nous avons répertorié les nouvelles fonctionnalités à l’API, ainsi que d’un petit nombre de modifications avec rupture.

#### <a name="completed-events"></a>Événements terminés
Toutes les API désormais correspondre à un``` _competed``` événement et tous les événements sont déclenchés, quel que soit la réussite ou l’échec. L’ancien comportement était qu’il déclenché en cas d’échec, pour le titre de prendre des mesures. Et étant donné que les appels sont traités par lot, cela signifie qu’à l’achèvement, le titre obtiendra plusieurs ```_competed``` événements.

| API | Événement retourné |
|-----|----------------|
| ```lobby_session()->set_local_member_properties``` |  ```local_member_property_write_completed ```
| ```lobby_session()->set_local_member_connection_address``` | ```local_member_connection_address_write_completed``` |
| ```lobby_session()->set_properties``` | ```session_property_write_completed``` |
| ```lobby_session()->set_synchronized_properties``` | ```session_synchronized_property_write_completed``` |
| ```lobby_session()->set_synchronized_host``` | ```synchronized_host_write_completed``` |

En va de même pour ```game_session()```.

#### <a name="application-defined-context"></a>Contexte de l’application définie
Vous pouvez maintenant passer dans un contexte défini par l’application facultatif pour chaque set_ * méthodes mettre en corrélation l’événement multijoueur à l’appel d’origine.
Par exemple

```cpp
_XSAPIIMP xbox_live_result<void> set_properties(
        _In_ const string_t& name,
        _In_ const web::json::value& valueJson,
        _In_ context_t context = nullptr
       );
```

Le contexte serait ensuite être renvoyé dans le titre par le biais du ```_completed``` événement.

#### <a name="breaking-changes"></a>Changements importants

1.  Les deux inviter API (```invite_friends``` & ```invite_users```) sont maintenant synchrones. Une fois terminée, elle retourne un événement invite_sent.

2.  ```write_synchronized_properties_and_commit``` a été renommé en ```set_synchronized_properties```. Une fois terminée, elle retourne un ```session_synchronized_property_write_completed``` événement.

3.  ```write_synchronized_host_and_commit``` a été renommé en ```set_synchronized_host```. Une fois terminée, elle retourne un ```synchronized_host_write_completed``` événement.

4.  Sur ```lobby_session()```

  *Removed*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
```

  *Added*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

5.  Sur ```game_session()```

  *Removed*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_arbitration_server& arbitration_server() const;
```
  *Added*

```cpp
_XSAPIIMP const std::unordered_map<string_t, multiplayer_session_reference>& tournament_teams() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

6.  Modifier dans les types d’événements :

  *Removed*

```cpp
write_pending_changes_failed,
tournament_property_changed,
arbitration_property_changed
```

  *Renamed*

  ```write_synchronized_properties_completed``` renommé en ```session_synchronized_property_write_completed```

  ```write_synchronized_host_completed``` renommé en ```synchronized_host_write_completed```
