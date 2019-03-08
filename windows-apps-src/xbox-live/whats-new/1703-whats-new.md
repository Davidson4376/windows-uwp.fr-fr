---
title: Nouveautés pour le Kit de développement Xbox Live - mars 2017
description: Nouveautés pour le Kit de développement Xbox Live - mars 2017
ms.assetid: 03180585-6f87-4929-acfc-750bd78988a0
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: be8127e01d8eaae96a1d71f71967a653c00b0280
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595344"
---
# <a name="whats-new-for-the-xbox-live-sdk---march-2017"></a>Nouveautés pour le Kit de développement Xbox Live - mars 2017

Veuillez consulter la [What ' s New - décembre 2016](1612-whats-new.md) article pour ce qui a été ajouté dans la version de décembre 2016.

## <a name="xbox-services-api"></a>API des Services Xbox

### <a name="data-platform-2017"></a>Plateforme de données 2017

Nous avons introduit une API simplifiée de statistiques.  En règle générale, vous deviez envoyer des événements correspondant aux règles statistiques définies sur XDP ou partenaires, ces seraient mise à jour les valeurs statistiques dans le cloud.  Nous faisons référence à ce modèle en tant que statistiques 2013.

Statistiques 2017, votre titre est maintenant dans le contrôle des valeurs des jours fériés.  Vous appelez simplement une API avec la dernière valeur stat, et qui est envoyé au service directement, sans la nécessité pour les événements.  Cet exemple utilise la nouvelle `StatsManager` API et vous trouverez plus d’informations dans [statistiques](../leaderboards-and-stats-2017/player-stats.md)

### <a name="github"></a>GitHub

Xbox Live API (XSAPI) est désormais disponible sur GitHub à l’adresse [ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api).  Les fichiers binaires qui sont fournis avec le XDK ou en tant que NuGet packages est toujours recommandé d’utiliser, toutefois vous avez la possibilité d’utiliser la source et nous apprécions les contributions de code source.  

## <a name="xbox-live-creators-program"></a>Programme Créateurs Xbox Live

Le programme Xbox Live Creators est un programme de développeurs offre un sous-ensemble des fonctionnalités Xbox Live à un public plus large de développeur.  Si vous êtes dans le ID@Xbox programme, cela n’aura pas aucun impact sur vous.

Vous trouverez plus d’informations sur le programme dans [vue d’ensemble du programme de développeurs](../developer-program-overview.md).

## <a name="documentation"></a>Documentation

Il existe les nouveaux articles suivants

| Article | Description |
|---------|-------------|
|[Configuration du Service Live Xbox](../xbox-live-service-configuration.md) | Mise à jour d’informations sur le processus de configuration de service pour le titre de la Xbox Live
| [Configurer Xbox Live dans Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) | Nouvelles informations sur le programme d’installation Unity pour les développeurs de programme Xbox Live Creators |
| [Les bacs à sable Xbox Live](../xbox-live-sandboxes.md) | Un guide simplifié à l’isolation de contenu et des bacs à sable Xbox Live |
| [Comptes de tests en temps réel de Xbox](../xbox-live-test-accounts.md) | Informations sur la façon de test travail de comptes et comment les créer sur les partenaires |
