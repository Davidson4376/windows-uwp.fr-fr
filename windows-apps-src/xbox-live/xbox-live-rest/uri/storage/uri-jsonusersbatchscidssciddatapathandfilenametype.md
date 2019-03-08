---
title: /json/users/batch/scids/{scid}/data/{pathAndFileName},json
assetID: 06ae159f-7739-1330-df15-871d260e6ba1
permalink: en-us/docs/xboxlive/rest/uri-jsonusersbatchscidssciddatapathandfilenametype.html
description: " /json/users/batch/scids/{scid}/data/{pathAndFileName},json"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b28b0faad1c321137344455bc7cd471f7cb6eba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598594"
---
# <a name="jsonusersbatchscidssciddatapathandfilenamejson"></a>/json/users/batch/scids/{scid}/data/{pathAndFileName},json
Télécharge plusieurs fichiers à partir de plusieurs utilisateurs avec le même nom de fichier. Le domaine pour ces URI est `titlestorage.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| GUID| ID de la configuration de service à rechercher.| 
| pathAndFileName| chaîne| Chemin d’accès et le nom de l’élément accessible. Caractères valides dans la partie chemin d’accès (jusqu'à et y compris la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_) et de la barre oblique (/). La partie chemin d’accès peut être vide. Caractères valides dans la partie du nom de fichier (tout ce qui suit la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_), point (.) et trait d’union (-). Le nom de fichier ne peut pas être vide, se terminer par un point ou contenir deux points consécutifs.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[POST](uri-jsonusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;Télécharge plusieurs fichiers à partir de plusieurs utilisateurs avec le même nom de fichier. Le fichier à télécharger est déterminé par l’URI de la demande. Le corps de la requête contient la liste des XUIDs des utilisateurs dont les fichiers à télécharger. Le corps de la réponse sera un message MIME à parties multiples, avec chaque partie représentant un fichier pour un utilisateur particulier avec son propre ensemble d’en-têtes. Il est possible pour les parties de la réponse à comporter un mélange de réussites et échecs.
 
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[URI de stockage de titre](atoc-reference-storagev2.md)

   