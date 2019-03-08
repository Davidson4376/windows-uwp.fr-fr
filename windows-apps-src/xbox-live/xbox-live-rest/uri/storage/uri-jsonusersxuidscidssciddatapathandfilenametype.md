---
title: /json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json
assetID: d004d715-d73c-693e-2a0b-ce64f482642b
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapathandfilenametype.html
description: " /json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ee2f21426f0fad1cdb84adba864aeebd0d8e1edc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614024"
---
# <a name="jsonusersxuidxuidscidssciddatapathandfilenamejson"></a>/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json
Téléchargements, télécharge ou supprime un fichier. Le domaine pour ces URI est `titlestorage.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| La Xbox ID de l’utilisateur (XUID) du lecteur qui effectue la demande.| 
| scid| GUID| ID de la configuration de service à rechercher.| 
| pathAndFileName| chaîne| Chemin d’accès et le nom de l’élément accessible. Caractères valides dans la partie chemin d’accès (jusqu'à et y compris la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_) et de la barre oblique (/). La partie chemin d’accès peut être vide. Caractères valides dans la partie du nom de fichier (tout ce qui suit la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_), point (.) et trait d’union (-). Le nom de fichier ne peut pas être vide, se terminer par un point ou contenir deux points consécutifs.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[SUPPRIMER](uri-jsonusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;Supprime un fichier. 

[GET](uri-jsonusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;Télécharge un fichier.

[PUT](uri-jsonusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;Télécharge un fichier. Téléchargement de bloc multiples n’est pas pris en charge pour les données de type json. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent 

[URI de stockage de titre](atoc-reference-storagev2.md)

   