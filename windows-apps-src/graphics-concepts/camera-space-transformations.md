---
title: Transformations de l’espace de caméra
description: Les vertex dans l’espace de caméra sont calculés en transformant les vertex de l’objet avec la matrice globale.
ms.assetid: 86EDEB95-8348-4FAA-897F-25251B32B076
keywords:
- Transformations de l’espace de caméra
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9de6759fb15aef4b32a5e9022a27cab09af300f8
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6980732"
---
# <a name="camera-space-transformations"></a>Transformations de l’espace de caméra


Les vertex dans l’espace de caméra sont calculés en transformant les vertex de l’objet avec la matrice globale.

V = V \* wvMatrix

Les normales du vertex, dans l’espace de caméra, sont calculées en transformant les normales d’objet avec la transposition inverse de la matrice globale. La matrice globale peut ou non être orthogonale.

N = N \* (wvMatrix⁻¹)<sup>T</sup>

L’inversion matricielle et la transposition matricielle fonctionnent sur une matrice 4x4. La multiplication associe la normale à la partie 3x3 de la matrice 4x4 qui en résulte.

Si l’état de rendu est défini de manière à normaliser les normales, les vecteurs normaux du vertex sont normalisés après la transformation dans l’espace de caméra, comme indiqué ci-après:

N = norm(N)

La position de la lumière dans l’espace de caméra est calculée en transformant la position de la source de lumière avec la matrice globale.

Lₚ = Lₚ \* vMatrix

La direction de la lumière dans l'espace de caméra, dans le cas d'une lumière directionnelle, est calculée en multipliant la direction de la source de lumière par la matrice globale, puis en normalisant et en inversant le résultat.

L<sub>dir</sub> = -norm(L<sub>dir</sub> \* wvMatrix)

Pour une lumière ponctuelle et un projecteur, la direction par rapport à la lumière est calculée comme suit:

L<sub>dir</sub> = norm(V \* Lₚ), où les paramètres sont définis dans le tableau suivant.

| Paramètre       | Valeur par défaut | Type                                          | Description                                               |
|-----------------|---------------|-----------------------------------------------|-----------------------------------------------------------|
| L<sub>dir</sub> | Non applicable           | Vecteur 3D (valeurs à virgule flottante x, y et z) | Vecteur de direction entre le vertex de l’objet et la lumière          |
| V               | Non applicable           | Vecteur 3D (valeurs à virgule flottante x, y et z) | Position du vertex dans l'espace de caméra                           |
| wvMatrix        | Identité      | Matrice 4x4 de valeurs à virgule flottante           | Matrice composite contenant les transformations globales et d’affichage |
| N               | Non applicable           | Vecteur 3D (valeurs à virgule flottante x, y et z) | Normale du vertex                                             |
| Lₚ              | Non applicable           | Vecteur 3D (valeurs à virgule flottante x, y et z) | Position de la lumière dans l’espace de caméra                            |
| vMatrix         | Identité      | Matrice 4x4 de valeurs à virgule flottante           | Matrice contenant la transformation d’affichage                      |

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Formules mathématiques d’éclairage](mathematics-of-lighting.md)

 

 




