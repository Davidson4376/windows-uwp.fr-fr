---
title: Mémoire et performances du gestionnaire social
description: Décrit les considérations relatives à la mémoire et les performances lors de l’utilisation de Xbox Live sociaux API Gestionnaire.
ms.assetid: 2540145e-b8e2-4ab5-9390-65e2c9b19792
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, le Gestionnaire de réseau social, personnes
ms.localizationpriority: medium
ms.openlocfilehash: 7c6a0a31c8fe82faa644a59147060f9323d42eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618454"
---
# <a name="social-manager-memory-and-performance-overview"></a>Vue d’ensemble de la Performance et de mémoire sociale Manager

## <a name="memory"></a>Mémoire
Sociale Manager permet désormais qu’il a alloué la mémoire persistante à conserver dans l’espace de titre. Un allocateur de mémoire personnalisé pour une utilisation par le Gestionnaire de réseau social peut être spécifié en appelant `xbox_live_services_settings::set_memory_allocation_hooks()`. Ce hook d’allocateur de mémoire sera peut également servir pour les allocations de mémoire volumineux futurs utilise la Xbox Live API (XSAPI).

Le pire des cas pour l’allocation de mémoire par le Gestionnaire de réseau social par défaut doivent être environ 4,75 Mo (1). Il existe certains surcharge que le Gestionnaire de réseau social peut allouer selon les mises à jour de l’agrégation RTA et HTTP. Si la création d’un `xbox_social_user_group` à partir de la liste, chaque ajouté membre occuperont 2,43 Ko supplémentaires. Si un utilisateur est ajouté via la `create_social_social_user_group_from_list`, `update_social_user_group`, ou l’utilisateur qui ajoute une fonction friend en dehors du titre, cela peut entraîner un realloc trouver de l’espace de mémoire contiguë.

(1) le Mo 4,75 provient = 1000 `xbox_social_user` à 2,43 Ko chacun * 2. La version 2 est que le Gestionnaire de réseau social conserve une double mémoire tampon.

## <a name="additional-user-limits"></a>Limites d’utilisateurs supplémentaires
Les réseaux sociaux Manager possède actuellement une restriction de 100 utilisateurs ajoutés au graphique. Il s’agit en raison de deux problèmes :

1. Le nombre maximal d’abonnements RTA qu’un utilisateur peut avoir est 1100. Si un utilisateur local a des amis de 1000, qui laisse uniquement 100 pour des abonnements supplémentaires.
2. La quantité maximale de taille de lot d’utilisateurs qui peuvent être envoyés à PeopleHub est actuellement environ 100.

Les réseaux sociaux Manager communique cela en n’autorisant ne pas un groupe d’utilisateurs social à partir de la liste contenir plus de 100 utilisateurs. Il existe une limite globale de 100 total des utilisateurs supplémentaires qui peuvent être dans le système à tout moment via `create_social_user_group_from_list`.

## <a name="processing-time"></a>Durée moyenne de traitement
Le gestionnaire sociale au pire cas 1100 les utilisateurs doivent. Les caractéristiques de performances du Gestionnaire de réseau social sur Xbox One a le pire des cas de ms 0,3 à 0,5 ms pour `do_work` en fonction du nombre de graphes sociales créé. Le cas par défaut est ms 0,01 pour avec aucun groupes créé jusqu'à 0,2 ms pour un groupe avec 1 000 utilisateurs, qu’il contient.
