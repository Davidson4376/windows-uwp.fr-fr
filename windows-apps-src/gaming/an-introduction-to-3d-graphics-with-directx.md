---
title: Graphismes 3D de base pour jeux DirectX
description: Nous vous expliquons comment utiliser la programmation DirectX pour implémenter les concepts fondamentaux des graphismes 3D.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, jeux, directx, graphismes
ms.localizationpriority: medium
ms.openlocfilehash: 556c5c74e5c8284e56047b4b8a9b2c2bf9c0155c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369074"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Graphismes 3D de base pour jeux DirectX



Nous vous expliquons comment utiliser la programmation DirectX pour implémenter les concepts fondamentaux des graphismes 3D.

**Objectif :** Apprenez à programmer une application graphique 3D.

## <a name="prerequisites"></a>Prérequis


Nous partons du principe que vous êtes familiarisé avec C++. Vous avez également besoin d’une expérience de base dans les concepts de programmation graphique.

**Durée totale :** 30 minutes.

## <a name="where-to-go-from-here"></a>Où aller à partir d’ici


Ici, nous expliquent comment développer des graphiques 3D avec DirectX et C++\\Cx. Ce didacticiel en cinq parties présente l’API [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d), ainsi que les concepts et le code qui sont également employés dans de nombreux autres exemples DirectX. Ces parties s’appuient les unes sur les autres, de la configuration de DirectX pour votre application du Windows Store en C++ jusqu’à l’application de textures aux primitives et à l’ajout d’effets.

> **Remarque**  ce didacticiel utilise un système de coordonnées droitier des vecteurs de colonne. De nombreux exemples et applications DirectX utilisent un système de coordonnées pour gaucher avec des vecteurs lignes. Pour une solution de calcul graphique plus complète et qui prend en charge un système de coordonnées pour gaucher avec des vecteurs lignes, songez à utiliser [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal). Pour plus d’informations, voir [Utilisation de DirectXMath avec Direct3D](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration-d3dx).

 

Les opérations suivantes sont abordées :

-   Initialiser les interfaces [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) à l’aide de Windows Runtime
-   Appliquer d’opérations par vertex shader
-   Configurer la géométrie
-   Rastériser la scène (aplanissement de la scène 3D en projection 2D)
-   Éliminer les surfaces masquées

> **Remarque**  

 

Nous créons ensuite un appareil Direct3D, une chaîne d’échange et une vue de cible de rendu, puis présentons l’image rendue à l’écran.

[Démarrage rapide : configuration des ressources de DirectX et affichage d’une image](setting-up-directx-resources.md)

## <a name="related-topics"></a>Rubriques connexes


* [Direct3D 11 Graphics](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 




