---
title: POST (/handles)
assetID: 21f3e289-0b0e-2731-befb-bd4c0d71973e
permalink: en-us/docs/xboxlive/rest/uri-handlespost.html
description: " POST (/handles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ed3482b8e629749d294ed25944db16372cc7fee6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594744"
---
# <a name="post-handles"></a>POST (/handles)
Définit la session multijoueur pour l’activité actuelle de l’utilisateur et invite les membres de la session si nécessaire.

> [!IMPORTANT]
> Cette méthode est utilisée par le mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4EHB)
  * [Codes d’état HTTP](#ID4EPB)
  * [Corps de la demande](#ID4EVB)
  * [Corps de la réponse](#ID4EJC)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes

Cette méthode HTTP/REST peut être utilisée pour définir la session pour l’activité en cours. Dans ce cas, la méthode peut être encapsulée par **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SetActivityAsync**. Le corps de la demande doit définir la session à l’aide de la référence le **sessionRef** objet dans le fichier JSON, avec le champ de type pour « activité ». Aucun corps de réponse n’est récupéré. Pour les définitions des éléments spécifiés dans une référence de session, consultez **Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference**.

Cette méthode POST peut également être utilisée pour inviter les utilisateurs spécifiés par les descripteurs à une session. Dans ce cas, la méthode peut être encapsulée par **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SendInvitesAsync**. Cette utilisation de la méthode POST requiert votre corps de la requête pour définir la référence de la session, mais avec le type de champ défini sur « inviter ». Le corps de réponse est un handle de l’invitation.

<a id="ID4EHB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

Aucune

<a id="ID4EPB"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4EVB"></a>


## <a name="request-body"></a>Corps de la requête

<a id="ID4E1B"></a>


### <a name="request-body-for-setting-activity"></a>Corps pour la définition d’activité de la demande


```cpp
{
  "version": 1,
  "type": "activity",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    // The session name is optional in a POST; if not specified, MPSD fills in a GUID.//
    "name": "session-49"
  },
}

```


<a id="ID4EBC"></a>


### <a name="request-body-for-sending-invites"></a>Corps de la demande pour l’envoi des invitations


```cpp
{
  // Common handle fields
  "id: "47ca0049-a5ba-4bc1-aa22-fcf834ce4c13",
  "version": 1,
  "type": "invite",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    "name": "session-49"
   },
   "inviteAttributes": {
     "titleId" : {titleId}, // The title being invited to, in decimal uint32. This value is used to find the title name and/or image.
     "context" : {context}, // The title defined context token. Must be 256 characters or less when URI-encoded.
     "contextString" : {contextstring}, // The string name of a custom invite string to display in the invite notification.
     "senderString" : {sender} // The string name of the sender when the sender is a service.
   },
   "invitedXuid": "3210",
}

```


<a id="ID4EJC"></a>


## <a name="response-body"></a>Corps de la réponse

<a id="ID4EOC"></a>


### <a name="response-body-for-setting-activity"></a>Corps de réponse pour la définition d’activité
Aucun.  
<a id="ID4ESC"></a>


### <a name="response-body-for-sending-invites"></a>Corps de la réponse pour l’envoi des invitations
Handle d’invitation.   
<a id="ID4EXC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EZC"></a>


##### <a name="parent"></a>Parent

[/handles](uri-handles.md)
