---
title: Nouveautés pour le Kit de développement Xbox Live - juin 2015
description: Nouveautés pour le Kit de développement Xbox Live - juin 2015
ms.assetid: 354bcd47-2564-4dd5-89e3-242bca462b35
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a42d0fb0a3cb457a60a0542bfc5966893d00f18b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627874"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2015"></a>Nouveautés pour le Kit de développement Xbox Live - juin 2015

La version de juin de la Xbox Live SDK inclut les mises à jour suivantes :

## <a name="os-and-tool-support"></a>Prise en charge du système d’exploitation et d’outil ##
Le Kit de développement Xbox Live prend désormais en charge la dernière version d’Insider de Windows 10 et Visual Studio 2015 RC.

## <a name="title-callable-ui-apis"></a>API de l’interface utilisateur pouvant être appelé par titre

| Remarque |
|------|
| Cette section s’applique uniquement aux titres UWP, les titres XDK doivent faire référence à la classe Windows.Xbox.UI.SystemUI pour TCUI  |

Le Xbox Live SDK contient désormais un wrapper les API qui prennent en charge l’interface utilisateur pouvant être appelé de titre (TCUI) pour afficher l’interface utilisateur sur un appareil Windows 10 PC/Mobile.

Ces API est uniquement disponibles pour les applications UWP.

Vous pouvez appeler les wrappers TCUI à partir du TitleCallableUI (WinRT) et les classes de title_callable_ui (C++).

Le stock IU incluent :
* Un interface utilisateur du sélecteur de lecteur
* Un interface utilisateur du sélecteur de jeu invitation
* Une carte de visite l’interface utilisateur de lecteur
* Un ami ajouter/supprimer l’interface utilisateur
* Un primes de titre du jeu d’afficher l’interface utilisateur

Voir le nouveau *TCUI* exemple par exemple l’utilisation des nouvelles API. Vous trouverez l’exemple sous {*racine source de kit de développement logiciel*} \Samples\TCUI.

## <a name="new-authentication-model-for-uwp-apps"></a>Nouveau modèle d’authentification pour les applications UWP
Le Kit de développement Xbox Live prend désormais en charge à l’aide d’un compte Microsoft (MSA) pour identifier le joueur Xbox Live sur un appareil Windows 10 PC/Mobile.

Vous pouvez maintenant connecter un utilisateur à l’aide de la XboxLiveUser (WinRT) et les classes de xbox_live_user (C++).

## <a name="new-api-for-writing-events-in-uwp-apps"></a>Nouvelle API pour écrire des événements dans les applications UWP

| Remarque |
|------|
| Cette section s’applique uniquement aux titres UWP.  Les développeurs XDK doivent faire référence à la [les événements de jeu](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/game-events) article pour plus d’informations XDK  |

Les classes d’events_service (C++) et un nouveau EventsService (WinRT) vous permettent d’écrire des événements dans le jeu que vous peuvent mettre à jour les statistiques de l’utilisateur, des primes, des classements, etc. Ces nouvelles classes sont pour les applications UWP uniquement.

## <a name="breaking-change-to-event-handlers"></a>Changement cassant aux gestionnaires d’événements ##
Tous les gestionnaires d’événements dans le Kit de développement logiciel C++ ont été modifiés à partir d’une seule `void set_*_handler()` méthode à une paire de `function_context add_*_handler()` et `void remove_*_handler(function_context context)` méthodes.

Chaque `add_*_handler()` retourne maintenant de méthode un `function_context` objet. Lorsque vous supprimez le Gestionnaire d’événements, vous devez passer le `function_context` objet.

Exemple :
```
function_context subscriptionLostContext = xbox_live_context()->multiplayer_service().add_multiplayer_subscription_lost_handler(...);

localUser->xbox_live_context()->multiplayer_service().remove_multiplayer_subscription_lost_handler(subscriptionLostContext);
```

## <a name="new-apis-for-managing-multiplayer-scenarios"></a>Nouvelles API pour la gestion des scénarios multijoueurs
Le Xbox Live SDK inclut désormais un ensemble de APIs C++ pour la gestion des scénarios multijoueurs courants. Les API sont inclus dans l’espace de noms xbox.services.experimental.multiplayer.

Ces API sont dans l’espace de noms expérimental qui signifie qu’elles sont susceptibles de changer en fonction des commentaires.  Nous encourageons les développeurs à les utiliser et de fournir des commentaires.

Voir le nouveau *MultiplayerManager* exemple par exemple l’utilisation de ces nouvelles API. Vous trouverez l’exemple sous {*racine source de kit de développement logiciel*} \Samples\MultiplayerManager.
