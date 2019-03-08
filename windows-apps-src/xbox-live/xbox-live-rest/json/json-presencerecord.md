---
title: PresenceRecord (JSON)
assetID: 414e6ef5-f7bd-70d0-7386-7aa1c3a56e21
permalink: en-us/docs/xboxlive/rest/json-presencerecord.html
description: " PresenceRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6d531352c4336e00c93a91e7c945602ab69695f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622584"
---
# <a name="presencerecord-json"></a>PresenceRecord (JSON)
Données relatives à la présence en ligne d’un utilisateur unique.
<a id="ID4EN"></a>


## <a name="presencerecord"></a>PresenceRecord

L’objet PresenceRecord a la spécification suivante.

| Membre| Type| Description|
| --- | --- | --- |
| xuid| chaîne| L’ID d’utilisateur Xbox (XUID) de l’utilisateur cible. La présence de données fourni est pour cet utilisateur.|
| appareils| tableau de [DeviceRecord](json-devicerecord.md)| Liste des enregistrements d’appareil de l’utilisateur.|
| État| chaîne| Activité de l’utilisateur sur Xbox LIVE. Valeurs possibles : <ul><li>En ligne : Utilisateur a un enregistrement d’au moins un appareil.</li><li>Suite : L’utilisateur est connecté à Xbox LIVE, mais pas active dans n’importe quel titre.</li><li>hors-ligne : Utilisateur n’est pas présent sur n’importe quel appareil.</li></ul> | 
| lastSeen| [LastSeenRecord](json-lastseenrecord.md)| Les dernières informations de vue sont uniquement disponibles lorsque l’utilisateur n’est aucun DeviceRecords valide. Si l’objet a été supprimé du cache, ses données ne peuvent pas être retournées, car il n’existe aucun magasin persistant.|

<a id="ID4E2C"></a>


## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON


```json
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:"W8",
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page"
      }
    }]
  }]
}

```


<a id="ID4EED"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EGD"></a>


##### <a name="parent"></a>Parent

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EQD"></a>


##### <a name="reference"></a>Référence

[POST (/ users/batch)](../uri/presence/uri-usersbatchpost.md)

 [GET (/users/me)](../uri/presence/uri-usersmeget.md)

 [DELETE (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentdelete.md)

 [GET (/users/xuid({xuid}))](../uri/presence/uri-usersxuidget.md)
