---
title: PUT (/users/me/gamerpic)
assetID: ddf71c62-197d-a81d-35a7-47c6dc9e1b0c
permalink: en-us/docs/xboxlive/rest/uri-usersmegamerpicput.html
description: " PUT (/users/me/gamerpic)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7aedc7cbd8366c9cb8d3a60e2cb1f5e843b24a8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661654"
---
# <a name="put-usersmegamerpic"></a>PUT (/users/me/gamerpic)
Télécharge un gamerpic 1080 x 1080. 
  * [Corps de la demande](#ID4EQ)
  * [Codes d’état HTTP](#ID4EZ)
  * [Corps de la réponse](#ID4EXC)
 
<a id="ID4EQ"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Le corps de la demande est un gamerpic (fichier PNG de 1080 x 1080).
  
<a id="ID4EZ"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | 
| 200| OK| GET réussie.| 
| 201| Créé.| Le téléchargement a réussi.| 
| 403| Interdit| Le privilège est révoqué.| 
| 500| Erreur| Nous avons rencontré un problème.| 
  
<a id="ID4EXC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Aucun objet n’est envoyés dans le corps de la réponse.
  
<a id="ID4ECD"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EED"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/gamerpic](uri-usersmegamerpic.md)

   