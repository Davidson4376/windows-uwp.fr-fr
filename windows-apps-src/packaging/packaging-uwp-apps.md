---
author: msatranjr
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: "Création de packages d’application UWP"
description: "Pour vendre ou distribuer votre application UWP à d’autres utilisateurs, vous devez créer un package d’application appxupload."
translationtype: Human Translation
ms.sourcegitcommit: 68081887e16801cd28726a2a33fb7993edf71e89
ms.openlocfilehash: e274557883071c65313893ce725cc2307856174b

---
# Création de packages d’application UWP

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Pour vendre ou distribuer votre application UWP à d’autres utilisateurs, vous devez créer un package d’application appxupload. Lorsque vous créez l’appxupload, un autre package appx est alors généré pour le test et le chargement indépendant. Vous pouvez distribuer votre application directement en chargeant de manière indépendante le package appx sur un appareil. Cet article décrit le processus de configuration, de création et de test d’un package d’application UWP. Pour plus d’informations sur le chargement indépendant, voir [Charger de manière indépendante des applications dans Windows10](https://technet.microsoft.com/library/mt269549.aspx).

Pour Windows10, vous générez un package (.appxupload) qui peut être chargé sur le Windows Store. Votre application est ensuite disponible pour être installée et exécutée sur tout appareil Windows 10. Voici les étapes pour créer un package d’application.

1.  [Avant de créer un package pour votre application](#before-packaging-your-app). Suivez ces étapes pour vous assurer que votre application est prête à être placée dans un package pour son envoi au Windows Store.
2.  [Configurer un package d’application.](#configure-an-app-package) Utilisez le concepteur de manifeste pour configurer le package. Par exemple, ajoutez des images de vignette et choisissez les orientations prises en charge par votre application.
3.  [Créer un package d’application](#create-an-app-package). Utilisez l’Assistant dans Microsoft Visual Studio pour créer un package d’application, puis certifiez votre package à l’aide du kit de certification des applications Windows.
4.  [Chargez de manière indépendante votre package d’application](#sideload-your-app-package). Après le chargement de manière indépendante de votre application sur un appareil, vous pouvez tester qu’elle fonctionne correctement.

Après avoir effectué les étapes ci-dessus, vous êtes prêt à vendre votre application dans le Windows Store. Si vous avez une application métier que vous ne prévoyez pas de vendre puisqu’elle est destinée aux utilisateurs internes uniquement, vous pouvez charger de manière indépendante cette application pour l’installer sur tout appareil Windows 10.

## Avant de créer un package pour votre application

1.  Testez votre application. Avant de créer un package pour votre application pour son envoi au Windows Store, assurez-vous que tout fonctionne comme prévu sur toutes les familles d’appareils que vous envisagez de prendre en charge. Ces familles d’appareils peuvent inclure des ordinateurs de bureau, des appareils portables, Surface Hub, XBOX, IoT, ou autres.
2.  Optimisez votre application. Vous pouvez utiliser les outils de profilage et de débogage de Visual Studio pour optimiser les performances de votre application UWP. Par exemple, l’outil Chronologie pour la réactivité de l’interface utilisateur, l’outil Utilisation de la mémoire, l’outil Utilisation du processeur, etc. Pour plus d’informations sur ces outils, voir [Exécuter les outils de diagnostic sans débogage](https://msdn.microsoft.com/library/dn957936.aspx).
3.  Vérifiez la compatibilité de .NET Native (pour les applications Visual Basic et C#). Avec UWP, il existe désormais un nouveau compilateur natif qui améliore les performances d’exécution de votre application. Avec cette modification, il est fortement recommandé de tester votre application dans cet environnement de compilation. Par défaut, la configuration du build **Release** active la chaîne d’outils .NET Native. Il est donc important de tester votre application avec cette configuration **Release** et de vérifier que votre application se comporte comme prévu. Certains problèmes courants de débogage qui peuvent se produire avec .NET Native sont expliqués plus en détail [ici](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx).

## Configurer un package d’application

Le fichier manifeste de l’application (package.appxmanifest.xml) possède les propriétés et les paramètres qui sont nécessaires pour créer votre package d’application. Par exemple, les propriétés dans le fichier manifeste décrivent l’image à utiliser en tant que vignette de votre application et les orientations prises en charge par votre application quand un utilisateur fait pivoter l’appareil.

Visual Studio dispose d’un concepteur de manifeste qui facilite la mise à jour du fichier manifeste sans modifier le code XML brut du fichier.

Visual Studio peut associer votre package au Windows Store. Lors de cette opération, certains champs dans l’onglet Packages du concepteur de manifeste sont automatiquement mis à jour.

**Configurer un package avec le concepteur du manifeste**

1.  Dans l’**Explorateur de solutions**, développez le nœud de projet de votre application UWP.
2.  Double-cliquez sur le fichier **Package.appxmanifest**. Si le fichier manifeste est déjà ouvert dans le mode code XML, Visual Studio vous invite à fermer le fichier.
3.  Vous pouvez maintenant décider comment configurer votre application. Chaque onglet contient des informations que vous pouvez configurer pour votre application et des liens vers des informations supplémentaires si nécessaire.<br/>
    ![](images/packaging-screen1.jpg)

    Vérifiez que vous disposez de toutes les images requises pour une application UWP dans l’onglet **Visual Assets**.

    À partir de l’onglet **Packaging**, vous pouvez entrer des données de publication. C’est ici que vous pouvez choisir le certificat à utiliser pour signer votre application. Toutes les applications UWP doivent être signées avec un certificat. Pour charger un package d’application de manière indépendante, vous devez approuver le package. Le certificat doit être installé sur cet appareil pour approuver le package. Pour plus d’informations sur le chargement indépendant, voir [Activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Enregistrez votre fichier une fois que vous avez apporté les modifications nécessaires pour votre application.

## Créer un package d’application

Pour distribuer une application via le Windows Store, vous devez créer un package appxupload. Pour ce faire, utilisez l’Assistant **Create App Packages**. Suivez ces étapes pour créer un package approprié pour un envoi vers le Windows Store avec Microsoft Visual Studio2015.

**Pour créer votre package d’application**

1.  Dans l’**Explorateur de solutions**, ouvrez la solution pour votre projet d’application UWP.
2.  Cliquez avec le bouton droit sur le projet et choisissez **Store**->**Create App Packages**. Si cette option est désactivée ou n’apparaît pas du tout, vérifiez que le projet est bien un projet UWP.<br/>
    ![](images/packaging-screen2.jpg)

    L’Assistant **Create App Packages** s’affiche.

3.  Sélectionnez «Oui» dans la première boîte de dialogue demandant si vous voulez générer des packages à charger dans le Windows Store, puis cliquez sur Suivant.<br/>
    ![](images/packaging-screen3.jpg)

    Si vous choisissez «Non», Visual Studio ne générera pas le package .appxupload requis pour l’envoi au Windows Store. Si vous souhaitez uniquement charger de manière indépendante votre application pour l’exécuter sur des appareils internes, vous pouvez sélectionner cette option. Pour plus d’informations sur le chargement indépendant, voir [Activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Connectez-vous au tableau de bord du Centre de développement Windows à l’aide de votre compte de développeur. (Si vous ne disposez pas encore d’un compte de développeur, l’Assistant vous aidera à en créer un.)
5.  Sélectionnez le nom de l’application pour votre package, ou réservez-en un nouveau avec le portail du Centre de développement Windows, si vous ne l’avez pas déjà fait.<br/>
    ![](images/packaging-screen4.jpg)
6.  Assurez-vous de sélectionner les trois configurations d’architecture (x86, x64 et ARM) dans la boîte de dialogue **Select and Configure Packages**. De cette façon, votre application pourra être déployée sur une large gamme d’appareils. Dans la zone de liste **Generate app bundle**, sélectionnez **Always**. Cela permet de simplifier le processus d’envoi au Windows Store, car vous n’aurez qu’un seul fichier à charger(.appxupload). L’ensemble d’applications contient tous les packages nécessaires pour le déploiement sur des appareils avec chaque architecture de processeur.<br/>
    ![](images/packaging-screen5.jpg)
7.  Il est conseillé d’inclure les fichiers de symboles PDB complets pour la meilleure expérience [d’analyse des incidents](http://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/) du Centre de développement Windows. Vous pouvez en apprendre davantage sur le débogage avec des symboles en consultant [Débogage avec des symboles](https://msdn.microsoft.com/library/windows/desktop/Ee416588).
8.  Vous pouvez maintenant configurer les détails pour créer votre package. Lorsque vous êtes prêt à publier votre application, vous allez charger les packages à partir de l’emplacement de sortie.
9.  Cliquez sur **Create** pour générer votre package appxupload.
10. Vous voyez maintenant cette boîte de dialogue.<br/>
    ![](images/packaging-screen6.jpg)

    Validez votre application avant de l’envoyer au Windows Store pour certification sur un ordinateur local ou distant. (Vous pouvez uniquement valider les versions Release pour votre package d’application et non les versions Debug.)

11. Pour valider localement, laissez l’option **Local machine** sélectionnée, puis cliquez sur **Launch Windows App Certification Kit**. Pour plus d’informations sur le test de votre application avec le Kit de certification des applications Windows, voir [Kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).

    Le Kit de certification des applications Windows effectue des tests et vous présente les résultats. Voir [Tests du kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/mt186450).

    Si vous avez un appareil Windows 10 distant que vous voulez utiliser pour le test, vous devrez installer manuellement le kit de certification des applications Windows sur cet appareil. La section suivante vous guidera lors de ces étapes. Une fois cette opération terminée, vous pouvez sélectionner **Remote machine**, puis cliquer sur **Launch Windows App Certification Kit** pour vous connecter à l’appareil distant et exécuter les tests de validation.

12. Une fois que le Kit de certification des applications Windows a terminé et que votre application a réussi, vous êtes prêt à charger le fichier dans le Windows Store. Assurez-vous de charger le fichier approprié. Vous le trouverez dans le dossier racine de votre solution \\[AppName]\\AppPackages. Il se termine par l’extension de fichier .appxupload. Le nom sera sous la forme \[AppName\]\_\[AppVersion\]\_x86\_x64\_arm\_bundle.appxupload.

**Valider votre package d’application sur un appareil Windows10 distant**

1.  Activez votre appareil Windows 10 pour le développement en suivant les instructions [Activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/Dn706236).
    **Important** Vous ne pouvez pas valider votre package d’application sur un appareil ARM distant pour Windows 10.
2.  Téléchargez et installez les outils de contrôle à distance de Visual Studio. Ils sont utilisés pour exécuter le kit de certification des applications Windows à distance. Vous pouvez obtenir plus d’informations sur ces outils, y compris sur l’endroit où les télécharger, en consultant [Exécuter les applications du Windows Store sur un ordinateur distant](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor).
3.  Téléchargez le [Kit de certification des applications Windows](http://go.microsoft.com/fwlink/p/?LinkID=309666) requis, puis installez-le sur votre appareil Windows 10 distant.
4.  Sur la page **Package Creation Completed** de l’Assistant, choisissez la case d’option **Remote Machine**, puis choisissez le bouton de sélection en regard du bouton **Test Connection**.
    **Remarque** La case d’option **Remote Machine** est disponible uniquement si vous avez sélectionné au moins une configuration de solution qui prend en charge la validation. Pour plus d’informations sur le test de votre application avec le Kit de certification des applications Windows, voir [Kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).
5.  Spécifiez une forme d’appareil dans votre sous-réseau, ou fournissez le nom de serveur de nom de domaine (DNS, Domain Name System) ou l’adresse IP d’un appareil en dehors de votre sous-réseau.
6.  Dans la liste **Authentication Mode**, choisissez **None** si votre appareil ne requiert pas d’authentification avec vos informations d’identification Windows.
7.  Choisissez le bouton **Select**, puis le bouton **Launch Windows App Certification Kit**. Si les outils à distance sont en cours d’exécution sur cet appareil, Visual Studio s’y connecte, puis effectue les tests de validation. Voir [Tests du kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/mt186450).

## Charger de manière indépendante votre package d’application

Avec des packages d’application UWP, vous ne pouvez pas simplement installer une application sur votre appareil comme c’est le cas pour les applications de bureau. En règle générale, vous téléchargez ces applications à partir du Windows Store et c’est de cette manière qu’elles sont installées sur votre appareil. Toutefois, vous pouvez charger des applications de manière indépendante sur votre appareil sans les envoyer au Windows Store. Cela vous permet de les installer et de les tester en utilisant le package d’application (.appx) que vous avez créé. Si vous disposez d’une application que vous ne voulez pas vendre dans le Windows Store (une application métier par exemple), vous pouvez charger cette application de manière indépendante pour que les autres utilisateurs de votre société puissent l’utiliser.

La liste suivante fournit les conditions requises pour le chargement indépendant de votre application.

-   Vous devez [activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/Dn706236).
-   Pour charger votre application de manière indépendante sur un appareil Windows 10 Mobile, vous devez utiliser l’outil [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md).

**Charger une application de manière indépendante sur un ordinateur de bureau, un ordinateur portable ou une tablette**

1.  Copiez les dossiers pour la version que vous souhaitez installer sur l’appareil cible.

    Si vous avez créé un ensemble d’applications, vous aurez un dossier en fonction du numéro de version et un dossier \_test. Par exemple, ces deux dossiers (dans lesquels la version à installer est 1.0.2):

    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0
    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test

    Si vous n’avez pas d’ensemble d’applications, vous pouvez simplement copier le dossier pour l’architecture appropriée et le dossier de test correspondant. Par exemple, ces deux dossiers.

    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64
    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64\_Test
2.  Sur l’appareil cible, ouvrez le dossier test. Par exemple, C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test
3.  Cliquez avec le bouton droit sur le fichier **Add-AppDevPackage.ps1**, puis choisissez **Run with PowerShell** et suivez les invites.<br/>
    ![](images/packaging-screen7.jpg)

    Une fois que le package d’application est installé, vous verrez ce message dans votre fenêtre PowerShell: votre application a été correctement installée.

    **Remarque** Pour ouvrir le menu contextuel sur une tablette, touchez l’écran dans lequel vous voulez cliquer avec le bouton droit, restez appuyé jusqu’à ce qu’apparaisse un cercle complet, puis levez le doigt. Le menu contextuel s’affiche quand vous levez le doigt.
4.  Cliquez sur le bouton Démarrer et tapez le nom de votre application pour la lancer.

 

 







<!--HONumber=Aug16_HO5-->


