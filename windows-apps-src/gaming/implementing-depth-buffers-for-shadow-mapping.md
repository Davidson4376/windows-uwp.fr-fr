---
author: mtoepke
title: "Procédure pas à pas: tampons de profondeur de volume d'ombres dans Direct3D 11"
description: Cette procédure pas à pas montre comment effectuer le rendu de volumes d’ombre («shadow volumes») avec des mappages de profondeur, en utilisant Direct3D 11 sur des appareils de tout niveau de fonctionnalité Direct3D.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, jeux, directx, volumes d’ombre, tampons de profondeur, directx11
ms.localizationpriority: medium
ms.openlocfilehash: 369fd133ffba2947b06a3fc9391979c17973ea52
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653698"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>Procédure pas à pas: Implémenter des volumes d’ombre à l’aide de tampons de profondeur dans Direct3D11



Cette procédure pas à pas indique comment effectuer le rendu de volumes ombrés avec des cartes de profondeur, en utilisant Direct3D11 sur des périphériques dotés de tous niveaux de fonctionnalités Direct3D.
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Article</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">Créer des ressources de périphérique pour un tampon de profondeur</a></p></td>
<td align="left"><p>Découvrez comment créer les ressources de périphérique Direct3D nécessaires pour prendre en charge le test de profondeur des volumes d’ombre.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">Générer le rendu du mappage d’ombre dans le tampon de profondeur</a></p></td>
<td align="left"><p>Générez le rendu du point de vue de la lumière pour créer un mappage de profondeur en deux dimensions qui représente le volume de l’ombre.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">Générer le rendu de la scène avec un test de profondeur</a></p></td>
<td align="left"><p>Créez un effet d’ombre en ajoutant un test de profondeur à votre nuanceur de vertex (ou géométrie) et votre nuanceur de pixels.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">Prendre en charge les mappages d’ombre sur une gamme de matériel</a></p></td>
<td align="left"><p>Générez le rendu des ombres en haute fidélité sur des appareils plus rapides et celui d’ombres plus rapides sur des appareils moins puissants.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Application du mappage d’ombres pour le portage Direct3D 9


Dans Windows 8, une nouvelle fonctionnalité de comparaison de la profondeur a été ajoutée aux niveaux de fonctionnalité 9\_1 et 9\_3. Vous pouvez désormais transférer du code de rendu contenant des volumes d’ombre sur DirectX 11. Le convertisseur Direct3D 11 peut être utilisé sur les appareils fonctionnant avec des niveaux de fonctionnalité 9 inférieurs. Cette procédure pas à pas montre comment une application ou un jeu Direct3D 11 peut implémenter des volumes d’ombres classiques avec le test de profondeur. Le code couvre le processus suivant :

1.  Création de ressources de périphérique Direct3D pour le mappage d’ombres.
2.  Ajout d’une passe de rendu pour créer le mappage de profondeur.
3.  Ajout du test de profondeur à la passe de rendu principale.
4.  Implémentation du code de nuanceur requis.
5.  Options d’accélération du rendu sur le matériel avec un niveau de fonctionnalité inférieur.

Au terme de cette procédure pas à pas, vous serez capable d’implémenter un volume d’ombre de base dans Direct3D 11, totalement compatible avec les niveaux de fonctionnalité 9\_1 et supérieurs.

## <a name="prerequisites"></a>Prérequis


Vous devez [préparer votre environnement au développement de jeux de plateforme Windows universelle (UWP) DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Vous n’avez pas encore besoin de modèle, mais vous devez disposer de Microsoft Visual Studio 2015 pour générer l’exemple de code de cette procédure pas à pas.

## <a name="related-topics"></a>Rubriques connexes


**Direct3D**

* [Écriture de nuanceurs HLSL dans Direct3D9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [Créer un projet DirectX11 pour UWP](user-interface.md)

**Articles techniques sur le mappage d’ombres**

* [Techniques courantes pour améliorer les mappages de profondeur d’ombre](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [Mappages d’ombres en cascade (CSM)](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 




