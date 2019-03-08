---
title: UpdateMetadataRequest (JSON)
assetID: 0bc210e3-c1dc-9267-e322-aadb9f0a074a
permalink: en-us/docs/xboxlive/rest/json-updatemetadatarequest.html
description: " UpdateMetadataRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a76e4b12e0ffadb112913775b500ac0d39d413d5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597764"
---
# <a name="updatemetadatarequest-json"></a>UpdateMetadataRequest (JSON)
Les métadonnées doivent être mises à jour pour un élément. 
<a id="ID4EN"></a>

 
## <a name="updatemetadatarequest"></a>UpdateMetadataRequest
 
L’objet UpdateMetadataRequest a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| userCaption| chaîne| Convertit la chaîne non localisée utilisateur entré pour le clip de jeu.| 
| visibility| [Énumération de GameClipVisibility](../enums/gvr-enum-gameclipvisibility.md)| Modifie la visibilité de l’élément de jeux qu’il sera publié dans le système.| 
| titleData| chaîne| Conteneur de propriétés du titre spécifique. Taille maximale : 10 KO.| 
  
<a id="ID4EBC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 
Modification de nom de l’élément et la visibilité :
 

```json
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
Permet de modifier uniquement les propriétés titre (il s’agit juste d’un exemple, étant donné que le schéma de ce champ est à l’appelant) :
 

```json
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>Référence 

[POST (/ users/me/scids / {scid} /clips/ {gameClipId})](../uri/dvr/uri-usersmescidclipsgameclipidpost.md)

   