---
title: Déployer une application par le biais de l’inscription de fichiers libres
description: Ce guide montre comment utiliser la disposition de fichiers libres pour valider et partager des applications Windows 10 sans avoir à les empaqueter.
ms.date: 6/1/2018
ms.topic: article
keywords: Windows 10, uwp, portail de l’appareil, le Gestionnaire d’applications, déploiement, Kit de développement logiciel
ms.localizationpriority: medium
ms.openlocfilehash: 928c07bd23228f0fefd78be6019a0d116b2e6e4b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635424"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Déployer une application par le biais de l’inscription de fichiers libres 

Ce guide montre comment utiliser la disposition de fichiers libres pour valider et partager des applications Windows 10 sans avoir à les empaqueter. L’inscription de dispositions de fichier libre permet aux développeurs de valider rapidement leurs applications sans avoir à empaqueter et installer les applications. 

## <a name="what-is-a-loose-file-layout"></a>Qu’est une disposition libre de fichier ?

Disposition de fichier libre est simplement le fait de placer le contenu de l’application dans un dossier au lieu de passer par le processus d’empaquetage. Le contenu du package est « faiblement » disponibles dans un dossier et non compressé. 

> [!WARNING]
> L’inscription de mise en page de fichier libre est pour les développeurs et concepteurs valider rapidement leurs applications pendant le développement actif. Cette approche ne doit pas être utilisé pour « dogfood » ou l’application de vol. Nous recommandons que la validation finale exécutée sur une application empaquetée est signée avec un certificat approuvé. 

## <a name="advantages-of-loose-file-registration"></a>Avantages de l’enregistrement du fichier libre

- **Validation rapide** -étant donné que les fichiers d’application sont déjà décompressés, les utilisateurs peuvent rapidement s’inscrire à la disposition de fichier libre et lancer l’application. Tout comme une application standard, l’utilisateur sera en mesure d’utiliser l’application tel qu’il a été créé. 
- **Faciliter la distribution de réseau** -si les fichiers libres sont situés dans un partage réseau au lieu d’un lecteur local, les développeurs peuvent envoyer à l’emplacement du partage réseau à d’autres utilisateurs qui ont accès au réseau, et ils peuvent s’inscrire à la disposition de fichier libre et exécuter le application. Cela permet à plusieurs utilisateurs valider l’application simultanément. 
- **Collaboration** -l’enregistrement du fichier libre permet aux développeurs et concepteurs de continuer à travailler sur les ressources visuelles alors que l’application est enregistrée. Les utilisateurs verront ces modifications lors du lancement de l’application. Notez que vous pouvez modifier uniquement les ressources statiques de cette manière. Si vous devez modifier tout code ou créé dynamiquement le contenu, vous devez recompiler l’application.

## <a name="how-to-register-a-loose-file-layout"></a>Comment inscrire une disposition libre de fichier

Windows fournit plusieurs outils de développement pour enregistrer des dispositions de fichier libre sur les appareils locaux et distants. Vous pouvez choisir parmi `WinDeployAppCmd` (outil de kit de développement logiciel Windows), Windows Device Portal, PowerShell, et [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). Ci-dessous nous allons vous présenter comment enregistrer les fichiers libres à l’aide de ces outils. Mais tout d’abord, assurez-vous d’avoir après l’installation :

- Vos appareils doivent être sur Windows 10 Creators Update (Build 14965) ou version ultérieure.
- Vous devez activer [mode développeur](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) et [découverte](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery) sur tous les appareils.

> [!IMPORTANT]
> L’enregistrement du fichier libre est disponible uniquement sur les appareils qui prennent en charge le protocole de partage réseau (SMB) : Bureau et Xbox. 

### <a name="register-with-windeployappcmd"></a>Inscrire avec WinDeployAppCmd

Si vous utilisez les outils du Kit de développement logiciel correspondant à Windows 10 Creators Update (Build 14965) ou version ultérieure, vous pouvez utiliser la `WinDeployAppCmd` commande dans une invite de commandes.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Chemin d’accès réseau** : le chemin d’accès aux fichiers libres de l’application.

**Adresse IP** : l’adresse IP de l’ordinateur cible.

**code PIN de l’ordinateur cible** – un code confidentiel, si nécessaire, pour établir une connexion avec l’appareil cible. Vous êtes invité à réessayer avec le `-pin` option si l’authentification est requise. Consultez [découverte](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) pour savoir comment obtenir un code confidentiel.

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal est disponible sur tous les appareils Windows 10 et est utilisé par les développeurs pour tester et valider leur travail. Il répond à tous les publics de la Communauté des développeurs avec son navigateur l’expérience utilisateur et les points de terminaison REST. Pour plus d’informations sur le portail de l’appareil, consultez le [vue d’ensemble de Windows Device Portal](device-portal.md).

Pour enregistrer la disposition de fichier libre dans le portail de l’appareil, procédez comme suit.

1. Connectez-vous au portail de l’appareil en suivant les étapes décrites dans le **le programme d’installation** section de la [vue d’ensemble de Windows Device Portal](device-portal.md).
1. Dans l’onglet Gestionnaire d’applications, sélectionnez **enregistrer à partir du partage réseau**.
1. Entrez le chemin d’accès du partage réseau à la disposition de fichier libre. 
1. Si l’appareil de l’hôte n’a pas accès au partage réseau, il sera invité à entrer les informations d’identification requises.
1. Une fois l’inscription terminée, vous pouvez lancer l’application.

Sur la page du Gestionnaire d’applications du portail de l’appareil, vous pouvez également enregistrer des dispositions de fichier libre facultatif pour votre application principale en sélectionnant le **je souhaite spécifier les packages facultatifs** case à cocher, puis en spécifiant le réseau de partagent des chemins d’accès des applications facultatives . 

### <a name="powershell"></a>PowerShell 

Windows PowerShell vous permet également à enregistrer des dispositions de fichier libre, mais uniquement sur l’appareil local. Si vous avez besoin inscrire une disposition à un périphérique distant, vous devez utiliser une des autres méthodes. 

Pour enregistrer la disposition de fichier libre, lancez PowerShell, puis entrez les informations suivantes.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="mapped-network-drives"></a>Lecteurs réseau mappés
Actuellement, les lecteurs réseau mappés ne sont pas pris en charge pour les enregistrements de fichier libre. Consultez le lecteur mappé avec intégral le chemin d’accès du partage réseau.

### <a name="registration-failure"></a>Échec de l’inscription
L’appareil sur lequel l’inscription se déroule devez avoir accès à la disposition de fichier. Si la disposition de fichier est hébergée sur un partage réseau, vérifiez que l’appareil a accès. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Modifications apportées aux éléments visuels ne sont pas en cours de chargement dans l’application 
L’application charge ses ressources visuelles au moment du lancement. Si des modifications ont été apportées pour les ressources visuelles après le lancement de l’application, vous devez relancer l’application pour afficher les dernières modifications.