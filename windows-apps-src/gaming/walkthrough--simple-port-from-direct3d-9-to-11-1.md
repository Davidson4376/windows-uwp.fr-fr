---
author: mtoepke
title: "Procédure pas à pas: Porter une application Direct3D9 simple vers DirectX11 et la plateforme Windows universelle (UWP)"
description: "Cet exercice de portage indique comment faire passer une infrastructure de rendu simple de Direct3D9 à Direct3D11 et à la plateforme Windows universelle (UWP)."
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 53e0b3f5a69f133e74430b1a2e32a13180569f06

---

# Procédure pas à pas: Porter une application Direct3D9 simple vers DirectX11 et la plateforme Windows universelle (UWP)


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cet exercice de portage indique comment faire passer une infrastructure de rendu simple de Direct3D9 à Direct3D11 et à la plateforme Windows universelle (UWP).
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
<td align="left"><p>[Initialiser Direct3D11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)</p></td>
<td align="left"><p>Montre comment convertir du code d’initialisation Direct3D9 en Direct3D11, notamment comment obtenir des handles vers le périphérique Direct3D et le contexte de périphérique, et comment utiliser DXGI pour configurer une chaîne d’échange.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Convertir l’infrastructure de rendu](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)</p></td>
<td align="left"><p>Montre comment convertir une infrastructure de rendu simple de Direct3D9 à Direct3D11, notamment comment porter des tampons de géométrie, comment compiler et charger des programmes de nuanceurs HLSL et comment implémenter la chaîne de rendu dans Direct3D11.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Porter la boucle de jeu](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)</p></td>
<td align="left"><p>Indique comment implémenter une fenêtre pour un jeu UWP et comment récupérer la boucle de jeu, notamment comment générer un élément [<strong>IFrameworkView</strong>](https://msdn.microsoft.com/library/windows/apps/hh700478) pour contrôler un élément [<strong>CoreWindow</strong>](https://msdn.microsoft.com/library/windows/apps/br208225) en plein écran.</p></td>
</tr>
</tbody>
</table>

 

Cette rubrique examine progressivement deux chemins de code qui effectuent la même tâche graphique de base: afficher un cube en forme de vertex qui tourne. Dans les deux cas, le code couvre le processus suivant :

1.  Création d’un périphérique Direct3D et d’une chaîne d’échange.
2.  Création d’une mémoire tampon de vertex et d’un tampon d’index pour représenter un maillage de cube en couleur.
3.  Création d’un nuanceur de vertex qui transforme les vertex en espace d’écran, d’un nuanceur de pixels qui fusionne les valeurs de couleurs, compilation des nuanceurs et chargement des nuanceurs en tant que ressources Direct3D.
4.  Implémentation de la chaîne de rendu et présentation du cube dessiné à l’écran.
5.  Création d’une fenêtre, démarrage d’une boucle principale et traitement des messages de fenêtre.

À l’issue de cette procédure pas à pas, vous devrez connaître les différences élémentaires suivantes entre Direct3D 9 et Direct3D 11 :

-   Séparation du périphérique, du contexte de périphérique et de l’infrastructure graphique.
-   Processus de compilation des nuanceurs et chargement du bytecode de nuanceur au moment de l’exécution.
-   Manière de configurer des données par vertex pour le stade d’assembleur d’entrée (stade IA).
-   Manière d’utiliser un [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) pour créer un affichage CoreWindow.

Notez que cette procédure pas à pas utilise [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) pour des raisons de simplicité, et ne couvre pas l’interopérabilité XAML.

## Conditions préalables


Vous devez [Préparer votre environnement pour le développement de jeux UWP DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Vous n’avez pas encore besoin de modèle, mais Microsoft Visual Studio 2015 est nécessaire pour charger les exemples de code de cette procédure pas à pas.

Pour vous familiariser avec les concepts de programmation pour DirectX11 et UWP présentés dans cette procédure pas à pas, voir les [concepts et considérations en matière de portage](porting-considerations.md).

## Rubriques connexes


**Direct3D** 
 [Écriture de nuanceurs HLSL dans Direct3D9](https://msdn.microsoft.com/library/windows/desktop/bb944006)

[Créer un projet DirectX11 pour UWP](user-interface.md)

**Windows Store**
[**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)

[**Handle sur l’opérateur Object (^)**]https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx

 

 







<!--HONumber=Aug16_HO3-->


