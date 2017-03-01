---
author: laurenhughes
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: "Empaquetage d’applications"
description: "Cette section contient ou associe des articles sur l’empaquetage d’applications de plateforme Windows universelle (UWP)."
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0a9689738fac363012fb9af197f52ac8813b47c9
ms.lasthandoff: 02/07/2017

---
# <a name="packaging-apps"></a>Empaquetage d’applications

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## <a name="purpose"></a>Objectif

Cette section contient ou associe des articles sur l’empaquetage d’applications de plateforme Windows universelle (UWP).

| Rubrique | Description |
|-------|-------------|
| [Création de packages d’application UWP](packaging-uwp-apps.md) | Pour vendre ou distribuer votre application UWP à d’autres utilisateurs, vous devez créer un package d’application appxupload. Lorsque vous créez l’appxupload, un autre package appx est alors généré pour le test et le chargement indépendant. Vous pouvez distribuer votre application directement en chargeant de manière indépendante le package appx sur un appareil. Cet article décrit le processus de configuration, de création et de test d’un package d’application UWP. Pour plus d’informations sur le chargement indépendant, consultez [Charger des versions test d’applications avec DISM](http://go.microsoft.com/fwlink/?LinkID=231020). |
| [Créer un package d’application avec l’outil MakeAppx.exe](create-app-package-with-makeappx-tool.md) | MakeAppx.exe crée, chiffre, déchiffre et extrait les fichiers des packages d’application et des ensembles d’applications. |
| [Créer un certificat pour la signature de packages](create-certificate-package-signing.md) | Créer et exporter un certificat pour la signature de packages d’applications à l’aide d’outils PowerShell. |
| [Signer un package d’application à l’aide de SignTool](sign-app-package-using-signtool.md) | Utilisez SignTool pour signer manuellement un package d’application à l’aide d’un certificat. |
| [Installer des applications avec l’outil WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Le déploiement d’applications Windows (WinAppDeployCmd.exe) est un outil de ligne de commande qui permet de déployer une application UWP à partir d’un ordinateur Windows 10 et vers tout appareil Windows 10 Mobile. Vous pouvez utiliser cet outil pour déployer un package .appx lorsque l’appareil Windows 10 Mobile est connecté via un port USB ou disponible sur le même sous-réseau, sans avoir besoin de Microsoft Visual Studio ni de la solution pour cette application. Cet article décrit comment installer des applications UWP à l’aide de cet outil. |
| [Configuration de builds automatisées pour votre application UWP](auto-build-package-uwp-apps.md) | Cette rubrique vous montre comment utiliser Visual Studio Team Services (VSTS) pour empaqueter votre application dans le cadre d’un processus automatisé de génération, le cas échéant. |
| [Déclarations des fonctionnalités d’application](app-capability-declarations.md) | Pour pouvoir accéder à certaines API, à certaines ressources (images ou musique) ou à certains appareils (appareil photo ou microphone), les fonctionnalités doivent être déclarées dans le [manifeste du package](https://msdn.microsoft.com/library/windows/apps/BR211474) de votre application UWP. |
| [Télécharger et installer des mises à jour de package pour votre application](self-install-package-updates.md) | Votre application UWP peut rechercher par programmation les mises à jour de packages et installer les mises à jour. Votre application peut également rechercher les packages qui ont été marqués comme obligatoires sur le tableau de bord du Centre de développement Windows et désactiver la fonctionnalité jusqu’à ce que la mise à jour obligatoire soit installée. Cet article décrit comment effectuer ces tâches. |
 

