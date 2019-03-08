---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting
assetID: cf8319f6-46a2-b263-ea4c-f1ce403b571b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcasting.html
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 98eaa60204e3c98eb1b09a13372f7b0c084a6608
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651464"
---
# <a name="usersxuidxuidgroupsmonikerbroadcasting"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting
Accède à l’enregistrement de la présence des utilisateurs de diffusion spécifiés par le moniker de groupes liés à la XUID qui s’affiche dans l’URI. Le domaine pour ces URI est `userpresence.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| chaîne| Xbox utilisateur ID (XUID) de l’utilisateur lié aux XUIDs dans le groupe.| 
| moniker| chaîne| Chaîne définissant le groupe d’utilisateurs. Le moniker du acceptées uniquement à l’heure actuelle est « People », avec un « P » majuscule.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting )](uri-usersxuidgroupsmonikerbroadcastingget.md)

&nbsp;&nbsp;Récupère l’enregistrement de la présence des utilisateurs de diffusion spécifiés par le moniker de groupes lié à la XUID qui s’affiche dans l’URI.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 

[URI de la présence](atoc-reference-presence.md)

   