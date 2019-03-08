---
title: TitleBlob (JSON)
assetID: fd1c904d-e8d0-f61f-e403-40b25bd4ac14
permalink: en-us/docs/xboxlive/rest/json-titleblob.html
description: " TitleBlob (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a0b17a46d1c71ffdf9098d4637ca59d840c90a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612584"
---
# <a name="titleblob-json"></a>TitleBlob (JSON)
Contient des informations sur une vignette à partir de stockage. 
<a id="ID4EP"></a>

 
## <a name="titleblob"></a>TitleBlob
 
L’objet TitleBlob a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| clientFileTime| DateTime| [facultatif] Date et heure du dernier téléchargement du fichier.| 
| displayName| chaîne| [facultatif] Nom du fichier qui est affiché à l’utilisateur.| 
| ETag| chaîne| Indicateur du fichier utilisé dans de télécharger et de demandes de téléchargement.| 
| fileName| chaîne| Nom du fichier.| 
| size| entier signé 64 bits| Taille du fichier en octets.| 
| smartBlobType| chaîne| [facultatif] Type de données. Les valeurs possibles sont : config, json, binaire.| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "fileName":"foo\bar\blob.txt,binary",
    "clientFileTime":"2012-01-01T01:02:03.1234567Z",
    "displayName":"Friendly Name",
    "size":12,
    "etag":"0x8CEB3E4F8F3A5BF",
    "smartBlobType":"binary"
}
      
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   