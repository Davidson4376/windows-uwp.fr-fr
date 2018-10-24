---
author: ridomin
title: Résoudre les problèmes d’installation avec le fichier du programme d'installation d'application
description: Problèmes fréquents en cas de chargement de version test des applications avec le fichier du programme d'installation d'application.
ms.author: rmpablos
ms.date: 5/2/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, programme d’installation de l’application, AppInstaller, charger une version test
ms.localizationpriority: medium
ms.openlocfilehash: e94eb0e819796dda456899bb877057e4532f5ce9
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5475967"
---
# <a name="troubleshoot-installation-issues-with-the-app-installer-file"></a>Résoudre les problèmes d’installation avec le fichier du programme d'installation d'application

SI vous rencontrez des problèmes lors de l'installation d'une application depuis le fichier de programme d'installation d'application, cette rubrique vous aidera part des conseils de résolution des problèmes.

## <a name="prerequisites"></a>Éléments prérequis

Pour être en mesure de charger les applications de manière indépendante dans Windows10, l'appareil de l'utilisateur doit réunir les exigences suivantes:

- L'appareil doit être activé en mode Développeur ou en avec les applications chargées de manière indépendante. Pour plus d’informations, consultez [Activer votre appareil pour le développement](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
- Le certificat utilisé pour signer le package doit être approuvé par l'appareil. Pour plus d’informations, consultez la section **Certificats approuvés** ci-dessus.
- La version de Windows10 doit prendre en charge le schéma de fichier `.appinstaller` et le protocole de distribution.

## <a name="common-issues"></a>Problèmes courants

Certains problèmes courants surviennent lors du chargement de manière indépendante d'une application pour la première fois sur l'ordinateur de l'utilisateur. Les prochaines sections décrivent les problèmes les plus fréquents ainsi que leurs solutions.

### <a name="windows-version"></a>Version de Windows

Chaque publication de Windows10 améliore l'expérience de chargement indépendant. Dans le tableau ci-dessous, vous trouverez les fonctionnalités disponibles pour chaque publication majeure. Si vous tentez de charger de manière indépendante une application à l'aide d'une méthode non prise en charge dans votre version de Windows10, une erreur de déploiement survient.

| Version | Notes de chargement indépendant |
|---------|----------------|
| Build17134 (Mise à jour d'avril2018, version1804)    | Le fichier `.appinstaller` est accessible via les dossiers UNC/de partage. Les vérifications de mise à jour configurable sont également disponibles. |
| Build16299 (Mise à jour Créateurs en automne, version1709) | Le fichier `.appinstaller` a été introduit pour fournir des mises à jour automatiques à votre application. Cette version prend uniquement en charge les points de terminaison HTTP. Les vérifications de mise à jour ne sont pas configurables, elles se produisent toutes les 24heures. |
| Build15063 (Mise à jour Créateurs, version1703)      | L'application App Installer est en mesure de télécharger les dépendances d'application (uniquement en mode publication) depuis le Store. |
| Build14393 (mise à jour anniversaire, version1607)   | L'application App Installer a été introduite pour installer les fichiers .appx et .appxbundle, le fichier .appinstaller n'est pas pris en charge. |
| Build10586 (mise à jour de novembre, version1511)      | Le chargement indépendant est uniquement disponible via PowerShell à l'aide de la commande [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps). |
| Build10240 (Windows10, version1507)           | Le chargement indépendant est uniquement disponible via PowerShell à l'aide de la commande [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps). |

### <a name="trusted-certificates"></a>Certificats approuvés

Le package d'application doit être signé avec un certificat approuvé par l'appareil. Les certificats fournis par les Autorités de certificat sont approuvés par défaut dans le système d'exploitation Windows. Néanmoins, si le certificat n'est pas approuvé, il doit être installé dans l'appareil **avant** l'installation de l'application. Pour approuver le certificat, celui-ci doit être présent dans l'un des magasins de certificats d'ordinateur local suivants sur votre appareil:

- Éditeurs approuvés
- Personnes autorisées
- Autorités racines approuvées (non recommandé)

 >[!IMPORTANT]
 > L'installation d'un certificat dans le store Ordinateur local nécessite l'accès administratif.

### <a name="dependencies-not-installed"></a>Dépendances non installées. 

Les applications UWP peuvent disposer de dépendances d'infrastructure selon la plateforme d'application utilisée pour générer l'application. Si vous utilisez C# ou VB, l’application nécessite l'exécution .NET et les packages d'infrastructure .NET. Les applications C++ nécessite l'élément VCLibs.

>[!IMPORTANT] 
> Si le package d'application est généré dans une configuration en mode Publication, les dépendances de l'infrastructure seront obtenues depuis le MicrosoftStore. Néanmoins, si l'application est générée dans une configuration en mode Débogage, les dépendances seront obtenues depuis l'emplacement spécifié dans le fichier `.appinstaller`.

### <a name="files-not-accessible"></a>Fichiers non accessibles

Lors de l'installation à partir d'un point de terminaison HTTP, il est important de vérifier que tous les fichiers sont accessibles avec le type de MIME adéquat. La méthode la plus simple de vérifier ces fichiers consiste à suivre les liens fournis à la page HTML générée par VisualStudio. Vous devez vérifier ces fichiers:

- `.appinstaller` fichier, disponible en tant que `application/xml`
- `.appx` et fichiers `.appxbundle`, disponibles en tant que `application/vns.ms-appx`

## <a name="isolate-app-installer-app-issues"></a>Problèmes isolés de l'application App Installer

Si l'application App Installer ne peut pas installer l'application ces étapes permettent d'identifier le problème d'installation.

### <a name="verify-app-package-file-installation"></a>Vérifier l’installation de fichier de package app

- Téléchargez le fichier de package d’application dans un dossier local et essayez d’installer à l’aide de la commande PowerShell [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) .

- Téléchargez le fichier `.appinstaller` dans un dossier local et tentez de l'installer à l'aide de la commande PowerShell `Add-AppxPackage -Appinstaller`.

## <a name="related-logs"></a>Journaux associés

L'infrastructure de déploiement d'application fournit des journaux destinés au débogages dans l'observateur d'événements Windows. Ces journaux sont disponibles ici: `Application and Services Logs->Microsoft->Windows->AppxDeployment-Server`



