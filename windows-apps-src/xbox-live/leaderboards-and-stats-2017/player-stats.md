---
title: Statistiques joueur
description: Découvrez comment configurer des statistiques sur le Xbox Live.
ms.assetid: 5ec7cec6-4296-483d-960d-2f025af6896e
ms.date: 07/30/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, statistiques, classements
ms.localizationpriority: medium
ms.openlocfilehash: e8b88f3db93f98789ada3c3b3dfe74195925657e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597304"
---
# <a name="player-stats"></a>Statistiques de lecteur

Comme décrit dans [vue d’ensemble de la plate-forme de données](../data-platform/data-platform.md), statistiques sont des informations que vous souhaitez effectuer le suivi sur un lecteur essentielles. Par exemple *Head captures* ou *temps le plus rapide de présentation*. Statistiques sont utilisés pour générer des tableaux de résultats dans un nombre de scénarios qui permettra de joueurs à comparer leurs efforts et les compétences grâce à leurs amis et chaque autre joueur dans la Communauté d’un titre. Configuré des statistiques s’affichent dans un titre [jeu Hub](../data-platform/designing-xbox-live-experiences.md) classement où un lecteur verront leur rang par rapport à leurs amis ayant également jouer le titre. Les statistiques qui s’affichent dans jeu Hub d’un titre sont appelés **fonctionnalité statistiques**, également appelé **héros statistiques**. Outre affichent dans le Hub de jeu, à compter de la mise à jour Xbox One 1806 statistiques proposées peut également régulièrement apparaissent dans des blocs de contenu épinglés que les utilisateurs peuvent ajouter à leur vue d’accueil. Cela autorise l’accès rapide au leaderboards social pour directement à partir de la vue d’accueil. Il est important de se rappeler que statistiques proposées sont parmi plusieurs éléments possibles affichées dans des blocs de contenu épinglé, les utilisateurs peuvent verrez donc pas toujours les si les services de contenu de Microsoft à déterminer qu'un autre élément de contenu peut être plus intéressant.

## <a name="stats-2013-and-2017"></a>Statistiques 2013 et 2017

Il existe actuellement deux implémentations pour les statistiques de la Xbox Live, les statistiques 2013 et les statistiques 2017. Les deux sont disponibles pour ID@Xbox et les développeurs de partenaires gérés. Les développeurs de programme Xbox Live Creators peuvent utiliser uniquement les statistiques 2017 et peuvent donc ignorer les statistiques 2013.

Les développeurs de programme Xbox Live Creators peuvent passer directement à la [statistiques 2017 document](stats2017.md).

Ces deux implémentations fonctionnent sur les principes fondamentalement différentes. Lors de l’utilisation de statistiques 2013 vous envoyer **événements** pour le Service Xbox Live, contenant certaines informations sur une action exécutée par un utilisateur. Les informations contenues dans ces **événements** est utilisé pour mettre à jour les statistiques en conséquence. Dans les statistiques 2013 le service suivre et mettre à jour tous vos valeurs statistiques, rendant la source de vérité pour des valeurs statistiques pour un lecteur ou un groupe de lecteurs. Par opposition, dans les statistiques 2017 vous enverra la valeur réelle de stat lui-même pour le serveur à utiliser. Dans 2017 statistiques que le serveur ne peu ou aucune validation sur la valeur qui lui sont envoyées, et par conséquent, c’est à votre titre pour effectuer le suivi de valeurs statistiques correctes et être la source de vérité pour des valeurs statistiques. Nous vous recommandons de suivez et stockez vos statistiques dans le cloud avec le [Xbox Live une plateforme de stockage](../storage-platform/storage-platform.md) si vous décidez d’implémenter des statistiques 2017. Vous pouvez considérer de statistiques 2017 comme un service de rapport. Vous envoyez la statistique correcte pour votre jeu sur le serveur, vous stat serez ensuite se trouvent sur le serveur et attendre peuvent être affichées sur demande ou mis à jour.

## <a name="a-brief-history"></a>Bref historique

Avec le lancement de Xbox One, Xbox Live a introduit un nouveau modèle événementiel de statistiques, le lecteur statistiques 2013. Cela a débouché une multitude d’avantages : un seul événement à partir d’un jeu peut mettre à jour les données de plusieurs fonctionnalités Xbox Live, tels que des tableaux de résultats et des résultats ; Configuration de Xbox Live se trouve sur le serveur et non dans le client ; et bien plus encore. Dans les années suivant le lancement de Xbox One, nous vous avons écouté étroitement aux commentaires des développeurs de jeux et les développeurs régulièrement demandé un service de statistiques plus simple qui serait leur permettre de contourner la complexité est fourni avec un événement piloté par le système, ainsi que pour autoriser ils utilisent des statistiques qu’ils ont été pratiquant déjà des méthodes de suivi. Basé sur les commentaires des développeurs qu'une nouvelle version simplifiée de statistiques a été créée qui seraient remis le contrôle de la logique de statistiques dans les mains du développeur. Que le système est Stats 2017, un service qui prend simplement la valeur passé par le titre en donnant aux développeurs de contrôle de la logique de détermination d’une valeur stat.

## <a name="how-stats-are-handled-in-2013-and-2017"></a>Gestion des statistiques en 2013 et 2017

Jetons un œil à la configuration et la mise à jour des statistiques pour 2013 et 2017. Imaginons que nous ferez des statistiques de certains rpg générique et que vous souhaitez effectuer le suivi des monstres tués.

Dans statistiques 2013 votre titre enverrait un *événement* qui contient des informations sur une action exécutée par un lecteur. Dans ce cas l’action sera opération un orc tandis que le joueur avait une à double tranchant équipée. Certaines des informations contenues dans cet événement peuvent être que slay d’action, la chose slayed était un orc, le type de combat était mêlée, et l’arme utilisé a été un mot de passe. Statistiques 2013 s’exécute ces informations à travers un nombre de règles configurées par vous, le développeur, sur le [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts) ou [partenaires](https://partner.microsoft.com/dashboard), et selon les statistiques de mise à jour, également configurées par vous, l’événement. Dans les statistiques 2013 le service sera suivi de ce que la valeur de vos statistiques slaying doit être. L’orc slay avec les événements à double tranchant pu mettre à jour plusieurs statistiques telles que, nombre de victimes, nombre d’orcs slain et nombre de victimes de mot de passe.

Dans les statistiques 2017, vous enverrez les valeurs réelles de vos statistiques. Pour l’orc slay avec exemple de mot de passe, le titre de la conserver une trace du nombre de tue globale et à double tranchant tue orcs slain individuellement et envoyer le service le nombre de mises à jour pour chaque statistique. Le service a des contrôles de validation minimal pour vous assurer que vous envoyez un nombre qui a du sens, donc c’est absolument à votre titre afin d’envoyer la statistique correcte. Bien que vous pouvez utiliser le service de statistiques 2017 de rappeler les valeurs des statistiques au début d’une session de jeu, vous ne devez pas utiliser le service Stats 2017 pour confirmer la valeur d’un stat tandis que la session est en cours.

Vous trouverez ci-dessous une illustration du fonctionnement des deux versions de statistiques.
![Illustration de différence de statistiques](../images/stats/Stats2013-7DiagramColored.jpg)

## <a name="a-few-more-notes"></a>Quelques remarques plus

Pour ID@Xbox et aux développeurs de partenaire géré il existe quelques différences de plus vous devez être conscient lors du choix entre statistiques 2013 et statistiques 2017 pour votre titre.

Statistiques 2013 permet plusieurs vues de classement.
Statistiques 2013 permet que plus facilement Explorer les métadonnées de vos statistiques. Dans notre orc peu exemple la statistique globale a été suivi de victimes. Statistiques 2013 permet de détailler vos statistiques à ce qui a été arrêtée et avec quel armes, ainsi que n’importe quel autre kill définition des informations que vous pouvez configurer.

Statistiques 2013 a un stat native de minutes de jeu. Statistiques 2013 vous permet de qu'accéder à des temps de lecture du lecteur dans le jeu sans configurer la statistique. Des statistiques de lecture devra être suivies par votre titre dans 2017 de statistiques.

Statistiques 2013 a une fréquence de mise à jour quasiment en temps réel.
Le système d’événements statistiques 2013 met à jour vos statistiques presque instantanément. Dans les statistiques 2017, il existe un délai d’attente de cinq minutes avant la mise à jour des statistiques sont vidés sur le service. Le développeur peut vider manuellement les statistiques, mais il sera toujours limité à un minimum de 30 secondes entre les vidages.

Statistiques 2013 peut prendre en charge d’autres Services de Xbox Live.
Avec Stats 2013, vous pouvez utiliser des statistiques pour déverrouiller des primes et prendre des décisions de matchmaking. 2017 de statistiques est uniquement utilisé pour produire des tableaux de résultats.

## <a name="further-reading"></a>Informations supplémentaires

Pour obtenir une explication plus approfondie de statistiques 2013, vous pouvez lire le [pour partenaires uniquement sur les statistiques 2013](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/user-stats).
Pour obtenir une explication plus détaillée de statistiques 2017, lisez le [statistiques 2017 Documentation](stats2017.md).