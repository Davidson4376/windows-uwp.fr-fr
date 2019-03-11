---
title: Concepts de multijoueur Xbox Live
description: Découvrez les concepts multijoueurs courants utilisés par les systèmes de multijoueur Xbox Live.
ms.assetid: 1e765f19-1530-4464-b5cf-b00259807fd3
ms.date: 08/25/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, mode multijoueur
ms.localizationpriority: medium
ms.openlocfilehash: 0e2ba32b2eaff757cfa6401952567f6b0163f632
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623794"
---
# <a name="xbox-live-multiplayer-concepts"></a>Concepts de multijoueur Xbox Live

Cette rubrique traite de plusieurs multijoueurs termes et concepts importants qui sont fréquemment utilisées dans la documentation de Xbox Live. Avoir une bonne maîtrise de ces concepts vous permettra de comprendre le fonctionnement de la Xbox Live multijoueurs.

## <a name="multiplayer-session"></a>Session multijoueur

Une session multijoueur représente un groupe d’utilisateurs de Xbox Live et les propriétés qui s’y rapportent. La session est créée et gérée par les titres et est représenté sous la forme d’un document JSON sécurisé qui se trouve dans le cloud de Xbox Live. Le document de session lui-même contient des informations sur les utilisateurs de Xbox Live qui sont connectés à la session, les traces de combien sont des métadonnées disponibles, personnalisée (pour la session, ainsi que pour chaque membre de la session), et autres informations relatives à la session de jeu.

Chaque session est basée sur un modèle de session, qui sont définies par le développeur de jeux et sont configurés dans la configuration du service Xbox Live pour une instance de titre.

Tandis qu’un titre peut créer et mettre à jour d’une session, il ne peut pas supprimer directement une session.  Au lieu de cela, une fois que tous les acteurs sont supprimées d’une session, le service de multijoueur avec Xbox Live supprimera automatiquement la session après un délai d’attente spécifié. Pour plus d’informations sur les sessions, consultez [détails de la session MPSD](multiplayer-appendix/mpsd-session-details.md).

Titres peuvent choisir d’utiliser plusieurs sessions, mais une implémentation classique multijoueur utilise deux sessions :

* Salle d’attente de session - il s’agit d’une session qui représente un groupe d’amis qui doit rester ensemble entre plusieurs essais, niveaux, cartes, etc., le jeu.
* Session de jeu - il s’agit d’une session qui représente les personnes que vous évoluez dans une instance de session spécifique d’un jeu, round, correspondance, niveau, etc. Cette session peut inclure les membres de plusieurs sessions d’introduction qui rejoignent l’instance de la session ensemble, généralement via un service équivalent.

Voici un exemple de scénario : Catherine veut jouer à certains jeux multijoueurs avec ses amis, John et Lisa. Catherine démarre un jeu et invite John et Lisa dans son jeu. Une fois ces derniers rejoignent, Catherine, John et Lisa sont toutes dans une session d’introduction. Dans cette session, ils décident de jouer dans une correspondance en ligne avec d’autres personnes. Le jeu crée une session de jeu et utilise le service Xbox Live pour remplir les emplacements de lecteur restants à d’autres joueurs Xbox Live.

Supposons que Marc et Pierre sont mises en correspondance avec eux, et cinq d'entre eux imbriquent l’arrondi. Après la fin de round, Catherine, John, Lisa laissez la session de jeu, mais sont toujours ensemble dans la session d’introduction (sans Marc et Pierre) et pouvez choisir de lire un autre round, ou basculer vers un autre mode de jeu.

### <a name="session-member"></a>Membre de la session

Un membre de la session est un utilisateur de Xbox Live qui fait partie d’une session.

### <a name="arbiter"></a>Arbitre

Un arbitre est une console ou un périphérique qui gère l’état de la session d’un match. Par exemple, l’arbitre est responsable de la publication d’une session de jeu à matchmaking afin de trouver plus de joueurs.

L’arbitre est définie par le titre. Il peut être identique à l’hôte du jeu, mais ne doit pas être identiques.

### <a name="session-host"></a>Hôte de session

L’hôte de session est la console ou périphérique qui exécute le jeu lire simulation de titres basé sur une architecture de réseau peer-to-peer hôte. Cette console ou un appareil est généralement le même que l’arbitre, mais il ne devra pas être identiques.

## <a name="multiplayer-service-session-directory"></a>Annuaire de sessions des services multijoueurs

Le service Xbox Live multijoueurs fonctionne dans le nuage Xbox Live et centralise les métadonnées du système en mode multijoueur d’un jeu sur plusieurs clients. Le système qui assure le suivi de ces métadonnées est connu en tant que l’annuaire de sessions multijoueur ou MPSD pour faire plus court. Vous pouvez considérer MPSD en tant que bibliothèque de sessions de jeu actives. Votre jeu peut ajouter, rechercher, modifier ou supprimer des sessions actives relatives à votre titre. Le MPSD gère également l’état de session et les met à jour des sessions lorsque cela est nécessaire.

MPSD permet de titres de partager les informations de base nécessaires pour se connecter à un groupe d’utilisateurs. Elle garantit que les fonctionnalités de la session sont synchronisé et cohérente. Il coordonne avec le shell et Xbox console systèmes d’exploitation de l’envoi/acceptation d’invitations et jointe par le biais de la carte de joueur.

### <a name="session-handles"></a>Handles de session

Une session est identifiée de manière unique dans MPSD par une combinaison d’éléments de données :

* La configuration du service ID (SCID) du titre
* Le nom du modèle de session qui a été utilisé pour créer la session
* Le nom de la session

Un descripteur de session est un objet JSON qui contient une référence à une session spécifique existe dans MPSD. Handles de session permettent aux membres de Xbox Live joindre aux sessions existantes.

Chaque descripteur de session inclut un guid qui identifie de façon unique le handle, ce qui permet des titres faire référence à la session à l’aide d’un guid unique.

Il existe plusieurs types de handles de session :

* inviter handle
* handle de recherche
* handle de l’activité
* handle de corrélation
* handle de transfert

#### <a name="invite-handle"></a>Inviter handle

Un handle de l’invitation est passé à un membre lorsqu’ils sont invités à rejoindre un jeu. Le handle de l’invitation contient des informations qui vous permet de jointure de jeux du membre invité la session correcte.

#### <a name="search-handle"></a>Handle de recherche

Un handle de recherche inclut des métadonnées supplémentaires relatives à la session et permet des titres rechercher des sessions qui répondent aux critères sélectionnés.

#### <a name="activity-handle"></a>Handle de l’activité

Un handle d’activité permet aux membres voir les autres membres de son réseau social lecture et peuvent être utilisés rejoindre partie d’un ami.

#### <a name="correlation-handle"></a>Handle de corrélation

Un handle de corrélation efficacement fonctionne comme un alias pour une session, ce qui permet un jeu faire référence à une session en utilisant uniquement l’id du descripteur de corrélation.

### <a name="transfer-handle"></a>Handle de transfert

Un handle de transfert est utilisé pour déplacer des joueurs à partir d’une session à une autre session.

### <a name="invites"></a>Invitations

Xbox Live fournit un système d’invitation est pris en charge par le service multijoueur. Elle permet de joueurs inviter d’autres joueurs à leurs sessions de jeu. Joueurs invités reçoivent une invitation à un jeu et un titre utilise ces informations pour rejoindre une session existante et une expérience multijoueur. Titres contrôlent le flux d’invitation et lorsque les invitations peuvent être envoyées.

Invitations peuvent être envoyées via l’interpréteur de commandes par l’utilisateur ou directement à partir de titre. Le texte de notification pour une invitation peut définir de façon dynamique par un titre pour fournir plus d’informations pour le joueur invité. Invitations peuvent également inclure des données supplémentaires pour le titre n’est pas visible à l’acteur et peuvent être utilisées pour transporter des informations supplémentaires.

### <a name="join-in-progress"></a>Jointure en cours d’exécution

En plus des invitations, Xbox Live fournit également une option d’interface pour les joueurs à rejoindre une session active de jeu d’amis ou d’autres joueurs connus. Cela permet à un autre chemin d’accès dans une session active de jeu et est également piloté par le MPSD. Titres contrôlent quand les sessions sont joignables et session pour exposer de jointure en cours d’exécution.

### <a name="protocol-activation"></a>Activation de protocole

Si Catherine envoie une invitation à Lisa participer à son jeu, Lisa reçoit une notification sur son appareil qu’elle peut choisir d’accepter ou refuser.

Si elle accepte l’invitation, le système d’exploitation tente de lancer le jeu, si le jeu n’est pas déjà en cours d’exécution et déclenche un événement d’activation qui contient des informations sur la raison pour laquelle le jeu a été activé et des détails supplémentaires (dans le cas d’une invitation, par exemple, le détails incluent l’ID de l’acteur invité, ainsi que le membre a été invité à la session.)

Le processus de gestion de cet événement est appelé à l’activation de protocole et indique que le jeu doit être automatiquement dans un état spécifique, ce qui est détaillé dans les arguments d’événement d’activation. Si le membre ne rejoint pas un jeu multijoueur, l’id de handle de session est spécifié comme l’un des arguments.

Dans le cas de Lisa, accepter l’invitation doit automatiquement démarrer le jeu (si nécessaire) et lui joindre à la même session de jeu en tant que Catherine, sans Lisa ait besoin de prendre les actions supplémentaires.

Peut de l’activation de protocole être déclenché en acceptant une invitation, joindre un autre membre de passionné par le biais de leur carte de visite ou en cliquant sur un profond lié prime.

## <a name="smartmatch-matchmaking"></a>SmartMatch matchmaking

SmartMatch est le nom du service Xbox Live pour matchmaking anonyme. Ce service fait correspondre les joueurs du même jeu selon configurable un ensemble de règles de correspondance.

Le service travaille en étroite collaboration avec le MPSD et utilise des sessions de matchmaking d’entrée et sortie. Matchmaking est effectuée sur le service, ce qui permet des titres pour facilement fournissent d’autres expériences pendant le flux de matchmaking, par exemple joueur dans le titre.

Personnes ou groupes que vous souhaitez entrer matchmaking créer une session de ticket de correspondance, puis demander le service pour rechercher d’autres acteurs avec lesquels vous souhaitez configurer une correspondance. Cela entraîne la création d’une table temporaire « correspondance ticket » résidant dans le service (sur un hopper correspondance) pour une période donnée.

Le service choisit les sessions à lire en fonction de la configuration de la règle, les statistiques stockées pour chaque lecteur et d’autres informations donné au moment de la faire correspondre la demande. Le service crée ensuite une session de cible de correspondance qui contient tous les joueurs ont été mis en correspondance et notifie les titres des utilisateurs de la correspondance.

Lorsque la session cible est prête, titres peuvent effectuer de qualité de service (QoS) des vérifications pour confirmer que le groupe peut jouer ensemble et/ou joindre les joueurs à la session pour commencer le jeu. Cours de la QoS processus et matchmade partie, titres informé l’état de session dans MPSD, et ils recevoir des notifications de MPSD sur les modifications apportées à la session. Ces modifications incluent les utilisateurs rejoint ou quitte et modifications apportées à l’arbitre de session.

### <a name="match-ticket-session"></a>Correspond à la session de ticket

Une session de ticket correspondance représente les clients pour les lecteurs qui souhaitent établir une correspondance. Il est généralement créée en fonction sur un groupe de joueurs qui se trouvent dans une salle d’attente ensemble, ou sur les autres regroupements spécifiques à titre de joueurs. Dans certains cas, la session de ticket peut être une session de jeu déjà en cours d’exécution qui cherche à plus de joueurs.

### <a name="match-ticket"></a>Ticket de correspondance

Envoi d’une session de ticket à matchmaking entraîne la création d’un ticket de correspondance qui assure le suivi de la tentative de matchmaking. Attributs peuvent être ajoutés au ticket, par exemple, le mappage de jeu ou au niveau du lecteur. Ceux-ci, ainsi que les attributs des acteurs dans la session de ticket, sont utilisés pour déterminer la correspondance.

### <a name="hoppers"></a>Exploits en

Exploits en sont des endroits logiques où les tickets de correspondance sont collectés et spécifiés au début de matchmaking. Que les tickets dans le même hopper peuvent être mis en correspondance. Un titre peut avoir plusieurs exploits en mais peut démarrer uniquement matchmaking sur l’un à la fois. Par exemple, un titre peut créer un hopper quel lecteur compétence est l’élément le plus important pour la correspondance. Il peut utiliser un autre hopper dans lequel les joueurs sont mis en correspondance uniquement si elles ont acheté le même contenu téléchargeable.

Vous configurez les exploits en pour matchmaking dans la configuration du service. TBD.

## <a name="quality-of-service-qos"></a>Qualité de service (QoS)

Lorsque les joueurs jouent un jeu multijoueur en ligne, la qualité du jeu est affectée par la qualité de la communication réseau entre les appareils qui hébergent les jeux. Un liaison réseau défectueuse peut entraîner des expériences jeu indésirables comme décalage ou connexion annule en raison de la bande passante ou latence.

Qualité de service fait référence à mesurer la force d’une connexion en ligne (bande passante et latence) entre joueurs pour garantir que tous les joueurs ont une qualité de connexion réseau suffisante. Cela est particulièrement important pour les lecteurs qui sont mises en correspondance pendant matchmaking afin de garantir une bonne expérience en raison de net. Il est moins pertinents pour les invitations où amis jouent ensemble et sont généralement prêt à accepter les conséquences d’une mauvaise connexion.

Vous pouvez configurer la session pour gérer la qualité de service automatiquement en fonction des critères spécifiques, ou votre jeu peut gérer de mesurer la qualité de service chaque fois que tout le monde rejoint la session.
