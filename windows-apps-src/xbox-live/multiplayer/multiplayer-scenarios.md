---
title: Scénarios multijoueurs
description: Découvrez les différents scénarios multijoueurs et comment Xbox Live prend en charge ces scénarios.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 338c6e65846ca0ce1307e1e5843f37fe63ecf70a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618294"
---
# <a name="multiplayer-scenarios"></a>Scénarios multijoueurs
Il existe de nombreux types de scénarios multijoueurs et en choisissant le scénario approprié peut accroître l’engagement de lecteur et le lecteur de base pour votre jeu, ce qui contribue à étendre la durée de vie active de votre jeu aussi longtemps que possible.

Même des jeux qui sont essentiellement des expériences de lecteur unique peuvent tirer parti des fonctionnalités de Xbox Live concurrentielles comme les classements, des statistiques ou des éléments de réseaux sociaux.

La liste suivante décrit certains des scénarios multijoueurs plus courants que vous pouvez implémenter avec Xbox Live. La liste est classée en termes de complexité et la quantité de travail requise pour implémenter et tester, et vous devez envisager de ces facteurs lorsque vous choisissez les scénarios de type que vous souhaitez votre jeu pour prendre en charge.

## <a name="comparative-indirect-play"></a>Comparatif Play (Indirect)
Dans ce scénario, des lecteurs sont en concurrence avec eux indirectement, sans jeu directe dans la même instance d’un jeu. Il est parfaitement adaptée pour les jeux qui sont destinés à une expérience de joueur, mais contiennent certains aspects de concurrence qui permettent de comparer comment ils ont fait d’autres joueurs joueurs.

Fonctionnalité d’exemple pour ce scénario inclut :

* Tableaux de résultats, où un joueur essaie d’atteindre la meilleure « score » dans une catégorie par rapport à d’autres joueurs. Cela peut inclure un système de classement des réseaux sociaux pour toute mise en concurrence avec vos amis uniquement.
* Primes et les statistiques, où passionné veut être en mesure de comparer sa progression/performance par rapport à celle de ses amis et éventuellement brag sur les pulsations une prime en particulièrement difficile.
* « Fantôme » ou « virtuel mode multijoueur », où un joueur peut s’affronter par rapport à un enregistrement d’un autre ordinateur de jeu (ou leurs propres) des performances précédentes, comme un tour dans un jeu de course.

Ce type de mode multijoueur peut être obtenu en utilisant les services Xbox Live suivants :

* Présence
* statistiques
* Gestionnaire sociale
* Succès
* Classements
* Stockage connecté

Ce type de mode multijoueur ne nécessite aucune des services spécifiques Xbox Live multijoueurs. Ce scénario de test nécessite plusieurs comptes Xbox Live uniquement.

## <a name="local-play-living-room-play"></a>Play local (salon Play)
Ce type de mode multijoueur repose sur deux ou plusieurs lecteurs de jeu ensemble (par rapport à l’autre, ou coopération) sur un seul appareil. Un titre peut utiliser un seul écran pour tous les acteurs ou une expérience d’écran de fractionnement pour chaque joueur. Dans les jeux de tour basé vous pouvez également utiliser une approche « siège chaud » où chaque joueur prend le contrôle du jeu pendant leur tour.

Sur la console Xbox One plusieurs joueurs peuvent être connectés sur une console unique. Chaque joueur est liée à un contrôleur. Actuellement, les appareils Windows 10 uniquement en charge la connexion d’un seul compte Xbox Live, mais Microsoft cherche à cette modification dans une prochaine mise à jour.

Bien qu’il soit possible de concevoir un seul style de play local de multijoueur à l’aide des services multijoueurs de Xbox Live, il peut être préférable de considérer ce scénario est un sous-ensemble d’un scénario multijoueur plus vaste qui incorpore des expériences en ligne, comme le supplémentaires investissement nécessaire est minime par rapport au retour potentiel d’expansion le scénario multijoueur.

Ce type de mode multijoueur peut utiliser des services similaires en tant que le scénario précédent :

* Présence
* statistiques
* Gestionnaire sociale
* Succès
* Classements
* Stockage connecté

Ce scénario de test nécessite plusieurs comptes Xbox Live et plusieurs contrôleurs sur un seul appareil.

##  <a name="online-play-with-friends"></a>Lire en ligne avec vos amis
Ce scénario est l’expérience multijoueur en ligne la plus traditionnelle. Dans ce scénario, un membre de Xbox Live veut jouer à un jeu avec des amis uniquement, mais il ne souhaite pas jouer avec des inconnus. Vos amis sont soit invités ou joindre un jeu en cours, car il est en cours.

Pour le contrôle parental un titre multijoueur plus large (comme indiqué plus loin) doit avoir la possibilité de limiter multijoueur en ligne à vos amis uniquement et revenir à ce scénario. Contrôles parentaux qui limitent les interactions avec les personnes étrangères sont également appliquées via le service Xbox Live.

Ce type de mode multijoueur peut être obtenu en utilisant les services Xbox Live suivants :

* Gestionnaire de mode multijoueur
* Présence
* statistiques
* Gestionnaire sociale

Ce scénario de test nécessite plusieurs comptes Xbox Live et plusieurs appareils.

## <a name="online-play-through-a-list-of-game-sessions"></a>Jeu en ligne dans une liste de sessions de jeu
Dans ce scénario, un lecteur peut parcourir la liste des sessions de jeu joignable dans un jeu, puis sélectionnez celui que vous souhaitez joindre. Le joueur a également la possibilité de créer des instances de la nouvelle session de jeu en hébergeant localement un jeu. Ces instances de jeux peuvent autoriser des préférences personnalisées (comme mode de jeu, niveau ou jeux de règles). Selon la conception de titre, les sessions de jeu peuvent prendre en charge les restrictions par exemple, exigez un mot de passe à la jointure ou certains niveaux de lecteur/compétence. Ces instances de la session de jeu peuvent également être entièrement publique ou masquées, selon la façon dont votre jeu implémente la session de navigation et la jointure.

Ce scénario permet aux joueurs de sélectionner automatiquement une session de jeu. Cela donne le contrôle à l’acteur, mais il n’existe aucune garantie qu’une session fournira une bonne expérience. Une session ne peut-être pas être remplie avec le lecteur approprié défini au niveau de compétence intéressantes. Une liste de sessions est plus efficace lorsque les jeux ont une petite Communauté multijoueur active.

Ce type de mode multijoueur peut utiliser des services similaires en tant que le scénario précédent :

* Gestionnaire de mode multijoueur
* [Navigation dans la session](session-browse.md)
* Présence
* statistiques
* Gestionnaire sociale

Test de ce scénario nécessite un grand ensemble de comptes Xbox Live et les périphériques à tester avec précision. Les développeurs doivent noter que dynamics player true pour les listes de session peut uniquement être testé avec lecteur grandes bases.

## <a name="online-play-though-looking-for-group"></a>Bien que vous recherchez groupe de jeu en ligne
Ce scénario est semblable au scénario de la liste de sessions, mais il diffère à partir de celui-ci dans les points importants. Au lieu d’une liste de jeux dans le titre de la plateforme fournit des fonctionnalités pour les sessions de jeu de liste en dehors du jeu. Ces publications de recherche pour le groupe sont destinées à fournir une expérience d’autres réseaux sociale et incluent le jeu, les compétences et les restrictions de la relation de réseaux sociaux. Ainsi, un jeu fournir une expérience améliorée sur session répertorie et permet également de contrôler davantage le créateur de la session.

Le lecteur qui a créé la session « recherche de groupe » peut approuver ou refuser les demandes pour rejoindre leur groupe à partir d’autres joueurs. Cela permet aux joueurs de trouver d’autres joueurs qui partagent leurs préférences playstyle.

Le service Xbox Live LFG est une nouveauté pour 2016 et API préliminaires sont devrait être disponible pour les titres en mai.

Ce type de mode multijoueur peut utiliser des services similaires en tant que le scénario précédent :

* Gestionnaire de mode multijoueur
* Recherchez le groupe de Xbox
* Présence
* statistiques
* Gestionnaire sociale
* Service LFG

Test de ce scénario nécessite un grand ensemble de comptes Xbox Live et les périphériques à tester.

## <a name="simple-matchmaking"></a>MatchMaking simple
Dans ce scénario, un lecteur (ou un groupe de joueurs) recherche d’autres joueurs (qui peuvent ou ne peuvent pas être connus à l’acteur) pour un jeu en ligne. Au lieu de sélectionner les amis, le jeu fournit un flux de matchmaking simple qui permet aux joueurs de jointure dans des sessions de jeu avec d’autres joueurs. Le flux de matchmaking dans ce scénario est simple : lecteurs rechercher et trouver d’autres joueurs sans aucune configuration matchmaking significatives. Ce scénario fonctionne mieux avec un large public en ligne.

Pour respecter les joueurs ensemble en conséquence, n’importe quel matchmaking doit tenter de respecter la réputation de listes rouges et vertes sur Xbox Live. Le service Xbox Live SmartMatch gère automatiquement ces restrictions. En respectant les restrictions, comme ces contribue à assurer une plus sûre et la meilleure expérience pour les joueurs.

Une partie importante de matchmaking sont les vérifications de mise en réseau de qualité de Service (QoS). Ces vérifications garantissent que la connectivité réseau entre deux joueurs est suffisante pour une expérience de jeu de bonne. Cela diffère de la lire en ligne avec le scénario d’amis dans la mesure où il est possible de répéter matchmaking et rechercher d’autres joueurs dans le cas d’une connexion réseau incorrecte.

Ce type de mode multijoueur peut utiliser des services similaires en tant que le scénario précédent :

* Gestionnaire de mode multijoueur
* Présence
* statistiques
* Gestionnaire sociale
* SmartMatch matchmaking

Test de ce scénario nécessite un grand ensemble de comptes Xbox Live et les périphériques à tester avec précision. Les développeurs doivent noter que dynamics player true pour les listes de session peut uniquement être testé avec lecteur grandes bases. Outils sont disponibles pour simplifier la condition de réseau et le test de SmartMatch.

## <a name="skill-based-matchmaking"></a>Matchmaking basés sur les compétences
Matchmaking basés sur les compétences est un affinage du scénario de matchmaking simple. Dans ce scénario qu'inclut le service ensembles de règles plus avancées telles que des compétences, au niveau du lecteur et autres propriétés spécifiques au jeu. Le service utilise des règles qui utilisent ces correspondent à des paramètres pour rechercher une session de plus haute qualité pour le lecteur. En fonction de la conception de jeux paramètres de correspondance sont configurées directement par le lecteur ou sont définies automatiquement par le titre.

Xbox Live SmartMatch fournit un ensemble de règles qui peuvent être utilisées pour matchmaking basés sur les compétences. Le service de SmartMatch utilise une fonctionnalité de Xbox Live appelée TrueSkill permettant d’évaluer la compétence relative d’un lecteur.

Ce type de mode multijoueur peut utiliser des services similaires en tant que le scénario précédent :

* Gestionnaire de mode multijoueur
* Présence
* statistiques
* Gestionnaire sociale
* SmartMatch matchmaking

Test de ce scénario nécessite un grand ensemble de comptes Xbox Live et les appareils. Les développeurs doivent noter que dynamics player true pour les listes de session peut uniquement être testé avec lecteur grandes bases. Outils sont disponibles pour simplifier la condition de réseau, TrueSkill et SmartMatch test.

## <a name="tournaments"></a>Tournois
Tournois permettent aux joueurs compétents entrer en concurrence avec eux dans un format organisé. En règle générale, tournois sont hébergés et organisées par indépendants des organisations tierces.

Xbox Live a ajouté une nouvelle fonctionnalité dans 2016 pour permettre des titres d’utiliser matchmaking Xbox Live en conjonction avec organisateurs de tournoi tiers. Cette fonctionnalité permet des titres intégrer en toute transparence tournois avec l’expérience d’interpréteur de commandes Xbox, sans nécessiter de joueurs de s’inscrire sur des sites Web externes.

Ce type de mode multijoueur est possible en utilisant les services Xbox Live telle que :

* Présence
* statistiques
* Social
* Réputation
* Mode multijoueur
* SmartMatch matchmaking
* Tournois

Test de ce scénario nécessite un grand ensemble de comptes Xbox Live et les appareils.
