---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empaquetage d’applications
description: Cette section contient ou associe des articles sur la création de packages d’application de plateforme Windows universelle (UWP).
ms.date: 07/22/2019
ms.topic: article
keywords: windows 10, uwp, création de package
ms.localizationpriority: medium
ms.openlocfilehash: d067511e4ceb5aaf072aabf8f74ccf186a7787dc
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682674"
---
# <a name="packaging-apps"></a>Empaquetage d’applications

Cette section contient des liens vers des articles sur l’empaquetage d’applications de plateforme Windows universelle (UWP) dans des packages d’applications MSIX et AppX en vue de les déployer et les installer. Certains de ces liens redirigent vers les articles appropriés de la [documentation MSIX](https://docs.microsoft.com/windows/msix/).

> [!NOTE]
> Le format d’empaquetage d’application d’origine pour les applications UWP dans Windows 10 s’appelait AppX. À partir de Windows 10, version 1809, ce format d’empaquetage a été renommé MSIX et a été étendu pour prendre en charge tous les types d’applications Windows, notamment les applications de bureau .NET et C++/Win32. La prise en charge de MSIX est aussi étendue aux versions antérieures de Windows. Pour plus d’informations, consultez la [documentation MSIX](https://docs.microsoft.com/windows/msix/).

| Rubrique | Description |
|-------|-------------|
| [Créer un package d’application UWP avec Visual Studio](/windows/msix/package/packaging-uwp-apps) | Pour distribuer ou vendre votre application UWP (plateforme Windows universelle), vous devez créer un package d’application pour celle-ci. |
| [Création manuelle de package d’application](/windows/msix/package/manual-packaging-root) | Si vous souhaitez créer et signer un package d’application mais que vous n’avez pas utilisé Visual Studio pour développer votre application, vous devez utiliser les outils de création manuelle de package d’application. |
| [Architectures de package d’application](/windows/msix/package/device-architecture) | Découvrez-en plus sur les architectures de processeur à utiliser quand vous générez votre package d’application. |
| [Installation par streaming d’application UWP](/windows/msix/package/streaming-install) | L’installation d’applications en streaming vous permet de spécifier les parties de votre application que vous souhaitez voir téléchargées en premier sur le Microsoft Store. Quand les fichiers essentiels de l’application sont téléchargés en premier, l’utilisateur peut lancer l’application et interagir avec elle pendant que le reste du téléchargement s’achève en arrière-plan. |
| [Création de packages facultatifs et d’ensembles associés](/windows/msix/package/optional-packages) | Les packages facultatifs ont un contenu qui peut être inclus dans un package principal. Ils sont utiles pour le contenu téléchargeable (DLC), pour diviser une application volumineuse en cas de restrictions de taille, ou pour distribuer du contenu supplémentaire indépendamment de votre application d’origine. |
| [Packages facultatifs avec code exécutable](/windows/msix/package/optional-packages-with-executable-code) | Découvrez comment utiliser Visual Studio pour créer un package facultatif avec du code exécutable. |
| [Installer des applications Windows 10 avec le Programme d’installation d’application](/windows/msix/app-installer/app-installer-root) | Le Programme d’installation d’application permet d’installer des applications Windows 10 en double-cliquant sur le package d’application. |
| [Installer des applications avec l’outil WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Le déploiement d’applications Windows s’effectue via WinAppDeployCmd.exe, un outil en ligne de commande qui permet de déployer une application UWP à partir d’une machine Windows 10 vers un appareil Windows 10 Mobile. Vous pouvez utiliser cet outil pour déployer un package d’application quand l’appareil Windows 10 Mobile est connecté via un port USB ou est disponible sur le même sous-réseau, sans avoir besoin de Microsoft Visual Studio ni de la solution liée à cette application. Cet article décrit comment installer des applications UWP à l’aide de cet outil. |
| [Configurer des builds automatisées pour votre application UWP](auto-build-package-uwp-apps.md) | Cette rubrique vous montre comment utiliser Visual Studio Team Services (VSTS) pour empaqueter votre application dans le cadre d’un processus automatisé de génération, le cas échéant. |
| [Déclarations des fonctionnalités d’application](app-capability-declarations.md) | Pour accéder à certaines API, certaines ressources (images ou musique) ou certains appareils (appareil photo ou microphone), les fonctionnalités doivent être déclarées dans le [manifeste du package](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) de votre application. |
| [Télécharger et installer des mises à jour de package à partir du Store](self-install-package-updates.md) | Votre application UWP peut rechercher par programmation les mises à jour de packages et installer les mises à jour. Votre application peut également rechercher les packages marqués comme étant obligatoires dans l’Espace partenaires Windows et désactiver des fonctionnalités jusqu’à ce que la mise à jour obligatoire soit installée.  |
