---
title: Conception des expériences Xbox Live
description: Apprenez à concevoir des expériences exceptionnelles pour les membres de Xbox Live en planifiant des statistiques, les classements et les primes pour votre titre.
ms.assetid: d2a85621-7d02-4b11-a7fa-9ebaee0092d5
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, statistiques, primes, tableaux de résultats, conception
ms.localizationpriority: medium
ms.openlocfilehash: 0f080593727ec6d7ddd529b2ce976708174250ba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600784"
---
# <a name="designing-xbox-live-experiences"></a>Conception des expériences Xbox Live

Cette rubrique vous guide dans un exemple de processus de penser et en ajoutant des classements, les primes et les statistiques de l’utilisateur à votre jeu en utilisant les fonctionnalités de plateforme de données de Xbox Live.

## <a name="example"></a>Exemple
Nous allons configurer un jeu fictif qui vous montre comment la plate-forme de données peut vous aider à environ incroyables expériences Xbox Live dans le tableau de bord Xbox One, l’application Xbox sur Windows 10 et dans votre jeu. Pour ce scénario, nous allons utiliser imaginaire racing jeu appelé _concurrences 2015_.

Pour obtenir le meilleur parti de ces expériences, nous allons suivre ces phases de planification recommandée :
1. Concevez vos expériences XBL
2. Les statistiques seront nécessaires pour dynamiser vos scénarios de conception
3. Configurer les statistiques proposées, primes ou tableaux de résultats en fonction des besoins


## <a name="1-design-your-xbox-live-experiences"></a>1. Concevez vos expériences Xbox Live
Nous voulons _concurrence 2015_ à tirer le meilleur parti de Xbox Live afin de conserver les utilisateurs engagés à l’intérieur et en dehors du jeu. Les première posez-vous les questions :

1. Que faire nous voulons primes onglet expérience de notre GameHub comme ? (Statistiques proposées & Leaderboards Social)
2. Les objectifs et les motivations nous voulons donner aux joueurs à faire dans le jeu ? (Succès)
3. Les scores ou les statistiques nous voulons lecteurs à utiliser pour classer par rang eux-mêmes par rapport à d’autres joueurs ? (Leaderboards)


### <a name="gamehubs---featured-statistics-and-social-leaderboards"></a>GameHubs - statistiques proposées et les classements de réseaux sociaux
Expériences de GameHubs sont destination où les utilisateurs puissent connaître toutes les informations sur un jeu. Comme un développement et/ou d’un serveur de publication, voici le lieu idéal où vous pouvez _showcase_ formidable et plus riche votre jeu est aux utilisateurs de Xbox ayant pas encore acheté le jeu. GameHubs sont également conçues pour indique la progression et les métriques attrayantes aux joueurs qui possèdent déjà le titre. Sous l’onglet des primes, les utilisateurs peuvent trouver progression de jeu et primes, héros statistiques et classements sociaux.

De cette section vise à capturer les métriques plus attrayantes pour le jeu et de les exposer une image unique de l’expérience des joueurs avec le jeu et classez-les par rapport à leurs amis dans un classement de réseaux sociaux. Par exemple, l’onglet de la réalisation de 6 Forza pourrait ressembler à ceci :

![Image de Gamehub](../images/omega/forza_gamehub.png)


Vous remarquerez que certaines statistiques tel que fin de la Place de premier et Miles piloté par les ornements or, argent ou bronze qui illustrent où le joueur évalue par rapport à ses amis. Interaction avec une de ces statistiques affiche un classement complète comme suit :

![Classement](../images/omega/progress_gamehub_lb.png)

 Pour _concurrence 2015_, nous pensons que ce qui suit est un bon ensemble de statistiques qui représentent l’expérience du joueur avec le titre, ainsi que bon classement métriques :
 * Présentation rapide
 * Plus la 1re place wins
 * Miles piloté par


### <a name="achievements"></a>Succès
Primes sont un mécanisme de l’échelle du système de dirigeant et en récompensant les actions des utilisateurs dans le jeu de manière cohérente sur tous les jeux. Cette conception correctement permet aux utilisateurs de guide pour découvrir le jeu à son plein et étend la durée de vie du jeu.

Pour _concurrences 2015_, ici est un sous-ensemble des primes que nous souhaitons configurer :
* Terminer la 1 présentation en moins de 60 secondes
* Terminer les 10 concurrences dans la 1ère place
* Effectuez au moins 1 concurrence dans chacune les pistes
* Propriétaires de 5 voitures
* Lecteur pour les 10 000 miles
* ...


###  <a name="leaderboards"></a>Classements
Classements permettent aux joueurs d’évaluer par rapport à d’autres joueurs pour leur permettre de réaliser dans le jeu des actions spécifiques. Outre les classements social dans le GameHubs, nous pouvons configurer les tableaux globaux de scores à utiliser dans le jeu. Il existe quelques les classements nous devrons attraper tous nos utilisateurs classent sur :

* Temps de présentation plus rapide
 * Métadonnées : suivre où cela s’est produit
 * Métadonnées : voiture utilisée
 * Métadonnées : conditions météorologiques
* Plus la 1re place wins
* La plupart des voitures collectées

## <a name="next-steps"></a>Étapes suivantes
Maintenant que vous savez comment concevoir efficacement les statistiques, les classements et les primes, vous pouvez maintenant commencer à commencer à implémenter dans votre titre.  Les sections suivantes décrit le processus de bout en bout à compter de configuration dans l’espace partenaires.

Consultez [statistiques](../leaderboards-and-stats-2017/player-stats.md)
