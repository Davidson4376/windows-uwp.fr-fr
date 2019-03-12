---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empaquetage d’applications
description: Cette section contient ou associe des articles sur la création de packages d’application de plateforme Windows universelle (UWP).
ms.date: 09/30/2018
ms.topic: article
keywords: "windows\_10, uwp, création de package"
ms.localizationpriority: medium
---
# <a name="packaging-apps"></a>Empaquetage d’applications


## <a name="purpose"></a>Objectif

Cette section contient ou associe des articles sur la création de packages d’application de plateforme Windows universelle (UWP).

| Rubrique | Description |
|-------|-------------|
| [Créer un package d’application UWP avec Visual Studio](packaging-uwp-apps.md) | Pour distribuer ou vendre votre application UWP (plateforme Windows universelle), vous devez créer un package d’application pour celle-ci. |
| [Création manuelle de package d’application](manual-packaging-root.md) | Si vous souhaitez créer et signer un package d’application mais que vous n’avez pas utilisé Visual Studio pour développer votre application, vous devez utiliser les outils de création manuelle de package d’application. |
| [Architectures de package d’application](device-architecture.md) | En savoir plus sur les architectures de processeur à utiliser quand vous générez votre package d’application UWP. |
| [Installation par streaming d’application UWP](streaming-install.md) | L’installation par streaming d’application UWP vous permet de spécifier les parties de votre application que vous souhaitez voir téléchargées en premier sur le Microsoft Store. Quand les fichiers essentiels de l’application sont téléchargés en premier, l’utilisateur peut lancer l’application et interagir avec elle pendant que le reste du téléchargement s’achève en arrière-plan. |
| [Création de packages facultatifs et d’ensembles associés](optional-packages.md) | Les packages facultatifs ont un contenu qui peut être inclus dans un package principal. Ils sont utiles pour le contenu téléchargeable (DLC), pour diviser une application volumineuse en cas de restrictions de taille, ou pour distribuer du contenu supplémentaire indépendamment de votre application d’origine. |
| [Packages facultatifs avec code exécutable](optional-packages-with-executable-code.md) | Découvrez comment utiliser Visual Studio pour créer un package facultatif avec du code exécutable. |
| [Installer des applications UWP avec le Programme d’installation d’application](appinstaller-root.md) | Le Programme d’installation d’application permet d’installer des applications UWP en double-cliquant sur le package d’application. |
| [Installer des applications avec l’outil WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Le déploiement d’applications Windows s’effectue via WinAppDeployCmd.exe, un outil en ligne de commande qui permet de déployer une application UWP à partir d’une machine Windows 10 vers un appareil Windows 10 Mobile. Vous pouvez utiliser cet outil pour déployer un package d’application quand l’appareil Windows 10 Mobile est connecté via un port USB ou est disponible sur le même sous-réseau, sans avoir besoin de Microsoft Visual Studio ni de la solution liée à cette application. Cet article décrit comment installer des applications UWP à l’aide de cet outil. |
| [Configurer des builds automatisées pour votre application UWP](auto-build-package-uwp-apps.md) | Cette rubrique vous montre comment utiliser Visual Studio Team Services (VSTS) pour empaqueter votre application dans le cadre d’un processus automatisé de génération, le cas échéant. |
| [Déclarations des fonctionnalités d’application](app-capability-declarations.md) | Pour pouvoir accéder à certaines API, à certaines ressources (images ou musique) ou à certains appareils (appareil photo ou microphone), les fonctionnalités doivent être déclarées dans le [manifeste du package](https://msdn.microsoft.com/library/windows/apps/BR211474) de votre application UWP. |
| [Télécharger et installer des mises à jour de package à partir du Store](self-install-package-updates.md) | Votre application UWP peut rechercher par programmation les mises à jour de packages et installer les mises à jour. Votre application peut également rechercher les packages marqués comme étant obligatoires dans l’Espace partenaires Windows et désactiver des fonctionnalités jusqu’à ce que la mise à jour obligatoire soit installée.  |
