---
title: Développement de MarbleMaze, jeu pour UW en C++ et DirectX
description: Cette section de la documentation décrit comment utiliser DirectX et Visual C++ afin de créer un jeu pour la plateforme Windows universelle (UWP) en 3D.
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: windows10, uwp, jeux, exemple, directx, 3d
ms.localizationpriority: medium
ms.openlocfilehash: e61c96a1b4deb7dd1beb0233814f86ce1b5fb42c
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8479200"
---
# <a name="developing-marble-maze-a-uwp-game-in-c-and-directx"></a>Développement de MarbleMaze, jeu pour UWP en C++ et DirectX




Cette rubrique décrit comment utiliser DirectX et Visual C++ afin de créer un jeu pour la plateforme Windows universelle (UWP) en 3D. Le jeu, appelé Marble Maze, s’appuie sur plusieurs facteurs de forme tels que les tablettes, ainsi que les PC de bureau et les portables traditionnels.

> [!NOTE]
> Pour télécharger le code source de Marble Maze, voir l’[exemple sur GitHub](http://go.microsoft.com/fwlink/?LinkId=624011).

> [!IMPORTANT]
> MarbleMaze illustre des modèles de conception que nous considérons comme exemplaires pour la création de jeux UWP. Vous pouvez personnaliser de nombreux détails d’implémentation pour les adapter à vos propres méthodes et aux critères spécifiques du jeu que vous développez. Vous êtes libre d’utiliser des techniques ou des bibliothèques différentes si elles sont mieux adaptées à vos besoins. (Toutefois, vérifiez toujours que votre code passe le [Kit de certification des applications Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).) Si nous considérons que l’implémentation de Marble Maze est essentielle pour la réussite du développement du jeu, nous le soulignons dans cette documentation.

 

## <a name="introducing-marble-maze"></a>Présentation de Marble Maze


Nous avons sélectionné Marble Maze car il est relativement simple, mais permet malgré tout de découvrir l’étendue des fonctionnalités présentes dans la plupart des jeux. Il indique comment utiliser les graphiques et gérer les données d’entrée et audio. Il montre également la mécanique du jeu, par exemple, les règles et les objectifs.

Marble Maze s’inspire du jeu de labyrinthe de table qui se compose généralement d’une boîte à trous et d’une bille en acier ou en verre. L’objectif de Marble Maze est le même que celui de la version de table : incliner le labyrinthe pour guider la bille de l’entrée vers la sortie du labyrinthe le plus rapidement possible, sans faire tomber la bille dans les trous. Marble Maze ajoute le concept de points de contrôle. Si la bille tombe dans un trou, le jeu est redémarré au dernier emplacement du point de contrôle sur lequel la bille est passée.

Marble Maze offre à l’utilisateur plusieurs moyens d’interagir avec le tableau de jeu. Si vous avez un appareil tactile ou activé par accéléromètre, vous pouvez les utiliser pour bouger le tableau de jeu. Vous pouvez également utiliser une manette Xbox One ou une souris pour contrôler l’action de jeu.

![capture d’écran du jeu Marble Maze.](images/marblemaze-2.png)

## <a name="prerequisites"></a>Prérequis


-   Windows 10 Creators Update
-   [Microsoft Visual Studio2017](https://www.visualstudio.com/downloads/)
-   Connaissance de la programmation en C++
-   Connaissance de la terminologie DirectX et DirectX
-   Connaissances de base de COM

## <a name="who-should-read-this"></a>Public concerné par cette documentation


Si vous êtes intéressé de la création de jeux en 3D ou autres applications gourmandes en graphiques pour Windows 10, c’est pour vous. Nous espérons que vous utiliserez les principes et les pratiques décrits dans cette documentation pour créer votre propre jeu pour UWP. Une expérience ou un intérêt prononcé pour la programmation en C++ et DirectX vous aidera à exploiter au mieux cette documentation. Si DirectX ne vous est pas familier, vous pouvez quand même en tirer parti si vous connaissez les environnements de programmation graphique 3D de même type.

Le document [Procédure pas à pas: créer un jeu UWP simple avec DirectX](tutorial--create-your-first-uwp-directx-game.md) décrit un autre exemple qui implémente un jeu de tir 3D de base en DirectX et C++.

## <a name="what-this-documentation-covers"></a>Points traités dans cette documentation


Cette documentation explique comment :

-   utiliser l’API Windows Runtime et DirectX pour créer un jeu pour UWP ;
-   utiliser [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476080) et [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) pour exploiter du contenu visuel tel que des modèles, des textures, des nuanceurs de vertex et de pixels, ainsi que des superpositions 2D;
-   intégrer des mécanismes d’entrée tels que les fonctions tactiles, l’accéléromètre et la manette Xbox One;
-   utiliser [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) pour incorporer de la musique et des effets sonores.

## <a name="what-this-documentation-does-not-cover"></a>Points non traités dans cette documentation


Cette documentation ne traite pas les aspects suivants du développement de jeux. Ils sont suivis par d’autres ressources qui les traitent.

-   Principes de conception des jeux 3D.
-   Notions de base de la programmation en C++ ou DirectX.
-   Procédure de conception des ressources telles que les textures, les modèles ou les données audio.
-   Procédure de dépannage des problèmes liés au comportement ou aux performances de votre jeu.
-   Procédure de préparation de votre jeu pour une utilisation dans d’autres parties du monde.
-   Procédure de certification et de publication de votre jeu dans le Microsoft Store.

Marble Maze utilise également la bibliothèque [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) pour exploiter la géométrie 3D et exécuter des calculs de physique, tels que les collisions. DirectXMath n’est pas abordé en profondeur dans cette section. Pour en savoir plus sur l’usage que fait Marble Maze de DirectXMath, voir le code source.

Bien que Marble Maze fournisse de nombreux composants réutilisables, il ne s’agit pas d’une infrastructure de développement de jeu complète. Si nous estimons qu’un composant de Marble Maze est réutilisable dans votre jeu, nous le précisons dans la documentation.

## <a name="next-steps"></a>Étapes suivantes


Nous vous recommandons de commencer par les [Notions de base de l’exemple Marble Maze](marble-maze-sample-fundamentals.md) pour en savoir plus sur la structure de Marble Maze et obtenir des instructions de codage et de style que le code source de Marble Maze applique. Le tableau suivant récapitule les documents de cette section pour vous permettre de vous y référer plus facilement.

## <a name="in-this-section"></a>Dans cette section


| Titre                                                                                                                    | Description                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Notions de base de l’exemple Marble Maze](marble-maze-sample-fundamentals.md)                                                   | Présente la structure de jeu et les instructions de codage et de style que le code source applique.                                                                                                                                 |
| [Structure de l’application Marble Maze](marble-maze-application-structure.md)                                               | Décrit la structure du code de l’application Marble Maze, ainsi que les différences entre la structure d’une application pour UWP en DirectX et celle d’une application de bureau classique.                                                                                    |
| [Ajout de contenu visuel à l’exemple Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)                   | Décrit certaines pratiques importantes à connaître quand vous utilisez Direct3D et Direct2D. Décrit également comment Marble Maze applique ces pratiques dans le cas du contenu visuel.                                                                           |
| [Ajout d’entrées et d’interactivité à l’exemple Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md) | Décrit comment Marble Maze fonctionne avec l’accéléromètre, les fonctions tactiles et la manette Xbox One pour permettre aux utilisateurs de naviguer dans les menus et d’interagir avec le tableau de jeu. Décrit également quelques-unes des meilleures pratiques à connaître quand vous utilisez une entrée. |
| [Ajout d’audio à l’exemple Marble Maze](adding-audio-to-the-marble-maze-sample.md)                                     | Décrit comment fonctionne Marble Maze avec les données audio pour ajouter de la musique et des effets sonores à l’expérience de jeu.                                                                                                                                                  |

 

 

 




