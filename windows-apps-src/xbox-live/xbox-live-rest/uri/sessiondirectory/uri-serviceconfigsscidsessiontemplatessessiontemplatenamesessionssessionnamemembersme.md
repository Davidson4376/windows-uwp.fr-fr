---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me
assetID: a6c2fa17-8bed-d0df-d7ff-db1aa60f44b3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b0ecadb543434383ef32c7fd2a749760d5bcc98
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608454"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me
Prend en charge une opération de suppression pour supprimer des membres de la session.
<a id="ID4EO"></a>


## <a name="domain"></a>domaine.
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="remarks"></a>Notes

Toutes les opérations de ressource de membre de session nécessitent un utilisateur de l’ID d’utilisateur Xbox (XUID) de revendication d’autorisation.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID). Partie 1 de l’identificateur de session.|
| sessionTemplateName| chaîne| Nom de l’instance actuelle du modèle de session. Partie 2 de l’identificateur de session.|
| sessionName| GUID| ID unique de la session. Partie 3 de l’identificateur de session.|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>Méthodes valides

[DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersmedelete.md)

&nbsp;&nbsp;Supprime un membre d’une session.

<a id="ID4EYC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4E1C"></a>


##### <a name="parent"></a>Parent

[URI du répertoire de session](atoc-reference-sessiondirectory.md)
