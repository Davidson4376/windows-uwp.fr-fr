---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: Icônes
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 61157ad23eb55447137531922ea23fa0120e2b98
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653918"
---
# <a name="icons-for-uwp-apps"></a>Icônes pour les applications UWP



Des icônes efficaces s’harmonisent avec la typographie et avec le reste du langage de conception. Elles ne mélangent pas les métaphores et communiquent uniquement les informations nécessaires, le plus rapidement et simplement possible. 

## <a name="linear-scaling-size-ramps"></a>Gammes de tailles de mise à l’échelle linéaire 

<table>
    <tr> 
        <td>16pxx16px</td>
        <td>24pxx24px</td>
        <td>32pxx32px</td>
        <td>48pxx48px</td>
    </tr>
    <tr> 
        <td><img src="images/icons-16x16.png" alt="Icons at 16x16 effective pixels" /></td>
        <td><img src="images/icons-24x24.png" alt="Icons at 24x24 effective pixels" /></td>
        <td><img src="images/icons-32x32.png" alt="Icons at 32x32 effective pixels" /></td>
        <td><img src="images/icons-48x48.png" alt="Icons at 48x48 effective pixels" /></td>
    </tr>
</table>

## <a name="common-shapes"></a>Formes courantes

Les icônes doivent généralement optimiser l’espace dont elles disposent et n’inclure qu’un remplissage minime. Ces formes fournissent des points de départ pour le dimensionnement des formes de base. 

![Grille de 32px par 32px](images/icons-common-shapes.png)

Utilisez la forme qui correspond à l’orientation de l’icône et créez votre composition autour de ces paramètres de base. Les icônes ne doivent pas nécessairement remplir la forme ou tenir entièrement à l’intérieur de cette dernière, et peuvent être ajustées selon les besoins pour garantir un équilibre optimal. 

<table class="uwpd-noborder">
    <tr>
        <td>Cercle<td>
        <td>Carré</td>
        <td>Triangle</td>
    </tr>
    <tr>
        <td><img src="images/icons-common-shapes-examples-1.png" alt="A circle" /><td>
        <td><img src="images/icons-common-shapes-examples-2.png" alt="A square" /></td>
        <td><img src="images/icons-common-shapes-examples-3.png" alt="A triangle " /></td>
    </tr>
        <tr>
        <td>Rectangle horizontal<td>
        <td colspan="2">Rectangle vertical</td>        
        </tr>
    <tr>
        <td><img src="images/icons-common-shapes-examples-4.png" alt="A horizontal rectangle" /><td>
        <td colspan="2"><img src="images/icons-common-shapes-examples-5.png" alt="A vertical rectangle" /></td>
         
    </tr>

</table>

## <a name="angles"></a>Angles

Outre l’utilisation d’une grille et d’une épaisseur de trait identiques, les icônes sont construites avec des éléments communs. 

L’utilisation de ces seuls angles lors de la création de formes garantit la cohérence et le rendu adéquat de l’ensemble de nos icônes. 

Vous pouvez combiner, joindre, faire pivoter et refléter ces lignes lorsque vous créez des icônes. 

<table>
    <tr>
        <td><b>1:1</b><br/>45°</td>
        <td><b>1:2</b><br />26,57° (vertical)<br/>63,43° (horizontal)</td>
        <td><b>1:3</b><br/>18,43° (vertical)<br/>71,57° (horizontal)</td>
        <td><b>1:4</b><br/>14,04° (vertical)<br/>75,96° (horizontal)</td>
    </tr>
    <tr>
        
        <td><img src="images/icons-grid-1-1.png" alt="1:1" /></td>
        <td><img src="images/icons-grid-1-2.png" alt="1:2" /></td>
        <td><img src="images/icons-grid-1-3.png" alt="1:3" /></td>
        <td><img src="images/icons-grid-1-4.png" alt="1:4" /></td>
    </tr>  
</table>

<p>En voici quelques exemples:</p>

<table>
    <tr>
        <td><img src="images/icons-angles-examples-1.png" alt="A 1:1 angle example" /></td>
        <td><img src="images/icons-angles-examples-2.png" alt="A 1:2 angle example" /></td>
        <td><img src="images/icons-angles-examples-3.png" alt="A 1:3 angle example" /></td>
        <td><img src="images/icons-angles-examples-4.png" alt="A 1:4 angle example" /></td>
    </tr>
</table>

## <a name="curves"></a>Courbes

Les traits courbes sont créés à partir de sections d’un cercle complet et ne doivent pas être inclinés, sauf s’ils ont besoin d’être alignés sur la grille de pixels. 

<table>
    <tr>
        <td>1/4 de cercle</td>
        <td>1/8 de cercle</td>
    </tr>
    <tr>
        <td><img src="images/icons-curves-14circle.png" alt="1/4 circle" /></td>
        <td><img src="images/icons-curves-18circle.png" alt="1/8 circle" /></td>
    </tr>
    <tr>
        <td><img src="images/icons-curves-examples-1.png" alt="1/4 cirlce example" /></td>
        <td><img src="images/icons-curves-examples-2.png" alt="1/8 circle example" /></td>
    </tr>    
</table>

## <a name="geometric-construction"></a>Construction géométrique

Nous vous recommandons de n’utiliser que des formes géométriques pures lorsque vous construisez des icônes.

![Icône en forme de guitare avec superposition géométrique ](images/icons-geometric-construction.png)

## <a name="filled-shapes"></a>Formes remplies 

Les icônes peuvent contenir des formes remplies si nécessaire, mais elles ne doivent pas dépasser 4px pour une grille de 32px×32px. La taille des cercles remplis ne doit pas être supérieure à 6pxx6px. 

![Remplissage 5px par 8px ](images/icons-filled-shapes.png)

## <a name="badges"></a>Badges

Un «badge» est un terme générique désignant un élément ajouté à une icône non destiné à être intégré à l’élément d’icône de base. Ces éléments véhiculent généralement d’autres informations concernant l’icône, comme un état ou une action. Les autres termes couramment employés pour désigner ce type d’élément sont: superposition, annotation ou modificateur. 

![Badge d’état ](images/icons-badge-status.png)

![Badge d’action ](images/icons-badge-action.png)

Les badges d’état utilisent un objet coloré rempli surmontant l’icône, tandis que les badges d’action sont intégrés à l’icône avec le même style monochrome et la même épaisseur de trait.

<table>
<tr>
    <td>Badges d’état courants</td>
    <td>Badges d’action courants</td>
</tr>
<tr>
    <td><img src="images/icons-badge-common-states-1.png" alt="Status badge " /></td>
    <td><img src="images/icons-badge-common-states-2.png" alt="Action badge " /></td>
</tr>
</table>
<p></p>

### <a name="badge-color"></a>Couleur de badge 

L’utilisation d’une couleur de badge ne doit être utilisée que pour communiquer l’état d’une icône. Les couleurs utilisées dans le badge d’état transmettent des messages émotionnels spécifiques à l’utilisateur. 

<table>
<tr><td>Vert: #128B44</td><td>Bleu: #2C71B9</td><td>Jaune: #FDC214</td></tr>
<tr><td>Positif: effectué, terminé </td><td>Neutre: aide, notification </td><td>Mise en garde: alerte, avertissement </td></tr>
<tr><td><img src="images/icons-color-inbadging-1.png" alt="Green status" /></td><td><img src="images/icons-color-inbadging-2.png" alt="Blue status" /></td>
<td><img src="images/icons-color-inbadging-3.png" alt="Yellow status" /></td></tr>
</table>
<p></p>

### <a name="badge-position"></a>Position de badge

Par défaut, un badge d’état ou d’action est positionné dans le coin inférieur droit. N’utilisez d’autres positions que si la conception n’autorise pas ce positionnement par défaut. 

### <a name="badge-sizing"></a>Dimensionnement de badge

Les badges doivent présenter une taille de 10 à 18px sur une grille de 32px × 32px. 

## <a name="related-articles"></a>Articles connexes

* [Recommandations en matière de ressources de vignette et d’icône](../shell/tiles-and-notifications/app-assets.md)
