---
title: "Procédure pas à pas : tampons de profondeur de volume d'ombres dans Direct3D 11"
description: Cette procédure pas à pas montre comment effectuer le rendu de volumes d’ombre (« shadow volumes ») avec des mappages de profondeur, en utilisant Direct3D 11 sur des appareils de tout niveau de fonctionnalité Direct3D.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, directx, volumes d’ombre, tampons de profondeur, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 2feecb3080efefb2f9625fd8b66c5b722ad02a45
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622274"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>Démonstration : Implémenter des volumes de clichés instantanés à l’aide des tampons de profondeur dans Direct3D 11



Cette procédure pas à pas montre comment effectuer le rendu de volumes d’ombre (« shadow volumes ») avec des mappages de profondeur, en utilisant Direct3D 11 sur des appareils de tout niveau de fonctionnalité Direct3D.
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">Créer des ressources de périphérique de mémoire tampon de profondeur</a></p></td>
<td align="left"><p>Découvrez comment créer les ressources de périphérique Direct3D nécessaires pour prendre en charge le test de profondeur des volumes d’ombre.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">Restituer la carte de clichés instantanés dans la mémoire tampon de profondeur</a></p></td>
<td align="left"><p>Générez le rendu du point de vue de la lumière pour créer un mappage de profondeur en deux dimensions qui représente le volume de l’ombre.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">Afficher la scène avec des tests en profondeur</a></p></td>
<td align="left"><p>Créez un effet d’ombre en ajoutant un test de profondeur à votre nuanceur de vertex (ou géométrie) et votre nuanceur de pixels.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">Prise en charge des cartes de clichés instantanés sur une gamme de matériel</a></p></td>
<td align="left"><p>Générez le rendu des ombres en haute fidélité sur des appareils plus rapides et celui d’ombres plus rapides sur des appareils moins puissants.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Application du mappage d’ombres pour le portage Direct3D 9


Windows 8 adde la fonctionnalité de comparaison profondeur d pour le niveau de fonctionnalité 9\_1 et 9\_3. Vous pouvez désormais transférer du code de rendu contenant des volumes d’ombre sur DirectX 11. Le convertisseur Direct3D 11 peut être utilisé sur les appareils fonctionnant avec des niveaux de fonctionnalité 9 inférieurs. Cette procédure pas à pas montre comment une application ou un jeu Direct3D 11 peut implémenter des volumes d’ombres classiques avec le test de profondeur. Le code couvre le processus suivant :

1.  Création de ressources de périphérique Direct3D pour le mappage d’ombres.
2.  Ajout d’une passe de rendu pour créer le mappage de profondeur.
3.  Ajout du test de profondeur à la passe de rendu principale.
4.  Implémentation du code de nuanceur requis.
5.  Options d’accélération du rendu sur le matériel avec un niveau de fonctionnalité inférieur.

À la fin de cette procédure pas à pas, vous devez être familiarisé avec l’implémentation d’une technique de volume de clichés instantanés compatible base dans Direct3D 11, qui est compatible avec le niveau de fonctionnalité 9\_1 et versions ultérieures.

## <a name="prerequisites"></a>Conditions préalables


Vous devez [préparer votre environnement au développement de jeux de plateforme Windows universelle (UWP) DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Vous n’avez pas encore un modèle, mais vous aurez besoin de Microsoft Visual Studio 2015 pour générer l’exemple de code pour cette procédure pas à pas.

## <a name="related-topics"></a>Rubriques connexes


**Direct3D**

* [Écriture de nuanceurs HLSL dans Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [Créer un nouveau projet de DirectX 11 pour UWP](user-interface.md)

**Articles techniques de mappage de clichés instantanés**

* [Techniques courantes pour améliorer les cartes de profondeur de clichés instantanés](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [Mappages de clichés instantanés en cascade](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 




