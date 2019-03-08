---
title: PermissionCheckBatchUserResponse (JSON)
assetID: c587dbc1-9436-4d55-afcb-deb47e3c2664
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchuserresponse.html
description: " PermissionCheckBatchUserResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c9e20cc195ad737a7e847a8ad41b76247220adfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628354"
---
# <a name="permissioncheckbatchuserresponse-json"></a>PermissionCheckBatchUserResponse (JSON)
Les raisons d’une autorisation de lot vérifier pour la liste des valeurs d’autorisation pour un utilisateur cible unique. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchuserresponse"></a>PermissionCheckBatchUserResponse
 
L’objet PermissionCheckBatchUserResponse a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| Utilisateur| chaîne| Obligatoire. Ce membre est <b>true</b> si l’utilisateur demandeur est autorisé à effectuer l’action demandée par l’utilisateur cible.| 
| Permissions| Tableau de [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)| Obligatoire. Un [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md) pour chaque autorisation a été demandée pour la demande d’origine, dans le même ordre que dans la demande.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "User": {"Xuid": "12345"},
    "Permissions":
    [
        {
            "isAllowed": true
        },
        {
            "isAllowed": false
        },
        {
            "isAllowed": false,
            "reasons":
            [
                {"reason": "BlockedByRequestor"},
                {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
            ]
        }
    ]
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   