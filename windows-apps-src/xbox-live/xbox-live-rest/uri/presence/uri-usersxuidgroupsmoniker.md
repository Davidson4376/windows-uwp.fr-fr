---
title: /users/xuid({xuid})/groups/{moniker}
assetID: 7c73236b-95ee-723b-e5e0-68252c953e14
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmoniker.html
description: " /users/xuid({xuid})/groups/{moniker}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 25ddc8120f05f04d5285fbbe4efc5a41f98265ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617554"
---
# <a name="usersxuidxuidgroupsmoniker"></a>/users/xuid({xuid})/groups/{moniker}
Accède à la PresenceRecord pour un groupe. Le domaine pour ces URI est `userpresence.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| chaîne| Xbox utilisateur ID (XUID) de l’utilisateur lié aux XUIDs dans le groupe.| 
| moniker| chaîne| Chaîne définissant le groupe d’utilisateurs. Le moniker du acceptées uniquement à l’heure actuelle est « People », avec un « P » majuscule.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET (/ users/xuid ({xuid}) /groups/ {nom})](uri-usersxuidgroupsmonikerget.md)

&nbsp;&nbsp;Obtient le PresenceRecord pour un groupe.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 

[URI de la présence](atoc-reference-presence.md)

   