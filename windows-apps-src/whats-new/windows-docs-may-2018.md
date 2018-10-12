---
author: QuinnRadich
title: Quelles sont les nouveautés dans la documentation Windows en mai 2018 - développer des applications UWP
description: Nouvelles fonctionnalités, des vidéos et des conseils aux développeurs ont été ajoutées à la documentation du développeur Windows 10 pour les mois de mai 2018 et de la conférence Microsoft Build.
keywords: Nouveautés, mise à jour, fonctionnalités, conseils aux développeurs, Windows 10, mai, build
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4564319"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Quelles sont les nouveautés dans la documentation du développeur Windows en mai 2018

La documentation du développeur Windows est constamment mise à jour afin d'intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Les présentations de fonctionnalités, conseils aux développeurs, vidéos et exemples suivantes ont été à disposition dans le mois de mai coïncider avec la conférence [Microsoft Build 2018](https://www.microsoft.com/build) des développeurs.

[Installez les outils et le kit de développement logiciel (SDK)](http://go.microsoft.com/fwlink/?LinkId=821431) sur Windows10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou découvrir comment utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="motion-in-fluent-design"></a>Mouvement dans Fluent Design

L’utilisateur de mouvement dans le système Fluent Design évolue, qui repose sur les principes fondamentaux de minutage, d’accélération, la direction et gravité. Appliquer ces principes de base de vous guider l’utilisateur dans votre application et les connecte avec son expérience numérique en reflétant le monde physique. En savoir plus dans cet article:

* [Vue d’ensemble du mouvement](../design/motion/index.md) a été mis à jour pour refléter ces principes de base.
* [Mouvement en pratique](../design/motion/motion-in-practice.md) fournit des exemples illustrant comment appliquer ces principes de base au sein de votre application.
* [Direction et gravité](../design/motion/directionality-and-gravity.md) renforce le modèle mental de l’utilisateur de votre application.
* [Minutage et accélération](../design/motion/timing-and-easing.md) ajoute le réalisme au mouvement dans votre application.

![Mouvement en action](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Mises à jour de la conception Fluent

Les mises à jour visuelles et modifications mineures ont été apportées aux pages Fluent Design suivantes:

* [Alignement, marges, espacement](../design/layout/alignment-margin-padding.md)
* [Couleur](../design/style/color.md)
* [Notions de base sur les commandes](../design/basics/commanding-basics.md)
* [Fluent Design pour les applications Windows](../design/fluent-design-system/index.md)
* [Introduction à la conception de l’application](../design/basics/design-and-ui-intro.md)
* [Notions de base sur la navigation](../design/basics/navigation-basics.md)
* [Techniques de conception réactive](../design/layout/responsive-design.md)
* [Tailles d’écran et points d’arrêt](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Vue d’ensemble de style](../design/style/index.md)
* [Style d’écriture](../design/style/writing-style.md)

En outre, nous avons réécrit les pages suivantes avec toute nouvelle information sur leurs zones de contenu:

* [Icônes](../design/style/icons.md) maintenant fournit des recommandations pratiques pour l’utilisation des icônes et les rendre interactif.
* [Typographie](../design/style/typography.md) regroupe les informations provenant des articles similaires, tout placer dans un emplacement unique avec des conseils mis à jour et d’illustrations.

![Image de palette de couleurs](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Fichiers de programme d’installation de l’application dans Visual Studio

Les fichiers de programme d’installation de l’application peuvent désormais être créés avec Visual Studio 2017, version 15.7 de mise à jour. [Découvrez comment utiliser Visual Studio pour créer un fichier de programme d’installation de l’application](../packaging/create-appinstallerfile-vs.md) et activer mises à jour automatiques à vos applications. Si vous rencontrez des problèmes, reportez-vous à la [résolution des problèmes d’installation avec le fichier de programme d’installation de l’application](../packaging/troubleshoot-appinstaller-issues.md) pour afficher les problèmes courants et solutions.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Edge contrôle WebView pour les applications Windows Forms et WPF

Afficher le contenu web dans votre application de bureau en utilisant le contrôle WebView, auparavant uniquement disponible pour les applications UWP. Ce contrôle utilise le moteur d’incorporer un affichage que restitue mise en forme enrichie HTML de rendu contenu à partir d’un serveur web distant, votre code généré de manière dynamique ou fichiers de contenu de Microsoft Edge. Recherchez le contrôle WebView dans la dernière version de la [Windows Community Toolkit.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Recherchez d’autres contrôles comme WebView dans les futures versions de Windows Community Toolkit. Pour plus d’informations, voir [hôte UWP des contrôles dans les applications WPF et Windows Forms.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Le pointage du regard à l’entrée et interactions

[Effectuez le suivi du pointage du regard, de l'attention et de la présence de l'utilisateur en fonction de l’emplacement et des mouvements de ses yeux.](../design/input/gaze-interactions.md) Ce nouveau moyen puissant d’utiliser et d’interagir avec vos applications UWP est particulièrement utile comme technologie d’assistance. L’entrée du pointage du regard fournit également de remarquables opportunités pour les jeux (notamment pour l’acquisition de cible et le suivi) et les autres scénarios interactifs dans lequel les périphériques d’entrée classiques (clavier, souris, tactile) ne sont pas disponibles.

### <a name="msix-packaging-format"></a>Format d’empaquetage MSIX

Annoncé lors de la conférence Microsoft Build 2018, MSIX est un nouveau format de package de mise en conteneur qui s’applique à toutes les applications Windows, y compris Win32, Windows Forms, WPF et UWP. Ce nouveau format hérite de fonctionnalités UWP:

* Installation robuste et la mise à jour. 
* Géré le modèle de sécurité avec un système de fonctionnalité flexible.
* Prise en charge pour le Microsoft Store, la gestion de l’entreprise et de nombreux modèles de distribution personnalisée.

Outils de création de ces packages seront disponibles dans une prochaine version de Visual Studio et le Kit de développement Windows.

Le format d’empaquetage MSIX est un format open source qui vous permet de nos partenaires prendre en charge de l’écosystème MSIX avec leurs solutions et les outils. Pour en savoir plus sur le format d’empaquetage MSIX, consultez le [Kit de développement logiciel MSIX](https://github.com/Microsoft/msix-packaging). 

![Image d’empaquetage MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Packages facultatifs avec code exécutable

Packages facultatifs dans votre application peuvent contenir maintenant de code c# exécutable. [Découvrez comment utiliser Visual Studio pour configurer les packages facultatifs de module complémentaire pour prendre en charge votre package d’application principale.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transitions de page

[Les transitions de page](../design/motion/page-transitions.md) dirigent les utilisateurs entre les pages d’une application. Ils permettent aux utilisateurs de comprendre où ils se trouvent dans la hiérarchie de navigation et fournir des commentaires sur la relation entre les pages.

### <a name="project-rome"></a>Projet «Rome»

L’équipe de projet «Rome» a entièrement révisé leur iOS et le SDK Android, d’ajouter de nouvelles fonctionnalités telles que des activités de l’utilisateur et la refactorisation une grande partie de leur code pour fournir une expérience de programmation cohérente entre les différents kits de développement. [Tous les nouvelle référence d’API et la documentation de procédures](https://docs.microsoft.com/windows/project-rome/) est soumis live au cours de la conférence des développeurs Build 2018.

### <a name="sets"></a>Jeux

La fonctionnalité jeux est disponible dans les versions d’évaluation de Windows Insider. Lorsque vous utilisez la fonctionnalité de jeux, votre application est dessinée dans une fenêtre qui peut être partagée avec d’autres applications, avec chaque application ayant son propre onglet dans la barre de titre. [Conception de jeux](../design/shell/design-for-sets.md) comprend des conseils sur la façon d’optimiser votre application pour fournir la meilleure expérience possible dans l’interface utilisateur définit.

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="get-started"></a>Prise en main

Nous avons aux retraités rajeunit notre Get démarré contenu avec les nouvelles pistes d’apprentissage. Ces nouvelles rubriques visent à fournir des nouveaux développeurs Windows 10 avec les informations sur les tâches courantes ils souhaitent accomplir. Ils ne sont pas des didacticiels et ne fournissent pas une procédure pas à pas PORTATIF, mais au lieu de cela, signalez lorsqu’il existe une documentation existante et comment l’utiliser. Consultez l’article revisitée [commencer à coder](../get-started/create-uwp-apps.md) page, ou à découvrir chaque piste d’apprentissage individuels:

* [Construire un formulaire](../get-started/construct-form-learning-track.md)
* [Afficher les clients dans une liste](../get-started/display-customers-in-list-learning-track.md)
* [Paramètres d'enregistrement et de chargement](../get-started/settings-learning-track.md)
* [Travailler avec des fichiers](../get-started/fileio-learning-track.md)

![Obtenir une image prise en main](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Rapport sur les performances publicitaires

Le [rapport sur les performances publicitaires](../publish/advertising-performance-report.md) dans le tableau de bord du centre de développement offre désormais les mesures de visibilité. Nous avons également ajouté l’article [optimiser la visibilité de vos unités publicitaires](../monetize/optimize-ad-unit-viewability.md) pour fournissent des recommandations permettant d’optimiser la visibilité de vos annonces.

### <a name="targeted-push-notifications"></a>Notifications push ciblées

La page de [Notifications](../publish/send-push-notifications-to-your-apps-customers.md) dans le tableau de bord du centre de développement fournit désormais des données d’analytique supplémentaires pour toutes vos notifications dans les vues de carte graphique et le monde.

## <a name="videos"></a>Vidéos

### <a name="cwinrt"></a>C++/WinRT

C++ / WinRT est une nouvelle façon de création et l’utilisation APIs Windows Runtime. Il est implémenté unique dans les fichiers d’en-tête et conçue pour vous fournir un accès aux fonctionnalités d’application moderne. [Regardez la vidéo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) pour savoir comment il fonctionne, puis [Lisez la documentation du développeur](../cpp-and-winrt-apis/index.md) pour plus d’informations.

### <a name="multi-instance-uwp-apps"></a>Applications UWP à instances multiples

Windows vous permet désormais d’exécuter plusieurs instances de votre application UWP, avec chaque dans son propre processus distinct. [Regardez la vidéo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) pour savoir comment créer une nouvelle application qui prend en charge cette fonctionnalité, puis [Lisez la documentation du développeur](../launch-resume/multi-instance-uwp.md) pour plus d’informations sur la façon d’et pourquoi utiliser cette fonctionnalité.

## <a name="samples"></a>Exemples

### <a name="customer-database-tutorial"></a>Didacticiel de base de données client

Ce didacticiel crée une application UWP de base pour la gestion d’une liste de clients et présente les concepts et pratiques utiles pour le développement d’entreprise. Il vous guide par le biais de la mise en œuvre des éléments d’interface utilisateur et en ajoutant des opérations par rapport à une base de données SQLite locale et fournit des conseils libres pour se connecter à une base de données reste à distance, si vous souhaitez aller plus loin. [Consultez le didacticiel ici](../enterprise/customer-database-tutorial.md)