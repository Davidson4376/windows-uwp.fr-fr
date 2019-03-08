---
title: PagingInfo (JSON)
assetID: 645e575d-3e8e-d954-90e6-e51dd83da93b
permalink: en-us/docs/xboxlive/rest/json-paginginfo.html
description: " PagingInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0e773d73499e79fe23f736a536027932ca1a07b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651214"
---
# <a name="paginginfo-json"></a>PagingInfo (JSON)
Contient des informations de pagination pour les résultats sont retournés dans les pages de données. 
<a id="ID4EN"></a>

 
## <a name="paginginfo"></a>PagingInfo
 
| Membre| Type| Description| 
| --- | --- | --- | 
| continuationToken| chaîne| Un jeton de continuation opaque utilisé pour accéder à la page suivante de résultats. Nombre maximal de 32 caractères. Les appelants peuvent fournir cette valeur dans le <b>continuationToken</b> paramètre de requête afin de récupérer l’ensemble suivant d’éléments dans la collection. Si cette propriété est <b>null</b>, il existe des éléments supplémentaires dans la collection. Cette propriété est obligatoire et est fournie même si la collection est averti par radiomessagerie avec <b>skipItems</b>.| 
| totalItems| entier signé 32 bits| Le nombre total d’éléments dans la collection. Il n’est pas fourni si le service ne peut pas fournir une vue en temps réel à la taille de la collection.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
   "continuationToken":"16354135464161213-0708c1ba-147f-48cc-9ad9-546gaadg648"
   "totalItems":5,
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   