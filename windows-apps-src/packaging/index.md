---
author: msatranjr
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: "Empaquetage d’applications"
description: "Cette section contient ou associe des articles sur la création de packages d’application de plateforme Windows universelle (UWP)."
translationtype: Human Translation
ms.sourcegitcommit: f7eec4d8df2b62143c198fe507dffd936e2e325c
ms.openlocfilehash: 230c77cbe60631d643bebe905a92cdbeb76af4d2

---
# Création de packages d’application

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Objectif

Cette section contient ou associe des articles sur l’empaquetage d’applications de plateforme Windows universelle (UWP).

| Rubrique | Description |
|-------|-------------|
| [Création de packages d’application UWP](packaging-uwp-apps.md) | Pour vendre ou distribuer votre application UWP à d’autres utilisateurs, vous devez créer un package d’application appxupload. Lorsque vous créez l’appxupload, un autre package appx est alors généré pour le test et le chargement indépendant. Vous pouvez distribuer votre application directement en chargeant de manière indépendante le package appx sur un appareil. Cet article décrit le processus de configuration, de création et de test d’un package d’application UWP. Pour plus d’informations sur le chargement indépendant, voir [Charger de manière indépendante des applications avec DISM](http://go.microsoft.com/fwlink/?LinkID=231020). |
| [Installer des applications avec l’outil WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Le déploiement d’applications Windows (WinAppDeployCmd.exe) est un outil de ligne de commande qui permet de déployer une application UWP à partir d’un ordinateur Windows 10 et vers tout appareil Windows 10 Mobile. Vous pouvez utiliser cet outil pour déployer un package .appx lorsque l’appareil Windows 10 Mobile est connecté via un port USB ou disponible sur le même sous-réseau, sans avoir besoin de Microsoft Visual Studio ni de la solution pour cette application. Cet article décrit comment installer des applications UWP à l’aide de cet outil. |
| [Déclarations des fonctionnalités d’application](app-capability-declarations.md) | Pour pouvoir accéder à certaines API, à certaines ressources (images ou musique) ou à certains appareils (appareil photo ou microphone), les fonctionnalités doivent être déclarées dans le [manifeste du package](https://msdn.microsoft.com/library/windows/apps/BR211474) de votre application UWP. |
| [Télécharger et installer des mises à jour de package pour votre application](self-install-package-updates.md) | Votre application UWP peut rechercher par programmation les mises à jour de packages et installer les mises à jour. Votre application peut également rechercher les packages qui ont été marqués comme obligatoires sur le tableau de bord du Centre de développement Windows et désactiver la fonctionnalité jusqu’à ce que la mise à jour obligatoire soit installée. Cet article décrit comment effectuer ces tâches. |
 



<!--HONumber=Aug16_HO5-->


