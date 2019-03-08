---
title: À l’aide de SmartMatch Matchmaking
description: Découvrez comment utiliser Xbox Live SmartMatch pour faire correspondre les lecteurs dans un jeu multijoueur.
ms.assetid: 10b6413e-51d9-4fec-9110-5e258d291040
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, matchmaking de multijoueurs, one, xbox, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 89f33768efcd649987866fd0798c222aa97f7ff8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592094"
---
# <a name="using-smartmatch-matchmaking"></a>À l’aide de SmartMatch Matchmaking

L’organigramme suivant illustre le processus de matchmaking SmartMatch.

![](../../images/multiplayer/Multiplayer_2015_SmartMatch_Matchmaking.png)

## <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>Création d’une Session de Ticket de correspondance et un Ticket de correspondance

Avant de commencer matchmaking, un « scout « matchmaking configure une session de ticket de correspondance pour représenter le groupe de personnes qui souhaitent Entrez matchmaking ensemble. Tous les utilisateurs de ce groupe de rejoindre la session à l’aide de la **MultiplayerSession.Join (méthode) (chaîne, booléen, booléen)**.

Une fois que la session de ticket a été créée et remplie avec des lecteurs, le titre soumet le service équivalent à l’aide de la session le **MatchmakingService.CreateMatchTicketAsync méthode**. Cette méthode crée un ticket de correspondance qui représente la session de ticket et met à jour le champ /servers/matchmaking/properties/system/status dans la session de ticket à la « recherche ». Pour plus d'informations, consultez [Procédure : Créer un Ticket de correspondance](multiplayer-how-tos.md).

La réponse à partir de la méthode de création de tickets de correspondance est un **CreateMatchTicketResponse classe** objet. La réponse contient l’ID de ticket de correspondance, un GUID qui peut être utilisé peut être utilisé pour annuler matchmaking en supprimant le ticket. La réponse contient également un temps d’attente moyen pour hopper, ce qui peut être utilisée pour définir les attentes des utilisateurs.


## <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>Définition des attributs de Matchmaking sur la Session et lecteurs

Lorsque vous soumettez une session sur matchmaking, le titre peut définir les attributs que le service utilise pour regrouper la session avec les autres sessions. Attributs peuvent être spécifiés au niveau du ticket ou au niveau d’un membre.


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>Définition des attributs de Matchmaking au niveau du Ticket de correspondance

Le titre soumet des attributs au niveau du ticket de correspondance dans le *ticketAttributesJson* paramètre de la **MatchmakingService.CreateMatchTicketAsync** (méthode). Attributs spécifiés au niveau du ticket remplacent les mêmes attributs spécifiés au niveau d’un membre.


### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>Définition des attributs de Matchmaking au niveau d’un membre

Le titre spécifie les attributs d’un membre sur chaque membre au sein de la session de ticket de correspondance. Ces propriétés sont définies en appelant le **MultiplayerSession.SetCurrentUserMemberCustomPropertyJson méthode**, à l’aide d’un nom de propriété de « matchAttrs ». Cet appel place les attributs dans la base de données /members/ {index} / propriétés/personnalisé/matchAttrs champ sur chaque lecteur au sein de la session de ticket.

Le processus de matchmaking « aplanit » d’un membre chaque dans un seul attribut au niveau du ticket, selon la méthode de mise à plat spécifiée pour cet attribut dans la configuration de Xbox Live pour le hopper. Cela peut être configuré sur [XDP](https://xdp.xboxlive.com) ou [partenaires](https://partner.microsoft.com/dashboard).


## <a name="making-the-match"></a>La correspondance

Avec la session de ticket et la correspondance ticket configuré, les correspondances de service matchmaking la session de ticket représenté avec les autres sessions de ticket représentant les autres groupes et crée ou identifie une session de cible de correspondance. Le service crée également des réservations pour les lecteurs de mise en correspondance dans la session cible et marque ensuite les sessions de ticket comme mis en correspondance. MPSD notifie le titre de cette modification à la session de ticket.

Maintenant le titre doit ensuite prendre des mesures pour initialiser la session cible afin de confirmer que suffisamment de lecteurs sont affichées et effectuez la qualité de service (QoS) des vérifications pour vous assurer qu’ils puissent se connecter entre eux avec succès. Si l’initialisation et/ou de qualité de service échoue, le titre de la marque de la session de ticket pour la nouvelle soumission à matchmaking afin que vous pouvez trouver un autre groupe. Pour plus d’informations sur les processus, consultez [initialisation de la Session cible et QoS](smartmatch-matchmaking.md).

Pendant l’activité de correspondance, les modifications suivantes sont apportées aux objets JSON pour la session :

-   champ /Servers/Matchmaking/Properties/System/Status défini sur « trouvé »
-   champ /Servers/Matchmaking/Properties/System/targetSessionRef défini sur session cible
-   /Members/ {index} / propriétés/custom/champ matchAttrs pour chaque session de ticket copiés vers le /members/ {index} / constantes/custom/matchmakingResult/playerAttrs champ
-   Pour chaque joueur, ticket attributs copiés à partir du champ ticketAttributes dans le ticket de correspondance dans /members/ {index} / constantes/custom/matchmakingResult/ticketAttrs champ


## <a name="maintaining-the-match-ticket"></a>Maintenir le Ticket de correspondance

Le service utilise un instantané de la session de ticket à l’heure à laquelle le ticket de correspondance est créé pour la session. Ainsi, si des lecteurs sont rejoindre ou quitter la session de ticket, le titre doit utiliser le service à supprimer, puis recréer le ticket de correspondance.


## <a name="reusing-the-game-session-as-a-match-ticket-session"></a>Réutilisation de la Session de jeu en tant qu’une Session de Ticket de correspondance

| Important                                                                                                                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Il est important de savoir que deux sessions avec *preserveSession* toujours la valeur ne sera pas capable de faire correspondre avec les autres, dans la mesure où ils ne peuvent pas être combinés. Le flux de matchmaking utilisé par le titre à prendre cela en compte. |

Un titre peut réutiliser une session de jeu existante en tant qu’une session de ticket de correspondance pour rechercher les joueurs plus participer à un jeu qui est déjà en cours. Pour ce faire, le titre doit créer le ticket de correspondance en appelant **CreateMatchTicketAsync** avec la *preserveSession* paramètre défini sur **PreserveSessionMode.Always** . Ensuite, le service s’assure que la session existante utilisée pour le ticket est conservée tout au long du processus de matchmaking et devient la session résultante de la cible.


## <a name="deleting-the-match-ticket"></a>Supprimer le Ticket de correspondance

Pour supprimer le ticket de correspondance, les appels de titre **MatchmakingService.DeleteMatchTicketAsync méthode**. Suppression du ticket :

1.  Matchmaking s’arrête pour les utilisateurs dans la session de ticket.
2.  Mises à jour le champ /servers/matchmaking/properties/system/status dans la session de ticket à « annulée ».


## <a name="performing-matchmaking-for-games-using-xbox-live-compute"></a>Exécution Matchmaking pour jeux à l’aide de calcul en direct Xbox

Voici les étapes de haut niveau qui ont lieu pour obtenir un matchmade utilisateur dans un jeu basé sur la Xbox Live Compute. Un flux similaire doit s’appliquer pour les jeux hébergés par des tiers.
1.  Le scout crée une session de ticket pour représenter le groupe. Cette session contient une liste des centres de données potentielle, situés dans la configuration de session dans /constants/system/measurementServerAddresses. Il est fourni à partir du modèle session si la liste de centre de données est statique ou à partir du client d’écriture lors de la création de session après avoir obtenu à partir de la Xbox Live Compute premier. Cette session contient également des valeurs maxAllowedPlayers dans l’objet personnalisé/targetSessionConstants/gameServerPlatform gsiSetId et gameVariantId.
2.  Tous les autres clients dans le groupe de rejoignent la session de ticket.
3.  Tous les membres du groupe de téléchargement les valeurs measurementServerAddresses à partir de l’objet /constants/system pour la session de ticket, exécuter la commande ping à l’aide de l’API de la plateforme et téléchargement une liste ordonnée de centres de données par défaut à la session, tel que défini dans /members/ {index} /Properties/System/serverMeasurements.

| Remarque                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Le titre peut définir et récupérer les valeurs de measurementServerAddresses à partir de la session à l’aide de la **MultiplayerSession.SetMeasurementServerAddresses** (méthode) et le  **Propriété de MultiplayerSessionConstants.MeasurementServerAddressesJson**. |

4.  Les appels de scout **CreateMatchTicketAsync**, en passant une référence à la session de ticket.

| Remarque                                                                                                                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Si les objets de session de ticket ont des constantes ne correspondent pas, la méthode de ticket create risque de ne pas réussir. Cela peut être évité en ajoutant une doit règle pour le hopper, pour empêcher la mise en correspondance avec des lecteurs qui ont des constantes. |

Si le **MatchTicketDetailsResponse.PreserveSession propriété** est définie sur jamais, le service copie les mesures de serveur à partir de chaque membre dans la représentation interne du ticket. Elle aplatit les mesures de serveur des membres du ticket dans une collection de mesures de serveur unique pour le ticket, stockée dans la représentation interne du ticket comme un attribut du ticket « spéciales ».

Si **MatchTicketDetailsResponse.PreserveSession** est défini sur toujours, le serveur de mesures ne sont pas utilisés. Au lieu de cela, le service copie la valeur de /properties/system/matchmaking/serverConnectionString pour la session dans la représentation interne du ticket, comme une collection de serverMeasurements de taille 1 avec une latence de zéro.

5.  Le service correspond à la session de ticket avec d’autres personnes représentant les autres groupes, en prenant le serveur de collections de mesure en compte. Il essaie de correspond au groupe avec d’autres groupes qui ont les mêmes centres de données préféré hautement.
6.  Une fois un groupe de mise en correspondance a été trouvé, le service crée ou identifie une session de la cible et ajoute tous les joueurs depuis les sessions de ticket sont mis en correspondance ensemble. Le service écrit les mesures de serveur aplati final pour le groupe de mise en correspondance dans /properties/system/serverConnectionStringCandidates. Il écrit les mesures de serveur envoyée à l’origine pour chaque membre qui vient d’être ajouté dans la session cible dans /members/ {index} / constantes/système/matchmakingResult/serverMeasurements.
7.  Tous les clients effectuent une initialisation sur la session cible, comme indiqué ci-dessus. Toutefois, étant donné que les clients doivent se connecter à Xbox Live Compute, ils n’effectuent pas de QoS avec eux pour vérifier la connectivité.
8.  Certains ou tous les clients appellent le **GameServerPlatformService.AllocateClusterAsync méthode**. La première condition déclenche l’allocation, tandis que les autres reçoivent uniquement un accusé de réception. La méthode obtient la session cible à partir de MPSD et choisit un centre de données basé sur les préférences de centre de données dans la session cible, ainsi que d’une charge et d’autres connaissances spécifiques de calcul Xbox Live.
9.  Tous les clients jouent.


## <a name="see-also"></a>Voir également

[Opérations d’exécution de SmartMatch](smartmatch-matchmaking.md)

[SmartMatch Matchmaking](smartmatch-matchmaking.md)

**Microsoft.Xbox.Services.Matchmaking Namespace**

**Microsoft.Xbox.Services.Multiplayer Namespace**
