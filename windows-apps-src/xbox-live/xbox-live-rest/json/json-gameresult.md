---
title: GameResult (JSON)
assetID: 43d863c0-2179-ae46-5d4a-2f08cd44b667
permalink: en-us/docs/xboxlive/rest/json-gameresult.html
description: " GameResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b408b1aaae5e6f54958a016575c4a2c37765f1e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602334"
---
# <a name="gameresult-json"></a>GameResult (JSON)
Un objet JSON qui représente les données qui décrit les résultats d’une session de jeu. 
<a id="ID4EN"></a>

  
 
L’objet JSON de GameResult possède les membres suivants.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| blob| tableau d’entiers non signés 8 bits| Données de résultat spécifique titre personnalisé.| 
| Résultat| chaîne| Le résultat de la participation du joueur dans la session de jeu. Les valeurs valides sont « Win », « Perte » ou « Lier ». | 
| score| entier signé 64 bits| Le score le joueur reçu dans la session de jeu.| 
| heure| entier signé 64 bits| Heure du lecteur pour la session de jeu.| 
| xuid| entier non signé 64 bits| L’ID d’utilisateur Xbox du lecteur auxquels s’appliquent les résultats.| 
  
<a id="ID4EPC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
   "xuid": 2533274790412952,
   "outcome": "Win",
   "score": 100
}
    
```

  
<a id="ID4EYC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E1C"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   