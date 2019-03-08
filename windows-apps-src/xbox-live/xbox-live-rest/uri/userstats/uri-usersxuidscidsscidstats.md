---
title: /users/xuid({xuid})/scids/{scid}/stats
assetID: 3cf9ffd4-9a8b-2658-402b-2e933f7f6f1b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstats.html
description: " /users/xuid({xuid})/scids/{scid}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 53a6c7bb0e7390b024b01e221d8061316a80509e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650414"
---
# <a name="usersxuidxuidscidsscidstats"></a>/users/xuid({xuid})/scids/{scid}/stats
Accède à une configuration de service la portée définie par une liste délimitée par des virgules des noms de statistiques utilisateur pour le compte de l’utilisateur spécifié. Le domaine pour ces URI est `userstats.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| GUID| Xbox utilisateur ID (XUID) de l’utilisateur au nom duquel pour accéder à la configuration de service.| 
| scid| GUID| Identificateur de la configuration de service qui contient la ressource sollicitée.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET](uri-usersxuidscidsscidstatsget.md)

&nbsp;&nbsp;Obtient une configuration de service la portée définie par une liste délimitée par des virgules des noms de statistiques utilisateur pour le compte de l’utilisateur spécifié.

[OBTENIR des métadonnées de valeur](uri-usersxuidscidsscidstatsgetvaluemetadata.md)

&nbsp;&nbsp;Obtient une liste des statistiques spécifié, notamment les métadonnées associées aux valeurs statistiques, d’un utilisateur dans une configuration de service spécifié.
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>Parent 

[URI de statistiques utilisateur](atoc-reference-userstats.md)

   