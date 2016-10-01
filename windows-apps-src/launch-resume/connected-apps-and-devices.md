---
author: TylerMSFT
title: "Applications et appareils connectés (projet «Rome»)"
description: "Cette section décrit comment utiliser le projet «Rome» pour identifier les appareils connectés, lancer une application sur un autre appareil et communiquer avec une application sur un appareil distant."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: 4f49acfd7efcb10d99f9d23884d20c0fc51e5a4a

---

# Applications et appareils connectés (projet «Rome»)

Cette section explique comment connecter des applications sur différents appareils et plateformes à l’aide du projet «Rome». Découvrez comment détecter les appareils connectés, lancer une application sur un autre appareil et communiquer avec une application sur un appareil distant.

La plupart des gens possèdent plusieurs appareils et commencent souvent une activité sur un appareil pour la terminer sur un autre. Pour ce faire, les applications doivent être accessibles sur plusieurs appareils et plateformes.

Les [API Systèmes distants](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems) intégrées dans Windows10 version1607 vous permettent d’écrire des applications grâce auxquelles les utilisateurs peuvent commencer une tâche sur un appareil et la terminer sur un autre. La tâche reste le point central, et les utilisateurs peuvent faire leur travail sur l’appareil le plus pratique pour eux. Par exemple, vous pouvez écouter la radio sur votre téléphone en voiture, mais une fois à la maison, vous voudrez sans doute transférer la lecture à votre Xbox One qui est raccordée à votre chaîne stéréo.

Vous pouvez également utiliser le projet «Rome» pour les appareils compléments, ou des scénarios de contrôle à distance. Utilisez les API de messagerie d’application pour créer un canal d’application entre deuxappareils afin d’échanger des messages personnalisés. Par exemple, vous pouvez écrire une application pour votre téléphone, qui contrôle la lecture sur votre téléviseur, ou une application complément qui fournit des informations sur les personnages d’une émission de télévision que vous regardez sur une autre application.  

Les appareils peuvent être connectés à proximité par Bluetooth et sans fil, ou à distance par le cloud, à l’aide du compte Microsoft de la personne qui les utilise.

Pour des exemples illustrant comment détecter le système distant, lancer une application sur un système distant et utiliser des services d’application pour échanger des messages entre des applications qui s’exécutent sur deuxsystèmes, consultez la section [Exemple Systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ).

| Activité à distance | Description                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Détecter des appareils distants](discover-remote-devices.md)  | Découvrez comment détecter les appareils auxquels vous pouvez vous connecter. |
| [Lancer une application sur un appareil distant](launch-a-remote-app.md) | Découvrez comment lancer une application sur un appareil distant.  |
| [Communiquer avec un service d’application distant](communicate-with-a-remote-app-service.md) | Découvrez comment interagir avec une application sur un appareil distant. |



<!--HONumber=Aug16_HO5-->


