---
title: /users/xuid({xuid})/outbox
assetID: 0b66b885-15ff-be55-f8be-e6e9d85d087e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutbox.html
description: " /users/xuid({xuid})/outbox"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 88f3f3753aeac99db0a8a53e0a2ddde21d034ac5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607544"
---
# <a name="usersxuidxuidoutbox"></a>/users/xuid({xuid})/outbox
Fournit la messagerie d’envoi uniquement accès à un utilisateur de la boîte d’envoi pour les Services Xbox LIVE. Le domaine pour ces URI est `msg.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI 
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid | entier non signé 64 bits | L’ID d’utilisateur Xbox (XUID) de l’acteur qui effectue la demande. | 
  
<a id="ID4EXB"></a>

 
## <a name="valid-methods"></a>Méthodes valides 

[POST (/users/xuid({xuid})/outbox)](uri-usersxuidoutboxpost.md)

&nbsp;&nbsp;Envoie un message spécifié à une liste de destinataires. 
 
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent  

[URI d’utilisateurs](atoc-reference-users.md)

   