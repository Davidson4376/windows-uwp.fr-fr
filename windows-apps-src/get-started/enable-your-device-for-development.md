---
author: QuinnRadich
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: Activer votre appareil pour le développement
description: Configurez votre appareil Windows10 pour le développement et le débogage.
keywords: Commencer avec une licence de développeur Visual Studio, appareil avec licence de développeur activée
ms.author: quradic
ms.date: 05/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad817bbae2fb8b28b95095880aa1a65c391720f3
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2856862"
---
# <a name="enable-your-device-for-development"></a>Activer votre appareil pour le développement

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>Activer le Mode développeur, charger une version test des applications et accéder à d’autres fonctionnalités de développement

![Activer vos appareils pour le développement](images/developer-poster.png)

Si vous utilisez votre ordinateur pour des activités quotidiennes ordinaires, comme les jeux, la navigation sur le web, la messagerie ou les applications Office, vous n’avez *pas* besoin d’activer le Mode développeur et ne devez pas le faire. Les autres informations de cette page ne sont pas importantes pour vous, et vous pouvez en toute sécurité revenir à ce que vous faisiez précédemment. Merci de votre attention!

Toutefois, si vous utilisez Visual Studio sur un ordinateur pour créer un logiciel pour la première fois, vous *devez* activer le Mode développeur sur le PC de développement, ainsi que sur tous les appareils que vous allez utiliser pour tester votre code. Si vous ouvrez un projet UWP alors que le Mode développeur n’est pas activé, la page des paramètres **Pour les développeurs** s'ouvre ou la boîte de dialogue suivante s’affiche dans Visual Studio:

![Boîte de dialogue d’activation du mode développeur affichée dans Visual Studio](images/latestenabledialog.png)

Si cette boîte de dialogue s'affiche, cliquez sur **paramètres pour les développeurs** afin d'ouvrir la page de paramètres **Pour les développeurs**.

> [!NOTE]
> Vous pouvez à tout moment accéder à la page **Pour les développeurs** en vue d’activer ou de désactiver le mode développeur: entrez simplement «pour les développeurs» dans la zone de recherche de Cortana, dans la barre des tâches.

## <a name="accessing-settings-for-developers"></a>Accès aux paramètres pour les développeurs

Pour activer le mode développeur ou accéder à d'autres paramètres:

1.  À partir de la boîte de dialogue des paramètres **pour les développeurs**, choisissez le niveau d’accès dont vous avez besoin.
2.  Lisez la clause d’exclusion de responsabilité pour le paramètre choisi, puis cliquez sur **Oui** pour accepter la modification.

> [!NOTE]
> L’activation du mode développeur requiert un accès administrateur. Si votre appareil appartient à votre organisation, il se peut que cette option soit désactivée.

Voici la page de paramètres pour la famille d’appareils de bureau:

![Pour afficher vos options, accédez aux Paramètres, sélectionnez Mise à jour et sécurité, puis Pour les développeurs.](images/devmode-pc-options.png)

Voici la page des paramètres relative à la famille d’appareils mobiles:

![Accédez aux Paramètres de votre téléphone, puis choisissez Mise à jour et sécurité.](images/devmode-mob.png)

## <a name="which-setting-should-i-choose-sideload-apps-or-developer-mode"></a>Quel paramètre choisir: Charger la version test des applications ou Mode développeur?

 Vous pouvez activer un appareil pour le développement ou simplement pour le chargement indépendant.

-   *Applications Microsoft Store* est le paramètre par défaut. Si vous ne développez pas des applications, ou si vous utilisez des applications internes spécifiques développées par votre entreprise, ce paramètre doit être activé.
-   *Charger la version test des applications* consiste à installer, puis à exécuter ou tester une application qui n’a pas été certifiée par le MicrosoftStore. Il peut par exemple s’agir d’une application utilisée en interne au sein de votre entreprise.
-   Le *mode développeur* vous permet de procéder au chargement indépendant des applications et d’exécuter des applications à partir de VisualStudio en mode débogage. 

Par défaut, vous pouvez uniquement installer des applications de plateforme Windows universelle (UWP) à partir du MicrosoftStore. La modification de ces paramètres en vue d’utiliser les fonctionnalités de développement peut entraîner la modification du niveau de sécurité de votre appareil. N’installez pas d’applications à partir de sources non vérifiées.

### <a name="sideload-apps"></a>Charger la version test des applications

Le paramètre Charger la version test des applications est généralement utilisé par des sociétés ou des écoles qui ont besoin d’installer des applications personnalisées sur des appareils gérés, sans passer par le MicrosoftStore. Dans ce cas, l’organisation applique généralement une stratégie visant à désactiver le paramètre *ApplicationsUWP*, comme le montre l’image précédente de la page des paramètres. L’organisation fournit aussi le certificat nécessaire et l’emplacement d’installation pour le chargement indépendant des applications. Pour plus d’informations, voir les articles TechNet [Charger la version test des applications dans Windows10](https://technet.microsoft.com/library/mt269549.aspx) et [Prendre en main le déploiement d’applications dans MicrosoftIntune](https://technet.microsoft.com/library/dn646955.aspx).

Informations spécifiques à la famille d’appareils

-   Pour la famille d’appareils de bureau: vous pouvez installer un package d’application (.appx) et tout certificat nécessaire à l’exécution de l’application en exécutant le script Windows PowerShell créé avec le package («Add-AppDevPackage.ps1»). Pour plus d’informations, voir [Création de packages d’application UWP](../packaging/packaging-uwp-apps.md).

-   Pour la famille d’appareils mobiles: si le certificat requis est déjà installé, vous pouvez appuyer sur le fichier pour installer tout fichier .appx reçu par courrier électronique ou sur une carte SD.


Le paramètre **Charger la version test des applications** est une option plus sécurisée que le mode développeur, car vous ne pouvez pas installer d’applications sans certificat approuvé sur l’appareil.

> [!NOTE]
> Si vous effectuez un chargement indépendant des applications, veillez à ce que les applications que vous installez proviennent toujours de sources fiables. Quand vous procédez au chargement d’une version test d’une application qui n’a pas été certifiée par le MicrosoftStore, vous indiquez que vous avez obtenu l’ensemble des droits nécessaires au chargement d’une version test de cette application et que vous êtes l’unique responsable des dommages résultant de l’installation et de l’exécution de cette application. Voir la section Windows &gt; MicrosoftStore de cette [déclaration de confidentialité](http://go.microsoft.com/fwlink/?LinkId=521839).


### <a name="developer-mode"></a>Mode développeur

Le mode développeur remplace l’exigence de Windows8.1 relative à la détention d’une licence de développeur.  Le paramètre Mode développeur est proposé en plus du chargement indépendant. Il offre une fonction de débogage et d’autres options de déploiement, notamment le démarrage d’un service SSH pour permettre le déploiement de cet appareil. Pour arrêter ce service, vous devez désactiver le mode développeur.

Quand vous activez le mode développeur sur le bureau, un ensemble de fonctionnalités est installé, à savoir:
- Portail d’appareil Windows. Portail d’appareil est activé et les règles de pare-feu associées sont configurées seulement si l’option **Activer Portail d’appareil** est activée.
- Installation et configuration des règles de pare-feu pour les servicesSSH qui permettent l’installation à distance des applications. L’activation de l’option **Découverte d'appareils** activera le serveurSSH.


## <a name="additional-developer-mode-features"></a>Fonctionnalités supplémentaires du mode développeur

Pour chaque famille d’appareils, des fonctionnalités de développement supplémentaires peuvent être disponibles. Ces fonctionnalités sont disponibles uniquement quand le mode développeur est activé sur l’appareil, et peuvent varier selon la version de votre système d’exploitation.

Cette image montre les fonctionnalités du mode développeur pour Windows10:

![Options du mode développeur](images/devmode-mob-options.png) 

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>Portail d’appareil

Pour en savoir plus sur DevicePortal, consultez [Vue d’ensemble de WindowsDevicePortal](../debug-test-perf/device-portal.md).

Pour obtenir des instructions d’installation spécifiques pour l’appareil, voir:
- [Device Portal pour Bureau](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-desktop)
- [Device Portal pour HoloLens](https://developer.microsoft.com/windows/holographic/using_the_windows_device_portal)
- [Device Portal pour IoT](https://developer.microsoft.com/windows/iot/docs/DevicePortal)
- [Device Portal pour appareils mobiles](../debug-test-perf/device-portal-mobile.md)
- [Device Portal pour Xbox](../debug-test-perf/device-portal-xbox.md)

Si vous rencontrez des difficultés pour activer le Mode développeur ou Device Portal, consultez le forum [Problèmes connus](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) pour chercher des solutions à ces problèmes, ou visitez [Échec de l’installation du package Mode développeur ou du lancement de Device Portal](#failure-to-install-developer-mode-package) pour plus d’informations et savoir quelles bases de connaissances WSUS autoriser pour débloquer le package Mode développeur. 

### <a name="ssh"></a>SSH

Les servicesSSH sont activés dès lors que vous activez la découverte d'appareils sur votre appareil.  Ils sont utilisés lorsque votre appareil est une cible de déploiement distant pour des applicationsUWP.   Ces services se nomment «SSH Server Broker» et «SSH Server Proxy».

> [!NOTE]
> Il ne s’agit pas de l’implémentation OpenSSH de Microsoft, que vous pouvez trouver sur [GitHub](https://github.com/PowerShell/Win32-OpenSSH).  

Pour tirer parti des services SSH, vous pouvez activer la découverte d’appareils pour permettre le couplage de code PIN. Si vous avez l’intention d’exécuter un autre service SSH, vous pouvez le configurer sur un autre port ou désactiver les services SSH du mode développeur. Pour désactiver les services SSH, désactivez la découverte d’appareils.  

La connexion SSH s’effectue via le compte DevToolsUser, qui accepte un mot de passe pour l’authentification.  Ce mot de passe est le code PIN qui s’affiche sur l’appareil après avoir appuyé sur le bouton de couplage de la découverte d’appareils. Il est valide uniquement lorsque le code PIN est affiché.  Un sous-système SFTP est également activé pour la gestion manuelle du dossier DevelopmentFiles, dans lequel les déploiements de fichiers isolés sont installés à partir de Visual Studio. 

#### <a name="caveats-for-ssh-usage"></a>Avertissements concernant l’utilisation de SSH
Le serveur SSH existant utilisé dans Windows n’est pas encore conforme au protocole. De ce fait, l’utilisation d’un client SFTP ou SSH peut nécessiter une configuration spéciale.  En particulier, le sous-système SFTP exécutant la version3 ou inférieure, tout client qui se connecte doit être configuré de façon à anticiper un ancien serveur.  Sur des appareils plus anciens, le serveurSSH utilise `ssh-dss` pour l’authentification de clé publique, ce qui est déconseillé par OpenSSH.  Pour se connecter à ces appareils, le clientSSH doit être configuré manuellement pour accepter `ssh-dss`.  

### <a name="device-discovery"></a>Découverte d’appareils

Quand vous activez la découverte d’appareils, vous consentez à rendre votre appareil visible des autres appareils du réseau via mDNS.  Cette fonctionnalité vous permet également d’obtenir le code PIN SSH pour effectuer le couplage à cet appareil, en appuyant sur le bouton de couplage affiché lorsque la découverte d’appareils est activée.  Cette invite de code PIN doit être affichée sur l’écran afin de terminer votre premier déploiement Visual Studio ciblant l’appareil.  

![Couplage de code PIN](images/devmode-pc-pinpair.PNG)

N’activer la découverte d’appareils que si vous envisagez de faire de l’appareil une cible de déploiement. Par exemple, si vous utilisez DevicePortal pour déployer une application sur un téléphone à des fins de test, vous devez activer la découverte d’appareils sur le téléphone, mais pas sur votre PC de développement.

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Optimisations pour l’Explorateur Windows, Bureau à distance et PowerShell (Bureau uniquement)

 Pour la famille d’appareils de bureau, la page de paramètres **Pour les développeurs** propose des raccourcis vers les paramètres qui vous permettent d’optimiser votre PC pour les tâches de développement. Pour chaque paramètre, vous pouvez cocher la case correspondante et cliquer sur **Appliquer**, ou cliquez sur le lien **Afficher les paramètres** pour ouvrir la page de paramètres de cette option. 


## <a name="notes"></a>Remarques
Dans les versions antérieures de Windows10Mobile, une option de vidages sur incident était présente dans le menu Paramètres de développeur.  Elle a été déplacée vers [Portail d’appareil](../debug-test-perf/device-portal.md) afin de pouvoir être utilisée à distance, plutôt que simplement via USB.  

Vous pouvez utiliser plusieurs outils pour déployer une application à partir d’un PC Windows10 sur un appareil Windows10. Les deux appareils doivent être connectés au même sous-réseau du réseau par une connexion filaire ou sans fil, ou ils doivent être connectés par USB. Dans les deux cas, seul le package d’application (.appx/.appxbundle) est installé, et non les certificats.

-   Utilisez l’outil de déploiement d’applications Windows10 (WinAppDeployCmd). En savoir plus sur [l’outil WinAppDeployCmd](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx).
-   Vous pouvez utiliser [Portail d’appareil](../debug-test-perf/device-portal.md) pour effectuer un déploiement de votre navigateur vers un appareil mobile exécutant Windows10 version1511 ou ultérieure. Utilisez la page **[Applications](../debug-test-perf/device-portal.md#apps-manager)** dans Device Portal pour charger un package d’application (.appx) sur le serveur et l’installer sur l’appareil.

## <a name="failure-to-install-developer-mode-package"></a>Échec de l’installation du package Mode développeur
Parfois, en raison de problèmes réseau ou d’administration, le Mode développeur ne s’installe pas correctement. Le package Mode développeur est nécessaire pour un déploiement **à distance** sur ce PC - à l’aide de Device Portal depuis un navigateur ou de la fonction Découverte d’appareils pour activer SSH--mais pas pour un développement local.  Même si vous rencontrez ces problèmes, vous pouvez toujours déployer votre application localement à l’aide de Visual Studio, ou à partir de cet appareil sur un autre appareil. 

Voir le forum [Problèmes connus](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) pour rechercher des solutions de contournement à ces problèmes et bien plus encore. 

> [!NOTE]
> Si le Mode développeur ne s’installe correctement, nous vous invitons à une demande d’évaluation de fichiers. Dans l’application de **Commentaires Hub** , sélectionnez **Ajouter nouveau commentaires**, choisissez la catégorie de la **Plateforme de développement** et de la sous-catégorie **Mode développeur** . Envoi de commentaires permettra à Microsoft de résoudre le problème que vous avez rencontré.

### <a name="failed-to-locate-the-package"></a>Échec de la localisation du package

«Le package Mode développeur n’a pas pu être localisé dans Windows Update. Code d’erreur 0x80004005. En savoir plus»   

Cette erreur peut se produire en raison d’un problème de connectivité réseau, des paramètres d’Entreprise ou d’un package manquant. 

Pour résoudre ce problème:

1. Assurez-vous que votre ordinateur est connecté à Internet. 
2. Si vous utilisez un ordinateur appartenant à un domaine, adressez-vous à votre administrateur réseau. Le package Mode développeur, comme toutes les fonctionnalités à la demande, est bloqué par défaut dans WSUS. 2.1. Pour débloquer le package Mode développeur dans les versions précédentes et actuelles, les bases de connaissances suivantes doivent être autorisées dans WSUS: 4016509, 3180030, 3197985  
3. Recherchez les mises à jour de Windows dans Paramètres > Mises à jour et sécurité > Mises à jour Windows.
4. Vérifiez que le package Mode développeur Windows est présent dans Paramètres &gt; Système &gt; Applications et fonctionnalités &gt; Gérer les fonctionnalités facultatives &gt; Ajouter une fonctionnalité. S’il n’est pas présent, Windows ne peut pas trouver le package approprié pour votre ordinateur. 

Après avoir suivi les étapes ci-dessus, désactivez puis réactivez le Mode développeur pour vérifier le correctif. 


### <a name="failed-to-install-the-package"></a>Échec de l’installation du package

«Échec d’installation du package Mode développeur. Code d’erreur 0x80004005. En savoir plus»

Cette erreur peut se produire en raison d’incompatibilités entre votre version de Windows et le package Mode développeur. 

Pour résoudre ce problème:

1. Recherchez les mises à jour de Windows dans Paramètres &gt; Mises à jour et sécurité &gt; Mises à jour Windows.
2. Redémarrez votre ordinateur pour vérifier que toutes les mises à jour sont appliquées.


## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>Utiliser des stratégies de groupe ou des clés de Registre pour activer un appareil

Pour la plupart des développeurs, vous pouvez utiliser l’application Paramètres pour activer votre appareil pour le débogage. Dans certains scénarios, comme les tests automatisés, vous pouvez employer d’autres méthodes pour activer votre appareil de bureau Windows10 pour le développement.  Notez que ces étapes n’activeront pas le serveur SSH ou n’autoriseront pas l’appareil à être ciblé pour le déploiement distant et le débogage. 

Vous pouvez utiliser gpedit.msc pour définir les stratégies de groupe visant à activer l’appareil, sauf si vous disposez de Windows10 Famille. Si vous disposez de Windows10 Famille, vous devez exécuter des commandes regedit ou PowerShell pour définir les clés de Registre directement en vue d’activer votre appareil.

**Utiliser gpedit afin d’activer votre appareil**

1.  Exécutez **Gpedit.msc**.
2.  Accédez à Stratégie de l’ordinateur local &gt; Configuration ordinateur &gt; Modèles d’administration &gt; Composants Windows &gt; Déploiement du package d’application
3.  Pour activer le chargement indépendant, modifiez les stratégies afin d’activer:

    -   **Autoriser l’installation des applications approuvées**

    - ou -

    Pour activer le mode développeur, modifiez les stratégies pour activer les deux options suivantes:

    -   **Autoriser l’installation des applications approuvées**
    -   **Autorise le développement d’applications UWP et leur installation depuis un environnement de développement intégré**

4.  Redémarrez votre machine.

**Utiliser regedit pour activer votre appareil**

1.  Exécutez **regedit**.
2.  Pour activer le chargement indépendant, définissez cette valeur DWORD sur1:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - ou -

    Pour activer le mode développeur, définissez ces valeurs DWORD sur 1:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**Utiliser PowerShell pour activer votre appareil**

1.  Exécutez PowerShell avec des privilèges administrateur.
2.  Pour activer le chargement indépendant, exécutez cette commande:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - ou -

    Pour activer le mode développeur, exécutez cette commande:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"**

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>Mettre à niveau votre appareil de Windows8.1 vers Windows10

Après avoir créé des applications ou effectué un chargement indépendant d’applications sur votre appareil Windows8.1, vous devez installer une licence de développeur. Si vous mettez à niveau votre appareil de Windows 8.1 vers Windows 10, ces informations sont conservées. Exécutez la commande suivante pour les supprimer de votre appareil Windows 10 mis à niveau. Cette étape n’est pas obligatoire si vous effectuez une mise à niveau directement de Windows 8.1 vers Windows 10 version 1511 ou ultérieure.

**Pour annuler l’inscription d’une licence de développeur**

1.  Exécutez PowerShell avec des privilèges administrateur.
2.  Exécutez la commande suivante: **unregister-windowsdeveloperlicense**.

Après cela, vous devez activer votre appareil pour le développement, comme décrit dans cette rubrique, afin de pouvoir continuer à développer dessus. Si vous ne le faites, vous risquez d’obtenir une erreur quand vous déboguez votre application ou tentez de créer un package pour celle-ci. Voici un exemple de cette erreur :

Erreur : DEP0700 : Échec de l’inscription de l’application.

## <a name="see-also"></a>Voir aussi

* [Votre première application](your-first-app.md)
* [Publier votre application UWP](https://developer.microsoft.com/store/publish-apps)
* [Articles sur les procédures de développement d’applications UWP](https://developer.microsoft.com/windows/apps/develop)
* [Exemples de code pour les développeurs UWP](https://developer.microsoft.com/windows/samples)
* [Qu’est-ce qu’une application UWP?](universal-application-platform-guide.md)
* [Créer un compte Windows](sign-up.md)
