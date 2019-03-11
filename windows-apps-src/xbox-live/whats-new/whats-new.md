---
title: Nouveautés de la Xbox Live
description: Nouveautés de la Xbox Live SDK
ms.date: 10/23/2018
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33e5b6afbf0d60679bfce1789be2d965fd881f1c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614114"
---
# <a name="whats-new-for-xbox-live"></a>Nouveautés pour Xbox Live
Vous pouvez également vérifier le [historique des validations de Xbox Live API GitHub](https://github.com/Microsoft/xbox-live-api/commits/master) pour afficher toutes les modifications de code récents aux API Xbox Live.

## <a name="in-this-article"></a>Dans cet article

* [Juin 2018](#june-2018)
* [Août 2017](#august-2017)
* [Juillet 2017](#july-2017)
* [Juin 2017](#june-2017)
* [Mai 2017](#may-2017)
* [Avril 2017](#april-2017)
* [Mars 2017](#march-2017)
* [Archivé](#archived)

## <a name="june-2018"></a>Juin 2018

### <a name="xbox-live-features"></a>Fonctionnalités de Xbox Live

#### <a name="c-api-layer-for-xsapi"></a>Couche API C pour XSAPI

Les API C sont désormais disponibles pour certaines fonctionnalités Xbox Live. La nouvelle couche d’API fournit un certain nombre d’avantages pour les fonctionnalités prises en charge, y compris la gestion personnalisée de la mémoire, la gestion de thread manuel pour les tâches asynchrones et une nouvelle bibliothèque HTTP.

Pour plus d’informations, consultez [Xbox Live C API](../xsapi-flat-c.md).

## <a name="august-2017"></a>Août 2017

### <a name="xbox-live-features"></a>Fonctionnalités Xbox Live

#### <a name="in-game-clubs"></a>Dans le jeu clubs

Les développeurs peuvent créer maintenant « dans le jeu clubs ». Dans le jeu clubs diffèrent des clubs de Xbox standard car ils sont entièrement personnalisables par un développeur et peuvent être utilisées à la fois à l’intérieur et à l’extérieur du jeu. En tant que développeur de jeux, vous pouvez les utiliser pour créer rapidement n’importe quel type de scénarios de groupe persistant à l’intérieur de vos jeux tels que les équipes, clans, assurée, métiers, etc. qui correspondent à vos besoins spécifiques.

Xbox live membres peuvent accéder des clubs dans le jeu en dehors de votre jeu sur n’importe quel expérience Xbox pour rester connectés entre eux et à votre jeu à l’aide des fonctionnalités de club telles que la conversation, flux, LFG et Mixer librement sur la console Xbox, PC ou appareils iOS/Android.

API sont disponibles pour créer et gérer des clubs de dans le jeu directement à partir de votre jeu. Ces API existe dans l’espace de noms xbox::services::clubs.

## <a name="july-2017"></a>Juillet 2017

### <a name="xbox-live-features"></a>Fonctionnalités Xbox Live

#### <a name="tournaments"></a>Tournois

Nouvelles API ont été ajoutées pour prendre en charge de tournois. Vous pouvez maintenant utiliser la classe xbox::services::tournaments::tournament_service pour accéder au service de tournois de votre titre.
Ces nouvelles tournoi API activer les scénarios suivants :

* Interroger le service pour rechercher tous les tournois existants pour le titre en cours.
* Extraire des informations sur un tournoi à partir du service.
* Interroger le service pour récupérer la liste des équipes pour un tournoi.
* Extraire des informations sur les équipes pour un tournoi à partir du service.
* Suivre les modifications apportées à tournois et les équipes en utilisant des abonnements de l’activité en temps réel (RTA).

## <a name="june-2017"></a>Juin 2017

### <a name="xbox-live-features"></a>Fonctionnalités Xbox Live

#### <a name="game-chat-2"></a>Game Chat 2

Une version mise à jour et améliorée de jeu Chat est désormais disponible. Pour plus d’informations, consultez le [vue d’ensemble du jeu Chat 2](../multiplayer/chat/game-chat-2-overview.md).

### <a name="xbox-live-tools"></a>Outils de Xbox Live

#### <a name="xbox-live-powershell-module"></a>Xbox Live Module PowerShell

* Modules PowerShell ont été ajoutées pour faciliter le basculement de bacs à sable sur votre ordinateur de développement. Pour plus d’informations, consultez [outils](../tools/tools.md)

#### <a name="bug-fixes"></a>Correctifs de bogues

* Divers correctifs de bogues. Vérifier le [historique des validations de GitHub](https://github.com/Microsoft/xbox-live-api/commits/master) pour obtenir une liste complète.

## <a name="may-2017"></a>Mai 2017

### <a name="xbox-services-apis"></a>API de Services Xbox

#### <a name="multiplayer"></a>Mode multijoueur

* Interrogation maintenant les descripteurs de recherche inclut les propriétés de session personnalisée dans la réponse.

#### <a name="bug-fixes"></a>Correctifs de bogues

* Correction de « json incorrect » qui est retourné au lieu d’un code d’erreur HTTP valid.

## <a name="april-2017"></a>Avril 2017

### <a name="xbox-services-apis"></a>API de Services Xbox

#### <a name="visual-studio-2017"></a>Visual Studio 2017

API Xbox Live ont été mis à jour pour prendre en charge Visual Studio 2017, pour les titres de plateforme universelle Windows (UWP) et Xbox One.

#### <a name="tournaments"></a>Tournois

Nouvelles API ont été ajoutées pour prendre en charge de tournois. Vous pouvez maintenant utiliser le `xbox::services::tournaments::tournament_service` classe pour accéder au service de tournois de votre titre.

Ces nouvelles tournoi API activer les scénarios suivants :

* Interroger le service pour rechercher tous les tournois existants pour le titre en cours.
* Extraire des informations sur un tournoi à partir du service.
* Interroger le service pour récupérer la liste des équipes pour un tournoi.
* Extraire des informations sur les équipes pour un tournoi à partir du service.
* Suivre les modifications apportées à tournois et les équipes en utilisant des abonnements de l’activité en temps réel (RTA).

## <a name="march-2017"></a>Mars 2017

### <a name="xbox-services-api"></a>API des Services Xbox

#### <a name="data-platform-2017"></a>Plateforme de données 2017

Nous avons introduit une API simplifiée de statistiques.  En règle générale, vous deviez envoyer des événements correspondant aux règles statistiques définies sur XDP ou partenaires, ces seraient mise à jour les valeurs statistiques dans le cloud.  Nous faisons référence à ce modèle en tant que statistiques 2013.

Statistiques 2017, votre titre est maintenant dans le contrôle des valeurs des jours fériés.  Vous appelez simplement une API avec la dernière valeur stat, et qui est envoyé au service directement, sans la nécessité pour les événements.  Cet exemple utilise la nouvelle `StatsManager` API et vous trouverez plus d’informations dans [statistiques](../leaderboards-and-stats-2017/player-stats.md)

#### <a name="github"></a>GitHub

Xbox Live API (XSAPI) est désormais disponible sur GitHub à l’adresse [ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api).  Les fichiers binaires qui sont fournis avec le XDK ou en tant que NuGet packages est toujours recommandé d’utiliser, toutefois vous avez la possibilité d’utiliser la source et nous apprécions les contributions de code source.  

### <a name="xbox-live-creators-program"></a>Programme Créateurs Xbox Live

Le programme Xbox Live Creators est un programme de développeurs offre un sous-ensemble des fonctionnalités Xbox Live à un public plus large de développeur.  Si vous êtes dans le ID@Xbox programme, cela n’aura pas aucun impact sur vous.

Vous trouverez plus d’informations sur le programme dans [vue d’ensemble du programme de développeurs](../developer-program-overview.md).

### <a name="documentation"></a>Documentation

Il existe les nouveaux articles suivants :

| Article | Description |
|---------|-------------|
|[Configuration du Service Live Xbox](../xbox-live-service-configuration.md) | Mise à jour d’informations sur le processus de configuration de service pour le titre de la Xbox Live
| [Configurer Xbox Live dans Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) | Nouvelles informations sur le programme d’installation Unity pour les développeurs de programme Xbox Live Creators |
| [Les bacs à sable Xbox Live](../xbox-live-sandboxes.md) | Un guide simplifié à l’isolation de contenu et des bacs à sable Xbox Live |
| [Comptes de tests en temps réel de Xbox](../xbox-live-test-accounts.md) | Informations sur la façon de test travail de comptes et comment les créer sur les partenaires |

## <a name="archived"></a>Archivé

* [Décembre 2016](1612-whats-new.md)
* [Novembre 2016](1611-whats-new.md)
* [Août 2016](1608-whats-new.md)
* [Juin 2016](1606-whats-new.md)
* [Avril 2016](1603-whats-new.md)
* [Mars 2016](1603-whats-new.md)
* [Février 2016](1602-whats-new.md)
* [Octobre 2015](1510-whats-new.md)
* [Septembre 2015](1509-whats-new.md)
* [Août 2015](1508-whats-new.md)
* [Juin 2015](1506-whats-new.md)
