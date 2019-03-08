---
title: ServiceError (JSON)
assetID: 81c43f6e-bfff-c4b5-d25c-eace22649f01
permalink: en-us/docs/xboxlive/rest/json-serviceerror.html
description: " ServiceError (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da3d682a1b66d25a12f21a93e9596d13afae7f90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631704"
---
# <a name="serviceerror-json"></a>ServiceError (JSON)
Contient des informations sur une erreur retournée lors de l’échec d’un appel au service. 
<a id="ID4EN"></a>

 
## <a name="serviceerror"></a>ServiceError
 
L’objet constatez a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| code| entier signé 32 bits | Le type d’erreur. Consultez le tableau ci-dessous pour les valeurs possibles. | 
| Source| chaîne | Le nom du service qui a généré l’erreur. Par exemple, une valeur de <code>ReputationFD</code> indique que l’erreur a été dans le service de réputation. | 
| description| chaîne| Une description de l’erreur. | 
 
<a id="ID4EBC"></a>

 
### <a name="error-codes"></a>Codes d’erreur
 
| Valeur| Description| 
| --- | --- | --- | --- | --- | 
| 0| Réussite sans erreur| 
| 4000| Document JSON le corps de demande non valide envoyé avec une validation d’échec de la demande POST. Consultez le champ de description pour plus d’informations. | 
| 4100| Utilisateur est pas existe le XUID contenues dans l’URI de demande ne représente pas un utilisateur valide sur XBOX Live.| 
| 4500| Erreur d’autorisation l’appelant n’est pas autorisé à effectuer l’opération demandée.| 
| 5000| Service erreur s’est une erreur interne dans le service| 
| 5300| Service non disponible le service n’est pas disponible.| 
   
<a id="ID4EQE"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
    
```

  
<a id="ID4EZE"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E2E"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   