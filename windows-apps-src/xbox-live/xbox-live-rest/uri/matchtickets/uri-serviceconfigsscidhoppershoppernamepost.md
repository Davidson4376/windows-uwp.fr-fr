---
title: POST (/serviceconfigs/{scid}/hoppers/{hoppername})
assetID: 8cbf62aa-d639-e920-1e39-099133af17f8
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamepost.html
description: " POST (/serviceconfigs/{scid}/hoppers/{hoppername})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7682b92ec61c98679904825e360d73318e9fee90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659834"
---
# <a name="post-serviceconfigsscidhoppershoppername"></a>POST (/serviceconfigs/{scid}/hoppers/{hoppername})

Crée le ticket de correspondance spécifiées.

> [!IMPORTANT]
> Cette méthode est conçue pour une utilisation avec contrat 103 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 103 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4E5)
  * [Autorisation](#ID4EJB)
  * [Codes d’état HTTP](#ID4E3C)
  * [Corps de la demande](#ID4EFD)
  * [Corps de la réponse](#ID4E3G)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes

Cette méthode HTTP/REST crée un ticket de correspondance pour un hopper avec un nom particulier à la configuration du service au niveau du code (SCID). Cette méthode peut être encapsulée par le **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.CreateMatchTicketAsync** (méthode).  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID) pour la session.|
| hoppername | chaîne | Le nom de la hopper. |

<a id="ID4EJB"></a>


## <a name="authorization"></a>Authorization

| Type| Obligatoire| Description| Cas d’absence de réponse|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Privilèges et le Type d’appareil| oui| Lorsque le type de périphérique de l’utilisateur est défini dans la console, seuls les utilisateurs disposant des privilèges multijoueur dans leurs revendications sont autorisés à effectuer des appels vers le service. | 403|
| Type de périphérique| oui| Lorsque deviceType de l’utilisateur est absent ou non la console, le titre est mis en correspondance dans la valeur ne doit pas être un titre de la console uniquement. | 403|
| ID de titre/preuve de Type d’achat/appareil| oui| Le titre est mis en correspondance dans doit autoriser matchmaking pour la revendication titre spécifié, la combinaison de type de périphérique. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>Corps de la requête

<a id="ID4ELD"></a>


### <a name="required-members"></a>Membres requis

| Membre| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| serviceConfig| GUID| SCID pour la session.|
| hopperName| chaîne| Nom de la hopper.|
| giveUpDuration| entier signé 32 bits| Délai d’attente maximal (nombre entier de secondes).|
| preserveSession| énumération| Une valeur qui indique si la session est réutilisée en tant que la session dans laquelle faire correspondre. Les valeurs possibles sont « toujours » et « jamais ». |
| ticketSessionRef| MultiplayerSessionReference| L’objet MultiplayerSessionReference pour la session dans laquelle le lecteur ou le groupe est en cours de lecture. |
| ticketAttributes| collection d’objets| Les attributs et les valeurs fournies par l’utilisateur sur le groupe de joueurs.|

<a id="ID4EXF"></a>


### <a name="prohibited-members"></a>Membres interdits

Tous les autres membres sont interdites dans une demande.

<a id="ID4ECG"></a>


### <a name="sample-request"></a>Exemple de demande

La session référencé par le **ticketSessionRef** objet doit être créé avant un ticket de correspondance peut être créé, et la session doit contenir les joueurs à mettre en correspondance, ainsi que leurs attributs de lecteur spécifique. Chaque joueur doit créer ou rejoindre la session par rapport à la MPSD, ajout d’attributs de correspondance associé à la session. Les attributs de correspondance sont placés dans un champ de propriété personnalisée appelé matchAttrs sur chaque lecteur.

Une demande de création ou de jointure est soumise à **https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templatename}/sessions/{sessionname}** peut se présenter comme suit :


```cpp
{
   "members": {
     "me": {
       "constants": {
         "system": {
           "xuid": 2535285330879750
         }
      },
      "properties": {
         "custom": {
           "matchAttrs": {
             "skill": 5,
             "ageRange": "teenager"
           }
         }
      }
    }
  }
}

```


Une fois que la session a été créée, le titre peut appeler le service de matchmaking pour créer le ticket pour cette session.


> [!NOTE] 
> Un titre peut permettre aux utilisateurs de cet appel de nouvelle tentative, mais ne doit pas recommencer il automatiquement si les données échouent.  



```cpp
POST /serviceconfigs/{scid}/hoppers/{hoppername}

{
  "giveUpDuration":10,
  "preserveSession": "never",
  "ticketSessionRef": {
     "scid": "ABBACDDC-0000-0000-0000-000000000001",  
     "templateName": "TestTemplate",
     "name": "5E55104-0000-0000-0000-000000000001"
  },
  "ticketAttributes": {
    "desiredMap": "Test Map",
    "desiredGameType": "Test GameType"
  }
}

```


<a id="ID4E3G"></a>


## <a name="response-body"></a>Corps de la réponse

| Membre| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ticketId| GUID| L’ID pour le ticket en cours de création.|
| waitTime| entier signé 32 bits| La moyenne temps d’attente pour le hopper (nombre entier de secondes).|


```cpp
{
  "ticketId":"0584338f-a2ff-4eb9-b167-c0e8ddecae72",
  "waitTime":60
}

```


<a id="ID4EHAAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EJAAC"></a>


##### <a name="parent"></a>Parent  

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)
