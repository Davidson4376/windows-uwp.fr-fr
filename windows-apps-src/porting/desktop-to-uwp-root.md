---
Description: Créer un package d’application Windows moderne pour votre application ou jeu Win32, WPF ou Windows Forms existant. Ajouter des expériences utilisateur modernes pour les utilisateurs Windows 10 et simplifier le déploiement et la MONÉTISATION.
Search.Product: eADQiWindows 10XVcnh
title: Créer des packages d’application de bureau
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 70a9b6046e3b7be9ac84678ac21c0c9f89a4a7b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627824"
---
# <a name="package-desktop-applications-desktop-bridge"></a>Applications de bureau de package (pont du bureau)

Prenez votre application de bureau existante et ajouter des expériences utilisateur modernes pour les utilisateurs Windows 10. Ensuite, touchez un plus large public sur les marchés internationaux en la distribuant via le Microsoft Store. Vous pouvez MONÉTISER votre application beaucoup plus simples manières en tirant parti des fonctionnalités intégrées directement dans le magasin. Bien entendu, vous n’êtes pas obligé d’utiliser le Store. N’hésitez pas à utiliser vos canaux existants.

![Pont du bureau](images/desktop-to-uwp/desktop-bridge-4.png)

Lorsque vous créez un package pour votre application de bureau, votre application obtiendra une identité et avec cette identité, votre application de bureau a accès à Windows plateforme universelle (UWP) API. Vous pouvez les utiliser pour offrir des expériences modernes et attrayantes telles que des vignettes dynamiques et des notifications.  Utiliser la compilation conditionnelle simple et le runtime vérifie pour exécuter le code UWP uniquement lorsque votre application s’exécute sur Windows 10.

Outre le code que vous utilisez pour égayer les expériences Windows 10, votre application reste inchangée et vous pouvez continuer à distribuer à vos existant Windows 7, Windows Vista ou base d’utilisateurs Windows XP. Sur Windows 10, votre application continue à s’exécuter dans le niveau de confiance totale en mode utilisateur comme il fait aujourd'hui.

>[!IMPORTANT]
>La possibilité de créer un package d’application Windows pour votre application de bureau (également appelé le pont du bureau) a été introduite dans Windows 10, version 1607, et il peut être utilisé uniquement dans les projets qui ciblent la mise à jour anniversaire Windows 10 (10.0 ; Build 14393) ou une version ultérieure dans Visual Studio.

> [!NOTE]
> Obtenez <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">cette série</a> de courtes vidéos publiées par Microsoft Virtual Academy. Ces vidéos vous guident tout au long de l’ensemble du processus consistant à mettre votre application de bureau pour la plateforme Windows universelle (UWP).

## <a name="benefits"></a>Avantages

Voici quelques raisons de créer un package d'application Windows pour votre application de bureau :

:heavy_check_mark: **Rationalisation du déploiement**. Les applications et les jeux qui utilisent le pont bénéficient d’une expérience de déploiement exceptionnelle. Cette expérience permet de s’assurer que les utilisateurs peuvent installer une application et mettre à jour d’en toute confiance. Si l’utilisateur choisit de désinstaller l’application, il est entièrement supprimé sans laisser aucune trace. Cela réduit le temps consacré à la création d’expériences d’installation et à l’actualisation des utilisateurs.

:heavy_check_mark: **Mises à jour automatiques et licences**. Votre application peut participer aux licences intégrées de la Microsoft Store et des fonctionnalités de mise à jour automatique. La mise à jour automatique est un mécanisme extrêmement fiable et efficace, car seules les parties modifiées de fichiers sont téléchargées.

:heavy_check_mark: **Augmentation de la portée et simplification de la monétisation**. Choisir une distribution via le Microsoft Store permet d’atteindre des millions d’utilisateurs de Windows 10 en mesure d’acquérir des applications, des jeux et d’effectuer des achats dans l’application à l’aide d’options de paiement locales.

:heavy_check_mark: **Ajout de fonctionnalités UWP**.  Vous pouvez ajouter des fonctionnalités UWP au package de votre application à votre propre rythme, comme une interface utilisateur XAML, des mises à jour de vignette en direct, des tâches en arrière-plan UWP, des services d’application, et bien plus encore.

:heavy_check_mark: **Élargissement des cas d’utilisation sur l’appareil**. Le pont vous permet de faire migrer graduellement votre code vers la plateforme Windows universelle pour atteindre chaque appareil Windows 10, y compris les téléphones, la console Xbox One et le casque HoloLens.

Pour voir une liste plus complète des avantages, consultez [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="prepare"></a>Préparation

Tout d’abord, préparez votre application en consultant l’article [préparer empaqueter votre application de bureau](desktop-to-uwp-prepare.md)et puis ciblant un des problèmes qui s’appliquent à votre application avant de créer un package d’application Windows pour elle. Vous devrez peut-être pas apporter de nombreuses modifications à votre application avant de créer le package. Toutefois, il existe certaines situations qui peuvent vous amener à modifier votre application avant de créer un package pour celle-ci.

<a id="convert" />

## <a name="package"></a>Package

Voici certains outils que vous pouvez employer pour créer un package d'application Windows pour votre application.

### <a name="desktop-app-converter"></a>Desktop App Converter

Bien que le terme « Converter » apparaisse dans son nom, cet outil ne convertit pas réellement votre application. Votre application reste inchangée. Toutefois, cet outil génère un package d’application Windows pour vous. Il peut être très pratique dans les cas où votre application effectue un grand nombre de modifications du système, ou si vous avez le moindre doute sur ce que fait votre programme d’installation.

Le Desktop App Converter traduit les actions de votre programme d’installation pour le fichier virtuel et le système de Registre que la version empaquetée de votre application utilisera. Desktop App Converter accomplit d’autres tâches pour vous. En voici quelques-unes :

:heavy_check_mark: Enregistrer automatiquement vos gestionnaires d’aperçus, de miniatures et de propriétés, les règles du pare-feu, les indicateurs d’URL.

:heavy_check_mark: Registres consigner automatiquement les mappages de types qui permettent aux utilisateurs de regrouper des fichiers à l’aide de la **type** colonne dans l’Explorateur de fichiers.

:heavy_check_mark: Enregistre vos serveurs COM publics.

:heavy_check_mark: Génère un certificat que vous pouvez utiliser pour exécuter votre application.

:heavy_check_mark: Valide de votre application par rapport à l’application de bureau empaquetée en matière de Microsoft Store.

Une autre bonne raison d’utiliser le Desktop App Converter est si vous tenir à jour votre application à l’aide d’un environnement de développement différents autres que Visual Studio. Vous pouvez utiliser le Desktop App Converter même si votre application n’a pas un programme d’installation.

Consultez [empaqueter une application de bureau à l’aide de la de Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md)

### <a name="visual-studio"></a>Visual Studio

Si vous effectuez la maintenance de votre application à l’aide de Visual Studio et qu'elle ne dispose d’un programme d’installation, ou que celui-ci n'effectue pas trop de tâches compliquées, pensez à utiliser Visual Studio.

Visual Studio simplifie grandement la création de packages. Il suffit d’ajouter un projet de création de packages, de référencer votre projet d'application de bureau, puis d'appuyer sur F5 pour déboguer votre application. Plus aucun ajustement n'est nécessaire. Cette nouvelle expérience plus fluide améliore considérable l'expérience de la version précédente de Visual Studio. Voici quelques autres choses que vous pouvez faire avec.

:heavy_check_mark: Générer automatiquement les ressources visuelles.

:heavy_check_mark: Apporter des modifications dans le manifeste à l’aide d’un concepteur visuel.

:heavy_check_mark: Générer votre package à l’aide d’un Assistant.

:heavy_check_mark: Facilement attribuer une identité à votre application à partir d’un nom que vous avez déjà réservés dans [partenaires](https://partner.microsoft.com/dashboard).

Consultez [empaqueter une application de bureau à l’aide de Visual Studio](desktop-to-uwp-packaging-dot-net.md)

### <a name="third-party-installer"></a>Programme d’installation tiers

 Plusieurs produits tiers populaires et les programmes d’installation prennent désormais en charge la possibilité pour empaqueter une application de bureau. Vous pouvez les utiliser pour générer des programmes d’installation MSI ou des packages d’applications en quelques clics seulement. Comme nous ne produisons pas de documentation sur l’utilisation de ces outils, nous vous conseillons de visiter leurs sites Web pour en savoir plus.

#### <a name="advanced-installer"></a>Programme d’installation avancée

Caphyon propose un outil de création de package d’applications de bureau gratuit, basé sur l’interface graphique, qui vous permet de générer un package d'application Windows pour votre application en quelques clics. Il peut utiliser n’importe quel programme d’installation ; même ceux qui s’exécutent en mode silencieux et effectue une validation de vérification pour déterminer si l’application est appropriée pour l’empaquetage.
Desktop App Converter s’intègre également avec Hyper-V et [VMware](https://www.vmware.com/). Cela signifie que vous pouvez utiliser vos propres machines virtuelles, sans avoir à télécharger une image [Docker](https://docs.docker.com/) correspondante dont la taille peut dépasser 3 Go.

<img width="20%" src="images/desktop-to-uwp/Advanced_Installer_Vertical.png">

Vous pouvez utiliser [Advanced Installer](https://www.advancedinstaller.com/) pour générer des [packages d’application Windows](https://www.advancedinstaller.com/uwp-app-package.html) et MSI à partir de projets existants. Vous pouvez également utiliser Advanced Installer pour importer les packages d’application Windows générés à l’aide de Microsoft Desktop App Converter. Après leur importation, vous pouvez les gérer à l’aide des outils visuels, qui sont spécifiquement conçus pour les applications UWP.

Advanced Installer fournit également une extension pour Visual Studio 2017 et 2015 qui permet de [générer et déboguer des applications Pont du bureau](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

Consultez cette [vidéo](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) pour une présentation rapide.

> [!TIP]
> Veillez à consulter l’article récemment publié [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html).

#### <a name="cloudhouse-compatibility-containers"></a>Conteneurs de compatibilité Cloudhouse

Pour les clients d’entreprise qui ont des applications métier non compatibles avec Windows 10 et Windows 10 S, les conteneurs de compatibilité de Cloudhouse permettent aux applications Windows XP et Windows 7 de s'exécuter sur Windows 10, puis de les convertir pour pouvoir s'exécuter sur la plateforme Windows universelle (UWP) et être livrées via le Microsoft Store pour Entreprises ou Microsoft InTune, sans modification du code source. Inscrivez-vous pour un [essai gratuit](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/desktop-to-uwp/cloudhouse-container-logo.png">

Cloudhouse fournit un gestionnaire de package automatique empaquetage des applications métier en [compatibilité conteneurs](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) sur les systèmes d’exploitation qui s’exécute les applications sur aujourd'hui (par exemple : Windows XP), puis [préparer pour la conversion](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) vers UWP. Le conteneur est ensuite converti vers le nouveau format de package d’application Windows grâce à son intégration avec l’outil Desktop App Converter de Microsoft.

L'outil de création de package automatique utilise l'analyse d'installation/de capture et d'exécution pour créer un conteneur pour l’application qui inclut les fichiers, le Registre, les runtimes et les dépendances de l’application, ainsi que le moteur de redirection et de compatibilité nécessaire pour que l’application s’exécute sur Windows 10. Le conteneur assure l'isolation de l’application et de ses runtimes afin qu’ils ne soient pas affectés ou n'entrent pas en conflit avec d’autres applications s’exécutant sur l’appareil de l’utilisateur.

En savoir plus sur la façon de fournir des applications d’entreprise via le Microsoft Store pour Entreprises. Lire dans notre [blog de version](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp).

#### <a name="firegiant"></a>FireGiant

Le [extension de FireGiant MSIX](https://www.firegiant.com/products/wix-expansion-pack/msix) vous permet de créer des packages d’application Windows et les packages MSI simultanément à partir du même code source WiX. Chaque fois que vous générez, vous pouvez cibler Windows 10 avec un package d’application Windows et les versions antérieures de Windows avec MSI.

<img width="20%" src="images/desktop-to-uwp/FG3rdPartyLogo.png">

L’extension de FireGiant MSIX utilise l’analyse statique et émulation intelligente de vos projets WiX pour créer des packages d’application Windows sans la surcharge de disque espace et runtime de conteneurs ou des machines virtuelles.

Étant donné que l’extension FireGiant MSIX ne convertit pas votre programme d’installation en l’exécutant, vous pouvez conserver votre programme d’installation WiX sans avoir à le convertir à plusieurs reprises pour les packages d’applications Windows. Tous les utilisateurs sur les différentes versions de Windows obtiennent vos dernières améliorations et vous n’avez pas à vous soucier de la désynchronisation des packages d’application MSI et Windows.

Regardez cette [vidéo](https://www.youtube.com/watch?v=AFBpdBiAYQE) et voir comment avec quelques lignes de code FireGiant PDG Rob Mensching crée une version Appx (package d’application Windows) de l’outil de compression de 7-Zip open source populaire, puis comment il améliore les deux applications de Windows et Packages MSI avec les modifications dans le même code source WiX.

#### <a name="installaware"></a>InstallAware

Install**Aware**, avec son [palmarès](https://www.installaware.com/press-room.htm) de prise en charge rapide des innovations de Microsoft, crée des [packages d’application Windows (Pont du bureau)](https://www.installaware.com/appx-builder.htm), App-V (Application Virtualization), MSI (Windows Installer) et des packages EXE (Code natif) à partir d’une source unique.

<img width="20%" src="images/desktop-to-uwp/installaware.png">

Install**Aware** fournit des extensions gratuites Install**Aware** pour les versions de Visual Studio 2012-2017. Vous pouvez les utiliser pour créer des packages d’application Windows en un seul clic, directement à partir de la [barre d’outils Visual Studio](https://www.installaware.com/visual-studio-installer-2015.htm).

Vous pouvez également importer n’importe quelle configuration, même si vous n’en avez pas le code source à l’aide de Package**Aware** (captures de configuration sans instantané) ou de l’Assistant Importation de base de données (pour tous les programmes d’installation MSI et les modules de fusion MSM). Vous pouvez utiliser des [outils d’interface utilisateur graphique](https://www.installaware.com/scripting-two-way-integrated-ide.htm) pour maintenir et améliorer vos importations, visuellement ou par des scripts.

Les [options de création APPX avancées](https://www.installaware.com/mhtml5/desktop/appx.htm) vous aident à cibler des soumissions au Microsoft Store ou à produire des fichiers binaires de package d’application Windows signés pour la distribution indépendante aux utilisateurs finaux. Vous pouvez même créer des packages d’installation **WSA**(Windows Server Applications) qui ciblent les déploiements à **Nano Server** à partir d’une source unique et avec une prise en charge complète pour l'[automatisation de ligne de commande](https://www.installaware.com/scripting-automation-interface.htm), en plus d’une interface graphique utilisateur.

Install**Aware** a également [mis en open source](https://www.installaware.com/gnu.asp) une **bibliothèque du générateur APPX**, avec un exemple d'applet de ligne de commande, sous licence GPL GNU Affero. Ils sont destinés à être utilisés avec des plateformes open source telles que WiX.

#### <a name="installshield"></a>InstallShield

InstallShield fournit une solution unique pour développer des programmes d’installation MSI et EXE, créer des packages de plateforme Windows universelle (UWP) et Windows Server App (WSA) et virtualiser des applications avec un minimum de script, de codage et de reprise.

<img width="20%" src="images/desktop-to-uwp/InstallShield-logo.jpg">

Analysez votre projet InstallShield en quelques secondes pour économiser des heures de travail d’investigation en identifiant automatiquement les problèmes potentiels de compatibilité entre votre application et les packages UWP et WSA.

Préparez-vous pour le Microsoft Store et simplifiez l’expérience d’installation de votre logiciel sur Windows 10 en créant des packages d’application UWP à partir de vos projets InstallShield existants. Générez à la fois des packages Windows Installer et d’application UWP pour prendre en charge tous les scénarios de déploiement souhaités par vos clients. Prenez en charge les déploiements de Nano Server et Windows Server 2016 en créant des packages WSA à partir de vos projets InstallShield.

Développez votre installation en modules pour faciliter le déploiement et la maintenance, puis fusionnez les composants et les dépendances au moment de la génération dans un seul package d’application UWP pour le Microsoft Store. Pour la distribution directe en dehors de la banque, regroupez vos packages d’application UWP et d'autres dépendances avec un programme d’installation d’interface utilisateur de suite/avancé.

En savoir plus dans ce [livre électronique](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

#### <a name="pace-suite"></a>Suite PACE

La [Suite PACE](https://pacesuite.com/) est un outil de création de packages d’applications que vous pouvez utiliser pour porter vos applications de bureau vers la plateforme Windows universelle.

<img width="20%" src="images/desktop-to-uwp/PACE.png">

Avec la suite PACE, vous n’avez pas besoin préparer d'environnement spécial de création de package, ni d'installer de composants supplémentaires du kit de développement Windows. La suite PACE peut produire des packages d’application Windows indépendamment de votre environnement de création de package standard, sous Windows 10 ou Windows Server 2016. Consultez cet [exemple illustré](https://pacesuite.com/convert-exe-to-appx/) pour découvrir comment la suite PACE aborde le réempaquetage d'un programme d'installation dans un package d'application Windows.

Outre la création de packages d’application Windows, vous pouvez également utiliser la suite PACE pour créer des packages Windows Installer (MSI), des correctifs (MSP), des transformations (MST) et des packages App-V. En ce qui concerne la création MSI, la suite PACE est utile pour gérer les mises à niveau, les paramètres d'autorisation, les actions personnalisées, les scripts et d’autres éléments. Vous pouvez également publier vos applications directement dans le System Center Configuration Manager.

Pour consulter toutes les fonctionnalités de création de packages d’application, voir [Fonctionnalités de la suite PACE](https://pacesuite.com/features/).

#### <a name="rad-studio"></a>RAD Studio

Voir [RAD Studio d’Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

#### <a name="raypack-studio"></a>RayPack Studio

Raynet de l’empaquetage de solution, [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio)réintégration framework et prend en charge la création de packages pour les applications de bureau en tant qu’un ou plusieurs résultats possibles de conversion efficace et facile à configurer.

<img width="20%" src="images/desktop-to-uwp/RaynetLogo_v3.png">

Il est possible d'utiliser les environnements virtuels existants (VMware Workstation, Hyper-V) pour effectuer la conversion automatique/en bloc sans nécessiter de fastidieuse configuration d’environnement. Un des composants du studio, ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), est en mesure d’effectuer des tests de filtrage et de compatibilité préalables à la conversion pour vérifier que le logiciel est éligible à celle-ci. En outre, les utilisateurs peuvent désormais effectuer des contrôles exhaustifs de collision et de compatibilité avec différentes éditions de Windows 10, notamment les mises à jour anniversaire et Creators.

Outre la création de packages logiciels au format APPX/UWP de Windows 10, RayPack Studio peut également être utilisé pour créer des packages Windows Installer classiques (MSI), des correctifs (MSP), des transformations (MST) et des packages App-V. De plus, cette solution est livrée avec un ensemble de produits et composants logiciels dédiés à l'empaquetage logiciel professionnel d'entreprise. Outre la création de packages logiciels et la virtualisation, RayPack Studio prend en compte toutes les tâches liées à la création de packages : contrôle des conflits et de la compatibilité des applications et des packages logiciels ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), évaluation de logiciel ([RayEval](https://raynet.de/Raynet-Products/RayEval)) et assurance qualité ([RayQC](https://raynet.de/Raynet-Products/RayQC)).

Combiné avec [RayFlow](https://raynet.de/Raynet-Products/RayFlow), le système de workflow d'entreprise de Raynet, les utilisateurs peuvent travailler efficacement au logiciel tout au long du cycle de vie d'application de l'entreprise, depuis la commande du package jusqu'à l'acceptation par l'utilisateur et le déploiement, en passant par l'évaluation, l'analyse, l'empaquetage et l'assurance qualité. Tous les packages et formats peuvent être stockés et déployés directement dans SCCM ou d’autres solutions. L'intégralité du cycle de vie de l'application est suivi et géré par RayFlow. En outre, les systèmes de commande tels que ServiceNow peuvent être intégrés. Raynet crée des fabriques de création de packages logiciels dans le monde entier avec ses outils dédiés aux fournisseurs de services.

Faites l'expérience et profitez de [la licence d’essai gratuite](https://raynet.de/contact?init=license) de RayPack Studio et RayFlow de Raynet. Pour plus d’informations, rendez-vous sur [www.raynet.de](https://raynet.de/home).

**Liens connexes** :

* Raynet :[https://raynet.de/home](https://raynet.de/home)
* RayPack Studio : [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow :[https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval :[https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC :[https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced : [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* Licence d'essai gratuite : [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)

### <a name="manual-packaging"></a>Création manuelle de package d’application

En dernier recours, vous pouvez convertir votre application sans utiliser l’une de ces outils. Si vous souhaitez un contrôle minutieux de la conversion, vous pouvez créer un fichier de manifeste puis exécuter l’outil **MakeAppx.exe** pour créer votre package d’application Windows.

Consultez [empaqueter une application de bureau manuellement](desktop-to-uwp-manual-conversion.md).

## <a name="integrate"></a>Intégrer

Si votre application doit intégrer le système (par exemple : établir des règles de pare-feu), décrire ces éléments dans le manifeste du package de votre application et que le système s’occupe du reste. Pour la plupart de ces tâches, vous n’avez pas à écrire du code. Avec un peu de XML dans le manifeste, vous pouvez faire choses tels que démarrer un processus lorsque l’utilisateur ouvre une session, intégrer votre application dans l’Explorateur de fichiers et ajouter votre application une liste de cibles impression qui s’affichent dans d’autres applications.

Consultez [intégrer votre application de bureau empaquetée avec Windows 10](desktop-to-uwp-extensions.md).

## <a name="enhance"></a>Améliorer

Dès que vous avez empaqueté votre application, vous pouvez valoriser celle-ci à l’aide de fonctionnalités telles que les vignettes dynamiques et les notifications Push. Certaines de ces fonctionnalités peuvent améliorer considérablement le niveau d’engagement de votre application et vous coûter très peu de temps à ajouter. Certaines améliorations demandent un peu plus de codage.

Voir [Améliorer votre application de bureau pour Windows 10](desktop-to-uwp-enhance.md).

## <a name="extend"></a>Étendre

Certaines expériences Windows 10 (par exemple: une page d'interface utilisateur tactile) doivent s'exécuter à l'intérieur d'un conteneur d'application moderne. En règle générale, vous devez d’abord déterminer si vous pouvez ajouter votre expérience en [améliorant](desktop-to-uwp-enhance.md) votre application de bureau avec des API UWP. Si vous devez utiliser un composant UWP, pour obtenir de l’expérience, vous pouvez ajouter un projet UWP à votre solution et utiliser des services d’application pour la communication entre votre application de bureau et le composant UWP.

Consultez [Étendre votre application de bureau avec des composants UWP modernes](desktop-to-uwp-extend.md).

## <a name="migrate"></a>Migrer

Il n’existe aucun outil qui puisse convertir une application de bureau vers une application UWP, mais vous pouvez réutiliser une grande partie de votre code existant, ce qui réduit le coût de création. C'est possible en déplaçant autant de logique métier que possible dans les bibliothèques .NET Standard 2.0.

.Net Standard 2.0 inclut un important accroissement du nombre des API .NET, ainsi qu'un shim de compatibilité pour vos packages NuGet favoris et les bibliothèques tierces.

Faites migrer votre code dans les bibliothèques .NET Standard, puis créez une application de plateforme Windows universelle (UWP) pour toucher tous les appareils Windows 10.

Voir [Partager du code entre une application de bureau et une application UWP](desktop-to-uwp-migrate.md)


## <a name="test"></a>Tester

Pour tester votre application dans un paramètre réaliste comme vous préparer pour distribution, il est préférable signer votre application, puis installez-le. Voir [Tester votre application](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-debug#test-your-app).

>[!IMPORTANT]
> Si vous envisagez de publier votre application sur le Microsoft Store, assurez-vous que votre application fonctionne correctement sur les appareils qui exécutent Windows 10 en mode de S. Il s’agit d’une exigence de magasin. Consultez [Tester votre application Windows pour Windows 10 en mode S](desktop-to-uwp-test-windows-s.md).

## <a name="validate"></a>Valider

Pour donner à votre application les meilleures chances d’être publiées sur le Microsoft Store ou devenir [certifiés Windows](https://go.microsoft.com/fwlink/p/?LinkID=309666), valider et tester localement avant de le soumettre pour certification.

Si vous utilisez la DAC pour empaqueter votre application, vous pouvez utiliser la nouvelle ``-Verify`` indicateur permettant de valider votre package sur l’application de bureau empaquetée en matière de Store. Voir [Créer un package d'application, signer et préparer une application pour la soumission au Store](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters).

Si vous utilisez Visual Studio, vous pouvez valider votre application à partir de la **créer des Packages d’application** Assistant. Voir [Créer un fichier de chargement de package d’application](../packaging/packaging-uwp-apps.md#create-an-app-package-upload-file).

Pour exécuter l’outil manuellement, voir [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md).

Pour consulter la liste des tests utilisés au cours de la certification des applications Windows pour valider votre application, voir [Tests d’application Windows Pont du bureau](../debug-test-perf/windows-desktop-bridge-app-tests.md).

## <a name="distribute"></a>Distribuer

Vous pouvez distribuer votre application en la publiant le Microsoft Store ou par chargement de version test sur d’autres systèmes.

Consultez [distribuer une application de bureau empaquetée](desktop-to-uwp-distribute.md).

## <a name="support-and-feedback"></a>Support et commentaires

**Trouvez des réponses à vos questions**

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Donner votre avis ou faire des suggestions de fonctionnalité**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

## <a name="in-this-section"></a>Dans cette section

| Rubrique | Description |
|-------|-------------|
| [Préparer empaqueter une application](desktop-to-uwp-prepare.md) | Fournit une liste d’éléments à passer en revue avant de créer un package de votre application de bureau. |
| [Package d’application à l’aide de la de Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | Montre comment exécuter Desktop App Converter. |
| [Empaqueter une application de bureau manuellement](desktop-to-uwp-manual-conversion.md) | Découvrez comment créer un package et un manifeste d’application manuellement. |
| [Empaqueter une application de bureau à l’aide de Visual Studio](desktop-to-uwp-packaging-dot-net.md)| Vous montre comment empaqueter votre application de bureau à l’aide de Visual Studio. |
| [Intégrer votre application de bureau Windows 10](desktop-to-uwp-extensions.md) | Intégrer votre application avec Windows 10 à l’aide en décrivant des tâches dans le fichier de manifeste de package de votre projet d’empaquetage. |
| [Améliorer votre application de bureau pour Windows 10](desktop-to-uwp-enhance.md)| Utilisez les API UWP pour ajouter des expériences modernes qui se déclenchent pour les utilisateurs de Windows 10. |
| [API UWP disponibles pour une application de bureau empaquetée](desktop-to-uwp-supported-api.md) | Consultez quelles API UWP sont disponibles pour votre application de bureau empaquetée à utiliser. |
| [Étendre votre application de bureau avec des composants UWP modernes](desktop-to-uwp-extend.md)| Ajouter des expériences avancées qui doivent s’exécuter au sein d’un conteneur d’application UWP. Connectez votre application de bureau avec le processus UWP à l’aide des services d’application.|
| [Exécuter, déboguer et tester une application de bureau empaquetée](desktop-to-uwp-debug.md) | Explique les options de débogage de votre application empaquetée. |
| [Distribuer une application de bureau empaquetée ](desktop-to-uwp-distribute.md) | Voir comment vous pouvez distribuer votre application convertie aux utilisateurs.  |
| [Issues(desktop-to-uwp-known-issues.md) connus | Répertorie les problèmes connus avec l’empaquetage d’applications de bureau. |
