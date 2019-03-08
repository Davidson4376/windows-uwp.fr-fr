---
title: AggregateSessionsResponse (JSON)
assetID: 020ee9b2-c96c-2e65-4e6d-f9f4bd25a374
permalink: en-us/docs/xboxlive/rest/json-aggregatesessionsresponse.html
description: " AggregateSessionsResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ef026fd5096d047b2014faaf95a667c69827e043
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608304"
---
# <a name="aggregatesessionsresponse-json"></a>AggregateSessionsResponse (JSON)
Contient les données agrégées pour les sessions de Fitness (pertinence) d’un utilisateur. 
<a id="ID4EN"></a>

 
## <a name="aggregatesessionsresponse"></a>AggregateSessionsResponse
 
L’objet AggregateSessionsResponse a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| totalDurationInSeconds| entier signé 64 bits| Durée totale de sessions en secondes pendant la période d’agrégation.| 
| totalJoules| entier signé 64 bits| Nombre total de l’énergie consommée — en joules, sur la période d’agrégation. | 
| totalSessions| entier signé 64 bits| Nombre total de sessions sur la période d’agrégation.| 
| weightedAverageMets| nombre à virgule flottante simple précision | Équivalent métabolique moyenne pondérée de la valeur de la tâche (économie) sur la période d’agrégation. La valeur de l’économie est le rapport de taux métabolique d’un individu pendant une activité par rapport aux taux métabolique de l’individu au repos. Étant donné que le taux métabolique positionné est 1.0, quel que soit le poids d’un individu et économie valeurs sont par rapport à un individu positionné taux métabolique, ils permettent à comparer l’intensité d’une activité en cours d’exécution par des personnes des différents poids.| 
  
<a id="ID4ESC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
   "totalSessions" : 300,
   "totalDurationInSeconds" : 1240,
   "totalJoules" :  21600,
   "weightedAvgMet" : 21,
}

    
```

  
<a id="ID4E2C"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E4C"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   