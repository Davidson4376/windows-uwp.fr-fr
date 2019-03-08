---
title: Profile (JSON)
assetID: b92b1750-c2df-39b6-6c5c-f9e8068c8097
permalink: en-us/docs/xboxlive/rest/json-profile.html
description: " Profile (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7299fcb4d375a3fc35ad67306b70f5fa4afde963
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607514"
---
# <a name="profile-json"></a>Profile (JSON)
Les paramètres de profil personnel pour un utilisateur. 
<a id="ID4EN"></a>

 
## <a name="profile"></a>Profil
 
L’objet de profil a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| AppDisplayName| chaîne| Nom d’affichage dans les applications. Cela peut être le nom d’utilisateur « réel » ou leur identité, en fonction de la confidentialité. Ce paramètre représente la chaîne d’identité de l’utilisateur qui doit être utilisé pour l’affichage dans les applications.| 
| GameDisplayName| chaîne| Nom d’affichage dans les jeux. Cela peut être le nom d’utilisateur « réel » ou leur identité, en fonction de la confidentialité. Ce paramètre représente la chaîne d’identité de l’utilisateur qui doit être utilisé pour l’affichage dans les jeux.| 
| Gamertag| chaîne| Identité de l’utilisateur.| 
| AppDisplayPicRaw| chaîne| URL d’application brutes affichage pic (voir ci-dessous).| 
| GameDisplayPicRaw| chaîne| URL de pic d’affichage de jeu brutes (voir ci-dessous).| 
| AccountTier| chaîne| Quel type de compte de l’utilisateur dispose ? Or, argent ou FamilyGold ?| 
| TenureLevel| entier non signé 32 bits| Nombre d’années l’utilisateur a été avec Xbox Live ?| 
| Score de joueur| entier non signé 32 bits| Score de joueur de l’utilisateur.| 
  


> [!NOTE] 
> Images peuvent être « image » l’utilisateur ou leur gamerpic XboxOne, en fonction de la confidentialité. Ces paramètres représentent des url de l’image de l’utilisateur qui doit être utilisé pour l’affichage sur le client. Cette image peut être vide (indiquant que l’utilisateur n’a pas encore définir n’importe quelle image). 


 
L’URL brute est une URL redimensionnable. Il peut être utilisé pour spécifier une de ces tailles et met en forme à l’aide en ajoutant `&format={format}&w={width}&h={height}` à l’URI :
 
Format : png
 
Tailles : 64x64, 208x208, 424x424
 
<a id="ID4E2D"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E4D"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   