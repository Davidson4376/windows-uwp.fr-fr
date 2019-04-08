---
title: Quelles sont les nouveautés dans Windows Docs en mai 2018 - développer des applications UWP
description: Guide du développeur, des vidéos et des nouvelles fonctionnalités ont été ajoutés à la conférence Microsoft Build et de la documentation pour développeurs Windows 10 pour mai 2018.
keywords: Quelles sont les nouveautés, mettre à jour, fonctionnalités, Guide du développeur, Windows 10, mai, génération
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69df2bbe8bc91fcf4a2631c0f257fc44851c24f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598784"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Quelles sont les nouveautés dans la documentation du développeur Windows en mai 2018

La documentation du développeur Windows est constamment mise à jour afin d'intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Vues d’ensemble des fonctionnalités suivantes, des instructions destinées aux développeurs, des vidéos et des exemples mis à la disposition du mois de mai pour coïncider avec la [Microsoft Build 2018](https://www.microsoft.com/build) conférence des développeurs.

[Installez les outils et le kit de développement logiciel (SDK)](https://go.microsoft.com/fwlink/?LinkId=821431) sur Windows 10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou explorer la procédure permettant d’utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="motion-in-fluent-design"></a>Mouvement de conception Fluent

L’utilisateur du mouvement dans le système de conception Fluent est en constante évolution, basé sur les principes fondamentaux de minutage d’accélération, une direction et gravité. Appliquer ces principes de base de guider l’utilisateur dans votre application et de les connecter avec leur expérience numérique en reflétant le monde physique. En savoir plus dans cet article :

* [La vue d’ensemble du mouvement](../design/motion/index.md) a été mis à jour pour refléter ces principes de base.
* [Mouvement dans la pratique](../design/motion/motion-in-practice.md) fournit des exemples montrant comment appliquer ces principes de base au sein de votre application.
* [Une direction et la gravité](../design/motion/directionality-and-gravity.md) solidification modèle mental de l’utilisateur de votre application.
* [Accélération et d’échéance](../design/motion/timing-and-easing.md) ajoute réalisme au mouvement dans votre application.

![Mouvement en action](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Mises à jour de conception Fluent

Mises à jour visuelles et des modifications mineures ont été apportées pour les pages suivantes de la conception Fluent :

* [Alignement, marges intérieures, des marges](../design/layout/alignment-margin-padding.md)
* [Couleur](../design/style/color.md)
* [Notions de base sur les commandes](../design/basics/commanding-basics.md)
* [Fluent Design pour les applications de Windows](../design/fluent-design-system/index.md)
* [Introduction à la conception de l’application](../design/basics/design-and-ui-intro.md)
* [Notions de base sur la navigation](../design/basics/navigation-basics.md)
* [Techniques de conception réactives](../design/layout/responsive-design.md)
* [Tailles d’écran et points d’arrêt](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Vue d’ensemble de style](../design/style/index.md)
* [Style d’écriture](../design/style/writing-style.md)

En outre, nous avons réécrit les pages suivantes avec les informations de toute nouvelle sur leurs zones de contenu :

* [Icônes](../design/style/icons.md) fournit désormais des conseils pratiques pour l’aide des icônes et de les rendre interactif.
* [Typographie](../design/style/typography.md) consolide des informations à partir d’articles similaires, tout placer dans un emplacement unique avec des instructions de mise à jour et des illustrations.

![Image de palette de couleurs](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Fichiers de programme d’installation de l’application dans Visual Studio

Fichiers de programme d’installation de l’application peuvent maintenant être créés avec Visual Studio 2017, 15.7 de mise à jour. [Découvrez comment utiliser Visual Studio pour créer un fichier de programme d’installation de l’application](../packaging/create-appinstallerfile-vs.md) et activer les mises à jour automatiques pour vos applications. Si vous rencontrez des problèmes, consultez [résolution des problèmes d’installation avec le fichier de programme d’installation de l’application](../packaging/troubleshoot-appinstaller-issues.md) pour afficher les problèmes courants et solutions.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Edge contrôle WebView pour les applications Windows Forms et WPF

Afficher le contenu web dans votre application de bureau en utilisant le contrôle WebView, auparavant disponible uniquement pour les applications UWP. Ce contrôle utilise le rendu d’un moteur pour incorporer une vue que rendus mise en forme enrichie HTML contenu à partir d’un serveur web à distance, du code généré dynamiquement ou les fichiers de contenu Microsoft Edge. Rechercher le contrôle WebView dans la dernière version de la [Kit de ressources de communauté Windows.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Recherchez les autres contrôles comme WebView dans les versions futures de la boîte à outils de la Communauté Windows. Pour plus d’informations, consultez [UWP de l’hôte de contrôle dans les applications WPF et Windows Forms.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Remplacez les entrées et interactions

[Effectuer le suivi d’un utilisateur regards attention et présence en fonction de l’emplacement et le déplacement de leurs yeux.](../design/input/gaze-interactions.md) Cette nouvelle méthode performante pour utiliser et d’interagir avec vos applications UWP est particulièrement utile en tant qu’une technologie d’assistance. Entrée des regards fournit également des opportunités intéressantes pour les jeux (y compris d’acquisition de cible et de suivi) et les autres scénarios interactifs où les périphériques d’entrée classiques (clavier, souris, tactile) ne sont pas disponibles.

### <a name="msix-packaging-format"></a>Format d’empaquetage MSIX

Annoncé lors de la conférence Microsoft Build 2018, MSIX est un nouveau format de package de mise en conteneur qui s’applique à toutes les applications Windows, notamment Win32, Windows Forms, WPF et UWP. Ce nouveau format hérite des fonctionnalités exceptionnelles UWP :

* Installation robuste et la mise à jour. 
* Géré le modèle de sécurité avec un système flexible de fonctionnalité.
* Prise en charge pour le Microsoft Store, de gestion d’entreprise et de nombreux modèles de distribution personnalisée.

Outils pour créer ces packages seront disponibles dans une prochaine version de Visual Studio et le Kit de développement logiciel Windows.

Le format d’empaquetage MSIX est un format open source, ce qui le rend plus facile pour nos partenaires prendre en charge de l’écosystème MSIX avec leurs outils et leurs solutions. Pour en savoir plus sur le format d’empaquetage MSIX, consultez [MSIX SDK](https://github.com/Microsoft/msix-packaging). 

![Image d’empaquetage MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Packages facultatifs avec code exécutable

Les packages facultatifs dans votre application peuvent maintenant contenir exécutable C# code. [Découvrez comment utiliser Visual Studio pour configurer des packages de module complémentaire facultatif pour prendre en charge de votre package d’application principal.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transitions de page

[Transitions de page](../design/motion/page-transitions.md) naviguer utilisateurs entre les pages dans une application. Elles aident les utilisateurs de comprendre où elles se trouvent dans la hiérarchie de navigation et fournir des commentaires sur la relation entre les pages.

### <a name="project-rome"></a>Projet Rome

L’équipe de Project Rome a repensé leurs iOS et les kits Android SDK, ajout de nouvelles fonctionnalités telles que les activités des utilisateurs et de refactorisation une grande partie de leur code pour fournir une expérience de programmation cohérente entre les différents kits de développement. [Toutes les nouvelles API référence et des procédures docs](https://docs.microsoft.com/windows/project-rome/) seront lancés au cours de la conférence des développeurs Build 2018.

### <a name="sets"></a>Jeux

La fonctionnalité de jeux est disponible dans Insider de Windows génère de la version préliminaire. Lorsque vous utilisez la fonctionnalité de jeux, votre application est dessinée dans une fenêtre qui peut être partagée avec d’autres applications, avec chaque application ayant son propre onglet dans la barre de titre. 

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="get-started"></a>Prise en main

Nous avons aux retraités rajeunit nos le contenu avec le nouveau parcours d’apprentissage de démarrage. Ces nouvelles rubriques visent à fournir des nouveaux développeurs Windows 10 avec des informations sur certaines tâches courantes, ils peuvent souhaiter effectuer. Ils ne sont pas des didacticiels et ne fournissent pas une procédure pas à pas portable, mais au lieu de cela souligner où se trouve de documentation existante et comment l’utiliser. Découvrez la version revisitée [commencer à coder](../get-started/create-uwp-apps.md) page, ou explorez chaque piste de formation individuels :

* [Construire un formulaire](../get-started/construct-form-learning-track.md)
* [Afficher les clients dans une liste](../get-started/display-customers-in-list-learning-track.md)
* [Enregistrer et charger les paramètres](../get-started/settings-learning-track.md)
* [Travailler avec des fichiers](../get-started/fileio-learning-track.md)

![Obtenir une image prise en main](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Rapport sur les performances publicitaires

Le [publier le rapport de performances](../publish/advertising-performance-report.md) dans partenaires propose désormais des mesures de la plupart des ordinateurs. Nous avons également ajouté la [optimiser la plupart des ordinateurs de vos unités ad](../monetize/optimize-ad-unit-viewability.md) article pour fournir des recommandations pour optimiser la plupart des ordinateurs de vos annonces.

### <a name="targeted-push-notifications"></a>Notifications Push ciblées

Le [Notifications](../publish/send-push-notifications-to-your-apps-customers.md) page dans l’espace partenaires fournit désormais des données d’analytique supplémentaire pour toutes vos notifications dans les affichages de mappage et world.

## <a name="videos"></a>Vidéos

### <a name="cwinrt"></a>C++/WinRT

C++ / c++ / WinRT est une nouvelle façon de création et d’utilisation de Windows Runtime APIs. Il a implémenté sole dans les fichiers d’en-tête et conçu pour vous fournir un accès privilégié aux fonctionnalités des applications modernes. [Regardez la vidéo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) à découvrir son fonctionnement, puis [lire la documentation développeur](../cpp-and-winrt-apis/index.md) pour plus d’informations.

### <a name="multi-instance-uwp-apps"></a>Applications UWP à instances multiples

Windows vous permet désormais à exécuter plusieurs instances de votre application UWP, avec chacune dans son propre processus distinct. [Regardez la vidéo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) pour apprendre à créer une application qui prend en charge cette fonctionnalité, puis [lire la documentation développeur](../launch-resume/multi-instance-uwp.md) pour obtenir des instructions sur la procédure et pourquoi utiliser cette fonctionnalité.

## <a name="samples"></a>Exemples

### <a name="customer-database-tutorial"></a>Didacticiel de base de données client

Ce didacticiel crée une application UWP de base pour la gestion d’une liste de clients et introduit des concepts et méthodes utiles dans le développement d’entreprise. Il présente l’implémentation des éléments d’interface utilisateur et l’ajout des opérations sur une base de données SQLite locale et fournit des conseils de libre pour la connexion à une base de données reste à distance si vous souhaitez aller plus loin. [Consultez le didacticiel ici](../enterprise/customer-database-tutorial.md)