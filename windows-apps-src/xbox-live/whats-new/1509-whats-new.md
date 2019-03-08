---
title: Nouveautés pour le Kit de développement Xbox Live - septembre 2015
description: Nouveautés pour le Kit de développement Xbox Live - septembre 2015
ms.assetid: 84b82fde-f6f3-4dc2-b2df-c7c7313a2cc3
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c618a738dc2670d3d3de1fa2f4c4108c24130eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602274"
---
# <a name="whats-new-for-the-xbox-live-sdk---september-2015"></a>Nouveautés pour le Kit de développement Xbox Live - septembre 2015

Veuillez consulter la [What ' s New - août 2015](1508-whats-new.md) article pour ce qui a été ajouté dans la version d’août 2015.

La version de septembre de Xbox Live SDK inclut les mises à jour suivantes :

## <a name="os-and-tool-support"></a>Prise en charge du système d’exploitation et d’outil ##
Le Xbox Live SDK prend en charge Windows 10 RTM [Version 10.0.10240] et Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="contextual-search-apis"></a>API de recherche contextuelle
* Activer votre titre ou une application pour rechercher des diffusions à partir de votre game(s) avec des statistiques en temps réel de votre choix.
* Consultez les nouvelles API dans Microsoft::Xbox::Services::ContextualSearch

## <a name="app-insights-for-events"></a>Application Insights pour les événements

| Remarque |
|------|
| Application Insights s’applique uniquement aux titres UWP.  Si vous développez un titre XDK, cette section ne s’applique pas à vous |

<p/>

* Les événements écrits à l’aide de write_in_game_event() peuvent être affichés à l’aide d’AppInsights
* Sur cette documentation sera proposée à l’avenir, en attendant, contactez votre mère pour obtenir l’accès

## <a name="logging"></a>Journalisation
* service_call_logging_config in xbox::services::experimental
* Pour démarrer et arrêter les traces via xbTrace.exe sur la console, vous devez appeler register_for_protocol_activation sur la classe service_call_logging_config.  Effectuez cet appel une seule fois pendant l’initialisation de votre jeu.

## <a name="resync-for-rta"></a>Resynchronisation pour l’agrégation RTA
* La resynchronisation peut se produire lorsque le service d’agrégation RTA est convaincu que les informations des utilisateurs peuvent être obsolètes
* Titres doivent appeler les appels HTTP correspondants pour les abonnements auxquels ils sont abonnés à
* Titres n’ont pas à se réabonner
* Xbox::services::real_time_activity_service::add_resync_handler ajouté
* Xbox::services::real_time_activity_service::remove_resync_handler supprimé
* Http_status_429_too_many_requests ajouté
* Cette condition d’erreur se produira lorsqu’un titre est limité pour l’envoi de trop nombreuses requêtes http

## <a name="documentation"></a>Documentation
* Migration à Xbox Live Services API 2.0
* Gestion des erreurs
* Authentification de Xbox Live dans Windows 10
* Recherche contextuelle
