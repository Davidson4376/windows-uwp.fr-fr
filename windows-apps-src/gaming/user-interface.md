---
title: Modèles de projet de jeu DirectX
description: Découvrez les modèles pour la création d’un jeu pour la plateforme Windows universelle (UWP) et DirectX.
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, jeux, directx, modèles
ms.localizationpriority: medium
ms.openlocfilehash: 668a41a69c2b7dab338d251d95e23e801fa85cf6
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321140"
---
# <a name="directx-game-project-templates"></a>Modèles de projet de jeu DirectX



Les modèles DirectX et de plateforme Windows universelle (UWP) vous permettent de créer rapidement un projet en tant que point de départ pour votre jeu.

## <a name="prerequisites"></a>Prérequis


Pour créer le projet, procédez comme suit :

-   [Téléchargez Microsoft Visual Studio 2015](https://visualstudio.microsoft.com/vs/). Visual Studio 2015 dispose d’outils pour la programmation, tels que des outils de débogage graphique. Pour obtenir une vue d’ensemble des fonctionnalités et outils graphiques et de jeux DirectX, voir [Outils Visual Studio pour le développement de jeux DirectX](set-up-visual-studio-for-game-development.md).

## <a name="choosing-a-template"></a>Choix d’un modèle


Visual Studio 2015 comprend trois modèles de DirectX et UWP :

-   Application DirectX 11 (Windows universel) : le modèle Application DirectX 11 (Windows universel) crée un projet UWP dont le rendu s’effectue directement dans une fenêtre d’application à l’aide de DirectX 11.
-   Application DirectX 12 (Windows universel) : le modèle Application DirectX 12 (Windows universel) crée un projet UWP dont le rendu s’effectue directement dans une fenêtre d’application à l’aide de DirectX 12.
-   Application DirectX 11 et XAML (Windows universel) : le modèle Application DirectX 11 et XAML (Windows universel) crée un projet UWP dont le rendu s’effectue à l’intérieur d’un contrôle XAML à l’aide de DirectX 11. Ce modèle utilise une classe [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) pour vous permettre d’utiliser des contrôles d’interface utilisateur XAML. L’utilisation du modèle XAML peut donc faciliter l’ajout d’éléments d’interface utilisateur, mais risque également de réduire les performances.

Le modèle que vous choisissez dépend des performances et des technologies que vous souhaitez utiliser.

## <a name="template-structure"></a>Structure du modèle


Les modèles Windows universels DirectX contiennent les fichiers suivants :

-   pch.h et pch.cpp : prise en charge de l’en-tête précompilé.
-   Package.appxmanifest : propriétés du package de déploiement de l’application.
-   \*.pfx - certificats pour l’application.
-   Dépendances externes : liens vers des fichiers externes que le projet utilise.
-   \*Main.h et \*Main.cpp - méthodes pour la gestion des ressources de l’application, la mise à jour d’état de l’application et le frame de rendu.
-   App.h et App.cpp : point d’entrée principal de l’application. Connecte l’application avec l’interpréteur de commandes Windows et gère les événements de cycle de vie de l’application. Ces fichiers s’affichent uniquement dans les modèles d’application DirectX 11 (Windows universelle) et les modèles d’application DirectX 12 (Windows universelle).
-   App.xaml, App.xaml.cpp et App.xaml.h : point d’entrée principal pour l’application. Connecte l’application avec l’interpréteur de commandes Windows et gère les événements de cycle de vie de l’application. Ces fichiers s’affichent uniquement dans le modèle d’application Direct 11 et XAML (Windows universelle).
-   DirectXPage.xaml, DirectXPage.xaml.cpp et DirectXPage.xaml.h : page qui héberge un SwapChainPanel DirectX. Ces fichiers s’affichent uniquement dans le modèle d’application Direct 11 et XAML (Windows universelle).
-   Contenu
    -   Sample3DSceneRenderer.h et Sample3DSceneRenderer.cpp : convertisseur d’échantillon qui instancie un pipeline de rendu de base.
    -   SampleFpsTextRenderer.h et SampleFpsTextRenderer.cpp : rend la valeur actuelle de fréquence d’images dans l’angle inférieur droit de l’écran utilisant Direct2D et DirectWrite. Ces fichiers s’affichent uniquement dans les modèles d’application DirectX 11 (Windows universelle) et d’application DirectX 11 et WAML (Windows universelle).
    -   SamplePixelShader.hlsl : exemple simple de nuanceur de pixels.
    -   SampleVertexShader.hlsl : exemple simple de nuanceur de vertex.
    -   ShaderStructures.h : structures utilisées pour envoyer une date à l’exemple de nuanceur de vertex.
-   Commun
    -   StepTimer.h : classe d’assistance pour le minutage d’animation et de simulation.
    -   DirectXHelper.h : fonctions d’assistance diverses.
    -   DeviceResources.h et Device Resources.cpp : fournit une interface pour une application qui détient DeviceResources pour être avertie de la perte ou de la création de l’appareil.
    -   d3dx12.h : contient la bibliothèque d’utilitaires D3DX12. Ce fichier s’affiche uniquement dans l’application DirectX 12 (Windows universelle).
-   Ressources : images de logo et d’élément SplashScreen utilisées par l’application.

## <a name="next-steps"></a>Étapes suivantes


Vous disposez à présent d’un point de départ. Apportez-y vos connaissances et vos compétences de développeur de jeux, notamment en matière de jeux pour le Microsoft Store.

Si vous portez un jeu existant, voir les rubriques suivantes.

-   [Portage depuis OpenGL ES 2.0 vers Direct3D 11.1](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [Port à partir de DirectX 9 à Universal Windows Platform](porting-your-directx-9-game-to-windows-store.md)

Si vous créez un jeu DirectX, voir les rubriques suivantes.

-   [Créez un jeu UWP simple avec DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Développement de Marble Maze, un jeu de plateforme Windows universelle en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)