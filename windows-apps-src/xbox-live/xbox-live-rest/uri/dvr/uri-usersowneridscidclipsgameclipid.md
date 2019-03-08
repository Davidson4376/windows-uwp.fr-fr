---
title: /users/{ownerId}/scids/{scid}/clips/{gameClipId}
assetID: 49b68418-71f1-c5a2-3a9b-869fd1fa663c
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipid.html
description: " /users/{ownerId}/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7ea92e89d54df17e8d82084d840a7ee9ef7d032
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658334"
---
# <a name="usersowneridscidsscidclipsgameclipid"></a>/users/{ownerId}/scids/{scid}/clips/{gameClipId}
Accéder à un seul élément de jeu à partir du système si tous les ID pour le localiser sont connues. Les domaines pour ces URI sont `gameclipsmetadata.xboxlive.com` et `gameclipstransfer.xboxlive.com`, selon la fonction de l’URI en question.
 
  * [Paramètres d’URI](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| ownerId| chaîne| Identité de l’utilisateur de l’utilisateur dont la ressource est accédée. Formats pris en charge : « me » ou « xuid(123456789) ». Longueur maximum : 16.| 
| scid| chaîne| ID de configuration de service de la ressource est accédée. Doit correspondre à la SCID de l’utilisateur authentifié.| 
| gameClipId| chaîne| ID du clip de jeu de la ressource est accédée.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET (/users/ {ownerId} /scids/ {scid} /clips/ {gameClipId})](uri-usersowneridscidclipsgameclipidget.md)

&nbsp;&nbsp;Obtenir un seul élément de jeu à partir du système si tous les ID pour le localiser sont connues.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[URI de jeux DVR](atoc-reference-dvr.md)

   