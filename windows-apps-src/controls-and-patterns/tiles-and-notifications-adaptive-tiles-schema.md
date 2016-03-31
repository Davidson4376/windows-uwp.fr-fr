---
Description: Voici les éléments et attributs permettant de créer des vignettes adaptatives.
title: Schéma et modèles de vignette adaptative
ms.assetid: 858FB05E-87A2-49CF-BE48-570980AD36C8
label: Schéma et modèles de vignette adaptative
template: detail.hbs
---

# Modèles de vignette adaptative : schéma et conseils


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Voici les éléments et attributs permettant de créer des vignettes adaptatives. Pour consulter des instructions et des exemples, voir [Créer des vignettes adaptatives](tiles-and-notifications-create-adaptive-tiles.md).

## <span id="tile_element"> </span> <span id="TILE_ELEMENT"> </span>Élément de vignette


``` syntax
<tile>
  
  <!-- Child elements -->
  visual
  
</tile>
```

## <span id="visual_element"> </span> <span id="VISUAL_ELEMENT"> </span>Élément visuel


``` syntax
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

## <span id="binding_element"> </span> <span id="BINDING_ELEMENT"> </span>Élément de liaison


``` syntax
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

## <span id="image_element"> </span> <span id="IMAGE_ELEMENT"> </span>Élément d’image


``` syntax
<image
  src = string
  placement? = "inline" | "background" | "peek"
  alt? = string
  addImageQuery? = boolean
  hint-crop? = "none" | "circle"
  hint-removeMargin? = boolean
  hint-align? = "stretch" | "left" | "center" | "right" />
```

## <span id="text_element"> </span> <span id="TEXT_ELEMENT"> </span>Élément de texte


``` syntax
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

Valeurs de textStyle : caption captionSubtle body bodySubtle base baseSubtle subtitle subtitleSubtle title titleSubtle titleNumeral subheader subheaderSubtle subheaderNumeral header headerSubtle headerNumber

## <span id="group_element"> </span> <span id="GROUP_ELEMENT"> </span>Élément de groupe


``` syntax
<group>

  <!-- Child elements -->
  subgroup+

</group>
```

## <span id="subgroup_element"> </span> <span id="SUBGROUP_ELEMENT"> </span>Élément de sous-groupe


``` syntax
<subgroup
  hint-weight? = [0-100]
  hint-textStacking? = "top" | "center" | "bottom" >

  <!-- Child elements -->
  ( text
  | image
  )*

</subgroup>
```

**Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui créent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Rubriques connexes


* [Créer des vignettes adaptatives](tiles-and-notifications-create-adaptive-tiles.md)
 

 




<!--HONumber=Mar16_HO1-->
