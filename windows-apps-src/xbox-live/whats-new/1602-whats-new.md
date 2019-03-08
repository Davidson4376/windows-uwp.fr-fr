---
title: Nouveautés pour le Kit de développement Xbox Live - février 2016
description: Nouveautés pour le Kit de développement Xbox Live - février 2016
ms.assetid: 7ff34ea4-f07d-4584-98e4-13977994ccca
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c13b2893397a0d84e8919146435ece338bc1afde
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594734"
---
# <a name="whats-new-for-the-xbox-live-sdk---february-2016"></a>Nouveautés pour le Kit de développement Xbox Live - février 2016

Veuillez consulter la [What ' s New - octobre 2015](1510-whats-new.md) article pour ce qui a été ajouté dans la version 1510

## <a name="os-and-tool-support"></a>Prise en charge du système d’exploitation et d’outil
Le Xbox Live SDK prend en charge Windows 10 RTM [Version 10.0.10240] et Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="throttling"></a>la limitation
- Limitation affinée sera bientôt être transférée vers la plupart des Services Xbox Live.  API de Service Xbox (XSAPI) gérer les nouvelles tentatives automatiquement et vous informer des appels qui sont limitées au cours du développement.  Vous trouverez plus de détails dans le [meilleures pratiques appelant Xbox Live](../using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) article dans la Documentation.

## <a name="leaderboards"></a>Classements
- Classements multicolonne est désormais accessible par l’API GetLeaderboard. Si vous fournissez un vecteur des noms de colonnes supplémentaires, le vecteur de colonnes du résultat est rempli si ces colonnes existent.

## <a name="documentation"></a>Documentation
- [Application Insights](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/application-insights) documentation est ici.  Vous pouvez utiliser Application Insights avec un compte Azure gratuit pour afficher les événements de plateforme de données en quasi-temps réel.  Cette fonctionnalité est actuellement uniquement disponible pour les applications UWP en cours d’exécution sur Windows 10 sur le bureau.
- Documentation mise à jour sur l’outil événements courants de Xbox pour les développeurs UWP expliquant comment générer des wrappers pour l’envoi des événements de plateforme de données.  Veuillez noter que cette étape est facultative et vous pouvez continuer à utiliser l’API WriteInGameEvent si vous préférez.
- Utilisation de Fiddler pour déboguer des événements de plateforme de données et assurez-vous qu’ils sont correctement envoyés.  Il s’agit uniquement pour les événements UWP.
- Pour plus d’informations sur la façon de collecter les journaux de l’outil Analyseur de Trace en direct sont disponibles.  Consultez le [analyser les appels à Xbox Live Services](../tools/analyze-service-calls.md) article.
