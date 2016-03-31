---
title: Créer un jeu de plateforme Windows universelle (UWP) simple avec DirectX
description: Dans cet ensemble de didacticiels, vous allez apprendre à créer un jeu de plateforme Windows universelle (UWP) de base avec DirectX et C++.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
mots clés : [« exemple de jeu DirectX », « exemple de jeu, plateforme Windows universelle (UWP) », « jeu Direct3D 11 »]
---

# Créer un jeu de plateforme Windows universelle (UWP) simple avec DirectX


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Dans cet ensemble de didacticiels, vous allez apprendre à créer un jeu de plateforme Windows universelle (UWP) de base avec DirectX et C++. Nous abordons toutes les principales parties d’un jeu, y compris les processus de chargement de composants tels que les illustrations et maillages, de création d’une boucle de jeu principale, d’implémentation d’un pipeline de rendu simple et d’ajout de son et de contrôles.

Nous vous montrons les techniques et éléments à prendre en compte pour le développement de jeux UWP. Nous ne fournissons pas un jeu complet de bout en bout. Nous nous concentrons plutôt sur les principaux concepts de développement de jeux UWP DirectX, et faisons appel à des considérations spécifiques à Windows Runtime relatives à ces concepts.

## Objectif


-   Utiliser les composants et concepts de base d‘un jeu UWP DirectX et être plus à l’aise lors de la conception de jeux UWP avec DirectX.

## Ce que vous devez savoir avant de commencer


Avant de commencer ce didacticiel, vous devez être familiarisé avec les sujets suivants.

-   Microsoft C++ avec extensions des composants (C++/CX). Il s’agit d’une mise à jour de Microsoft C++ qui incorpore le décompte de références automatique et qui est le langage de développement des jeux UWP avec DirectX 11.1 ou versions ultérieures.
-   Algèbre linéaire de base et concepts physiques newtoniens.
-   Terminologie de programmation de graphiques de base.
-   Concepts de programmation Windows de base.
-   Connaissances de base des API [Direct2D](https://msdn.microsoft.com/en-us/library/windows/apps/dd370990.aspx) et [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh404569).

##  Exemple de jeu de tir Direct3D Windows Store


Cet exemple implémente un pas de tir subjectif simple, où le joueur tire des balles sur des cibles en mouvement. Le joueur gagne un certain nombre de points en atteignant chaque cible et il peut traverser 6 niveaux de défis de plus en plus difficiles. À la fin des niveaux, les points sont comptés et un score final est attribué au joueur.

L’exemple illustre les concepts de jeu suivants :

-   Interopérabilité entre DirectX 11.1 et Windows Runtime
-   Caméra et perspective 3D subjectives
-   Effets 3D stéréoscopiques
-   Détection des collisions entre des objets en 3D
-   Gestion des entrées du joueur pour les contrôles tactiles, de souris et de manette Xbox 360
-   Mixage audio et lecture
-   Machine à états de jeu de base

![Exemple de jeu en action](images/simple3dgame-display.png)


| Rubrique | Description |
|---------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Configurer le projet de jeu](tutorial--setting-up-the-games-infrastructure.md) | La première étape de l’assemblage de votre jeu consiste à configurer un projet dans Microsoft Visual Studio de façon à réduire la quantité de travail sur l’infrastructure de code nécessaire. Vous pouvez gagner du temps et éviter bien des tracas en utilisant le modèle approprié et en configurant le projet spécifiquement pour le développement de jeux. Nous vous guidons tout au long de l’installation et la configuration d’un projet de jeu simple. |
| [Définir l’infrastructure d’application UWP du jeu](tutorial--building-the-games-metro-style-app-framework.md) | La première partie du codage d’un jeu UWP avec DirectX consiste à créer l’infrastructure qui permet à l’objet jeu d’interagir avec Windows. Cela inclut des propriétés Windows Store telles que la gestion des événements de pause/reprise, la sélection de fenêtre et l’ancrage, ainsi que les événements, interactions et transitions pour l’interface utilisateur. Nous passons en revue la façon dont l’exemple de jeu est structuré et la façon dont il définit la machine à états principale pour l’interaction du joueur avec le système. |
| [Définir l’objet jeu principal](tutorial--defining-the-main-game-loop.md) | Examinons maintenant les détails de l’objet principal de l’exemple de jeu et la façon dont les règles qu’il implémente se traduisent en interactions avec le monde du jeu. |
| [Assembler l’infrastructure de rendu](tutorial--assembling-the-rendering-pipeline.md) | Il est maintenant temps d’examiner comment l’exemple de jeu utilise cette structure et ces états pour afficher ses graphiques. Dans cette rubrique, nous allons examiner comment implémenter une infrastructure de rendu, en partant de l’initialisation du périphérique graphique jusqu’à la présentation des objets graphiques à afficher. |
| [Ajouter une interface utilisateur](tutorial--adding-a-user-interface.md) | Vous venez de voir comment l’exemple de jeu implémente l’objet jeu principal ainsi que l’infrastructure de rendu de base. Examinons maintenant la façon dont l’exemple de jeu fournit des informations sur l’état du jeu au joueur. Ici, vous allez apprendre à ajouter de simples options de menu et composants d’affichage à tête haute sur la sortie du pipeline graphique 3D. |
| [Ajouter des contrôles](tutorial--adding-controls.md) | Examinons maintenant la façon dont l’exemple de jeu implémente des contrôles de déplacement/vue dans un jeu 3D et développe des contrôles tactiles, de souris et de manette de jeu de base. |
| [Ajouter du son](tutorial--adding-sound.md) | Au cours de cette étape, nous étudions comment l’exemple de jeu de tir crée un objet pour la lecture audio avec les API [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813). |
| [Développer l’exemple de jeu](tutorial-resources.md) | Félicitations ! À ce stade, vous maîtrisez les principaux composants d’un jeu 3D DirectX UWP de base. Vous pouvez configurer l’infrastructure d’un jeu, y compris le fournisseur de vues et le pipeline de rendu, et implémenter une boucle de jeu de base. Vous pouvez également créer une superposition de l’interface utilisateur de base, et incorporer des sons et des contrôles. Vous allez créer votre propre jeu, et voici donc quelques ressources pour approfondir vos connaissances en matière de développement de jeux DirectX. |
 

 

 




<!--HONumber=Mar16_HO1-->
