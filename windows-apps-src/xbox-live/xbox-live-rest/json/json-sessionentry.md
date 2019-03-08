---
title: SessionEntry (JSON)
assetID: b5cf5c3d-83b8-635f-d1a5-0be5d9434ea5
permalink: en-us/docs/xboxlive/rest/json-sessionentry.html
description: " SessionEntry (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 73133f898ff219477cb60f54798cbd81acb87ebe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589734"
---
# <a name="sessionentry-json"></a>SessionEntry (JSON)
Contient les données d’une session de Fitness (pertinence). 
<a id="ID4EN"></a>

 
## <a name="sessionentry"></a>SessionEntry
 
L’objet SessionEntry a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| durationInSeconds| entier signé 32 bits | Durée, en secondes, de la session. | 
| joules| entier signé 32 bits | Énergie — en joules — gravés dans la session. | 
| remplies| nombre à virgule flottante simple précision| Moyenne atteignent la valeur sur la durée de session. La valeur de l’économie est le rapport de taux métabolique d’un individu pendant une activité par rapport aux taux métabolique de l’individu au repos. Étant donné que le taux métabolique positionné est 1.0, quel que soit le poids d’un individu et économie valeurs sont par rapport à un individu positionné taux métabolique, ils permettent à comparer l’intensité d’une activité en cours d’exécution par des personnes des différents poids.| 
| serverTimestamp| DateTime| Heure, basé sur l’heure UTC, entrée a été entrée sur le serveur. | 
| Source| entier non signé 8 bits| Source de la session.| 
| Horodatage| DateTime| Heure, basée sur le temps universel coordonné (UTC) : entrée a été créée sur le client. | 
| titleId| entier non signé 64 bits| Titre : au format décimal, qui créé l’entrée.| 
  
<a id="ID4EFE"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
   "titleId" : "1234567",
   "timestamp" : "2011-11-18T08:08:46Z",
   "serverTimestamp" : "2011-11-20T04:04:23Z",
   "durationInSeconds" : 240,
   "joules" :  1600,
   "met" :  "124"
   "source" :  "1"
}
    
```

  
<a id="ID4EOE"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EQE"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   