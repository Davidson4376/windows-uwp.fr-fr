---
author: c-don
title: Déployer une application par le biais de l’inscription de fichiers libres
description: Ce guide vous montre comment utiliser la disposition de fichier isolé de valider et de partager des applications Windows 10 sans avoir à les empaqueter.
ms.author: cdon
ms.date: 6/1/2018
ms.topic: article
keywords: Windows 10, uwp, portail d’appareil, le Gestionnaire d’applications, déploiement, sdk
ms.localizationpriority: medium
ms.openlocfilehash: 16dc7c3d8182e249134be941d466574cddc36157
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7295031"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Déployer une application par le biais de l’inscription de fichiers libres 

Ce guide vous montre comment utiliser la disposition de fichier isolé de valider et de partager des applications Windows 10 sans avoir à les empaqueter. Inscription de dispositions de fichiers libres permet aux développeurs de valider rapidement leurs applications sans avoir besoin de créer un package et installer les applications. 

## <a name="what-is-a-loose-file-layout"></a>Qu’est une disposition de fichier isolé?

Disposition de fichiers libres est simplement le fait de placer le contenu de l’application dans un dossier au lieu de passer par le processus de création de packages. Le contenu du package est «faiblement» disponibles dans un dossier et non compressé. 

> [!WARNING]
> Inscription de disposition de fichiers libres est pour les développeurs et les concepteurs pour rapidement valider leurs applications au cours de développement actif. Cette approche ne doit pas être utilisé pour «dogfood» ou l’application de la version d’évaluation. Nous recommandons que la validation finale être exécutée sur une application empaquetée qui est signée avec un certificat approuvé. 

## <a name="advantages-of-loose-file-registration"></a>Avantages de l’inscription de fichiers libres

- **Validation rapide** - dans la mesure où les fichiers d’application sont déjà installés, les utilisateurs peuvent rapidement inscrire la disposition de fichier isolé et lancer l’application. Comme une application standard, l’utilisateur sera en mesure d’utiliser l’application qu’il a été conçu. 
- **Simplifier la distribution dans l’application réseau** - si les fichiers isolés sont situés dans un partage réseau au lieu d’un lecteur local, les développeurs peuvent envoyer à l’emplacement du partage réseau à d’autres utilisateurs qui ont accès au réseau, et ils peuvent inscrire la disposition de fichier isolé et exécuter l’application. Ainsi, pour plusieurs utilisateurs valider l’application simultanément. 
- **Collaboration** - inscription de fichiers libres permet aux développeurs et aux concepteurs de continuer à travailler sur les ressources visuelles pendant que l’application est inscrite. Les utilisateurs verront ces modifications lors du lancement de l’application. Notez que vous pouvez modifier uniquement les ressources statiques de cette manière. Si vous devez modifier le code ou le contenu créé dynamiquement, vous devez recompiler l’application.

## <a name="how-to-register-a-loose-file-layout"></a>Comment inscrire une disposition de fichiers libres

Windows fournit plusieurs outils de développement pour inscrire les dispositions de fichiers isolés sur des appareils locaux et distants. Vous pouvez choisir parmi `WinDeployAppCmd` (outil de SDK Windows), Windows Device Portal, PowerShell et [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). Vous trouverez ci-dessous, nous allons sur comment enregistrer des fichiers libres à l’aide de ces outils. Mais tout d’abord, assurez-vous d’avoir après l’installation:

- Vos périphériques doivent être sur Windows 10 Creators Update (Build 14965) ou version ultérieure.
- Vous devez activer [le mode développeur](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) et [la découverte d’appareils](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery) sur tous les appareils.

> [!IMPORTANT]
> Inscription de fichiers libres est uniquement disponible sur les appareils qui prennent en charge le protocole de partage réseau (SMB): ordinateurs de bureau et Xbox. 

### <a name="register-with-windeployappcmd"></a>S’inscrire avec WinDeployAppCmd

Si vous utilisez les outils du SDK correspondant à Windows 10 Creators Update (Build 14965) ou version ultérieure, vous pouvez utiliser la `WinDeployAppCmd` commande dans une invite de commandes.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Chemin d’accès réseau** : le chemin d’accès aux fichiers isolés de l’application.

**Adresse IP** : l’adresse IP de l’ordinateur cible.

**code confidentiel de l’ordinateur cible** – un code confidentiel, si nécessaire, pour établir une connexion avec l’appareil cible. Vous sera invité à réessayer avec le `-pin` option si l’authentification est nécessaire. Consultez [La découverte d’appareils](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) pour savoir comment obtenir un code confidentiel.

### <a name="windows-device-portal"></a>WindowsDevicePortal

Windows Device Portal est disponible sur tous les appareils Windows 10 et est utilisée par les développeurs pour tester et valider leur travail. Elle satisfait à tous les publics de la Communauté des développeurs avec son expérience utilisateur du navigateur et les points de terminaison REST. Pour plus d’informations sur Device Portal, consultez la [vue d’ensemble de Windows Device Portal](device-portal.md).

Pour inscrire la disposition de fichier isolé dans Device Portal, procédez comme suit.

1. Se connecter à Device Portal en suivant les étapes décrites dans la section de **configuration** de [vue d’ensemble de Windows Device Portal](device-portal.md).
1. Dans l’onglet Gestionnaire d’applications, sélectionnez **l’inscrire à partir du partage réseau**.
1. Entrez le chemin d’accès du partage réseau à la disposition de fichier isolé. 
1. Si l’appareil hôte n’a pas accès au partage réseau, il sera invité à entrer les informations d’identification requises.
1. Une fois que l’inscription est terminée, vous pouvez lancer l’application.

Sur la page du Gestionnaire d’applications du portail d’appareil, vous pouvez également enregistrer des dispositions de fichiers libres facultative pour votre application principale en sélectionnant la case à cocher **que je veux spécifier les packages facultatifs** et puis en spécifiant les chemins d’accès du partage réseau des applications facultatives. 

### <a name="powershell"></a>PowerShell 

Windows PowerShell vous permet également d’inscrire des dispositions de fichiers libres, mais uniquement sur l’appareil local. Si vous avez besoin d’inscrire une disposition à un appareil distant, vous devez utiliser l’une des autres méthodes. 

Pour inscrire la disposition de fichier isolé, lancer PowerShell et entrez les informations suivantes.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="mapped-network-drives"></a>Lecteurs réseau mappés
Actuellement, les lecteurs réseau mappés ne sont pas pris en charge pour les enregistrements de fichiers isolés. Consultez le lecteur mappé avec full le chemin d’accès du partage réseau.

### <a name="registration-failure"></a>Échec de l’inscription
L’appareil sur lequel l’inscription se déroule devront avoir accès à la disposition de fichier. Si la disposition de fichier est hébergée sur un partage réseau, assurez-vous que l’appareil a accès. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Les modifications apportées à des ressources visuelles ne sont pas en cours de chargement dans l’application 
L’application charge ses ressources visuelles au moment du lancement. Si des modifications ont été apportées pour les ressources visuelles après le lancement de l’application, vous devez relancer l’application pour afficher les dernières modifications apportées.