---
author: QuinnRadich
title: Nouveautés dans les documents Windows 2018 mai - pour développer des applications UWP
description: Nouvelles fonctionnalités, des vidéos et des instructions pour les développeurs ont été ajoutés à la documentation du développeur Windows 10 pour mai 2018 et la conférence Microsoft Build.
keywords: Quelles sont les nouveautés, mettre à jour, des fonctionnalités, des conseils pour les développeurs, Windows 10, mai, build
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2887868"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Nouveautés dans la documentation de développeur Windows 2018 mai

La documentation du développeur Windows est constamment mise à jour afin d'intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Ont été apportées disponibles dans le mois de mai coïncider avec la conférence de développeurs [Microsoft Build 2018](https://www.microsoft.com/build) suivant vues d’ensemble de la fonctionnalité, des instructions pour les développeurs, vidéos et exemples.

[Installez les outils et le kit de développement logiciel (SDK)](http://go.microsoft.com/fwlink/?LinkId=821431) sur Windows10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou découvrir comment utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="motion-in-fluent-design"></a>Mouvement de conception Fluent

L’utilisateur de mouvement dans le système de conception Fluent évolue, basé sur les concepts fondamentaux de minutage d’accélération, directions et gravité. L’application de ces principes de base vous aideront à l’utilisateur par le biais de votre application et les relie avec leur expérience numérique en reflétant le monde physique. Pour plus d’informations dans cet article:

* [Vue d’ensemble du mouvement](../design/motion/index.md) a été mis à jour pour refléter ces principes de base.
* [Pratique de mouvement](../design/motion/motion-in-practice.md) fournit des exemples de la façon d’appliquer ces principes de base au sein de votre application.
* [Directions et gravité](../design/motion/directionality-and-gravity.md) solidification modèle mental de l’utilisateur de votre application.
* [Minutage et accélération](../design/motion/timing-and-easing.md) ajoute réalisme à l’animation dans votre application.

![Mouvement en action](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Mises à jour de la conception Fluent

Les modifications mineures et les mises à jour Visual ont été apportées aux pages de conception Fluent suivantes:

* [Alignement, le remplissage, les marges](../design/layout/alignment-margin-padding.md)
* [Couleur](../design/style/color.md)
* [Notions de base sur les commandes](../design/basics/commanding-basics.md)
* [Conception Fluent pour les applications Windows](../design/fluent-design-system/index.md)
* [Introduction à la conception de l’application](../design/basics/design-and-ui-intro.md)
* [Notions de base sur la navigation](../design/basics/navigation-basics.md)
* [Techniques de conception réactive](../design/layout/responsive-design.md)
* [Tailles d’écran et points d’arrêt](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Vue d’ensemble de style](../design/style/index.md)
* [Style d’écriture](../design/style/writing-style.md)

En outre, nous avons réécrit les pages suivantes avec toute nouvelle plus d’informations sur les zones de contenu:

* [Icônes](../design/style/icons.md) maintenant fournit des recommandations pratiques pour l’utilisation des icônes et les rendre interactif.
* [Typographie](../design/style/typography.md) regroupe les informations provenant des articles similaires, tout insérer dans un seul emplacement avec la mise à jour des instructions et des illustrations.

![Image de palette de couleurs](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Fichiers de programme d’installation de l’application dans Visual Studio

Fichiers d’installation d’application maintenant peuvent être créés avec Visual Studio 2017, 15.7 mise à jour. [Découvrez comment utiliser Visual Studio pour créer un fichier d’installation de l’application](../packaging/create-appinstallerfile-vs.md) et activer mises à jour automatiques à vos applications. Si vous exécutez des problèmes, voir [résolution des problèmes d’installation avec le fichier d’installation de l’application](../packaging/troubleshoot-appinstaller-issues.md) pour afficher les problèmes et solutions courants.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Bord du contrôle d’affichage Web pour les applications Windows Forms et WPF

Affichage du contenu web dans votre application de bureau à l’aide du contrôle en mode Web, auparavant uniquement disponible pour les applications UWP. Ce contrôle utilise le rendu du contenu à partir d’un serveur web distant, code généré dynamiquement ou fichiers de contenu de moteur d’incorporer un affichage que restitue mise en forme enrichie HTML Microsoft Edge. Recherchez le contrôle de l’affichage Web dans la dernière version de la [Kit de ressources de la Communauté Windows.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Recherchez d’autres contrôles, comme l’affichage Web dans les versions futures de la boîte à outils de la Communauté Windows. Pour plus d’informations, voir [hôte UWP des contrôles dans les applications WPF et Windows Forms.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Remplacez les entrées et interactions

[Effectuez le suivi du pointage du regard, de l'attention et de la présence de l'utilisateur en fonction de l’emplacement et des mouvements de ses yeux.](../design/input/gaze-interactions.md) Cette nouvelle méthode performante à utiliser et d’interagir avec vos applications UWP est particulièrement utile comme une technologie d’aide. Entrée regard fournit également des opportunités pour les jeux (y compris acquisition cible et suivi) et les autres scénarios interactifs où traditionnels périphériques d’entrée (clavier, souris, tactile) ne sont pas disponibles.

### <a name="msix-packaging-format"></a>Format de Packaging MSIX

Annoncé à la conférence Microsoft Build 2018, MSIX est un nouveau format de conteneurs sont qui s’applique à toutes les applications Windows, y compris Win32, Windows Forms, WPF et UWP. Ce nouveau format hérite de fonctionnalités UWP:

* Installation robuste et mise à jour. 
* Gérés de modèle de sécurité avec un système de fonctionnalité flexible.
* Prise en charge de Microsoft Store, la gestion de l’entreprise et d’autres modèles de distribution personnalisés.

Outils permettant de créer ces packages sera disponibles dans une version future de Visual Studio et le Kit de développement logiciel Windows.

Le format de packaging MSIX est un format open source qui facilite pour les partenaires prendre en charge de l’écosystème MSIX avec leurs solutions et les outils. Pour en savoir plus sur le format de packaging MSIX, voir le [SDK MSIX](https://github.com/Microsoft/msix-packaging). 

![Image de packaging MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Packages facultatifs avec code exécutable

Modules facultatifs dans votre application peuvent maintenant contenir le code c# exécutable. [Découvrez comment utiliser Visual Studio pour configurer les packages de module complémentaire facultatif pour prendre en charge votre package d’application principale.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transitions de page

[Transitions de page](../design/motion/page-transitions.md) naviguer utilisateurs entre les pages dans une application. Elles permettent aux utilisateurs de comprendre où ils se trouvent dans la hiérarchie de navigation et fournissez des commentaires sur la relation entre les pages.

### <a name="project-rome"></a>Projet «Rome»

L’équipe de projet Rome a remodelés leur iOS et les kits de développement Android, les nouvelles fonctionnalités telles que des activités de l’utilisateur et refactorisation de leur code pour fournir une expérience de programmation cohérente entre les différents kits de développement. [Tous les nouveaux référence des API et des documents](https://docs.microsoft.com/windows/project-rome/) sera opérationnel au cours de la conférence de développeurs 2018 Build.

### <a name="sets"></a>Ensembles de

La fonctionnalité ensembles est disponible dans Windows initiés preview génère. Lorsque vous utilisez la fonctionnalité ensembles, vous application est dessinée dans une fenêtre qui peut-être être partagée avec d’autres applications, avec chaque application ayant son propre onglet dans la barre de titre. [Conception pour les jeux](../design/shell/design-for-sets.md) a des instructions sur la façon d’optimiser votre application pour fournir la meilleure expérience dans l’interface utilisateur définit.

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="get-started"></a>Prise en main

Nous avons aux retraités rajeunit notre Get en route avec les nouvelles pistes de formation. Ces nouvelles rubriques visent à fournir des nouveaux développeurs Windows 10 avec des informations sur certaines tâches courantes, ils peuvent souhaiter faire. Ils ne sont pas des didacticiels et ne fournissent pas une procédure pas à pas portables, mais au lieu de cela, signalez lorsqu’il existe une documentation existante et comment l’utiliser. Extraire le revisitée [Démarrer le codage](../get-started/create-uwp-apps.md) de page ou explorez chaque piste learning individuels:

* [Construire un formulaire](../get-started/construct-form-learning-track.md)
* [Afficher les clients dans une liste](../get-started/display-customers-in-list-learning-track.md)
* [Paramètres d'enregistrement et de chargement](../get-started/settings-learning-track.md)
* [Travailler avec des fichiers](../get-started/fileio-learning-track.md)

![Obtenir une image prise en main](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Rapport sur les performances publicitaires

Le [rapport de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre pour développeurs fournit maintenant des mesures de la plupart des ordinateurs. Nous avons également ajouté l’article [optimiser la plupart des ordinateurs de vos unités ad](../monetize/optimize-ad-unit-viewability.md) pour fournir des recommandations pour l’optimisation de la plupart des ordinateurs de vos annonces.

### <a name="targeted-push-notifications"></a>Notifications push ciblé

La page [Notifications](../publish/send-push-notifications-to-your-apps-customers.md) dans le tableau de bord du Centre pour développeurs fournit désormais des données supplémentaires analytique pour toutes vos notifications dans les affichages de carte graphique et le monde.

## <a name="videos"></a>Vidéos

### <a name="cwinrt"></a>C++/WinRT

C + / WinRT est une nouvelle façon de création et d’utilisation APIs Windows Runtime. Elle a implémenté unique dans les fichiers d’en-tête et conçus pour vous fournir un accès aux fonctionnalités de l’application moderne de première classe. [Regarder la vidéo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) pour découvrir comment il fonctionne, puis [Lisez la documentation du développeur](../cpp-and-winrt-apis/index.md) pour plus d’informations.

### <a name="multi-instance-uwp-apps"></a>Applications UWP à instances multiples

Windows vous permet désormais d’exécuter plusieurs instances de votre application UWP, chacun dans son propre processus distinct. [Regarder la vidéo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) pour découvrir comment créer une nouvelle application qui prend en charge cette fonctionnalité, puis [Lisez la documentation du développeur](../launch-resume/multi-instance-uwp.md) pour obtenir des instructions sur la manière et pourquoi utiliser cette fonctionnalité.

## <a name="samples"></a>Exemples

### <a name="customer-database-tutorial"></a>Didacticiel de base de données client

Ce didacticiel crée une application UWP de base pour la gestion d’une liste de clients et présente les concepts et les pratiques utiles dans le développement de l’entreprise. Il vous guide par le biais de l’implémentation des éléments d’interface utilisateur et ajout d’opérations par rapport à une base de données locale SQLite et fournit des conseils en vrac pour se connecter à une base de données distante reste si vous souhaitez aller plus loin. [Extraire le didacticiel ici](../enterprise/customer-database-tutorial.md)