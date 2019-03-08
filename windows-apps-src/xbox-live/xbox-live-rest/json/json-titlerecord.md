---
title: TitleRecord (JSON)
assetID: 8e1bd699-e408-67c8-31da-2d968adfbc21
permalink: en-us/docs/xboxlive/rest/json-titlerecord.html
description: " TitleRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89baf7e9a737428d492246f1647a561a4a8170cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603984"
---
# <a name="titlerecord-json"></a>TitleRecord (JSON)
Informations sur un titre, y compris son nom et un horodatage de dernière modification. 
<a id="ID4EN"></a>

 
## <a name="titlerecord"></a>TitleRecord
 
Un TitleRecord doit contenir un DeviceRecord ou un LastSeenRecord, mais ne peut pas contenir à la fois.
 
L’objet TitleRecord a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| id| entier non signé 32 bits| N ° titre de l’enregistrement.| 
| name| chaîne| Nom localisé du titre.| 
| activité| [ActivityRecord](json-activityrecord.md)| L’activité de l’utilisateur dans le titre. Renvoyé uniquement si la profondeur est « tout ».| 
| lastModified| DateTime| Horodatage UTC lors de la dernière mise à jour de l’enregistrement.| 
| sélection élective| chaîne| L’emplacement de l’application dans l’interface utilisateur. Possibilités incluent « fill », « complète », « aligné » ou « en arrière-plan ». La valeur par défaut est « complet » pour les appareils sans la possibilité de placer des applications.| 
| État| chaîne| L’état du titre. Peut être « actif » ou « inactif » (la valeur par défaut). Le titre définit l’état en fonction de ses propres critères pour l’activité et d’inactivité.| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    }
    
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUD"></a>

 
##### <a name="reference"></a>Référence 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   