---
title: /json/users/xuid({xuid})/scids/{scid}/data/{path}
assetID: c2745955-5e52-ea2b-7389-cb85202e01c3
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapath.html
description: " /json/users/xuid({xuid})/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 275842420b73abc6c2fd8a8fafa265777e2fa00f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637154"
---
# <a name="jsonusersxuidxuidscidssciddatapath"></a>/json/users/xuid({xuid})/scids/{scid}/data/{path}
Répertorie des informations de fichier dans un chemin d’accès spécifié. Le domaine pour ces URI est `titlestorage.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| La Xbox ID de l’utilisateur (XUID) du lecteur qui effectue la demande.| 
| scid| GUID| ID de la configuration de service à rechercher.| 
| path| chaîne| Le chemin d’accès pour les éléments de données à retourner. Tous les répertoires et sous-répertoires correspondant retournées. Caractères valides sont les lettres majuscules (A-Z), des lettres minuscules (a-z), chiffres (0-9), trait de soulignement (_) et par une barre oblique (/). Peut être vide. Longueur maximale de 256.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET](uri-jsonusersxuidscidssciddatapath-get.md)

&nbsp;&nbsp;Répertorie des informations de fichier dans un chemin d’accès spécifié.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[URI de stockage de titre](atoc-reference-storagev2.md)

   