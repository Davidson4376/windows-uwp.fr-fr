---
title: Nouveautés dans les API de Xbox Live - juillet 2017
description: Nouveautés dans les API de Xbox Live - juillet 2017
ms.assetid: ''
ms.date: 07/16/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, quelles sont les nouveautés, juillet 2017
ms.localizationpriority: medium
ms.openlocfilehash: 47db721a98b71fd6f4b5175a88ddccd00048d8d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618824"
---
# <a name="whats-new-for-the-xbox-live-apis---july-2017"></a>Nouveautés pour les API Live Xbox - juillet 2017

Veuillez consulter la [What ' s New - juin 2017](1706-whats-new.md) article pour ce qui a été ajouté dans la version de juin 2017.

Vous pouvez également vérifier le [historique des validations de Xbox Live API GitHub](https://github.com/Microsoft/xbox-live-api/commits/master) pour afficher toutes les modifications de code récents aux API Xbox Live.

## <a name="xbox-live-features"></a>Fonctionnalités Xbox Live

### <a name="multiplayer-updates"></a>Mises à jour en mode multijoueur

Interrogation maintenant les handles de l’activité et les handles de recherche inclut les propriétés de session personnalisée dans la réponse.

### <a name="tournaments"></a>Tournois

Nouvelles API ont été ajoutées pour prendre en charge de tournois. Vous pouvez maintenant utiliser la classe xbox::services::tournaments::tournament_service pour accéder au service de tournois de votre titre.
Ces nouvelles tournoi API activer les scénarios suivants :
* Interroger le service pour rechercher tous les tournois existants pour le titre en cours.
* Extraire des informations sur un tournoi à partir du service.
* Interroger le service pour récupérer la liste des équipes pour un tournoi.
* Extraire des informations sur les équipes pour un tournoi à partir du service.
* Suivre les modifications apportées à tournois et les équipes en utilisant des abonnements de l’activité en temps réel (RTA).
