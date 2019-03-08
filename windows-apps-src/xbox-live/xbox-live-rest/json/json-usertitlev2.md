---
title: UserTitle (JSON)
assetID: 3e5767af-5704-8463-696b-42a7d2a1e231
permalink: en-us/docs/xboxlive/rest/json-usertitlev2.html
description: " UserTitle (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33901a5bde25fd17072c2b45d587a33209424378
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623024"
---
# <a name="usertitle-json"></a>UserTitle (JSON)
Contient les données de titre d’utilisateur. 
<a id="ID4EN"></a>

 
## <a name="usertitle"></a>UserTitle
 
L’objet UserTitle a la spécification suivante. Toutes les propriétés sont requises.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| lastUnlock| DateTime| Heure de que dernière une prime.| 
| titleId| entier non signé 32 bits| Identificateur unique pour le titre.| 
| titleVersion| chaîne| La version du titre.| 
| serviceConfigId| chaîne| ID du jeu de configuration principal de service associé au titre.| 
| titleType| chaîne| Le type de titre.| 
| Plateforme| chaîne| La plateforme prise en charge.| 
| name| chaîne| Le nom de ce titre. Longueur maximale de 22.| 
| earnedAchievements| entier non signé 32 bits| Le nombre de primes obtenues pour le titre, y compris les primes déverrouillés et terminé avec succès les défis.| 
| currentGamerscore| entier non signé 32 bits| Le score de joueur total, cet utilisateur a gagné dans ce titre.| 
| maxGamerscore| entier non signé 32 bits| Le total gamescore possible pour ce titre.| 
  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   