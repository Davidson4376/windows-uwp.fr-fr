---
title: Applications et appareils connectés (projet « Rome »)
description: Cette section explique comment utiliser la plateforme Systèmes distants pour identifier les appareils distants, lancer une application sur un appareil distant et communiquer avec un service d’application sur un appareil distant.
ms.date: 06/08/2018
ms.topic: article
keywords: appareils Windows 10, uwp, connectés, les systèmes distants, rome, project rome
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: ae9229378f75adeb215a881bdaf955b010cd7806
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366374"
---
# <a name="connected-apps-and-devices-project-rome"></a>Applications et appareils connectés (projet « Rome »)

Cette section explique comment connecter des applications sur les appareils et plateformes à l’aide de [Project Rome](https://developer.microsoft.com/en-us/windows/project-rome). Pour savoir comment implémenter Project Rome dans un scénario multiplateforme, visitez le [page docs principaux pour Project Rome](https://docs.microsoft.com/en-us/windows/project-rome/).

La plupart des utilisateurs ont plusieurs appareils et commencent souvent une activité sur un pour la terminer sur un autre. Pour répondre à cette situation, les applications doivent être accessibles sur plusieurs appareils et plateformes. Project Rome permet de détecter les appareils à distance, lancer une application sur un périphérique distant et communiquer avec un service d’application sur un périphérique distant.

Les [API Systèmes distants](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems) intégrées dans Windows 10 version 1607 vous permettent d’écrire des applications grâce auxquelles les utilisateurs peuvent commencer une tâche sur un appareil et la terminer sur un autre. La tâche reste le point central, et les utilisateurs peuvent faire leur travail sur l’appareil le plus pratique pour eux. Par exemple, un utilisateur écoute la radio sur son téléphone en voiture, mais une fois à la maison, il voudra sans doute transférer la lecture à son Xbox One qui est raccordée à sa chaîne stéréo.

Vous pouvez également utiliser le projet Rome pour les appareils compléments ou des scénarios de contrôle à distance. Utilisez les API de messagerie de service d'application pour créer un canal d’application entre deux appareils afin d’échanger des messages personnalisés. Par exemple, vous pouvez écrire une application pour votre téléphone, qui contrôle la lecture sur votre téléviseur, ou une application complément qui fournit des informations sur les personnages d’une émission de télévision que vous regardez via une autre application.  

Les appareils peuvent être connectés à proximité par Bluetooth et sans fil, ou à distance par le cloud ; ils sont liés par le compte Microsoft (MSA) de la personne qui les utilise.

Pour des exemples illustrant comment détecter le système distant, lancer une application sur un système distant et utiliser des services d’application pour échanger des messages entre des applications qui s’exécutent sur deux systèmes, consultez la section [Exemple de Systèmes distants UWP](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ).

Pour plus d’informations sur le projet Rome en général, y compris les ressources pour l’intégration inter-plateforme, accédez à [aka.ms/project-rome](https://aka.ms/project-rome).

| Rubrique | Description |
|-------|-------------|
| [Lancer une application sur un appareil distant](launch-a-remote-app.md) | Découvrez comment lancer une application sur un appareil distant. Cette rubrique décrit le cas d'utilisation le plus simple et la configuration préliminaire.  |
| [Découvrir des appareils distants](discover-remote-devices.md)  | Découvrez comment détecter les appareils auxquels vous pouvez vous connecter. |
| [Communiquer avec un service d’application distant](communicate-with-a-remote-app-service.md) | Découvrez comment interagir avec une application sur un appareil distant. |
| [Connecter des appareils via des sessions à distance](remote-sessions.md) | Créez des expériences partagées sur plusieurs appareils en les rejoignant dans une session à distance. |
| [Poursuivre l’activité utilisateur, même sur différents appareils](useractivities.md)| Aider les utilisateurs à reprendre ce que leur faisiez dans votre application, même sur plusieurs appareils.|
| [Meilleures pratiques d’activités utilisateur](useractivities-best-practices.md)| Découvrez les pratiques recommandées pour la création et la mise à jour des activités de l’utilisateur.|
