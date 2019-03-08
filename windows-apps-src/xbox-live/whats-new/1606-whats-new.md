---
title: Nouveautés pour le Kit de développement Xbox Live - juin 2016
description: Nouveautés pour le Kit de développement Xbox Live - juin 2016
ms.assetid: 306e7fa8-14f0-437f-a991-6693f5c0d6a8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d984d054d9e5fd7f9d34b42c1a224d53632e719
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595284"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2016"></a>Nouveautés pour le Kit de développement Xbox Live - juin 2016

Veuillez consulter la [What ' s New - avril 2016](1604-whats-new.md) article pour ce qui a été ajouté dans la version d’avril 2016.

> **Remarque :** Si vous utilisez le Kit de développeurs Xbox (XDK), puis le *What ' s New - avril 2016* article couvre les nouvelles fonctionnalités qui a été ajoutée dans la mesure où le XDK mars mise en production.

## <a name="os-and-tool-support"></a>Prise en charge du système d’exploitation et d’outil
Le Xbox Live SDK prend en charge Windows 10 RTM [Version 10.0.10240] et Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="contextual-search"></a>Recherche contextuelle
Les nouvelles classes suivantes ont été ajoutées à la `contextual_search` API pour récupérer le jeu d’éléments :

* `contextual_search_game_clip`
* `contextual_search_game_clip_stat`
* `contextual_search_game_clip_thumbnail`
* `contextual_search_game_clip_uri_info`
* `contextual_search_game_clips_result`

Consultez la documentation de référence pour plus d’informations.

## <a name="networking"></a>Mise en réseau
La Xbox Live SDK inclut désormais une nouvelle assertion qui détecte si les websockets 5 ou plus sont créés par l’utilisateur dans une instance unique de titre.

Vous pouvez désactiver cette assertion en appelant `disable_asserts_for_maximum_number_of_websockets_activated()`.

> **Remarque :** Gestionnaire de sociale et le Gestionnaire de mode multijoueur utilisent un websocket combiné unique si vous utilisez ces fonctionnalités dans votre titre.

## <a name="tools"></a>Outils
* La Xbox Live résilience plug-in pour Fiddler est désormais inclus dans le Kit de développement Xbox Live.  
Il est un module complémentaire à Fiddler qui permet aux développeurs de bloquer de manière sélective les appels à Xbox Live.
À l’aide de ce plug-in, les développeurs peuvent tester la résilience entre les interruptions de service partielle dans leurs titres.
L’outil inclut un nombre de Xbox Live service points de terminaison intégrés et différents types d’erreurs pour le test.
Les titres de tous les Xbox One et UWP sont pris en charge.
