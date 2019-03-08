---
title: URI des jeux DVR
assetID: 472f705e-bf28-7894-b1ba-80933d8746a6
permalink: en-us/docs/xboxlive/rest/atoc-reference-dvr.html
description: " URI des jeux DVR"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b4bfd6e51efce4c6ec85db99a10a44a776dcb840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617844"
---
# <a name="game-dvr-uris"></a>URI des jeux DVR
 
Cette section fournit des détails sur les adresses de l’identificateur URI (Universal Resource) et les méthodes de protocole HTTP (Hypertext Transport) associées à partir de Xbox Live Services pour *jeux DVR*.
 
Uniquement les consoles peuvent enregistrer un clip de jeu, mais n’importe quel appareil qui peut accéder à peut afficher un élément.
 
Selon la fonction de l’URI en question, les domaines pour ces URI sont :
 
   *  gameclipsmetadata.xboxlive.com 
   *  gameclipstransfer.xboxlive.com 
  
<a id="ID4EZB"></a>

 
## <a name="in-this-section"></a>Dans cette section

[/public/scids/{scid}/clips](uri-publicscidclips.md)

&nbsp;&nbsp;Clips publiques d’accès. Cet URI peut réellement être spécifié sous deux formes, `/public/scids/{scid}/clips` et `/public/titles/{titleId}/clips`. Pour plus d’informations, voir ci-dessous.

[/{uri}](uri-uri.md)

&nbsp;&nbsp;Accéder aux données de clip de jeu.

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

&nbsp;&nbsp;Accès initial transférer les demandes.

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

&nbsp;&nbsp;Accéder aux données de clip de jeu et les métadonnées.

[/users/{ownerId}/clips](uri-usersowneridclips.md)

&nbsp;&nbsp;Liste d’accès des éléments de l’utilisateur.

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

&nbsp;&nbsp;Accéder à un seul élément de jeu à partir du système si tous les ID pour le localiser sont connues.
 
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   