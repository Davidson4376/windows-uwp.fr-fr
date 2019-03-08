---
title: /users/xuid({xuid})/achievements
assetID: 4dd89962-ab73-c25b-7a11-3ed35475492e
permalink: en-us/docs/xboxlive/rest/uri-achievementsusersxuidachievementsv2.html
description: " /users/xuid({xuid})/achievements"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6d4ac8c1fbf3d5f3fcc645e284059c1644495d62
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607984"
---
# <a name="usersxuidxuidachievements"></a>/users/xuid({xuid})/achievements
 
Cet identificateur URI (Universal Resource) fournit l’accès aux primes d’utilisateur.
 
Le domaine pour ces URI est `achievements.xboxlive.com`.
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur dont (ressource) est accessible. Doit correspondre à la XUID de l’utilisateur authentifié.| 
  
<a id="ID4EAC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET](uri-achievementsusersxuidachievementsgetv2.md)

&nbsp;&nbsp;Obtient la liste des primes définis sur le titre, ceux déverrouillé par l’utilisateur ou ceux de que l’utilisateur est en cours d’exécution.
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>Parent 

[URI de primes](atoc-reference-achievementsv2.md)

   