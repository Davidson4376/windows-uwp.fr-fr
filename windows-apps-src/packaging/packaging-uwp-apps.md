---
author: laurenhughes
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Création de packages d’application UWP
description: Pour distribuer ou vendre votre application de plateforme Windows universelle (UWP), vous devez créer un package d’application pour elle.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: 1ce80206823694f06e4aa5c3480b4dcb30c4f95c
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4616963"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>Créer un package d’application UWP avec Visual Studio

Pour vendre ou distribuer votre application de plateforme Windows universelle (UWP) à d’autres utilisateurs, vous devez la mettre en package. Si vous ne souhaitez pas distribuer votre application via le Microsoft Store, vous pouvez charger le package d’application directement sur un appareil ou le distribuer via une [Installation web](installing-UWP-apps-web.md). Cet article décrit le processus de configuration, de création et de test d’un package d’application UWP à l'aide de Visual Studio. Pour plus d’informations sur la gestion et déploiement d'applications cœur de métier (LOB), consultez [Gestion d'applications d'entreprise](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management).

Dans Windows 10, vous pouvez soumettre un package d’application, un ensemble d’applications ou un fichier de chargement de package application complète au centre de développement Windows. Parmi ces options, la soumission d'un fichier de téléchargement de package fournira la meilleure expérience. 

## <a name="types-of-app-packages"></a>Types de packages d’application

- **Package d’application (.appx ou .msix)**  
    Un fichier contenant votre application dans un format pouvant être chargé de manière indépendante sur un appareil. N’importe quel fichier de package d’application unique créé par Visual Studio est **pas** destiné à être soumis au centre de développement et doit être utilisé pour le chargement indépendant et à des fins de tests uniquement. Si vous souhaitez soumettre votre application au centre de développement, utilisez le fichier de chargement de package d'application.  

- **Ensemble d’applications (.appxbundle ou .msixbundle)**  
    Un ensemble d'applications désigne un type de package pouvant contenir plusieurs packages d'application, chacun étant généré pour prendre en charge une architecture d'appareil spécifique. Par exemple, un ensemble d'applications peut contenir troispackages d'application distincts pour les configurations x86, x64 et ARM. Les ensembles d'applications doivent être générés autant que possible. En effet, ils permettent aux applications d'être disponibles à un éventail d'appareils des plus larges.  

- **Fichier de chargement de package d'applications (.appxupload)**  
    Un fichier unique pouvant contenir plusieurs packages d'applications ou un ensemble d'applications pour prendre en charge diverses architectures de processeur. Le fichier de chargement peut également contenir un fichier de symbole pour [Analyser les performances de l’application](https://docs.microsoft.com/windows/uwp/publish/analytics) après la publication de votre applications dans le Microsoft Store. Ce fichier sera automatiquement créé pour vous si vous mettez en package votre application avec VisualStudio avec l'intention de la soumettre au centre de développement pour une publication. Il est important de noter que cette situation s'applique **uniquement** aux soumissions de package d'applications au centre de développement pouvant être créé à l'aide de Visual Studio.

Voici un aperçu des étapes nécessaires pour préparer et créer un package d’application:

1.  [Avant de créer un package pour votre application](#before-packaging-your-app). Suivez ces étapes pour vous assurer que votre application est prête à être mise en package pour la soumission au centre de développement.
2.  [Configurer un package d’application](#configure-an-app-package). Utilisez le concepteur de manifeste VisualStudio pour configurer le package. Par exemple, ajoutez des images de vignette et choisissez les orientations prises en charge par votre application.
3.  [Créer un fichier de chargement de package d’application](#create-an-app-package-upload-file). Utilisez l’assistant de package d'application dans Microsoft Visual Studio pour créer un package d’application, puis certifiez votre package à l’aide du kit de certification des applications Windows.
4.  [Chargez de manière indépendante votre package d’application](#sideload-your-app-package). Après le chargement indépendant de votre application sur un appareil, vous pouvez tester qu’elle fonctionne comme vous le souhaitez.

Après avoir effectué les étapes ci-dessus, vous êtes prêt à distribuer votre application. Si vous avez une application métier que vous ne prévoyez pas de vendre puisqu’elle est destinée aux utilisateurs internes uniquement, vous pouvez charger de manière indépendante cette application pour l’installer sur tout appareil Windows10.

## <a name="before-packaging-your-app"></a>Avant de créer un package pour votre application

1.  **Testez votre application.** Avant de créer un package pour votre application pour son envoi au centre de développement, assurez-vous que tout fonctionne comme prévu sur toutes les familles d’appareils que vous envisagez de prendre en charge. Ces familles d’appareils peuvent inclure des ordinateurs de bureau, des appareils portables, Surface Hub, Xbox, IoT, ou autres.
2.  **Optimisez votre application.** Vous pouvez utiliser les outils de profilage et de débogage de Visual Studio pour optimiser les performances de votre application UWP. Par exemple, l’outil Chronologie pour la réactivité de l’interface utilisateur, l’outil Utilisation de la mémoire, l’outil Utilisation du processeur, etc. Pour plus d'informations sur l'utilisation de ces outils, consultez la rubrique [Vue d'ensemble de la fonctionnalité de profilage](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour).
3.  **Vérifiez la compatibilité de .NET Native (pour les applications Visual Basic et C#).** Dans la plateforme Windows universelle, un nouveau compilateur natif améliore les performances d’exécution de votre application. Avec cette modification, vous devriez tester votre application dans cet environnement de compilation. Par défaut, la configuration de build **Release** active la chaîne d’outils .NET Native. Il est donc important de tester votre application avec cette configuration **Release** et de vérifier que votre application se comporte comme prévu. Certains problèmes courants de débogage qui peuvent se produire avec .NET Native sont expliqués plus en détail dans [Débogage des applications universelles Windows .NET Native](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx).

## <a name="configure-an-app-package"></a>Configurer un package d’application

Le fichier manifeste de l’application (Package.appxmanifest.xml) est un fichier XML qui contient les propriétés et les paramètres nécessaires pour créer votre package d’application. Par exemple, les propriétés dans le fichier manifeste d'application décrivent l’image à utiliser en tant que vignette de votre application et les orientations prises en charge par votre application quand un utilisateur fait pivoter l’appareil.

Le Concepteur de manifeste de Visual Studio vous permet de mettre à jour le fichier manifeste sans modifier le code XML brut du fichier.

**Configurer un package avec le concepteur du manifeste**

1.  Dans l’**Explorateur de solutions**, développez le nœud de projet de votre application UWP.
2.  Double-cliquez sur le fichier **Package.appxmanifest**. Si le fichier manifeste est déjà ouvert dans le mode code XML, Visual Studio vous invite à fermer le fichier.
3.  Vous pouvez maintenant décider comment configurer votre application. Chaque onglet contient des informations que vous pouvez configurer pour votre application ainsi que des liens vers des informations supplémentaires si nécessaire.  
    ![Concepteur de manifeste dans Visual Studio](images/packaging-screen1.jpg)

    Vérifiez que vous avez toutes les images requises pour une application UWP dans l’onglet **Actifs visuels**.

    À partir de l’onglet **Packaging**, vous pouvez entrer des données de publication. C’est ici que vous pouvez choisir le certificat à utiliser pour signer votre application. Toutes les applications UWP doivent être signées avec un certificat. 
    
    >[!IMPORTANT]
    >Si vous publiez votre application dans le MicrosoftStore, elle sera signée pour vous avec un certificat approuvé. Cela permet à l'utilisateur d'installer et d'exécuter votre application sans installer le certificat de signature d'application associé. 
    
    Si vous ne publiez pas votre application et souhaitez simplement charger un package d'application de manière indépendante, vous devez d'abord approuver le package. Pour approuver le package, le certificat doit être installé sur l'appareil de l'utilisateur. Pour plus d’informations sur le chargement indépendant, consultez [Activer votre appareil pour le développement](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

4.  Enregistrez votre fichier **Package.appxmanifest** une fois que vous avez apporté les modifications nécessaires pour votre application.

Si vous distribuez votre application via le MicrosoftStore, VisualStudio peut associer votre package au Store. Lorsque vous associez votre application, certains champs dans l’onglet Packages du concepteur de manifeste sont automatiquement mis à jour.

## <a name="create-an-app-package-upload-file"></a>Créer un fichier de chargement de package d’application

Pour distribuer une application via le Microsoft Store, vous devez créer un package d’application (.appx ou .msix), un ensemble d’applications (.appxbundle ou .msixbundle), ou un package de chargement (.appxupload) et [soumettre l’application empaquetée au centre de développement](https://docs.microsoft.com/windows/uwp/publish/app-submissions). Bien qu'il soit possible de soumettre un package d'application ou un ensemble d'applications seulement au centre de développement, nous vous encourageons à soumettre un package de chargement.

>[!NOTE]
> Le fichier de chargement du package d'application (.appxupload) est le **seul** type de package d'application valide pour le centre de développement pouvant être créé à l'aide de VisualStudio. D'autres exemplaires valides de [packages d'application peuvent être créés manuellement](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool), sans VisualStudio. 

Vous pouvez y procéder à l'aide de l'assistant **Créer des packages d'application**. Suivez ces étapes pour créer un package approprié pour une soumission au centre de développement à l'aide de Visual Studio.

**Pour créer votre fichier de chargement de package d’application.**

1.  Dans l’**Explorateur de solutions**, ouvrez la solution pour votre projet d’application UWP.
2.  Cliquez avec le bouton droit sur le projet et choisissez **Store**->**Create App Packages**. Si cette option est désactivée ou n’apparaît pas, vérifiez que le projet est bien un projet Windows universel.  
    ![Menu contextuel avec navigation vers Créer des packages d’application](images/packaging-screen2.jpg)

    L’assistant **Créer des packages d’application** s’affiche.

3.  Sélectionnez «Oui» dans la première boîte de dialogue demandant si vous voulez générer des packages à charger dans le centre de développement, puis cliquez sur «Suivant».  
    ![Fenêtre Créer vos packages affichée](images/packaging-screen3.jpg)

    Si vous choisissez «Non», Visual Studio ne génère pas le fichier (.appxupload) de package d'application de chargement pour les soumissions au centre de développement. Si vous souhaitez uniquement charger de manière indépendante votre application pour l’exécuter sur des appareils internes ou à des fins de test, vous pouvez sélectionner cette option. Pour plus d’informations sur le chargement indépendant, consultez [Activer votre appareil pour le développement](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
4.  Connectez-vous au tableau de bord du Centre de développement Windows à l’aide de votre compte de développeur. Si vous ne disposez pas encore d’un compte de développeur, l’Assistant vous aidera à en créer un.
5.  Sélectionnez le nom d’application de votre package ou réservez-en un nouveau sur le portail du Centre de développement Windows, si ce n’est déjà fait.  
    ![Fenêtre Créer des packages d’application avec la sélection de nom d’application affichée](images/packaging-screen4.jpg)
6.  Veillez à sélectionner les troisconfigurations d'architecture (x86, x64 et ARM) dans la boîte de dialogue **Sélectionner et configurer des packages** afin de garantir que le bon déploiement de votre application sur un large éventail d'appareils. Dans la zone de liste **Générer l'ensemble d'applications**, sélectionnez **Toujours**. Un ensemble d’applications (.appxbundle) est préférée un fichier de package d’application unique, car elle contient une collection de packages d’applications configurés pour chaque type d’architecture de processeur. Lorsque vous choisissez de générer un ensemble d'applications, celui-ci sera inclus dans le fichier (.appxupload) de chargement de package d'application final avec les informations analytiques de débogage et d'incident. Si vous ne savez pas quelle(s) architecture(s) choisir ou si vous souhaitez en savoir plus sur les architectures utilisées par divers appareils, consultez [Architectures de package d’application](https://docs.microsoft.com/windows/uwp/packaging/device-architecture).  
    ![Fenêtre Créer des packages d’application avec la configuration de package affichée](images/packaging-screen5.jpg)


7.  Incluez les fichiers de symbole PDB complet pour [Analyser les performances de l’application](https://docs.microsoft.com/windows/uwp/publish/analytics) à partir du centre de développement Windows après la publication de votre application. Configurez tous les détails supplémentaires, notamment la numérotation de version ou l'emplacement de sortie de l'application.
9.  Cliquez sur **Créer** pour générer le package d'application. Si vous avez sélectionné **Oui** à l’étape3 et si vous créez un package pour une soumission au centre de développement, l'assistant créera un fichier (.appxupload) de chargement de package. Si vous avez sélectionné **non** à l’étape3, l’assistant créera soit un package d'application unique, soit un ensemble d'applications, en fonction de vos sélections à l'étape6.
10. Une fois votre application mise en package avec succès, cette boîte de dialogue s'affiche.  
    ![Fenêtre de création de package terminée avec options de validation affichées](images/packaging-screen6.jpg)

    Validez votre application avant de l’envoyer au centre de développement en certification sur un ordinateur local ou distant. Vous pouvez uniquement valider les versions Release pour votre package d’application et non les versions Debug.

11. Pour valider localement votre application, laissez l’option **Ordinateur local** sélectionnée, puis cliquez sur **Lancer le Kit de certification des applications Windows**. Pour plus d’informations sur le test de votre application avec le Kit de certification des applications Windows, voir [Kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).

    Le Kit de certification des applications Windows effectue divers tests et renvoie les résultats. Voir [Tests du kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/mt186450) pour plus d'informations spécifiques.

    Si vous avez un appareil Windows10 distant que vous voulez utiliser pour le test, vous devrez installer manuellement le kit de certification des applications Windows sur cet appareil. La section suivante vous guidera lors de ces étapes. Une fois cette opération terminée, vous pouvez sélectionner **Remote machine**, puis cliquer sur **Launch Windows App Certification Kit** pour vous connecter à l’appareil distant et exécuter les tests de validation.

12. Une fois que le kit de certification des applications Windows a terminé et que votre application a réussi, vous êtes prêt à soumettre votre application au centre de développement. Assurez-vous de charger le fichier approprié. L'emplacement par défaut du fichier se trouve dans le dossier racine de votre solution `\[AppName]\AppPackages`. Il se termine par l’extension de fichier .appxupload. Le nom sera sous la forme `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` si vous avez opté pour un ensemble d’applications avec l’entièreté de l’architecture du package sélectionnée.

Pour plus d'informations sur la soumission de votre application au centre de développement, consultez [Soumissions d’applications](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

**Valider votre package d’application sur un appareil Windows10 distant**

1.  Activez votre appareil Windows 10 pour le développement en suivant les instructions [Activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/Dn706236).
    **Important** Vous ne pouvez pas valider votre package d’application sur un appareil ARM distant pour Windows10.
2.  Téléchargez et installez les outils de contrôle à distance de Visual Studio. Ils sont utilisés pour exécuter le kit de certification des applications Windows à distance. Vous pouvez obtenir plus d’informations sur ces outils, y compris sur l’endroit où les télécharger, en consultant [Exécuter les applications UWP sur un ordinateur distant](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor).
3.  Téléchargez le [Kit de certification des applications Windows](http://go.microsoft.com/fwlink/p/?LinkID=309666) requis, puis installez-le sur votre appareil Windows 10 distant.
4.  Sur la page **Package Creation Completed** de l’Assistant, choisissez la case d’option **Remote Machine**, puis choisissez le bouton de sélection en regard du bouton **Test Connection**.
    **Remarque** La case d’option **Remote Machine** est disponible uniquement si vous avez sélectionné au moins une configuration de solution qui prend en charge la validation. Pour plus d’informations sur le test de votre application avec le Kit de certification des applications Windows, voir [Kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).
5.  Spécifiez une forme d’appareil dans votre sous-réseau, ou fournissez le nom de serveur de nom de domaine (DNS, Domain Name System) ou l’adresse IP d’un appareil en dehors de votre sous-réseau.
6.  Dans la liste **Authentication Mode**, choisissez **None** si votre appareil ne requiert pas d’authentification avec vos informations d’identification Windows.
7.  Choisissez le bouton **Select**, puis le bouton **Launch Windows App Certification Kit**. Si les outils à distance sont en cours d’exécution sur cet appareil, VisualStudio s’y connecte, puis effectue les tests de validation. Consultez [Tests du kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/mt186450).

## <a name="sideload-your-app-package"></a>Charger de manière indépendante votre package d’application

Introduits dans la mise à jour anniversaire Windows10, les packages d’application peuvent être installés simplement en double-cliquant sur le fichier du package d’application. Pour cela, accédez à votre package d’application ou un fichier d’application un ensemble d’applications et double-cliquez dessus. Le programme d’installation de l’application se lance et fournit les informations essentielles de l’application, ainsi qu'un bouton Installer, une barre de progression de l’installation et des messages d’erreur appropriés. 

![Affichage d'un programme d’installation d'application pour l’installation d’un exemple d’application appelée Contoso](images/appinstaller-screen.png)

> [!NOTE]
> Le programme d’installation de l’application suppose que l’application est approuvée par l’appareil. Si vous chargez de manière indépendante une application de développeur ou d’entreprise, vous devez installer le certificat de signature dans le magasin de personnes autorisées ou d'éditeurs autorisés sur l'appareil. Si vous ne savez pas comment procéder, voir [Installation de certificats de test](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates).

### <a name="sideload-your-app-on-previous-versions-of-windows"></a>Charger de manière indépendante votre application sur des versions précédentes de Windows
Avec des packages d’application UWP, les applications ne sont pas installées sur un appareil comme le sont les applications de bureau. En règle générale, vous téléchargez les applications UWP à partir du Microsoft Store, ce qui installe également l’application sur votre appareil automatiquement. Les applications peuvent être installées sans être publiées dans le Store (chargement indépendant). Cela vous permet d’installer et les applications de test à l’aide du package d’application fichier que vous avez créé. Si vous disposez d’une application que vous ne voulez pas vendre dans le Store (une application métier par exemple), vous pouvez charger cette application de manière indépendante pour que les autres utilisateurs de votre société puissent l’utiliser.

La liste suivante fournit les conditions requises pour le chargement indépendant de votre application.

-   Vous devez [activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/Dn706236).
-   Pour charger votre application de manière indépendante sur un appareil Windows10 Mobile, utilisez l’outil [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md).

**Charger une application de manière indépendante sur un ordinateur de bureau, un ordinateur portable ou une tablette**

1.  Copiez les dossiers pour la version de l'application à installer sur l’appareil cible.

    Si vous avez créé un ensemble d’applications, vous aurez un dossier en fonction du numéro de version et un dossier `*_Test`. Par exemple, ces deux dossiers (dans lesquels la version à installer est 1.0.2.0):

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    Si vous n’avez pas d’ensemble d’applications, copiez le dossier pour l’architecture appropriée et son dossier `*_Test` correspondant. Ces deux dossiers sont un exemple de package d’application avec l'architecture x64 et son dossier `*_Test`:

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  Sur l’appareil cible, ouvrez le dossier `*_Test`.
3.  Cliquez avec le bouton droit sur le fichier **Add-AppDevPackage.ps1**. Choisissez **Exécuter avec PowerShell** et suivez les invites.  
    ![Explorateur de fichiers affichant le script PowerShell](images/packaging-screen7.jpg)

    Une fois le package d’application installé, la fenêtre PowerShell affiche le message suivant: **Votre application a été correctement installée**.

    **Conseil**: pour ouvrir le menu contextuel sur une tablette, touchez l’écran à l’endroit où vous désirez cliquer avec le bouton droit, restez appuyé jusqu’à ce qu’apparaisse un cercle complet, puis levez le doigt. Le menu contextuel s’ouvre quand vous levez le doigt.
4.  Cliquez sur le bouton Démarrer pour rechercher l'application par son nom, puis lancez-la.
