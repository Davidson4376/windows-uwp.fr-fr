---
author: martinekuan
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: "Activer votre appareil pour le développement"
description: "Il existe une approche différente pour le développement des appareils Windows10."
keywords: enable device
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 4c890d6202a3151e8fc0cf03b3ff33b98cd6a863

---
# Activer votre appareil pour le développement

Il existe une approche différente pour le développement des appareils Windows 10. Il n’est plus nécessaire de disposer d’une licence de développeur pour chacun des appareils que vous souhaitez utiliser pour développer, installer ou tester votre application. Il vous suffit de configurer votre appareil une seule fois pour ces tâches, à partir des paramètres. Et le tour est joué. Plus de renouvellement de vos licences de développeur tous les 30 ou 90 jours !

Si vous utilisez toujours un appareil Windows8.1 pour développer ou tester vos applications avec Microsoft VisualStudio2013 ou Microsoft VisualStudio2015, il vous faut [obtenir une licence de développeur](https://msdn.microsoft.com/library/windows/apps/Hh974578) ou [enregistrer votre WindowsPhone](https://msdn.microsoft.com/library/windows/apps/Dn614128).

## Utiliser les fonctionnalités de développement

### Développer votre application avec Microsoft Visual Studio

Si vous utilisez Microsoft Visual Studio sur un appareil Windows 10 et que vous ouvrez une solution dédiée à une application Windows 8.1 ou Windows 10, vous êtes invité à activer votre appareil avec cette boîte de dialogue. Vous devez activer votre appareil pour l’utilisation des concepteurs et le débogage de votre application.

![Boîte de dialogue d’activation du mode développeur affichée dans Visual Studio](images/latestenabledialog.png)

Lorsque cette boîte de dialogue s’affiche, cliquez sur **Paramètres pour les développeurs** pour accéder directement à la page **Mise à jour et sécurité**, comme illustré ci-dessous. Vous pouvez également cliquer sur **OK**, puis suivre les étapes ci-dessous pour activer votre appareil Windows10 pour le développement.

### Activer vos appareils Windows 10

Pour Windows 10, vous sélectionnez les fonctionnalités de développement que vous souhaitez activer sur l’appareil. Cela vaut pour tous les appareils Windows 10 : ordinateurs de bureau, tablettes et téléphones. Vous pouvez activer un appareil pour le développement, ou simplement pour procéder à un chargement indépendant.

-   Le *chargement indépendant* consiste à installer, puis à exécuter ou tester une application qui n’a pas été certifiée par le WindowsStore. Il peut par exemple s’agir d’une application utilisée en interne au sein de votre entreprise.
-   Le *mode développeur* vous permet de procéder au chargement indépendant des applications et d’exécuter des applications à partir de VisualStudio en mode débogage.

**Remarque** Si vous effectuez un chargement indépendant des applications, veillez à recourir à des sources fiables. Lorsque vous procédez au chargement indépendant d’une application qui n’a pas été certifiée par le WindowsStore, vous indiquez que vous avez obtenu l’ensemble des droits nécessaires au chargement indépendant de cette application et que vous êtes l’unique responsable des dommages résultant de l’installation et de l’exécution de cette application. Voir la section Windows &gt; Windows Store de cette [déclaration de confidentialité](http://go.microsoft.com/fwlink/?LinkId=521839).

**Pour utiliser les fonctionnalités de développement**

1.  Sur l’appareil que vous souhaitez activer, accédez aux **Paramètres**. Choisissez **Mise à jour et sécurité**, puis **Pour les développeurs**.
2.  Choisissez le niveau d’accès dont vous avez besoin. Pour plus d’informations sur les options, voir [Quels paramètres choisir: Charger la version test des applications ou Mode développeur?](#WhichSettings)
3.  Lisez la clause d’exclusion de responsabilité pour le paramètre choisi, puis cliquez sur **Oui** pour accepter la modification.

Voici la page des paramètres relative à la famille d’appareils de bureau.

![Pour afficher vos options, accédez aux Paramètres, sélectionnez Mise à jour et sécurité, puis Pour les développeurs.](images/devmode-pc-options.png)

Voici la page des paramètres relative à la famille d’appareils mobiles.

![Accédez aux Paramètres de votre téléphone, puis sélectionnez Mise à jour et sécurité.](images/devmode-mob.png)

### Quels paramètres choisir : Charger la version test des applications ou Mode développeur ?

Par défaut, vous pouvez installer des applications de plateforme Windows universelle (UWP) uniquement à partir du Windows Store. La modification de ces paramètres en vue d’utiliser les fonctionnalités de développement peut entraîner la modification du niveau de sécurité de votre appareil. N’installez pas d’applications à partir de sources non vérifiées.

**Charger la version test des applications**

Le paramètre Charger la version test des applications est généralement utilisé par des sociétés ou écoles qui ont besoin d’installer des applications personnalisées sur des appareils gérés sans passer par le Windows Store. Dans ce cas, l’organisation applique généralement une stratégie visant à désactiver le paramètre *Applications du WindowsStore*, comme illustré précédemment dans l’image de la page des paramètres du téléphone. L’organisation fournit également le certificat nécessaire et l’emplacement d’installation pour le chargement indépendant des applications. Pour plus d’informations, voir les articles TechNet [Charger la version test des applications dans Windows10](https://technet.microsoft.com/library/mt269549.aspx) et [Prendre en main le déploiement d’applications dans MicrosoftIntune](https://technet.microsoft.com/library/dn646955.aspx).

Informations spécifiques à la famille d’appareils

-   Sur la famille d’appareils de bureau: vous pouvez installer un package d’application (.appx) et tout certificat nécessaire à l’exécution de l’application en exécutant le script Windows PowerShell créé avec le package («Add-appdevpackage.ps1»).

-   Sur la famille d’appareils mobiles: si le certificat requis est déjà installé, vous pouvez appuyer sur le fichier pour installer tout fichier .appx reçu par courrier électronique ou sur une carte SD.

Le paramètre **Charger la version test des applications** constitue une option plus sécurisée que le mode développeur, car vous ne pouvez pas installer d’applications sans certificat approuvé sur l’appareil.

**Mode développeur**

Le paramètre Mode développeur est proposé en plus du chargement indépendant. Il offre une fonction de débogage et d’autres options de déploiement. Il remplace l’exigence de Windows 8.1 relative à la détention d’une licence de développeur.

Informations spécifiques à la famille d’appareils

-   Sur la famille d’appareils de bureau :

    Activez le mode développeur pour développer et déboguer des applications dans Visual Studio. Comme mentionné précédemment, une invite s’affichera dans Visual Studio si le mode développeur n’est pas activé.

-   Sur la famille d’appareils mobiles :

    Activez le mode développeur pour déployer des applications à partir de Visual Studio et les déboguer sur l’appareil.

    Vous pouvez appuyer sur le fichier pour installer tout fichier .appx reçu par courrier électronique ou sur une carte SD. N’installez pas d’applications à partir de sources non vérifiées.

**Conseil**  
Vous pouvez utiliser plusieurs outils pour déployer une application à partir d’un PC Windows10 sur un appareil mobile Windows10. Les deux appareils doivent être connectés au même sous-réseau du réseau par une connexion filaire ou sans fil, ou ils doivent être connectés par USB. Dans tous les cas, seul le package d’application (.appx) est installé et non les certificats.

-   Utilisez l’outil de déploiement d’applications Windows 10 (WinAppDeployCmd). En savoir plus sur [l’outil WinAppDeployCmd](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx).
-   Depuis Windows10 version1511, vous pouvez utiliser [Device Portal](#device_portal) pour effectuer un déploiement à partir de votre navigateur sur un appareil mobile exécutant Windows10 version1511 ou ultérieure. Utilisez la page **Applications** dans Device Portal (&lt;IP&gt;/appmanager.md) pour charger un package d’application (.appx) et l’installer sur l’appareil.

 

### Définir des stratégies de groupe ou des clés de Registre

Vous pouvez également utiliser des stratégies de groupe ou des clés de Registre pour activer votre appareil de bureau Windows 10 pour le développement.

**Sur la famille d’appareils de bureau**

Utilisez gpedit.msc pour définir les stratégies de groupe afin d’activer votre appareil, sauf si vous disposez de Windows 10 Famille. Si vous disposez de Windows 10 Famille, vous devez exécuter des commandes regedit ou PowerShell pour définir les clés de Registre directement afin d’activer votre appareil.

**Utiliser gpedit afin d’activer votre appareil**

1.  Exécutez **Gpedit.msc**.
2.  Accédez à Stratégie de l’ordinateur local &gt; Configuration ordinateur &gt; Modèles d’administration &gt; Composants Windows &gt; Déploiement du package d’application
3.  Pour activer le chargement indépendant, modifiez les stratégies afin d’activer:

    -   **Autoriser l’installation des applications approuvées**

    - - ou -

    Pour activer le mode développeur, modifiez les stratégies pour activer les deux options suivantes:

    -   **Autoriser l’installation des applications approuvées**
    -   **Autorise le développement d’applications du Windows Store et leur installation depuis un environnement de développement intégré**

4.  Redémarrez votre machine.

**Utiliser regedit pour activer votre appareil**

1.  Exécutez **regedit**.
2.  Pour activer le chargement indépendant, définissez cette valeur DWORD sur1:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - - ou -

    Pour activer le mode développeur, définissez ces valeurs DWORD sur 1:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**Utiliser PowerShell pour activer votre appareil**

1.  Exécutez PowerShell avec des privilèges administrateur.
2.  Pour activer le chargement indépendant, exécutez cette commande:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - - ou -

    Pour activer le mode développeur, exécutez cette commande:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"**

## Fonctionnalités du mode développeur

Pour chaque famille d’appareils, des fonctionnalités de développement supplémentaires peuvent être disponibles. Ces fonctionnalités sont disponibles uniquement lorsque le **Mode développeur** est activé sur l’appareil, et peuvent varier en fonction de la version de votre système d’exploitation.

Cette image illustre les fonctionnalités de développement pour la famille d’appareils mobiles sur Windows10, Version1511.

![Options du mode développeur pour les appareils mobiles](images/devmode-mob-options.png)

### <span id="device-discovery-and-pairing"></span>Détection d’appareils et DevicePortal

Pour en savoir plus sur la détection des appareils et DevicePortal, voir [Vue d’ensemble de Windows DevicePortal](../debug-test-perf/device-portal.md).

Pour obtenir des instructions d’installation spécifiques pour l’appareil, voir:
- [Device Portal pour HoloLens](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [Device Portal pour IoT](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [Device Portal pour appareils mobiles](../debug-test-perf/device-portal-mobile.md)
- [Device Portal pour Xbox](../debug-test-perf/device-portal-xbox.md)

### Rapport d’erreurs

Définissez cette valeur pour spécifier le nombre de vidages sur incident enregistrés sur votre téléphone.

La collecte des vidages sur incident sur votre téléphone vous permet d’accéder aux informations d’incident importantes immédiatement après l’incident. Les vidages sont collectés uniquement pour les applications signées par les développeurs. Vous pouvez trouver les vidages dans le système de stockage de votre téléphone, dans le dossier Documents\Debug. Pour plus d’informations sur les fichiers de vidage, voir [Utiliser les fichiers de dump pour déboguer les pannes et les blocages d’application dans Visual Studio](https://msdn.microsoft.com/library/d5zhxt22.aspx).

## Mise à niveau de votre appareil de Windows 8.1 vers Windows 10

Après avoir créé des applications ou effectué un chargement indépendant d’applications sur votre appareil Windows 8.1, vous devez installer une licence de développeur. Si vous mettez à niveau votre appareil de Windows 8.1 vers Windows 10, ces informations sont conservées. Exécutez la commande suivante pour les supprimer de votre appareil Windows 10 mis à niveau. Cette étape n’est pas obligatoire si vous effectuez une mise à niveau directement de Windows 8.1 vers Windows 10 version 1511 ou ultérieure.

**Pour annuler l’inscription d’une licence de développeur**

1.  Exécutez PowerShell avec des privilèges administrateur.
2.  Exécutez la commande suivante: **unregister-windowsdeveloperlicense**.

Après cela, vous devez activer votre appareil pour le développement, comme décrit dans cette rubrique, afin de pouvoir continuer à développer dessus. Si vous ne le faites, vous risquez d’obtenir une erreur quand vous déboguez votre application ou tentez de créer un package pour celle-ci. Voici un exemple de cette erreur :

Erreur : DEP0700 : Échec de l’inscription de l’application.





<!--HONumber=Jun16_HO4-->


