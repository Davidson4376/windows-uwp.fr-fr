---
title: DELETE (/handles/{handleId})
assetID: 71cceff4-3a74-dc05-12db-cfe3f20a8aea
permalink: en-us/docs/xboxlive/rest/uri-handleshandleiddelete.html
description: " DELETE (/handles/{handleId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 354f3563c48139edc5d5cc041e8304998af55620
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613404"
---
# <a name="delete-handleshandleid"></a>DELETE (/handles/{handleId})
Supprime les handles spécifiés par l’ID de handle.

> [!IMPORTANT]
> Cette méthode est utilisée par le mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4EAB)
  * [Codes d’état HTTP](#ID4ELB)
  * [Corps de la demande](#ID4ESB)
  * [Corps de la réponse](#ID4E4B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes
Cette méthode HTTP/REST supprime les poignées pour l’ID spécifié et l’activité l’utilisateur actuel pour la session. Cette méthode peut être encapsulée par **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.ClearActivityAsync**.  
<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| handleId| GUID| ID unique de la poignée pour la session.|

<a id="ID4ELB"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4ESB"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4E4B"></a>


## <a name="response-body"></a>Corps de la réponse

Aucun objet n’est envoyés dans le corps de la réponse.

<a id="ID4EIC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EKC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}](uri-handleshandleid.md)
