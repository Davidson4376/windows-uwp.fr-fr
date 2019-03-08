---
title: Configuration du service Xbox Live
description: Découvrez comment configurer le service Xbox Live à votre jeu.
ms.assetid: 631c415b-5366-4a84-ba4f-4f513b229c32
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, une, la configuration de service
ms.localizationpriority: medium
ms.openlocfilehash: d12c66e61a189c13ddbcd96dd99caa351206ecf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640904"
---
# <a name="xbox-live-service-configuration"></a>Configuration du service Xbox Live

## <a name="what-is-service-configuration"></a>Qu’est la Configuration de Service ?

Vous connaissez peut-être certaines des fonctionnalités Xbox Live comme [primes](achievements-2017/achievements.md), [Leaderboards](leaderboards-and-stats-2017/leaderboards.md) et [Matchmaking](multiplayer/multiplayer-concepts.md#smartmatch-matchmaking).

Dans le cas où vous n’êtes pas le cas, nous expliquerons brièvement Leaderboards comme exemple. Classements permettent des joueurs afficher une valeur représentant une réalisation, par rapport à d’autres joueurs.  Par exemple les meilleurs scores dans un jeu d’arcade, des heures de présentation dans un jeu de course ou headshots dans un tir. Mais, contrairement à une machine arcade qui affiche uniquement les scores les plus élevés des lecteurs ayant jouer sur l’ordinateur physique, à Xbox Live il est possible d’afficher les meilleurs scores à partir du monde.

Mais pour ce faire, vous devez effectuer une configuration à usage unique afin que Xbox Live connaît votre classement. Par exemple si les valeurs doivent être triées dans l’ordre croissant ou décroissant de valeur, et quelle partie des données qu’il doit être de tri.

Cette configuration se produit sur [partenaires](https://partner.microsoft.com/dashboard) la plupart du temps. Mais certains développeurs utiliseront [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com).

Si vous êtes un titre de votre développement en tant que partie du programme Xbox Live Creators, vous utilisez [partenaires](https://partner.microsoft.com/dashboard), et vous pouvez lire [mise en route avec Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md) pour apprendre à configurer.

Si vous êtes un ID@Xbox développeur ou pour travailler avec un serveur de publication est un Microsoft Partner, veuillez lire sur.

## <a name="choose-your-development-portal"></a>Choisissez votre portail de développement

Comme mentionné ci-dessus, il existe deux portails différents qui peuvent être utilisées pour configurer les Services Xbox Live. Centre de partenaires à l’adresse [ https://partner.microsoft.com/dashboard ](https://partner.microsoft.com/dashboard) et le portail de développement Xbox (XDP) à l’adresse [ https://xdp.xboxlive.com ](https://xdp.xboxlive.com).

Partenaires est recommandée pour tous les titres à l’avenir, mais pour certaines fonctionnalités, vous pouvez souhaitez toujours utiliser XDP. Cette rubrique vous conseiller où configurer votre titre.

Vous trouverez des informations sur les pages de configuration de service spécifique en fonction de votre portail choisi :

* [Configuration de partenaires](configure-xbl/windows-dev-center.md)
* [Configuration du portail de développement Xbox](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-service-configuration) - pour accéder à ce lien, vous devez disposer d’un compte Microsoft (MSA) qui a été activé pour un accès complet Xbox Live.

Si vous avez déjà un titre configuré, vous pouvez faire défiler vers le bas pour [obtenir votre ID](#get_ids) pour savoir comment obtenir les différents identificateurs nécessaires à la configuration de votre titre.

### <a name="pcmobile-uwp-game-only"></a>Jeu de PC/Mobile UWP uniquement
Partenaires est recommandé de configurer et gérer des jeux UWP qui s’exécutent uniquement sur les PC Windows 10 et/ou des appareils mobiles Windows 10.

#### <a name="using-xdp-to-configure-uwp-titles"></a>À l’aide de XDP pour configurer les titres UWP

Voulez-vous utiliser XDP pour configurer les titres UWP si vous avez des exigences suivantes :

1. Vous utilisez Arena.
2. Vous disposez des utilisateurs existants, les groupes et les paramètres des autorisations sur XDP que vous souhaitez continuer à utiliser.
3. Vous utilisez des outils qui fonctionnent uniquement sur XDP telles que l’outil de tournois ou de la visionneuse de l’historique de session annuaire de sessions multijoueurs.
4. Vous développez un titre qui auront play multiplateformes entre un jeu de Xbox XDK un en fonction et une version UWP PC/mobile du même jeu.

Si vous n’appartiennent à une de ces catégories, vous devez utiliser les partenaires. Sinon, vous pouvez voir ci-dessous pour savoir comment utiliser XDP pour configurer un titre UWP.

Pour configurer les Services Xbox Live pour les applications UWP XDP contient quelques avertissements importants :

* **Une fois la configuration du service d’un jeu Xbox Live est publiée sur CERT/vente au détail XDP, il n’existe aucune revenir !** La configuration du service Xbox Live pour qui doit rester dans XDP pendant la durée de vie du titre du jeu de jeux.
* **Il n’existe aucun chemin de migration à partir de XDP pour partenaires.** Si vous démarrez votre configuration Xbox Live dans XDP, vous devez le recréer manuellement dans partenaires si vous souhaitez déplacer.

Étant donné ces considérations relatives deux à, il est recommandé à l’aide de partenaires pour les jeux PC/Mobile, à moins que vous se répartissent dans les catégories décrites ci-dessus.

### <a name="cross-play-between-xbox-one-and-pcmobile-games"></a>Cross-play entre des jeux Xbox One et PC/Mobile ###
Jeux inter-périphériques entre Xbox One et le PC, appelé cross-play, sont une expérience de présentation Windows 10. Dans ce scénario, les versions de la Xbox One et PC d’un jeu partagent la même configuration de Xbox Live.

Ce scénario couvre également le cas où vous avez un jeu existant de XDK une Xbox et à ajouter la prise en charge de cross-play pour une version UWP de votre jeu.

Pour mettre en œuvre cross-play, procédez comme suit :

* **Utilisez XDP pour configurer et publier votre jeu XDK.** Partenaires ne prend pas en charge les jeux de XDK une Xbox pour l’instant.
* **Utilisez une configuration unique service Xbox Live que vous avez créé dans XDP pour le XDK et les versions UWP d’un jeu.** XDP dispose désormais de nouvelles fonctionnalités qui permettent un jeu de partager une seule configuration service Xbox Live entre la version XDK et la version UWP d’un jeu.
* **Utiliser le centre de partenaires pour ingérer et publiez votre jeu UWP.** Toutefois, n’utilisez pas d’espace partenaires pour configurer les services Xbox Live, comme votre jeu utilisera la configuration du service que vous avez créé dans XDP.
* **Ne séparez pas la configuration du service Xbox Live entre XDP et partenaires.** XDP et partenaires ne sont pas prenant en charge les unes des autres, et une configuration de service à partir d’une source de la publication remplace la configuration publiée à partir de l’autre source. Cela peut entraîner des données utilisateur à être perdu (réalisations manquantes, effacé enregistre de jeux, etc.) qui peut créer une expérience utilisateur incorrects. Pour cette raison, **nous exigeons que 100 % de la configuration du service Xbox Live est effectuée dans XDP pour cross-play XDK + UWP jeux.**

Il est plus en détail sur ce processus, y compris les éléments qui sont *pas* libre-service dans le [mise en route avec les jeux de cross-play](get-started-with-partner/get-started-with-cross-play-games.md) guide

### <a name="separate-versions-of-xbox-one-and-pcmobile-games-that-are-not-cross-play"></a>Versions distinctes de jeux Xbox One et PC/Mobile qui ne sont pas cross-play
Vous pouvez décider de conserver votre Xbox One version d’un jeu distinct de la version/Mobile PC du même jeu. Dans ce cas, vous créez deux produits distinctes, suivez les instructions pour XDK une Xbox uniquement et un jeu PC/Mobile UWP uniquement respectivement.

Vous ne pouvez pas utiliser la même configuration de service pour les deux versions dans ce cas, et vous devez créer manuellement la configuration du service pour chaque version distincte de votre jeu, XDP ou de l’espace partenaires en conséquence.

<a name="get_ids"></a>

## <a name="get-your-ids"></a>Obtenir votre ID

Pour permettre aux services Xbox Live, vous devrez obtenir plusieurs ID pour configurer votre kit de développement et votre titre. Il peuvent être obtenus en effectuant la configuration du service Xbox Live.

Si vous n’avez pas actuellement d’un titre dans XDP ou partenaires, consultez la section ci-dessus [les portails de Configuration du Service Xbox Live](#xbox_live_portals) pour obtenir des conseils.

### <a name="critical-ids"></a>ID critiques

Il existe trois identificateurs qui sont critiques pour le développement des titres et des applications pour Xbox One : l’ID de bac à sable, l’ID de titre et le SCID.

Bien qu’il soit nécessaire de disposer d’un ID de bac à sable à utiliser un kit de développement, l’ID de titre et SCID ne sont pas requis pour le développement initial, mais sont requis pour toute utilisation de services Xbox Live. Il est donc recommandé de vous procurer les trois à la fois.

#### <a name="sandbox-ids"></a>ID de bac à sable

Le bac à sable fournit une isolation de contenu pour votre kit de développement pendant le développement, en veillant à avoir un environnement propre pour développer et tester votre titre. L’ID de bac à sable identifie votre bac à sable. Une console peut-être accéder uniquement à un bac à sable à tout moment, même si un bac à sable sont accessibles par plusieurs consoles.

ID de bac à sable respectent la casse.

**Partenaires**

Si vous configurez le titre de votre centre de partenaires, vous obtenez votre ID de bac à sable sur la « Xbox Live » racine page de configuration comme indiqué ci-dessous :

![](images/getting_started/devcenter_sandbox_id.png)

**XDP**

Si vous configurez votre titre sur XDP, vous obtenez votre ID de bac à sable sur la page Vue d’ensemble de votre produit, comme indiqué ci-dessous :

![](images/getting_started/xdp_sandbox_id.png)

#### <a name="service-configuration-id-scid"></a>ID de Configuration de service (SCID)

Dans le cadre du développement, vous allez créer une multitude d’autres fonctionnalités en ligne, des primes et des événements. Ces font tous partie de votre configuration de service et demander la SCID pour l’accès.

SCIDs respectent la casse.

**Partenaires**

Pour récupérer votre SCID dans Partner Center, accédez à la section Services de Xbox Live et à *le programme d’installation de Xbox Live* . Votre SCID s’affiche dans le tableau ci-dessous :

![](images/getting_started/devcenter_scid.png)

**XDP**

Pour récupérer votre SCID sur XDP, accédez à la section « Configuration du produit » sous le titre et vous verrez l’ID de titre et SCID.

![](images/getting_started/xdp_scid.png)

#### <a name="title-id"></a>ID de titre

L’ID de titre identifie de manière unique votre titre aux services Xbox Live. Il est utilisé dans les services pour permettre à vos utilisateurs pour accéder aux contenu en direct de votre titre, leurs statistiques utilisateur, primes et ainsi de suite et pour activer la fonctionnalité multijoueur en direct.

ID de titre peuvent respecter la casse, selon où et comment elles sont utilisées.

**Partenaires**

Votre ID de titre dans partenaires se trouve dans la même table que le SCID dans le *le programme d’installation de Xbox Live* page.

**XDP**

Votre ID de titre sur XDP est obtenu à partir d’au même emplacement que le SCID est.

<a name="xbox_live_portals"></a>
