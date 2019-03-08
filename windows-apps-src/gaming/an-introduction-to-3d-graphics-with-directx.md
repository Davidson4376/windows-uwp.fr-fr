---
title: Graphismes 3D de base pour jeux DirectX
description: Nous vous expliquons comment utiliser la programmation DirectX pour implémenter les concepts fondamentaux des graphismes 3D.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, jeux, directx, graphismes
ms.localizationpriority: medium
ms.openlocfilehash: 5dbdf6072f57d12d424f0787cfa2e8993a1624af
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621794"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Graphismes 3D de base pour jeux DirectX



Nous vous expliquons comment utiliser la programmation DirectX pour implémenter les concepts fondamentaux des graphismes 3D.

**Objectif :** Apprenez à programmer une application graphique 3D.

## <a name="prerequisites"></a>Conditions préalables


Nous partons du principe que vous êtes familiarisé avec C++. Vous avez également besoin d’une expérience de base dans les concepts de programmation graphique.

**Durée totale :** 30 minutes.

## <a name="where-to-go-from-here"></a>Où aller à partir d’ici


Ici, nous expliquent comment développer des graphiques 3D avec DirectX et C++\\Cx. Ce didacticiel en cinq parties présente l’API [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466), ainsi que les concepts et le code qui sont également employés dans de nombreux autres exemples DirectX. Ces parties s’appuient les unes sur les autres, de la configuration de DirectX pour votre application du Windows Store en C++ jusqu’à l’application de textures aux primitives et à l’ajout d’effets.

> **Remarque**  ce didacticiel utilise un système de coordonnées droitier des vecteurs de colonne. De nombreux exemples et applications DirectX utilisent un système de coordonnées pour gaucher avec des vecteurs lignes. Pour une solution de calcul graphique plus complète et qui prend en charge un système de coordonnées pour gaucher avec des vecteurs lignes, songez à utiliser [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833). Pour plus d’informations, voir [Utilisation de DirectXMath avec Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff729728#Use_DXMath_with_D3D).

 

Les opérations suivantes sont abordées :

-   Initialiser les interfaces [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) à l’aide de Windows Runtime
-   Appliquer d’opérations par vertex shader
-   Configurer la géométrie
-   Rastériser la scène (aplanissement de la scène 3D en projection 2D)
-   Éliminer les surfaces masquées

> **Remarque**  

 

Nous créons ensuite un appareil Direct3D, une chaîne d’échange et une vue de cible de rendu, puis présentons l’image rendue à l’écran.

[Démarrage rapide : configuration des ressources de DirectX et affichage d’une image](setting-up-directx-resources.md)

## <a name="related-topics"></a>Rubriques connexes


* [Direct3D 11 Graphics](https://msdn.microsoft.com/library/windows/desktop/ff476080)
* [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)
* [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)

 

 




