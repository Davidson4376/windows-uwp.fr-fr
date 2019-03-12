---
title: "Guide du développeur Xbox\_Live"
description: "Découvrez comment utiliser les services Xbox\_Live pour connecter votre jeu au réseau de jeux Xbox\_Live."
ms.date: 08/22/2017
ms.topic: article
keywords: "windows\_10, uwp, jeux, xbox, xbox\_live"
ms.localizationpriority: medium
---
# <a name="what-is-xbox-live"></a>Qu’est-ce que Xbox Live ?

Xbox Live est un réseau de jeux Premier qui connecte des millions de joueurs dans le monde entier. Vous pouvez ajouter Xbox Live à votre jeu Windows 10 ou Xbox One afin de bénéficier des fonctionnalités et services Xbox Live.

Avec le programme Créateurs Xbox Live, toute personne disposant d’un compte [Espace partenaires](https://partner.microsoft.com/dashboard) peut générer un jeu pour la plateforme Windows universelle (UWP) compatible Xbox Live qui peut s’exécuter sur les PC Windows 10 et les consoles Xbox One.

Pour les développeurs de jeux qui souhaitent bénéficier de l’expérience Xbox Live complète, notamment le mode multijoueur, les succès et le développement de console Xbox natif, il existe des programmes de développement supplémentaires qui sont décrits en détail dans la [Vue d’ensemble du programme pour les développeurs](developer-program-overview.md).

Voici quelques raisons d’ajouter Xbox Live à votre jeu :

- Xbox Live regroupe des joueurs sur Xbox One et Windows 10, afin que les joueurs puissent jouer avec leurs amis et se connecter à une très grande communauté de joueurs.
- Xbox Live permet aux joueurs de générer un héritage de jeux en déverrouillant les succès, en partageant des clips de jeu épique, en amassant un score de joueur et en perfectionnant leur avatar.
- Xbox Live permet aux joueurs de jouer et de reprendre là où ils se sont interrompus sur une autre console Xbox One ou un autre PC, en récupérant tous leurs enregistrements à partir d’un autre appareil.
- Avec plus de 1 milliard de parties multijoueurs chaque mois, Xbox Live est conçu pour garantir les performances, la vitesse et la fiabilité.
- Avec le mode multijoueur inter-appareil, les joueurs peuvent jouer avec vos amis, qu’ils soient sur Xbox One ou sur un PC Windows 10.

> [!note]
> Ces rubriques sont destinées aux développeurs de jeux qui souhaitent ajouter une prise en charge de Xbox Live à leur jeu. Si vous cherchez des informations sur Xbox Live grand public, consultez [Xbox Live](https://www.xbox.com/live/).

## <a name="how-xbox-live-works"></a>Fonctionnement de Xbox Live

Sur le plan technique, Xbox Live est une collection de microservices qui exposent les fonctionnalités Xbox Live, comme le profil, les amis et la présence, les statistiques, les classements, les succès, le mode multijoueur et le matchmaking. Les données Xbox Live sont stockées dans le cloud. Vous pouvez y accéder en utilisant des points de terminaison REST et des websockets sécurisés qui sont accessibles à partir d’un ensemble d’API côté client conçues pour les développeurs de jeux.

En plus des API REST, il existe des API côté client qui wrappent les fonctionnalités REST. Pour plus d’informations, consultez [Présentation des API Xbox Live](introduction-to-xbox-live-apis.md).

### <a name="get-started-with-xbox-live"></a>Bien démarrer avec Xbox Live

Les guides suivants peuvent vous aider à commencer à développer avec Xbox Live, que vous soyez développeur pour console Xbox ou UWP.  Il existe également des guides pour vous préparer à des moteurs de jeu.

| Rubrique                                                                                                                                             | Description                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Vue d’ensemble du programme pour les développeurs](developer-program-overview.md) | Décrit les différents programmes pour les développeurs qui permettent le développement Xbox Live. |
| [Bien démarrer avec le programme Créateurs Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md) | Indique comment commencer avec Xbox Live dans le programme Créateurs Xbox Live. |
| [Bien démarrer avec Xbox Live en tant que développeur ID@Xbox ou géré](get-started-with-partner/get-started-with-xbox-live-partner.md) | Indique comment commencer avec Xbox Live en tant que développeur dans le programme ID@Xbox. |

### <a name="using-xbox-live"></a>Utilisation de Xbox Live

Une fois qu’un titre est créé et que les principes de base fonctionnent, cette section fournit les informations nécessaires avant de vous lancer et de commencer à coder.

| Rubrique                                                                                                                                             | Description                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Utilisation de Xbox Live](using-xbox-live/using-xbox-live.md) | Une fois que vous avez configuré votre titre et intégré le SDK Xbox Live, vous êtes prêt à vous connecter et à obtenir davantage d’informations sur la programmation pour Xbox Live.
| [Bonnes pratiques pour appeler Xbox Live](using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) | Familiarisez-vous avec les notions de base des modèles d’appel Xbox Live et les bonnes pratiques permettant de garantir que votre titre fonctionne correctement et qu’il n’est pas à débit limité.
| [Résolution des problèmes liés à l’API des services Xbox Live](using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md) | Problèmes courants que vous pouvez rencontrer et suggestions de résolution.

### <a name="xbox-live-social-platform"></a>Plateforme sociale Xbox Live

Les fonctionnalités sociales de Xbox Live peuvent faire croître votre public de façon drastique en vous faisant connaître auprès de plus de 55 millions d’utilisateurs actifs.  Cette section explique comment démarrer avec les fonctionnalités sociales de Xbox Live.

| Rubrique                                                                                                                                             | Description                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plateforme sociale Xbox Live](social-platform/social-platform.md) | Si vous pouvez connecter un utilisateur, vous pouvez commencer à utiliser des fonctionnalités sociales de Xbox Live, comme l’utilisation d’un graphe social, Rich Presence et plus encore. |

### <a name="xbox-live-data-platform"></a>Plateforme de données Xbox Live

La Plateforme de données Xbox Live définit l’utilisation des statistiques, des succès et des classements des utilisateurs.  Lisez cette série de rubriques pour en savoir plus sur la façon d’utiliser ces éléments dans votre titre.

| Rubrique                                                                                                                                             | Description                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plateforme de données Xbox Live](data-platform/data-platform.md) | Présentation sommaire de la Plateforme de données, ainsi que des instructions sur la façon de mieux intégrer les statistiques, les classements et les succès à votre titre.
| [Statistiques des joueurs](leaderboards-and-stats-2017/player-stats.md) | Les statistiques constituent le fondement des classements.  Découvrez comment les définir et les utiliser ici.
| [Classements](leaderboards-and-stats-2017/leaderboards.md) | Mettez en avant le côté compétitif de vos utilisateurs en incorporant intelligemment les classements.
| [Succès](achievements-2017/achievements.md) | Les succès sont une des fonctionnalités les plus connues de Xbox Live et un facteur essentiel de l’engagement des joueurs. Découvrez comment les utiliser dans votre titre.

### <a name="xbox-live-multiplayer-platform"></a>Plateforme multijoueur Xbox Live

Le mode multijoueur est un excellent moyen de prolonger la durée de vie de votre titre et de garder à jour les expériences de jeu.  Xbox Live fournit des fonctionnalités complètes de mode multijoueur et de matchmaking.  Vous disposez également de plusieurs options d’API qui fournissent différents niveaux de simplicité et de flexibilité.

| Rubrique                                                                                                                                             | Description                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plateforme multijoueur Xbox Live](multiplayer/multiplayer-intro.md) | Si vous débutez avec le développement du mode multijoueur Xbox Live ou que vous n’êtes pas familiarisé avec les nouvelles API, telles que le Gestionnaire de mode multijoueur et le mode multijoueur intégré Xbox (XIM), commencez ici. |
| [Scénarios multijoueurs](multiplayer/multiplayer-scenarios.md) | Suggestions et instructions sur la façon dont vous pouvez intégrer le mode multijoueur à votre titre. |
| [Mode multijoueur intégré Xbox](multiplayer/xbox-integrated-multiplayer.md) | Le mode multijoueur intégré Xbox (XIM) est une interface autonome simple permettant d’ajouter des réseaux multijoueurs en temps réel et une communication chat à votre titre. |
| [Gestionnaire de mode multijoueur](multiplayer/multiplayer-manager.md) | Le Gestionnaire de mode multijoueur fournit une API axée sur des scénarios multijoueurs courants. |

### <a name="xbox-live-storage-platform"></a>Plateforme de stockage Xbox Live

La Plateforme de stockage Xbox Live fournit les deux services Stockage de titres et Stockage connecté.  Il s’agit de deux services différents mais complémentaires.  Le Stockage connecté vous permet d’implémenter des enregistrements de jeux dans le cloud. Ils sont conservés d’un appareil à l’autre, quel que soit l’utilisateur connecté.  Le Stockage de titres vous permet de stocker des objets blob de données qui peuvent être par utilisateur ou par titre, et partagés entre différents utilisateurs.

| Rubrique                                                                                                                                             | Description                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plateforme de stockage Xbox Live](storage-platform/storage-platform.md) | Utilisez les services de stockage Xbox Live pour stocker les enregistrements de jeux, les relectures instantanées, les préférences utilisateur et d’autres données dans le cloud. |
| [Stockage connecté](storage-platform/connected-storage/connected-storage-technical-overview.md) | Vue d’ensemble et guide de programmation sur le Stockage connecté. |
| [Stockage de titres](storage-platform/xbox-live-title-storage/xbox-live-title-storage.md) | Vue d’ensemble et guide de programmation sur le Stockage de titres. |
