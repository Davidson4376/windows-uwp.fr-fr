---
title: /users/{ownerId}/people/xuids
assetID: db2faec7-9f6c-f240-586a-45d6ed596e88
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuids.html
description: " /users/{ownerId}/people/xuids"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 27b7695ba163bf0ca832a96df030868e646e0abc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626334"
---
# <a name="usersowneridpeoplexuids"></a>/users/{ownerId}/people/xuids
Accède aux personnes par XUID collection de personnes de l’appelant. Le domaine pour ces URI est `social.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| ownerId| chaîne| Identificateur de l’utilisateur dont la ressource est accédée. Doit correspondre à l’utilisateur authentifié. Les valeurs possibles sont « me », xuid({xuid}) ou gt({gamertag}).| 
  
<a id="ID4EOB"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[POST](uri-usersowneridpeoplexuidspost.md)

&nbsp;&nbsp;Obtient les personnes par XUID provenant de personnes de l’appelant de collection.
 
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   