---
title: MultiplayerActivityDetails (JSON)
assetID: f982aa5e-2694-4ef9-bc55-6c099a3cf9ec
permalink: en-us/docs/xboxlive/rest/json-multiplayeractivitydetails.html
description: " MultiplayerActivityDetails (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 188bcebb8d6bff879f30dcc83d7039fbcbfae0b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658104"
---
# <a name="multiplayeractivitydetails-json"></a>MultiplayerActivityDetails (JSON)
Un objet JSON qui représente le **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**. 

> [!NOTE] 
> Cet objet est implémenté par le mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure.  

 
<a id="ID4ES"></a>

  
 
L’objet JSON de MultiplayerActivityDetails a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | --- | 
| SessionReference| MultiplayerSessionReference| Un <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference</b> objet représentant les informations d’identification de la session.| 
| HandleId| entier non signé 64 bits| L’ID de handle correspondant à l’activité.| 
| TitleId| entier non signé 32 bits| L’ID de titre qui doit être lancée pour joindre l’activité.| 
| Visibilité| MultiplayerSessionVisibility| Un <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b> valeur indiquant l’état de visibilité de la session.| 
| JoinRestriction| MultiplayerSessionJoinRestriction| Un <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionJoinRestriction</b> valeur indiquant la restriction de jointure pour la session. Cette restriction s’applique si le champ de visibilité est défini sur « Ouvrir ».| 
| Clos| Valeur booléenne| True si la session est fermée temporairement pour joindre et false sinon.| 
| OwnerXboxUserId| entier non signé 64 bits| ID d’utilisateur de Xbox du membre qui est propriétaire de l’activité.| 
| MaxMembersCount| entier non signé 32 bits| Nombre d’emplacements au total.| 
| MembersCount| entier non signé 32 bits| Nombre d’emplacements occupés.| 
  
<a id="ID4E3D"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
  "results": [{
    "id": "11111111-ebe0-42da-885f-033860a818f6",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "party",
      "name": "e3a836aeac6f4cbe9bcab985494d3175"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": true,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  },
  {
    "id": "11111111-ebe0-42da-885f-033860a818f7",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "TitleStorageTestDefault",
      "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": false,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  }]
}
    
```

  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   