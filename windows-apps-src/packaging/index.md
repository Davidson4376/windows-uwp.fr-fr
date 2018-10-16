---
author: laurenhughes
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empaquetage d’applications
description: Cette section contient ou associe des articles sur l’empaquetage d’applications de plateforme Windows universelle (UWP).
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, création de packages
ms.localizationpriority: medium
ms.openlocfilehash: ce77391fc189ef33aba3002685b0662d7cab1953
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4622083"
---
# <a name="packaging-apps"></a>Création de packages d’applications


## <a name="purpose"></a>Objectif

Cette section contient ou associe des articles sur l’empaquetage d’applications de plateforme Windows universelle (UWP).

| Rubrique | Description |
|-------|-------------|
| [Créer un package d’application UWP avec Visual Studio](packaging-uwp-apps.md) | Pour distribuer ou vendre votre application de plateforme Windows universelle (UWP), vous devez créer un package d’application pour elle. |
| [Création manuelle de packages d’application](manual-packaging-root.md) | Si vous souhaitez créer et signer un package d’application mais n’avez pas utilisé Visual Studio pour développer votre application, il vous faudra utiliser les outils de création manuelle de packages d’application. |
| [Architectures de package d'application](device-architecture.md) | En savoir plus sur les architectures de processeur que vous devez utiliser lorsque vous générez votre package d’application UWP. |
| [Installation en continu d’une application UWP](streaming-install.md) | Le mode d’installation en continu des applications de la plateforme Windows universelle (UWP) vous permet de spécifier les parties de votre application que vous souhaitez voir téléchargées en premier par le MicrosoftStore. Lorsque les fichiers essentiels de l’application sont téléchargés en priorité, l’utilisateur peut lancer l’application et interagir avec elle pendant que le reste du téléchargement s’achève en arrière-plan. |
| [Packages facultatifs et création d’ensembles connexes](optional-packages.md) | Les packages facultatifs intègrent du contenu qui peut être inclus dans un package principal. Ils sont utiles pour le contenu téléchargeable (DLC), pour diviser une application volumineuse en cas de restrictions de taille, ou pour distribuer un contenu supplémentaire indépendamment de votre application d’origine. |
| [Packages facultatifs avec code exécutable](optional-packages-with-executable-code.md) | Découvrez comment utiliser Visual Studio pour créer un package facultatif avec du code exécutable. |
| [Installer des applications UWP avec le Programme d’installation d’application](appinstaller-root.md) | Le Programme d’installation d’application permet d'installer des applications UWP en double-cliquant sur le package de l’application. |
| [Installer des applications avec l’outil WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Le déploiement d’applications Windows (WinAppDeployCmd.exe) est un outil de ligne de commande qui permet de déployer une application UWP à partir d’un ordinateur Windows10 et vers tout appareil Windows10 Mobile. Vous pouvez utiliser cet outil pour déployer un package d’application lorsque l’appareil Windows 10 Mobile est connecté via un port USB ou disponible sur le même sous-réseau sans avoir besoin de Microsoft Visual Studio ou la solution pour cette application. Cet article décrit comment installer des applications UWP à l’aide de cet outil. |
| [Configuration de builds automatisées pour votre application UWP](auto-build-package-uwp-apps.md) | Cette rubrique vous montre comment utiliser Visual Studio Team Services (VSTS) pour empaqueter votre application dans le cadre d’un processus automatisé de génération, le cas échéant. |
| [Déclarations des fonctionnalités d’application](app-capability-declarations.md) | Pour pouvoir accéder à certaines API, à certaines ressources (images ou musique) ou à certains appareils (appareil photo ou microphone), les fonctionnalités doivent être déclarées dans le [manifeste du package](https://msdn.microsoft.com/library/windows/apps/BR211474) de votre application UWP. |
| [Télécharger et installer des mises à jour de package sur le Store](self-install-package-updates.md) | Votre application UWP peut rechercher par programmation les mises à jour de packages et installer les mises à jour. Votre application peut également rechercher les packages qui ont été marqués comme obligatoires sur le tableau de bord du Centre de développement Windows et désactiver la fonctionnalité jusqu’à ce que la mise à jour obligatoire soit installée.  |
