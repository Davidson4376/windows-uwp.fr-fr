---
title: PersonSummary (JSON)
assetID: 22fedb5f-5602-98d8-04a6-786fe3905921
permalink: en-us/docs/xboxlive/rest/json-personsummary.html
description: " PersonSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a787992507405a70185140e879be731d72806eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612624"
---
# <a name="personsummary-json"></a>PersonSummary (JSON)
Collection de [personne (JSON)](json-person.md) objets. 
<a id="ID4ER"></a>

 
## <a name="personsummary"></a>PersonSummary
 
L’objet PersonSummary a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| hasCallerMarkedTargetAsFavorite| Valeur booléenne| Indique si l’appelant a marqué la cible en tant que favori. Exemples de valeurs : true| 
| hasCallerMarkedTargetAsKnown| Valeur booléenne| Indique si l’appelant a marqué la cible comme connues. Exemples de valeurs : true| 
| isCallerFollowingTarget| Valeur booléenne| Indique si l’appelant est après la cible. Exemples de valeurs : true| 
| isTargetFollowingCaller| Valeur booléenne| Indique si la cible est suivant l’appelant. Exemples de valeurs : true| 
| legacyFriendStatus| chaîne| État hérité friend de la cible, comme indiqué par l’appelant. Peut être « None », « MutuallyAccepted », « OutgoingRequest » ou « IncomingRequest ». Exemples de valeurs « MutuallyAccepted »| 
| recentChangeCount| entier non signé 32 bits| Facultatif. Nombre de changements récents dans le graphique social de la cible. Cette valeur existe uniquement lorsqu’un utilisateur consulte leur propres résumé. Exemples de valeurs 5| 
| targetFollowerCount| > entier non signé 32 bits| Nombre de personnes qui suivent la cible. Exemples de valeurs 1308| 
| targetFollowingCount| entier non signé 32 bits| Nombre de personnes qui suivre la cible. Exemples de valeurs 112| 
| Filigrane| chaîne| Facultatif. Filigrane de modifications récentes pour la cible. Cette valeur existe uniquement lorsqu’un utilisateur consulte leur propres résumé. Exemples de valeurs 5| 
  
<a id="ID4E4D"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}
    
```

  
<a id="ID4EGE"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EIE"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   