---
title: Configuration du service Xbox Live Creators
description: En savoir plus sur la configuration du service Xbox Live pour le programme Creators.
ms.assetid: 22b8f893-abb3-426e-9840-f79de0753702
ms.date: 10/03/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 76d25a48caadb908e30e6e1897c19178e2b837e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662234"
---
# <a name="xbox-live-service-configuration-for-the-creators-program"></a>Configuration du service Xbox Live pour le programme Créateurs.

## <a name="what-is-service-configuration"></a>Qu’est la Configuration de Service ?

Vous connaissez peut-être certaines des fonctionnalités Xbox Live comme [Leaderboards](../leaderboards-and-stats-2017/leaderboards.md) et [stockage connecté](../storage-platform/connected-storage/connected-storage-technical-overview.md).

Dans le cas où vous n’êtes pas le cas, nous expliquerons brièvement Leaderboards comme exemple. Classements permettent des joueurs afficher une valeur représentant une réalisation, par rapport à d’autres joueurs. Par exemple les meilleurs scores dans un jeu d’arcade, des heures de présentation dans un jeu de course ou headshots dans un tir. Mais, contrairement à une machine arcade qui affiche uniquement les scores les plus élevés des lecteurs ayant jouer sur l’ordinateur physique, à Xbox Live il est possible d’afficher les meilleurs scores à partir du monde.

Mais pour ce faire, vous devez effectuer une configuration à usage unique afin que Xbox Live connaît votre classement. Par exemple si les valeurs doivent être triées dans l’ordre croissant ou décroissant de valeur, et quelle partie des données qu’il doit être de tri.

Cette configuration se produit dans [partenaires](https://partner.microsoft.com/dashboard) pour le programme Xbox Live Creators et vous pouvez lire [mise en route avec Xbox Live](get-started-with-xbox-live-creators.md) pour apprendre à configurer.

## <a name="get-your-ids"></a>Obtenir votre ID

Pour permettre aux services Xbox Live, vous devrez obtenir plusieurs ID pour configurer votre environnement de développement et le titre. Il peuvent être obtenus en mettant à jour la configuration de votre service Xbox Live.

Si vous n’avez pas actuellement d’un titre dans Partner Center, consultez [création et test d’un nouveau titre de créateurs](create-and-test-a-new-creators-title.md) pour obtenir des conseils.

### <a name="critical-ids"></a>ID critiques

Il existe trois identificateurs qui sont critiques pour le développement des titres et des applications pour Xbox One : l’ID de bac à sable, l’ID de titre et la configuration du service ID (SCID).

Bien qu’il soit nécessaire de disposer d’un ID de bac à sable pour configurer votre environnement de développement, l’ID de titre et SCID ne sont pas requis pour le développement initial, mais sont requis pour toute utilisation de services Xbox Live. Il est donc recommandé de vous procurer les trois à la fois. Vous pouvez afficher tous les ID sur la « Xbox Live » racine page de configuration comme indiqué ci-dessous :

![](../images/getting_started/devcenter_sandbox_id.png)

#### <a name="sandbox-ids"></a>ID de bac à sable

Le bac à sable fournit une isolation de contenu pour votre environnement pendant le développement, de s’assurer qu’il est nettoyé pour développer et tester votre titre. L’ID de bac à sable identifie votre bac à sable. Une console peut-être accéder uniquement à un bac à sable à tout moment, même si un bac à sable sont accessibles par plusieurs consoles.

ID de bac à sable respectent la casse.

#### <a name="service-configuration-id-scid"></a>ID de Configuration de service (SCID)

Dans le cadre du développement, vous allez créer des statistiques, des tableaux de résultats et une multitude d’autres fonctionnalités en ligne. Ces font tous partie de votre configuration de service et demander la SCID pour l’accès. SCIDs respectent la casse.

#### <a name="title-id"></a>ID de titre

L’ID de titre identifie de manière unique votre titre aux services Xbox Live. Il est utilisé dans les services pour permettre à vos utilisateurs pour accéder aux contenu en direct de votre titre, leurs statistiques utilisateur, primes et ainsi de suite et pour activer la fonctionnalité multijoueur en direct.

ID de titre peuvent respecter la casse, selon où et comment elles sont utilisées.

## <a name="publish-your-xbox-live-service-configuration"></a>Publiez votre configuration de service Xbox Live

Lorsque vous apportez des modifications à la configuration de Xbox Live à votre jeu, vous devez publier les modifications avant qu’ils sont récupérés par le reste de la Xbox Live et peuvent être vus par votre jeu. Lorsque vous travaillez toujours sur votre jeu, vous publiez dans votre propre bac à sable de développement. Le bac à sable de développement vous permet de travailler sur les modifications apportées à votre jeu dans un environnement isolé. Quand votre jeu est publié pour le grand public, la configuration de Xbox Live sera automatiquement publiée pour le bac à sable de vente au détail.
Par défaut, les Consoles une Xbox et PC Windows 10 sont dans le bac à sable de vente au détail.

Dans la page de configuration Xbox Live, cliquez sur le **Test** bouton Publier la configuration actuelle de Xbox Live à votre sandbox de développement.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)