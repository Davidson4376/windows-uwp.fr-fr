---
author: mcleblanc
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: "Tester les applications de Surface Hub à l’aide de Visual Studio"
description: "Le simulateur de Visual Studio fournit un environnement permettant de concevoir, développer, déboguer et tester des applications UWP, y compris les applications générées pour Surface Hub."
translationtype: Human Translation
ms.sourcegitcommit: 0bf96b70a915d659c754816f4c115f3b3f0a5660
ms.openlocfilehash: 1939508e8ace2fe3ed9210d6969d1c68843c32a9

---

# Tester les applications de Surface Hub à l’aide de Visual Studio
Le simulateur de Visual Studio fournit un environnement dans lequel vous pouvez concevoir, développer, déboguer et tester des applications de plateforme Windows universelle (UWP), y compris les applications que vous avez conçues pour Microsoft Surface Hub. Le simulateur n’utilise pas la même interface utilisateur que Surface Hub, mais il permet de tester l’apparence et le comportement de votre application avec la taille d’écran et la résolution de Surface Hub.

Pour plus d’informations, voir [Exécuter des applications du Windows Store dans le simulateur](https://msdn.microsoft.com/library/hh441475.aspx).

## Ajouter des résolutions de Surface Hub au simulateur
Pour ajouter des résolutions de Surface Hub au simulateur :

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

   > **Remarque**  L’enregistrement de fichiers dans ce dossier nécessite des privilèges d’administration.

4. Exécutez votre application dans le simulateur Visual Studio. Cliquez sur le bouton **Modifier la résolution** de la palette et sélectionnez une configuration de Surface Hub dans la liste.

    ![Résolutions du simulateur Visual Studio](images/vs-simulator-resolutions.png)

   > **Conseil**  [Activez le mode tablette](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet) pour simuler plus précisément l’expérience sur un appareil Surface Hub.

## Déployer des applications sur un appareil Surface Hub à partir de Visual Studio
Le déploiement manuel d’une application est un processus simple.

### Activer le mode développeur
Par défaut, Surface Hub installe uniquement des applications du Windows Store. Pour installer des applications signées par d’autres sources, vous devez activer le mode développeur.

> **Remarque**  Une fois que le mode développeur a été activé, vous devez réinitialiser l’appareil Surface Hub pour le redésactiver. La réinitialisation de l’appareil supprime l’ensemble des configurations et des fichiers utilisateur locaux, puis réinstalle Windows.

1. À partir du menu **Démarrer** de l’appareil Surface Hub, ouvrez l’application Paramètres.

   >  **Remarque**  L’accès à l’application Paramètres nécessite des privilèges d’administration.

2. Accédez à **Mise à jour et sécurité &gt; Pour les développeurs**.

3. Choisissez **Mode développeur** et acceptez le message d’avertissement.

### Déployer votre application à partir de Visual Studio
Pour plus d’informations, voir [Déploiement et débogage des applications de plateforme Windows universelle (UWP)](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > **Remarque**  Cette fonctionnalité nécessite au moins **Visual Studio 2015 Update 1**.

1. Accédez à la liste déroulante des cibles de débogage à côté du bouton **Démarrer le débogage**, puis sélectionnez **Ordinateur distant**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Liste déroulante des cibles de débogage Visual Studio](images/vs-debug-target.png)

2. Entrez l’adresse IP de l’appareil Surface Hub. Vérifiez que le mode d’authentification **Universel** est sélectionné.

   > **Conseil**  Une fois que vous avez activé le mode développeur, vous pouvez trouver l’adresse IP de l’appareil Surface Hub sur l’écran d’accueil.

3. Choisissez **Démarrer le débogage (F5)** pour déployer et déboguer votre application sur l’appareil Surface Hub, ou appuyez sur Ctrl+F5 pour déployer uniquement votre application.

   > **Conseil**  Si l’appareil Surface Hub apparaît sur l’écran d’accueil, masquez-le en appuyant sur n’importe quel bouton.



<!--HONumber=Aug16_HO3-->


