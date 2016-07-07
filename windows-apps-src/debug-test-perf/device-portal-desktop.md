---
author: mcleblanc
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Device Portal pour Bureau
description: "Découvrez comment WindowsDevicePortal ouvre les diagnostics et l’automatisation sur votre bureau Windows."
ms.sourcegitcommit: f09f0233ec11b41989cf52da3c5e8cb37a97b607
ms.openlocfilehash: 7be27f5fb15676c5330f22995dd044899eddfd3d

---
# DevicePortal pour Bureau

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

## Configurer DevicePortal sur le bureau Windows

### Activer DevicePortal

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

## Pages DevicePortal

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

## Définition des numéros de port

Si vous souhaitez sélectionner des numéros de port pour DevicePortal (par exemple, 80 et 443), vous pouvez définir les clés de Registre suivantes:

- Sous HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service
    - Utiliser des ports dynamiques: un DWORD requis. Définissez ce paramètre sur0 pour conserver les numéros de port que vous avez choisis.
    - Port Http: un DWORD requis. Contient le numéro de port que DevicePortal va écouter pour les connexions HTTP.  
    - Port Https: un DWORD requis. Contient le numéro de port que Device Portal va écouter pour les connexions HTTPS.

## Échec d’installation du package Mode développeur
Parfois, en raison de problèmes réseau ou de compatibilité, le Mode développeur ne s’installe pas correctement. Le package Mode développeur est requis pour le déploiement à distance (DevicePortal et SSH) mais pas pour le développement local.  

### Échec de localisation du package

«Le package Mode développeur n’a pas pu être localisé dans la mise à jour Windows. Code d’erreur 0x001234. En savoir plus»   

Cette erreur peut se produire en raison d’un problème de connectivité réseau, des paramètres d’Entreprise ou d’un package manquant. 

Pour résoudre ce problème:

1. Assurez-vous que votre ordinateur est connecté à Internet. 
2. Si vous utilisez un ordinateur appartenant à un domaine, adressez-vous à votre administrateur réseau. 
3. Recherchez les mises à jour de Windows dans Paramètres &gt; Mises à jour et sécurité &gt; [Mises à jour Windows](ms-settings:windowsupdate).
4. Vérifiez que le package Mode développeur Windows est présent dans Paramètres &gt; Système &gt; Applications et fonctionnalités &gt; [Gérer les fonctionnalités facultatives](ms-settings:optionalfeatures) &gt; Ajouter une fonctionnalité. S’il n’est pas présent, Windows ne peut pas trouver le package approprié pour votre ordinateur. 

Après avoir suivi les étapes ci-dessus, désactivez puis réactivez le Mode développeur pour vérifier le correctif. 


### Échec de l’installation du package

«Échec d’installation du package Mode développeur. Code d’erreur 0x001234. En savoir plus»

Cette erreur peut se produire en raison d’incompatibilités entre votre version de Windows et le package Mode développeur. 

Pour résoudre ce problème:

1. Recherchez les mises à jour de Windows dans Paramètres &gt; Mises à jour et sécurité &gt; [Mises à jour Windows](ms-settings:windowsupdate).
2. Redémarrez votre ordinateur pour vérifier que toutes les mises à jour sont appliquées.



<!--HONumber=Jun16_HO5-->


