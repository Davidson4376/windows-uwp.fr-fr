---
author: GrantMeStrength
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: "Activer votre appareil pour le développement"
description: "Configurez votre appareil Windows 10 pour le développement et le débogage."
keywords: activer un appareil
translationtype: Human Translation
ms.sourcegitcommit: ed91f7585b63199ab9d591c712d4260a3b452b85
ms.openlocfilehash: 416dce2f7cbe3bba9285f7e354868a2c00728802

---
# <a name="enable-your-device-for-development"></a>Activer votre appareil pour le développement

Avant d’écrire des applications, vous devez activer le mode développeur sur votre PC de développement et sur les appareils sur lesquels vous entendez tester votre code.

## <a name="use-developer-features"></a>Utiliser les fonctionnalités de développement

### <a name="develop-your-app-with-microsoft-visual-studio"></a>Développer votre application avec Microsoft Visual Studio

Vous devez activer le mode développeur sur votre PC avant de pouvoir ouvrir un projet d’application UWP dans Visual Studio. Si vous ouvrez un projet UWP et que le mode développeur n’est pas activé, la page de paramètres **Pour les développeurs** s’ouvre automatiquement. Pour activer le mode développeur, suivez les instructions de la section suivante.

Quand vous ouvrez un projet d’application UWP dans Visual Studio sur Windows 10, version 1511 ou antérieure, vous obtenez cette boîte de dialogue dans Visual Studio. 

![Boîte de dialogue d’activation du mode développeur affichée dans Visual Studio](images/latestenabledialog.png)

Une fois cette boîte de dialogue affichée, cliquez sur **paramètres pour les développeurs** pour ouvrir la page de paramètres **Pour les développeurs** et activer le mode développeur.

> Vous pouvez à tout moment accéder à la page **Pour les développeurs** en vue d’activer ou de désactiver le mode développeur : entrez simplement « paramètres développeur » dans la zone de recherche de Cortana, dans la barre des tâches.

### <a name="enable-your-windows-10-devices"></a>Activer vos appareils Windows 10

Vous pouvez activer un appareil pour le développement ou simplement pour le chargement indépendant.

-   Le *chargement indépendant* consiste à installer, puis à exécuter ou tester une application qui n’a pas été certifiée par le Windows Store. Il peut par exemple s’agir d’une application utilisée en interne au sein de votre entreprise.
-   Le *mode développeur* vous permet de procéder au chargement indépendant des applications et d’exécuter des applications à partir de Visual Studio en mode débogage. 

    Quand vous activez le mode développeur, un ensemble d’options est installé, à savoir :
    - Installation de Windows Device Portal. Device Portal est activé et les règles de pare-feu associées sont configurées seulement si l’option **Activer Device Portal** est activée.
    - Installation, activation et configuration des règles de pare-feu pour les services SSH qui permettent l’installation à distance des applications.
    - (Bureau uniquement) Activation facultative du sous-système Windows pour Linux. Pour plus d’informations, voir [À propos de Bash sur Ubuntu sur Windows](https://msdn.microsoft.com/commandline/wsl/about).

Pour plus d’informations sur les options, voir [Quels paramètres choisir : Charger la version test des applications ou Mode développeur ?](https://msdn.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#which-settings-should-i-choose-sideload-apps-or-developer-mode)

**Pour utiliser les fonctionnalités de développement**

1.  Sur l’appareil que vous souhaitez activer, accédez aux **Paramètres**. Choisissez **Mise à jour et sécurité**, puis **Pour les développeurs**.
2.  Choisissez le niveau d’accès dont vous avez besoin (pour développer des applications UWP, choisissez **Mode développeur**). 
3.  Lisez la clause d’exclusion de responsabilité pour le paramètre choisi, puis cliquez sur **Oui** pour accepter la modification.

> [!NOTE]
> Si votre appareil appartient à votre organisation, il se peut que certaines options soient désactivées, comme dans l’illustration ci-dessous.

Voici la page de paramètres pour la famille d’appareils de bureau.

![Pour afficher vos options, accédez aux Paramètres, sélectionnez Mise à jour et sécurité, puis Pour les développeurs.](images/devmode-pc-options.png)

Voici la page des paramètres relative à la famille d’appareils mobiles.

![Accédez aux Paramètres de votre téléphone, puis choisissez Mise à jour et sécurité.](images/devmode-mob.png)

## <a name="developer-mode-features"></a>Fonctionnalités du mode développeur

Pour chaque famille d’appareils, des fonctionnalités de développement supplémentaires peuvent être disponibles. Ces fonctionnalités sont disponibles uniquement quand le mode développeur est activé sur l’appareil, et peuvent varier selon la version de votre système d’exploitation.

Cette image montre les fonctionnalités de développement pour la famille d’appareils mobiles sur Windows 10, Version 1511.

![Options du Mode développeur pour les appareils mobiles](images/devmode-mob-options.png) 

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>Device Portal

Pour en savoir plus sur la découverte d’appareils et sur Device Portal, consultez [Vue d’ensemble de Windows Device Portal](../debug-test-perf/device-portal.md).

Pour obtenir des instructions d’installation spécifiques pour l’appareil, voir :
- [Device Portal pour Bureau](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-desktop)
- [Device Portal pour HoloLens](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [Device Portal pour IoT](https://developer.microsoft.com/windows/iot/docs/DevicePortal)
- [Device Portal pour appareils mobiles](../debug-test-perf/device-portal-mobile.md)
- [Device Portal pour Xbox](../debug-test-perf/device-portal-xbox.md)

Si vous avez des difficultés à activer le mode développeur ou Device Portal, consultez le forum [Problèmes connus](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) pour chercher des solutions à ces problèmes. 

###<a name="ssh"></a>SSH

Les services SSH sont activés dès lors que vous activez le mode développeur sur votre appareil.  Ils sont utilisés du moment où votre appareil est une cible de déploiement pour des applications UWP.   Ces services se nomment « SSH Server Broker » et « SSH Server Proxy ».

> [!NOTE]
> Il ne s’agit pas de l’implémentation OpenSSH de Microsoft, que vous pouvez trouver sur [GitHub](https://github.com/PowerShell/Win32-OpenSSH).

Pour tirer parti des services SSH, vous pouvez activer la découverte d’appareils pour permettre le couplage de code PIN. Si vous avez l’intention d’exécuter un autre service SSH, vous pouvez le configurer sur un autre port ou désactiver les services SSH du mode développeur. Pour désactiver les services SSH, désactivez simplement le mode développeur.  

### <a name="device-discovery"></a>Découverte d’appareils

Quand vous activez la découverte d’appareils, vous consentez à rendre votre appareil visible des autres appareils du réseau via mDNS.  Cette fonctionnalité permet aussi d’obtenir le code PIN SSH pour le couplage à cet appareil.  

![Couplage de code PIN](images/devmode-pc-pinpair.PNG)

N’activer la découverte d’appareils que si vous envisagez de faire de l’appareil une cible de déploiement. Par exemple, si vous utilisez Device Portal pour déployer une application sur un téléphone à des fins de test, vous devez activer la découverte d’appareils sur le téléphone, mais pas sur votre PC de développement.

### <a name="error-reporting-mobile-only"></a>Rapport d’erreurs (Mobile uniquement)

Définissez cette valeur pour spécifier le nombre de vidages sur incident enregistrés sur votre téléphone.

La collecte des vidages sur incident sur votre téléphone vous permet d’accéder aux informations d’incident importantes immédiatement après l’incident. Les vidages sont collectés uniquement pour les applications signées par les développeurs. Vous pouvez trouver les vidages dans le système de stockage de votre téléphone, dans le dossier Documents\Debug. Pour plus d’informations sur les fichiers de vidage, voir [Utiliser les fichiers de dump pour déboguer les pannes et les blocages d’application dans Visual Studio](https://msdn.microsoft.com/library/d5zhxt22.aspx).

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Optimisations pour l’Explorateur Windows, Bureau à distance et PowerShell (Bureau uniquement)

 Pour la famille d’appareils de bureau, la page de paramètres **Pour les développeurs** propose des raccourcis vers les paramètres qui vous permettent d’optimiser votre PC pour les tâches de développement. Pour chaque paramètre, vous pouvez cocher la case correspondante et cliquer sur **Appliquer**, ou cliquez sur le lien **Afficher les paramètres** pour ouvrir la page de paramètres de cette option. 

## <a name="which-settings-should-i-choose-sideload-apps-or-developer-mode"></a>Quels paramètres choisir : Charger la version test des applications ou Mode développeur ?

Par défaut, vous pouvez installer des applications de plateforme Windows universelle (UWP) uniquement à partir du Windows Store. La modification de ces paramètres en vue d’utiliser les fonctionnalités de développement peut entraîner la modification du niveau de sécurité de votre appareil. N’installez pas d’applications à partir de sources non vérifiées.

### <a name="sideload-apps"></a>Charger la version test des applications

Le paramètre Charger la version test des applications est généralement utilisé par des sociétés ou écoles qui ont besoin d’installer des applications personnalisées sur des appareils gérés sans passer par le Windows Store. Dans ce cas, l’organisation applique généralement une stratégie visant à désactiver le paramètre *Applications du Windows Store*, comme le montre l’image précédente de la page des paramètres. L’organisation fournit aussi le certificat nécessaire et l’emplacement d’installation pour le chargement indépendant des applications. Pour plus d’informations, voir les articles TechNet [Charger la version test des applications dans Windows 10](https://technet.microsoft.com/library/mt269549.aspx) et [Prendre en main le déploiement d’applications dans Microsoft Intune](https://technet.microsoft.com/library/dn646955.aspx).

Informations spécifiques à la famille d’appareils

-   Pour la famille d’appareils de bureau : vous pouvez installer un package d’application (.appx) et tout certificat nécessaire à l’exécution de l’application en exécutant le script Windows PowerShell créé avec le package (« Add-AppDevPackage.ps1 »). Pour plus d’informations, voir [Création de packages d’application UWP](../packaging/packaging-uwp-apps.md).

-   Pour la famille d’appareils mobiles : si le certificat requis est déjà installé, vous pouvez appuyer sur le fichier pour installer tout fichier .appx reçu par courrier électronique ou sur une carte SD.

Le paramètre **Charger la version test des applications** est une option plus sécurisée que le mode développeur, car vous ne pouvez pas installer d’applications sans certificat approuvé sur l’appareil.

> [!NOTE]
> Si vous effectuez un chargement indépendant des applications, veillez à ce que les applications que vous installez proviennent toujours de sources fiables. Quand vous procédez au chargement indépendant d’une application qui n’a pas été certifiée par le Windows Store, vous indiquez que vous avez obtenu l’ensemble des droits nécessaires au chargement indépendant de cette application et que vous êtes l’unique responsable des dommages résultant de l’installation et de l’exécution de cette application. Voir la section Windows &gt; Windows Store de cette [déclaration de confidentialité](http://go.microsoft.com/fwlink/?LinkId=521839).

### <a name="developer-mode"></a>Mode développeur

Le mode développeur remplace l’exigence de Windows 8.1 relative à la détention d’une licence de développeur.  Le paramètre Mode développeur est proposé en plus du chargement indépendant. Il offre une fonction de débogage et d’autres options de déploiement, notamment le démarrage d’un service SSH pour permettre le déploiement de cet appareil. Pour arrêter ce service, vous devez désactiver le mode développeur.

Informations spécifiques pour la famille d’appareils

-   Pour la famille d’appareils de bureau :

    Activez le mode développeur pour développer et déboguer des applications dans Visual Studio. Comme indiqué précédemment, une invite s’affiche dans Visual Studio si le mode développeur n’est pas activé.

    Permet d’activer le sous-système Windows pour Linux. Pour plus d’informations, voir [À propos de Bash sur Ubuntu sur Windows](https://msdn.microsoft.com/commandline/wsl/about).

-   Pour la famille d’appareils mobiles :

    Activez le mode développeur pour déployer des applications à partir de Visual Studio et les déboguer sur l’appareil.

    Vous pouvez appuyer sur le fichier pour installer tout fichier .appx reçu par courrier électronique ou sur une carte SD. N’installez pas d’applications à partir de sources non vérifiées.

**Conseil**  
Vous pouvez utiliser plusieurs outils pour déployer une application à partir d’un PC Windows 10 sur un appareil mobile Windows 10. Les deux appareils doivent être connectés au même sous-réseau du réseau par une connexion filaire ou sans fil, ou ils doivent être connectés par USB. Dans tous les cas, seul le package d’application (.appx) est installé et non les certificats.

-   Utilisez l’outil de déploiement d’applications Windows 10 (WinAppDeployCmd). En savoir plus sur [l’outil WinAppDeployCmd](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx).
-   À compter de Windows 10 version 1511, vous pouvez utiliser [Device Portal](#device_portal) pour effectuer un déploiement de votre navigateur vers un appareil mobile exécutant Windows 10 version 1511 ou ultérieure. Utilisez la page **[Applications](../debug-test-perf/device-portal.md#apps)** dans Device Portal pour charger un package d’application (.appx) sur le serveur et l’installer sur l’appareil.

## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>Utiliser des stratégies de groupe ou des clés de Registre pour activer un appareil

Pour la plupart des développeurs, vous pouvez utiliser l’application Paramètres pour activer votre appareil pour le débogage. Dans certains scénarios, comme les tests automatisés, vous pouvez employer d’autres méthodes pour activer votre appareil de bureau Windows 10 pour le développement.

Vous pouvez utiliser gpedit.msc pour définir les stratégies de groupe visant à activer l’appareil, sauf si vous disposez de Windows 10 Famille. Si vous disposez de Windows 10 Famille, vous devez exécuter des commandes regedit ou PowerShell pour définir les clés de Registre directement en vue d’activer votre appareil.

**Utiliser gpedit afin d’activer votre appareil**

1.  Exécutez **Gpedit.msc**.
2.  Accédez à Stratégie de l’ordinateur local &gt; Configuration ordinateur &gt; Modèles d’administration &gt; Composants Windows &gt; Déploiement du package d’application
3.  Pour activer le chargement indépendant, modifiez les stratégies afin d’activer :

    -   **Autoriser l’installation des applications approuvées**

    - ou -

    Pour activer le mode développeur, modifiez les stratégies pour activer les deux options suivantes :

    -   **Autoriser l’installation des applications approuvées**
    -   **Autorise le développement d’applications du Windows Store et leur installation depuis un environnement de développement intégré**

4.  Redémarrez votre machine.

**Utiliser regedit pour activer votre appareil**

1.  Exécutez **regedit**.
2.  Pour activer le chargement indépendant, définissez cette valeur DWORD sur 1 :

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - ou -

    Pour activer le mode développeur, définissez ces valeurs DWORD sur 1 :

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**Utiliser PowerShell pour activer votre appareil**

1.  Exécutez PowerShell avec des privilèges administrateur.
2.  Pour activer le chargement indépendant, exécutez cette commande :

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - ou -

    Pour activer le mode développeur, exécutez cette commande :

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"**

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>Mettre à niveau votre appareil de Windows 8.1 vers Windows 10

Après avoir créé des applications ou effectué un chargement indépendant d’applications sur votre appareil Windows 8.1, vous devez installer une licence de développeur. Si vous mettez à niveau votre appareil de Windows 8.1 vers Windows 10, ces informations sont conservées. Exécutez la commande suivante pour les supprimer de votre appareil Windows 10 mis à niveau. Cette étape n’est pas obligatoire si vous effectuez une mise à niveau directement de Windows 8.1 vers Windows 10 version 1511 ou ultérieure.

**Pour annuler l’inscription d’une licence de développeur**

1.  Exécutez PowerShell avec des privilèges administrateur.
2.  Exécutez la commande suivante : **unregister-windowsdeveloperlicense**.

Après cela, vous devez activer votre appareil pour le développement, comme décrit dans cette rubrique, afin de pouvoir continuer à développer dessus. Si vous ne le faites, vous risquez d’obtenir une erreur quand vous déboguez votre application ou tentez de créer un package pour celle-ci. Voici un exemple de cette erreur :

Erreur : DEP0700 : Échec de l’inscription de l’application.



<!--HONumber=Dec16_HO1-->


