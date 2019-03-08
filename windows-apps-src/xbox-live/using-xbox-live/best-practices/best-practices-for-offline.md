---
title: Meilleures pratiques pour le mode hors ligne
description: En savoir plus sur les meilleures pratiques pour la gestion des scénarios hors connexion avec un titre de Xbox Live est activé.
ms.assetid: 6290dd67-1145-4fe2-8ada-c3a29a9ad29a
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, hors connexion
ms.localizationpriority: medium
ms.openlocfilehash: 5680b045873ebf6eed1422cb915f3f53df748790
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618724"
---
# <a name="best-practices-for-offline"></a>Meilleures pratiques pour le mode hors ligne

Xbox One est conçu comme un appareil connecté, et une expérience optimale, telles que les jeux multijoueur et vidéo en flux continu, nécessite une connectivité. Toutefois, Xbox One prend en charge plusieurs formes de lecture hors connexion. Cette rubrique explique comment gérer la lecture hors connexion.

## <a name="overview"></a>Vue d’ensemble

Un nombre croissant de consommateurs dans le monde entier ont accès à Internet. Toutefois, il existe toujours endroits du monde où la connectivité est imprévisible et certains cas d’échec des routeurs, fiber est coupé, serveurs se bloquer, ou supprimer des services sans fil.

Pour prendre en charge le plus large ensemble de consommateurs et des expériences, Xbox One permet pour les cas courants où une connectivité Internet est perdu de temps à autre, ou n’est pas disponible complètement. Votre jeu est informé des problèmes de connectivité, et vous êtes libre de choisir comment répondre — si qui se poursuit le jeu complet, la rétrogradation vers un mode hors connexion, ou se terminant par le jeu complètement.

## <a name="normal-online-operation"></a>Fonctionnement normal en ligne

En règle générale, les consoles Xbox One fonctionnent dans un état totalement connecté, où l’utilisateur a un stable connexion Internet et un accès complet à Xbox Live et à des services tiers. Cet état connecté permet au service Xbox Live valider périodiquement l’état de la console, pour fournir des mises à jour et effectuer d’autres services d’arrière-plan qui bénéficient de votre jeu et les utilisateurs.

Nous recommandons que vous supposez que les utilisateurs ont une connectivité en ligne la plupart du temps.

## <a name="offline-play-principles"></a>Principes de la lecture hors connexion

Il existe des cas où une connexion en ligne n’est pas disponible. De la lecture hors connexion, Xbox One est conçu avec les principes suivants à l’esprit :

-   Plus important encore, les utilisateurs continuer à jouer en dépit des problèmes de connectivité.

-   Les utilisateurs continuer à jouer même s’ils n’ont aucune connexion du tout.

-   Rendre play hors ligne simple et prévisible pour les utilisateurs, tout en conservant l’esprit d’une expérience toujours connectés.

## <a name="offline-modes"></a>Modes hors connexion

Il existe deux principales *une perte de connectivité* scénarios :

-   Perte complète de service Internet

-   Perte d’un ou plusieurs services en ligne

Dans chacun de ces modes, il existe une multitude de situations qui peuvent se produire. Ils sont expliqués ci-dessous avec les exemples de scénarios courants en mode hors connexion qui affectent le jeu.

## <a name="offline-scenario-no-internet-service-upon-game-start"></a>Scénario hors connexion : aucun service Internet au démarrage de jeu

Jeux peuvent déclarer eux-mêmes comme l’un des trois types :

-   *Xbox Live requis*: tous les modes de jeu nécessitent une connexion Internet.

-   *Xbox Live Gold requis*: tous les modes de jeu nécessitent une connectivité Internet et un membre Xbox Live Gold.

-   *Xbox Live inutile*: le jeu a au moins un mode de lecture ne nécessitant pas de connectivité Internet. Techniquement, ce type n’est pas explicitement déclaré dans le manifeste d’application. Une application qui ne se déclare elle-même comme un des deux premiers types est considéré comme « Xbox Live ne pas obligé, » ou pris en charge en mode hors connexion.

Quand un jeu est démarré par l’utilisateur et la console est hors connexion, le système vérifie la déclaration de la connectivité de jeu dans le manifeste d’application. Si le jeu nécessite une connexion (une des deux premiers cas ci-dessus), le système affiche un message à l’utilisateur automatiquement et ne pas lance le titre.

Si la console est hors connexion, le système lance uniquement les titres qui ont au moins un mode de lecture ne nécessitant pas de connectivité. En d’autres termes, le système ne lancera pas « Xbox Live requis » ou « Gold Live Xbox requis » des titres.

## <a name="offline-scenario-connectivity-lost-during-gameplay"></a>Scénario hors connexion : connectivité perdue pendant la session de jeu

Si un jeu est déjà en cours d’exécution lorsque la connexion est perdue, le titre est communiquée par le système. Si le jeu n’utilise pas de services en ligne, passez à la session sans interruption. Si le jeu utilise activement des services en ligne, basculez vers un mode dans lequel ces services ne sont plus nécessaires ou informer le lecteur que la session de jeu se termine en raison de l’état hors connexion.

Les titres qui sont déclarés « Xbox Live requis » ou « Gold Live Xbox requis » seront automatiquement suspendu par le système lorsque la console perd toute la connectivité réseau pour une période de temps et le système fournira automatiquement un message d’erreur à l’utilisateur.

Comme avec tout autre scénario impliquant la suspension de jeu, vous devez enregistrer état afin que l’utilisateur ne perdre des données et pouvez rapidement revenir à cet état après la reprise de connectivité.

## <a name="offline-scenario-when-a-single-xbox-live-service-is-down"></a>Scénario hors connexion : quand un seul service Xbox Live est arrêté

Il existe des autres situations dans lequel les services en ligne spécifiques est peut-être indisponibles même si la connectivité Internet est parfaite.

Par exemple, un seul service Xbox Live peut être hors connexion pendant une courte période de temps. Dans ce cas, l’appel à spécifique au service arrivera à expiration ou signaler une erreur dans le jeu. Vous devez traiter correctement les États de service hors connexion tout comme vous le feriez pour des situations sur Xbox 360 ou sur Windows.

Au minimum, le jeu doit se bloque pas ou se bloquer. Si le jeu ne peut pas continuer sans que le service, la situation à l’utilisateur de rapports et autoriser l’utilisateur à continuer dans une autre zone du jeu où le service n’est pas requis.

Dans le meilleur des cas, continuer le jeu et le cache des données pour envoyer ultérieurement (si le jeu a été écrit pour le service) ou des hypothèses raisonnable sur les données (si le jeu a été lecture à partir du service).

## <a name="offline-scenario-when-a-third-party-service-is-down"></a>Scénario hors connexion : quand un service tiers est arrêté

Si votre jeu s’appuie sur un service en ligne par des tiers, puis votre jeu doit également être résilient en cas d’échec de ce service. Si le service échoue, puis votre jeu doit se bloque pas ou se bloquer. Vous pouvez signaler l’erreur de service à l’utilisateur si le jeu ne peut pas continuer. Dans l’idéal, le jeu doit se poursuivre, ou vous devez autoriser l’utilisateur à continuer dans une zone de jeu qui ne nécessite pas le service en ligne.

## <a name="offline-scenario-when-xbox-live-cloud-compute-is-down"></a>Scénario hors connexion : lorsque le calcul de cloud Xbox Live est arrêté

La présente Xbox One est puissance du cloud. Certains jeux peut s’appuyer totalement sur un service toujours connectées, telles que le calcul cloud Xbox Live, qui permet d’accéder aux fonctionnalités de calcul supplémentaires ou des serveurs de jeux toujours disponible. Ce mode toujours connecté à la fois autorisé et encouragé où il améliore l’expérience des joueurs.

Si votre jeu utilise ce mode, il est recommandé que votre jeu soit résistante aux interruptions de service (de plusieurs secondes et jusqu'à une minute) qui sont en raison d’une perte totale de connectivité à Internet ou de perte d’un service cloud particulier. Toutefois, le jeu n’est pas nécessaire d’avoir un mode hors connexion. Si le jeu a réellement besoin de connectivité et de connectivité n’est pas disponible, puis informe l’utilisateur et terminer la session de jeu.

## <a name="xbox-requirements"></a>Exigences de Xbox

Le plus important lorsque vous traitez des scénarios hors connexion est la stabilité du jeu. Qu’il s’agisse de la perte de connectivité complète ou simplement la perte d’un service en ligne spécifique, votre jeu ne doit pas se bloquer, se bloquer ou l’utilisateur peut perdre d’état. Votre jeu doit avoir un système robust pour la gestion des scénarios de délai d’expiration réseau et de gestion des erreurs retournées à partir de n’importe quelle API qui accède à un service en ligne.

Jeux ne sont pas requis pour prendre en charge la lecture hors connexion. Si votre jeu, simplement, ne peut pas continuer en raison d’une perte de connectivité du service, puis avertir l’utilisateur, mettre fin à la session de jeux, revenez au menu principal ou d’un état interactif initial.

## <a name="best-practices"></a>Meilleures pratiques

Voici les meilleures pratiques pour gérer les situations hors connexion :

-   Concevez des jeux de supposer que les utilisateurs ont une connectivité en ligne la plupart du temps.

-   Si cela est pertinent pour votre conception de jeu, puis envisagez de concevoir des modes de jeu permettant aux utilisateurs d’avoir une expérience plus agréable, même si la console est dans un état hors connexion.

-   Services peuvent devenir indisponibles. Connexions peuvent échouer. Générer des erreurs robuste de gestion des API qui peut le délai d’attente, ou signaler des conditions de défaillance lors d’un service en ligne est en panne ou lorsqu’une connexion Internet est perdue. Privilégiez les utilisateurs à jouer en dépit de ces problèmes.

-   Obéissent aux exigences de Xbox (XRs). Ne pas se bloquer ou se bloquer.

-   Lorsqu’il reçoit une notification de suspension du titre PLM, enregistrer l’état afin que l’utilisateur ne perdre des données et pouvez rapidement revenir à cet état lors de la reprise du jeu.

-   Indicateur le titre de la manière appropriée dans le manifeste d’application. Uniquement marquer votre titre que comme « Xbox Live requise » si tous les modes de jeu nécessitent une connectivité.

-   Jeux Xbox One sont autorisés à utiliser et s’appuient sur les services en ligne dans tous les modes de jeu. Si le jeu ne peut pas simplement continuer en raison d’une perte de connectivité du service, puis avertir l’utilisateur, mettre fin à la session de jeux, revenez au menu principal ou d’un état interactif initial.

-   Ne comptez pas sur le service d’aide de Xbox pour les messages d’erreur et aide associées aux États hors connexion. Le service d’aide de Xbox requiert une connexion à la Xbox Live.

## <a name="conclusion"></a>Conclusion

Xbox One est conçu pour un monde toujours connecté. Toutefois, sur la façon de cette vision, la console permet de joueurs bénéficier de scénarios hors connexion. Jeux doivent être robustes face aux défaillances de connectivité. Pour les jeux de lecture hors connexion des expériences, autoriser vos joueurs bénéficier de ces expériences de manière optimale. Pour les jeux qui sont toujours en ligne lorsque la connectivité réseau est perdure, renvoyez de manière appropriée l’utilisateur à un état hors connexion.
