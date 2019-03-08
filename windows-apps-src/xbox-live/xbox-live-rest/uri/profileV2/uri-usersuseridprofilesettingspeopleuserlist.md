---
title: /users/{userId}/profile/settings/people/{userList}?settings={settings}
assetID: 0ba20eba-f0ab-28ab-61d3-b4f9e4c07bc5
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlist.html
description: " /users/{userId}/profile/settings/people/{userList}?settings={settings}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 24b58c817156a7c372a8e6acfab895e6b7c51207
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636964"
---
# <a name="usersuseridprofilesettingspeopleuserlistsettingssettings"></a>/users/{userId}/profile/settings/people/{userList}?settings={settings}
Accéder au profil pour un ou plusieurs utilisateurs, avec prise en charge du Moniker de personnes. Le domaine pour ces URI est `profile.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| userId| chaîne| Puis-je être 'xuid(12345)', 'gt(myGamertag)' ou 'me'.| 
| userList| chaîne| Une liste nommée de personnes pour obtenir les paramètres de. Actuellement, les personnes est la seule liste pris en charge.| 
  
<a id="ID4E1B"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET (/users/{userId}/profile/settings/people/{userList})](uri-usersuseridprofilesettingspeopleuserlistget.md)

&nbsp;&nbsp;Obtenir le profil d’un utilisateur ou les utilisateurs, avec un Moniker de personnes prennent en charge.
 
<a id="ID4EEC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EGC"></a>

 
##### <a name="parent"></a>Parent 

[URI de profils](atoc-reference-profiles.md)

 [Profil (JSON)](../../json/json-profile.md)

   