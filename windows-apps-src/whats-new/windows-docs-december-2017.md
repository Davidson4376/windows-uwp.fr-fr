---
author: QuinnRadich
title: Nouveautés apportées dans la documentation Windows en décembre2017 - Développer des applicationsUWP
description: De nouvelles fonctionnalités, des vidéos et des conseils aux développeurs ont été ajoutés à la documentation du développeur Windows10 en décembre2017.
keywords: nouveautés, mise à jour, fonctionnalités, conseils aux développeurs, Windows10, décembre
ms.author: quradic
ms.date: 12/14/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 93ef3de0dc86ab9708f7be99836204c2232dfef4
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2907776"
---
# <a name="whats-new-in-the-windows-developer-docs-in-december-2017"></a>Nouveautés apportées dans la documentation du développeur Windows en décembre2017

La documentation du développeur Windows est constamment mise à jour afin d'intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Les présentations de fonctionnalités, les conseils aux développeurs et les exemples qui suivent ont été récemment mis à disposition, après la parution de la mise à jour Fall Creators Update. Ils contiennent des informations nouvelles et mises à jour destinées aux développeurs Windows.

[Installez les outils et le kit de développement logiciel (SDK)](http://go.microsoft.com/fwlink/?LinkId=821431) sur Windows10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou découvrir comment utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="windows-mixed-reality-enthusiasts-guide"></a>Windows Mixed Reality: Guide pour les fans

Particulièrement destiné aux passionnés de technologie immergés dans l'univers de la réalité mixte, le [Guide des fans](https://docs.microsoft.com/en-us/windows/mixed-reality/enthusiast-guide/) répond aux principales questions posées sur Windows Mixed Reality. 

Vous trouverez dans ce guide: 
- Le Forum aux questions avant d’acheter, 
- Comment vérifier la compatibilité de votre PC, 
- Des instructions de configuration, 
- Comment utiliser le casque et les contrôleurs, 
- Comment télécharger et jouer à des jeux immersifs, lire des vidéos à 360°, des applications2D, WebVR et SteamVR, 
- Comment résoudre les problèmes, et bien plus encore.

![Utilisateur du casque Windows Mixed Reality et des contrôleurs de mouvement](images/BeforeYouBegin-tile.jpg)

### <a name="keyboard-interactions"></a>Interactions avec le clavier

Concevez et optimisez vos applications UWP pour offrir une expérience accessible et des fonctionnalités aux utilisateurs avancés, avec la mise à jour des [interactions avec le clavier](../design/input/keyboard-interactions.md). Nous avons mis à jour nos recommandations et nos conseils pour prendre en compte les nouvelles améliorations de ces interactions, apportées dans la mise à jour Fall Creators Update.

Voir [Raccourcis clavier](../design/input/keyboard-accelerators.md) et [Personnaliser les interactions clavier](../design/input/custom-keyboard-interactions.md) pour étendre les fonctionnalités au clavier de vos applications.

Sur les appareils qui prennent en charge les interactions tactiles, ajoutez des fonctionnalités au clavier en vous inspirant des articles [Répondre à la présence du clavier tactile](../design/input/respond-to-the-presence-of-the-touch-keyboard.md) et [Utiliser l’étendue des entrées pour modifier le clavier tactile](../design/input/use-input-scope-to-change-the-touch-keyboard.md).

### <a name="microsoft-collaborate"></a>Microsoft Collaborate

Le portail MicrosoftCollaborate fournit des outils et des services permettant de simplifier la collaboration d'ingénierie dans l’écosystème Microsoft. Il permet de partager des éléments de travail (bogues, des demandes de fonctionnalités, etc.) et de diffuser du contenu (builds, documents, spécifications). [En savoir plus](https://docs.microsoft.com/en-us/collaborate).

![Microsoft Collaborate dans le tableau de bord du Centre de développement](images/microsoft_collaborate_screenshot.PNG)

### <a name="package-desktop-applications-with-uwp-projects"></a>Créer des packages d'application de bureau avec les projets UWP

Visual Studio2017 version15.5 a mis à jour le modèle **Projet de création de packages d’application Windows**, de sorte qu’il beaucoup plus facile d'y inclure un projet UWP. Il n'est plus nécessaire d'utiliser un projet de création de package JavaScript, puis d'ajuster manuellement le manifeste du package.  

Voir [Créer un package d'application à l’aide de Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-packaging-dot-net) pour obtenir des conseils sur l’utilisation de ce nouveau modèle pour empaqueter votre application de bureau.

Voir [Étendre votre application de bureau avec des composants UWP modernes](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) pour obtenir des instructions sur l’ajout d’un projet UWP à votre package.

### <a name="subscription-add-ons-are-now-available-to-developers-in-the-windows-dev-center-insider-program"></a>Les extensions d’abonnement sont désormais disponibles pour les développeurs dans le programme Insider du Centre de développement Windows

Tous les développeurs qui ont rejoint le [programme Insider du Centre de développement](../publish/dev-center-insider-program.md) peuvent désormais utiliser les extensions d’abonnement pour vendre des produits numériques dans leurs applications (comme des fonctionnalités d'application ou du contenu numérique) avec une facturation périodique automatique; Pour plus d’informations, consultez l’article [Activer les extensions d’abonnement de votre application](../monetize/enable-subscription-add-ons-for-your-app.md).

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="color"></a>Couleur

Nous avons ajouté quelques nouvelles recommandations sur l’utilisation des couleurs dans vos applications, afin d'offrir aux utilisateurs la meilleure expérience possible. Elles comprennent des scénarios d’utilisation d'API ainsi que des instructions générales sur la conception d’une interface utilisateur et l’accessibilité. Nous avons également mis à jour la liste des couleurs d’accentuation disponibles sur Xbox. [Consultez ici l’article sur les couleurs mis à jour.](../design/style/color.md)

![palette de couleurs windows universelle](../design/basics/images/colors.png)

### <a name="data-access-guides"></a>Guides d'accès aux données

Nous avons ajouté un [Guide SQLServer](../data-access/sql-server-databases.md) pour montrer de quelle manière votre application peut accéder directement à une base de données SQLServer. Aucune couche de service n'est requise.

En outre, nous avons complètement remodelé notre [guide SQLite](../data-access/sqlite-databases.md) pour le rendre plus clair, et nous y avons intégré les dernières bonnes pratiques de stockage et d'extraction de données dans une base de données légère sur l’appareil des utilisateurs.

### <a name="forms"></a>Formulaires

Nous avons ajouté un article sur [comment construire des formulaires dans les applications](../design/controls-and-patterns/forms.md), les formulaires étant destinés à recueillir et soumettre des données des utilisateurs. Cet article comprend des informations spécifiques sur l’implémentation des formulaires et des recommandations d’ordre général sur le lieu et le moment de leur utilisation.

### <a name="intro-to-app-design"></a>Introduction à la conception d'application

Le guide de conception pour la plateforme Windows universelle (UWP) est une ressource destinée à vous aider à concevoir et des applications belles et élégantes. [Notre nouvelle introduction](../design/basics/design-and-ui-intro.md) présente une vue d’ensemble des fonctionnalités de conception universelle qui sont incluses dans chaque application UWP. Elle explique en outre comment utiliser la documentation pour créer des interfaces utilisateur (UI) qui s’adaptent parfaitement à toute une gamme d’appareils.


### <a name="request-ratings-and-reviews"></a>Demander des évaluations et des avis

Nous avons ajouté un article qui explique comment [demander des évaluations et des avis pour votre application](../monetize/request-ratings-and-reviews.md). Vous pouvez afficher une boîte de dialogue d'évaluation et d'avis dans le contexte de votre application, ou vous pouvez ouvrir la page évaluation et avis de votre application dans le Store.

## <a name="samples"></a>Exemples

### <a name="customer-orders"></a>Commandes des clients

L'exemple de [base de données de commandes client](https://github.com/Microsoft/Windows-appsample-customers-orders-database) a été mis à jour de façon à afficher les bonnes pratiques concernant l’accès des données, telles que l'utilisation d'un modèle de référentiel et la manière de connecter plusieurs sources de données (notamment Sqlite, SQL Azure et un service REST).

## <a name="videos"></a>Vidéos

### <a name="package-a-net-app-in-visual-studio"></a>Créer un package d’application .NET dans Visual Studio

Porter votre application de bureau vers la plateforme Windows universelle (UWP) est plus simple que jamais. [Regardez la vidéo](https://www.youtube.com/watch?v=fJkbYPyd08w) pour savoir comment empaqueter votre application .NET pour la distribution, puis [consultez cette page](../porting/desktop-to-uwp-packaging-dot-net.md) pour plus d’informations.