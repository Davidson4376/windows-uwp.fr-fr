---
title: PermissionCheckResponse (JSON)
assetID: 7a749001-b569-b0e0-2a35-f299474c8710
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresponse.html
description: " PermissionCheckResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7623a45fa21e30a015bd5c6a7c1f5add19cc189b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623364"
---
# <a name="permissioncheckresponse-json"></a>PermissionCheckResponse (JSON)
Les résultats d’une vérification à partir d’un seul utilisateur pour un paramètre d’autorisation unique par rapport à un utilisateur de cible unique. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckresponse"></a>PermissionCheckResponse
 
L’objet PermissionCheckResponse a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| IsAllowed| Valeur booléenne| Obligatoire. Ce membre est <b>true</b> si l’utilisateur demandeur est autorisé à effectuer l’action demandée par l’utilisateur cible.| 
| Résultats| Tableau de [PermissionCheckResult (JSON)](json-permissioncheckresult.md)| Facultatif. Si <b>IsAllowed</b> a la valeur false et la vérification a été refusée par quelque chose lié au demandeur, indique la raison pour laquelle l’autorisation a été refusée.| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   