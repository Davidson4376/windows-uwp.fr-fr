---
title: Nouveautés pour le Kit de développement Xbox Live - novembre 2016
description: Nouveautés pour le Kit de développement Xbox Live - novembre 2016
ms.assetid: 5cf9ba9d-5a15-4e62-bc1f-45ff8b8bf3b0
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a9efeec63399916444de9ba33d4e587a914f2a7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658564"
---
# <a name="whats-new-for-the-xbox-live-sdk---november-2016"></a>Nouveautés pour le Kit de développement Xbox Live - novembre 2016

Veuillez consulter la [What ' s New - août 2016](1608-whats-new.md) article pour ce qui a été ajouté dans la version d’août 2016.

## <a name="xbox-services-api"></a>API des Services Xbox

### <a name="simplified-achievements"></a>Primes simplifiées

* [Simplifié des primes](../achievements-2017/simplified-achievements.md) sont une nouvelle méthode plus simple pour configurer et utiliser des primes.  Vous n’avez plus à envoyer des événements et avoir les services Xbox Live à décider si votre réussite est déverrouillé.  Le titre est responsable de cette décision.
* Si vous avez été partie de l’implémentation pilote anticipé de primes simplifié, nous avons également ajouté une prise en charge hors connexion.
* Il existe un nouvel échantillon appelé SimplifiedAchievements qui montre combien il est facile à utiliser.

### <a name="multiplayer"></a>Mode multijoueur

* [Navigation dans la session](../multiplayer/session-browse.md) est un nouveau moyen de vos utilisateurs rechercher un jeu multijoueur.  Navigation dans la session permet aux joueurs de rechercher une liste de sessions de jeux multijoueurs ouvertes qui répondent aux critères spécifiés.
* Le [mode multijoueur Manager](../multiplayer/multiplayer-manager.md) dispose désormais des fonctionnalités de remplissage automatique.  Si activé, mode multijoueur Manager recherche dans les membres par le biais de matchmaking pour remplir les emplacements ouverts pendant jeu.
* Une version de préproduction de [XIM (Xbox intégré multijoueur)](../multiplayer/xbox-integrated-multiplayer.md) est désormais disponible pour le développement du XDK.  XIM est une interface autonome pour ajouter facilement multijoueur communication mise en réseau et les conversations en temps réel à votre jeu grâce à la puissance des services Xbox Live.

### <a name="social-manager"></a>Gestionnaire sociale

* Nombreux [sociale Manager](../social-platform/intro-to-social-manager.md) modifications de l’API :
    * Le Gestionnaire de réseau social a quitté l’espace de noms expérimentale. Spécial grâce à ceux qui ont été précoces et a donné des commentaires !
    * `xbox_social_user`
        * `string_t` objets ont été modifiés pour `char*` objets
        * Enregistrement de présence personnalisé avec une limite de six `social_manager_presence_title_record` par `social_manager_presence_record`
    * `social_event`
        * Retourne un `std::vector<xbox_user_id_container>` au lieu de `std::vector<xbox_social_user>`
        * Ce vecteur peut être passé dans la nouvelle API, `xbox_social_user_group::get_users_from_xbox_user_ids()`
    * `xbox_social_user_group`
        * `users()` API maintenant retourne un `std::vector<xbox_social_user*>`. Ces pointeurs sont invalidés lors du prochain appel à `social_manager::do_work()`
        * `get_copy_of_users` retourner un `std::vector<xbox_social_user>` en tant que copie les utilisateurs actuels dans le social_user_group à l’appelant. Appeler cette fonction peut affecter `do_work` heure d’achèvement.
* Les réseaux sociaux Gestionnaire maintenant jamais échoue après l’initialisation. Sociale Manager réessaiera automatiquement RTA sur la déconnexion. Le `error` et `rta_disconnect_error` événements ont été déconseillées et supprimées
* Titre peut spécifier des allocateurs de mémoire personnalisés. Consultez la documentation de nouveau : [Sociale le Gestionnaire de mémoire et performances](../social-platform/social-manager-memory-and-performance-overview.md)

### <a name="xbox-one-uwp"></a>Une UWP Xbox
* TCUI API ajoute la prise en charge des utilisateurs multiples pour les applications UWP une Xbox.  Le C++ XSAPI ajoute la nouvelle Windows::System::User ^ paramètres spécifient l’utilisateur, et les API WinRT XSAPI ajoute ForUserAsync APIs.
* Exemples UWP mis à jour pour prendre en charge multi-utilisateur sur Xbox

### <a name="other"></a>Autre

* C++ / c++ / WinRT prise en charge.   Vous pouvez trouver plus en détail [ici](../introduction-to-xbox-live-apis.md)
* Nouvelles fonctionnalités pour ajouter et supprimer vos propres rappels de journalisation.  Le niveau de diagnostic recevront à votre rappel vous pouvez régler avec précision votre comportement.  Consultez `add_logging_handler` et `remove_logging_handler` dans le `microsoft::xbox::services::system::xbox_live_services_settings` espace de noms

## <a name="documentation"></a>Documentation
* Il existe une nouvelle documentation sur le [mode multijoueur Manager](../multiplayer/multiplayer-manager.md), [XIM](../multiplayer/xbox-integrated-multiplayer.md), et [concepts multijoueurs](../multiplayer/multiplayer-concepts.md) Xbox Live.
* Le [introduction Xbox Live](../get-started-with-partner/get-started-with-xbox-live-partner.md) sections ont été réécrits.  Si vous créez un nouveau titre de Xbox Live est activé, ou obtenir des informations sur l’incorporation d’autres fonctionnalités Xbox Live dans votre jeu, vous pouvez voir la nouvelle documentation [ici](../get-started-with-partner/get-started-with-xbox-live-partner.md).

## <a name="tools"></a>Outils
* L’analyseur Trace Live Xbox que vous trouverez sur [ https://aka.ms/XboxLiveTools ](https://aka.ms/XboxLiveTools) dispose désormais d’un mode de certificat.  
* Dans l’Analyseur de Trace Xbox Live est également prise en charge des console multiples.  Si vous passez dans une trace Fiddler qui contient les requêtes HTTP pour plusieurs consoles, des rapports distincts seront générés pour chacun d'entre eux.
