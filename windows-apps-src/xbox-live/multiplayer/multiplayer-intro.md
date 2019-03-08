---
title: Xbox Live plateforme multijoueur
description: En savoir plus sur les plateformes multijoueurs qui sont pris en charge par Xbox Live.
ms.assetid: 958b94b3-dccd-479a-bf52-54f7ff1656fa
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, mode multijoueur
ms.localizationpriority: medium
ms.openlocfilehash: c71fc28a9bb8668d0253834c1a2c9c6ca7736fd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651154"
---
# <a name="xbox-live-multiplayer-platform"></a>Xbox Live plateforme multijoueur

La plateforme de mode multijoueur Xbox Live Responsabilise votre jeu pour rassembler les joueurs Xbox Live sur Internet et permettent d’augmenter considérablement la durée de vie et l’utilisation d’un titre au-delà play solo classique.

En créant une excellente expérience multijoueur pour votre public, vous pouvez exploiter le grand réseau social de joueurs de Xbox Live pour augmenter la base de votre jeu d’utilisateurs et de promouvoir une Communauté soutenue de fans dédiés qui joue ensemble.


## <a name="what-is-the-xbox-live-multiplayer-platform"></a>Qu’est la plateforme de mode multijoueur Xbox Live ?

La plateforme de mode multijoueur Xbox Live est un ensemble d’API que vous pouvez utiliser pour implémenter le jeu multijoueur en temps réel du client. Les principaux sous-systèmes dans la suite de l’API sont :

-   Le **Xbox Live multijoueurs Session Directory (MPSD)** service. Le service MPSD fonctionne avec des expériences de l’interface utilisateur intégrées pour faciliter aux utilisateurs de recherche et inviter des uns des autres pour le jeu. Intégration aux services Xbox Live permet également aux clients d’utiliser le Chat tiers de Xbox Live à assembler.
-   **Installations de matchmaking simples et avancées.** Xbox Live fournit des fonctionnalités enclos traditionnel, mais également navigation dans la session et la prise en charge pour les scénarios de matchmaking hautement personnalisés. Xbox Live recherche pour le groupe (LFG) permet également de lecteurs pour rechercher d’autres, rally dans Chat tiers et ensuite lire votre jeu.
-   **Homologue à homologue et client-serveur d’API de mise en réseau** obtenir une communication sécurisée en temps réel en tirant parti des normes Internet modernes et surveille activement de Xbox Live. Normalisation et l’intégration avec le réseau Xbox Live, résolution des problèmes d’expériences autoriser les utilisateurs à corriger rapidement les problèmes de connectivité.  
-   **Vocale et texte chat solutions** qui facilitent la communication de dans le jeu safe en tirant parti de la Xbox Live de graphique social, media services et un matériel spécialisé encodage sur les appareils Xbox One.

Pour une vue d’ensemble des scénarios multijoueurs courants et les fonctionnalités Xbox Live peuvent vous aider à implémenter ces scénarios, consultez [niveaux multijoueur](multiplayer-scenarios.md).

## <a name="how-can-i-implement-xbox-live-multiplayer-in-my-game"></a>Comment puis-je implémenter Xbox Live multijoueurs dans mon jeu ?
Selon votre scénario, la plateforme de mode multijoueur Xbox Live offre plusieurs approches pour implémenter Xbox Live multijoueurs dans votre jeu.

### <a name="xbox-integrated-multiplayer-xim"></a>Mode multijoueur intégré Xbox (XIM)
XIM est une solution clé en main pour l’ajout d’une mise en réseau en mode multijoueur en temps réel et la communication à votre jeu grâce à la puissance des services Xbox Live. L’objectif de XIM est pour le rendre plus facile que jamais le développement des jeux multijoueurs haute qualité peer-to-peer (P2P) sur Xbox One & Windows 10.

XIM offre les fonctionnalités suivantes :
- Prise en charge des jeux invitations et matchmaking simple.
- Simple et sécurisé en temps réel réseau est augmenté en toute transparence par le Service de relais de Xbox Live multijoueurs.
- Fonctionnalités vocales et texte conversation simple, avec des fonctionnalités de transcription et la narration de communication pour une expérience utilisateur plus accessible.
- Outils de détection et la gestion de la congestion du réseau, ainsi que pour la migration de l’état jeu.

XIM est la solution la plus simple de multijoueur disponible via la plateforme multijoueur Xbox Live, mais également la moins personnalisable et convient uniquement pour les jeux de P2P. Pour plus d’informations sur XIM, consultez [mode multijoueur intégré Xbox](xbox-integrated-multiplayer.md).

### <a name="xbox-multiplayer-manager"></a>Gestionnaire de mode multijoueur Xbox
Le Gestionnaire de mode multijoueur Xbox (MPM) est un client API qui fournit l’accès flexible à l’annuaire de session multijoueurs de Xbox Live, invitation et les services de matchmaking.

Il implémente les nombreux scénarios multijoueurs courants de manière efficace qui respecte les bonnes pratiques et gère également la plupart des exigences Xbox (XRs) que votre jeu doit implémenter afin de passer la certification.

Le Gestionnaire de mode multijoueur Xbox n’implémente pas une mise en réseau ou une couche de conversation. MPM est conçu comme une flexible mais simplifiée et centralisée multijoueur API de gestion pour votre jeu associé à une couche de mise en réseau sécurisée implémentée via Windows.Networking.XboxLive. Dans le jeu communication peut être ajoutée avec la [jeux 2 Chat](chat/game-chat-2-overview.md) API ou via XIM Chat réservations. Les API de conversation 2 de mise en réseau et de jeux sont documentées dans le XDK une Xbox et Xbox Live plateforme Extensions SDK.

Si vous hébergez des serveurs dédiés pour votre jeu multijoueur, MPM est le meilleur choix. MPM convient également pour les scénarios avancés tels que l’intégration avec tournois de Xbox Live. Pour plus d’informations sur MPM, consultez [Introduction au mode multijoueur Manager](multiplayer-manager/multiplayer-manager-api-overview.md).

Pour utiliser le mode multijoueur Manager, vous devez configurer le service Xbox Live pour vos scénarios multijoueurs. Pour plus d’informations sur cette configuration, consultez [configurer le service multijoueur](service-configuration/configure-the-multiplayer-service.md).

>Mode multijoueur Manager ne gère pas actuellement de navigation dans la session. Pour plus d’informations, consultez [mode multijoueur Session Parcourir](session-browse.md).

### <a name="api-capabilites"></a>Fonctionnalités de l’API

Fonctionnalités | Mode multijoueur intégré Xbox| Gestionnaire de mode multijoueur
--  | -- | --
Visibilité |  XIM est fourni sous forme de bibliothèque compilé sans source.  | MPM est fourni avec la source, et ainsi vous pouvez personnaliser le comportement en interagissant directement avec les services Xbox Live ou XSAPI.
Session et Matchmaking | XIM fournit des règles de matchmaking préconfiguré de façon simple et ne nécessite pas de configuration multijoueur. | MPM [nécessite la configuration du service multijoueur](service-configuration/configure-the-multiplayer-service.md), ce qui permet de spécifier sophistiquée comportement matchmaking et session.
Mise en réseau | XIM fournit un réseau de lecteur au lecteur simple et sécurisé, soutenu par le service Xbox Live relais si nécessaire. | MPM est conçu afin que vous pouvez incorporer dans votre propre solution de mise en réseau sécurisée à l’aide de Windows.Networking.XboxLive.
Conversation de jeu | XIM fournit les conversations vocales et de texte intégrée. | Dans le jeu communication peut être implémentée avec l’API de 2 jeux conversation ou à l’aide de XIM réservations out-of-band pour activer la conversation pour un MPM géré une liste.
