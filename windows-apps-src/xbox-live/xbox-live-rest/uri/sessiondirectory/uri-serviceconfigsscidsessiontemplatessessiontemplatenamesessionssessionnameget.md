---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: 6a4c4a13-c968-3271-cbc3-b742a8de98b3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.html
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 21d534d7b55934d7174c925838ed88980acff609
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655064"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
Obtient un objet de session.

> [!IMPORTANT]
> Cette méthode URI requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4EMB)
  * [Codes d’état HTTP](#ID4EZB)
  * [Corps de la demande](#ID4E6B)
  * [Corps de la réponse](#ID4EKC)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes

Cette méthode HTTP/REST lit un document de session pour le nom spécifié et récupère la session. En cas de réussite, elle retourne l’objet de session, avec tous ses attributs, obtenu à partir du serveur. Cette méthode peut être encapsulée par **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionAsync**. Les paramètres de la méthode GET directement correspondent à ceux spécifiés dans le **MultiplayerSessionReference** objet pour la session, passé dans le *sessionReference* paramètre de  **GetCurrentSessionAsync**.

Le format de transmission pour la méthode GET est indiqué ci-dessous.

```cpp
GET /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
      Content-Type: application/json

```



<a id="ID4EMB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID). Partie 1 de l’identificateur de session.|
| sessionTemplateName| chaîne| Nom de l’instance actuelle du modèle de session. Partie 2 de l’identificateur de session.|
| sessionName| GUID| ID unique de la session. Partie 3 de l’identificateur de session.|

<a id="ID4EZB"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4E6B"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4EKC"></a>


## <a name="response-body"></a>Corps de la réponse
Voir la structure de réponse dans [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4ETC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EVC"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)
