---
author: v-angraf
title: API REST pour Xbox Device Portal
description: Informations de référence sur les API pour UWP sur XboxOne.
ms.author: v-angraf
ms.date: 10/25/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: 894bc6f657f4a65072056a14171bf86b92cced38
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5548571"
---
# <a name="xbox-device-portal-rest-api"></a>API REST pour Xbox Device Portal

Cette section contient des rubriques de référence pour l’API REST pour Xbox Device Portal, utilisée pour configurer et gérer votre console à distance.

| URI        | Description |
|------------|-------------|
|[/api/app/packagemanager/register](wdp-loose-folder-register-api.md)| Inscrit une application qui est contenue dans un dossier isolé. |
|[/api/app/packagemanager/upload](wdp-folder-upload.md)| Charge un dossier complet sur la console. |
|[/ext/app/sshpins](uwp-sshpins-api.md)| Désactivez tous les codes confidentiels SSH approuvés à distance. Vous devrez refaire le couplage de code PIN pour le développement UWP Visual Studio. |
|[/ext/app/deployinfo](uwp-deployinfo-api.md)| Nécessite des informations de déploiement pour un ou plusieurs packages installés. |
|[/ext/fiddler](wdp-fiddler-api.md)| Activer et désactiver le suivi réseau de Fiddler. |
|[/ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| Obtenir le trafic HTTP à partir de l'application focalisée sur Xbox. |
|[/ext/networkcredential](uwp-networkcredentials-api.md)| Ajouter, supprimer ou mettre à jour les informations d’identification réseau. |
|[/ext/remoteinput](uwp-remoteinput-api.md)| Envoyer à distance l'entrée du clavier, de la souris ou de la manette sur une Xbox. |
|[/ext/remoteinput/controllers](uwp-remoteinput-controllers-api.md)| Obtenir le nombre de contrôleurs physiques connectés ou désactiver tous les contrôleurs physiques. |
|[/ext/screenshot](wdp-media-capture-api.md)| Capture une représentation PNG de l’écran actuellement affiché sur la console. |
|[/ext/settings](wdp-xboxsettings-api.md)| Accède aux paramètres du développeur XboxOne. |
|[/ext/smb/developerfolder](wdp-smb-api.md)| Accède au dossier de développement sur votre console via l’Explorateur de fichiers sur votre ordinateur de développement. |
|[/ext/user](wdp-user-management.md)| Gère les utilisateurs sur la console XboxOne. |
|[/ext/xbox/info](wdp-xboxinfo-api.md)| Donne des informations sur l’appareil Xbox One. |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| Gère votre sandbox XboxLive. |

## <a name="see-also"></a>Voir également

- [Plateforme UWP sur XboxOne](index.md)
- [WindowsDevicePortal](../debug-test-perf/device-portal.md)