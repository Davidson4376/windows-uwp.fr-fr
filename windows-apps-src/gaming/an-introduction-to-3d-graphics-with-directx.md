---
author: mtoepke
title: Graphismes 3D de base pour jeux DirectX
description: Nous vous expliquons comment utiliser la programmation DirectX pour implémenter les concepts fondamentaux des graphismes 3D.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
---

# Graphismes 3D de base pour jeux DirectX


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Nous vous expliquons comment utiliser la programmation DirectX pour implémenter les concepts fondamentaux des graphismes 3D.

**Objectif :** découvrir comment programmer une application de graphisme 3D.

## Prérequis


Nous partons du principe que vous êtes familiarisé avec C++. Vous avez également besoin d’une expérience de base dans les concepts de programmation graphique.

**Durée de réalisation totale :** 30 minutes.

## Où aller à partir d’ici


Nous expliquons ici comment développer des graphismes 3D avec DirectX et C++\\Cx. Ce didacticiel en cinq parties présente l’API [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466), ainsi que les concepts et le code qui sont également employés dans de nombreux autres exemples DirectX. Ces parties s’appuient les unes sur les autres, de la configuration de DirectX pour votre application du Windows Store en C++ jusqu’à l’application de textures aux primitives et à l’ajout d’effets.

> **Remarque** Ce didacticiel utilise un système de coordonnées pour droitier avec des vecteurs colonnes. De nombreux exemples et applications DirectX utilisent un système de coordonnées pour gaucher avec des vecteurs lignes. Pour une solution de calcul graphique plus complète et qui prend en charge un système de coordonnées pour gaucher avec des vecteurs lignes, songez à utiliser [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833). Pour plus d’informations, voir [Utilisation de DirectXMath avec Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff729728#Use_DXMath_with_D3D).

 

Les opérations suivantes sont abordées :

-   Initialiser les interfaces [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) à l’aide de Windows Runtime
-   Appliquer d’opérations par vertex shader
-   Configurer la géométrie
-   Rastériser la scène (aplanissement de la scène 3D en projection 2D)
-   Éliminer les surfaces masquées

> **Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui développent des applications de la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

Nous créons ensuite un appareil Direct3D, une chaîne d’échange et une vue de cible de rendu, puis présentons l’image rendue à l’écran.

[Démarrage rapide : configuration de ressources DirectX et affichage d’une image](setting-up-directx-resources.md)

## Rubriques connexes


* [Graphismes Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080)
* [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)
* [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)

 

 






<!--HONumber=May16_HO2-->


