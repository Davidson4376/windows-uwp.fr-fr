---
title: PermissionCheckResult (JSON)
assetID: 1cf147fa-4ff1-3299-0822-0fc1726d1600
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresult.html
description: " PermissionCheckResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dc13826be1b6f81201d069f5ade7ea5ba6668cd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598444"
---
# <a name="permissioncheckresult-json"></a>PermissionCheckResult (JSON)
Les résultats d’une vérification à partir d’un seul utilisateur pour un paramètre d’autorisation unique par rapport à un utilisateur de cible unique. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckresult"></a>PermissionCheckResult
 
L’objet PermissionCheckResult a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| reason| chaîne| Facultatif. Un <b>PermissionResultCode</b> valeur qui indique la raison pour laquelle l’autorisation a été refusée si <b>IsAllowed</b> a la valeur false.| 
| restrictedSetting| chaîne| Facultatif. Si le <b>PermissionResultCode</b> valeur dans le <b>raison</b> membre indique qu’une vérification de privilège pour le demandeur a échoué, indique les privilèges a échoué.| 
  
<a id="ID4E6B"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "reason": "MissingPrivilege",
    "restrictedSetting": "VideoCommunications"
}
    
```

  
<a id="ID4EIC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EKC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   