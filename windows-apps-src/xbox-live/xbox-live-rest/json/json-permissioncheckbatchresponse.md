---
title: PermissionCheckBatchResponse (JSON)
assetID: f157ed8d-7483-1b34-bc3d-e3fcf6a7d055
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchresponse.html
description: " PermissionCheckBatchResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6ac79d3361e83993c372b1d651e97e900788d39f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613344"
---
# <a name="permissioncheckbatchresponse-json"></a>PermissionCheckBatchResponse (JSON)
Les résultats d’une autorisation de traitement par lots recherchent une liste de valeurs d’autorisation pour plusieurs utilisateurs. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchresponse"></a>PermissionCheckBatchResponse
 
L’objet PermissionCheckBatchResponse a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| Réponses| Tableau de [PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)| Obligatoire. Un [PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md) objet pour chaque autorisation a été demandée pour la demande d’origine, dans le même ordre que dans cette demande.| 
  
<a id="ID4EQB"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRestrictsTarget", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}
    
```

  
<a id="ID4EZB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E2B"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   