---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Tester les applications de Surface Hub à l’aide de Visual Studio
description: Le simulateur de Visual Studio fournit un environnement permettant de concevoir, développer, déboguer et tester des applications UWP, y compris les applications générées pour Surface Hub.
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8a846aab77fad6f087057fe930d11ac781ed7171
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362201"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Tester les applications de Surface Hub à l’aide de Visual Studio
Le simulateur de Visual Studio fournit un environnement dans lequel vous pouvez concevoir, développer, déboguer et tester des applications de plateforme Windows universelle (UWP), y compris les applications que vous avez conçues pour Microsoft Surface Hub. Le simulateur n’utilise pas la même interface utilisateur en tant que Surface Hub, mais il est utile pour tester l’apparence et le comporte avec la taille de l’écran de l’appareil Surface Hub et la résolution de votre application.

Pour plus d’informations sur l’outil de simulateur en général, consultez [exécuter des applications UWP dans le simulateur](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator).

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Ajouter des résolutions de Surface Hub au simulateur
Pour ajouter des résolutions de Surface Hub au simulateur :

1. Créer une configuration pour le 55" Surface Hub en enregistrant le code XML suivant dans un fichier nommé *HardwareConfigurations-SurfaceHub55.xml*.  

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

2. Créer une configuration pour le 84" Surface Hub en enregistrant le code XML suivant dans un fichier nommé *HardwareConfigurations-SurfaceHub84.xml*.

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

3. Copiez les deux fichiers XML dans *C:\Program Files (x86) \Common Files\Microsoft Shared\Windows simulateur\\&lt;numéro de version&gt;\HardwareConfigurations*.

   > [!NOTE]
   > Des privilèges d’administrateur sont requis pour enregistrer des fichiers dans ce dossier.

4. Exécutez votre application dans le simulateur Visual Studio. Cliquez sur le bouton **Modifier la résolution** de la palette et sélectionnez une configuration de Surface Hub dans la liste.

    ![Résolutions du simulateur Visual Studio](images/vs-simulator-resolutions.png)

   > [!TIP]
   > [Activer le mode de tablette](https://windows.microsoft.com/windows-10/getstarted-like-a-tablet) pour mieux simuler l’expérience d’un appareil Surface Hub.

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Déployer des applications sur un appareil Surface Hub à partir de Visual Studio
Déploiement manuel d’une application à un appareil Surface Hub est un processus simple.

### <a name="enable-developer-mode"></a>Activer le mode développeur
Par défaut, Surface Hub installe uniquement les applications à partir du Microsoft Store. Pour installer des applications signées par d’autres sources, vous devez activer le mode développeur.

> [!NOTE]
> Une fois que le mode développeur a été activé, vous devez réinitialiser l’appareil Surface Hub si vous souhaitez les désactiver à nouveau. La réinitialisation de l’appareil supprime l’ensemble des configurations et des fichiers utilisateur locaux, puis réinstalle Windows.

1. À partir du menu **Démarrer** de l’appareil Surface Hub, ouvrez l’application Paramètres.

   > [!NOTE]
   > Des privilèges d’administrateur sont requis pour accéder à l’application de paramètres sur Surface Hub.

2. Accédez à **mise à jour & sécurité \> pour les développeurs**.

3. Choisissez **Mode développeur** et acceptez le message d’avertissement.

### <a name="deploy-your-app-from-visual-studio"></a>Déployer votre application à partir de Visual Studio
Pour plus d’informations sur le processus de déploiement en général, consultez [déploiement et débogage des applications UWP](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > [!NOTE]
   > Cette fonctionnalité requiert Visual Studio 2015 Update 1 ou version ultérieure, mais nous vous recommandons d’utiliser la dernière version plus récente de Visual Studio. Une instance de Visual Studio à jour sera gibe vous tous les derniers développements et mises à jour de sécurité.

1. Accédez à la liste déroulante des cibles de débogage à côté du bouton **Démarrer le débogage**, puis sélectionnez **Ordinateur distant**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Liste déroulante des cibles de débogage Visual Studio](images/vs-debug-target.png)

2. Entrez l’adresse IP de l’appareil Surface Hub. Vérifiez que le mode d’authentification **Universel** est sélectionné.

   > [!TIP] 
   > Une fois que vous avez activé le mode développeur, vous trouverez l’adresse IP de la Surface du Hub sur l’écran d’accueil.

3. Sélectionnez **démarrer le débogage (F5)** pour déployer et déboguer votre application sur l’appareil Surface Hub ou appuyez sur Ctrl + F5 pour déployer simplement votre application.

   > [!TIP]
   > Si l’appareil Surface Hub affiche l’écran d’accueil, fait disparaître en choisissant un bouton.
