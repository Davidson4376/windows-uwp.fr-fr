---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting/count
assetID: 535c8d46-7001-c31e-3e9d-82ad275095ae
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcastingcount.html
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting/count"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7a39bc9c3302ba26949700774997355a216fe70d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651354"
---
# <a name="usersxuidxuidgroupsmonikerbroadcastingcount"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting/count
Accède au nombre d’utilisateurs diffusion spécifiés par le moniker de groupes liés à la XUID qui s’affiche dans l’URI. Le domaine pour ces URI est `userpresence.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| chaîne| Xbox utilisateur ID (XUID) de l’utilisateur lié aux XUIDs dans le groupe.| 
| moniker| chaîne| Chaîne définissant le groupe d’utilisateurs. Le moniker du acceptées uniquement à l’heure actuelle est « People », avec un « P » majuscule.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )](uri-usersxuidgroupsmonikerbroadcastingcountget.md)

&nbsp;&nbsp;Récupère le nombre d’utilisateurs diffusion spécifiés par le moniker de groupes lié à la XUID qui s’affiche dans l’URI.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 

[URI de la présence](atoc-reference-presence.md)

   