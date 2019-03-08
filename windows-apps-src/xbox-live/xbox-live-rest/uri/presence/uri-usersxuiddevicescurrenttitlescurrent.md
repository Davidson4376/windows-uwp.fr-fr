---
title: /users/xuid({xuid})/devices/current/titles/current
assetID: f149c68b-9874-e348-4e1d-6acf5d305c49
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrent.html
description: " /users/xuid({xuid})/devices/current/titles/current"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0b5c17f1791d69ca8a4c902b6c37736c4da28a31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645664"
---
# <a name="usersxuidxuiddevicescurrenttitlescurrent"></a>/users/xuid({xuid})/devices/current/titles/current
Accéder à la présence d’un titre ou un utilisateur d’un titre. Le domaine pour ces URI est `userpresence.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur cible.| 
  
<a id="ID4EUB"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[DELETE (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentdelete.md)

&nbsp;&nbsp;Supprimer la présence d’un titre de fermeture, au lieu d’attendre le [PresenceRecord](../../json/json-presencerecord.md) le point d’expirer.

[POST (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentpost.md)

&nbsp;&nbsp;Mettre à jour un titre avec la présence d’un utilisateur.
 
<a id="ID4EBC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EDC"></a>

 
##### <a name="parent"></a>Parent 

[URI de la présence](atoc-reference-presence.md)

   