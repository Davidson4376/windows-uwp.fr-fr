---
title: PermissionCheckBatchRequest (JSON)
assetID: 3100b17c-1b60-fdf2-3af9-7e424f1903ee
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchrequest.html
description: " PermissionCheckBatchRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 538547c85648970ab3e9fe3ae413e8a03df814ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623504"
---
# <a name="permissioncheckbatchrequest-json"></a>PermissionCheckBatchRequest (JSON)
Collection d’objets de PermissionCheckBatchRequest. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckbatchrequest"></a>PermissionCheckBatchRequest
 
L’objet PermissionCheckBatchRequest a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| Utilisateurs| Tableau d’utilisateurs| Obligatoire. Tableau de cibles pour vérifier l’autorisation par rapport à. Chaque entrée dans ce tableau est un ID d’utilisateur Xbox (XUID) ou un utilisateur hors réseau anonyme pour les scénarios de réseau (« anonymousUser » : « allUsers »). | 
| Permissions| Tableau de [PermissionId énumération](../enums/privacy-enum-permissionid.md)| Obligatoire. Les autorisations pour la vérification par rapport à chaque utilisateur.| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ShareFriendList",
        "ShareGameHistory"
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   