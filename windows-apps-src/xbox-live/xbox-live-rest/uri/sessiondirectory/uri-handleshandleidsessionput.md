---
title: PUT (/handles/{handle-id}/session)
assetID: d8a3d473-1192-ec0c-3935-c301f4f61e03
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionput.html
description: " PUT (/handles/{handle-id}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a1872857d8b8e692f67e3c7b2a067ae86663c00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641604"
---
# <a name="put-handleshandle-idsession"></a>PUT (/handles/{handle-id}/session)
Crée ou met à jour d’une session par le déréférencement d’un handle.

> [!IMPORTANT]
> Cette méthode est utilisée par le mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4ECB)
  * [Codes d’état HTTP](#ID4ENB)
  * [Corps de la demande](#ID4EUB)
  * [Corps de la réponse](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes

Cette méthode HTTP/REST écrit une session de nouveau ou mis à jour dans le service multijoueur, en utilisant l’ID de handle de session fourni. Le résultat est un objet représentant la session de nouveau ou mis à jour, tel que retourné par le serveur. Cette méthode peut être encapsulée par **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionByHandleAsync**.

L’appelant de cette méthode obtient l’ID de handle à partir d’un joueur **MultiplayerActivityDetails** objet. Vous pouvez également l’appelant obtient l’ID de l’activation d’un protocole après qu’un utilisateur a accepté une invitation à un jeu.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| handleId| GUID| ID unique de la poignée pour la session.|

<a id="ID4ENB"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4EUB"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4E6B"></a>


## <a name="response-body"></a>Corps de la réponse

Aucun objet n’est envoyés dans le corps de la réponse.

<a id="ID4EKC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EMC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}/session](uri-handleshandleidsession.md)
