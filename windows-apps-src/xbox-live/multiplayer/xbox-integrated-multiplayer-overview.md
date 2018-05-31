---
title: Vue d’ensemble du mode multijoueur intégrée Xbox
author: KevinAsgari
description: Découvrez le mode multijoueur intégré Xbox (XIM), une solution tout en un de jeu multijoueur/réseau/chat pour les jeux du XboxLive.
ms.assetid: edbb28e6-1b6c-4f12-a9c6-fa8961de99a8
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, jeux, uwp, windows10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5af9a5c386dc620c19183747750081ea1bd5a66d
ms.sourcegitcommit: db09dcb08da5995c46c2729896e56be3774ee5ba
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/07/2018
ms.locfileid: "1563392"
---
# <a name="xbox-integrated-multiplayer-overview"></a>Vue d’ensemble du mode multijoueur intégrée Xbox

 Le mode multijoueur intégré Xbox (XIM) constitue une interface autonome pour l'ajout aisé de réseaux multijoueur en temps réel et de communication chat à votre jeu grâce à la puissance des services XboxLive. Il est orienté autour de quelques concepts clés:

 - **Réseau XIM** - Une représentation logique d'un ensemble d'utilisateurs interconnectés au sein d'une expérience multijoueur particulière, ainsi qu'un état basique décrivant cette collection. Les participants peuvent utiliser uniquement un réseau XIM à la fois, mais peuvent basculer d'un réseau XIM conceptuel à un autre de manière transparente.
 - `xim_player` - Un objet représentant un utilisateur humain individuel inscrit sur un appareil local ou distant et participant dans un réseau XIM. Un seul utilisateur physique qui rejoint, quitte, puis rejoint le même réseau XIM est considéré comme deux instances de joueur distinctes.
 - `xim_authority` - Un objet représentant une entité unique avec l'autorisation et la responsabilité de gérer un affichage cohérent de l'état spécifique au jeu dans un réseau XIM pour tous les participants. L'utilisation de cet objet est facultative. À l'heure actuelle, sa fonctionnalité est également indisponible dans cette version logicielle.
 - `xim_state_change` - Une structure représentant une notification à l’appareil local concernant une modification asynchrone dans certains aspects du réseau XIM.
 - `xim::start/finish_processing_state_changes` - La paire de méthodes appelée par l'application pour chaque trame d'interface utilisateur pour réaliser des opérations asynchrones, pour récupérer des résultats à traiter sous la forme de structures `xim_state_change`, puis pour libérer les ressources associées une fois ces opérations terminées.
 - **Matchmaking** - Le processus facultatif de découverte de joueurs distants supplémentaires avec des intérêts ou des niveaux de compétence similaires pour participer à un réseau XIM sans nécessité de cultiver une relation sociale.

À un niveau très élevé, l'application utilise la bibliothèque pour configurer un ensemble d'utilisateurs sur l'appareil local et à déplacer vers un réseau XIM en tant que nouveaux joueurs. Dans la mesure où les instances des appareils distants procèdent de la même manière avec leurs propres utilisateurs et que chaque état de processus de démarrage + de fin continuels modifie chaque trame de l'interface utilisateur, chaque instance participante reçoit les mises à jour `xim_state_change` décrivant les joueurs `xim_player` locaux et distants rejoignant ce réseau XIM.

Au sein du réseau XIM, l'application peut envoyer ses propres messages de données spécifiques au jeu, notamment les mises à jour des mouvements d'avatar. Celles-ci peuvent cibler les autres joueurs ou un `xim_authority` automatiquement sélectionné pour résider sur l'un des appareils participants (en fonction de la meilleure qualité de réseau, de la stabilité, de la réputation du joueur et d'autres facteurs). Ainsi, l'application de cet appareil `xim_authority` peut envoyer des messages en retour aux joueurs, en les fusionnant de manière typique pour l'efficacité du réseau et pour arbitrer en cas de conflit. Tous les messages reçus sont livrés à l'application sous la forme `xim_state_change`, en indiquant la source visée et les destinations locales.

La communication par chat vocal ou textuel est également automatiquement mise à disposition de tous les joueurs lorsque les paramètres de confidentialité et la configuration de l'application le permettent. Pour les joueurs ayant activé la conversion de synthèse vocale par texte ou de texte par synthèse vocale, XIM réalisera de manière transparent la traduction pour livrer les messages textuels de chat représentant l'audio vocal entrant, et lira un audio vocal synthétisé pour les messages textuels du chat, de manière respective.

Lorsqu'un joueur cette de participer au réseau XIM (volontairement ou en raison de problèmes de connectivité du réseau), les mises à jour `xim_state_change` sont fournies à toutes les instances d'application en indiquant que le joueur `xim_player` a quitté le jeu. Si l'appareil sélectionné en tant que `xim_authority` part, les instances d'application sont informés de la même manière que l'autorité a commencé à se déplacer et à démarrer la «réconciliation» de tout état spécifique au jeu en fournissant de manière facultative un tampon de données au remplacement `xim_authority` à des fins de nouvelle synchronisation. La nouvelle autorité est en mesure d'interpréter les données de réconciliation de tous les joueurs comme souhaité, et de fournir en retour un tampon de données «réconcilié» à tous les joueurs pour achever le déplacement de l'autorité et le processus de nouvelle synchronisation. La même procédure de réconciliation basique est également offerte aux nouveaux appareils participants afin que l'autorité soit en mesure de leur fournir de manière pratique l'état de jeu d'autorité lorsque leurs joueurs rejoignent le réseau XIM, sans code supplémentaire. Notez que, à l'heure actuelle, les modifications d'état d'autorité ne sont pas fournis dans cette version logicielle.

Une application peut déterminer le réseau XIM auquel participer de diverses manières. Bien souvent, l'application commence par déplacer automatiquement les utilisateurs locaux vers un nouveau réseau disponible aux amis des utilisateurs, dans lequel les utilisateurs locaux peuvent envoyer des invitations ou identifier leur réseau XIM dans le cadre d'une activité joignable (via les cartes des joueurs, par exemple). Lorsque ces utilisateurs socialement identifiés sont prêts, l'application peut démarrer le processus de «matchmaking» Xbox Live et déplacer tous les joueurs vers un nouveau réseau XIM contenant également les joueurs distants «matchés» pour remplir vos listes d'équipe/opposant comme souhaité. Ensuite, lorsque cette expérience multijoueur s'achève, les instances d'application peuvent déplacer leurs joueurs locaux (et, facultativement, les joueurs distants d'origine pré-matchés) à nouveau vers un réseau XIM privé ou vers un autre réseau XIM aléatoire et identifié lors du matchmaking. Le chat vocal et textuel reste disponible tout au long de l'expérience. Cette facilité de déplacement des joueurs d'un réseau XIM à un autre est un aspect centrale de l'API et reflète les attentes modernes d'expériences de jeu satisfaisantes et très sociales.

Pour commencer, consultez [Utilisation du mode XIM](xbox-integrated-multiplayer/using-xim.md).

## <a name="xims-relationship-to-other-modules"></a>Relation du mode XIM à d'autres modules

Le mode XIM a été conçu pour fournir une interface tout en un pratique à des jeux ayant des besoins multijoueur basiques. Il englobe la fonctionnalité de plusieurs modules, en particulier le module `multiplayer_manager` d'API de service Xbox (XSAPI), la bibliothèque `Microsoft::Xbox::GameChat` et le réseau multijoueur sécurisé `Windows::Networking::XboxLive`, dans le cadre d'un API simplifié. Cela a pour effet de réduire la quantité typique de code, de tâches et de concepts impliqués dans la génération de jeux multijoueur ne nécessitant pas une flexibilité ou un contrôle maximum absolu. Néanmoins, les applications dont les exigences ne sont pas alignées avec les suppositions de simplification du mode XIM peuvent souhaiter utiliser ces composants plutôt directement.

Alors que le mode XIM a été conçu pour éliminer le besoin de gérer les éléments tels que les document de session Multiplayer Session Directory (MPSD) ou un transport de réseau via les composants sous-jacents, il n'écarte pas leur utilisation simultanée/en cohabitation dans le cadre d'une liste de joueurs distincte ou d'un maillage de communication. Dans ce cas, l'application est responsable de garantir l'utilisation coopérative des ressources réseau entre le mode XIM et ses propres mécanismes. À l'heure actuelle, le mode XIM prend en charge les «réservations hors bande» pour faciliter son utilisation en tant que solution de chat dédiée dont la liste d'utilisateurs est dirigée uniquement par une entrée externe.

XboxLive fournit de nombreuses fonctionnalités supplémentaires. Elles apportent de la valeur aux jeux multijoueurs, mais elles sont moins directement impliquées dans la configuration d'un chat multijoueur et d'une communication réseau. De ce fait, elles ne sont pas incorporées dans ce module. Les applications sont encouragées pour se diriger vers les API de services Xbox (XSAPI) pour découvrir les commentaires et les succès des joueurs, les classements, les stockages, et bien d'autres éléments.


## <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>Utilisation du mode XIM en tant que solution de chat dédiée via des réservations hors bande

La plupart des applications utilisent le mode XIM pour traiter tous les aspects du rapprochement des joueurs. Après tout, la mise au point concernant l'assemblage de toutes les fonctionnalités nécessaire à la prise en charge de scénarios multijoueur communs de bout en bout est la raison pour laquelle ce mode est nommé «multijoueur intégré Xbox». Néanmoins, certaines applications implémentant leurs propres solutions de mise en raison à l'aide de serveurs Internet dédiés souhaiteraient également profiter des avantages conférés par l'établissement fiable, à faible latence et à faible coût de la communication par chat pair à paire. Le mode XIM reconnaît ce besoin. Il prend actuellement en charge un mode d'extension visant à offrir l'avantage de sa communication de pair simplifiée pour augmenter la gestion interne des joueurs se produisant en dehors de l'API XIM.

> Pour obtenir la documentation détaillée relative à l'utilisation du mode XIM via des réservations hors bande, consultez [Réservations XIM](xbox-integrated-multiplayer/xim-reservations.md).
