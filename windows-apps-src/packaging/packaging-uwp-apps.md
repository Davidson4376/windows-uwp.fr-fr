---
author: laurenhughes
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: "Création de packages d’application UWP"
description: "Pour distribuer ou vendre votre application de plateforme Windows universelle (UWP), vous devez créer un package d’application pour elle."
ms.author: lahugh
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: c6aa08c8c2e62bfed388000944de5520cbe58501
ms.sourcegitcommit: 63c815f8c6665872987b5410cabf324f2b7e3c7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/10/2017
---
# <a name="package-a-uwp-app-with-visual-studio"></a>Créer un package d’application UWP avec Visual Studio

\[ Mise à jour pour les applicationsUWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Pour vendre ou distribuer votre application de plateforme Windows universelle (UWP) à d’autres utilisateurs, vous devez créer un package d’application UWP pour elle. Si vous ne souhaitez pas distribuer votre application via le Windows Store, vous pouvez charger le package d’application directement sur un appareil. Cet article décrit le processus de configuration, de création et de test d’un package d’application UWP à l'aide de Visual Studio. Pour plus d’informations sur le chargement indépendant d'applications d'entreprise et métier, voir [Chargement indépendant d’applications métier dans Windows10](https://docs.microsoft.com/windows/application-management/sideload-apps-in-windows-10).

Dans Windows10, vous pouvez télécharger un package d'application (.appx), un ensemble d’applications (.appxbundle) ou un fichier de téléchargement complet (.appxupload). Le fichier .appxupload est un dossier qui contient à la fois le package ou l'ensemble d’applications et les autres informations importantes de débogage. Il est vivement recommandé de soumettre un fichier .appxupload. Une fois que votre package d’application a été soumis au Windows Store, votre application est disponible pour être installée et exécutée sur n’importe quel appareil Windows10. 

Voici un aperçu des étapes nécessaires pour préparer et créer un package d’application.

1.  [Avant de créer un package pour votre application](#before-packaging-your-app). Suivez ces étapes pour vous assurer que votre application est prête à être empaquetée pour la soumission au Windows Store.
2.  [Configurer un package d’application](#configure-an-app-package). Utilisez le concepteur de manifeste pour configurer le package. Par exemple, ajoutez des images de vignette et choisissez les orientations prises en charge par votre application.
3.  [Créer un package d’application](#create-an-app-package). Utilisez l’Assistant de package d’application Visual Studio pour créer un package d’application. Puis certifiez votre package à l'aide du Kit de certification des applications Windows.
4.  [Chargez de manière indépendante votre package d’application](#sideload-your-app-package). Après le chargement indépendant de votre application sur un appareil, vous pouvez tester qu’elle fonctionne correctement.

Après avoir effectué les étapes ci-dessus, vous êtes prêt à distribuer votre application dans le Windows Store. Si vous avez une application métier que vous ne prévoyez pas de vendre puisqu’elle est destinée aux utilisateurs internes uniquement, vous pouvez charger de manière indépendante cette application pour l’installer sur tout appareil Windows10.

## <a name="before-packaging-your-app"></a>Avant de créer un package pour votre application

1.  **Testez votre application.** Avant de créer un package pour votre application pour son envoi au Windows Store, assurez-vous que tout fonctionne comme prévu sur toutes les familles d’appareils que vous envisagez de prendre en charge. Ces familles d’appareils peuvent inclure des ordinateurs de bureau, des appareils portables, Surface Hub, Xbox, IoT, ou autres.
2.  **Optimisez votre application.** Vous pouvez utiliser les outils de profilage et de débogage de Visual Studio pour optimiser les performances de votre application UWP. Par exemple, l’outil Chronologie pour la réactivité de l’interface utilisateur, l’outil Utilisation de la mémoire, l’outil Utilisation du processeur, etc. Pour plus d’informations sur ces outils, voir [Exécuter les outils de diagnostic sans débogage](https://msdn.microsoft.com/library/dn957936.aspx).
3.  **Vérifiez la compatibilité de .NET Native (pour les applications Visual Basic et C#).** Dans la plateforme Windows universelle, un nouveau compilateur natif améliore les performances d’exécution de votre application. Avec cette modification, il est fortement recommandé de tester votre application dans cet environnement de compilation. Par défaut, la configuration du build **Release** active la chaîne d’outils .NET Native. Il est donc important de tester votre application avec cette configuration **Release** et de vérifier que votre application se comporte comme prévu. Certains problèmes courants de débogage qui peuvent se produire avec .NET Native sont expliqués plus en détail dans [Débogage des applications universelles Windows .NET Native](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx).

## <a name="configure-an-app-package"></a>Configurer un package d’application

Le fichier manifeste de l’application (Package.appxmanifest.xml) est un fichier XML qui contient les propriétés et les paramètres nécessaires pour créer votre package d’application. Par exemple, les propriétés dans le fichier manifeste décrivent l’image à utiliser en tant que vignette de votre application et les orientations prises en charge par votre application quand un utilisateur fait pivoter l’appareil.

Le Concepteur de manifeste de Visual Studio vous permet de mettre à jour le fichier manifeste sans modifier le code XML brut du fichier.

**Configurer un package avec le concepteur du manifeste**

1.  Dans l’**Explorateur de solutions**, développez le nœud de projet de votre application UWP.
2.  Double-cliquez sur le fichier **Package.appxmanifest**. Si le fichier manifeste est déjà ouvert dans le mode code XML, Visual Studio vous invite à fermer le fichier.
3.  Vous pouvez maintenant décider comment configurer votre application. Chaque onglet contient des informations que vous pouvez configurer pour votre application ainsi que des liens vers des informations supplémentaires si nécessaire.<br/>
    ![Concepteur de manifeste dans Visual Studio](images/packaging-screen1.jpg)

    Vérifiez que vous avez toutes les images requises pour une application UWP dans l’onglet **Actifs visuels**.

    À partir de l’onglet **Packaging**, vous pouvez entrer des données de publication. C’est ici que vous pouvez choisir le certificat à utiliser pour signer votre application. Toutes les applications UWP doivent être signées avec un certificat. Pour charger un package d’application de manière indépendante, vous devez approuver le package. Le certificat doit être installé sur cet appareil pour approuver le package. Pour plus d’informations sur le chargement indépendant, voir [Activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Enregistrez votre fichier une fois que vous avez apporté les modifications nécessaires pour votre application.

Visual Studio peut associer votre package au Windows Store. Lors de cette opération, certains champs dans l’onglet Packages du concepteur de manifeste sont automatiquement mis à jour.

## <a name="create-an-app-package"></a>Créer un package d’application

Pour distribuer une application via le Windows Store, vous devez créer un package d'application (.appx), un ensemble d’applications (.appxbundle) ou un package de chargement (.appxupload). Pour ce faire, utilisez l’Assistant **Créer des packages d’application**. Suivez ces étapes pour créer un package approprié pour une soumission au Windows Store à l'aide de Visual Studio.

**Pour créer votre package d’application**

1.  Dans l’**Explorateur de solutions**, ouvrez la solution pour votre projet d’application UWP.
2.  Cliquez avec le bouton droit sur le projet et choisissez **Store**->**Create App Packages**. Si cette option est désactivée ou n’apparaît pas, vérifiez que le projet est bien un projet UWP.<br/>
    ![Menu contextuel avec navigation vers Créer des packages d’application](images/packaging-screen2.jpg)

    L’assistant **Créer des packages d’application** s’affiche.

3.  Sélectionnez «Oui» dans la première boîte de dialogue demandant si vous voulez générer des packages à charger dans le WindowsStore, puis cliquez sur «Suivant».<br/>
    ![Fenêtre Créer vos packages affichée](images/packaging-screen3.jpg)

    Si vous choisissez «Non», Visual Studio ne génère pas le package .appxupload requis pour l’envoi au WindowsStore. Si vous souhaitez uniquement charger de manière indépendante votre application pour l’exécuter sur des appareils internes, vous pouvez sélectionner cette option. Pour plus d’informations sur le chargement indépendant, voir [Activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Connectez-vous au tableau de bord du Centre de développement Windows à l’aide de votre compte de développeur. (Si vous ne disposez pas encore d’un compte de développeur, l’Assistant vous aidera à en créer un.)
5.  Sélectionnez le nom d’application de votre package ou réservez-en un nouveau sur le portail du Centre de développement Windows, si ce n’est déjà fait.<br/>
    ![Fenêtre Créer des packages d’application avec la sélection de nom d’application affichée](images/packaging-screen4.jpg)
6.  Veillez à sélectionner les troisconfigurations d’architecture (x86, x64 et ARM) dans la boîte de dialogue **Sélectionner et configurer des packages**. De cette façon, votre application pourra être déployée sur une large gamme d’appareils. Dans la zone de liste **Générer le lot d'applications**, sélectionnez **Toujours**. Cela permet de simplifier le processus de soumission au Windows Store, car vous n’aurez qu’un seul fichier à charger (.appxupload). L’ensemble d’applications contient tous les packages nécessaires à déployer sur les appareils avec chaque architecture de processeur, ainsi que des informations importantes sur l'analyse des incidents et le débogage. Pour en savoir plus sur les architectures de package utilisées par les différents appareils, voir [Architectures de package d’application](https://docs.microsoft.com/windows/uwp/packaging/device-architecture).<br/>
    ![Fenêtre Créer des packages d’application avec la configuration de package affichée](images/packaging-screen5.jpg)
7.  Il est vivement recommandé d’inclure les fichiers de symboles PDB complets pour optimiser l’expérience d’[analyse des incidents](http://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/) à partir du Centre de développement Windows. Vous pouvez en apprendre davantage sur le débogage avec des symboles en consultant [Débogage avec des symboles](https://msdn.microsoft.com/library/windows/desktop/Ee416588).
8.  Vous pouvez maintenant configurer les détails pour créer votre package. Lorsque vous êtes prêt à publier votre application, vous allez charger les packages à partir de l’emplacement de sortie.
9.  Cliquez sur **Create** pour générer votre package appxupload.
10. Vous voyez maintenant cette boîte de dialogue.<br/>
    ![Fenêtre de création de package terminée avec options de validation affichées](images/packaging-screen6.jpg)

    Validez votre application avant de l’envoyer au WindowsStore en certification sur un ordinateur local ou distant. Vous pouvez uniquement valider les versions Release pour votre package d’application et non les versions Debug.

11. Pour valider localement, laissez l’option **Ordinateur local** sélectionnée, puis cliquez sur **Lancer le Kit de certification des applications Windows**. Pour plus d’informations sur le test de votre application avec le Kit de certification des applications Windows, voir [Kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).

    Le Kit de certification des applications Windows effectue divers tests et renvoie les résultats. Voir [Tests du kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/mt186450) pour plus d'informations spécifiques.

    Si vous avez un appareil Windows10 distant que vous voulez utiliser pour le test, vous devrez installer manuellement le kit de certification des applications Windows sur cet appareil. La section suivante vous guidera lors de ces étapes. Une fois cette opération terminée, vous pouvez sélectionner **Remote machine**, puis cliquer sur **Launch Windows App Certification Kit** pour vous connecter à l’appareil distant et exécuter les tests de validation.

12. Une fois que le Kit de certification des applications Windows a terminé et que votre application a réussi, vous êtes prêt à soumettre votre application au Windows Store. Assurez-vous de charger le fichier approprié. Vous le trouverez dans le dossier racine de votre solution \\\[AppName\]\\AppPackages. Il se termine par l’extension de fichier .appxupload (ou l’extension.appx/.appxbundle, si c’est ce que vous utilisez). Le nom sera sous la forme \[AppName\]\_\[AppVersion\]\_x86\_x64\_arm\_bundle.appxupload.

**Valider votre package d’application sur un appareil Windows10 distant**

1.  Activez votre appareil Windows 10 pour le développement en suivant les instructions [Activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/Dn706236).
    **Important** Vous ne pouvez pas valider votre package d’application sur un appareil ARM distant pour Windows10.
2.  Téléchargez et installez les outils de contrôle à distance de Visual Studio. Ils sont utilisés pour exécuter le kit de certification des applications Windows à distance. Vous pouvez obtenir plus d’informations sur ces outils, y compris sur l’endroit où les télécharger, en consultant [Exécuter les applications du Windows Store sur un ordinateur distant](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor).
3.  Téléchargez le [Kit de certification des applications Windows](http://go.microsoft.com/fwlink/p/?LinkID=309666) requis, puis installez-le sur votre appareil Windows 10 distant.
4.  Sur la page **Package Creation Completed** de l’Assistant, choisissez la case d’option **Remote Machine**, puis choisissez le bouton de sélection en regard du bouton **Test Connection**.
    **Remarque** La case d’option **Remote Machine** est disponible uniquement si vous avez sélectionné au moins une configuration de solution qui prend en charge la validation. Pour plus d’informations sur le test de votre application avec le Kit de certification des applications Windows, voir [Kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).
5.  Spécifiez une forme d’appareil dans votre sous-réseau, ou fournissez le nom de serveur de nom de domaine (DNS, Domain Name System) ou l’adresse IP d’un appareil en dehors de votre sous-réseau.
6.  Dans la liste **Authentication Mode**, choisissez **None** si votre appareil ne requiert pas d’authentification avec vos informations d’identification Windows.
7.  Choisissez le bouton **Select**, puis le bouton **Launch Windows App Certification Kit**. Si les outils à distance sont en cours d’exécution sur cet appareil, Visual Studio s’y connecte, puis effectue les tests de validation. Voir [Tests du kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/mt186450).

## <a name="sideload-your-app-package"></a>Charger de manière indépendante votre package d’application

Introduits dans la mise à jour anniversaire Windows10, les packages d’application peuvent être installés simplement en double-cliquant sur le fichier du package d’application. Pour cela, accédez à votre package application (.appx) ou au fichier de l'ensemble d’applications (.appxbundle) et double-cliquez dessus. Le programme d’installation de l’application se lance et fournit les informations essentielles de l’application, ainsi qu'un bouton Installer, une barre de progression de l’installation et des messages d’erreur appropriés. 

![Affichage d'un programme d’installation d'application pour l’installation d’un exemple d’application appelée Contoso](images/appinstaller-screen.png)

> [!NOTE]
> Le programme d’installation de l’application suppose que l’application est approuvée par l’appareil. Si vous chargez de manière indépendante une application de développeur ou d’entreprise, vous devez installer le certificat de signature dans le magasin d’Autorités de certification racines de confiance sur l’appareil. Si vous ne savez pas comment procéder, voir [Installation de certificats de test](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates).

### <a name="sideload-your-app-on-previous-versions-of-windows"></a>Charger de manière indépendante votre application sur des versions précédentes de Windows
Avec des packages d’application UWP, les applications ne sont pas installées sur un appareil comme le sont les applications de bureau. En règle générale, vous téléchargez les applications UWP à partir du Windows Store, ce qui installe également l’application sur votre appareil automatiquement. Les applications peuvent être installées sans être soumises au Windows Store (chargement indépendant). Cela vous permet de les installer et de les tester en utilisant le package d’application (.appx) que vous avez créé. Si vous disposez d’une application que vous ne voulez pas vendre dans le Windows Store (une application métier par exemple), vous pouvez charger cette application de manière indépendante pour que les autres utilisateurs de votre société puissent l’utiliser.

La liste suivante fournit les conditions requises pour le chargement indépendant de votre application.

-   Vous devez [activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/Dn706236).
-   Pour charger votre application de manière indépendante sur un appareil Windows10 Mobile, utilisez l’outil [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md).

**Charger une application de manière indépendante sur un ordinateur de bureau, un ordinateur portable ou une tablette**

1.  Copiez les dossiers pour la version de l'application à installer sur l’appareil cible.

    Si vous avez créé un ensemble d’applications, vous aurez un dossier en fonction du numéro de version et un dossier `*\_Test`. Par exemple, ces deux dossiers (dans lesquels la version à installer est 1.0.2.0):

    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0`
    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test`

    Si vous n’avez pas d’ensemble d’applications, copiez le dossier pour l’architecture appropriée et son dossier `*\_Test` correspondant. Ces deux dossiers sont un exemple de package d’application avec l'architecture x64 et son dossier `*\_Test`:

    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64`
    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64\_Test`

2.  Sur l’appareil cible, ouvrez le dossier `*\_Test`.
3.  Cliquez avec le bouton droit sur le fichier **Add-AppDevPackage.ps1**. Choisissez **Exécuter avec PowerShell** et suivez les invites.<br/>
    ![Explorateur de fichiers affichant le script PowerShell](images/packaging-screen7.jpg)

    Une fois le package d’application installé, la fenêtre PowerShell affiche le message suivant: **Votre application a été correctement installée**.

    **Conseil**: pour ouvrir le menu contextuel sur une tablette, touchez l’écran à l’endroit où vous désirez cliquer avec le bouton droit, restez appuyé jusqu’à ce qu’apparaisse un cercle complet, puis levez le doigt. Le menu contextuel s’ouvre quand vous levez le doigt.
4.  Cliquez sur le bouton Démarrer pour rechercher l'application par son nom, puis lancez-la.
