---
author: andrewleader
Description: Here are the elements and attributes you use to create adaptive tiles.
title: Schéma et modèles de vignette adaptative
ms.assetid: 858FB05E-87A2-49CF-BE48-570980AD36C8
label: Adaptive tile schema and templates
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 30a0e3056f8b7be2ed9d033e2da57795aec6946f
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4058373"
---
# <a name="adaptive-tile-templates-schema-and-guidance"></a>Modèles de vignette adaptative: schéma et conseils

 

Voici les éléments et attributs permettant de créer des vignettes adaptatives. Consultez les instructions et les exemples indiqués dans [Créer des vignettes adaptatives](create-adaptive-tiles.md).

## <a name="tile-element"></a>élément de vignette


``` xml
<tile>
  
  <!-- Child elements -->
  visual
  
</tile>
```

## <a name="visual-element"></a>élément visuel


``` xml
<visual
  version? = integer
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string >
    
  <!-- Child elements -->
  binding+

</visual>
```

## <a name="binding-element"></a>élément de liaison


``` xml
<binding
  template = tileTemplateNameV3
  fallback? = tileTemplateNameV1
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string
  hint-textStacking? = "top" | "center" | "bottom"
  hint-overlay? = [0-100] >

  <!-- Child elements -->
  ( image
  | text
  | group
  )*

</binding>
```

## <a name="image-element"></a>élément d’image


``` xml
<image
  src = string
  placement? = "inline" | "background" | "peek"
  alt? = string
  addImageQuery? = boolean
  hint-crop? = "none" | "circle"
  hint-removeMargin? = boolean
  hint-align? = "stretch" | "left" | "center" | "right" />
```

## <a name="text-element"></a>élément de texte


``` xml
<text
  lang? = string
  hint-style? = textStyle
  hint-wrap? = boolean
  hint-maxLines? = integer
  hint-minLines? = integer
  hint-align? = "left" | "center" | "right" >

  <!-- text goes here -->

</text>
```

Valeurs de textStyle: caption captionSubtle body bodySubtle base baseSubtle subtitle subtitleSubtle title titleSubtle titleNumeral subheader subheaderSubtle subheaderNumeral header headerSubtle headerNumeral

## <a name="group-element"></a>élément de groupe


``` xml
<group>

  <!-- Child elements -->
  subgroup+

</group>
```

## <a name="subgroup-element"></a>élément de sous-groupe


``` xml
<subgroup
  hint-weight? = [0-100]
  hint-textStacking? = "top" | "center" | "bottom" >

  <!-- Child elements -->
  ( text
  | image
  )*

</subgroup>
```

## <a name="related-topics"></a>Rubriques connexes


* [Créer des vignettes adaptatives](create-adaptive-tiles.md)
 

 




