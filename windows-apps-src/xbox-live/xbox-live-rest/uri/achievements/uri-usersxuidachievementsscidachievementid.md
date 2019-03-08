---
title: /users/xuid({xuid})/achievements/{scid}/{achievementid}
assetID: 656a6d63-1a11-b0a5-63d2-2b010abd62e7
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementid.html
description: " /users/xuid({xuid})/achievements/{scid}/{achievementid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 00c577f60b67f15f75c47b5e737ca12819695110
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599444"
---
# <a name="usersxuidxuidachievementsscidachievementid"></a>/users/xuid({xuid})/achievements/{scid}/{achievementid}
Retourne des détails concernant la prime, y compris ses métadonnées configurée et les données spécifiques à l’utilisateur. 

> [!NOTE] 
> Prise en charge uniquement pour la plateforme. 

 
Le domaine pour ces URI est `achievements.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4E2)
 
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | 
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur dont la ressource est accédée. Doit correspondre à la XUID de l’utilisateur authentifié.| 
| scid| GUID| Identificateur unique de la configuration de service dont la réalisation est accédée.| 
| achievementid| entier non signé 32 bits| Identificateur unique (au sein de la SCID spécifié) de la prime est accédée.| 
  
<a id="ID4EMC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})](uri-usersxuidachievementsscidachievementidget.md)

&nbsp;&nbsp;Obtient les détails de la prime.
 
<a id="ID4EWC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EYC"></a>

 
##### <a name="parent"></a>Parent 

[URI de primes](atoc-reference-achievementsv2.md)

   