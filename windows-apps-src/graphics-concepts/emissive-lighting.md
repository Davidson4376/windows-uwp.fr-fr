---
title: "Éclairage émissif"
description: "L’éclairage émissif correspond à la luminosité émise par un objet ; par exemple, une lueur."
ms.assetid: 262EB9E2-DF96-401F-93D6-51A7BB60074B
keywords:
- "Éclairage émissif"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 50bc904e5810340846b3fd84ffca214d07d38750
ms.lasthandoff: 02/07/2017

---

# <a name="emissive-lighting"></a>Éclairage émissif


L’*éclairage émissif* correspond à la luminosité émise par un objet ; par exemple, une lueur. Sous l’effet de l’émission, l’objet rendu paraît lumineux par lui-même. L’émission affecte la couleur d’un objet et peut, par exemple, procurer de la brillance à un matériau sombre et assurer une partie de la couleur émise.

L’éclairage émissif est décrit par un seul terme.

Éclairage émissif = Cₑ

où :

| Paramètre | Valeur par défaut | Type                                                                 | Description     |
|-----------|---------------|----------------------------------------------------------------------|-----------------|
| Cₑ        | (0,0,0,0)     | Rouge, vert, bleu et transparence alpha (correspondant tous à des valeurs en virgule flottante) | Couleur émissive. |

 

Les valeurs possibles de Cₑ sont couleur 1 ou couleur 2. Si la couleur de vertex n’est pas fournie, la couleur émissive du matériau est utilisée.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


Dans cet exemple, la couleur de l’objet est définie à partir de la lumière ambiante de la scène et d’une couleur ambiante de matériau.

Selon l’équation, la couleur résultante des vertex de l’objet correspond à la couleur du matériau.

La figure ci-après illustre la couleur du matériau (vert). La lumière émissive éclaire de la même couleur tous les vertex de l’objet. Elle ne dépend ni de la normale de vertex, ni de la direction de la lumière. La sphère apparaît donc comme un cercle 2D, car il n’y a aucune différence d’ombrage autour de la surface de l’objet.

![illustration d’une sphère verte](images/lighte.jpg)

La figure ci-après illustre la façon dont la lumière émissive fusionne avec les trois autres types de lumières. Le côté droit de la sphère présente un mélange de lumière émissive verte et de lumière ambiante rouge. Sur le côté gauche de la sphère, la lumière émissive verte fusionne avec la lumière ambiante rouge et la lumière diffuse en produisant un dégradé rouge. Le reflet spéculaire est blanc en son centre et crée un anneau jaune lorsque la valeur de lumière spéculaire diminue brutalement en laissant les valeurs de lumière ambiante, diffuse et émissive qui fusionnent pour produire du jaune.

![illustration d’une sphère verte avec une lumière émissive](images/lightadse.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Formules mathématiques d’éclairage](mathematics-of-lighting.md)

 

 





