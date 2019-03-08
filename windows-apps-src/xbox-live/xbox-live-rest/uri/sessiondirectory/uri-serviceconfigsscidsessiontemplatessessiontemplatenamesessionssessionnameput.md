---
title: PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: e3e4f164-ac5e-cbd9-8c05-2e1ac00dc55e
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.html
description: " PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d35b3f89f8b866a5236e8f5ac91eb37d9a82d306
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598554"
---
# <a name="put-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
Crée, met à jour ou rejoint une session.

> [!IMPORTANT]
> Cette méthode URI requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4EYB)
  * [Codes d’état HTTP](#ID4EFC)
  * [Corps de la demande](#ID4EOC)
  * [Corps de la réponse](#ID4E4C)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes

Cette méthode HTTP/REST crée des jointures, ou met à jour d’une session, selon le sous-ensemble du même modèle de corps de demande JSON est envoyé. En cas de réussite, elle retourne un **MultiplayerSession** de l’objet qui contient la réponse retournée par le serveur. Les attributs qu’il peuvent être différents à partir des attributs dans le passé dans **MultiplayerSession** objet. Cette méthode peut être encapsulée par **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionAsync**.

Opérations de mise à jour et de la création de session utilisent une requête PUT avec un corps de l’application/json qui représente les modifications à appliquer. Les opérations sont idempotentes, autrement dit, plusieurs applications des modifications n’ont aucun effet supplémentaire.

Le corps de demande JSON reflète la structure de données de session. Tous les champs et sous-champs sont facultatifs.

Le format de câble pour la création de sessions de la méthode PUT ou de rejoindre le mode est indiqué ci-dessous.

> [!NOTE]
> Prendre en charge à l’aide de ce modèle. Upates sont appliquées aveuglément, quel que soit l’état actuel de la session.



```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



Le format de câble pour le mode de mise à jour de la méthode PUT session est indiqué ci-dessous.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



Le format de câble pour la méthode PUT mettre à jour les propriétés de la session est indiqué ci-dessous. Il équivaut à une opération PUT à la session URI avec un corps de rien d’autre que l’objet ci-dessous en tant que propriétés. La différence est que cette opération renvoie le code d’erreur 404 introuvable si la session n’existe pas. Cette opération prend en charge l’en-tête If-Match.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001/properties HTTP/1.1
         Content-Type: application/json

         { "system": { }, "custom": { } }

```



<a id="ID4EYB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID). Partie 1 de l’identificateur de session.|
| sessionTemplateName| chaîne| Nom de l’instance actuelle du modèle de session. Partie 2 de l’identificateur de session.|
| sessionName| GUID| ID unique de la session. Partie 3 de l’identificateur de session.|

<a id="ID4EFC"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4EOC"></a>


## <a name="request-body"></a>Corps de la requête

Voici un exemple de corps de demande pour créer ou rejoindre une session. Les membres suivants de corps de la demande sont facultatifs. Tous les autres membres possibles sont interdites dans une demande.

| Membre| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- |
| constants| objet| Paramètres en lecture seule qui sont fusionnés avec le modèle de session pour produire l’une des constantes pour la session. |
| propriétés | objet | Modifications à fusionner dans les propriétés de session.|
| members.me | objet| Constantes et des propriétés qui fonctionnent comme leurs équivalents de niveau supérieur. N’importe quelle méthode PUT, l’utilisateur doit être un membre de la session et ajoute l’utilisateur si nécessaire. Si « me » est spécifié comme null, le membre qui effectue la requête est supprimé de la session. |
| membres | objet| Autres objets qui représentent les utilisateurs à ajouter à la session, indexé par un index de base zéro. Le nombre de membres dans une demande commence toujours par 0, même si la session déjà contient des membres. Les membres sont ajoutés à la session dans l’ordre dans lequel ils apparaissent dans la demande. Propriétés de membre peuvent uniquement être définies par l’utilisateur auquel ils appartiennent. |
| serveurs | objet| Valeurs indiquant les mises à jour et des ajouts à la session du jeu de participants de serveur associé. Si un serveur est spécifié comme null, cette entrée de serveur est supprimée de la session. |



```cpp
{
  "properties": {
    "custom": {"KANWE": "MGMSY"},
    "system": {}
  },
  "constants": {
    "custom": {},
    "system": {"visibility": "open"}
  },
  "members": {
    "reserve_0": {
    "constants": {
      "custom": {"type": "leader"},
      "system": {"xuid": "5500461"} }}
   }
}

```


<a id="ID4E4C"></a>


## <a name="response-body"></a>Corps de la réponse

Exemple de corps de réponse pour créer ou rejoindre une session :


```cpp
{
  "contractVersion": 104,
  "correlationId": "0FE81338-EE96-46E3-A3B5-2DBBD6C41C3B",
  "nextTimer": "2009-06-15T13:45:30.0900000Z",

  "initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
  },

  "hostCandidates": [ "ab90a362", "99582e67" ],

  "constants": {
    "system": {"visibility": "open"},
    "custom": {}
  },

  "properties": {
     "system": { "turn": [] },
     "custom": { "myProperty": "myValue" }
  },

  "members": {
      "1": {
        "properties": {
        "system": { },
        "custom": { }
      },

      "constants": {
        "system": { "xuid": "5500461" },
        "custom": { }
      }

      "gamertag": "stacy",
      "deviceToken": "9f4032ba7",
      "reserved": true,
      "activeTitleId": "8397267",
      "joinTime": "2009-06-15T13:45:30.0900000Z",
      "turn": true,
      "initializationFailure": "latency",
      "initializationEpisode": 1,
      "next": 4
    },
  },

  "membersInfo": {
      "first": 1,
      "next": 4,
      "count": 1,
      "accepted": 0
  },

  "servers": {
      "name": {
        "constants": { },
        "properties": { }
      }
  }
}

```


<a id="ID4EID"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EKD"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)
