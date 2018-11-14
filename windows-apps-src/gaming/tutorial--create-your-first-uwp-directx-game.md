---
author: joannaleecy
title: Créer un jeu de plateforme Windows universelle (UWP) DirectX
description: Dans cet ensemble de didacticiels, vous allez apprendre à créer un jeu de plateforme Windows universelle (UWP, Universal Windows Platform) de base avec DirectX et C++.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: Exemple de jeu DirectX, exemple de jeu, plateforme Windows universelle (UWP), jeu Direct3D11
ms.author: joanlee
ms.date: 12/01/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1c589eeb71d93619b92254207ddc5eb1b1494a55
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6269975"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>Créer un jeu de plateforme Windows universelle (UWP) simple avec DirectX

Dans cet ensemble de didacticiels, vous allez apprendre à créer un jeu de plateforme Windows universelle (UWP) de base avec DirectX et C++. Nous abordons toutes les principales parties d’un jeu, y compris les processus de chargement de composants tels que les illustrations et maillages, de création d’une boucle de jeu principale, d’implémentation d’un pipeline de rendu simple et d’ajout de son et de contrôles.

Nous vous montrons les techniques et éléments à prendre en compte pour le développement de jeux UWP. Nous ne fournissons pas un jeu complet de bout en bout. Nous nous concentrons plutôt sur les principaux concepts de développement de jeux UWP DirectX, et faisons appel à des considérations spécifiques à Windows Runtime relatives à ces concepts.

## <a name="objective"></a>Objectif

Utiliser les composants et concepts de base d‘un jeu UWP DirectX et être plus à l’aise lors de la conception de jeux UWP avec DirectX.

## <a name="what-you-need-to-know-before-starting"></a>Ce que vous devez savoir avant de commencer


Avant de commencer ce didacticiel, vous devez être familiarisé avec les sujets suivants.

-   Microsoft C++ avec extensions de langage Windows Runtime (C++/CX). Il s’agit d’une mise à jour de Microsoft C++ qui incorpore le décompte de références automatique et qui est le langage de développement des jeux UWP avec DirectX 11.1 ou versions ultérieures.
-   Algèbre linéaire de base et concepts physiques newtoniens.
-   Terminologie de programmation de graphiques de base.
-   Concepts de programmation Windows de base.
-   Connaissances de base des API [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) et [Direct3D11](https://msdn.microsoft.com/library/windows/desktop/hh404569).

##  <a name="direct3d-uwp-shooting-game-sample"></a>Exemple de jeu de tir UWP Direct3D


Cet exemple implémente un pas de tir subjectif simple, où le joueur tire des balles sur des cibles en mouvement. Le joueur gagne un certain nombre de points en atteignant chaque cible et il peut traverser 6 niveaux de défis de plus en plus difficiles. À la fin des niveaux, les points sont comptés et un score final est attribué au joueur.

L’exemple illustre les concepts de jeu suivants :

-   Interopérabilité entre DirectX 11.1 et Windows Runtime
-   Caméra et perspective 3D subjectives
-   Effets 3D stéréoscopiques
-   Détection des collisions entre des objets en 3D
-   Gestion des entrées du joueur pour les contrôles tactiles, de souris et de manette Xbox
-   Mixage audio et lecture
-   Machine à états de jeu de base

![Exemple de jeu en action](images/simple-dx-game-overview.png)

| Rubrique | Description |
|-------|-------------|
|[Configurer le projet de jeu](tutorial--setting-up-the-games-infrastructure.md) | La première étape de l’assemblage de votre jeu consiste à configurer un projet dans Microsoft VisualStudio de façon à réduire la quantité de travail nécessaire sur l’infrastructure de code. Vous pouvez gagner du temps et éviter bien des tracas en utilisant le modèle approprié et en configurant le projet spécifiquement pour le développement de jeux. Nous vous guidons tout au long de l’installation et de la configuration d’un projet de jeu simple. |
| [Définir l’infrastructure d’application UWP du jeu](tutorial--building-the-games-uwp-app-framework.md) | Créez une infrastructure qui permet à l’objet jeu UWP DirectX d’interagir avec Windows. Cela comprend les propriétés WindowsRuntime telles que la gestion des événements de pause/reprise, la sélection de fenêtre et l’ancrage.  |
| [Gestion du flux de jeu](tutorial-game-flow-management.md) | Définissez la machine à états principale pour permettre l’interaction du joueur avec le système. Découvrez comment l’interface utilisateur interagit avec la machine à états du jeu globale et comment créer des gestionnaires d’événements pour les jeux UWP. |
| [Définir l’objet de jeu principal](tutorial--defining-the-main-game-loop.md) | Indiquez comment jouer en créant des règles. |
| [Infrastructure de renduI: présentation du rendu](tutorial--assembling-the-rendering-pipeline.md) | Assemblez une infrastructure de rendu pour afficher des graphiques. Cette rubrique est divisée en deux parties. La présentation du rendu explique comment présenter des objets de scène pour les afficher à l’écran. |
| [Infrastructure de renduII: rendu de jeu](tutorial-game-rendering.md) | Dans la deuxième partie de la rubrique de rendu, découvrez comment préparer les données requises avant d’effectuer le rendu. |
| [Ajouter une interface utilisateur](tutorial--adding-a-user-interface.md) | Ajoutez de simples options de menu et composants d’affichage à tête haute, en fournissant des commentaires au joueur. |
| [Ajouter des contrôles](tutorial--adding-controls.md) | Ajoutez des contrôles de déplacement/vue dans le jeu &mdash;: des contrôles tactiles, de manette de jeu et de souris de base. |
| [Ajouter du son](tutorial--adding-sound.md) | Découvrez comment créer des sons pour le jeu avec les API [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813). |
| [Étendre l’exemple de jeu](tutorial-resources.md) | Ressources pour approfondir vos connaissances en matière de développement de jeux DirectX, dont l’utilisation de XAML pour créer des superpositions. |