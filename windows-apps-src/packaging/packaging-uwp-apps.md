---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Création de packages d’application UWP
description: Pour distribuer ou vendre votre application de plateforme Windows universelle (UWP), vous devez créer un package d’application pour elle.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: 1038913732c8b25a5baa3daf2c04176ff747a85f
ms.sourcegitcommit: 2062d06567ef087ad73507a03ecc726a7d848361
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303594"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>Package d’une application UWP avec Visual Studio

Pour vendre ou distribuer votre application de plateforme Windows universelle (UWP) à d’autres utilisateurs, vous devez la mettre en package. Si vous ne souhaitez pas distribuer votre application via le Microsoft Store, vous pouvez charger le package d’application directement sur un appareil ou le distribuer via une [Installation web](installing-UWP-apps-web.md). Cet article décrit le processus de configuration, de création et de test d’un package d’application UWP à l'aide de Visual Studio. Pour plus d’informations sur la gestion et déploiement d'applications cœur de métier (LOB), consultez [Gestion d'applications d'entreprise](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management).

Dans Windows 10, vous pouvez envoyer un package d’application, un bundle d’applications ou un fichier de téléchargement de package d’application complet à l' [espace partenaires](https://partner.microsoft.com/dashboard). Parmi ces options, l’envoi d’un fichier de téléchargement de package d’application offre la meilleure expérience.

## <a name="types-of-app-packages"></a>Types de packages d’application

- **Package d’application (. AppX ou. msix)**  
    Un fichier contenant votre application dans un format pouvant être chargé de manière indépendante sur un appareil. Un fichier de package d’application unique créé par Visual Studio n’est **pas** destiné à être soumis à l’espace partenaires et doit être utilisé à des fins de chargement et de test uniquement. Si vous souhaitez soumettre votre application à l’espace partenaires, utilisez le fichier de chargement du package d’application.  

- **Bundle d’applications (. appxbundle ou. msixbundle)**  
    Un ensemble d'applications désigne un type de package pouvant contenir plusieurs packages d'application, chacun étant généré pour prendre en charge une architecture d'appareil spécifique. Par exemple, un ensemble d'applications peut contenir trois packages d'application distincts pour les configurations x86, x64 et ARM. Les ensembles d'applications doivent être générés autant que possible. En effet, ils permettent aux applications d'être disponibles à un éventail d'appareils des plus larges.  

- **Fichier de chargement du package d’application (. appxupload ou. msixupload)**  
    Un fichier unique pouvant contenir plusieurs packages d'applications ou un ensemble d'applications pour prendre en charge diverses architectures de processeur. Le fichier de téléchargement de package d’application contient également un fichier de symboles pour [analyser les performances](https://docs.microsoft.com/windows/uwp/publish/analytics) de l’application une fois que votre application a été publiée dans le Microsoft Store. Ce fichier sera automatiquement créé pour vous si vous empaquetez votre application avec Visual Studio dans le but de la soumettre à l’espace partenaires pour la publication.

Voici un aperçu des étapes nécessaires pour préparer et créer un package d’application :

1.  [Avant de créer un package pour votre application](#before-packaging-your-app). Suivez ces étapes pour vous assurer que votre application est prête à être empaquetée pour l’envoi de l’espace partenaires.
2.  [Configurer un package d’application.](#configure-an-app-package) Utilisez le concepteur de manifeste Visual Studio pour configurer le package. Par exemple, ajoutez des images de vignette et choisissez les orientations prises en charge par votre application.
3.  [Créer un fichier de chargement de package d’application](#create-an-app-package-upload-file). Utilisez l’assistant de package d'application dans Microsoft Visual Studio pour créer un package d’application, puis certifiez votre package à l’aide du kit de certification des applications Windows.
4.  [Chargez de manière indépendante votre package d’application](#sideload-your-app-package). Après le chargement indépendant de votre application sur un appareil, vous pouvez tester qu’elle fonctionne comme vous le souhaitez.

Après avoir effectué les étapes ci-dessus, vous êtes prêt à distribuer votre application. Si vous avez une application métier que vous ne prévoyez pas de vendre, car elle est réservée uniquement aux utilisateurs internes, vous pouvez chargement cette application pour l’installer sur n’importe quel appareil Windows 10.

## <a name="before-packaging-your-app"></a>Avant de créer un package pour votre application

1.  **Testez votre application.** Avant d’empaqueter votre application pour l’envoi de l’espace partenaires, assurez-vous qu’elle fonctionne comme prévu sur toutes les familles d’appareils que vous envisagez de prendre en charge. Ces familles d’appareils peuvent inclure des ordinateurs de bureau, des appareils portables, Surface Hub, Xbox, IoT, ou autres. Pour plus d’informations sur le déploiement et le test de votre application à l’aide de Visual Studio, consultez [déploiement et débogage d’applications UWP](../debug-test-perf/deploying-and-debugging-uwp-apps.md).
2.  **Optimisez votre application.** Vous pouvez utiliser les outils de profilage et de débogage de Visual Studio pour optimiser les performances de votre application UWP. Par exemple, l’outil Chronologie pour la réactivité de l’interface utilisateur, l’outil Utilisation de la mémoire, l’outil Utilisation du processeur, etc. Pour plus d'informations sur l'utilisation de ces outils, consultez la rubrique [Vue d'ensemble de la fonctionnalité de profilage](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour).
3.  **Vérifiez .NET Native compatibilité (pour les applications C# vb et).** Dans la plateforme Windows universelle, un nouveau compilateur natif améliore les performances d’exécution de votre application. Avec cette modification, vous devriez tester votre application dans cet environnement de compilation. Par défaut, la configuration de build **Release** active la chaîne d’outils .NET Native. Il est donc important de tester votre application avec cette configuration **Release** et de vérifier que votre application se comporte comme prévu. Certains problèmes courants de débogage qui peuvent se produire avec .NET Native sont expliqués plus en détail dans [Débogage des applications universelles Windows .NET Native](https://devblogs.microsoft.com/devops/debugging-net-native-windows-universal-apps/).

## <a name="configure-an-app-package"></a>Configurer un package d’application

Le fichier manifeste de l’application (Package.appxmanifest.xml) est un fichier XML qui contient les propriétés et les paramètres nécessaires pour créer votre package d’application. Par exemple, les propriétés dans le fichier manifeste d'application décrivent l’image à utiliser en tant que vignette de votre application et les orientations prises en charge par votre application quand un utilisateur fait pivoter l’appareil.

Le Concepteur de manifeste de Visual Studio vous permet de mettre à jour le fichier manifeste sans modifier le code XML brut du fichier.

### <a name="configure-a-package-with-the-manifest-designer"></a>Configurer un package avec le concepteur du manifeste

1.  Dans l’**Explorateur de solutions**, développez le nœud de projet de votre application UWP.
2.  Double-cliquez sur le fichier **Package.appxmanifest**. Si le fichier manifeste est déjà ouvert dans le mode code XML, Visual Studio vous invite à fermer le fichier.
3.  Vous pouvez maintenant décider comment configurer votre application. Chaque onglet contient des informations que vous pouvez configurer pour votre application ainsi que des liens vers des informations supplémentaires si nécessaire.  
    ![Concepteur de manifeste dans Visual Studio](images/packaging-screen1.jpg)

    Vérifiez que vous avez toutes les images requises pour une application UWP dans l’onglet **Actifs visuels**.

    À partir de l’onglet **Packaging**, vous pouvez entrer des données de publication. C’est ici que vous pouvez choisir le certificat à utiliser pour signer votre application. Toutes les applications UWP doivent être signées avec un certificat.

    > [!NOTE]
    > À compter de Visual Studio 2019, un certificat temporaire n’est plus généré dans les projets UWP. Pour créer ou exporter des certificats, utilisez les applets de commande PowerShell décrites dans [cet article](create-certificate-package-signing.md).

    > [!IMPORTANT]
    > Si vous publiez votre application dans le Microsoft Store, elle sera signée pour vous avec un certificat approuvé. Cela permet à l'utilisateur d'installer et d'exécuter votre application sans installer le certificat de signature d'application associé.

    Si vous ne publiez pas votre application et souhaitez simplement charger un package d'application de manière indépendante, vous devez d'abord approuver le package. Pour approuver le package, le certificat doit être installé sur l'appareil de l'utilisateur. Pour plus d’informations sur le chargement indépendant, voir [Activer votre appareil pour le développement](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

4.  Enregistrez votre fichier **Package.appxmanifest** une fois que vous avez apporté les modifications nécessaires pour votre application.

Si vous distribuez votre application via le Microsoft Store, Visual Studio peut associer votre package au Windows Store. Pour ce faire, cliquez avec le bouton droit sur le nom de votre projet dans Explorateur de solutions et choisissez **Store**->**associer l’application**au Windows Store. Vous pouvez également effectuer cette opération dans l’Assistant **créer des packages d’application** , qui est décrit dans la section suivante. Lorsque vous associez votre application, certains champs dans l’onglet Packages du concepteur de manifeste sont automatiquement mis à jour.

## <a name="create-an-app-package-upload-file"></a>Créer un fichier de chargement de package d’application

Pour distribuer une application via Microsoft Store vous devez créer un package d’application (. AppX ou. msix), un bundle d’applications (. appxbundle ou. msixbundle) ou un fichier de téléchargement de package d’application (. appxupload ou. msixupload) et [soumettre l’application empaquetée à l’espace partenaires](https://docs.microsoft.com/windows/uwp/publish/app-submissions). Bien qu’il soit possible d’envoyer un package d’application ou un bundle d’applications à l’espace partenaires seul, nous vous recommandons de soumettre un fichier de chargement de package d’application. Vous pouvez créer un fichier de téléchargement de package d’application à l’aide de l’Assistant **créer des packages d’application** dans Visual Studio, ou vous pouvez en créer un manuellement à partir de packages d’application ou de bundle d’applications existants.

>[!NOTE]
> Si vous souhaitez créer un package d’application (. AppX ou. msix) ou un bundle d’applications (. appxbundle ou. msixbundle) manuellement, consultez [créer un package d’application avec l’outil MakeAppx. exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="create-your-app-package-upload-file-using-visual-studio"></a>Créer un fichier de chargement de package d’application à l’aide de Visual Studio

1.  Dans l’**Explorateur de solutions**, ouvrez la solution pour votre projet d’application UWP.
2.  Cliquez avec le bouton droit sur le projet et choisissez **Store**->**Create App Packages**. Si cette option est désactivée ou n’apparaît pas, vérifiez que le projet est bien un projet Windows universel.  
    ![Menu contextuel avec navigation pour créer des packages d’application](images/packaging-screen2.jpg)

    L’assistant **Créer des packages d’application** s’affiche.

3.  Sélectionnez **je souhaite créer des packages à charger sur le Microsoft Store à l’aide d’un nouveau nom d’application** dans la première boîte de dialogue, puis cliquez sur **suivant**.  
    ![Boîte de dialogue créer vos packages affichée](images/packaging-screen3.jpg)

    Si vous avez déjà associé votre projet à une application du Windows Store, vous avez également la possibilité de créer des packages pour l’application de Store associée. Si vous choisissez de **créer des packages pour chargement**, Visual Studio ne génère pas le fichier de chargement du package d’application (. msixupload ou. appxupload) pour les soumissions de l’espace partenaires. Si vous souhaitez uniquement charger de manière indépendante votre application pour l’exécuter sur des appareils internes ou à des fins de test, vous pouvez sélectionner cette option. Pour plus d’informations sur le chargement indépendant, voir [Activer votre appareil pour le développement](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
4.  Sur la page suivante, connectez-vous avec votre compte de développeur à l’espace partenaires. Si vous ne disposez pas encore d’un compte de développeur, l’Assistant vous aidera à en créer un.
    ![Fenêtre créer des packages d’application avec la sélection du nom de l’application affichée](images/packaging-screen4.jpg)
5.  Sélectionnez le nom de l’application pour votre package dans la liste des applications actuellement inscrites à votre compte, ou réservez-en un nouveau si vous ne l’avez pas encore réservé dans l’espace partenaires.  
6.  Veillez à sélectionner les trois configurations d'architecture (x86, x64 et ARM) dans la boîte de dialogue **Sélectionner et configurer des packages** afin de garantir que le bon déploiement de votre application sur un large éventail d'appareils. Dans la zone de liste **Generate app bundle**, sélectionnez **Always**. Un bundle d’applications (. appxbundle ou. msixbundle) est préférable à un seul fichier de package d’application, car il contient une collection de packages d’application configurés pour chaque type d’architecture de processeur. Lorsque vous choisissez de générer l’offre groupée d’applications, l’offre groupée d’applications est incluse dans le fichier de téléchargement de package d’application (. appxupload ou. msixupload) final, ainsi que les informations analytiques de débogage et d’incident. Si vous ne savez pas quelle(s) architecture(s) choisir ou si vous souhaitez en savoir plus sur les architectures utilisées par divers appareils, consultez [Architectures de package d’application](https://docs.microsoft.com/windows/uwp/packaging/device-architecture).  
    ![Fenêtre créer des packages d’application avec la configuration de package affichée](images/packaging-screen5.jpg)
7.  Inclure des fichiers de symboles PDB complets pour [analyser les performances des applications](https://docs.microsoft.com/windows/uwp/publish/analytics) à partir de l’espace partenaires après la publication de votre application. Configurez tous les détails supplémentaires, notamment la numérotation de version ou l'emplacement de sortie de l'application.
8.  Cliquez sur **Créer** pour générer le package d'application. Si vous avez sélectionné l’une des options **je souhaite créer des packages à télécharger dans les Microsoft Store à l'** étape 3 et que vous créez un package pour l’envoi de l’espace partenaires, l’Assistant crée un fichier de téléchargement de package (. appxupload ou. msixupload). Si vous avez choisi de **créer des packages pour chargement** à l’étape 3, l’Assistant crée soit un package d’application unique, soit un bundle d’applications en fonction de vos sélections à l’étape 6.
9. Une fois votre application correctement empaquetée, cette boîte de dialogue s’affiche et vous pouvez récupérer le fichier de téléchargement de votre package d’application à partir de l’emplacement de sortie spécifié. À ce stade, vous pouvez [valider votre package d’application sur l’ordinateur local ou sur un ordinateur distant](#validate-your-app-package) et automatiser les soumissions du [magasin](#automate-store-submissions).
    ![Fenêtre de création de package terminée avec les options de validation affichées](images/packaging-screen6.jpg)

### <a name="create-your-app-package-upload-file-manually"></a>Créer manuellement le fichier de téléchargement de votre package d’application

1. Placez les fichiers suivants dans un dossier:
    - Un ou plusieurs packages d’application (. msix ou. AppX) ou un bundle d’applications (. msixbundle ou. appxbundle).
    - Un fichier. appxsym. Il s’agit d’un fichier. pdb compressé contenant les symboles publics de votre application utilisés pour l' [analyse des incidents](../publish/health-report.md) dans l’espace partenaires. Vous pouvez omettre ce fichier, mais si vous le faites, aucune information d’analyse ou de débogage sur incident ne sera disponible pour votre application.
2. Compressez le dossier.
3. Remplacez le nom de l’extension de dossier compressé par. msixupload ou. appxupload.

## <a name="validate-your-app-package"></a>Valider votre package d’application

Validez votre application avant de la soumettre à l’espace partenaires pour la certification sur un ordinateur local ou distant. Vous pouvez uniquement valider les versions Release pour votre package d’application et non les versions Debug. Pour plus d’informations sur l’envoi de votre application à l’espace partenaires, consultez la page envois de l' [application](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

### <a name="validate-your-app-package-locally"></a>Valider votre package d’application localement

1. Dans la page **fin** de la création du package final de l’Assistant **créer des packages d’application** , laissez l’option **ordinateur local** sélectionnée, puis cliquez sur **lancer le kit de certification des applications Windows**. Pour plus d’informations sur le test de votre application avec le Kit de certification des applications Windows, voir [Kit de certification des applications Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).

    Le kit de certification des applications Windows (WACK) effectue différents tests et retourne les résultats. Voir [Tests du kit de certification des applications Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests) pour plus d'informations spécifiques.

    Si vous disposez d’un appareil Windows 10 distant que vous souhaitez utiliser pour le test, vous devez installer manuellement le kit de certification des applications Windows sur cet appareil. La section suivante vous guidera lors de ces étapes. Une fois cette opération terminée, vous pouvez sélectionner **Remote machine**, puis cliquer sur **Launch Windows App Certification Kit** pour vous connecter à l’appareil distant et exécuter les tests de validation.

2. Une fois que WACK est terminé et que votre application a passé la certification, vous êtes prêt à soumettre votre application à l’espace partenaires. Assurez-vous de charger le fichier approprié. L’emplacement par défaut du fichier se trouve dans le dossier racine de votre solution `\[AppName]\AppPackages` et se termine par l’extension de fichier. appxupload ou. msixupload. Le nom se présente sous la forme `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` ou `[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload` si vous avez opté pour un lot d’applications avec l’ensemble de l’architecture de package sélectionnée.

### <a name="validate-your-app-package-on-a-remote-windows10-device"></a>Valider votre package d’application sur un appareil Windows 10 distant

1. Activez votre appareil Windows 10 pour le développement en suivant les instructions [activer votre appareil pour le développement](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) .
    >[!IMPORTANT]
    > Vous ne pouvez pas valider votre package d’application sur un appareil ARM distant pour Windows 10.
2. Téléchargez et installez les outils de contrôle à distance de Visual Studio. Ils sont utilisés pour exécuter le kit de certification des applications Windows à distance. Vous pouvez obtenir plus d’informations sur ces outils, y compris sur l’endroit où les télécharger, en consultant [Exécuter les applications UWP sur un ordinateur distant](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-on-a-remote-machine?view=vs-2015).
3. Téléchargez le [Kit de certification des applications Windows](https://go.microsoft.com/fwlink/p/?LinkID=309666) requis, puis installez-le sur votre appareil Windows 10 distant.
4. Sur la page **Package Creation Completed** de l’Assistant, choisissez la case d’option **Remote Machine**, puis choisissez le bouton de sélection en regard du bouton **Test Connection**.
    >[!NOTE]
    > La case d’option **ordinateur distant** est disponible uniquement si vous avez sélectionné au moins une configuration de solution qui prend en charge la validation. Pour plus d’informations sur le test de votre application avec le Kit de certification des applications Windows, voir [Kit de certification des applications Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).
5. Spécifiez une forme d’appareil dans votre sous-réseau, ou fournissez le nom de serveur de nom de domaine (DNS, Domain Name System) ou l’adresse IP d’un appareil en dehors de votre sous-réseau.
6. Dans la liste **Authentication Mode**, choisissez **None** si votre appareil ne requiert pas d’authentification avec vos informations d’identification Windows.
7. Choisissez le bouton **Select**, puis le bouton **Launch Windows App Certification Kit**. Si les outils à distance sont en cours d’exécution sur cet appareil, Visual Studio s’y connecte, puis effectue les tests de validation. Voir [Tests du kit de certification des applications Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests).

## <a name="automate-store-submissions"></a>Automatiser les soumissions du magasin

À compter de Visual Studio 2019, vous pouvez envoyer le fichier. appxupload généré au Microsoft Store directement à partir de l’IDE en sélectionnant l’option **Envoyer automatiquement au Microsoft Store après la validation du kit de certification des applications Windows** à la fin de la [Assistant Création de packages d’application](#create-your-app-package-upload-file-using-visual-studio). Cette fonctionnalité s’appuie sur Azure Active Directory pour accéder aux informations de compte de l’espace partenaires nécessaires à la publication de votre application. Pour utiliser cette fonctionnalité, vous devez associer Azure Active Directory à votre compte espace partenaires et récupérer plusieurs informations d’identification requises pour les soumissions.

### <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Associer Azure Active Directory à votre compte espace partenaires

Avant de pouvoir récupérer les informations d’identification requises pour les envois automatiques du magasin, vous devez d’abord suivre ces étapes dans le [tableau de bord espace partenaires](https://partner.microsoft.com/dashboard) , si vous ne l’avez pas déjà fait.

1. [Associez votre compte espace partenaires au Azure Active Directory de votre organisation](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center). Si votre organisation utilise déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’Azure AD. Dans le cas contraire, vous pouvez créer un locataire Azure AD à partir de l’espace partenaires sans frais supplémentaires.

2. [Ajoutez une application Azure ad à votre compte espace partenaires](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account). Cette Azure AD application représente l’application ou le service que vous allez utiliser pour accéder aux soumissions de votre compte Centre de développement. Vous devez affecter cette application au rôle **Gestionnaire** . Si cette application existe déjà dans votre annuaire Azure AD, vous pouvez la sélectionner dans la page **Ajouter des applications Azure AD** pour l’ajouter à votre compte du Centre de développement. Sinon, vous pouvez créer une application Azure AD dans la page **Ajouter des applications Azure AD**.

### <a name="retrieve-the-credentials-required-for-submissions"></a>Récupérer les informations d’identification requises pour les envois

Ensuite, vous pouvez récupérer les informations d’identification de l’espace partenaires requises pour les envois: l' **ID de locataire Azure**, l' **ID client** et la **clé cliente**.

1. Accédez au tableau de bord de l' [espace partenaires](https://partner.microsoft.com/dashboard) et connectez-vous avec vos informations d’identification de Azure ad.

2. Dans le tableau de bord de l’espace partenaires, sélectionnez l’icône d’engrenage (près de l’angle supérieur droit du tableau de bord), puis sélectionnez **paramètres du développeur**.

3. Dans le menu **paramètres** du volet gauche, cliquez sur **utilisateurs**.

4. Cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application. Dans cette page, copiez l' **ID de locataire** et les valeurs d' **ID client** .

5. Dans la section **clés** , cliquez sur **Ajouter une nouvelle clé**. Dans l’écran suivant, copiez la valeur de clé qui correspond à la **clé** secrète client. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page, veillez à ne pas les perdre. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys-for-an-azure-ad-application).

### <a name="configure-automatic-store-submissions-in-visual-studio"></a>Configurer les envois de banque automatique dans Visual Studio

Une fois les étapes précédentes terminées, vous pouvez configurer les envois de banque automatique dans Visual Studio 2019.

1. À la fin de l' [Assistant Création de packages d’application](#create-your-app-package-upload-file-using-visual-studio), sélectionnez **envoyer automatiquement au Microsoft Store après la validation du kit de certification des applications Windows** , puis cliquez sur reconfigurer.

2. Dans la boîte de dialogue **configurer les paramètres d’envoi Microsoft Store** , entrez l’ID de locataire, l’ID de client et la clé client Azure.

    ![Configurer les paramètres d’envoi Microsoft Store](images/packaging-screen8.jpg)

    > [!Important]
    > Vos informations d’identification peuvent être enregistrées dans votre profil pour être utilisées dans de futures soumissions.

3. Cliquez sur **OK**.

La soumission démarre une fois le test WACK terminé. Vous pouvez suivre la progression de l’envoi dans la fenêtre **vérifier et publier** .

![Progression de la vérification et de la publication](images/packaging-screen9.jpg)

## <a name="sideload-your-app-package"></a>Charger de manière indépendante votre package d’application

Avec les packages d’application UWP, les applications ne sont pas installées sur un appareil, comme c’est le cas avec les applications de bureau. En règle générale, vous téléchargez les applications UWP à partir du Microsoft Store, ce qui installe également l’application sur votre appareil automatiquement. Les applications peuvent être installées sans être publiées dans le Store (chargement indépendant). Cela vous permet d’installer et de tester des applications à l’aide du fichier de package d’application que vous avez créé. Si vous disposez d’une application que vous ne voulez pas vendre dans le Windows Store (une application métier par exemple), vous pouvez charger cette application de manière indépendante pour que les autres utilisateurs de votre société puissent l’utiliser.

Avant de pouvoir chargement votre application sur un appareil cible, vous devez [activer votre appareil pour le développement](../get-started/enable-your-device-for-development.md).

Pour chargement votre application sur un appareil Windows 10 mobile, utilisez l’outil [WinAppDeployCmd. exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) . Pour les ordinateurs de bureau, les ordinateurs portables et les tablettes, suivez les instructions ci-dessous.

### <a name="sideload-your-app-package-on-windows-10-anniversary-update-or-later"></a>Chargement votre package d’application sur la mise à jour anniversaire Windows 10 ou version ultérieure

Introduite dans la mise à jour anniversaire Windows 10 (Windows 10, version 1607), les packages d’application peuvent être installés simplement en double-cliquant sur le fichier de package d’application. Pour ce faire, accédez à votre fichier de package d’application ou de lot d’applications, puis double-cliquez dessus. Le [programme d’installation](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root) de l’application démarre et fournit les informations de base de l’application, ainsi qu’un bouton installer, la barre de progression de l’installation et tous les messages d’erreur pertinents.

![Affichage d'un programme d’installation d'application pour l’installation d’un exemple d’application appelée Contoso](images/appinstaller-screen.png)

> [!NOTE]
> Le programme d’installation de l’application suppose que l’application est approuvée par l’appareil. Si vous chargez de manière indépendante une application de développeur ou d’entreprise, vous devez installer le certificat de signature dans le magasin de personnes autorisées ou d'éditeurs autorisés sur l'appareil. Si vous ne savez pas comment procéder, voir [Installation de certificats de test](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates).

### <a name="sideload-your-app-package-on-previous-versions-of-windows"></a>Chargement de votre package d’application sur les versions précédentes de Windows

1.  Copiez les dossiers pour la version de l'application à installer sur l’appareil cible.

    Si vous avez créé un ensemble d’applications, vous aurez un dossier en fonction du numéro de version et un dossier `*_Test`. Par exemple, ces deux dossiers (dans lesquels la version à installer est 1.0.2.0) :

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    Si vous n’avez pas d’ensemble d’applications, copiez le dossier pour l’architecture appropriée et son dossier `*_Test` correspondant. Ces deux dossiers sont un exemple de package d’application avec l'architecture x64 et son dossier `*_Test` :

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  Sur l’appareil cible, ouvrez le dossier `*_Test`.
3.  Cliquez avec le bouton droit sur le fichier **Add-AppDevPackage.ps1**. Choisissez **Exécuter avec PowerShell** et suivez les invites.  
    ![Explorateur de fichiers accédé au script PowerShell affiché](images/packaging-screen7.jpg)

    Lorsque le package d’application a été installé, la fenêtre PowerShell affiche ce message: **Votre application a été correctement installée.**
    >[!TIP]
    > Pour ouvrir le menu contextuel sur une tablette, touchez l’écran où vous souhaitez cliquer avec le bouton droit, maintenez la touche enfoncée jusqu’à ce qu’un cercle complet apparaisse, puis soulevez le doigt. Le menu contextuel s’ouvre quand vous levez le doigt.

4.  Cliquez sur le bouton Démarrer pour rechercher l'application par son nom, puis lancez-la.
