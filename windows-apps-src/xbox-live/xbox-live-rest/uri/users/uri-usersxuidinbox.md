---
title: /users/xuid({xuid})/inbox
assetID: 352740c6-42e2-0000-495d-bf384dc3e941
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinbox.html
description: " /users/xuid({xuid})/inbox"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8ded70b32dfd291d17a43a1741b26710f681a397
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640894"
---
# <a name="usersxuidxuidinbox"></a>/users/xuid({xuid})/inbox
Fournit l’accès à un utilisateur de messagerie de boîte de réception pour les Services Xbox LIVE. Le domaine pour ces URI est `msg.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI 
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid | entier non signé 64 bits | L’ID d’utilisateur Xbox (XUID) de l’acteur qui effectue la demande. | 
| messageId | string[50] | ID du message ne soient récupérées ou supprimées. | 
  
<a id="ID4EDC"></a>

 
## <a name="valid-methods"></a>Méthodes valides 

[GET (/users/xuid({xuid})/inbox)](uri-usersxuidinboxget.md)

&nbsp;&nbsp;Récupère un nombre spécifié de résumés de message utilisateur à partir du service. 

[DELETE (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageiddelete.md)

&nbsp;&nbsp;Supprime un message de l’utilisateur dans la boîte de réception de l’utilisateur.

[GET (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageidget.md)

&nbsp;&nbsp;Récupère le texte de message détaillé pour un message utilisateur particulier, marquer comme étant en lecture sur le service. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent  

[URI d’utilisateurs](atoc-reference-users.md)

   