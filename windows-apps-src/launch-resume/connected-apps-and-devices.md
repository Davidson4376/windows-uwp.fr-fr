---
author: TylerMSFT
title: "Applications et appareils connectés (projet « Rome »)"
description: "Cette section explique comment utiliser la plateforme Systèmes distants pour identifier les appareils distants, lancer une application sur un appareil distant et communiquer avec un service d’application sur un appareil distant."
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 357cf459fffc46e5d77f316af881e1a6e8bd8867
ms.lasthandoff: 02/08/2017

---

# <a name="connected-apps-and-devices-project-rome"></a>Applications et appareils connectés (projet « Rome »)

Cette section explique comment connecter des applications sur différents appareils et plateformes à l’aide du projet « Rome ». Découvrez comment détecter les appareils distants, lancer une application sur un appareil distant et communiquer avec un service d’application sur un appareil distant.

La plupart des gens ont plusieurs appareils et commencent souvent une activité sur un pour la terminer sur un autre. Pour répondre à cette situation, les applications doivent être accessibles sur plusieurs appareils et plateformes.

Les [API Systèmes distants](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems) intégrées dans Windows 10 version 1607 vous permettent d’écrire des applications grâce auxquelles les utilisateurs peuvent commencer une tâche sur un appareil et la terminer sur un autre. La tâche reste le point central, et les utilisateurs peuvent faire leur travail sur l’appareil le plus pratique pour eux. Par exemple, vous pouvez écouter la radio sur votre téléphone en voiture, mais une fois à la maison, vous voudrez sans doute transférer la lecture à votre Xbox One qui est raccordée à votre chaîne stéréo.

Vous pouvez également utiliser le projet « Rome » pour les appareils compléments, ou des scénarios de contrôle à distance. Utilisez les API de messagerie d’application pour créer un canal d’application entre deux appareils afin d’échanger des messages personnalisés. Par exemple, vous pouvez écrire une application pour votre téléphone, qui contrôle la lecture sur votre téléviseur, ou une application complément qui fournit des informations sur les personnages d’une émission de télévision que vous regardez sur une autre application.  

Les appareils peuvent être connectés à proximité par Bluetooth et sans fil, ou à distance par le cloud, à l’aide du compte Microsoft de la personne qui les utilise.

Pour des exemples illustrant comment détecter le système distant, lancer une application sur un système distant et utiliser des services d’application pour échanger des messages entre des applications qui s’exécutent sur deux systèmes, consultez la section [Exemple de Systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ).

| Rubrique | Description |
|-------|-------------|
| [Détecter des appareils distants](discover-remote-devices.md)  | Découvrez comment détecter les appareils auxquels vous pouvez vous connecter. |
| [Lancer une application sur un appareil distant](launch-a-remote-app.md) | Découvrez comment lancer une application sur un appareil distant.  |
| [Communiquer avec un service d’application distant](communicate-with-a-remote-app-service.md) | Découvrez comment interagir avec une application sur un appareil distant. |

