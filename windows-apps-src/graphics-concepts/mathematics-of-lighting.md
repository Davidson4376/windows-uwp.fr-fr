---
title: Formules mathématiques d’éclairage
description: Le modèle d’éclairage Direct3D prend en charge les éclairages ambiant, diffus, spéculaire et émissif. La souplesse de la configuration permet de résoudre de nombreuses situations d’éclairage. Dans une scène, la quantité globale d’éclairage s’appelle l’illumination globale.
ms.assetid: D0521F56-050D-4EDF-9BD1-34748E94B873
keywords:
- Formules mathématiques d’éclairage
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 19734964c9b4ab087f7d5fd6ea749b57cccce26c
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6193788"
---
# <a name="mathematics-of-lighting"></a>Formules mathématiques d’éclairage


Le modèle d’éclairage Direct3D prend en charge les éclairages ambiant, diffus, spéculaire et émissif. La souplesse de la configuration permet de résoudre de nombreuses situations d’éclairage. Dans une scène, la quantité globale d’éclairage s’appelle l’*illumination globale*.

L’illumination globale est calculée comme suit:

```
Global Illumination = Ambient Light + Diffuse Light + Specular Light + Emissive Light 
```

L’[éclairage ambiant](ambient-lighting.md) est une valeur constante. L’éclairage ambiant est constant dans toutes les directions et colore tous les pixels d’un objet de la même façon. Il présente une grande rapidité de calcul, mais donne aux objets un aspect plat et irréaliste.

L’[éclairage diffus](diffuse-lighting.md) dépend de la direction de l’éclairage et de la normale à la surface de l’objet. L’éclairage diffus varie à la surface d’un objet, en réponse aux modifications de la direction d’éclairage et du vecteur numéral à la surface. Le calcul de l’éclairage diffus prend davantage de temps, car cette valeur varie pour chaque vertex d’objet. Parallèlement, il permet de nuancer l’illumination des objets et de leur octroyer une profondeur en 3dimensions.

L’[éclairage spéculaire](specular-lighting.md) désigne les reflets spéculaires vifs qui surviennent lorsque la lumière atteint la surface d’un objet et se reflète sur l’appareil photo. L’éclairage spéculaire est plus intense que la lumière diffuse et tombe plus rapidement sur la surface de l’objet. Le calcul de l’éclairage spéculaire prend plus de temps que celui de l’éclairage diffus. Toutefois, le fait de l’utiliser permet d’ajouter des informations importantes sur une surface.

L’[éclairage émissif](emissive-lighting.md) correspond à la luminosité émise par un objet; par exemple, une lueur. Sous l’effet de l’émission, l’objet rendu paraît lumineux par lui-même. L’émission affecte la couleur d’un objet. Elle peut, par exemple, procurer de la brillance à un matériau sombre qui récupère une portion de la couleur diffuse.

Un éclairage réaliste peut être obtenu en appliquant chacun de ces types d’éclairage sur une scène3D. Les valeurs calculées pour les composants ambiants, émissifs et diffus sont produites en tant que couleur diffuse de vertex, tandis que la valeur associée au composant d’éclairage spéculaire est produite en tant que couleur spéculaire de vertex. Les valeurs d’éclairage ambiant, diffus et spéculaire peuvent être affectées par l’atténuation et le facteur de point lumineux d’un éclairage donné. Voir [Atténuation et facteur de point lumineux](attenuation-and-spotlight-factor.md).

Pour obtenir un effet d’éclairage plus réaliste, vous ajoutez davantage de lumières; toutefois, ici, le rendu de la scène prend plus de temps à s’afficher. Pour concrétiser tous les effets voulus par un concepteur, certains jeux sollicitent une puissance CPU supérieure à la valeur généralement disponible. Dans ce cas, il est courant de réduire le nombre de calculs d’éclairage au minimum, via l’utilisation de mappages d’éclairage et de mappages d’environnement. Cette configuration permet d’ajouter de l’éclairage à une scène durant l’application des mappages de textures.

L’éclairage est calculé dans l’espace de la caméra. Voir [Transformations de l’espace de caméra](camera-space-transformations.md). L’éclairage optimisé peut être calculé dans l’espace du modèle, quand des conditions spéciales sont réunies: les vecteurs normaux sont déjà normalisés, aucune fusion des vertex n’est nécessaire et les matrices de transformation sont orthogonales.

L’ensemble des calculs d’éclairage sont exécutés dans l’espace du modèle, par la transformation de la position et de la direction de la source d’éclairage, mais aussi par l’ajustement de la position de la caméra. Ainsi, l’espace est modélisé via la transposition inverse de la matrice universelle. En conséquence, si les matrices universelle ou d’affichage introduisent une mise à l’échelle non uniforme, l’éclairage obtenu peut se révéler inexact.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


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
<td align="left"><p><a href="ambient-lighting.md">Éclairage ambiant</a></p></td>
<td align="left"><p>L’éclairage ambiant permet de donner à une scène un éclairage constant. Cette fonction permet d'éclairer tous les vertex d'objets de la même manière car elle ne dépend pas des autres facteurs d'éclairage, comme les normales de vertex, la direction de la lumière, la position de la lumière, la plage ou l'atténuation. L'éclairage ambiant est constant dans toutes les directions et colore tous les pixels d’un objet de la même façon. Il présente une grande rapidité de calcul, mais donne aux objets un aspect plat et irréaliste.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-lighting.md">Éclairage diffus</a></p></td>
<td align="left"><p>L'<em>éclairage diffus</em> dépend de la direction de la lumière et de la surface de l’objet. L’éclairage diffus varie à la surface d’un objet, en réponse aux modifications de la direction d’éclairage et du vecteur numéral à la surface. Le calcul de l’éclairage diffus prend davantage de temps, car cette valeur varie pour chaque vertex d’objet. Parallèlement, il permet de nuancer l’illumination des objets et de leur octroyer une profondeur en 3dimensions.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-lighting.md">Éclairage spéculaire</a></p></td>
<td align="left"><p>L'<em>éclairage spéculaire</em> identifie les points spéculaires lumineux qui apparaissent lorsque la lumière atteint la surface de l’objet et la reflète vers l’appareil photo. L’éclairage spéculaire est plus intense que la lumière diffuse et tombe plus rapidement sur la surface de l’objet. Le calcul de l’éclairage spéculaire prend plus de temps que celui de l’éclairage diffus. Toutefois, le fait de l’utiliser permet d’ajouter des informations importantes sur une surface.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="emissive-lighting.md">Éclairage émissif</a></p></td>
<td align="left"><p>L'<em>éclairage émissif</em> est la lumière émise par un objet; par exemple, un éclat. Sous l’effet de l’émission, l’objet rendu paraît lumineux par lui-même. L’émission affecte la couleur d’un objet. Elle peut, par exemple, procurer de la brillance à un matériau sombre qui récupère une portion de la couleur diffuse.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="camera-space-transformations.md">Transformations de l’espace de caméra</a></p></td>
<td align="left"><p>Les vertex dans l’espace de caméra sont calculés en transformant les vertex de l’objet avec la matrice globale.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="attenuation-and-spotlight-factor.md">Atténuation et facteur de point lumineux</a></p></td>
<td align="left"><p>Les composants d’éclairage diffus et spéculaire de l’équation d’illumination globale contiennent des termes qui décrivent l’atténuation de lumière et le cône de point lumineux.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Lumières et matériaux](lights-and-materials.md)

 

 




