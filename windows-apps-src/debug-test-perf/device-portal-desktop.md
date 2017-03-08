---
author: mcleblanc
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Device Portal pour Bureau
description: "Découvrez comment Windows Device Portal ouvre les diagnostics et l’automatisation sur votre bureau Windows."
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7b8b396078d59cc2ab3180e9af8b6017fd5edbda
ms.lasthandoff: 02/07/2017

---
# <a name="device-portal-for-desktop"></a>Device Portal pour le bureau

À partir de Windows 10 version 1607, des fonctionnalités de développement supplémentaires sont disponibles pour le bureau. Ces fonctionnalités sont disponibles uniquement lorsque le mode développeur est activé.

Pour en savoir plus sur la façon d’activer le mode développeur, consultez [Activer votre appareil pour le développement](../get-started/enable-your-device-for-development.md).

Device Portal vous permet d’afficher les informations de diagnostic et d’interagir avec votre bureau sur HTTP à l’aide de votre navigateur. Vous pouvez utiliser Device Portal pour :
- voir et manipuler une liste de processus en cours d’exécution ;
- installer, supprimer, démarrer et arrêter des applications ;
- modifier les profils de connexion Wi-Fi, afficher la force du signal et voir ipconfig ;
- afficher les graphiques d’utilisation du processeur, de la mémoire, des E/S, du réseau et du GPU en temps réel ;
- collecter les vidages de processus ;
- collecter les traces ETW ; 
- manipuler le stockage isolé des applications de version test chargée.

## <a name="set-up-device-portal-on-windows-desktop"></a>Configurer Device Portal sur le bureau Windows

### <a name="turn-on-device-portal"></a>Activer Device Portal

Dans le menu **Paramètres développeur**, si le Mode développeur est activé, vous pouvez activer Device Portal.  

Lorsque vous activez Device Portal, vous devez également créer un nom d’utilisateur et un mot de passe pour Device Portal. N’utilisez pas votre compte Microsoft ou d’autres informations d’identification Windows.  

Une fois Device Portal activé, vous verrez des liens permettant d’y accéder en bas de la section **Paramètres**. Prenez note du numéro de port apparaissant à la fin de l’URL : ce numéro de port est généré de manière aléatoire lorsque Device Portal est activé, mais il doit rester cohérent entre deux redémarrages du bureau. Si vous souhaitez définir les numéros de port manuellement afin qu’ils restent permanents, consultez [Configuration des numéros de port](device-portal-desktop.md#setting-port-numbers).

Vous pouvez choisir entre deux moyens de connexion à Device Portal : hôte local et sur le réseau local (y compris le VPN).

**Pour se connecter à Device Portal**

1. Dans votre navigateur, entrez l’adresse indiquée ici selon le type de connexion que vous utilisez.

    - Hôte local : `http://127.0.0.1:PORT` ou `http://localhost:PORT`

    Utilisez cette adresse pour afficher Device Portal localement.
    
    - Réseau local : `https://<The IP address of the desktop>:PORT`

    Utilisez cette adresse pour établir la connexion par le biais d’un réseau local.

Une connexion HTTPS est requise pour l’authentification et la communication sécurisée.

Si vous utilisez Device Portal dans un environnement protégé, comme un laboratoire de test, où vous faites confiance à tous les utilisateurs du réseau local, si aucune information personnelle n’est présente sur votre appareil et si vous présentez des exigences uniques, vous pouvez désactiver l’authentification. Cela permet une communication non chiffrée. Toute personne connaissant l’adresse IP de votre ordinateur pourra le contrôler.

## <a name="device-portal-pages"></a>Pages Device Portal

Sur le Bureau, Device Portal propose les pages standard. Pour obtenir une description détaillée, voir [Vue d’ensemble de Windows Device Portal](device-portal.md).

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

Si vous souhaitez sélectionner des numéros de port pour Device Portal (par exemple, 80 et 443), vous pouvez définir les clés de Registre suivantes :

- Sous HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service
    - Utiliser des ports dynamiques : un DWORD requis. Définissez ce paramètre sur 0 pour conserver les numéros de port que vous avez choisis.
    - Port Http : un DWORD requis. Contient le numéro de port que Device Portal va écouter pour les connexions HTTP.  
    - Port Https : un DWORD requis. Contient le numéro de port que Device Portal va écouter pour les connexions HTTPS.

## <a name="failure-to-install-developer-mode-package-or-launch-device-portal"></a>Échec de l’installation du package Mode développeur ou du lancement de Device Portal
Parfois, en raison de problèmes réseau ou de compatibilité, le Mode développeur ne s’installe pas correctement. Le package en mode développeur est nécessaire pour le déploiement **à distance** (Device Portal et SSH), mais pas pour le développement local.  Même si vous rencontrez ces problèmes, vous pouvez toujours déployer votre application localement à l’aide de Visual Studio. 

Voir le forum [Problèmes connus](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) pour rechercher des solutions de contournement à ces problèmes et bien plus encore. 

### <a name="failed-to-locate-the-package"></a>Échec de la localisation du package

« Le package Mode développeur n’a pas pu être localisé dans la mise à jour Windows. Code d’erreur 0x001234. En savoir plus »   

Cette erreur peut se produire en raison d’un problème de connectivité réseau, des paramètres d’Entreprise ou d’un package manquant. 

Pour résoudre ce problème :

1. Assurez-vous que votre ordinateur est connecté à Internet. 
2. Si vous utilisez un ordinateur appartenant à un domaine, adressez-vous à votre administrateur réseau. 
3. Recherchez les mises à jour de Windows dans Paramètres &gt; Mises à jour et sécurité &gt; Mises à jour Windows.
4. Vérifiez que le package Mode développeur Windows est présent dans Paramètres &gt; Système &gt; Applications et fonctionnalités &gt; Gérer les fonctionnalités facultatives &gt; Ajouter une fonctionnalité. S’il n’est pas présent, Windows ne peut pas trouver le package approprié pour votre ordinateur. 

Après avoir suivi les étapes ci-dessus, désactivez puis réactivez le Mode développeur pour vérifier le correctif. 


### <a name="failed-to-install-the-package"></a>Échec de l’installation du package

« Échec d’installation du package Mode développeur. Code d’erreur 0x001234  En savoir plus »

Cette erreur peut se produire en raison d’incompatibilités entre votre version de Windows et le package Mode développeur. 

Pour résoudre ce problème :

1. Recherchez les mises à jour de Windows dans Paramètres &gt; Mises à jour et sécurité &gt; Mises à jour Windows.
2. Redémarrez votre ordinateur pour vérifier que toutes les mises à jour sont appliquées.

