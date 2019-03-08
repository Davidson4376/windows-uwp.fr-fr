---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})
assetID: 39c921d1-a166-74b9-fcbc-ea3c0c58cc40
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservernamedelete.html
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 65da52284d49d4d9384685d073f13bd93b10a30b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658314"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameserversserver-name"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})
Supprime le serveur spécifié à partir d’une session.

> [!IMPORTANT]
> Cette méthode URI requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

  * [Paramètres d’URI](#ID4ET)
  * [Codes d’état HTTP](#ID4E5)
  * [Corps de la demande](#ID4EFB)
  * [Corps de la réponse](#ID4EOB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID). Partie 1 de l’identificateur de session.|
| sessionTemplateName| chaîne| Nom de l’instance actuelle du modèle de session. Partie 2 de l’identificateur de session.|
| sessionName| GUID| ID unique de la session. Partie 3 de l’identificateur de session.|

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4EFB"></a>


## <a name="request-body"></a>Corps de la requête
Voir la structure de la demande dans [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md).  
<a id="ID4EOB"></a>


## <a name="response-body"></a>Corps de la réponse
Voir la structure de réponse dans [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4E1B"></a>


## <a name="see-also"></a>Voir également

<a id="ID4E3B"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)
