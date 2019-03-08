---
title: Plateforme de données actives Xbox
description: Vue d’ensemble de la Xbox Live plateforme de données, qui se compose de services pour gérer les primes, statistiques et classements.
ms.assetid: a8bb7c4f-09fe-4dba-b3ce-1fab60453831
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, statistiques, primes, tableaux de résultats, plateforme de données
ms.localizationpriority: medium
ms.openlocfilehash: f9a14c508e265cdfc510896eb621466d03c66776
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634834"
---
# <a name="xbox-live-data-platform---stats-leaderboards-achievements"></a>Xbox Live des tableaux de résultats de la plateforme - statistiques, de données, primes

Écriture de données de jeu à la plateforme de données Xbox Live permet à votre titre à exécuter en tant que service. En outre, la plateforme de données Xbox Live lecteurs l’intérêt des utilisateurs avec votre titre à l’aide de statistiques, les classements et les primes et surfaces proposés statistiques dans l’interpréteur de commandes de console et l’application de la Xbox.

Un exemple d’une statistique pour un jeu de course peut-être que les concurrences total exécuter en mode de bande de glisser, ce qui peut être utilisé pour créer un classement pour comparer votre résultat par rapport à la Communauté et peut être reflété dans votre chaîne de présence enrichie en temps réel (par exemple , « GoTeamEmily en cours de lecture en Mode glisser bande : 523 concurrences terminées »). Les concurrences total en mode de bande de glissement peut également servir comme critère pour Matchmaking, ce qui garantit des joueurs avec une expérience similaire sont plus susceptibles d’être mis en correspondance ensemble.

Votre titre peut afficher des classements basés sur des statistiques. Par exemple, le classement peut être un classement global de concurrences terminée. Vous appelez ces services à l’aide de l’API Live Xbox directement, ou wrappers dans un moteur de jeu comme Unity.

Vous pouvez désigner certaines statistiques statistiques comme proposées. Statistiques proposées sont affichées dans GameHub de votre titre et comparez les joueurs par rapport à leurs amis.

Primes ne sont pas alimentées par des statistiques, et votre titre décide quand une prime est déverrouillée. Par exemple, votre titre peut avoir une prime pour l’exécution d’une concurrence dans moins d’une minute. Votre titre des conserve les paramètres nécessaires pour déverrouiller la réalisation. Dans cet exemple, il serait jusqu'à votre titre pour mesurer la durée pendant laquelle la concurrence a duré et attribuons ensuite la prime. En règle générale, Gamescore est attribué, ainsi que l’achèvement de primes. Il vous incombe à décidé de la quantité de score de joueur pour chaque prime.

## <a name="features"></a>Fonctionnalités ##
Les rubriques suivantes fournissent plus d’informations sur les fonctionnalités de la plateforme de données Xbox Live :

* [Statistiques de lecteur](../leaderboards-and-stats-2017/player-stats.md)
* [Primes](../achievements-2017/achievements.md)
* [Classements](../leaderboards-and-stats-2017/leaderboards.md)
