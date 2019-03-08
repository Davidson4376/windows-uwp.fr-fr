---
title: Service de l’activité en temps réel
description: En savoir plus sur le service Xbox Live en temps réel activité.
ms.assetid: 50de262f-fc55-4301-83b5-0a8a30bc7852
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, service de l’activité en temps réel.
ms.localizationpriority: medium
ms.openlocfilehash: 36389fac3bd6dea2d2e24c0935087781118d8046
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592084"
---
# <a name="real-time-activity-service"></a>Service de l’activité en temps réel

Le service de l’activité en temps réel (RTA) permet à une application sur n’importe quel appareil pour vous abonner aux données d’état, de statistiques de l’utilisateur et de présence. Le système autorise les abonnements aux données personnels et à d’autres données dans n’importe quel titre en fonction de leurs paramètres de confidentialité. Ainsi, un flux d’informations sans devoir interroger en permanence pour obtenir les dernières données.


## <a name="developer-scenarios"></a>Scénarios de développement

Il existe de nombreux scénarios qui prend en charge de l’agrégation RTA. Quelques-unes d'entre elles sont répertoriées ici, mais la véritable puissance de la valeur RTA est les nombreux scénarios qui vous affichera que nous n’avons pas encore imaginé. Vous pouvez permettent de définir la nouvelle génération de jeu où les utilisateurs ont souvent à portée de main leur Microsoft Surface ou Apple iPad tandis qu’elles interagissent avec le titre de la console. RTA utilise la technologie WebSocket pour les différentes procédures sous-rubrique comporte une vue d’ensemble de l’implémentation à l’aide de l’API de WebSockets fournie par Windows.

Voici certains scénarios simples que vous pouvez créer pour votre titre en utilisant la valeur RTA :

-   Application de progression de primes
-   Application de l’aide de jeux
-   Application de visionneuse escouade
-   Visionneuse de statistiques
-   Visionneuse de présence


## <a name="achievements-progress-app"></a>Application de progression de primes

Les utilisateurs sont presque toujours curieux de connaître leur progression certaines primes, en particulier les primes qui nécessitent d’effectuer une action d’un certain nombre de fois. Avec un accès en temps réel pour les statistiques d’un utilisateur (qui sont agrégées dans le service Xbox Live Player statistiques), vous pouvez présenter la progression en temps réel pour les joueurs et leurs amis vers primes et étapes majeures, sur Xbox One ou sur un appareil mobile, alors que l’utilisateur la lecture de votre titre.


## <a name="game-help-app"></a>Application de l’aide de jeux

Comme les utilisateurs naviguent dans votre titre, un accès aux données en temps réel vous permet de présenter jeu aide en regard de l’expérience sur Xbox One ou sur n’importe quel appareil mobile. Les utilisateurs s’affiche pour un nouveau mappage, nouvelle formation ou difficile lutte patron et votre Compagnon jeu aide peut afficher vidéos générées par l’utilisateur ou générés par le développeur et texte pour aider l’utilisateur via l’expérience dans le titre de votre.


## <a name="squad-viewer-app"></a>Application de visionneuse escouade

Dans les jeux multijoueurs co-op, un lecteur et ses collègues fonctionnent ensemble pour un objectif commun. Avec ce nombre de joueurs, il peut être difficile de suivre une vue plus large. Accès aux données en temps réel, vous pouvez créer une application Compagnon qui montre les mappages de mappage et de la carte thermique haut niveau d’où l’action peut être.


## <a name="statistics-viewer"></a>Visionneuse de statistiques

S’il est courant d’applications Compagnon considérer lorsque vous réfléchissez à la valeur RTA, vous pouvez également utiliser RTA dans votre titre core. Par exemple, vous pouvez utiliser RTA pour fournir un lecteur d’un jeu multijoueur un affichage de tout le monde statistiques actuelles dans le jeu, peut-être par le lecteur en appuyant simplement sur le bouton de la vue sur le contrôleur dans la correspondance multijoueur.


## <a name="presence-viewer"></a>Visionneuse de présence

Lorsque, dans une salle d’attente, il est utile d’avoir une vue à jour de qui est en ligne et si elles jouent le même titre. Vous pouvez vous abonner à la présence d’amis d’un utilisateur et afficher les amis sont en ligne, si elles commencent à lire votre titre, tout en temps réel.


## <a name="subscription-privacy-and-authorization"></a>Autorisation et confidentialité de l’abonnement

La dernière version de l’agrégation RTA inclut des vérifications pour la confidentialité et l’isolement de contenu / d’autorisation. Tant que contrôles de confidentialité et d’autorisation sont satisfaites, votre application peut s’abonner à des statistiques sont marquée comme RTA prenant en charge. (Pour plus d’informations sur la façon de rendre un, consultez RTA compatibles, statistiques [s’inscrire aux notifications statistiques](register-for-stat-notifications.md). Il existe aucune limitation sur les types de statistiques qui peuvent être rendus prenant en charge l’agrégation RTA, c’est à vous, le développeur. Toutefois, il existe une limite à la *nombre* de statistiques utilisateur peut s’abonner à par session de l’application. Si un utilisateur atteint cette limite, il ou elle reçoit une erreur sur l’abonnement suivant.


## <a name="in-this-section"></a>Dans cette section

[S’inscrire aux notifications de modification stat player](register-for-stat-notifications.md)  
Décrit comment activer l’activité en temps réel (RTA) pour plus d’informations d’état ou de statistiques.

[Le service de l’activité en temps réel (rta) à l’aide des API winrt de programmation](programming-the-real-time-activity-service.md)  
Décrit comment programmer le service de l’activité en temps réel (RTA) à l’aide de WinRT APIs.

[Le service de l’activité en temps réel (rta) à l’aide de l’interface restful de programmation](programming-the-real-time-activity-service.md)  
Décrit comment programmer le service de l’activité en temps réel (RTA) à l’aide de l’interface RESTful.

[Meilleures pratiques de l’activité en temps réel (rta)](rta-best-practices.md)  
Meilleures pratiques pour utiliser le service d’activité en temps réel (RTA) de services Xbox pour s’abonner à des données statistiques et l’état de la plateforme de données Xbox.
