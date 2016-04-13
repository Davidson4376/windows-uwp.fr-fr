---
title: Jeux et DirectX
description: La plateforme Windows universelle (UWP) offre de nouvelles opportunités sur le plan de la création, de la distribution et de la monétisation de jeux. Découvrez comment créer un nouveau jeu ou porter un jeu existant.
ms.assetid: 4073b835-c900-4ff2-9fc5-da52f9432a1f
---

# Jeux et DirectX


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

La plateforme Windows universelle (UWP) offre de nouvelles opportunités sur le plan de la création, de la distribution et de la monétisation de jeux. Découvrez comment créer un nouveau jeu ou porter un jeu existant.

| Rubrique | Description |
|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Guide de développement de jeux Windows 10](e2e.md) | Guide complet sur les ressources et les informations nécessaires au développement de jeux UWP. |
| [Technologies de jeu pour les applications de plateforme Windows universelle](game-development-platform-guide.md) | Ce guide décrit les technologies disponibles pour le développement de jeux UWP. |
| [Modèles de projet et outils pour les jeux](prepare-your-dev-environment-for-windows-store-directx-game-development.md) | Décrit ce dont vous avez besoin pour commencer à programmer des jeux DirectX pour UWP. |
| [Objet application et DirectX](about-the-metro-style-user-interface-and-directx.md) | Les applications UWP intégrant des jeux DirectX n’utilisent pas beaucoup d’éléments et d’objets d’interface utilisateur Windows. En effet, comme elles s’exécutent à un niveau inférieur de la pile Windows Runtime, elles doivent interopérer avec l’infrastructure d’interface utilisateur d’une manière plus basique, c’est-à-dire en accédant directement à l’objet application et en interopérant avec lui. Découvrez quand et comment cette interopération se produit et comment vous pouvez, en tant que développeur DirectX, exploiter efficacement ce modèle dans le cadre du développement de vos applications UWP. |
| [Lancement et reprise des applications](launching-and-resuming-apps-directx-and-cpp.md) | Découvrez comment lancer, suspendre et reprendre votre application DirectX UWP. |
| [Graphismes 2D pour jeux DirectX](working-with-2d-graphics-in-your-directx-game.md) | Nous allons découvrir comment utiliser les effets et les graphismes bitmap 2D, puis comment vous en servir dans votre jeu. |
| [Graphismes 3D de base pour jeux DirectX](an-introduction-to-3d-graphics-with-directx.md) | Nous vous expliquons comment utiliser la programmation DirectX pour implémenter les concepts fondamentaux des graphismes 3D. |
| [Charger des ressources dans votre jeu DirectX](load-a-game-asset.md) | La plupart des jeux, à un moment donné, chargent des ressources et des éléments multimédias (comme les nuanceurs, les textures, les maillages prédéfinis ou d’autres données graphiques) à partir d’un stockage local ou d’autres flux de données. Cette rubrique offre une vue d’ensemble des éléments à prendre en compte lors du chargement de ces fichiers en vue de leur utilisation dans votre jeu UWP. |
| [Créer un jeu UWP simple avec DirectX](tutorial--create-your-first-metro-style-directx-game.md) | Dans cet ensemble de didacticiels, vous allez apprendre à créer un jeu UWP de base avec DirectX et C++. Nous abordons toutes les principales parties d’un jeu, y compris les processus de chargement d’éléments multimédias tels que les illustrations et maillages, de création d’une boucle de jeu principale, d’implémentation d’un pipeline de rendu simple et d’ajout de son et de contrôles. |
| [Développement de Marble Maze, jeu pour la plateforme Windows universelle en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md) | Cette section de la documentation décrit comment utiliser DirectX et Visual C++ pour créer un jeu UWP en 3D. Cette documentation indique comment créer un jeu en 3D appelé Marble Maze qui tient compte des nouveaux facteurs de formes pour s’adapter aux tablettes, mais qui fonctionne également sur les ordinateurs de bureau et les ordinateurs portables classiques. |
| [Prise en charge de l’orientation de l’écran](supporting-screen-rotation-directx-and-cpp.md) | Dans cette rubrique, nous allons examiner les meilleures pratiques en matière de gestion de la rotation écran dans votre application DirectX UWP de manière à utiliser efficacement le matériel graphique de l’appareil Windows 10. |
| [Audio pour les jeux](working-with-audio-in-your-directx-game.md) | Apprenez à développer et à incorporer de la musique et des sons dans votre jeu DirectX, et à traiter les signaux audio afin de créer des sons dynamiques et positionnels. |
| [Contrôles tactiles pour les jeux](tutorial--adding-touch-controls-to-your-directx-game.md) | Découvrez comment ajouter des contrôles tactiles de base à votre jeu UWP en C++ avec DirectX. Nous allons vous montrer comment ajouter des contrôles tactiles pour déplacer une caméra de plan fixe dans un environnement Direct3D, où le glissement avec un doigt ou un stylet décale la perspective de la caméra. |
| [Contrôles de déplacement/vue pour les jeux](tutorial--adding-move-look-controls-to-your-directx-game.md) | Découvrez comment ajouter des contrôles de déplacement/vue de souris et de clavier classiques (également connus sous le nom de contrôles de vue à la souris) à votre jeu DirectX. |
| [Optimiser la boucle d’entrée et de rendu](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) | La latence d’entrée peut avoir un impact important sur un jeu. Son optimisation peut rendre un jeu plus fluide. De plus, une optimisation appropriée des événements d’entrée peut améliorer l’autonomie de la batterie. Apprenez à choisir les options de traitement appropriées de l’événement d’entrée [CoreDispatcher](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) pour vous assurer que votre jeu gère les entrées de façon aussi fluide que possible. |
| [Mise à l’échelle et superpositions de chaînes d’échange](multisampling--scaling--and-overlay-swap-chains.md) | Apprenez à créer des chaînes d’échange mises à l’échelle pour accélérer le rendu sur les appareils mobiles, et utilisez la superposition des chaînes d’échange (quand cela est possible) pour améliorer la qualité visuelle. |
| [Réduire la latence avec des chaînes d’échange DXGI 1.3](reduce-latency-with-dxgi-1-3-swap-chains.md) | Utilisez DXGI 1.3 pour réduire la latence d’image effective en attendant que la chaîne d’échange indique le moment approprié pour débuter le rendu d’une nouvelle image. |
| [Échantillonnage multiple dans les applications UWP](multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md) | Découvrez comment utiliser l’échantillonnage multiple dans des applications UWP générées avec Direct3D. |
| [Gérer des scénarios de suppression d’appareils dans Direct3D 11](handling-device-lost-scenarios.md) | Cette rubrique explique comment recréer la chaîne d’interface d’appareils Direct3D et DXGI quand la carte graphique est supprimée ou réinitialisée. |
| [Programmation asynchrone pour les jeux](asynchronous-programming-directx-and-cpp.md) | Cette rubrique traite des divers points à prendre en considération lorsque vous utilisez la programmation asynchrone et les threads avec DirectX. |
| [Mise en réseau pour les jeux](work-with-networking-in-your-directx-game.md) | Apprenez à développer et à incorporer des fonctionnalités réseau dans votre jeu DirectX. |
| [Technologie interop DirectX et XAML](directx-and-xaml-interop.md) | Vous pouvez utiliser XAML (Extensible Application Markup Language) et Microsoft DirectX conjointement dans votre jeu UWP. |
| [Créer un package de votre jeu](package-your-windows-store-directx-game.md) | Certains jeux UWP qui prennent notamment en charge plusieurs langues et comprennent des éléments multimédias spécifiques à la région ou des éléments multimédias haute définition facultatifs peuvent facilement devenir très volumineux. Dans cette rubrique, découvrez comment utiliser les packages et ensembles d’applications pour personnaliser vos applications afin que vos clients ne reçoivent que les ressources dont ils ont réellement besoin. |
| [Guides en matière de portage de jeu](porting-guides.md) | Fournit des guides relatifs au portage de vos jeux existants vers Direct3D 11, UWP et Windows 10. |
| [Ressources de programmation de jeux](additional-directx-game-programming-resources.md) | Pour plus d’informations sur la programmation de jeux sur Windows, consultez les ressources suivantes. |

 

> **Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui développent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

Pour utiliser au mieux les vues d’ensemble du développement de jeux et des didacticiels, vous devez être familiarisé avec les sujets suivants :

-   Microsoft C++ avec extensions des composants (C++/CX). Il s’agit d’une mise à jour de Microsoft C++ qui incorpore le décompte de références automatique et qui est le langage de développement des jeux pour UWP avec DirectX 11.1 ou versions ultérieures.
-   Terminologie de programmation de graphiques de base.
-   Concepts de programmation Windows de base.
-   Connaissances de base des API Direct3D 9 ou 11.

 

 






<!--HONumber=Mar16_HO1-->


