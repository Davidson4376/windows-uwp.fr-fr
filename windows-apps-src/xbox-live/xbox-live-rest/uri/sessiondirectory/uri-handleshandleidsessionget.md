---
title: GET (/handles/{handleId}/session)
assetID: 1f22954c-e77b-69c2-63f4-741fbd965f8f
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionget.html
description: " GET (/handles/{handleId}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 41911dc53b316f4f323b9859d9101581ec88e497
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593894"
---
# <a name="get-handleshandleidsession"></a>GET (/handles/{handleId}/session)
Obtient un objet de session pour l’identificateur de handle spécifié.

> [!IMPORTANT]
> Cette méthode est utilisée par le mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4EDB)
  * [Codes d’état HTTP](#ID4EOB)
  * [Corps de la demande](#ID4EVB)
  * [Corps de la réponse](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes

Cette méthode HTTP/REST récupère un objet de session à partir du serveur, en utilisant le pointeur côté service fourni à la session (handle). La valeur de retour est l’objet de session, avec tous ses attributs. Cette méthode peut être encapsulée par **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**.

L’appelant de cette méthode obtient l’ID de handle à partir d’un joueur **MultiplayerActivityDetails** objet. Vous pouvez également l’appelant obtient l’ID de l’activation d’un protocole après qu’un utilisateur a accepté une invitation à un jeu.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| handleId| GUID| ID unique de la poignée pour la session.|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4EVB"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4E6B"></a>


## <a name="response-body"></a>Corps de la réponse
Voir la structure de réponse dans [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4EIC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EKC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}/session](uri-handleshandleidsession.md)
