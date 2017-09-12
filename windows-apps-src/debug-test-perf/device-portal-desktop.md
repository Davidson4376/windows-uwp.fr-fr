---
author: PatrickFarley
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Device Portal pour Bureau
description: "Découvrez comment WindowsDevicePortal ouvre les diagnostics et l’automatisation sur votre bureau Windows."
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 32155bfbb676a5f79dd4b1629f0a88368da36828
ms.sourcegitcommit: 0fa9ae00117e8e6b04ed38956e605bb74c1261c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2017
---
# <a name="device-portal-for-desktop"></a>DevicePortal pour le bureau

À partir de Windows10 version1607, des fonctionnalités de développement supplémentaires sont disponibles pour le bureau. Ces fonctionnalités sont disponibles uniquement lorsque le mode développeur est activé.

Pour en savoir plus sur la façon d’activer le mode développeur, consultez [Activer votre appareil pour le développement](../get-started/enable-your-device-for-development.md).

DevicePortal vous permet d’afficher les informations de diagnostic et d’interagir avec votre bureau surHTTP à l’aide de votre navigateur. Vous pouvez utiliser DevicePortal pour:
- voir et manipuler une liste de processus en cours d’exécution;
- installer, supprimer, démarrer et arrêter des applications;
- modifier les profils de connexion Wi-Fi, afficher la force du signal et voir ipconfig;
- afficher les graphiques d’utilisation du processeur, de la mémoire, des E/S, du réseau et du GPU en temps réel;
- collecter les vidages de processus;
- collecter les traces ETW; 
- manipuler le stockage isolé des applications de version test chargée.

## <a name="set-up-device-portal-on-windows-desktop"></a>Configurer DevicePortal sur le bureau Windows

### <a name="turn-on-device-portal"></a>Activer DevicePortal

Dans le menu **Paramètres développeur**, si le Mode développeur est activé, vous pouvez activer DevicePortal.  

Lorsque vous activez DevicePortal, vous devez également créer un nom d’utilisateur et un mot de passe pour DevicePortal. N’utilisez pas votre compte Microsoft ou d’autres informations d’identification Windows.  

Une fois DevicePortal activé, vous verrez des liens permettant d’y accéder en bas de la section **Paramètres**. Prenez note du numéro de port apparaissant à la fin de l’URL: ce numéro de port est généré de manière aléatoire lorsque DevicePortal est activé, mais il doit rester cohérent entre deux redémarrages du bureau. Si vous souhaitez définir les numéros de port manuellement afin qu’ils restent permanents, consultez [Configuration des numéros de port](device-portal-desktop.md#setting-port-numbers).

Vous pouvez choisir entre deux moyens de connexion à DevicePortal: hôte local et sur le réseau local (y compris le VPN).

**Pour se connecter à Device Portal**

1. Dans votre navigateur, entrez l’adresse indiquée ici selon le type de connexion que vous utilisez.

    - Hôte local : `http://127.0.0.1:PORT` ou `http://localhost:PORT`

    Utilisez cette adresse pour afficher Device Portal localement.
    
    - Réseau local: `https://<The IP address of the desktop>:PORT`

    Utilisez cette adresse pour établir la connexion par le biais d’un réseau local.

Une connexion HTTPS est requise pour l’authentification et la communication sécurisée.

Si vous utilisez Device Portal dans un environnement protégé, comme un laboratoire de test, où vous faites confiance à tous les utilisateurs du réseau local, si aucune information personnelle n’est présente sur votre appareil et si vous présentez des exigences uniques, vous pouvez désactiver l’authentification. Cela permet une communication non chiffrée. Toute personne connaissant l’adresseIP de votre ordinateur pourra le contrôler.

## <a name="device-portal-pages"></a>Pages DevicePortal

Sur le Bureau, DevicePortal propose les pages standard. Pour obtenir une description détaillée, voir [Vue d’ensemble de WindowsDevicePortal](device-portal.md).

- Applications
- Processus
- Performances
- Débogage
- Suivi des événements pour Windows (ETW)
- Suivi des performances
- Appareils
- Réseau
- Explorateur de fichiers de l’application 

## <a name="setting-port-numbers"></a>Définition des numéros de port

Si vous souhaitez sélectionner des numéros de port pour DevicePortal (par exemple, 80 et 443), vous pouvez définir les clés de Registre suivantes:

- Sous HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service
    - Utiliser des ports dynamiques: un DWORD requis. Définissez ce paramètre sur0 pour conserver les numéros de port que vous avez choisis.
    - Port Http: un DWORD requis. Contient le numéro de port que DevicePortal va écouter pour les connexions HTTP.  
    - Port Https: un DWORD requis. Contient le numéro de port que Device Portal va écouter pour les connexions HTTPS.

## <a name="failure-to-install-developer-mode-package"></a>Échec de l’installation du package Mode développeur
Parfois, en raison de problèmes réseau ou de compatibilité, le Mode développeur ne s’installe pas correctement. Le package Mode développeur est nécessaire pour un déploiement **à distance** sur le PC - à l’aide de Device Portal depuis un navigateur ou de la fonction Découverte d’appareils pour activer SSH--mais pas pour un développement local.  Même si vous rencontrez ces problèmes, vous pouvez toujours déployer votre application localement à l’aide de VisualStudio. 

Voir le forum [Problèmes connus](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) et la [page sur le Mode développeur](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package) pour rechercher des solutions de contournement à ces problèmes et bien plus encore. 

