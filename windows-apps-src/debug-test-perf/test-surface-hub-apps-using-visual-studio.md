---
author: PatrickFarley
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Tester les applications de Surface Hub à l’aide de VisualStudio
description: Le simulateur de VisualStudio fournit un environnement permettant de concevoir, développer, déboguer et tester des applications UWP, y compris les applications générées pour SurfaceHub.
ms.author: pafarley
ms.date: 10/26/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 63214ce47bffc5a0b13f421e5185d06cd810ea34
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7434476"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Tester les applications de Surface Hub à l’aide de Visual Studio
Le simulateur de Visual Studio fournit un environnement dans lequel vous pouvez concevoir, développer, déboguer et tester des applications de plateforme Windows universelle (UWP), y compris les applications que vous avez conçues pour Microsoft Surface Hub. Le simulateur n’utilise pas la même interface utilisateur en tant que Surface Hub, mais il est utile pour tester l’apparence et le comporte avec la taille d’écran du Surface Hub et la résolution de votre application.

Pour plus d’informations sur l’outil en règle générale, voir [les applications UWP s’exécutent dans le simulateur](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator).

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Ajouter des résolutions de SurfaceHub au simulateur
Pour ajouter des résolutions de SurfaceHub au simulateur:

1. Créez une configuration pour le 55" Surface Hub en enregistrant le code XML suivant dans un fichier nommé *HardwareConfigurations-surfacehub55.XML*.  

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub55</Name>
            <DisplayName>Surface Hub 55"</DisplayName>
            <Resolution>
                <Height>1080</Height>
                <Width>1920</Width>
            </Resolution>
            <DeviceSize>55</DeviceSize>
            <DeviceScaleFactor>100</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

2. Créez une configuration pour le 84" Surface Hub en enregistrant le code XML suivant dans un fichier nommé *HardwareConfigurations-surfacehub84.XML*.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub84</Name>
            <DisplayName>Surface Hub 84"</DisplayName>
            <Resolution>
                <Height>2160</Height>
                <Width>3840</Width>
            </Resolution>
            <DeviceSize>84</DeviceSize>
            <DeviceScaleFactor>150</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

3. Copiez ces deux fichiers XML dans *C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;numéro de version&gt;\HardwareConfigurations*.

   > [!NOTE]
   > Enregistrement de fichiers dans ce dossier nécessite des privilèges d’administration.

4. Exécutez votre application dans le simulateur VisualStudio. Cliquez sur le bouton **Modifier la résolution** de la palette et sélectionnez une configuration de Surface Hub dans la liste.

    ![Résolutions du simulateur VisualStudio](images/vs-simulator-resolutions.png)

   > [!TIP]
   > [Activer le mode tablette](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet) afin de mieux simuler l’expérience d’un Surface Hub.

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Déployer des applications sur un appareil Surface Hub à partir de Visual Studio
Le déploiement manuel d’une application sur un Surface Hub est un processus simple.

### <a name="enable-developer-mode"></a>Activer le mode développeur
Par défaut, Surface Hub installe uniquement des applications depuis le Microsoft Store. Pour installer des applications signées par d’autres sources, vous devez activer le mode développeur.

> [!NOTE]
> Une fois que le mode développeur a été activé, vous devrez réinitialiser le Surface Hub, si vous le souhaitez pour le désactiver à nouveau. La réinitialisation de l’appareil supprime l’ensemble des configurations et des fichiers utilisateur locaux, puis réinstalle Windows.

1. À partir du menu **Démarrer** de l’appareil Surface Hub, ouvrez l’application Paramètres.

   > [!NOTE]
   > Accéder à l’application paramètres sur Surface Hub nécessite des privilèges d’administration.

2. Accédez à **mise à jour et sécurité \ > pour les développeurs**.

3. Choisissez **Mode développeur** et acceptez le message d’avertissement.

### <a name="deploy-your-app-from-visual-studio"></a>Déployer votre application à partir de VisualStudio
Pour plus d’informations sur le processus de déploiement en général, voir le [déploiement et débogage des applications UWP](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > [!NOTE]
   > Cette fonctionnalité nécessite Visual Studio 2015 Update 1 ou version ultérieure, toutefois, nous vous recommandons d’utiliser la dernière version la plus à jour de Visual Studio. Une instance de Visual Studio à jour sera gibe vous le dernier développement et mises à jour de sécurité.

1. Accédez à la liste déroulante des cibles de débogage à côté du bouton **Démarrer le débogage**, puis sélectionnez **Ordinateur distant**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Liste déroulante des cibles de débogage VisualStudio](images/vs-debug-target.png)

2. Entrez l’adresseIP de l’appareil SurfaceHub. Vérifiez que le mode d’authentification **Universel** est sélectionné.

   > [!TIP] 
   > Une fois que vous avez activé le mode développeur, vous pouvez trouver l’adresse IP du Surface Hub sur l’écran d’accueil.

3. Cliquez sur **Démarrer le débogage (F5)** pour déployer et déboguer votre application sur le Surface Hub, ou appuyez sur Ctrl + F5 pour déployer uniquement votre application.

   > [!TIP]
   > Si le Surface Hub affiche l’écran d’accueil, masquez-le en appuyant sur n’importe quel bouton.
