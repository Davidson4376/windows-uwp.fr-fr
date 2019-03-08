---
title: Nouveautés pour les API Live Xbox - avril 2017
description: Nouveautés pour les API Live Xbox - avril 2017
ms.assetid: ''
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, arena, tournois
ms.localizationpriority: medium
ms.openlocfilehash: a9256bfce05c14a0bfa63c7ec656dd899648d844
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651304"
---
# <a name="whats-new-for-the-xbox-live-apis---april-2017"></a>Nouveautés pour les API Live Xbox - avril 2017

Veuillez consulter la [What ' s New - mars 2017](1703-whats-new.md) article pour ce qui a été ajouté dans la version de mars 2017.

## <a name="xbox-services-apis"></a>API de Services Xbox

### <a name="visual-studio-2017"></a>Visual Studio 2017

API Xbox Live ont été mis à jour pour prendre en charge Visual Studio 2017, pour les titres de plateforme universelle Windows (UWP) et Xbox One.

### <a name="tournaments"></a>Tournois

Nouvelles API ont été ajoutées pour prendre en charge de tournois. Vous pouvez maintenant utiliser le `xbox::services::tournaments::tournament_service` classe pour accéder au service de tournois de votre titre.

Ces nouvelles tournoi API activer les scénarios suivants :

* Interroger le service pour rechercher tous les tournois existants pour le titre en cours.
* Extraire des informations sur un tournoi à partir du service.
* Interroger le service pour récupérer la liste des équipes pour un tournoi.
* Extraire des informations sur les équipes pour un tournoi à partir du service.
* Suivre les modifications apportées à tournois et les équipes en utilisant des abonnements de l’activité en temps réel (RTA).
