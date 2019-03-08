---
title: /users/xuid(xuid)/lists/PINS/{listname}
assetID: b6421b11-fcd1-cfdb-c1fa-6cab3dab89d9
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistname.html
description: " /users/xuid(xuid)/lists/PINS/{listname}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0f9610b400e9530f86e264cea30bfdfdd1b09c8d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627994"
---
# <a name="usersxuidxuidlistspinslistname"></a>/users/xuid(xuid)/lists/PINS/{listname}
Éléments d’accès dans une liste. Le domaine pour ces URI est `eplists.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| chaîne| ID d’utilisateur de la Xbox (XUID).| 
| listtype| chaîne| Type de la liste (comment il est utilisé et comment il agit). Toujours « PIN » pour ces méthodes associées.| 
| nom de la liste| chaîne| Nom de la liste (liste d’un type donné pour agir sur). Toujours « XBLPins » pour les éléments dans les codes confidentiels.| 
  
<a id="ID4EGC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[SUPPRIMER](uri-usersxuidlistspinslistnamedelete.md)

&nbsp;&nbsp;Supprime les éléments d’une liste.

[GET](uri-usersxuidlistspinslistnameget.md)

&nbsp;&nbsp;Retourne le contenu d’une liste.

[POST](uri-usersxuidlistspinslistnamepost.md)

&nbsp;&nbsp;Insère des éléments dans la liste à l’index en fonction du paramètre de chaîne de requête **insertIndex**.

[PUT](uri-usersxuidlistspinslistnameput.md)

&nbsp;&nbsp;Met à jour les éléments dans une liste en fonction de l’index spécifié pour chaque élément dans le corps de la demande.
 
<a id="ID4EZC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   