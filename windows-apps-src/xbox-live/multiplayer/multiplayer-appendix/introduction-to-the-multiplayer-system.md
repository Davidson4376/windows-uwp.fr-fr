---
title: Présentation du système multijoueur
description: Fournit une présentation de haut niveau dans le système de Xbox Live multijoueurs 2015.
ms.assetid: d025bd2b-2ca4-4ba9-9394-4950d96ad264
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, 2015 multijoueurs
ms.localizationpriority: medium
ms.openlocfilehash: 4a739aaa650a7086dfe58b1b8ca170e15b3b2ef0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629414"
---
# <a name="introduction-to-the-multiplayer-system"></a>Présentation du système multijoueur

| Remarque                                                                                                                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cet article est pour une utilisation avancée de l’API.  En tant que point de départ, veuillez examiner le [API du Gestionnaire de mode multijoueur](../multiplayer-manager.md) qui simplifie considérablement le développement.  Veuillez informer votre mère si vous recherchez un scénario non pris en charge dans le Gestionnaire de mode multijoueur. |

Ce document contient les sections suivantes
* Sur le système en mode multijoueur
* Architectures, des Interfaces et des composants multijoueurs
* Tiers pris en charge en mode multijoueur 2015
* Terminologie multijoueur
* Quelles sont les nouveautés en mode multijoueur 2015
* Différences entre la Xbox 360 et Xbox MPSD une Session fonctions

## <a name="about-the-multiplayer-system"></a>Sur le système en mode multijoueur


Mode multijoueur 2015 optimise l’utilisation directe de MPSD et sessions de jeu. Il fournit une meilleure prise en charge plusieurs sessions simultanées pour un utilisateur ou un titre unique et met à jour de l’API de Services Xbox (XSAPI) pour activer les titres à :

-   Publier l’activité actuelle et la disponibilité pour les jointures d’un utilisateur.
-   Envoyer des invitations à des sessions, ainsi que des chaînes de contexte de (titre spécifié) visible par l’utilisateur.
-   Détecter et rejoindre des sessions via du code de titre.
-   Mettre à jour les connexions de socket web à MPSD afin qu’ils peuvent recevoir des notifications brèves (épaule Tap) de la session change, par exemple, mises à jour qui reflètent les abonnements pour modifier les événements et les modifications d’état de connexion. MPSD utilise également les connexions de socket web rapidement détecter et agissent sur la déconnexion du client.
-   Utilisez SmartMatch matchmaking.

## <a name="multiplayer-components-interfaces-and-architectures"></a>Architectures, des Interfaces et des composants multijoueurs

### <a name="components-of-2015-multiplayer"></a>Composants de mode multijoueur 2015

Mode multijoueur est un système qui se compose de plusieurs composants. Il est suffisamment flexible pour permettre des autres composants, tels que des serveurs dédiés et les systèmes de matchmaking externe.
#### <a name="multiplayer-session-directory-mpsd"></a>Multiplayer Session Directory (MPSD)


Annuaire de sessions multijoueur (MPSD) est un service qui contient une collection de sessions. Une session est définie comme un document sécurisé résidant dans le cloud et qui représente un groupe de personnes de jeu. Pour plus d’informations MPSD, consultez [mode multijoueur Session Directory (MPSD)](multiplayer-session-directory.md).


#### <a name="multiplayer-apis"></a>API multijoueurs

Mode multijoueur 2015 fournit une API WinRT multijoueurs, implémentée par le biais du **Microsoft.Xbox.Services.Multiplayer Namespace**. Ses composants comprennent le **MultiplayerService classe**, définition d’un service web qui encapsule MPSD.

Si vous est également inclus, une API REST de mode multijoueur. Il définit les objets JSON et les URI qui sont appelées par les méthodes d’API de WinRT. La fonctionnalité REST peut être utilisée dans les appels directs à partir de vos titres de, mais il est conseillé d’accéder à MPSD indirectement via l’API WinRT. Pour plus d’informations, consultez **MPSD appelant**.

#### <a name="xbox-party-system"></a>Système Xbox tiers

En mode multijoueur 2015, le système de tiers Xbox prend en charge uniquement les parties de conversation des entités externes. Titres peuvent interagir avec le système de tiers pour découvrir l’appartenance des parties de la conversation. Pour plus d’informations, consultez **tiers pris en charge en mode multijoueur 2015**.

Le système tiers prend désormais en charge les jeux directement par le biais de la session MPSD, au lieu d’utiliser un jeu tiers. Il incombe le titre à utiliser des sessions pour activer les opérations telles que le membre interaction, extraction de lecteurs dans un jeu comme un espace est disponible et l’intérêt des utilisateurs pendant les périodes d’attente.


### <a name="2015-multiplayer-interfaces"></a>Interfaces multijoueur 2015

Mode multijoueur 2015 utilise plusieurs interfaces à d’autres composants de la Xbox.
#### <a name="xbox-secure-sockets"></a>Xbox Secure Sockets

Mode multijoueur 2015 prend en charge la communication réseau de bas niveau entre les périphériques à l’aide des sockets sécurisés Xbox et Winsock. La fonctionnalité de mise en réseau utilise Internet Protocol Security (IPSec) pour autoriser des titres fournir l’association de périphérique sécurisé. Consultez **des présentations de mise en réseau** pour plus d’informations de la Xbox sécuriser la fonctionnalité de sockets.


#### <a name="xbox-live-real-time-activity-service"></a>Service d’activité en temps réel en direct de Xbox

Mode multijoueur 2015 utilise le **service de l’activité en temps réel** pour autoriser des titres pour s’abonner aux changements de session MPSD et activer la détection automatique du client se déconnecte. Plus d’informations [Service de l’activité en temps réel (RTA)](../../real-time-activity-service/real-time-activity-service.md).


#### <a name="xbox-live-matchmaking-service"></a>Xbox Live Matchmaking Service

Le **service équivalent** forms groupes à partir de lecteurs, en fonction de leurs préférences et les données et les informations fournies lors de le SmartMatch matchmaking. Pour plus d’informations de mode multijoueur utilisent ce service, consultez [SmartMatch Matchmaking](smartmatch-matchmaking.md).


#### <a name="xbox-live-reputation-service"></a>Service de réputation en direct de Xbox

Le *service de réputation* gère les statistiques utilisateur sur la réputation et pendant matchmaking filtré de réputation. Il est utilisé en mode multijoueur 2015 via SmartMatch matchmaking. Pour plus d’informations, consultez [réputation](../../social-platform/people-system/reputation.md).


#### <a name="xbox-live-compute"></a>Calcul dynamique Xbox

Xbox Live Compute fournit une puissance de titres à l’aide du mode multijoueur 2015 de traitement de cloud. Pour plus d’informations sur l’utilisation de Xbox Live Compute, consultez [à l’aide de Xbox Live Compute en mode multijoueur](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer).


### <a name="typical-2015-multiplayer-architectures"></a>Architectures multijoueur 2015 standard

Cette section présente les architectures multijoueur 2015 plus classiques.
#### <a name="peer-to-peer-architecture"></a>Architecture de peer-to-peer

Dans l’architecture de peer-to-peer, le titre utilise matchmaking MPSD et SmartMatch pour découvrir les adresses de l’homologue. Les adresses sont ensuite utilisées pour se connecter pairs à l’aide des sockets sécurisés Xbox. Pour plus d’informations, consultez *Introduction à Winsock sur Xbox One*.

![Diagramme d’architecture d’égal à égal](../../images/multiplayer/Mult2015ArchPeer.png)


#### <a name="client-server-architecture"></a>Architecture client-serveur

Dans l’architecture multijoueur client-serveur, le titre utilise matchmaking MPSD et SmartMatch pour découvrir les adresses de serveur dédié. Ceux-ci sont ensuite utilisés pour se connecter au serveur dédié à l’aide des sockets sécurisés Xbox. Pour plus d’informations, consultez *Introduction à Winsock sur Xbox One*.

| Remarque                                                                         |
|-------------------------------------------------------------------------------------------|
| Xbox Live Compute des instances peuvent être utilisées en tant que les serveurs dans l’architecture client-serveur. |

![Diagramme d’architecture de serveur client](../../images/multiplayer/Mult2015ArchClientServer.png)


## <a name="parties-supported-by-2015-multiplayer"></a>Tiers pris en charge en mode multijoueur 2015
Mode multijoueur 2015 pour Xbox One n’expose pas le « jeu tiers » comme une construction au niveau du système. Toutefois, il prend en charge la partie « chat » au niveau du système, comme dans mode multijoueur 2014.

| Remarque                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------|
| Vos titres peuvent toujours implémenter des expériences utilisateur similaires à celles implémentées à l’aide de la partie jeu, mais à l’aide de sessions MPSD à la place. |


### <a name="chat-party"></a>Partie de la conversation

Une partie de la conversation est un groupe de personnes conversez avec d’autres, gérés par l’utilisateur via l’application Xbox tiers. Les utilisateurs peuvent être dans une partie de la conversation lors de la lecture dans une session de jeu, ou lors de l’exécution d’une autre activité de la console. Toutefois, il n’y a aucun lien entre les utilisateurs dans la partie de la conversation et d’autres activités pour ces utilisateurs.

La partie de la conversation est exposée à l’aide de la *PartyChat classe*, ce qui permet le titre examiner l’état de la partie de la conversation. Par exemple, un titre peut énumérer les membres de la partie de la conversation à l’aide de la *PartyChat.GetPartyChatViewAsync méthode*.


### <a name="implementing-features-similar-to-those-related-to-the-game-party"></a>Implémentation des fonctionnalités similaires à celles qui sont liées au jeu tiers

En mode multijoueur 2014, le jeu tiers pris en charge plusieurs objectifs. Il est autorisé d’un titre à :

-   Publier l’activité actuelle et la disponibilité pour les jointures d’un utilisateur
-   Envoyer des invitations (invitations) aux sessions
-   Découvrir et de rejoindre des sessions
-   Recevoir des notifications sur certaines modifications à ces sessions enregistrées au tiers
-   Utilisez SmartMatch matchmaking
-   Conserver un groupe de personnes dans plusieurs sessions de jeu

Mode multijoueur 2015 prend en charge toutes les fonctionnalités ci-dessus directement par le biais de sessions MPSD.

## <a name="multiplayer-terminology"></a>Terminologie multijoueur


| Terme                                 | Description|
| --- | --- |
| Lecteur actif                        | Un lecteur qui a été défini à l’état actif dans la session. Le titre définit un lecteur à cet état lorsque le joueur participe à un jeu. Pour plus d’informations, consultez [États de Session utilisateur](mpsd-session-details.md).                                                                                                                                                                                                                                                                                          |
| Arbitre                              | La console unique dans une session de jeu qui gère l’état de la session de répertoire (MPSD) session multijoueur pour un jeu, par exemple, dans la publication de la session de jeu à matchmaking, pour rechercher plus de joueurs. L’arbitre est définie par le titre. L’arbitre n’est pas toujours l’hôte du jeu. Consultez [Session arbitre](mpsd-session-details.md).                                                                                                                                                                            |
| Jeu réorganisée                        | Un type de jeu qui est créé uniquement par le biais d’un seul lecteur inviter d’autres joueurs, sans l’intervention de matchmaking.                                                                                                                                                                                                                                                                                                                                                                                                    |
| Partie de la conversation                           | Un groupe de personnes qui sont en conversation ensemble. Les personnes peuvent être engagés dans la même activité ou qu’ils peuvent être impliqués dans différentes activités, telles que des jeux, de musique ou d’applications. Consultez **tiers pris en charge en mode multijoueur 2015**.                                                                                                                                                                                                                                                                                 |
| Invitation à un jeu                          | Une invitation à rejoindre une session de jeu.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Session de jeu                         | Une session dans laquelle les utilisateurs sont en fait lecture ensemble. Tous les scénarios multijoueurs, par exemple, la jointure d’une salle d’attente, ou de matchmaking finissent dans une session de jeu. La session est souvent publiée en tant qu’activités actuel d’utilisateurs pour permettre des jointures. Il est également utilisé pour générer la récente liste de joueurs. Consultez [détails de la Session MPSD](mpsd-session-details.md).                                                                                                                                                           |
| Hôte de Session de jeu                    | La console en cours d’exécution du jeu jouent simulation de titres basé sur une architecture de réseau peer-to-peer hôte. Cette console est généralement identique à l’arbitre, mais il ne devra pas être identiques.                                                                                                                                                                                                                                                                                                                            |
| Poignée (ou Session)           | Une référence à une session MPSD qui a l’état supplémentaire et le comportement associé. Consultez [MPSD gère les Sessions](multiplayer-session-directory.md).                                                                                                                                                                                                                                                                                                                                                                    |
| Acteur inactif                      | Un lecteur qui a été défini sur l’état inactif au sein de la session. Le titre définit un lecteur à cet état lorsqu’un jeu est suspendu ou est inactif, tel que défini par le titre. Dans certains cas, MPSD peut également définir un lecteur comme étant inactive, mais il est principalement la responsabilité du titre à le faire. Pour plus d’informations, consultez [États de Session utilisateur](mpsd-session-details.md).                                                                                                                           |
| Hopper                               | Un Hopper est une collection piloté par une logique de tickets de correspondance. Un titre peut avoir d’exploits en plusieurs, mais que les tickets dans le même hopper peuvent être mis en correspondance. Par exemple, un titre peut créer un hopper quel lecteur compétence est l’élément le plus important pour la correspondance. Il peut utiliser un autre hopper dans lequel les joueurs sont mis en correspondance uniquement si elles ont acheté le même contenu téléchargeable. Pour plus d’informations sur l’emplacement où les exploits en s’intègrent dans le flux de travail SmartMatch, consultez [SmartMatch opérations d’exécution](smartmatch-matchmaking.md) |
| Joindre en cours                     | Le concept de joindre un autre lecteur 's jeu une fois que le jeu a commencé. Lecteurs peuvent rejoindre le jeu d’un ami via la carte de joueur de friend. Le titre peut ensuite déplacer ces lecteurs dans la session de jeu au moment opportun.                                                                                                                                                                                                                                                                                                      |
| Session de salle d’attente                        | Une session d’assistance pour les joueurs invités qui sont en attente pour rejoindre une session de jeu. Consultez [détails de la Session MPSD](mpsd-session-details.md).                                                                                                                                                                                                                                                                                                                                                                                       |
| Session de cible de correspondance                 | Une session de correspondance définie pendant SmartMatch matchmaking pour représenter la correspondance. Consultez [SmartMatch Matchmaking](smartmatch-matchmaking.md).                                                                                                                                                                                                                                                                                                                                                                                              |
| Session de Ticket de correspondance                 | Une session de correspondance préliminaire définies pendant SmartMatch matchmaking. Consultez [SmartMatch Matchmaking](smartmatch-matchmaking.md).                                                                                                                                                                                                                                                                                                                                                                                                         |
| Session MPSD                         | Un document sécurisé qui se trouve dans l’annuaire de sessions multijoueur (MPSD) dans le cloud de Xbox Live. Il contient un groupe d’utilisateurs qui peuvent être connectés lors de l’exécution d’un titre sur Xbox One, ainsi que des métadonnées sur les utilisateurs et leur jeu. Consultez [détails de la Session MPSD](mpsd-session-details.md).                                                                                                                                                                                                                  |
| Multiplayer Session Directory (MPSD) | Le service d’exploitation dans le cloud que le système en mode multijoueur utilise pour stocker et récupérer des sessions. Consultez [annuaire de sessions multijoueur (MPSD)](multiplayer-session-directory.md).                                                                                                                                                                                                                                                                                                                                                                |
| Application de tiers                            | Un système Xbox One aligner l’application qui permet aux utilisateurs d’afficher et gérer leurs parties.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Session de serveur                       | Une session de jeu créée par le traitement de Xbox Live Compute. Consultez [détails de la Session MPSD](mpsd-session-details.md).                                                                                                                                                                                                                                                                                                                                                                                                            |
| Épaule Tap                         | Une notification MPSD titre une modification potentiellement intéressante s’est produite sur le service. Le drainage épaule est un rappel rapide, souvent moins d’information à une notification régulière. Consultez [MPSD modifier la gestion des notifications et détection de la déconnexion](multiplayer-session-directory.md).                                                                                                                                                                                                                     |
| Matchmaking SmartMatch               | Une Xbox Live matchmaking fonctionnalité disponible pour les titres Xbox One, implémentées par le service. Le titre à l’aide de MPSD et matchmaking, effectue une demande à mettre en correspondance et est informé ultérieurement qu’un groupe de mise en correspondance a été trouvé. Consultez [SmartMatch Matchmaking](smartmatch-matchmaking.md).                                                                                                                                                                                                                                  |

## <a name="whats-new-in-2015-multiplayer"></a>Quelles sont les nouveautés en mode multijoueur 2015

| Attention                                                                                                                                                                                                                                               |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lorsque vous utilisez le mode multijoueur 2015, il est important de noter que les classes associées à la partie en mode multijoueur 2014 ne doivent pas être utilisés plus. Combinaison de fonctionnalités multijoueur 2015 avec les classes relatives au tiers provoque un comportement incohérent et ne doit jamais être tentée. |


### <a name="new-concepts-in-2015-multiplayer"></a>Nouveaux Concepts en mode multijoueur 2015


#### <a name="web-socket-connections-to-mpsd"></a>Connexions de Socket Web à MPSD

MPSD permet désormais de titres de conserver des connexions de socket web avec lui. Ces connexions permettent aux clients de recevoir des notifications lorsque modifiez des sessions. Pour plus d’informations, consultez [déconnecter de détection et de gestion de Notification de modification MPSD](multiplayer-session-directory.md).


#### <a name="mpsd-session-handles"></a>Handles de Session MPSD

Mode multijoueur 2015 prend en charge pour les descripteurs de session MPSD, qui sont des références à des sessions qui peuvent inclure des données typées. Pour plus d’informations, consultez [MPSD gère les Sessions](multiplayer-session-directory.md).


### <a name="summary-of-new-2015-multiplayer-winrt-api-functionality"></a>Résumé des nouvelles fonctionnalités de WinRT multijoueur API 2015

Nouvelles fonctionnalités les API WinRT multijoueur repose sur le XSAPI existant, pour faciliter que la transition pour les titres déjà utilisant le mode multijoueur 2014 en association avec l’API de jeux tiers.

Mode multijoueur 2015 ajoute le *MultiplayerActivityDetails classe*, qui représente les détails de l’activité en cours d’un utilisateur, par exemple, la session dans laquelle l’utilisateur est joignable.

Nouvelle fonctionnalité a été ajoutée à la *MultiplayerService classe*. Exemples sont des méthodes et propriétés de gestion des activités pour les utilisateurs et groupes de réseaux sociaux, récupération des sessions à l’aide de divers filtres et les handles, envoyer des invitations de jeu et la session lit et écrit à l’aide des poignées.

Le *MultiplayerSession classe* ajoute des fonctionnalités au travail avec les types de modification de session, les abonnements pour les notifications, comparaison de la session et définition d’une session comme étant fermé.

Le *MultiplayerSessionReference classe* est modifiée pour hériter de **IMultiplayerSessionReference**, pour prendre en charge les appels cross-espace de noms. La classe a également un nouveau chemin d’accès URI méthodes d’analyse.

| Remarque                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pour prendre en charge les événements et les abonnements aux notifications, mode multijoueur 2015 ajoute des fonctionnalités à la *Microsoft.Xbox.Services.RealTimeActivity Namespace*. Également inclus dans la nouvelle fonctionnalité est *SystemUI.ShowSendGameInvitesAsync méthode*, utilisé pour afficher l’invitation à un jeu l’interface utilisateur pour le mode multijoueur 2015. |

## <a name="differences-between-xbox-360-and-xbox-one-mpsd-session-functions"></a>Différences entre la Xbox 360 et Xbox MPSD une Session fonctions

| Fonction | Xbox 360 | Xbox One |
|---|---|---|
| **Obtenir des informations de session de jeu** | XSessionGetDetails, XSessionSearchByID ou titre effectue le suivi. | Titre de demande des informations de session MPSD. |
|**Migrer l’ordinateur hôte** | Si nécessaire, titre appelle XSessionMigrateHost. | Selon la cause de la migration, le titre peut être en mesure d’assigner un nouvel hôte pour la session, ou peut créer une nouvelle session MPSD. |
| **Plusieurs sessions de lecteur** | Il est difficile de gérer plusieurs sessions simultanément, par exemple, XNetReplaceKey et XNetUnregisterKey. | Session basée sur le service facilite la gestion d’une session plus propre et simplifie la gestion de plusieurs sessions. |
| **Signouts et se déconnecte** | Titre doit gérer se déconnecte et signout différemment avec XCloseHandle ou XSessionDelete. | MPSD simplifie signouts et se déconnecter de la gestion et délai d’attente défini dans la configuration de jeu. |
| **Matchmaking** | Requêtes de matchmaking basée sur le client | Matchmaking service qui permet une meilleure correspondance de qualité et plus facile matchmaking en arrière-plan dans le titre. |


### <a name="sessions"></a>Sessions

Sur la Xbox 360, une session représenté une instance de jeu. Les utilisateurs recherchés pour les sessions dans le service et renvoyé de statistiques à la fin d’une session.

Sur Xbox One, une session est plus générique et représente un groupe de lecteurs. Une session est requise pour les connexions de réseau entre les consoles et conserve des informations qui doivent être partagées entre tous les utilisateurs dans la session. Certains exemples de ces informations incluent le nombre de lecteurs autorisés dans la session, l’adresse sécurisé de chaque console dans la session et les données de jeu personnalisées.


### <a name="xbox-matchmaking"></a>Matchmaking de Xbox

Titres effectuées sur la Xbox 360, matchmaking en configurant un schéma d’attributs et un ensemble de requêtes pour effectuer des recherches dans ces attributs. Au moment de l’exécution, le titre a choisi de l’hôte une session ou la recherche d’une.

Sur Xbox One, matchmaking est basée sur le serveur, et les titres et les lecteurs ne sont plus décider d’héberger ou effectuez une recherche. Au lieu de cela, chaque groupe préformé de lecteurs crée une session de « ticket » et soumet cette session pour le service. Le service de recherche d’autres sessions, puis combine les groupes pour former une nouvelle session de « cible ». Les clients sont informés de la correspondance et effectuer la qualité de service (QoS) pour valider la connectivité avec d’autres membres de la session avant de commencer la lecture du jeu.


### <a name="xbox-live-compute-service"></a>Xbox Live Compute Service

Le service Xbox Live Compute est une nouvelle offre sur Xbox One. Il permet aux développeurs d’exploiter la puissance de calcul élastique du cloud et permet des scénarios multijoueurs plus grande que possible dans un réseau peer-to-peer. Pour plus d’informations sur le service Xbox Live Compute, consultez la documentation de Xbox Live Compute dans la documentation XDK.


## <a name="see-also"></a>Voir également

[Annuaire de sessions multijoueur (MPSD)](multiplayer-session-directory.md)

[SmartMatch Matchmaking](smartmatch-matchmaking.md)

[Service de l’activité en temps réel (RTA)](../../real-time-activity-service/real-time-activity-service.md)

[Réputation](../../social-platform/people-system/reputation.md)

[À l’aide de Xbox Live Compute en mode multijoueur (requiert l’accès de partenaire géré)](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer)
