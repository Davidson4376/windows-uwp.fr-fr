---
title: Statistiques joueur
description: Introduction aux statistiques 2017
ms.date: 07/02/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, statistiques, tableaux de résultats, statistiques 2017
ms.localizationpriority: medium
ms.openlocfilehash: eabcc655afa5a9d58ca8ef45569d099e503ebc32
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650864"
---
# <a name="stats-2017"></a>Statistiques 2017

Statistiques 2017 permet aux développeurs de configurer des statistiques pour les joueurs individuels indiquant la progression et brûlante dans le jeu. Ces statistiques sont un outil de réseaux sociaux qui permettra de lecteurs pour gagner en compétitivité avec leurs amis et du titre de la plus grande Communauté, ainsi que présentant certaines de ses fonctionnalités et défis que votre titre a à offrir. Si vous avez un jeu de course avec proposées statistiques telles que les dérives de la plus longue et meilleur temps de blocage, vous pouvez communiquer le type de jeu de course que vos joueurs peuvent s’attendre. Voyez comment pile joueurs par rapport à leurs amis et de la Communauté dans son ensemble leur permettra plus marge de manœuvre pour acheter et lire votre titre. Lecteurs affiche des statistiques proposées GameHub du titre. Statistiques proposées peuvent également régulièrement apparaître dans des blocs de contenu épinglés que les utilisateurs peuvent ajouter à leur vue d’accueil.

Statistiques 2017 fonctionne en acceptant les valeurs statistiques en tant que paires clé / valeur à partir de votre titre pour un lecteur et en stockant cette valeur stat afin qu’il peut être affiché ultérieurement. Statistiques 2017 est conçu pour prendre en charge des scénarios de classement Xbox Live en enregistrant des statistiques sur les joueurs de façon individuelles afin de pouvoir être comparées et classées sur un classement plus tard. Statistiques 2017 accepte les valeurs qui lui sont envoyées avec peu ou aucune validation donc c’est à votre titre pour gérer toute la logique qui détermine la valeur correcte pour une statistique.

## <a name="configured-stats-and-featured-leaderboards"></a>Statistiques configurés et les classements recommandés

Les statistiques sont configurés sur le [tableau de bord de centre de développement Windows](https://developer.microsoft.com/en-us/dashboard/windows/overview). Pour configurer des statistiques, que vous devez déjà disposer d’un titre configuré. Si vous n’avez pas configuré un titre de vous apprendre à le faire en lisant [créer et tester le titre d’un nouveau créateur](../get-started-with-creators/create-and-test-a-new-creators-title.md).  Les statistiques que vous configurez dans partenaires seront affiche dans GameHub du votre titre comme indiqué dans l’illustration :

Le *fonctionnalité statistiques* sont mises en surbrillance en jaune dans l’image ci-dessous.
![Classement de Page sociale Club officiel](../images/omega/gamehub_featuredstats.png)


Sur Xbox One, les utilisateurs peuvent épingler des jeux directement à leur écran d’accueil pour trouver rapidement des informations pertinentes, de contenus de proposés par la Communauté et de billets de développeur. Les statistiques que vous configurez peuvent également apparaître comme une partie du contenu d’accueil épinglé. Statistiques affichera le lecteur avec un ami classé légèrement plus élevé, en les encourageant à jouer à leur façon du classement. Tableaux de résultats dans le contenu épinglé s’affiche comme illustré dans l’image suivante.

![Halo 5 épinglé classement](../images/stats/Halo_5_Pinned_Leaderboard.png)

Comparer les statistiques proposées en cours de jeu basé sur des statistiques configurés avec les autres amis qui jouent également le titre. Cela encourage plus de temps de lecture pour votre titre lorsque vos amis sont en concurrence pour les meilleurs scores ou simplement créer une liaison une expérience partagée. Classements Stat proposées sont des tableaux de résultats mensuels qui est réinitialisées le premier jour du mois.

Les développeurs sont limités à avoir pas plus de 20 statistiques proposées pour leur titre, mais il n’est pas obligatoire pour les développeurs à inclure les statistiques proposées dans leur titre.

## <a name="further-reading"></a>Ressources supplémentaires
Apprenez à configurer des statistiques avec [configuration proposées les statistiques et classements 2017](../configure-xbl/dev-center/featured-stats-and-leaderboards.md) apprendre à mettre à jour les valeurs statistiques avec [2017 de statistiques de la mise à jour](player-stats-updating.md)