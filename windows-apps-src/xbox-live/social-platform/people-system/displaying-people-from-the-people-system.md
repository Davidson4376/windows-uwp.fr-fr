---
title: Afficher les personnes à partir du système de personnes
description: En savoir plus à propo le flux de code pour afficher des personnes en utilisant le système de personnes Xbox Live.
ms.assetid: c97b699f-ebc2-4f65-8043-e99cca8cbe0c
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 512de51f2a0e30a9b41a5e49f3dc3ababe30fc4d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658304"
---
# <a name="display-people-from-the-people-system"></a>Afficher les personnes à partir du système de personnes

Retournent uniquement les données appartienne à ce service des services au-delà de Xbox Live et retournent uniquement les références XUID aux utilisateurs ; par exemple, le service de personnes possède uniquement et retourne les XUIDs qui se trouvent sur la liste des contacts d’un utilisateur et des informations basiques sur chacune de ces XUIDs (par exemple, état favori). Le service de présence possède les données sur les informations d’état en ligne de XUIDs. Le service de leaderboards possède des informations de classement sur des listes de XUIDs. Afficher des informations de nom et le gamertag ne sont jamais retournées à partir de n’importe quel service autre que le service de profil et, par conséquent, l’appel de plusieurs services nécessaire à l’affichage des listes de personnes dans les expériences.

Le modèle général d’appel pour le service API sont à effectuer un aller-retour pour tout d’abord obtenir une liste de XUIDs à partir du service capable de mieux filtrer ou trier la liste, puis effectuer des appels aller-retour simultanées à d’autres services nécessaires pour obtenir des métadonnées supplémentaires obligatoires pour chaque XIUD. Dans le cas des images, un troisième aller-retour d’appels devra peut-être obtenir des images à partir de l’URL d’image.

Pour réduire le nombre d’allers-retours requis pour obtenir des données sur la liste de personnes d’un utilisateur, un personnes *moniker* est mis en place pour les services concernés. Cette nouvelle fonctionnalité permet aux appelants d’abstraitement express pour le service principal que le service doit obtenir la liste des personnes de l’utilisateur à partir du service de personnes et ensuite utiliser ce jeu de XUIDs étendue de la valeur de retour.

Voici quelques exemples de scénarios de flux appel qui illustrent comment titres obtenir des données à partir des services liés à des personnes :

-   Liste des utilisateurs actuellement dans le jeu
-   Liste des personnes de l’utilisateur actuel qui sont en ligne
-   Classement global contenant les utilisateurs aléatoires
-   Classement des personnes de l’utilisateur


## <a name="list-of-users-currently-in-game"></a>Liste des utilisateurs actuellement dans le jeu

| A titre  | Objectif  | Champ à afficher  | Flux d’appels
|-------------------------------------------------|----------------------------------------------------|--------------------|--------------------------------------|
| Liste des XUIDs aléatoire d’autres utilisateurs dans le jeu | Pour afficher les informations minimales pour chacun des autres utilisateurs | GameDisplayName \[profil\] | Appelle le profil avec la liste des XUIDs. |


## <a name="list-of-the-current-users-people-who-are-online"></a>Liste des personnes de l’utilisateur actuel qui sont en ligne

## <a name="title-has"></a>Titre a :
XUID de l’utilisateur actuel

## <a name="goal"></a>Objectif
Pour afficher une liste riche d’utilisateurs en ligne qui se trouvent dans la liste de personnes de l’utilisateur actuel

## <a name="field-to-render-owning-service"></a>Pour restituer \[propriétaire du service\]
* Indicateur favoris [personnes]
* Perso (Profile)
* GameDisplayName (Profile)
* État en ligne de base (boule verte) [présence]

## <a name="call-flow"></a>Flux d’appels
1. Appelez la présence, en passant le moniker de personnes pour obtenir le XUIDs et l’état en ligne pour chacune des personnes de l’utilisateur.
1. En parallèle :
 1. Appelle le profil, en passant la liste complète des XUIDs pour obtenir le nom d’affichage et l’URL d’image pour chacun.
 1. Appeler des personnes, en passant dans la liste des XUIDs pour déterminer si une est Favoris de l’utilisateur.
1. Après l’appel de profil :
 1. Obtenir des images pour chaque URL de l’image

## <a name="global-leaderboard-containing-random-users"></a>Classement global contenant les utilisateurs aléatoires

## <a name="title-has"></a>Titre a :
L’ID/nom du classement

## <a name="goal"></a>Objectif
Pour afficher les informations de base pour chaque utilisateur sur le classement

## <a name="field-to-render-owning-service"></a>Pour restituer [service propriétaire]
* Indicateur favoris [personnes]
* GameDisplayName (Profile)
* Rang [Leaderboards]
* Score [Leaderboards]

## <a name="call-flow"></a>Flux d’appels
1. Classements d’appel pour obtenir les XUIDs, classement et les scores pour le classement particulier.
1. En parallèle :
 1. Appelle le profil, en passant dans la liste des XUIDs pour obtenir le nom d’affichage et l’URL d’image pour chacun.
 1. Appeler des personnes, en passant dans la liste des XUIDs pour déterminer si une est Favoris de l’utilisateur.

## <a name="leaderboard-of-users-people"></a>Classement des personnes de l’utilisateur

## <a name="title-has"></a>Titre a :
* L’ID/nom du classement
* XUID de l’utilisateur actuel

## <a name="goal"></a>Objectif
Pour afficher les informations de base pour chaque utilisateur sur le classement

## <a name="field-to-render-owning-service"></a>Pour restituer [service propriétaire]
* Indicateur favoris [personnes]
* GameDisplayName (Profile)
* Rang [Leaderboards]
* Score [Leaderboards]

## <a name="call-flow"></a>Flux d’appels
1. Appeler des résultats commerciaux, en passant le moniker de personnes pour obtenir les XUIDs, classement et les scores pour le classement particulier limité à la liste des contacts de l’utilisateur.
1. En parallèle :
 1. Appelle le profil, en passant dans la liste des XUIDs pour obtenir le nom d’affichage et l’URL d’image pour chacun.
 1. Appeler des personnes, en passant dans la liste des XUIDs pour déterminer si une est Favoris de l’utilisateur.
