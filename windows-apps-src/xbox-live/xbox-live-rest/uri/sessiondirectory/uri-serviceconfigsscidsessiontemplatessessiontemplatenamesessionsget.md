---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)
assetID: 9daac964-0b25-3430-fcfd-0f8658aceee1
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionsget.html
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1020c9d9c378a95070a7b0bf3faeb9d2c6751d51
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627474"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatenamesessions"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)
Récupère les documents de modèle de session.

> [!IMPORTANT]
> Cette méthode URI requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4EKB)
  * [Codes d’état HTTP](#ID4EXB)
  * [Corps de la demande](#ID4EAC)
  * [Corps de la réponse](#ID4EKC)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes

Cette méthode HTTP/REST récupère des informations de modèle de session pour les filtres fournis. Cette méthode peut être encapsulée par **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsAsync**.


> [!NOTE] 
> Pour le mode multijoueur 2015, cette méthode est appelée par <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync</b>.  



> [!NOTE] 
> Chaque appel à cette méthode doit inclure un mot clé, un filtre d’ID utilisateur Xbox ou les deux. Si l’appelant ne dispose pas des autorisations appropriées pour le <i>privé</i> et <i>réservations</i> paramètres, la méthode retourne un code d’erreur de 403 interdit, que toutes ces sessions existent réellement ou non.  


<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID). ID de la partie 1 de la session.|
| keyword| chaîne| Un mot clé utilisé pour filtrer les résultats aux sessions simples identifiées par cette chaîne.|
| xuid| GUID| ID d’utilisateur de Xbox pour les utilisateurs pour lequel vous souhaitez récupérer les sessions. Les utilisateurs doivent être actives dans les sessions. |
| réservations| chaîne| Valeur qui indique si la liste des sessions inclut celles que les utilisateurs n’ont pas accepté. Ce paramètre peut uniquement être défini sur true. Ce paramètre exige de l’appelant à accéder au niveau du serveur à la session ou en XUID l’appelant demander faire correspondre le filtre d’ID utilisateur Xbox. |
| inactif| chaîne| Valeur qui indique si la liste des sessions inclut celles qui les utilisateurs ont accepté mais ne sont pas en cours d’exécution. Ce paramètre peut uniquement être défini sur true. |
| Privé| chaîne| Valeur qui indique si la liste des sessions inclut des sessions privées. Ce paramètre peut uniquement être défini sur true. Il est valide uniquement lors de l’interrogation de vos propres sessions, ou lors de l’interrogation de serveur à serveur. Ce paramètre sur true requiert que l’appelant à accéder au niveau du serveur à la session ou en XUID l’appelant demander faire correspondre le filtre d’ID utilisateur Xbox. |
| visibility| chaîne| Valeur d’énumération indiquant l’état de visibilité utilisé lors du filtrage des résultats. Actuellement ce paramètre peut uniquement être défini à ouvrir pour inclure les sessions ouvertes. Consultez <b>MultiplayerSessionVisibility</b>. |
| version| chaîne| Entier positif indiquant la version de session principale ou inférieur des sessions à inclure. La valeur doit être inférieure ou égale à la version de contrat de la demande modulo 100. |
| Take| chaîne| Entier positif indiquant le nombre maximal de sessions à récupérer.|

<a id="ID4EXB"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4EAC"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4EKC"></a>


## <a name="response-body"></a>Corps de la réponse

Le retour à partir de cette méthode est un tableau JSON des références de session, avec certaines données incluses session en ligne.


```cpp
{
    "results": [ {
            "xuid": "9876",    // If the session was found from a xuid, that xuid.
            "startTime": "2009-06-15T13:45:30.0900000Z",
            "sessionRef": {
                "scid": "foo",
                "templateName": "bar",
                "name": "session-seven"
            },
            "accepted": 4,    // Approximate number of non-reserved members.
            "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
            "visibility": "open",    // or "private", "visible", or "full"
            "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
            "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
            "keywords": [ "one", "two" ]
        }
    ]
}

```


<a id="ID4EUC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EWC"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)
