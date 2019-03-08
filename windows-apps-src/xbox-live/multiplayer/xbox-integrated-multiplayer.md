---
title: Mode multijoueur intégré Xbox
description: Découvrez le mode multijoueur intégré Xbox (XIM), une solution tout en un de jeu multijoueur/réseau/chat pour les jeux du Xbox Live.
ms.assetid: edbb28e6-1b6c-4f12-a9c6-fa8961de99a8
ms.date: 01/25/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, mode multijoueur intégré xbox
localizationpriority: medium
ms.openlocfilehash: aa82b1042f710d81d83b98767802c7538fd53fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641774"
---
# <a name="xbox-integrated-multiplayer-xim"></a>Mode multijoueur intégré Xbox (XIM)

- [Vue d’ensemble](#overview)
- [Concepts](#concepts)
- [Fonctionnalités](#features)
- [Relation aux autres modules](#relationship-to-other-modules)

## <a name="overview"></a>Vue d’ensemble

Le mode multijoueur intégré Xbox (XIM) constitue une interface autonome pour l'ajout aisé de réseaux multijoueur en temps réel et de communication chat à votre jeu grâce à la puissance des services Xbox Live. L’interface XIM ne nécessite pas d’un projet à choisir entre la compilation avec C / c++ / CX et C++ traditionnel ; Il peut être utilisé avec le. L’implémentation n’également lever des exceptions comme un moyen d’erreur non irrécupérable reporting donc vous pouvez l’utiliser facilement des projets sans exception si vous préférez.

Pour commencer, consultez [Utilisation du mode XIM](xbox-integrated-multiplayer/using-xim.md). Si vous utilisez C#, consultez [XIM à l’aide de C# ](xbox-integrated-multiplayer/using-xim-cs.md).

## <a name="concepts"></a>Concepts

XIM est axé sur les quelques concepts clés :

- Réseau XIM - une représentation logique d’un ensemble d’utilisateurs interconnectés participant à une expérience multijoueur particulier, ainsi que des état de base qui décrit cette collection. Les participants peuvent utiliser uniquement un réseau XIM à la fois, mais peuvent basculer d'un réseau XIM conceptuel à un autre de manière transparente.
- Matchmaking - le processus facultatif de découverte des lecteurs à distance supplémentaires avec les mêmes intérêts ou niveaux de compétence de participer à un réseau XIM sans nécessiter une relation de réseaux sociaux existante.
- Interrogation - le processus facultatif de découverte XIM réseaux sans nécessiter une relation de réseaux sociaux existante entre les participants.
- `xim_player` -Un objet représentant un utilisateur humain connecté sur un appareil local ou distant et participent à un réseau XIM. Un seul utilisateur physique qui rejoint, quitte, puis rejoint le même réseau XIM est considéré comme deux instances de joueur distinctes.
- `xim_state_change` -Une structure qui représente une notification sur l’appareil local concernant une modification asynchrone de certains aspects du réseau XIM.
- `xim::start_processing_state_changes` et `xim::finish_processing_state_changes` -la paire de méthodes appelé par l’application pour chaque trame de l’interface utilisateur d’effectuer des opérations asynchrones, pour récupérer les résultats doivent être traitées dans le formulaire de `xim_state_change` structures, puis pour libérer les ressources associées, une fois.

À un niveau très élevé, l’application game utilise la bibliothèque XIM pour configurer un ensemble d’utilisateurs connecté-dans sur l’appareil local pour être inséré dans un réseau XIM en tant que nouveaux lecteurs. Les appels de l’application `xim::start_processing_state_changes` et `xim::finish_processing_state_changes` chaque frame d’interface utilisateur. Comme les instances de l’application sur des périphériques distants ajouter leurs utilisateurs dans un réseau XIM, chaque instance de participant est fourni `xim_state_change` mises à jour décrivant les locaux et distants `xim_player`s rejoindre ce réseau XIM. Lorsqu'un joueur cette de participer au réseau XIM (volontairement ou en raison de problèmes de connectivité du réseau), les mises à jour `xim_state_change` sont fournies à toutes les instances d'application en indiquant que le joueur `xim_player` a quitté le jeu.

Une application peut déterminer le réseau XIM auquel participer de diverses manières. Bien souvent, l'application commence par déplacer automatiquement les utilisateurs locaux vers un nouveau réseau disponible aux amis des utilisateurs, dans lequel les utilisateurs locaux peuvent envoyer des invitations ou identifier leur réseau XIM dans le cadre d'une activité joignable (via les cartes des joueurs, par exemple). Lorsque ces utilisateurs socialement identifiés sont prêts, l'application peut démarrer le processus de « matchmaking » Xbox Live et déplacer tous les joueurs vers un nouveau réseau XIM contenant également les joueurs distants « matchés » pour remplir vos listes d'équipe/opposant comme souhaité. Ensuite, lorsque cette expérience multijoueur s'achève, les instances d'application peuvent déplacer leurs joueurs locaux (et, facultativement, les joueurs distants d'origine pré-matchés) à nouveau vers un réseau XIM privé ou vers un autre réseau XIM aléatoire et identifié lors du matchmaking. Le chat vocal et textuel reste disponible tout au long de l'expérience. Cette facilité de déplacement des joueurs d'un réseau XIM à un autre est un aspect centrale de l'API et reflète les attentes modernes d'expériences de jeu satisfaisantes et très sociales.

Par opposition à un modèle client-serveur, un réseau XIM est logiquement une maille totalement connectée d’appareils de l’homologue. Comme décrit dans la section de ce document, n’importe quel lecteur peut envoyer directement à n’importe quel autre via l’API. Toutes les méthodes qui affectent l’état du réseau XIM dans son ensemble peuvent être appelées par n’importe quel appareil participante.

XIM utilise la résolution des conflits d’écriture-last-wins simple si l’application n’empêche pas plus d’un participant de modification de l’état du réseau XIM même en même temps efficacement dans le cas contraire. Cela signifie que XIM n’impose pas les concepts de rôle pour « host » ou « serveur ». XIM n’également limiter les applications d’utiliser leurs propres concepts, tels que la prise en charge pour migrer des rôles définis par l’application à un autre participant lorsqu’un joueur quitte un réseau XIM.

## <a name="features"></a>Fonctionnalités

- Fournit un jeu avec la communication de conversations vocales et de texte qui observe et respecte les paramètres de confidentialité de lecteur

    La communication par chat vocal ou textuel est également automatiquement mise à disposition de tous les joueurs lorsque les paramètres de confidentialité et la configuration de l'application le permettent. Pour les joueurs ayant activé la conversion de synthèse vocale par texte ou de texte par synthèse vocale, XIM réalisera de manière transparent la traduction pour livrer les messages textuels de chat représentant l'audio vocal entrant, et lira un audio vocal synthétisé pour les messages textuels du chat, de manière respective.

- Permet de jeu envoyer des messages de ses propres données spécifiques de jeu

    Au sein du réseau XIM, l'application peut envoyer ses propres messages de données spécifiques au jeu, notamment les mises à jour des mouvements d'avatar. Tous les messages reçus sont livrés à l'application sous la forme `xim_state_change`, en indiquant la source visée et les destinations locales.

- Fonctions comme une solution dédiée conversation via les réservations de bande

    Pour obtenir la documentation détaillée relative à l'utilisation du mode XIM via des réservations hors bande, consultez [Réservations XIM](xbox-integrated-multiplayer/xim-reservations.md).

- Sans exception et peut être utilisé avec un C + c++ / CX ou C++ traditionnel

## <a name="relationship-to-other-modules"></a>Relation aux autres modules

Le mode XIM a été conçu pour fournir une interface tout en un pratique à des jeux ayant des besoins multijoueur basiques. Il englobe la fonctionnalité de plusieurs modules, en particulier le module `multiplayer_manager` d'API de service Xbox (XSAPI), la bibliothèque `xbox::services::game_chat_2` et le réseau multijoueur sécurisé `Windows::Networking::XboxLive`, dans le cadre d'un API simplifié. Cela a pour effet de réduire la quantité typique de code, de tâches et de concepts impliqués dans la génération de jeux multijoueur ne nécessitant pas une flexibilité ou un contrôle maximum absolu. Néanmoins, les applications dont les exigences ne sont pas alignées avec les suppositions de simplification du mode XIM peuvent souhaiter utiliser ces composants plutôt directement.

Alors que le mode XIM a été conçu pour éliminer le besoin de gérer les éléments tels que les document de session Multiplayer Session Directory (MPSD) ou un transport de réseau via les composants sous-jacents, il n'écarte pas leur utilisation simultanée/en cohabitation dans le cadre d'une liste de joueurs distincte ou d'un maillage de communication. Dans ce cas, l'application est responsable de garantir l'utilisation coopérative des ressources réseau entre le mode XIM et ses propres mécanismes. À l'heure actuelle, le mode XIM prend en charge les « réservations hors bande » pour faciliter son utilisation en tant que solution de chat dédiée dont la liste d'utilisateurs est dirigée uniquement par une entrée externe.

Xbox Live fournit de nombreuses fonctionnalités supplémentaires. Elles apportent de la valeur aux jeux multijoueurs, mais elles sont moins directement impliquées dans la configuration d'un chat multijoueur et d'une communication réseau. De ce fait, elles ne sont pas incorporées dans ce module. Les applications sont encouragées à rechercher à l’API de Services Xbox (XSAPI) primes de lecteur, tableaux de résultats, le stockage et bien plus encore.
