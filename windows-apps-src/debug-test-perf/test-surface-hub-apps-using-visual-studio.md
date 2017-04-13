---
author: mcleblanc
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: "Tester les applications de Surface Hub à l’aide de VisualStudio"
description: "Le simulateur de VisualStudio fournit un environnement permettant de concevoir, développer, déboguer et tester des applications UWP, y compris les applications générées pour SurfaceHub."
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: ed2f7dc63e478d3dde2eb58b502d373db3181501
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Tester les applications de Surface Hub à l’aide de Visual Studio
Le simulateur de Visual Studio fournit un environnement dans lequel vous pouvez concevoir, développer, déboguer et tester des applications de plateforme Windows universelle (UWP), y compris les applications que vous avez conçues pour Microsoft Surface Hub. Le simulateur n’utilise pas la même interface utilisateur que Surface Hub, mais il permet de tester l’apparence et le comportement de votre application avec la taille d’écran et la résolution de Surface Hub.

Pour plus d’informations, voir [Exécuter des applications du Windows Store dans le simulateur](https://msdn.microsoft.com/library/hh441475.aspx).

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Ajouter des résolutions de SurfaceHub au simulateur
Pour ajouter des résolutions de SurfaceHub au simulateur:

1. Créez une configuration pour le Surface Hub 55 pouces en enregistrant le code XML suivant dans un fichier nommé **HardwareConfigurations-SurfaceHub55.xml**.  

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

2. Créez une configuration pour le Surface Hub 84 pouces en enregistrant le code XML suivant dans un fichier nommé **HardwareConfigurations-SurfaceHub84.xml**.

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

3. Copiez ces deux fichiers XML dans **C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;numéro de version&gt;\HardwareConfigurations**.

   > **Remarque**&nbsp;&nbsp;L’enregistrement de fichiers dans ce dossier nécessite des privilèges d’administration.

4. Exécutez votre application dans le simulateur VisualStudio. Cliquez sur le bouton **Modifier la résolution** de la palette et sélectionnez une configuration de Surface Hub dans la liste.

    ![Résolutions du simulateur VisualStudio](images/vs-simulator-resolutions.png)

   > **Conseil**&nbsp;&nbsp;[Activez le mode tablette](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet) pour simuler plus précisément l’expérience sur un appareil Surface Hub.

## <a name="deploy-apps-to-a-surface-hub-from-visual-studio"></a>Déployer des applications sur un appareil Surface Hub à partir de VisualStudio
Le déploiement manuel d’une application est un processus simple.

### <a name="enable-developer-mode"></a>Activer le mode développeur
Par défaut, Surface Hub installe uniquement des applications du WindowsStore. Pour installer des applications signées par d’autres sources, vous devez activer le mode développeur.

> **Remarque**&nbsp;&nbsp;Une fois que le mode développeur a été activé, vous devez réinitialiser l’appareil Surface Hub pour le redésactiver. La réinitialisation de l’appareil supprime l’ensemble des configurations et des fichiers utilisateur locaux, puis réinstalle Windows.

1. À partir du menu **Démarrer** de l’appareil Surface Hub, ouvrez l’application Paramètres.

   >  **Remarque**&nbsp;&nbsp;L’accès à l’application Paramètres nécessite des privilèges d’administration.

2. Accédez à **Mise à jour et sécurité&gt; Pour les développeurs**.

3. Choisissez **Mode développeur** et acceptez le message d’avertissement.

### <a name="deploy-your-app-from-visual-studio"></a>Déployer votre application à partir de VisualStudio
Pour plus d’informations, voir [Déploiement et débogage des applications de plateforme Windows universelle (UWP)](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > **Remarque**&nbsp;&nbsp;Cette fonctionnalité nécessite au moins **Visual Studio2015 Update1**.

1. Accédez à la liste déroulante des cibles de débogage à côté du bouton **Démarrer le débogage**, puis sélectionnez **Ordinateur distant**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Liste déroulante des cibles de débogage VisualStudio](images/vs-debug-target.png)

2. Entrez l’adresseIP de l’appareil SurfaceHub. Vérifiez que le mode d’authentification **Universel** est sélectionné.

   > **Conseil**&nbsp;&nbsp;Une fois que vous avez activé le mode développeur, vous pouvez trouver l’adresse IP de l’appareil Surface Hub sur l’écran d’accueil.

3. Choisissez **Démarrer le débogage (F5)** pour déployer et déboguer votre application sur l’appareil Surface Hub, ou appuyez sur Ctrl+F5 pour déployer uniquement votre application.

   > **Conseil**&nbsp;&nbsp;Si l’appareil Surface Hub apparaît sur l’écran d’accueil, masquez-le en appuyant sur n’importe quel bouton.
