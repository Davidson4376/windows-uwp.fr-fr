---
title: /users/{ownerId}/people/{targetid}
assetID: 9dd19e75-3b48-d7e0-fc65-6760c15ddf62
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetid.html
description: " /users/{ownerId}/people/{targetid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6238996abdeaca9b7a9a7a20d3f1ae9702e95a73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661744"
---
# <a name="usersowneridpeopletargetid"></a>/users/{ownerId}/people/{targetid}
Accède à une personne par ID de la cible de collection de personnes de l’appelant. Le domaine pour ces URI est `social.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| ownerId| chaîne| Identificateur de l’utilisateur dont la ressource est accédée. Doit correspondre à l’utilisateur authentifié. Les valeurs possibles sont « me », xuid({xuid}) ou gt({gamertag}).| 
| targetid| chaîne| Identificateur de l’utilisateur dont les données sont récupérées à partir de la liste de personnes du propriétaire, un ID d’utilisateur Xbox (XUID) ou une identité. Exemples de valeurs : xuid(2603643534573581), gt(SomeGamertag).| 
  
<a id="ID4EQB"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET](uri-usersowneridpeopletargetidget.md)

&nbsp;&nbsp;Obtient une personne par ID de la cible de collection de personnes de l’appelant.
 
<a id="ID4E1B"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E3B"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   