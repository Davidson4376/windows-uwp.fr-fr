---
author: mtoepke
title: "Procédure pas à pas &#58; implémenter des volumes d’ombre à l’aide de tampons de profondeur dans Direct3D 11"
description: "Cette procédure pas à pas montre comment effectuer le rendu de volumes d’ombre (« shadow volumes ») avec des mappages de profondeur, en utilisant Direct3D 11 sur des appareils de tout niveau de fonctionnalité Direct3D."
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 4bb1f5c71d83b2dc3271b5b2ceaa9a1156b00004

---

# Procédure pas à pas &#58; implémenter des volumes d’ombre à l’aide de tampons de profondeur dans Direct3D 11


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cette procédure pas à pas montre comment effectuer le rendu de volumes d’ombre (« shadow volumes ») avec des mappages de profondeur, en utilisant Direct3D 11 sur des appareils de tout niveau de fonctionnalité Direct3D.
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
<td align="left"><p>[Créer des ressources de périphérique pour un tampon de profondeur](create-depth-buffer-resource--view--and-sampler-state.md)</p></td>
<td align="left"><p>Découvrez comment créer les ressources de périphérique Direct3D nécessaires pour prendre en charge le test de profondeur des volumes d’ombre.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Générer le rendu du mappage d’ombre dans le tampon de profondeur](render-the-shadow-map-to-the-depth-buffer.md)</p></td>
<td align="left"><p>Générez le rendu du point de vue de la lumière pour créer un mappage de profondeur en deux dimensions qui représente le volume de l’ombre.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Générer le rendu de la scène avec un test de profondeur](render-the-scene-with-depth-testing.md)</p></td>
<td align="left"><p>Créez un effet d’ombre en ajoutant un test de profondeur à votre nuanceur de vertex (ou géométrie) et votre nuanceur de pixels.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Prendre en charge les mappages d’ombre sur une gamme de matériel](target-a-range-of-hardware.md)</p></td>
<td align="left"><p>Générez le rendu des ombres en haute fidélité sur des appareils plus rapides et celui d’ombres plus rapides sur des appareils moins puissants.</p></td>
</tr>
</tbody>
</table>

 

## Application du mappage d’ombres pour le portage Direct3D 9


Dans Windows 8, une nouvelle fonctionnalité de comparaison de la profondeur a été ajoutée aux niveaux de fonctionnalité 9\_1 et 9\_3. Vous pouvez désormais transférer du code de rendu contenant des volumes d’ombre sur DirectX 11. Le convertisseur Direct3D 11 peut être utilisé sur les appareils fonctionnant avec des niveaux de fonctionnalité 9 inférieurs. Cette procédure pas à pas montre comment une application ou un jeu Direct3D 11 peut implémenter des volumes d’ombres classiques avec le test de profondeur. Le code couvre le processus suivant :

1.  Création de ressources de périphérique Direct3D pour le mappage d’ombres.
2.  Ajout d’une passe de rendu pour créer le mappage de profondeur.
3.  Ajout du test de profondeur à la passe de rendu principale.
4.  Implémentation du code de nuanceur requis.
5.  Options d’accélération du rendu sur le matériel avec un niveau de fonctionnalité inférieur.

Au terme de cette procédure pas à pas, vous serez capable d’implémenter un volume d’ombre de base dans Direct3D 11, totalement compatible avec les niveaux de fonctionnalité 9\_1 et supérieurs.

## Prérequis


Vous devez [préparer votre environnement au développement de jeux de plateforme Windows universelle (UWP) DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Vous n’avez pas encore besoin de modèle, mais vous devez disposer de Microsoft Visual Studio 2015 pour générer l’exemple de code de cette procédure pas à pas.

## Rubriques connexes


**Direct3D**

* [Écriture de nuanceurs HLSL dans Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [Créer un projet DirectX 11 pour UWP](user-interface.md)

**Articles techniques sur le mappage d’ombres**

* [Techniques courantes pour améliorer les mappages de profondeur d’ombre](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [Mappages d’ombres en cascade (CSM)](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 







<!--HONumber=Jun16_HO4-->


