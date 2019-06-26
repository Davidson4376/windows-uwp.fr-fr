---
title: Nouveautés apportées dans la documentation Windows en mai 2018 - Développer des applications UWP
description: De nouvelles fonctionnalités, des vidéos et des conseils aux développeurs ont été ajoutés à la documentation du développeur Windows 10 en mai 2018 et lors de la conférence Microsoft Build.
keywords: nouveautés, mise à jour, fonctionnalités, conseils aux développeurs, Windows 10, mai, build
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69df2bbe8bc91fcf4a2631c0f257fc44851c24f2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63805864"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Nouveautés apportées dans la documentation du développeur Windows en mai 2018

La documentation du développeur Windows est constamment mise à jour afin d’intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Les vues d’ensemble des fonctionnalités, les conseils aux développeurs, les vidéos et les exemples ci-après ont été mis à disposition au mois de mai pour coïncider avec la conférence [Microsoft Build 2018](https://www.microsoft.com/build) destinée aux développeurs.

[Installez les outils et le kit de développement logiciel (SDK)](https://go.microsoft.com/fwlink/?LinkId=821431) sur Windows 10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou explorer la procédure permettant d’utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="motion-in-fluent-design"></a>Animations dans Fluent Design

L’utilisation des animations dans le système Fluent Design évolue en fonction des principes fondamentaux de minutage, d’accélération, de direction et de gravité. La mise en application de ces principes fondamentaux guide l’utilisateur dans votre application et le connecte à son expérience numérique en reflétant le monde physique. En savoir plus dans ces articles :

* La [vue d’ensemble des animations](../design/motion/index.md) a été mise à jour pour refléter ces principes fondamentaux.
* Les [animations en pratique](../design/motion/motion-in-practice.md) fournissent des exemples d’application de ces principes fondamentaux au sein de votre application.
* [Direction et gravité](../design/motion/directionality-and-gravity.md) consolide le modèle mental de l’utilisateur de votre application.
* [Minutage et accélération](../design/motion/timing-and-easing.md) ajoute du réalisme aux animations dans votre application.

![Animations en action](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Mises à jour Fluent Design

Des mises à jour visuelles et des modifications mineures ont été apportées pour les pages suivantes de Fluent Design :

* [Alignement, marge, remplissage](../design/layout/alignment-margin-padding.md)
* [Couleur](../design/style/color.md)
* [Notions de base sur les commandes](../design/basics/commanding-basics.md)
* [Système Fluent Design pour créateurs d’applications Windows](../design/fluent-design-system/index.md)
* [Présentation de la conception des applications UWP](../design/basics/design-and-ui-intro.md)
* [Notions de base sur la navigation](../design/basics/navigation-basics.md)
* [Techniques de conception réactive](../design/layout/responsive-design.md)
* [Tailles d’écran et points d’arrêt](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Vue d’ensemble du style](../design/style/index.md)
* [Style d’écriture](../design/style/writing-style.md)

En outre, nous avons réécrit les pages suivantes avec de toutes nouvelles informations sur leurs zones de contenu :

* La page [Icônes](../design/style/icons.md) fournit désormais des conseils pratiques relatifs à l’utilisation et l’interactivité des icônes.
* La page [Typographie](../design/style/typography.md) consolide les informations issues d’articles similaires, avec des instructions et des illustrations mises à jour.

![Image de palette de couleurs](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Fichiers du Programme d’installation d’application dans Visual Studio

Il est désormais possible de créer des fichiers Programme d’installation d’application avec la mise à jour 15.7 de Visual Studio 2017. [Découvrez comment utiliser Visual Studio pour créer un fichier Programme d’installation d’application](../packaging/create-appinstallerfile-vs.md) et activer les mises à jour automatiques pour vos applications. Si vous rencontrez des problèmes, consultez la page [Résoudre les problèmes d’installation avec le fichier du programme d’installation d’application](../packaging/troubleshoot-appinstaller-issues.md) pour afficher les problèmes courants et les solutions afférentes.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Contrôle Edge WebView pour les applications Windows Forms et WPF

Affichez le contenu web dans votre application de bureau en utilisant le contrôle WebView, auparavant disponible uniquement pour les applications UWP. Ce contrôle utilise le moteur de rendu Microsoft Edge pour incorporer l’affichage de contenus HTML présentant une mise en forme riche à partir d’un serveur web distant, de code généré dynamiquement ou de fichiers de contenu. Le contrôle WebView est disponible dans la dernière version du [Kit de ressources de la Communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/).

Recherchez d’autres contrôles de type WebView dans les futures versions du Kit de ressources de la Communauté Windows. Pour plus d’informations, consultez [Héberger des contrôles UWP dans les applications WPF et Windows Forms.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Entrées et interactions liées au pointage du regard

[Effectuez le suivi du pointage du regard, de l’attention et de la présence de l’utilisateur en fonction de l’emplacement et des mouvements de ses yeux.](../design/input/gaze-interactions.md) Cette nouvelle méthode performante pour utiliser vos applications UWP et interagir avec elles est particulièrement utile en tant que technologie d’assistance. Les entrées liées au pointage du regard fournissent également des opportunités intéressantes pour les jeux (acquisition de cible et suivi notamment) et d’autres scénarios interactifs où les périphériques d’entrée classiques (clavier, souris, technologie tactile) ne sont pas disponibles.

### <a name="msix-packaging-format"></a>Format d’empaquetage MSIX

Annoncé lors de la conférence Microsoft Build 2018, MSIX est un nouveau format de package de mise en conteneur qui s’applique à toutes les applications Windows, notamment Win32, Windows Forms, WPF et UWP. Ce nouveau format hérite des fonctionnalités exceptionnelles d’UWP :

* Installation et mise à jour robustes. 
* Modèle de sécurité géré avec système de capacité flexible.
* Prise en charge du Microsoft Store, de la gestion d’entreprise et de nombreux modèles de distribution personnalisés.

Les outils permettant de créer ces packages seront disponibles dans une prochaine version de Visual Studio et du Kit de développement logiciel Windows.

Le format d’empaquetage MSIX est un format open source, ce qui facilite pour nos partenaires la prise en charge de l’écosystème MSIX avec leurs outils et leurs solutions. Pour en savoir plus sur le format d’empaquetage MSIX, consultez la page [MSIX SDK](https://github.com/Microsoft/msix-packaging) (SDK MSIX). 

![Image d’empaquetage MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Packages facultatifs avec code exécutable

Les packages facultatifs dans votre application peuvent maintenant contenir du code C# exécutable. [Découvrez comment utiliser Visual Studio pour configurer des packages complémentaires facultatifs afin de prendre en charge de votre package d’application principal.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transitions de page

Les [transitions de page](../design/motion/page-transitions.md) permettent aux utilisateurs de naviguer entre les pages d’une application. Elles permettent aux utilisateurs de mieux comprendre où ils se trouvent dans la hiérarchie de navigation et fournissent des informations sur les relations entre les pages.

### <a name="project-rome"></a>Projet Rome

L’équipe Projet Rome a repensé les Kits de développement logiciel (SDK) iOS et Android, en ajoutant des fonctionnalités telles que les activités de l’utilisateur et en refactorisant une grande partie du code en vue d’offrir une expérience de programmation cohérente entre les différents SDK. [Les références API et les documents de procédure](https://docs.microsoft.com/windows/project-rome/) seront publiés au cours de la conférence Build 2018 destinée aux développeurs.

### <a name="sets"></a>Jeux

La fonctionnalité Jeux est disponible dans les versions d’évaluation de Windows Insider. Lorsque vous utilisez la fonctionnalité Jeux, votre application est envoyée vers une fenêtre qui peut être partagée avec d’autres applications, et chaque application dispose de son propre onglet dans la barre de titre. 

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="get-started"></a>Prise en main

Nous avons redynamisé notre contenu de prise en main avec de nouvelles pistes d’apprentissage. Ces nouvelles rubriques visent à fournir aux nouveaux développeurs Windows 10 des informations sur certaines tâches courantes qu’ils souhaiteront peut-être effectuer. Ce ne sont pas des didacticiels et elles n’offrent pas de démonstration à portée de main. Elles indiquent où se trouve la documentation existante et expliquent comment l’utiliser. Découvrez la nouvelle version de la page [Commencer le codage](../get-started/create-uwp-apps.md) ou explorez chaque piste d’apprentissage individuelle :

* [Créer un formulaire](../get-started/construct-form-learning-track.md)
* [Afficher les clients dans une liste](../get-started/display-customers-in-list-learning-track.md)
* [Enregistrer et charger des paramètres](../get-started/settings-learning-track.md)
* [Utiliser des fichiers](../get-started/fileio-learning-track.md)

![Image Prise en main](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Rapport sur les performances publicitaires

Le [rapport sur les performances publicitaires](../publish/advertising-performance-report.md) dans l’Espace partenaires propose désormais des mesures de visibilité. Nous avons également ajouté l’article [Optimiser la visibilité de vos unités publicitaires](../monetize/optimize-ad-unit-viewability.md) qui fournit des recommandations d’optimisation de la visibilité de vos annonces.

### <a name="targeted-push-notifications"></a>Notifications Push ciblées

La page [Notifications](../publish/send-push-notifications-to-your-apps-customers.md) de l’Espace partenaires fournit désormais des données d’analyse supplémentaires pour toutes vos notifications sous la forme de graphiques et de carte du monde.

## <a name="videos"></a>Vidéos

### <a name="cwinrt"></a>C++/WinRT

C++/WinRT est le nouveau moyen de créer et d’utiliser les API Windows Runtime. Cette méthode est implémentée exclusivement dans les fichiers d’en-tête et fournit un accès de première classe aux fonctionnalités d’application modernes. [Regardez la vidéo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) pour découvrir son fonctionnement, puis [lisez la documentation destinée aux développeurs](../cpp-and-winrt-apis/index.md) pour plus d’informations.

### <a name="multi-instance-uwp-apps"></a>Applications UWP à instances multiples

Windows vous permet désormais d’exécuter plusieurs instances de votre application UWP, chacune dans un processus distinct. [Regardez la vidéo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) pour savoir comment créer une application compatible avec cette fonctionnalité, puis [lisez la documentation destinée aux développeurs](../launch-resume/multi-instance-uwp.md) pour obtenir des informations sur la procédure relative à cette fonctionnalité et son utilité.

## <a name="samples"></a>Exemples

### <a name="customer-database-tutorial"></a>Didacticiel sur les bases de données de clients

Ce didacticiel crée une application UWP de base pour la gestion d’une liste de clients et introduit des concepts et des pratiques utiles dans le développement de l’entreprise. Il présente l’implémentation des éléments d’interface utilisateur et l’ajout d’opérations sur une base de données SQLite locale. Il offre aussi des conseils généraux de connexion à une base de données REST distante si vous souhaitez aller plus loin. [Consultez le didacticiel ici](../enterprise/customer-database-tutorial.md)