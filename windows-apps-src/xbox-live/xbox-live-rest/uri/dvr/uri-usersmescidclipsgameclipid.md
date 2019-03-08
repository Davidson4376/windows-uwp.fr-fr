---
title: /users/me/scids/{scid}/clips/{gameClipId}
assetID: f5bead69-4fc9-f551-39cb-c8754645ac88
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipid.html
description: " /users/me/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3cbe2d2c996b466fd94287129f1add0868f05b05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661774"
---
# <a name="usersmescidsscidclipsgameclipid"></a>/users/me/scids/{scid}/clips/{gameClipId}
Accéder aux données de clip de jeu et les métadonnées. Les domaines pour ces URI sont `gameclipsmetadata.xboxlive.com` et `gameclipstransfer.xboxlive.com`, selon la fonction de l’URI en question.
 
  * [Paramètres d’URI](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| chaîne| ID de configuration de service de la ressource est accédée. Doit correspondre à la SCID de l’utilisateur authentifié.| 
| gameClipId| chaîne| ID du clip de jeu de la ressource est accédée.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[Supprimer (/ users/me/scids / {scid} /clips/ {gameClipId})](uri-usersmescidclipsgameclipiddelete.md)

&nbsp;&nbsp;Supprimer le clip de jeu

[POST (/ users/me/scids / {scid} /clips/ {gameClipId})](uri-usersmescidclipsgameclipidpost.md)

&nbsp;&nbsp;Mettre à jour les métadonnées d’élément de jeu pour les données de l’utilisateur.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[URI de jeux DVR](atoc-reference-dvr.md)

   